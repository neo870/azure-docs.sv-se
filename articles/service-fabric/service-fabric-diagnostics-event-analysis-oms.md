---
title: Azure Service Fabric-Händelseanalys med Log Analytics | Microsoft Docs
description: Läs mer om att visualisera och analysera händelser med hjälp av Log Analytics för övervakning och diagnostik för Azure Service Fabric-kluster.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/29/2018
ms.author: srrengar
ms.openlocfilehash: 6dee895ba9fc024baac0500619b7d6cc62167b6d
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/18/2018
ms.locfileid: "49404485"
---
# <a name="event-analysis-and-visualization-with-log-analytics"></a>Händelseanalys och visualisering med Log Analytics
Log Analytics samlar in och analyserar telemetri från program och tjänster i molnet och ger analysverktyg som hjälper dig att maximera tillgänglighet och prestanda. Den här artikeln beskrivs hur du kör frågor i Log Analytics för att få insikter och felsöka vad som händer i ditt kluster. Följande vanliga frågor tas upp:

* Hur felsöker jag health-händelser?
* Hur vet jag när en nod stängs av?
* Hur vet jag om min programtjänster har startats eller stoppats?

## <a name="log-analytics-workspace"></a>Log Analytics-arbetsyta

Log Analytics samlar in data från hanterade resurser, inklusive en Azure storage-tabell eller en agent och underhåller i ett centrallager. Data kan sedan användas för analys, aviseringar och visualisering eller ytterligare export. Log Analytics har stöd för händelser, prestandadata eller anpassade data. Kolla in [steg för att konfigurera diagnostics-tillägg för att aggregera händelser](service-fabric-diagnostics-event-aggregation-wad.md) och [steg för att skapa en Log Analytics-arbetsyta för att läsa från händelser i storage](service-fabric-diagnostics-oms-setup.md) att kontrollera att data flödar till Log Analytics .

När data har tagits emot av Log Analytics, Azure har flera *hanteringslösningar* som är förpaketerade lösningar för att övervaka inkommande data, anpassade till flera scenarier. Dessa inkluderar en *Service Fabric-analys* lösning och en *behållare* lösning som är de två mest relevanta som diagnostik och övervakning av när du använder Service Fabric-kluster. Den här artikeln beskriver hur du använder Service Fabric-analys-lösning som har skapats med arbetsytan.

## <a name="access-the-service-fabric-analytics-solution"></a>Lösning för Service Fabric-analys

1. Gå till resursgruppen där du skapade Service Fabric-analys-lösningen i Azure-portalen.

2. Välj resursen som **ServiceFabric\<nameOfOMSWorkspace\>**.

2. Sammanfattningsvis visas paneler i form av ett diagram för var och en av de lösningar som är aktiverad, inklusive ett för Service Fabric. Klicka på den **Service Fabric** graph (första bilden nedan) för att fortsätta till Service Fabric-analys-lösningen (andra bilden nedan).

    ![Service Fabric-lösning](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_summary.PNG)

    ![Service Fabric-lösning](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_solution.PNG)

Bilden ovan är startsidan för Service Fabric-analys-lösning. Det här är en ögonblicksbild vy över vad som händer i ditt kluster. Om du har aktiverat diagnostik när klustret har skapats kan du se händelser för 

* [Användningskanal](service-fabric-diagnostics-event-generation-operational.md): på högre nivå åtgärder som utförs av Service Fabric-plattformen (insamling av systemtjänster).
* [Reliable Actors programming modellhändelser](service-fabric-reliable-actors-diagnostics.md)
* [Reliable Services programming modellhändelser](service-fabric-reliable-services-diagnostics.md)

>[!NOTE]
>Förutom den operativa kanalen, mer detaljerad händelser kan samlas in av [uppdatera konfigurationen av diagnostiktillägget](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations).

### <a name="view-service-fabric-events-including-actions-on-nodes"></a>Visa Service Fabric-händelser, inklusive åtgärder på noder

1. På sidan Service Fabric-analys klickar du på diagrammet för **Service Fabric-händelser**.

    ![Service Fabric-lösningen, Användningskanal](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events_selection.png)

2. Klicka på **lista** att visa händelser i en lista. När här visas alla systemhändelser som har samlats in. Referens så de kommer från WADServiceFabricSystemEventsTable i Azure Storage-konto och på samma sätt reliable services och actors-händelser som visas bredvid är från dessa respektive tabeller.
    
    ![Fråga Användningskanal](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events.png)

Du kan också klicka på förstoringsglaset till vänster och använda Kusto-frågespråk för att hitta det du letar efter. Du kan exempelvis använda följande fråga för att hitta alla åtgärder som vidtas på noderna i klustret. Händelse-ID som används nedan hittar du i den [användningskanal händelser](service-fabric-diagnostics-event-generation-operational.md).

```kusto
ServiceFabricOperationalEvent
| where EventId < 25627 and EventId > 25619 
```

Du kan fråga på flera fält, till exempel de specifika noderna (dator) systemtjänsten (Aktivitetsnamn).

### <a name="view-service-fabric-reliable-service-and-actor-events"></a>Visa Service Fabric Reliable Service och aktören händelser

1. Service Fabric-analys-sidan, klicka på diagrammet för **Reliable Services**.

    ![Service Fabric-lösningen Reliable Services](media/service-fabric-diagnostics-event-analysis-oms/oms_reliable_services_events_selection.png)

2. Klicka på **lista** att visa händelser i en lista. Här kan du se händelser från tillförlitliga tjänster. Du kan se olika händelser för när tjänstens runasync startades och slutfördes som vanligtvis sker på distributioner och uppgraderingar. 

    ![Fråga Reliable Services](media/service-fabric-diagnostics-event-analysis-oms/oms_reliable_service_events.png)

Tillförlitliga aktörer händelser kan visas på liknande sätt. Om du vill konfigurera detaljerad händelser för reliable actors, måste du ändra den `scheduledTransferKeywordFilter` i konfigurationen för diagnostiktillägg (se nedan). Information om värdena för dessa finns i den [referens för reliable actors-händelser](service-fabric-reliable-actors-diagnostics.md#keywords).

```json
"EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
```

Kusto-frågespråket är kraftfullt. En annan värdefull fråga som du kan köra är att ta reda på vilka noder som genererar flest händelser. Frågan i skärmbilden nedan visar operativa händelser i Service Fabric aggregeras med specifik tjänst och nod.

![Fråga händelser per nod](media/service-fabric-diagnostics-event-analysis-oms/oms_kusto_query.png)

## <a name="next-steps"></a>Nästa steg

* Om du vill aktivera infrastrukturövervakning d.v.s. prestandaräknare, gå till [att lägga till Log Analytics-agenten](service-fabric-diagnostics-oms-agent.md). Agenten samlar in prestandaräknare och lägger till dem i din befintliga arbetsyta.
* Log Analytics tillhandahåller en Gateway (HTTP framåt Proxy) som kan användas för att skicka data till Log Analytics för lokala kluster. Läs mer om att [ansluter datorer utan Internetåtkomst till Log Analytics med Log Analytics-gateway](../log-analytics/log-analytics-oms-gateway.md).
* Konfigurera [automatiserade aviseringar](../log-analytics/log-analytics-alerts.md) att underlätta identifiering och diagnostik.
* Bekanta dig med funktionerna [loggsökning och frågor](../log-analytics/log-analytics-log-searches.md) i Log Analytics.
* Få en mer detaljerad översikt över Log Analytics och den erbjuder, läsa [vad är Log Analytics?](../operations-management-suite/operations-management-suite-overview.md).
