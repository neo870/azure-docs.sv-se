---
title: Fördefinierade entiteter för Språkförståelse (LUIS)
titleSuffix: Azure Cognitive Services
description: LUIS innehåller en uppsättning fördefinierade entiteter för att identifiera vanliga typer av information som datum, tider, nummer, mätningar och valuta. Fördefinierade entitet support varierar beroende på kultur LUIS-appen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 10/18/2018
ms.author: diberry
ms.openlocfilehash: 0fe9dbed302fd2d61305167a3bda25b1b403b761
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50139982"
---
# <a name="prebuilt-entities-to-recognize-common-data-types"></a>Färdiga entiteter för att identifiera vanliga datatyper

LUIS innehåller en uppsättning fördefinierade entiteter för att identifiera vanliga typer av information som datum, tider, nummer, mätning av faktisk användning och valuta. 

## <a name="add-a-prebuilt-entity"></a>Lägg till en fördefinierade entitet

1. Öppna din app genom att klicka på namnet på **Mina appar** , och klicka sedan på **entiteter** till vänster. 

1. På den **entiteter** klickar du på **Lägg till fördefinierade entitet**.

1. I **Lägg till fördefinierade entiteter** dialogrutan väljer du datetimeV2 fördefinierade entitet. 

    ![Fördefinierade entitet dialogrutan Lägg till](./media/luis-use-prebuilt-entity/add-prebuilt-entity-dialog.png)

1. Välj **Done** (Klar).

## <a name="publish-the-app"></a>Publicera appen

Det enklaste sättet att visa värdet för en fördefinierade entitet är att frågan från den publicerade slutpunkten. 

1. I det översta verktygsfältet väljer **publicera**. Publicera till **produktion**. 

1. När meddelandet grönt visas väljer du den **finns i listan över slutpunkter** länken för att se slutpunkterna.

1. Välj en slutpunkt. En ny webbläsarflik öppnas till denna slutpunkt. Håll webbläsarfliken öppen och Fortsätt den **Test** avsnittet.

## <a name="test"></a>Testa
När entiteten har lagts till kan behöver du inte att träna appen. 

Testa nya avsikten vid slutpunkten av lagt till ett värde för den **q** parametern. Använd följande tabell för föreslagna yttranden för **q**:

|Testa uttryck|Värdet för entitet|
|--|:--|
|Boka en flygning imorgon|2018-10-19|
|avbryta bokningen 3 mars|LUIS returnerade de senaste 3 mars tidigare (2018-03-03) och mars 3 i framtiden (2019-03-03) eftersom uttryck inte har angett ett år.|
|Schemalägga ett möte kl. 10|10:00:00|


## <a name="next-steps"></a>Nästa steg
> [!div class="nextstepaction"]
> [Fördefinierade enhetsreferens](./luis-reference-prebuilt-entities.md)
