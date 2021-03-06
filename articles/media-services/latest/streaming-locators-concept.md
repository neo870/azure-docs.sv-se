---
title: Strömning positionerare i Azure Media Services | Microsoft Docs
description: Den här artikeln innehåller en förklaring av vad positionerare för direktuppspelning är och hur de används av Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/22/2018
ms.author: juliako
ms.openlocfilehash: cb34855fc9451679c885eebb0ef5a2fab0be8c57
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50086853"
---
# <a name="streaming-locators"></a>Positionerare för direktuppspelning

Om du vill ge klienterna en URL som de kan använda för att spela upp kodade video eller ljud filer, måste du skapa en [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) och skapa strömmande URL: er. Mer information finns i [Stream en fil](stream-files-dotnet-quickstart.md).

## <a name="streaminglocator-definition"></a>StreamingLocator definition

I följande tabell visar de StreamingLocator egenskaper och ger definitionerna.

|Namn|Beskrivning|
|---|---|
|id |Fullständigt kvalificerade resurs-ID för resursen.|
|namn   |Namnet på resursen.|
|properties.alternativeMediaId|Alternativa Media-ID för den här positionerare för direktuppspelning.|
|properties.assetName   |Tillgångsnamn|
|properties.contentKeys |ContentKeys som används av den här Strömningspositionerare.|
|Properties.Created |Tiden för skapandet av den Strömningspositionerare.|
|properties.defaultContentKeyPolicyName|Namnet på ContentKeyPolicy som används av den här Strömningspositionerare.|
|properties.endTime |Sluttid för den Strömningspositionerare.|
|properties.startTime|Starttiden för den Strömningspositionerare.|
|properties.streamingLocatorId|StreamingLocatorId av positionerare för direktuppspelning.|
|properties.streamingPolicyName |Namnet på principen för direktuppspelning används av den här Strömningspositionerare. Ange namnet på Streaming principen som du skapade eller Använd någon av de fördefinierade principer för direktuppspelning. Den fördefinierade Streaming principer som är tillgängliga är: 'Predefined_DownloadOnly', 'Predefined_ClearStreamingOnly', 'Predefined_DownloadAndClearStreaming', 'Predefined_ClearKey', 'Predefined_MultiDrmCencStreaming' och ' Predefined_ MultiDrmStreaming'|
|typ|Typ av resursen.|

Läs den fullständiga definitionen [positionerare för direktuppspelning](https://docs.microsoft.com/rest/api/media/streaminglocators).

## <a name="filtering-ordering-paging"></a>Filtrering, skrivordning, växling

Media Services har stöd för följande OData-frågealternativ för positionerare för direktuppspelning: 

* $filter 
* $orderby 
* $top 
* $skiptoken 

Operatorn beskrivning:

* EQ = lika med
* Ne = inte lika med
* Ge = större än eller lika med
* Le = mindre än eller lika med
* Gt = större än
* Lt = mindre än

### <a name="filteringordering"></a>Filtrering/ordning

I följande tabell visar hur dessa alternativ kan användas på StreamingLocator egenskaper: 

|Namn|Filter|Beställa|
|---|---|---|
|id |||
|namn|Eq, ne, ge, le, gt, lt|Stigande och fallande|
|properties.alternativeMediaId  |||
|properties.assetName   |||
|properties.contentKeys |||
|Properties.Created |Eq, ne, ge, le, gt, lt|Stigande och fallande|
|properties.defaultContentKeyPolicyName |||
|properties.endTime |Eq, ne, ge, le, gt, lt|Stigande och fallande|
|properties.startTime   |||
|properties.streamingLocatorId  |||
|properties.streamingPolicyName |||
|typ   |||

### <a name="pagination"></a>Sidbrytning

Sidbrytning stöds för var och en av fyra aktiverade sorteringsordningar. För närvarande är sidstorleken 10.

> [!TIP]
> Du bör alltid använda nästa länk för att räkna upp samlingen och inte är beroende av en viss storlek.

Om ett frågesvar innehåller många objekt, tjänsten returnerar en ”\@odata.nextLink” egenskapen för att hämta nästa sida i resultatet. Detta kan användas för att bläddra igenom hela resultatmängden. Du kan inte konfigurera sidstorleken. 

Om StreamingLocators skapas eller tas bort vid bläddring genom samlingen, syns ändringarna i de returnerade resultaten (om dessa ändringar finns i en del av den samling som inte har hämtats.) 

I följande C#-exempel visar hur du räknar upp via alla StreamingLocators i kontot.

```csharp
var firstPage = await MediaServicesArmClient.StreamingLocators.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.StreamingLocators.ListNextAsync(currentPage.NextPageLink);
}
```

REST-exempel finns i [Streaming positionerare - lista](https://docs.microsoft.com/rest/api/media/streaminglocators/list)

## <a name="next-steps"></a>Nästa steg

[Strömma en fil](stream-files-dotnet-quickstart.md)
