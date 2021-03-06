---
title: Översikt över funktionstyper och funktioner för varaktiga funktioner – Azure
description: Lär dig vilka typer av funktioner och roller som tillåter funktionen att funktionen kommunikation som en del av en beständig funktionen orchestration.
services: functions
author: jeffhollan
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 07/04/2018
ms.author: azfuncdf
ms.openlocfilehash: 24889765369a2af7d87ddb0fdc8f7c9f91ac66fd
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44091399"
---
# <a name="overview-of-function-types-and-features-for-durable-functions-azure-functions"></a>Översikt över funktionstyper och funktioner för varaktiga funktioner (Azure Functions)

Azure varaktiga funktioner ger tillståndskänslig orkestrering av körning av funktion.  En hållbar funktion är en lösning som består av olika Azure-funktioner.  Var och en av dessa funktioner kan spela upp olika roller som en del av en orkestrering.  Följande dokument innehåller en översikt över typerna av funktioner som ingår i en hållbar funktionen dirigering.  Den innehåller också vissa vanliga mönster i ansluter funktioner tillsammans.  

![Typer av varaktiga funktioner][1]  

## <a name="types-of-functions"></a>Typer av funktioner

### <a name="activity-functions"></a>Aktivitetsfunktioner

Aktivitetsfunktioner är den grundläggande enheten för arbete i en hållbar dirigering.  Aktivitetsfunktioner är de funktioner och uppgifter som dirigeras i processen.  Du kan till exempel skapa en hållbar funktion att bearbeta en order - Kontrollera lagret, debitera kunden eller skapa en leverans.  Var och en av dessa uppgifter är en funktion för aktiviteten.  Aktivitetsfunktioner har inte några begränsningar i vilken typ av arbete som du kan göra i dem.  De kan skrivas på valfritt språk som stöds av Azure Functions.  Hållbar uppgift framework garanterar att varje funktion kallad aktivitet körs minst en gång under en orkestrering.

