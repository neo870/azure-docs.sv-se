---
title: Exempel på Bing insights – Bing Visual Search
titleSuffix: Azure Cognitive Services
description: Visar exempel på information om bilder visas på Bing.com.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: scottwhi
ms.openlocfilehash: 40b9685c56dd494e82c05bff9ed75f425a3006ff
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/09/2018
ms.locfileid: "48886635"
---
# <a name="examples-of-bing-insights-usage"></a>Exempel på användning av Bing insikter

Det här avsnittet innehåller exempel på hur Bing kan visa insikter på Bing.com.

## <a name="pagesincluding-insight-example"></a>PagesIncluding insight-exempel

Nedan visas hur Bing kan visa webbsidor som innehåller bilden. I exemplet visas en länk till den första webbsidan och användaren kan expandera eller komprimera listan över andra webbsidor som innehåller bilden.

![Utökade sidor, inklusive](./media/pages-including.PNG)


## <a name="shoppingsources-insight-example"></a>ShoppingSources insight-exempel

Nedan visas hur Bing kan visas i källorna för produkter som visas i bild.

![Shopping källor](./media/shopping-sources.PNG)


## <a name="visualsearch-insight-example"></a>VisualSearch insight-exempel

Följande visar hur Bing kan visa visuellt likartade bilder (se **relaterade bilder** i det här exemplet).

![Snarlika bilder](./media/similar-images.PNG)

## <a name="recipes-insight-example"></a>Recept insight-exempel

Nedan visas hur Bing kan visas med recept på den mat som visas i bild. I exempel låter användaren veta recept finns tillgängliga.

![Recept och sidor, inklusive](./media/recipes-pages-including.PNG)

 Och innehåller en länk till recept när du utökar listan.

![Utökade recept sidor, inklusive](./media/expanded-recipes-pages-including.PNG)


## <a name="relatedsearches-insight-example"></a>RelatedSearches insight-exempel

Nedan visas hur Bing kan visa relaterade sökningar av bilder som har skapats av andra. Om användaren klickar på bilden, tas användaren till Bing.com/images sökresultatsidan relaterade frågan.

![Relaterade sökningar för bilder](./media/bordered-related-searches.PNG)


## <a name="entity-insight-example"></a>Entiteten insight-exempel

Nedan visas hur Bing kan visa information om entiteten (person, plats eller sak) visas i bild. Om användaren klickar på länken entiteten tas användaren till Bing.com sökresultatsidan för entiteten.

![Entiteten visas i bild](./media/entity.PNG)


## <a name="displaying-other-insights-that-the-user-might-explore"></a>Visa andra insikter som du kan utforska

Nedan visas hur Bing kan visa annan information om den avbildning som du kan utforska.

![Utforska andra insikter om avbildningen](./media/apple-pie-more-tags.PNG)


## <a name="bounding-boxes-and-hot-spots"></a>Omgivande rutorna och aktiva punkter

Icke-standard-taggar är bland annat avgränsningsruta som identifierar intresseområde i avbildningen som taggen gäller för. Om rutan inte identifierar hela bilden, använder du rutan för att skapa en aktiv punkt på bilden. Användaren kan klicka på den aktiva punkten för att få information som rör innehållet finns under hotspot (eller rektangel). Till exempel om avbildningen är en avbildning av en hög sätt, kan resultaten innehålla taggar (och omgivande rutorna) för tillbehör som visas i bild, till exempel en vad-eller, smycken, scarfs osv. I följande exempel visas en hotspot rektangel för solglasögonföretag som visas i bild.

![Omgivande ruta och hotspot](./media/click-to-search.PNG)



## <a name="next-steps"></a>Nästa steg

Du kan kontrollera ut JSON bakom de här exemplen [standard insights](default-insights-tag.md) och [JSON-svar](overview.md#the-response).

Om du vill komma igång snabbt med din första begäran kan se Snabbstart: [C#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [node.js](quickstarts/nodejs.md)  |  [Python](quickstarts/python.md)





