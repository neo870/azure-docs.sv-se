---
title: Azure Active Directory-kodexempel | Microsoft Docs
description: Innehåller ett index över tillgängliga Azure Active Directory (V2-slutpunkt) kodexempel ordnas efter scenario.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/26/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: ef4bffe3aca05afbdf579b34ddb85c58095b3f3b
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50138216"
---
# <a name="azure-active-directory-code-samples-v20-endpoint"></a>Kodexempel för Azure Active Directory (v2.0-slutpunkt)

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Du kan använda Microsoft identity-plattformen att:

- Lägg till autentisering och auktorisering till dina webbprogram och webb-API: er.
- Kräv en åtkomsttoken för att få åtkomst till ett skyddat webb-API.

Den här artikeln beskriver kortfattat och tillhandahåller länkar till exempel för Azure AD v2.0-slutpunkten. De här exemplen visar hur man gör, tillsammans med kodfragment som du kan använda i dina program. På sidan kod exemplet hittar du detaljerade viktigt avsnitt som hjälp med krav, installation, och Ställ in. Kommentarer i koden finns det för att förstå viktiga avsnitt.

> [!NOTE]
> Om du är intresserad av v1.0 exempel, se [kodexempel för Azure AD (v1.0 slutpunkt)](sample-v1-code.md).

Information om grundläggande scenario för varje typ av exemplet finns i [apptyperna för Azure Active Directory v2.0-slutpunkten](v2-app-types.md).

