---
title: Azure Active Directory v2.0 omfattningar, behörigheter och godkännande | Microsoft Docs
description: En beskrivning av auktorisering i Azure AD v2.0-slutpunkten, inklusive omfattningar, behörigheter och godkännande.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 8f98cbf0-a71d-4e34-babf-e644ad9ff423
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: hirsin, jesakowi, justhu
ms.custom: aaddev
ms.openlocfilehash: da8eebb2fc6b87b8916e944495679b45aa34dbf2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46960336"
---
# <a name="permissions-and-consent-in-the-azure-active-directory-v20-endpoint"></a>Behörigheter och godkännande i Azure Active Directory v2.0-slutpunkten

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Program som integreras med Microsoft identity-plattformen att följa en modell för auktorisering som ger användare och administratörer kontroll över hur data kan nås. Implementeringen av auktoriseringsmodellen som har uppdaterats på v2.0-slutpunkten och den ändras hur en app måste interagera med Microsoft identity-plattformen. Den här artikeln beskriver de grundläggande principerna för den här auktoriseringsmodellen, inklusive omfattningar, behörigheter och godkännande.

> [!NOTE]
> V2.0-slutpunkten har inte stöd för alla scenarier och funktioner. Läs mer om för att avgöra om du ska använda v2.0-slutpunkten, [v2.0 begränsningar](active-directory-v2-limitations.md).

## <a name="scopes-and-permissions"></a>Omfång och behörigheter

Microsoft identity-plattformen implementerar den [OAuth 2.0](active-directory-v2-protocols.md) auktoriseringsprotokoll. OAuth 2.0 är en metod som en app från tredje part kan komma åt webb-värdbaserade resurser för en användares räkning. Någon webbaserat resurs som kan integreras med Microsoft identity-plattformen har en resurs-ID eller *program-ID-URI*. Några av Microsofts webbaserat resurser är till exempel:

* Microsoft Graph: `https://graph.microsoft.com`
* API för Office 365-e-post: `https://outlook.office.com`
* Azure AD Graph: `https://graph.windows.net`

> [!NOTE]
> Vi rekommenderar starkt att du använder Microsoft Graph i stället för Azure AD Graph, API: et för Office 365-e-post osv.

