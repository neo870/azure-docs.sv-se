---
title: En snabbstart för att skapa en Java-webbapp i Azure App Service i Linux
description: I den här snabbstarten distribuerar du din första Java Hello World med Azure App Service i Linux på bara några minuter.
services: app-service\web
documentationcenter: ''
author: msangapu
manager: cfowler
editor: ''
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: quickstart
ms.date: 03/07/2018
ms.author: msangapu
ms.custom: mvc
ms.openlocfilehash: e286942f092d2e8c22824a18f5a6503d04a1be0c
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50247563"
---
# <a name="quickstart-create-a-java-web-app-in-app-service-on-linux"></a>Snabbstart: Skapa en Java-webbapp i App Service i Linux

Med [App Service i Linux](app-service-linux-intro.md) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst som utgår från operativsystemet Linux. Den här snabbstarten visar hur du använder [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) med [Maven-plugin-program för Azure Web Apps (förhandsversion)](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) för att distribuera en webbarkivfil (WAR) för Java-webbapp.

![Exempelapp som körs i Azure](media/quickstart-java/java-hello-world-in-browser.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-java-app"></a>Skapa en Java-app

Kör följande Maven-kommando i Cloud Shell-prompten för att skapa en ny webbapp med namnet `helloworld`:

```bash
mvn archetype:generate -DgroupId=example.demo -DartifactId=helloworld -DarchetypeArtifactId=maven-archetype-webapp
```

## <a name="configure-the-maven-plugin"></a>Konfigurera Maven-plugin-programmet

Distribuera från Maven genom att använda kodredigeraren i Cloud Shell för att öppna projektets `pom.xml`-fil i katalogen `helloworld`. 

```bash
code pom.xml
```

Lägg sedan till följande plugin-definition i `<build>`-elementet i filen `pom.xml`.

```xml
<plugins>
    <!--*************************************************-->
    <!-- Deploy to Tomcat in App Service Linux           -->
    <!--*************************************************-->
      
    <plugin>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-webapp-maven-plugin</artifactId>
        <version>1.4.0</version>
        <configuration>
   
            <!-- Web App information -->
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appName>${WEBAPP_NAME}</appName>
            <region>${REGION}</region>
   
            <!-- Java Runtime Stack for Web App on Linux-->
            <linuxRuntime>tomcat 8.5-jre8</linuxRuntime>
   
        </configuration>
    </plugin>
</plugins>
```    


> [!NOTE] 
> I den här artikeln arbetar vi endast med Java-appar som paketerats i WAR-filer. Plugin-programmet stöder också JAR-webbprogram. Läs informationen om att [distribuera en Java SE JAR-fil till App Service på Linux](https://docs.microsoft.com/java/azure/spring-framework/deploy-spring-boot-java-app-with-maven-plugin?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) om du vill testa det.


Uppdatera följande platshållare i konfigurationen av plugin-program:

| Platshållare | Beskrivning |
| ----------- | ----------- |
| `RESOURCEGROUP_NAME` | Namnet på den nya resursgrupp där du vill skapa din webbapp. Genom att lägga alla resurser för en app i en grupp, kan du hantera dem tillsammans. Genom att till exempel ta bort resursgruppen skulle du ta bort alla resurser som är associerade med appen. Uppdatera det här värdet med ett unikt nytt resursgruppnamn, till exempel *TestResources*. Du använder den här resursgruppens namn för att rensa alla Azure-resurser i ett senare avsnitt. |
| `WEBAPP_NAME` | Appnamnet är en del av värdnamnet för webbappen när den distribueras till Azure (WEBAPP_NAME.azurewebsites.net). Uppdatera det här värdet med ett unikt namn för den nya Azure-webbappen, som blir värd för din Java-app, till exempel *contoso*. |
| `REGION` | En Azure-region där webbappen hanteras, till exempel `westus2`. Du kan hämta en lista över regioner från Cloud Shell eller CLI med kommandot `az account list-locations`. |

## <a name="deploy-the-app"></a>Distribuera appen

Distribuera din Java-app till Azure med följande kommando:

```bash
mvn package azure-webapp:deploy
```

När distributionen är klar bläddrar du till den distribuerade tillämpningen med hjälp av följande webbadress i webbläsaren, till exempel `http://<webapp>.azurewebsites.net/helloworld`. 

![Exempelapp som körs i Azure](media/quickstart-java/java-hello-world-in-browser-curl.png)

**Grattis!** Du har distribuerat din första Java-app till App Service på Linux.


[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]


## <a name="next-steps"></a>Nästa steg

I den här snabbstarten använde du Maven för att skapa en Java-webbapp, konfigurerade [Maven-plugin-programmet för Azure Web Apps ](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) och distribuerade sedan en webbarkivpaketerad Java-app till App Service i Linux. Om du vill veta hur du ansluter databaser, konfigurerar loggning och övervakning, konfigurerar säkerhet och anger körningsalternativ fortsätter du till utvecklarguiden för Java för App Service i Linux.

> [!div class="nextstepaction"]
> [Utvecklarguide för App Service på Linux Java](app-service-linux-java.md)