Du kan också bidra till exemplen på GitHub. Läs hur genom att läsa [Microsoft Azure Active Directory-exempel och dokumentation](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="single-page-applications-spa"></a>Enkelsidigt program (SPA)

De här exemplen visar hur du skriver ett enkelsidigt program som skyddas med Azure AD. De här exemplen Använd någon av varianter av MSAL.js:

* [Microsoft Authentication Library för JavaScript](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core)
* [Microsoft Authentication Library för Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular)
* [Microsoft Authentication Library för AngularJS](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angularjs)

 Plattform |  Anropar Microsoft Graph
 -------- |  ---------------------
![JavaScript](media/sample-v2-code/logo_js.png) JavaScript (msal.js)  | [JavaScript-graphapi-web-v2](https://github.com/Azure-Samples/active-directory-javascript-graphapi-web-v2)
![Angular JS](media/sample-v2-code/logo_angular.png) JavaScript (MSAL AngularJS) | [MsalAngularjsDemoApp](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-angularjs/samples/MsalAngularjsDemoApp)
![Angular](media/sample-v2-code/logo_angular.png) JavaScript (MSAL Angular) | [MSALAngularDemoApp](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-angular/samples/MSALAngularDemoApp)

## <a name="web-applications"></a>Webbprogram

Följande exempel illustrerar webbprogram som loggar in användare. Några exempel visar också programmet anropar Microsoft Graph eller dina egna webb-API med användarens identitet.

 Plattform | Endast loggar in användare | Loggar in användare och anropar Microsoft Graph
 -------- | ------------------- | ---------------------------------
![ASP.NET Core](media/sample-v2-code/logo_NETcore.png)<p/>ASP.NET Core 2.0 | [aspnetcore-webapp-openidconnect-v2](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2) | Samma prov i den [signInAndCallMsGraph](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/signInAndCallMsGraph) gren
![ASP.NET](media/sample-v2-code/logo_NETframework.png)<p/> ASP.NET | [Snabbstart för ASP.NET](https://GitHub.com/AzureAdQuickstarts/AppModelv2-WebApp-OpenIDConnect-DotNet) <p/> [DotNet-webapp-openidconnect-v2](https://GitHub.com/azure-samples/active-directory-dotnet-webapp-openidconnect-v2)  |  [DotNet-admin-begränsade-scope-v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2) <p/>[msgraph-utbildning-aspnetmvcapp](https://github.com/microsoftgraph/msgraph-training-aspnetmvcapp)
![Node.js](media/sample-v2-code/logo_nodejs.png)  |                   | [Snabbstart för node.js](https://github.com/azureadquickstarts/appmodelv2-webapp-openidconnect-nodejs)
![Ruby](media/sample-v2-code/logo_ruby.png) |                   | [msgraph-utbildning-rubyrailsapp](https://github.com/microsoftgraph/msgraph-training-rubyrailsapp)

## <a name="desktop-and-mobile-public-client-apps"></a>Stationära och mobila offentliga klientappar

Följande exempel visar offentlig klient program (desktop/mobila program) som har åtkomst till Microsoft Graph API eller dina egna webb-API för namnet på en användare. Alla dessa klientprogram använder Microsoft Authentication Libraries (MSAL).

Klientprogram | Plattform | Flow/bevilja | Anropar Microsoft Graph | En ASP.NET Core 2.0 webb-API-anrop
------------------ | -------- |  ----------| ---------- | -------------------------
Desktop (WPF)      | ![.NET / C#](media/sample-v2-code/logo_NET.png) | Interaktiv | [DotNet-desktop-msgraph-v2](http://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) | [DotNet-intern-aspnetcore-v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2)
Fjärrskrivbord (konsol)   | ![.NET / C# (skrivbord)](media/sample-v2-code/logo_NET.png) | Integrerad Windows-autentisering |[DotNet-iwa-v2](https://github.com/azure-samples/active-directory-dotnet-iwa-v2)
Fjärrskrivbord (konsol)   | ![.NET / C# (skrivbord)](media/sample-v2-code/logo_NETcore.png) | Användarnamn/lösenord |[dotnetcore-upp-v2](https://github.com/azure-samples/active-directory-dotnetcore-console-up-v2)
Mobile (UWP)   | ![.NET / C# (UWP)](media/sample-v2-code/logo_windows.png) | Interaktiv |[DotNet-intern-uwp-v2](https://github.com/azure-samples/active-directory-dotnet-native-uwp-v2) |
Mobil (Android, iOS, UWP)   | ![.NET / C# (Xamarin)](media/sample-v2-code/logo_xamarin.png) | Interaktiv |[xamarin-intern-v2](https://Github.com/azure-samples/active-directory-xamarin-native-v2) |
Mobile (iOS)       | ![iOS / Objective C eller swift](media/sample-v2-code/logo_iOS.png) | Interaktiv |[IOS-swift-intern-v2](https://github.com/azure-samples/active-directory-ios-swift-native-v2) <p/> [IOS-intern-nxoauth2-v2](https://github.com/azure-samples/active-directory-ios-native-nxoauth2-v2) |
Mobil (Android)   | ![Android / Java](media/sample-v2-code/logo_Android.png) | Interaktiv |  [Android-intern-v2](https://github.com/azure-samples/active-directory-android-native-v2 ) |

## <a name="daemon-applications"></a>Daemon för program

Följande exempel visar ett program som ansluter till Microsoft Graph API med identiteten (med inga användare).

Klientprogram | Plattform | Flow/bevilja | Anropar Microsoft Graph
------------------ | -------- | ---------- | --------------------
Konsolen | ![.NET Core](media/sample-v2-code/logo_NETcore.png)<p/> ASP.NET  | Klientautentiseringsuppgifter | [dotnetcore-daemon-v2](https://github.com/azure-samples/active-directory-dotnetcore-daemon-v2)
Webbapp | ![ASP.NET](media/sample-v2-code/logo_NETframework.png)<p/> ASP.NET  | Klientautentiseringsuppgifter | [DotNet-daemon-v2](https://github.com/azure-samples/active-directory-dotnet-daemon-v2)

## <a name="headless-applications"></a>Fjärradministrerad program

I följande exempel visas ett offentliga klientprogram som körs på en enhet utan att en webbläsare. Appen kan vara ett kommandoradsverktyg, eller köras på Linux/Mac eller ett IoT-program. Exemplet har en app som har åtkomst till Microsoft Graph API namnet på en användare som loggar in interaktivt på en annan enhet (till exempel en mobiltelefon). Det här klientprogrammet använder MicroSoft Authentication Libraries (MSAL).

Klientprogram | Plattform | Flow/bevilja | Anropar Microsoft Graph
------------------ | -------- |  ----------| ----------
Fjärrskrivbord (konsol)   | ![.NET / C# (skrivbord)](media/sample-v2-code/logo_NETcore.png) | Kodflöde för enhet |[dotnetcore-devicecodeflow-v2](https://github.com/azure-samples/active-directory-dotnetcore-devicecodeflow-v2)

## <a name="web-apis"></a>Webb-API:er

I följande exempel visas hur du skyddar ett webb-API med Azure AD v2.0-slutpunkten. Detta API utnyttjas av en WPF-program, men den kan anropas av alla program.

Plattform | Exempel
 -------- | -------------------
![.NET / C#](media/sample-v2-code/logo_NET.png) | WebAPI (tjänst) för [dotnet-intern-aspnetcore-v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2)

## <a name="other-microsoft-graph-samples"></a>Andra Microsoft Graph-exempel

Vill veta mer om [exempel](https://github.com/microsoftgraph/msgraph-community-samples/tree/master/samples#aspnet) och självstudier som demonstrerar olika användningsmönster för Microsoft Graph API, inklusive autentisering med Azure AD finns i [Microsoft Graph Community-exempel och självstudier](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Se också

[Utvecklarhandbok för Azure Active Directory](v1-overview.md)

[Azure AD Graph API konceptuell och referenser](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure AD Graph API hjälpbibliotek](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
