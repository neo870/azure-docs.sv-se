---
title: Begrepp för ändring i LUIS - Språkförståelse
titleSuffix: Azure Cognitive Services
description: Lär dig hur data kan ändras innan förutsägelser i Språkförståelse (LUIS)
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 6db7fd2474688608eb029fce1529ba1d3f00c5d3
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/22/2018
ms.locfileid: "49637178"
---
# <a name="data-alterations"></a>Ändringar av data
LUIS innehåller olika sätt att manipulera uttryck före eller under förutsägelser. Dessa inkluderar åtgärda stavning och åtgärda problem med tidszonen för prebuild datetimeV2. 

## <a name="correct-spelling-errors-in-utterance"></a>Rätta stavfel i uttryck
LUIS använder [Bing stavningskontroll kontrollera API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) att rätta stavfel i uttryck. LUIS måste den nyckel som associeras med den tjänsten. Skapa nyckeln och sedan lägga till nyckeln som en frågesträngsparameter på den [endpoint](https://aka.ms/luis-endpoint-apis). 

Du kan också korrigera stavfel i den **Test** panelen av [att ange nyckeln](luis-interactive-test.md#view-bing-spell-check-corrections-in-test-panel). Nyckeln sparas som en sessionsvariabel i webbläsaren till Test-panelen. Lägga till nyckeln i panelen Test i varje webbläsarsession som du vill att stavning korrigerats. 

Användning av nyckeln på panelen test och på slutpunkten räknas in i den [nyckelanvändning](https://azure.microsoft.com/pricing/details/cognitive-services/spellcheck-api/) kvot. LUIS implementerar stavningskontroll i Bing gränser för textlängd. 

Slutpunkten kräver två parametrar för stavningskorrigeringar ska fungera:

|Param|Värde|
|--|--|
|`spellCheck`|boolesk|
|`bing-spell-check-subscription-key`|[Bing stavningskontroll kontrollera API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) slutpunktsnyckel|

När [Bing stavningskontroll kontrollera API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) upptäcker ett fel och den ursprungliga uttryck korrigerad uttryck returneras tillsammans med förutsägelser från slutpunkten.

```JSON
{
  "query": "Book a flite to London?",
  "alteredQuery": "Book a flight to London?",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.780123
  },
  "entities": []
}
```
 
### <a name="whitelist-words"></a>Lista över tillåtna ord
Stavningskontroll i Bing API som används i LUIS inte stöder en vitlista av ord som ska förbises vid stavningskontroll kontrollera förändras. Om du vill lista för tillåten ord eller förkortningar bearbeta uttryck i klientprogrammet med en lista för tillåten innan du skickar uttryck till LUIS för avsiktlig förutsägelse.

## <a name="change-time-zone-of-prebuilt-datetimev2-entity"></a>Ändra tidszonen för fördefinierade datetimeV2 entitet
När en LUIS-app använder fördefinierade datetimeV2 entiteten, kan ett datetime-värde returneras i svaret förutsägelse. Tidszonen för begäran används för att fastställa rätt datum/tid att returnera. Om förfrågan kommer från en bot eller ett annat centraliserad program innan du kommer till LUIS, korrigera tidszonen LUIS använder. 

### <a name="endpoint-querystring-parameter"></a>Frågeparametern slutpunkt
Tidszonen korrigeras genom att lägga till användarens tidszon för den [endpoint](https://aka.ms/luis-endpoint-apis) med hjälp av den `timezoneOffset` param. Värdet för `timezoneOffset` ska vara positivt eller negativt tal, på bara några minuter att ändra tiden.  

|Param|Värde|
|--|--|
|`timezoneOffset`|positivt eller negativt tal på bara några minuter|

### <a name="daylight-savings-example"></a>Sommartid besparingar exempel
Om du behöver den returnerade fördefinierade datetimeV2 att justera för sommartid, bör du använda den `timezoneOffset` Frågeparametern med en +/-värdet i minuter för den [endpoint](https://aka.ms/luis-endpoint-apis) fråga.

Lägg till 60 minuter: 

https://{region}.API.cognitive.microsoft.com/Luis/v2.0/Apps/{appId}?q=TURN lamporna på? **timezoneOffset = 60**& utförlig = {boolesk} & stavningskontroll = {boolesk} & mellanlagring = {boolesk} & bing-stavningskontroll-kontroll-subscription-key = {sträng} & logg = {boolesk}

Ta bort 60 minuter: 

https://{region}.API.cognitive.microsoft.com/Luis/v2.0/Apps/{appId}?q=TURN lamporna på? **timezoneOffset = – 60**& utförlig = {boolesk} & stavningskontroll = {boolesk} & mellanlagring = {boolesk} & bing-stavningskontroll-kontroll-subscription-key = {sträng} & logg = {boolesk}

## <a name="c-code-determines-correct-value-of-timezoneoffset"></a>C#-kod anger rätt värde för timezoneOffset
Följande C#-kod använder den [TimeZoneInfo](https://docs.microsoft.com/dotnet/api/system.timezoneinfo?view=netframework-4.7.1) klassens [FindSystemTimeZoneById](https://docs.microsoft.com/dotnet/api/system.timezoneinfo.findsystemtimezonebyid?view=netframework-4.7.1#examples) metod för att fastställa rätt `timezoneOffset` baserat på systemtid:

```CSharp
// Get CST zone id
TimeZoneInfo targetZone = TimeZoneInfo.FindSystemTimeZoneById("Central Standard Time");

// Get local machine's value of Now
DateTime utcDatetime = DateTime.UtcNow;

// Get Central Standard Time value of Now
DateTime cstDatetime = TimeZoneInfo.ConvertTimeFromUtc(utcDatetime, targetZone);

// Find timezoneOffset
int timezoneOffset = (int)((cstDatetime - utcDatetime).TotalMinutes);
```

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Rätt stavfel i den här självstudiekursen](luis-tutorial-bing-spellcheck.md)