Detsamma gäller för resurser från tredje part som har integrerat med Microsoft identity-plattformen. Någon av följande resurser kan också definiera en uppsättning behörigheter som kan användas för att dela upp funktionerna i den här resursen i mindre segment. Till exempel [Microsoft Graph](https://graph.microsoft.com) har definierat behörighet att utföra följande uppgifter, bland annat:

* Läsa en användares kalender
* Skriva till en användares kalender
* Skicka e-post som en användare

Resursen har detaljerad kontroll över sina data och hur API-funktioner exponeras genom att definiera dessa typer av behörigheter. En app från tredje part kan begära dessa behörigheter från användare och administratörer, som måste godkänna begäran innan appen kan komma åt data eller utföra åtgärder för en användares räkning. Genom att dela upp resursens funktionen till mindre behörighetsuppsättningar, kan appar från tredje part byggas om du vill begära endast specifika behörigheter som de behöver för att fungera. Användare och administratörer kan veta exakt vilka data som appen har åtkomst till och de kan vara mer säker på att det inte fungerar med skadliga avsikter. Utvecklare bör alltid följa konceptet med minsta behörighet, ber om de behörigheter som de behöver för sina program ska fungera.

I OAuth, kallas dessa typer av behörigheter *scope*. De även ofta bara kallas *behörigheter*. En behörighet visas i Microsoft identity-plattformen som ett strängvärde. Fortsättning på Microsoft Graph-exemplet, är strängvärdet för varje behörighet:

* Läsa en användares kalender med hjälp av `Calendars.Read`
* Skriva till en användares kalender genom att använda `Calendars.ReadWrite`
* Skicka e-post som en användare med hjälp av `Mail.Send`

En app begär oftast behörigheterna genom att ange omfång i begäranden till v2.0 tillåta slutpunkt. Vissa behörigheter med hög behörighet kan dock bara beviljas genom administratörens godkännande och allmänt begärt/beviljas med hjälp av den [administratör medgivande endpoint](v2-permissions-and-consent.md#admin-restricted-scopes). Fortsätt att läsa om du vill veta mer.

## <a name="permission-types"></a>Behörighetstyper

Microsoft identity-plattformen stöder två typer av behörigheter: **delegerade behörigheter** och **programbehörigheter**.

- **Delegerade behörigheter** som används av appar som har en inloggad användare finns. För dessa appar godkänner användaren eller administratören behörigheter att app-begäranden och appen är delegerad behörighet att agera som den inloggade användaren att göra anrop till målresursen. Vissa delegerade behörigheter kan vara godkänts av icke-administratörer, men vissa högre privilegier behörigheter kräver [administratörens godkännande](v2-permissions-and-consent.md#admin-restricted-scopes).  

- **Programbehörigheter** som används av appar som körs utan en inloggad användare finns, till exempel appar som körs som Bakgrundstjänster eller daemon.  Behörigheter för programmet kan bara vara [samtyckt av en administratör](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant). 

_Gällande behörigheter_ är de behörigheter som din app har vid begäranden till målresursen. Det är viktigt att förstå skillnaden mellan den delegerade och behörigheter för program som din app har beviljats och dess gällande behörigheter vid anrop till målresursen.

- För delegerade behörigheter i _gällande behörigheter_ för din app är minst Privilegierade skärningspunkten för de delegerade behörigheter som appen har beviljats (via medgivande) och privilegier för den inloggade användaren. Din app kan aldrig ha fler behörigheter än den inloggade användaren. Inom organisationer kan behörigheter för den inloggade användaren fastställas med en princip eller av medlemskap i en eller flera administratörsroller. Läs mer om administratörsroller [Tilldela administratörsroller i Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md).
  Anta exempelvis att din app har beviljats den _User.ReadWrite.All_ delegerad behörighet. Den här behörigheten ger i princip din app behörighet att läsa och uppdatera profilen för alla användare i en organisation. Om den inloggade användaren är en global administratör, kommer din app att kunna uppdatera profilen för alla användare i organisationen. Men om den inloggade användaren inte har någon administratörsroll, kommer appen endast kunna uppdatera profilen för den inloggade användaren. Den kommer inte att kunna uppdatera profilerna för andra användare i organisationen, eftersom den användare som den har behörighet att agera på uppdrag åt inte har den behörigheten.
  
- För behörigheter för programmet, den _gällande behörigheter_ för din app kommer att nivån fullständig behörigheter underförstådd av behörigheten. Exempelvis kan en app som har den _User.ReadWrite.All_ programbehörigheten kan uppdatera profilen för alla användare i organisationen. 

## <a name="openid-connect-scopes"></a>OpenID Connect-scope

V2.0-implementeringen av OpenID Connect har några väldefinierade scope som inte gäller för en specifik resurs: `openid`, `email`, `profile`, och `offline_access`.

### <a name="openid"></a>openid

Om en app utför logga in med hjälp av [OpenID Connect](active-directory-v2-protocols.md), den måste begära den `openid` omfång. Den `openid` omfång visas på sidan work medgivande som behörigheten ”logga du in” och på samtyckessida för personliga Microsoft-konto som ”visa din profil och ansluta till appar och tjänster med ditt Microsoft-konto”-behörighet. Med den här behörigheten kan en app kan ta emot en unik identifierare för användaren i form av den `sub` anspråk. Det ger också åtkomst till appen till slutpunkten användarinformationen. Den `openid` omfång som kan användas i token v2.0-slutpunkten för att hämta ID-token som kan användas för att säkra HTTP-anrop mellan olika komponenter i en app.

### <a name="email"></a>e-post

Den `email` omfång kan användas med den `openid` omfång och alla andra. Den ger appen åtkomst till användarens primära e-postadress i form av den `email` anspråk. Den `email` anspråk som ingår i en token endast om en e-postadress är associerad med det användarkonto som inte är alltid fallet. Om den använder den `email` omfattning, din app ska vara beredd att hantera ett fall där den `email` anspråk finns inte i token.

### <a name="profile"></a>profil

Den `profile` omfång kan användas med den `openid` omfång och alla andra. Den ger appen åtkomst till en stor mängd information om användaren. Informationen om den kan komma åt omfattar, men är inte begränsad till användarens förnamn, efternamn, primära användarnamn och objekt-ID. En fullständig lista över anspråk för profilen som är tillgängliga i parametern id_tokens för en viss användare ser den [ `id_tokens` referens](id-tokens.md).

### <a name="offlineaccess"></a>offline_access

Den [ `offline_access` omfång](http://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) ger din appåtkomst till resurser å användarens vägnar en längre tid. På sidan work medgivande visas det här omfånget som ”åt dina data när som helst”-behörighet. På personliga Microsoft-konto medgivande sidan visas den som ”åt din information när som helst”-behörighet. När en användare godkänner den `offline_access` omfattning, din app kan ta emot uppdaterings-tokens från token v2.0-slutpunkten. Uppdateringstoken är långlivade. Din app kan hämta nya åtkomsttoken som äldsta förfaller.

Om din app inte begär det `offline_access` omfattning, det inte tar emot uppdateringstoken. Detta innebär att när du har löst in en auktoriseringskod i den [OAuth 2.0-auktoriseringskodflödet](active-directory-v2-protocols.md), får du endast en åtkomsttoken från den `/token` slutpunkt. Åtkomsttoken är giltig för en kort tid. Åtkomsttoken upphör vanligtvis i en timme. AT att punkt, din app måste därefter skickas användarna tillbaka till den `/authorize` slutpunkten för att få en ny auktoriseringskod. Under den här omdirigering, beroende på typen av app måste behöva användaren ange sina autentiseringsuppgifter igen eller godkänna igen behörigheter.

Mer information om hur du hämtar och använder uppdateringstoken finns i den [protokollreferens för v2.0](active-directory-v2-protocols.md).

## <a name="requesting-individual-user-consent"></a>Begär användargodkännande för enskilda

I en [OpenID Connect eller OAuth 2.0](active-directory-v2-protocols.md) auktorisering begär att en app kan begära de behörigheter som krävs med hjälp av den `scope` frågeparameter. Till exempel när en användare loggar in på en app, appen skickar en begäran som i följande exempel (med radbrytningar har lagts till för läsbarhet):

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=
https%3A%2F%2Fgraph.microsoft.com%2Fcalendars.read%20
https%3A%2F%2Fgraph.microsoft.com%2Fmail.send
&state=12345
```

Den `scope` parameter är en blankstegsavgränsad lista över delegerade behörigheter som appen begär. Varje behörighet anges genom att lägga till Behörighetsvärdet för till resursidentifierare (program-ID: T URI). I exemplet begäran måste appen behörighet att läsa användarens kalendern och skicka e-post som användaren.

När användaren anger sina autentiseringsuppgifter, v2.0-slutpunkten söker efter matchande koll på *användarmedgivande*. Om användaren inte har godkänt att någon av behörigheterna som krävs i förflutna eller har en administratör samtyckt till behörigheterna för hela organisationen, v2.0-slutpunkten uppmanar användaren att bevilja behörigheterna som krävs.

![Arbeta konto medgivande](./media/v2-permissions-and-consent/work_account_consent.png)

När användaren godkänner begäran behörighet, medgivande registreras och användaren behöver inte godkänna igen på efterföljande inloggningar till programmet.

## <a name="requesting-consent-for-an-entire-tenant"></a>Begär godkännande för en hel-klient

Ofta, när en organisation köper en licens eller prenumeration för ett program kan vill organisationen proaktivt konfigurera program för användning av alla medlemmar i organisationen. Som en del av den här processen kan kan en administratör ge samtycke för programmet att fungera som ombud för alla användare i klienten. Om administratören ger ditt medgivande för hela klientorganisationen, se organisationens användare inte en samtyckessida för programmet.

Om du vill begära godkännande för delegerade behörigheter för alla användare i en klient, kan din app använder medgivande admin-slutpunkten.

Dessutom måste programmen använda adminslutpunkten för godkännande för att begära behörigheter för programmet.

## <a name="admin-restricted-permissions"></a>Begränsat behörigheter

Vissa höga behörigheter i Microsofts ekosystem kan anges till *begränsat*. Följande är exempel på dessa typer av behörigheter:

* Läsa fullständiga profiler för alla användare med hjälp av `User.Read.All`
* Skriva data till en organisations katalog med hjälp av `Directory.ReadWrite.All`
* Läsa alla grupper i en organisations katalog med hjälp av `Groups.Read.All`

Även om en konsument-användare kan ge ett programåtkomst till den här typen av data, är organisationsanvändare begränsade från att bevilja åtkomst till samma uppsättning av känsliga företagsdata. Om ditt program begär åtkomst till någon av dessa behörigheter från en organisationsanvändare, får användaren ett felmeddelande som säger att de inte har behörighet att ge samtycke för din app-behörigheter.

Om din app kräver åtkomst till begränsat scope för organisationer, bör du begära dem direkt från en företagsadministratör också med hjälp av admin medgivande-slutpunkten, som beskrivs nedan.

Om programmet begär Privilegierade delegerade behörigheter och en administratör ger dessa behörigheter via medgivande adminslutpunkten, godkänns medgivande för alla användare i klienten.

Om programmet begär behörigheter för programmet och en administratör beviljar följande behörigheter via administratören godkänna endpoint, görs inte det här beviljandet för en viss användares räkning. I stället klientprogrammet beviljas behörigheter *direkt*. Dessa typer av behörigheter som vanligtvis endast används av daemon servies och andra icke-interaktiva program som körs i bakgrunden.

## <a name="using-the-admin-consent-endpoint"></a>Med hjälp av administratören medgivande slutpunkt

När en företagsadministratör använder ditt program och dirigeras till slutpunkten för auktorisering, Microsoft identity-plattformen identifierar användarens roll och be dem om de vill ge samtycke åt hela klientorganisationen för de behörigheter som du har begärt. Men finns det också en dedicerad administratörsanslutning medgivande slutpunkt som du kan använda om du vill proaktivt begär att en administratör ger behörighet åt hela klientorganisationen. Med den här slutpunkten behövs också för att begära programbehörigheter (som inte kan begäras med slutpunkten för auktorisering).

Om du följer dessa steg kan appen begära behörighet för alla användare i en klient, inklusive begränsat scope. Detta är en åtgärd med hög behörighet och bör endast göras om det behövs för ditt scenario.

Ett kodexempel som implementerar stegen finns i den [begränsat scope exempel](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2).

### <a name="request-the-permissions-in-the-app-registration-portal"></a>Begär behörighet i portalen för registrering av app

Administratörens godkännande accepterar inte en omfattningsparameter, så att alla behörigheter som begärts måste definieras statiskt i programmets registrering. I allmänhet är det bäst att kontrollera att de behörigheter som statiskt har definierats för ett visst program är en supermängd de behörigheter som det begär dynamiskt/inkrementellt.

Konfigurera listan över statiskt begärda behörigheter för ett program: 
1. Gå till ditt program i den [Programregistreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller [skapa en app](quickstart-v2-register-an-app.md) om du inte redan har gjort.
2. Leta upp den **Microsoft Graph-behörigheter** , och lägger sedan till de behörigheter som din app kräver.
3. **Spara** appregistreringen.

### <a name="recommended-sign-the-user-in-to-your-app"></a>Rekommenderat: Logga in användaren till din app

Vanligtvis när du skapar ett program som använder medgivande adminslutpunkten måste appen du en sida eller vy där administratören kan godkänna dess behörigheter. Den här sidan kan vara en del av appens registrering flöde, en del av appens inställningar, eller så kan vara ett dedikerat ”ansluta”-flöde. I många fall kan det vara bra för appen ska visa detta ”ansluta” Visa endast när en användare har loggat in med ett arbets- eller din skola Microsoft-konto.

När du loggar du in till din app kan identifiera du den organisation som administratören tillhör innan ber dem att godkänna behörigheterna som krävs. Även om de inte är absolut nödvändigt, det kan hjälpa dig att skapa en mer intuitiv upplevelse för din organisations användare. Om du vill registrera användare i följa våra [v2.0-protokollet självstudier](active-directory-v2-protocols.md).

### <a name="request-the-permissions-from-a-directory-admin"></a>Begär behörigheter från en directory-administratör

När du är redo att begära behörighet från administratören för din organisation kan du omdirigera användaren till v2.0 *medgivande adminslutpunkten*.

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro tip: Try pasting the below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parameter | Tillstånd | Beskrivning |
| --- | --- | --- |
| `tenant` | Krävs | Directory-klient som du vill begära behörighet från. Kan anges i GUID eller eget namnformat eller med det allmänna skyddet som hänvisas med ”vanliga” som visas i exemplet. |
| `client_id` | Krävs | Programmet med ID som den [Programregistreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) tilldelats din app. |
| `redirect_uri` | Krävs |Omdirigerings-URI där du vill att svaret skickas för din app för att hantera. Det måste exakt matcha en av omdirigerings-URI: er som du registrerade i portalen för registrering av appen. |
| `state` | Rekommenderas | Ett värde som ingår i den begäran som också kommer att returneras i token-svaret. Det kan vara en sträng med innehåll. Använda tillståndet för att koda information om användarens tillstånd i appen innan autentiseringsbegäran inträffat, till exempel sidan eller vyn som de befann sig i. |

Azure AD kräver nu en Innehavaradministratör för att logga in att slutföra begäran. Administratören uppmanas att godkänna de behörigheter som du har begärt för din app i portalen för registrering av appen.

#### <a name="successful-response"></a>Lyckat svar

Om administratören godkänner behörigheterna för din app, lyckat svar som ser ut så här:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parameter | Beskrivning |
| --- | --- | --- |
| `tenant` | Directory-klient som beviljats de behörigheter som den begärda i GUID-format för ditt program. |
| `state` | Ett värde som ingår i den begäran som också kommer att returneras i token-svaret. Det kan vara en sträng med innehåll. Tillståndet används för att koda information om användarens tillstånd i appen innan autentiseringsbegäran inträffat, till exempel sidan eller vyn som de befann sig i. |
| `admin_consent` | Anges till **SANT**. |

#### <a name="error-response"></a>Felsvar

Om administratören inte godkänner behörigheterna för din app, misslyckat svar som ser ut så här:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parameter | Beskrivning |
| --- | --- | --- |
| `error` |En felkodsträngen som kan användas för att klassificera typer av fel som inträffar och kan användas för att ta hänsyn till fel. |
| `error_description` |Ett felmeddelande som kan hjälpa utvecklare identifiera grundorsaken till ett fel. |

När du har fått ett lyckat svar från admin-slutpunkten för medgivande, kommer din app har fått de behörigheter som begärde. Därefter kan du begära en token för den resurs som du vill.

## <a name="using-permissions"></a>Med behörigheter

När användaren godkänner behörighet för din app, kan appen hämta åtkomsttoken som representerar appens behörighet att komma åt en resurs i vissa kapacitet. En åtkomsttoken kan bara användas för en enskild resurs, men kodad i åtkomsttoken är varje behörighet som din app har beviljats för den resursen. Om du vill hämta en åtkomsttoken kan appen göra en begäran v2.0 tokenslutpunkten, så här:

```
POST common/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "authorization_code",
    "client_id": "6731de76-14a6-49ae-97bc-6eba6914391e",
    "scope": "https://outlook.office.com/mail.read https://outlook.office.com/mail.send",
    "code": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq..."
    "redirect_uri": "https://localhost/myapp",
    "client_secret": "zc53fwe80980293klaj9823"  // NOTE: Only required for web apps
}
```

Du kan använda den resulterande åtkomsttoken i HTTP-förfrågningar till resursen. På ett tillförlitligt sätt betyder det att resursen att din app har rätt behörighet för att utföra en viss uppgift. 

Läs mer om OAuth 2.0-protokollet och hur du hämtar åtkomsttoken, den [protokollreferens för v2.0-slutpunkten](active-directory-v2-protocols.md).

## <a name="troubleshooting"></a>Felsökning

Om du eller ditt programs användare ser ett oväntat fel under medgivande, hittar du i den här artikeln felsökningsstegen: [ett oväntat fel inträffade när du utför medgivande till ett program](../manage-apps/application-sign-in-unexpected-user-consent-error.md).
