---
title: Hantera webbtrafik med Azure Application Gateway med hjälp av Ansible (förhandsversion)
description: Lär dig hur du kan använda Ansible för att skapa och konfigurera en Azure Application Gateway för att hantera webbtrafik
ms.service: ansible
keywords: ansible, azure, devops, bash, spelbok, azure application gateway, belastningsutjämnare, webbtrafik
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/20/2018
ms.openlocfilehash: 02b98cb22d897fc9599f6e44ddc57ef4211b0893
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47410867"
---
# <a name="manage-web-traffic-with-azure-application-gateway-using-ansible-preview"></a>Hantera webbtrafik med Azure Application Gateway med hjälp av Ansible (förhandsversion)
[Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/) är en lastbalanserare för webbtrafik som gör det möjligt för dig att hantera trafik till dina webbappar. 

Med Ansible kan du automatisera distributionen och konfigurationen av resurser i din miljö. Den här artikeln visar hur du kan använda Ansible för att skapa en Azure Application Gateway och använda den för att hantera trafik för två webbservrar som körs i Azure-containerinstanser. 

I den här guiden får du lära dig att:

> [!div class="checklist"]
> * Konfigurera nätverket
> * Skapa två Azure-containerinstanser med httpd-avbildning
> * Skapa en programgateway med ovanstående Azure-containerinstanser i serverdelspoolen


## <a name="prerequisites"></a>Nödvändiga komponenter
- **Azure-prenumeration** – Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) konto innan du börjar.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Ansible 2.7 krävs för att köra följande exempelspelböcker i den här självstudien. Du kan installera Ansible 2.7 RC-versionen genom att köra `sudo pip install ansible[azure]==2.7.0rc2`. Ansible 2.7 släpps i oktober 2018. Efter det behöver du inte ange versionen här, eftersom standardversionen blir 2.7. 

## <a name="create-a-resource-group"></a>Skapa en resursgrupp
En resursgrupp är en logisk container där Azure-resurser distribueras och hanteras.  

I följande exempel skapas en resursgrupp med namnet **myResourceGroup** på platsen **eastus**.

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus 
  tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"
```

Spara spelboken ovan som *rg.yml*. Om du vill köra spelboken använder du kommandot **ansible-playbook** så här:
```bash
ansible-playbook rg.yml
```

## <a name="create-network-resources"></a>Skapa nätverksresurser 
Du måste skapa ett virtuellt nätverk för att programgatewayen ska kunna kommunicera med andra resurser. 

I följande exempel skapar vi ett virtuellt nätverk med namnet **myVNet**, ett undernät med namnet **myAGSubnet** och en offentlig IP-adress med namnet **myAGPublicIPAddress** med en domän med namnet **mydomain**. 

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus 
    vnet_name: myVNet 
    subnet_name: myAGSubnet 
    publicip_name: myAGPublicIPAddress
    publicip_domain: mydomain
  tasks:
    - name: Create a virtual network
      azure_rm_virtualnetwork:
        name: "{{ vnet_name }}"
        resource_group: "{{ resource_group }}"
        address_prefixes_cidr:
            - 10.1.0.0/16
            - 172.100.0.0/16
        dns_servers:
            - 127.0.0.1
            - 127.0.0.2

    - name: Create a subnet
      azure_rm_subnet:
        name: "{{ subnet_name }}"
        virtual_network_name: "{{ vnet_name }}"
        resource_group: "{{ resource_group }}"
        address_prefix_cidr: 10.1.0.0/24

    - name: Create a public IP address
      azure_rm_publicipaddress:
        resource_group: "{{ resource_group }}" 
        allocation_method: Dynamic
        name: "{{ publicip_name }}"
        domain_name_label: "{{ publicip_domain }}"
```

Spara spelboken ovan som *vnet_create.yml*. Om du vill köra spelboken använder du kommandot **ansible-playbook** så här:
```bash
ansible-playbook vnet_create.yml
```

## <a name="create-backend-servers"></a>Skapa serverdelsservrar
I det här exemplet skapar du två Azure-containerinstanser med httpd-avbildningar som ska användas som serverdelsservrar för programgatewayen.  

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus 
    aci_1_name: myACI1
    aci_2_name: myACI2
  tasks:
    - name: Create a container with httpd image 
      azure_rm_containerinstance:
        resource_group: "{{ resource_group }}"
        name: "{{ aci_1_name }}"
        os_type: linux
        ip_address: public
        location: "{{ location }}"
        ports:
          - 80
        containers:
          - name: mycontainer
            image: httpd
            memory: 1.5
            ports:
              - 80

    - name: Create another container with httpd image 
      azure_rm_containerinstance:
        resource_group: "{{ resource_group }}"
        name: "{{ aci_2_name }}"
        os_type: linux
        ip_address: public
        location: "{{ location }}"
        ports:
          - 80
        containers:
          - name: mycontainer
            image: httpd
            memory: 1.5
            ports:
              - 80
