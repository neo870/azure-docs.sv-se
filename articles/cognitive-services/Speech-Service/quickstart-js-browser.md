---
title: 'Snabbstart: Taligenkänning i JavaScript i en webbläsare med hjälp av Speech Service SDK'
titleSuffix: Azure Cognitive Services
description: Lär dig om taligenkänning i JavaScript i en webbläsare med hjälp av Speech Service SDK
services: cognitive-services
author: fmegen
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 10/12/2018
ms.author: fmegen
ms.openlocfilehash: b01746c20dbef7726f129badac045c1fb440f602
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49467471"
---
# <a name="quickstart-recognize-speech-in-javascript-in-a-browser-using-the-speech-service-sdk"></a>Snabbstart: Taligenkänning i JavaScript i en webbläsare med hjälp av Speech Service SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

I den här artikeln lär du dig hur du skapar en webbplats med hjälp av JavaScript-bindningen i Cognitive Services Speech SDK för att transkribera tal till text.
Appen baseras på Microsoft Cognitive Services Speech SDK ([ladda ned version 1.0.1](https://aka.ms/csspeech/jsbrowserpackage)).

## <a name="prerequisites"></a>Nödvändiga komponenter

* En prenumerationsnyckel för taltjänsten. Se [Prova Speech Service kostnadsfritt](get-started.md).
* En PC eller Mac, med fungerande mikrofon.
* En textredigerare.
* En aktuell version av Chrome eller Microsoft Edge.
* Alternativt en webbläsare som stöder värd för PHP-skript.

## <a name="create-a-new-website-folder"></a>Skapa en ny webbplatsmapp

Skapa en ny, tom mapp. Om du vill värdbasera exemplet på en webbserver ser du till att webbservern kan komma åt mappen.

## <a name="unpack-the-speech-sdk-for-javascript-into-that-folder"></a>Packa upp Speech SDK för JavaScript i den mappen

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Ladda ned Speech SDK som ett [.zip-paket](https://aka.ms/csspeech/jsbrowserpackage) och packa upp det i den nya mappen. Detta bör resultera i att två filer packas upp, dvs. `microsoft.cognitiveservices.speech.sdk.bundle.js` och `microsoft.cognitiveservices.speech.sdk.bundle.js.map`.
Den senare filen är valfri och används för felsökning i SDK-kod, om det behövs.

## <a name="create-an-indexhtml-page"></a>Skapa en index.html-sida

Skapa en ny fil i mappen, med namnet `index.html` och öppna filen med en textredigerare.

1. Skapa följande HTML-stomme:

  ```html
  <html>
  <head>
      <title>Microsoft Cognitive Service Speech SDK JavaScript Quickstart</title>
  </head>
  <body>
    <!-- UI code goes here -->

    <!-- SDK reference goes here -->

    <!-- Optional authorization token request goes here -->

    <!-- Sample code goes here -->
  </body>
  </html>
  ```

1. Lägg till följande användargränssnittskod i filen, under den första kommentaren:

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#uidiv)]

1. Lägga till en referens till Speech SDK

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#speechsdkref)]

1. Koppla hanterare för igenkänningsknappen, igenkänningsresultatet och prenumerationsrelaterade fält som definieras av användargränssnittskoden:

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#quickstartcode)]

## <a name="create-the-token-source-optional"></a>Skapa tokenkälla (valfritt)

Om du vill värdbasera webbsidan på en webbserver kan du ange en tokenkälla för demoappen.
På så sätt lämnar prenumerationen aldrig servern och tillåter att användare använder talfunktioner utan att själva behöva ange en auktoriseringskod.

1. Skapa en ny fil med namnet `token.php`. I det här exemplet förutsätter vi att webbservern stöder PHP-skriptspråket. Ange följande kod:

  [!code-php[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/token.php)]

1. Redigera filen `index.html` och lägg till följande kod i filen:

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#authorizationfunction)]

> [!NOTE]
> Auktoriseringstoken har en begränsad livslängd.
> Det här förenklade exemplet visar inte hur du uppdaterar auktoriseringstoken automatiskt. Som användare kan du manuellt läsa in sidan igen eller trycka på F5 för att uppdatera.

## <a name="build-and-run-the-sample-locally"></a>Skapa och köra exemplet lokalt

Starta appen genom att dubbelklicka på index.html-filen eller öppna index.html med valfri webbläsare. Ett enkelt grafiskt användargränssnitt visas där du kan ange din prenumerationsnyckel och [region](regions.md) och utlösa en igenkänning med mikrofonen.

## <a name="build-and-run-the-sample-via-a-web-server"></a>Skapa och köra exemplet via en webbserver

Du kan starta appen genom att öppna valfri webbläsare och leda den till den offentliga URL där mappen finns, ange din [region](regions.md) och utlösa en igenkänning med mikrofonen. Om det har konfigurerats hämtas en token från tokenkällan.

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Leta efter det här exemplet i mappen `quickstart/js-browser`.

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Hämta våra exempel](speech-sdk.md#get-the-samples)