En aktivitet-funktion måste aktiveras via en [aktivitet utlösaren](durable-functions-bindings.md#activity-triggers).  Den här funktionen kommer att få en [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) som en parameter. Du kan också binda utlösaren till alla andra objekt att skicka in indata till funktionen.  Funktionen aktivitet kan också returnera värden till orchestrator.  Om skicka eller returnerar flera värden från en aktivitet-funktion, kan du [utnyttjar tupplar eller matriser](durable-functions-bindings.md#passing-multiple-parameters).  Aktivitetsfunktioner kan bara aktiveras från en orchestration-instans.  Medan kod kan delas mellan en aktivitet-funktion och en annan funktion (till exempel en HTTP-utlöst funktion), kan varje funktion bara ha en utlösare.

Mer information och exempel finns i den [varaktiga funktioner bindning artikeln](durable-functions-bindings.md#activity-triggers).

### <a name="orchestrator-functions"></a>Orchestrator-funktioner

Orchestrator-funktioner är helt i linje med en varaktig funktion.  Orchestrator-funktioner beskrivs de sätt och ordningen åtgärder utförs.  Orchestrator-funktioner beskrivs orchestration i kod (C# eller JavaScript), enligt den [varaktiga funktioner översikt](durable-functions-overview.md).  En orkestrering kan ha många olika typer av åtgärder, t.ex [Aktivitetsfunktioner](#activity-functions), [underordnade orkestreringar](#sub-orchestrations), [väntar på externa händelser](#external-events), och [ timers](#durable-timers).  

En orchestrator-funktion måste aktiveras via en [orchestration utlösaren](durable-functions-bindings.md#orchestration-triggers).

En initierare har startats med en [orchestrator klienten](#client-functions) som kan själva utlösas från valfri källa (HTTP, köer, händelseströmmar).  Varje instans av en orkestrering har en instans identifierare som kan vara autogenererade (rekommenderas) eller användare.  Den här identifieraren kan användas för att [hantera instanser](durable-functions-instance-management.md) för dirigering.

Mer information och exempel finns i den [varaktiga funktioner bindning artikeln](durable-functions-bindings.md#orchestration-triggers).

### <a name="client-functions"></a>Klientfunktioner

Klientfunktioner är de utlösta funktioner som skapar nya instanser av en orkestrering.  De är startpunkten för att skapa en instans av en beständig dirigering.  Klientfunktioner kan utlösas av eventuella utlösare (HTTP, köer, händelseströmmar, etc.) och skrivna på valfritt språk som stöds av appen.  Utöver utlösaren klientfunktioner har en [orkestreringsklient](durable-functions-bindings.md#orchestration-client) bindning som gör att de kan skapa och hantera varaktiga orkestreringar.  Det mest grundläggande exemplet på en klient-funktion är en HTTP-utlöst funktion som startar en orchestrator-funktion och returnerar svaret Kontrollera status som [följande exempel](durable-functions-http-api.md#http-api-url-discovery).

Mer information och exempel finns i den [varaktiga funktioner bindning artikeln](durable-functions-bindings.md#orchestration-client).

## <a name="features-and-patterns"></a>Funktioner och mönster

### <a name="sub-orchestrations"></a>Sub-orkestreringar

Förutom anropar Aktivitetsfunktioner kan orchestrator-funktioner anropa andra orchestrator-funktioner. Du kan till exempel skapa en större dirigering av ett bibliotek av orchestrator-funktioner. Eller du kan köra flera instanser av en orchestrator-funktion parallellt.

Mer information och exempel finns i den [underordnade orchestration artikeln](durable-functions-sub-orchestrations.md).

### <a name="durable-timers"></a>Hållbar timers

[Varaktiga funktioner](durable-functions-overview.md) ger *varaktiga timers* för användning i orchestrator-funktioner att implementera fördröjningar eller ställa in timeouter på asynkrona åtgärder. Hållbar timers ska användas i orchestrator-funktioner i stället för `Thread.Sleep` och `Task.Delay` (C#) eller `setTimeout()` och `setInterval()` (JavaScript).

Mer information och exempel på varaktiga timers kan hittas i den [varaktiga timers artikeln](durable-functions-timers.md)

### <a name="external-events"></a>Externa händelser

Orchestrator-funktioner kan vänta på externa händelser att uppdatera en orchestration-instans. Den här funktionen i varaktiga funktioner används ofta för hantering av mänsklig interaktion eller andra externa återanrop.

Mer information och exempel finns i den [externa händelser artikeln](durable-functions-external-events.md).

### <a name="error-handling"></a>Felhantering

Hållbar funktionen orkestreringar implementeras i kod och kan använda funktioner för felhantering i programmeringsspråket.  Det innebär att mönster som ”try-/ catch” fungerar i din orkestrering.  Varaktiga funktioner levereras också med vissa inbyggda återförsöksprinciper.  En åtgärd kan fördröja och försök aktiviteter automatiskt på undantag.  Återförsök kan du hantera tillfälliga undantag utan att behöva lämna dirigering.

Mer information och exempel finns i den [felhantering artikeln](durable-functions-error-handling.md).

### <a name="cross-function-app-communication"></a>Kommunikation mellan funktionsapp

En hållbar orchestration lagras vanligtvis i en kontext för en enskild funktion-app, finns men det mönster så att du kan koordinera orkestreringar mellan många funktionsappar.  Även om kommunikationen mellan appar kan hända via HTTP, med hjälp av beständiga ramverket för varje aktivitet innebär att du kan fortfarande ha en beständig process i två appar.

Ett exempel på en mellan funktionsapp orchestration i C# finns nedan.  En aktivitet börjar externa dirigering. En annan aktivitet sedan hämtar och returnerar status.  Orchestrator ska vänta tills statusen ska vara klar innan du fortsätter.

```csharp
[FunctionName("OrchestratorA")]
public static async Task RunRemoteOrchestrator(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    // Do some work...

    // Call a remote orchestration
    string statusUrl = await context.CallActivityAsync<string>(
        "StartRemoteOrchestration", "OrchestratorB");

    // Wait for the remote orchestration to complete
    while (true)
    {
        bool isComplete = await context.CallActivityAsync<bool>("CheckIsComplete", statusUrl);
        if (isComplete)
        {
            break;
        }

        await context.CreateTimer(context.CurrentUtcDateTime.AddMinutes(1), CancellationToken.None);
    }

    // B is done. Now go do more work...
}

[FunctionName("StartRemoteOrchestration")]
public static async Task<string> StartRemoteOrchestration([ActivityTrigger] string orchestratorName)
{
    using (var response = await HttpClient.PostAsync(
        $"https://appB.azurewebsites.net/orchestrations/{orchestratorName}",
        new StringContent("")))
    {
        string statusUrl = await response.Content.ReadAsAsync<string>();
        return statusUrl;
    }
}

[FunctionName("CheckIsComplete")]
public static async Task<bool> CheckIsComplete([ActivityTrigger] string statusUrl)
{
    using (var response = await HttpClient.GetAsync(statusUrl))
    {
        // 200 = Complete, 202 = Running
        return response.StatusCode == HttpStatusCode.OK;
    }
}
```

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Fortsätt att läsa dokumentationen för varaktiga funktioner](durable-functions-bindings.md)

> [!div class="nextstepaction"]
> [Installera tillägget varaktiga funktioner och -exempel](durable-functions-install.md)

<!-- Media references -->
[1]: media/durable-functions-types-features-overview/durable-concepts.png