```

Spara spelboken ovan som *aci_create.yml*. Om du vill köra spelboken använder du kommandot **ansible-playbook** så här:
```bash
ansible-playbook aci_create.yml
```

## <a name="create-the-application-gateway"></a>Skapa programgatewayen

Nu kan vi skapa en programgateway. I följande exempel skapar vi en programgateway med namnet **myAppGateway** med serverdel, klientdel och http-konfiguration.  

> [!div class="checklist"]
> * **appGatewayIP** definieras i blocket **gateway_ip_configurations**. Undernätsreferens krävs för gatewayens ip-konfiguration. 
> * **appGatewayBackendPool** definieras i blocket **backend_address_pools**. En programgateway måste ha minst en serverdelsadresspool. 
> * **appGatewayBackendHttpSettings** definieras i blocket **backend_http_settings_collection**. Anger att port 80 och ett HTTP-protokoll används för kommunikation. 
> * **appGatewayHttpListener** definieras i blocket **backend_http_settings_collection**. Standardlyssnaren är associerade med appGatewayBackendPool. 
> * **appGatewayFrontendIP** definieras i blocket **frontend_ip_configurations**. Tilldelar myAGPublicIPAddress till appGatewayHttpListener. 
> * **regel 1** definieras i blocket **request_routing_rules**. Standardhanteringsregeln är associerad med appGatewayHttpListener. 

```yml
- hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    vnet_name: myVNet
    subnet_name: myAGSubnet
    location: eastus
    publicip_name: myAGPublicIPAddress
    appgw_name: myAppGateway
    aci_1_name: myACI1
    aci_2_name: myACI2
  tasks:
    - name: Get info of Subnet
      azure_rm_resource_facts:
        api_version: '2018-08-01'
        resource_group: "{{ resource_group }}"
        provider: network
        resource_type: virtualnetworks
        resource_name: "{{ vnet_name }}"
        subresource:
          - type: subnets
            name: "{{ subnet_name }}"
      register: subnet

    - name: Get info of backend server 2
      azure_rm_resource_facts:
        api_version: '2018-04-01'
        resource_group: "{{ resource_group }}"
        provider: containerinstance
        resource_type: containergroups
        resource_name: "{{ aci_1_name }}"
      register: aci_1_output
    - name: Get info of backend server 2
      azure_rm_resource_facts:
        api_version: '2018-04-01'
        resource_group: "{{ resource_group }}"
        provider: containerinstance
        resource_type: containergroups
        resource_name: "{{ aci_2_name }}"
      register: aci_2_output

    - name: Create instance of Application Gateway
      azure_rm_appgateway:
        resource_group: "{{ resource_group }}"
        name: "{{ appgw_name }}"
        sku:
          name: standard_small
          tier: standard
          capacity: 2
        gateway_ip_configurations:
          - subnet:
              id: "{{ subnet.response[0].id }}"
            name: appGatewayIP
        frontend_ip_configurations:
          - public_ip_address: "{{ publicip_name }}"
            name: appGatewayFrontendIP
        frontend_ports:
          - port: 80
            name: appGatewayFrontendPort
        backend_address_pools:
          - backend_addresses:
              - ip_address: "{{ aci_1_output.response[0].properties.ipAddress.ip }}"
              - ip_address: "{{ aci_2_output.response[0].properties.ipAddress.ip }}"
            name: appGatewayBackendPool
        backend_http_settings_collection:
          - port: 80
            protocol: http
            cookie_based_affinity: enabled
            name: appGatewayBackendHttpSettings
        http_listeners:
          - frontend_ip_configuration: appGatewayFrontendIP
            frontend_port: appGatewayFrontendPort
            name: appGatewayHttpListener
        request_routing_rules:
          - rule_type: Basic
            backend_address_pool: appGatewayBackendPool
            backend_http_settings: appGatewayBackendHttpSettings
            http_listener: appGatewayHttpListener
            name: rule1
```

Spara spelboken ovan som *appgw_create.yml*. Om du vill köra spelboken använder du kommandot **ansible-playbook** så här:
```bash
ansible-playbook appgw_create.yml
```

Det kan ta flera minuter att skapa programgatewayen. 

## <a name="test-the-application-gateway"></a>Testa programgatewayen

I exempelspelboken för nätverksresurser ovan skapades domänen med namnet **mydomain** i **eastus**. Nu du kan navigera till webbläsaren och skriva `http://mydomain.eastus.cloudapp.azure.com`. Då bör du se följande sida som bekräftar att Application Gateway fungerar som förväntat.

![Access Application Gateway](media/ansible-create-configure-application-gateway/applicationgateway.PNG)

## <a name="clean-up-resources"></a>Rensa resurser

Om du inte behöver resurserna kan du ta bort dem genom att köra exemplet nedan. Exemplet tar bort en resursgrupp med namnet **myResourceGroup**. 

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
  tasks:
    - name: Delete a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        state: absent
```

Spara spelboken ovan som *rg_delete*.yml. Om du vill köra spelboken använder du kommandot **ansible-playbook** så här:
```bash
ansible-playbook rg_delete.yml
```

## <a name="next-steps"></a>Nästa steg
> [!div class="nextstepaction"] 
> [Ansible i Azure](https://docs.microsoft.com/azure/ansible/)
