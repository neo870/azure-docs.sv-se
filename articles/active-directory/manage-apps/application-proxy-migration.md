---
title: Uppgradera till Azure AD Application Proxy | Microsoft Docs
description: Välj vilken proxy-lösning som är bäst om du uppgraderar från Microsoft Forefront eller enhetlig åtkomst Gateway.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/27/2017
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: c4ecb812156eae7402065cff4dc4bae3aef1554b
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365183"
---
# <a name="compare-remote-access-solutions"></a>Jämför fjärråtkomstlösningar

Azure Active Directory Application Proxy är en av två fjärråtkomstlösningarna som Microsoft erbjuder. Den andra är Web Application Proxy, lokal version. Dessa två lösningar ersätta tidigare produkter som erbjuds av Microsoft: Microsoft Forefront Threat Management Gateway (TMG) och enhetlig åtkomst Gateway (UAG). Använd den här artikeln för att förstå hur dessa fyra lösningar jämfört med varandra. Använd den här artikeln för att planera din migrering till en av programproxyn för de som fortfarande använder de inaktuella TMG eller UAG lösningarna. 


## <a name="feature-comparison"></a>Jämför funktioner

Använd den här tabellen för att förstå hur Threat Management Gateway (TMG), en enhetlig åtkomst Gateway (UAG), Web Application Proxy (WAP) och Azure AD Application Proxy (AP) jämfört med varandra.

| Funktion | TMG | UAG | WAP | AP |
| ------- | --- | --- | --- | --- |
| Certifikatautentisering | Ja | Ja | - | - |
| Selektivt publicera webbläsarbaserade appar | Ja | Ja | Ja | Ja |
| Förautentisering och enkel inloggning | Ja | Ja | Ja | Ja | 
| Layer 2/3 brandvägg | Ja | Ja | - | - |
| Vidarebefordra proxyfunktioner | Ja | - | - | - |
| VPN-funktioner | Ja | Ja | - | - |
| Omfattande protokollstöd | - | Ja | Ja, om du kör över HTTP | Ja, om du kör via HTTP eller via Remote Desktop Gateway |
| Fungerar som AD FS-proxyservern | - | Ja | Ja | - |
| En portal för programåtkomst | - | Ja | - | Ja |
| Svaret brödtext länköversättning | Ja | Ja | - | Ja | 
| Autentisering med rubriker | - | Ja | - | Ja, med PingAccess | 
| Säkerhet för molnskala | - | - | - | Ja | 
| Villkorlig åtkomst | - | Ja | - | Ja |
| Inga komponenter i demilitariserad zon (DMZ) | - | - | - | Ja |
| Inga inkommande anslutningar | - | - | - | Ja |

För de flesta fall rekommenderar vi Azure AD-program till den moderna lösningen. Du kan inte använda anpassade domäner i Azure Active Directory Web Application Proxy är bara önskade i scenarier som kräver en proxyserver för AD FS. 

Azure AD-programproxyn erbjuder unika fördelar jämfört med liknande produkter, inklusive:

- Utöka Azure AD till lokala resurser
   - Skalbar säkerhet och skydd
   - Funktioner som villkorlig åtkomst och Multi-Factor Authentication är enkelt att aktivera
- Inga Component i demilitariserad zon
- Inga inkommande anslutningar som krävs
- En åtkomstpanelen att användarna kan gå till för alla program, inklusive O365, Azure AD-integrerade SaaS-appar och din lokala webbappar. 


## <a name="next-steps"></a>Nästa steg

- [Använda Azure AD-program för att tillhandahålla säker fjärråtkomst till lokala program](application-proxy.md)
- [Övergången från Forefront TMG och UAG tillämpningsproxy](https://blogs.technet.microsoft.com/isablog/2015/06/30/modernizing-microsoft-application-access-with-web-application-proxy-and-azure-active-directory-application-proxy/).
