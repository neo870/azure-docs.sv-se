---
title: Skapa en intern lastbalanserare för Azure Cloud Services | Microsoft Docs
description: Lär dig hur du skapar en intern lastbalanserare med hjälp av PowerShell i den klassiska distributionsmodellen
services: load-balancer
documentationcenter: na
author: genlin
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: genli
ms.openlocfilehash: fb543e705c613ee1e69564e391080cf5f8357ba5
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50417386"
---
# <a name="get-started-creating-an-internal-load-balancer-classic-for-cloud-services"></a>Komma igång med att skapa en intern lastbalanserare (klassisk) för molntjänster

> [!div class="op_single_selector"]
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [Molntjänster](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).  Den här artikeln beskriver den klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen. Lär dig hur du [utför dessa steg med hjälp av Resource Manager-modellen](load-balancer-get-started-ilb-arm-ps.md).

## <a name="configure-internal-load-balancer-for-cloud-services"></a>Konfigurera en intern lastbalanserare för molntjänster

Interna lastbalanserare stöds för både virtuella datorer och molntjänster. Slutpunkten för en intern lastbalanserare som skapas i en molntjänst som finns utanför ett regionalt virtuellt nätverk kan endast nås i molntjänsten.

Du måste konfigurera interna lastbalanserare när du skapar den första distributionen i molntjänsten, som du ser i exemplet nedan.

> [!IMPORTANT]
> Innan du kör stegen nedan måste du ha ett virtuellt nätverk för molndistributionen. Du behöver namnet på det virtuella nätverket och undernätet för att kunna skapa den interna belastningsutjämningen.

### <a name="step-1"></a>Steg 1

Öppna tjänstkonfigurationsfilen (.cscfg) för din distribution i Visual Studio och lägg till följande avsnitt för att skapa den interna belastningsutjämningen under det sista ”`</Role>`”-objektet för nätverkskonfigurationen.

```xml
<NetworkConfiguration>
    <LoadBalancers>
    <LoadBalancer name="name of the load balancer">
        <FrontendIPConfiguration type="private" subnet="subnet-name" staticVirtualNetworkIPAddress="static-IP-address"/>
    </LoadBalancer>
    </LoadBalancers>
</NetworkConfiguration>
```

Nu ska vi lägga till värdena för nätverkskonfigurationsfilen så att du ser hur det ser ut. I det här exemplet antar vi att du har skapat ett VNet med namnet ”test_vnet” och undernätet 10.0.0.0/24 med namnet test_subnet och den statiska IP-adressen 10.0.0.4. Lastbalanseraren ges namnet testLB.

```xml
<NetworkConfiguration>
    <LoadBalancers>
    <LoadBalancer name="testLB">
        <FrontendIPConfiguration type="private" subnet="test_subnet" staticVirtualNetworkIPAddress="10.0.0.4"/>
    </LoadBalancer>
    </LoadBalancers>
</NetworkConfiguration>
```

Mer information om lastbalanseringsschemat finns i [Add load balancer](https://msdn.microsoft.com/library/azure/dn722411.aspx) (Lägga till en lastbalanserare).

### <a name="step-2"></a>Steg 2

Ändra tjänstdefinitionsfilen (.csdef) för att lägga till slutpunkter för den interna belastningsutjämningen. Så fort en rollinstans skapas lägger tjänstdefinitionsfilen till rollinstansen i den interna belastningsutjämningen.

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancer="load-balancer-name" />
    </Endpoints>
</WorkerRole>
```

Vi använder värdena i exemplet ovan och lägger till dem i tjänstdefinitionsfilen.

```xml
<WorkerRole name="WorkerRole1" vmsize="A7" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="endpoint1" protocol="http" localPort="80" port="80" loadBalancer="testLB" />
    </Endpoints>
</WorkerRole>
```

Nätverkstrafiken kommer att lastbalanseras med lastbalanseraren testLB. Port 80 används för inkommande förfrågningar och samma port används för att skicka trafik till arbetsrollinstanser.

## <a name="next-steps"></a>Nästa steg

[Konfigurera ett distributionsläge för lastbalanserare med hjälp av käll-IP-tilldelning](load-balancer-distribution-mode.md)

[Konfigurera timeout-inställningar för inaktiv TCP för en lastbalanserare](load-balancer-tcp-idle-timeout.md)

