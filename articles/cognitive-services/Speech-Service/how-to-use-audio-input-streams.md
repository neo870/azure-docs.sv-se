---
title: Tal SDK ljud Indataströmmen begrepp
titleSuffix: Azure Cognitive Services
description: En översikt över funktionerna i SDK tal ljud Indataströmmen API.
services: cognitive-services
author: fmegen
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: fmegen
ms.openlocfilehash: b7779131a0fd30d263fa87f2e55c9642984d054b
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49467981"
---
# <a name="about-the-speech-sdk-audio-input-stream-api"></a>Om tal SDK ljudindata strömma API

Tal-SDK **ljud indata Stream** API gör det möjligt att strömma ljudströmmar till identifierarna istället för att använda mikrofonen eller indatafilen API: er.

Följande steg krävs när med ljud indataström:

- Identifiera formatet för ljudströmmen. Formatet måste stödjas av tal SDK och Speech-tjänsten. För närvarande stöds endast följande konfiguration:

  Inläsningar i PCM-format, en kanal, 16000 exempel per sekund, 32000 byte per sekund, justera två block (16-bitars inklusive utfyllnad för ett exempel), 16 bitar per sampling.

  Motsvarande kod i SDK för att skapa ljudformatet ser ut så här:

  ```
  byte channels = 1;
  byte bitsPerSample = 16;
  int samplesPerSecond = 16000;
  var audioFormat = AudioStreamFormat.GetWaveFormatPCM(samplesPerSecond, bitsPerSample, channels);
  ```

- Kontrollera att din kod kan ge ljud rådata enligt dessa specifikationer. Om dina ljud källdata inte matchar format som stöds, måste ljudet ska kodas i formatet som krävs.

- Skapa dina egna ljud Indataströmmen klass som härleds från `PullAudioInputStreamCallback`. Implementera de `Read()` och `Close()` medlemmar. Den exakta funktionssignaturen är språkberoende, men koden ser ut ungefär som detta kodexempel:

  ```
   public class ContosoAudioStream : PullAudioInputStreamCallback {
      ContosoConfig config;

      public ContosoAudioStream(const ContosoConfig& config) {
          this.config = config;
      }

      public size_t Read(byte *buffer, size_t size) {
          // returns audio data to the caller.
          // e.g. return read(config.YYY, buffer, size);
      }

      public void Close() {
          // close and cleanup resources.
      }
   };
  ```

- Skapa en ljud-konfiguration baserat på dina ljud-format och indata stream. Skicka i både din vanliga tal-konfiguration och ljud inkommande konfiguration när du skapar din Igenkännande. Exempel:

  ```
  var audioConfig = AudioConfig.FromStreamInput(new ContosoAudioStream(config), audioFormat);

  var speechConfig = SpeechConfig.FromSubscription(...);
  var recognizer = new SpeechRecognizer(speechConfig, audioConfig);

  // run stream through recognizer
  var result = await recognizer.RecognizeOnceAsync();

  var text = result.GetText();
  ```

## <a name="next-steps"></a>Nästa steg

* [Hämta en kostnadsfri utvärderingsprenumeration på Speech](https://azure.microsoft.com/try/cognitive-services/)
* [Se hur du kan känna igen tal i C#](quickstart-csharp-dotnet-windows.md)
