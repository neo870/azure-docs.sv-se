---
title: Azure Service Fabric CLI - sfctl är | Microsoft Docs
description: Beskriver Service Fabric CLI sfctl är kommandon.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 07/31/2018
ms.author: bikang
ms.openlocfilehash: 53e49099fd3486d51f021528c9354cf32f4952d2
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2018
ms.locfileid: "39492797"
---
# <a name="sfctl-is"></a>sfctl is
Fråga efter och skicka kommandon till tjänsten infrastruktur.

## <a name="commands"></a>Kommandon

|Kommando|Beskrivning|
| --- | --- |
| command | Initierar en administrativ kommando på den angivna Infrastructure Service-instansen. |
| DocumentDB | Anropar en skrivskyddad fråga på den angivna infrastruktur tjänstinstansen. |

## <a name="sfctl-is-command"></a>sfctl är kommando
Initierar en administrativ kommando på den angivna Infrastructure Service-instansen.

För kluster som har en eller flera instanser av tjänsten-infrastruktur konfigurerad, kan detta API skicka infrastruktur-specifika kommandon till en viss instans av tjänsten infrastruktur. Tillgängliga kommandon och deras motsvarande svar format varierar beroende på den infrastruktur som klustret körs. Detta API stöder Service Fabric-plattform. Det är inte avsedd att användas direkt från din kod.

### <a name="arguments"></a>Argument

|Argumentet|Beskrivning|
| --- | --- |
| --kommandot [krävs] | Texten för kommandot anropas. Innehållet i kommandot är infrastruktur-specifika. |
| --service-id | Identiteten för infrastruktur-tjänst. <br><br> Det här är det fullständiga namnet på tjänsten infrastruktur utan att URI-schemat 'fabric ”. Den här parametern krävs endast för det kluster som har mer än en instans av infrastruktur-tjänsten körs. |
| --timeout -t | Tidsgräns för Server på några sekunder.  Standard\: 60. |

### <a name="global-arguments"></a>Global argument

|Argumentet|Beskrivning|
| --- | --- |
| --Felsöka | Öka detaljnivå loggning för att visa alla felsöka loggar. |
| --hjälpa -h | Visa den här hjälpmeddelande och avsluta. |
| --utdata -o | Utdataformat.  Tillåtna värden\: json, jsonc, tabell, TVs.  Standard\: json. |
| – fråga | JMESPath-frågesträng. Se http\://jmespath.org/ för mer information och exempel. |
| --utförlig | Öka detaljnivå för loggning. Använd--felsökning för fullständig felsökningsloggar. |

## <a name="sfctl-is-query"></a>sfctl är fråga
Anropar en skrivskyddad fråga på den angivna infrastruktur tjänstinstansen.

För kluster som har en eller flera instanser av tjänsten-infrastruktur konfigurerad, är detta API ett sätt att skicka infrastruktur-specifika frågor till en viss instans av tjänsten infrastruktur. Tillgängliga kommandon och deras motsvarande svar format varierar beroende på den infrastruktur som klustret körs. Detta API stöder Service Fabric-plattform. Det är inte avsedd att användas direkt från din kod.

### <a name="arguments"></a>Argument

|Argumentet|Beskrivning|
| --- | --- |
| --kommandot [krävs] | Texten för kommandot anropas. Innehållet i kommandot är infrastruktur-specifika. |
| --service-id | Identiteten för infrastruktur-tjänst. <br><br> Det här är det fullständiga namnet på tjänsten infrastruktur utan att den ”fabric\:” URI-schema. Den här parametern krävs endast för det kluster som har mer än en instans av infrastruktur-tjänsten körs. |
| --timeout -t | Tidsgräns för Server på några sekunder.  Standard\: 60. |

### <a name="global-arguments"></a>Global argument

|Argumentet|Beskrivning|
| --- | --- |
| --Felsöka | Öka detaljnivå loggning för att visa alla felsöka loggar. |
| --hjälpa -h | Visa den här hjälpmeddelande och avsluta. |
| --utdata -o | Utdataformat.  Tillåtna värden\: json, jsonc, tabell, TVs.  Standard\: json. |
| – fråga | JMESPath-frågesträng. Se http\://jmespath.org/ för mer information och exempel. |
| --utförlig | Öka detaljnivå för loggning. Använd--felsökning för fullständig felsökningsloggar. |


## <a name="next-steps"></a>Nästa steg
- [Konfigurera](service-fabric-cli.md) Service Fabric CLI.
- Lär dig hur du använder Service Fabric CLI med hjälp av den [exempel på skript](/azure/service-fabric/scripts/sfctl-upgrade-application).