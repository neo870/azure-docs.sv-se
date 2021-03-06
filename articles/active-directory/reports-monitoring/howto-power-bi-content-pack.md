---
title: Så här använder du innehållspaketet för Azure Active Directory Power BI | Microsoft Docs
description: Lär dig hur du använder innehållspaketet för Azure Active Directory Power BI
services: active-directory
author: priyamohanram
manager: mtillman
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.component: report-monitor
ms.date: 12/06/2017
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: c0326a6b611d5f3d5633db2d2b64b8cdc15e10a7
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816692"
---
# <a name="how-to-use-the-azure-active-directory-power-bi-content-pack"></a>Så här använder du innehållspaketet för Azure Active Directory Power BI

|  |
|--|
|Azure AD Power BI-Innehållspaketet använder för närvarande, Azure AD Graph-API: er för att hämta data från Azure AD-klienten. Därför kan det hända att vissa skillnader mellan data som är tillgängliga i Innehållspaketet och data som hämtats med hjälp av den [Microsoft Graph API: er för rapportering](concept-reporting-api.md). |
|  |

Som IT-administratör måste du förstå hur dina användare införa och använda Azure Active Directory-funktioner. På så sätt kan du planera IT-infrastruktur och kommunikation för att öka användningen och få ut det mesta av Azure AD-funktioner. Power BI-Innehållspaketet för Azure Active Directory ger dig möjlighet att ytterligare analysera dina data för att samla in mer omfattande insikter om vad som händer med din katalog. Med integrering av Azure Active Directory-API: er i Power BI kan du enkelt ladda ned det färdiga Innehållspaketet och få insyn i alla aktiviteter i Azure Active Directory med hjälp av den omfattande visualiseringsupplevelsen som Power BI erbjuder. Du kan skapa en egen instrumentpanel och enkelt dela den med andra i din organisation. 

## <a name="prerequisites"></a>Förutsättningar

Du behöver en Azure AD premium (P1/P2) licens att använda Innehållspaketet. 

## <a name="install-the-content-pack"></a>Installera Innehållspaketet

Kolla in den [snabbstarten](quickstart-install-power-bi-content-pack.md) att installera Azure AD Power BI-Innehållspaketet.

## <a name="what-can-i-do-with-this-content-pack"></a>Vad kan jag göra med det här innehållspaketet?

Här är en snabb genomgång av olika rapporter i innehållspaketet innan vi går in på det du kan göra med innehållspaketet. Rapportdata finns för de **senaste 30 dagarna**.

### <a name="reports-included-in-this-version-of-azure-active-directory-logs-content-pack"></a>Rapporter som ingår i den här versionen av innehållspaketet för Azure Active Directory-loggar

**Appanvändning och trendrapporter**: Få insikter om de appar som används i din organisation, vilka som används mest och när de används. Du kan använda den här rapporten för att samla in insikter om hur en app som du nyligen distribuerade i organisationen används eller ta reda på vilka appar som är populära. På så sätt kan du förbättra användningen om du ser att en app inte används.

**Inloggningar efter plats och användare**: Ger information om alla inloggningar som utförs med hjälp av Azure Identity och information om användarnas identitet. Informationen hjälper dig att få djupare insikt i enskilda inloggningar och besvara frågor som:

- Varifrån loggade den här användaren in?
- Vilken användare har flest inloggningar och varifrån loggade användaren in? 
- Lyckades inloggningen?  
 
Du kan se ytterligare information genom att klicka på ett visst datum eller en viss plats.

**Unika användare per app**: Få en överblick över alla unika användare som använder en viss app. Detta omfattar endast användare som har loggat in till ett program ”*utan problem*”.

**Enhetsinloggningar**: Få en översikt över vilka operativsystem och webbläsare som användare använder i organisationen med detaljerad information om användarna som omfattar:

- Användarnamn
- IP-adress
- Plats 
- Inloggningsstatus 

Med den här rapporten kan du förstå de olika enhetsprofiler som används inom organisationen och bestämma principer för enheter baserat på vad som används

