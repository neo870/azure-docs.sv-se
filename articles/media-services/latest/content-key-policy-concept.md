---
title: Innehålls-principer för nycklar i Azure Media Services | Microsoft Docs
description: Den här artikeln innehåller en förklaring av vad Innehållsprinciper nyckel är och hur de används av Azure Media Services.
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
ms.openlocfilehash: 24597c36ab6569abb53c7be331631556e2abfe66
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50085970"
---
# <a name="content-key-policies"></a>Viktiga innehållsprinciper

Du kan använda Azure Media Services för att skydda dina mediefiler från den tidpunkt som den lämnar din dator via lagrings-, bearbetnings- och leverans. Med medietjänster kan du leverera live och på begäran innehåll dynamiskt krypterad med Advanced Encryption Standard (AES-128) eller någon av de tre största digital rights management (DRM) system: Microsoft PlayReady och Google Widevine Apple FairPlay. Media Services tillhandahåller också en tjänst för att leverera AES-nycklar och DRM (PlayReady, Widevine och FairPlay) licenser till auktoriserade klienter.

I Azure Media Services v3 kan principer för innehåll-nycklar du ange hur innehållsnyckeln levereras om du vill avsluta klienter via komponenten leverans av Media Services-nyckel. Mer information finns i [Content protection översikt](content-protection-overview.md).

## <a name="contentkeypolicies-definition"></a>ContentKeyPolicies definition

I följande tabell visar de ContentKeyPolicy egenskaper och ger definitionerna.

|Namn|Beskrivning|
|---|---|
|id|Fullständigt kvalificerade resurs-ID för resursen.|
|namn|Namnet på resursen.|
|Properties.Created |Skapandedatum för principen|
|properties.description |En beskrivning för principen.|
|properties.lastModified|Senaste ändringsdatum för principen|
|Properties.Options |Nyckeln principalternativen.|
|properties.policyId    |Äldre princip-ID.|
|typ   |Typ av resursen.|

Läs den fullständiga definitionen [Innehållsprinciper nyckeln](https://docs.microsoft.com/rest/api/media/contentkeypolicies).

## <a name="filtering-ordering-paging"></a>Filtrering, skrivordning, växling

Media Services har stöd för följande OData-frågealternativ för ContentKeyPolicies: 

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

I följande tabell visar hur dessa alternativ kan användas på StreamingPolicy egenskaper: 

|Namn|Filter|Beställa|
|---|---|---|
|id|||
|namn|Eq, ne, ge, le, gt, lt|Stigande och fallande|
|Properties.Created |Eq, ne, ge, le, gt, lt|Stigande och fallande|
|properties.description |Eq, ne, ge, le, gt, lt||
|properties.lastModified    |Eq, ne, ge, le, gt, lt|Stigande och fallande|
|Properties.Options |||
|properties.policyId    |Eq, ne||
|typ   |||

### <a name="pagination"></a>Sidbrytning

Sidbrytning stöds för var och en av fyra aktiverade sorteringsordningar. För närvarande är sidstorleken 10.

> [!TIP]
> Du bör alltid använda nästa länk för att räkna upp samlingen och inte är beroende av en viss storlek.

Om ett frågesvar innehåller många objekt, tjänsten returnerar en ”\@odata.nextLink” egenskapen för att hämta nästa sida i resultatet. Detta kan användas för att bläddra igenom hela resultatmängden. Du kan inte konfigurera sidstorleken. 

Om StreamingPolicy skapas eller tas bort vid bläddring genom samlingen, syns ändringarna i de returnerade resultaten (om dessa ändringar finns i en del av den samling som inte har hämtats.) 

I följande C#-exempel visar hur du räknar upp via alla ContentKeyPolicies i kontot.

```csharp
var firstPage = await MediaServicesArmClient.ContentKeyPolicies.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.ContentKeyPolicies.ListNextAsync(currentPage.NextPageLink);
}
```

REST-exempel finns i [nyckel Innehållsprinciper - lista](https://docs.microsoft.com/rest/api/media/contentkeypolicies/list)

## <a name="next-steps"></a>Nästa steg

[Använda dynamisk kryptering för AES-128 och nyckelleveranstjänst](protect-with-aes128.md)

[Använda DRM dynamisk kryptering och licens video-on-demand](protect-with-drm.md)
