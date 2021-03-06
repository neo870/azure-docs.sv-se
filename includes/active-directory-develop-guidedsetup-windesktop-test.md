---
title: ta med fil
description: ta med fil
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: ac4f826ed1d27ee39d8e35605a3baa7f94b33e64
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/24/2018
ms.locfileid: "50035247"
---
## <a name="test-your-code"></a>Testa din kod

Om du vill köra ditt projekt i Visual Studio, Välj **F5**. Programmets **MainWindow** visas som det visas här:

![Testa ditt program](./media/active-directory-develop-guidedsetup-windesktop-test/samplescreenshot.png)

Första gången du kör programmet och välj den **anropa Microsoft Graph API** knappen, uppmanas du för att logga in. Använda en Azure Active Directory-konto (arbets- eller skolkonto konto) eller ett Microsoft-konto (live.com, outlook.com) för att testa den.

![Logga in på programmet](./media/active-directory-develop-guidedsetup-windesktop-test/signinscreenshot.png)

### <a name="provide-consent-for-application-access"></a>Ge medgivande för programåtkomst

Första gången du loggar in på ditt program också uppmanas du att ange samtycka till att program kan komma åt din profil och logga in dig i som visas här:

![Ge ditt medgivande för programåtkomst](./media/active-directory-develop-guidedsetup-windesktop-test/consentscreen.png)

### <a name="view-application-results"></a>Visa program resultat

När du har loggat in visas den information om användarprofiler som returneras av anropet till Microsoft Graph API. Resultaten visas i den **API samtalsresultat** box. Grundläggande information om den token som köptes via anropet till `AcquireTokenAsync` eller `AcquireTokenSilentAsync` ska synas i den **tokeninformation** box. Resultaten innehåller följande egenskaper:

|Egenskap   |Format  |Beskrivning |
|---------|---------|---------|
|**Namn** |Användarens fullständiga namn |Användaren förnamn och efternamn.|
|**Användarnamn** |<span>user@domain.com</span> |Det användarnamn som används för att identifiera användaren.|
|**Token upphör att gälla** |DateTime |Den tid då token upphör att gälla. MSAL förlänger utgångsdatumet genom att förnya token efter behov.|
|**Åtkomsttoken** |Sträng |Token strängen som skickas till HTTP-begäranden som kräver en *auktoriseringsrubrik*.|

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Mer information om scope och delegerade behörigheter

Microsoft Graph API kräver den *user.read* omfattning att läsa en användares profil. Det här omfånget läggs automatiskt som standard i alla program som har registrerats i portalen för registrering av programmet. Andra API: er för Microsoft Graph, samt anpassade API: er för backend-servern, kan kräva ytterligare scope. Microsoft Graph API kräver den *Calendars.Read* omfattning att lista användarens kalendrar.

Om du vill få åtkomst till användarkalendrar i kontexten för ett program måste du lägga till den *Calendars.Read* delegerad behörighet att registreringsinformation för programmet. Lägg sedan till den *Calendars.Read* begränsa omfånget till den `acquireTokenSilent` anropa.

>[!NOTE]
>Användaren uppmanas för ytterligare medgivanden när du ökar antalet omfång.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