**SSPR-tratt**: Få förståelse för hur lösenordsåterställning fungerar i din organisation. Ta en förhandstitt på hur många lösenordsåterställningar som försökte utföras via SSPR-verktyget och hur många av dem som genomfördes. Lär dig mer om fel vid lösenordsåterställning med SSPR-tratten och förstå varför vissa fel uppstod. Den här rapporten ger en bättre förståelse för hur SSPR-verktyget används inom organisationen så att du kan fatta rätt beslut.

## <a name="customizing-azure-ad-activity-content-pack"></a>Anpassa innehållspaketet för Azure AD Activity

**Ändra visualisering**: Du kan ändra ett visuellt objekt i en rapport genom att klicka på **Redigera rapport** och välja det önskade visuella objektet.
 
![Innehållspaketet för Azure Active Directory Power BI](./media/howto-power-bi-content-pack/09.png) 
 
![Innehållspaketet för Azure Active Directory Power BI](./media/howto-power-bi-content-pack/10.png) 

**Ta med ytterligare fält**: Du kan lägga till ett fält i rapporten eller ta bort det genom att välja det visuella objekt som du vill lägga till/ta bort från fältet. Jag lägger till fältet ”inloggningsstatus” i tabellvyn i exemplet nedan. 
 
![Innehållspaketet för Azure Active Directory Power BI](./media/howto-power-bi-content-pack/11.png) 

**Fäst visuella objekt på instrumentpanelen**: Du kan anpassa instrumentpanelen och ta med egna visuella objekt i rapporten och fästa på instrumentpanelen. I exemplet nedan lade jag till ett nytt filter som kallas ”Inloggningsstatus” och tog med det i rapporten. Jag ändrade även det visuella objektet från stapeldiagram till linjediagram och kan fästa det här nya visuella objektet på instrumentpanelen.

![Innehållspaketet för Azure Active Directory Power BI](./media/howto-power-bi-content-pack/12.png) 

![Innehållspaketet för Azure Active Directory Power BI](./media/howto-power-bi-content-pack/13.png) 
 

 


**Dela instrumentpanelen**: När du har skapat innehållet kan du dela instrumentpanelen med användare i organisationen. Kom ihåg att de kan se fälten som du markerat när du delar rapporten.
 
![Innehållspaketet för Azure Active Directory Power BI](./media/howto-power-bi-content-pack/14.png) 



## <a name="scheduling-a-daily-refresh-of-your-power-bi-report"></a>Schemalägga en daglig uppdatering av Power BI-rapporter

Om du vill schemalägga en daglig uppdatering av en Power BI-rapport går du till **Datauppsättningar > Inställningar > Schemalägg uppdatering** och konfigurerar det enligt nedan.
 
![Innehållspaketet för Azure Active Directory Power BI](./media/howto-power-bi-content-pack/15.png) 

## <a name="updating-to-newer-version-of-content-pack"></a>Uppdatera till en nyare version av innehållspaketet

Om du vill uppdatera ditt innehållspaket för att få en nyare version:

- Ladda ned det nya innehållspaketet och konfigurera enligt anvisningarna i den här artikeln.

- När du har konfigurerat det går du till **Datakälla > Inställningar > Autentiseringsuppgifter för datakälla** och anger dina autentiseringsuppgifter på nytt enligt anvisningarna nedan

    ![Innehållspaketet för Azure Active Directory Power BI](./media/howto-power-bi-content-pack/16.png) 

När den nya versionen av innehållspaketet fungerar kan du ta bort den gamla versionen vid behov genom att ta bort de underliggande rapporterna och de datauppsättningar som är associerade med innehållspaketet.

## <a name="still-having-issues"></a>Har du fortfarande problem? 

Läs vår [felsökningsguide](troubleshoot-content-pack.md). Läs de här [hjälpartiklarna](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) för allmän hjälp med Power BI.
 

## <a name="next-steps"></a>Nästa steg

* [Installera Power BI-Innehållspaketet](quickstart-install-power-bi-content-pack.md).
* [Felsöka fel i innehållspaket](troubleshoot-content-pack.md).
* [Vad är Azure AD-rapporter? ](overview-reports.md).
