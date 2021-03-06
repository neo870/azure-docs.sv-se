---
title: Versionshistorik för Azure AD lösenord protection agent på plats
description: Dokument-versionen och beteende ändringshistorik
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 11/01/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: 89d64a28d2fe43464995e434c9f3807047b29492
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50913644"
---
# <a name="preview--azure-ad-password-protection-agent-version-history"></a>Förhandsversion: Azure AD lösenord protection agent-versionshistorik

|     |
| --- |
| Azure AD-lösenordsskydd är en funktion i offentliga förhandsversionen av Azure Active Directory. Mer information om förhandsversioner finns [kompletterande användningsvillkor för förhandsversioner av Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="12250"></a>1.2.25.0

Utgivningsdatum: 11/01/2018

Korrigeringar:

* DC-agenten och proxy-tjänsten ska inte längre misslyckas på grund av certifikat förtroende fel.
* DC-agenten och proxy-tjänsten har ytterligare korrigeringar för FIPS-kompatibla datorer.
* Proxy-tjänsten fungerar nu korrekt TLS 1.2 endast nätverksmiljön.
* Lägre prestanda och stabilitet korrigeringar
* Förbättrad loggning

Ändringar:

* Den lägsta operativsystemnivå för en Proxy-tjänsten är nu Windows Server 2012 R2. Den minsta nödvändiga operativsystemnivå för en DC-agenttjänsten ligger kvar på Windows Server 2012.
* Lösenord verifieringsalgoritm använder en tabell för normalisering av utökade tecken. Detta kan resultera i lösenord avvisas som godkändes i tidigare versioner.

## <a name="12100"></a>1.2.10.0

Utgivningsdatum: 8/17/2018

Korrigeringar:

* Registrera AzureADPasswordProtectionProxy och registrera AzureADPasswordProtectionForest stöder nu multifaktorautentisering
* Registrera AzureADPasswordProtectionProxy kräver en WS2012 eller senare domänkontrollanten i domänen för att undvika krypteringsfel.
* DC-agenttjänsten är mer tillförlitligt om hur du begär en ny lösenordsprincip för från Azure vid start.
* DC-agenttjänsten begär en ny lösenordsprincip för från Azure varje timme om det behövs, men kommer nu att göra det på ett slumpmässigt valda starttiden.
* DC-agenttjänsten kommer inte längre en obestämd fördröjning i ny DC-annons när installeras på en server innan dess befordran som en replik.
* DC-agenttjänsten följer nu Konfigurationsinställningen ”aktivera lösenordsskydd på Windows Server Active Directory”
* Installationsprogram för både DC-agenten och proxy har nu stöd för uppgradering på plats när du uppgraderar till framtida versioner.

> [!WARNING]
> Uppgradering från version 1.1.10.3 stöds inte och leder till installationsfel. Till uppgradera till version 1.2.10 eller senare, du måste först helt avinstallera programvaran för DC-agenten och proxy-tjänsten och sedan installera den nya versionen från grunden. Omregistrering av Azure AD-lösenordsskydd proxytjänsten krävs.  Det krävs inte registrera skogen.

> [!NOTE]
> Uppgraderingar på plats av DC-agentprogramvaran kräver en omstart.

* DC-agenten och proxy-tjänsten nu kan du köra på en server som konfigurerats att bara använda FIPS-kompatibla algoritmer.
* Lägre prestanda och stabilitet korrigeringar
* Förbättrad loggning

## <a name="11103"></a>1.1.10.3

Utgivningsdatum: 6/15/2018

Första offentliga förhandsversionen

## <a name="next-steps"></a>Nästa steg

[Distribuera Azure AD-lösenordsskydd](howto-password-ban-bad-on-premises-deploy.md)
