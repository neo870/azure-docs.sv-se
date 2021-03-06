---
title: Skapa och hantera åtgärdsgrupper i Azure portal
description: Lär dig hur du skapar och hanterar åtgärdsgrupper i Azure-portalen.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/12/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: 3ce7c5111fa176bb7fa734f54084b9e14e7afbef
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51016054"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Skapa och hantera åtgärdsgrupper i Azure portal
## <a name="overview"></a>Översikt ##
En åtgärdsgrupp är en samling av aviseringsinställningarna som definierats av användaren. Azure Monitor och Service Health-aviseringar är konfigurerade för att använda en specifik åtgärdsgrupp när aviseringen utlöses. Olika typer av aviseringar kan använda samma åtgärdsgruppen eller annan åtgärdsgrupper beroende på användarens krav.

Den här artikeln visar hur du skapar och hanterar åtgärdsgrupper i Azure-portalen.

Varje åtgärd består av följande egenskaper:

* **Namn på**: en unik identifierare i åtgärdsgruppen.  
* **Åtgärdstyp**: åtgärden som ska utföras. Exempel är att skicka en röst-anrop, SMS, e-postmeddelandet, eller utlösa olika typer av automatiska åtgärder. Se typer senare i den här artikeln. 
* **Information om**: motsvarande information som kan variera efter *åtgärdstyp*. 

Information om hur du använder Azure Resource Manager-mallar för att konfigurera åtgärdsgrupper finns i [åtgärd grupp Resource Manager-mallar](monitoring-create-action-group-with-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Skapa en grupp med hjälp av Azure-portalen ##
1. I den [portal](https://portal.azure.com)väljer **övervakaren**. Den **övervakaren** bladet konsoliderar alla dina övervakningsinställningar och -data i en vy.

    ![”Övervakningstjänsten”](./media/monitoring-action-groups/home-monitor.png)
1. Välj **aviseringar** därefter **hantera åtgärdsgrupper**.

    ![Hantera åtgärdsgrupper knappen](./media/monitoring-action-groups/manage-action-groups.png)
1. Välj **Lägg till åtgärdsgrupp**, och Fyll i fälten.

    ![Kommandot ”Lägg till åtgärdsgrupp”](./media/monitoring-action-groups/add-action-group.png)
1. Ange ett namn i den **namn på åtgärdsgrupp** och ange ett namn i den **kortnamnet** box. Det korta namnet används i stället för ett fullständigt åtgärdsgruppnamn när meddelanden skickas med den här gruppen.

      ![Dialogrutan Lägg till åtgärdsgrupp ”](./media/monitoring-action-groups/action-group-define.png)

1. Den **prenumeration** rutan autofills med din aktuella prenumeration. Den här prenumerationen är där åtgärdsgruppen sparas.

1. Välj den **resursgrupp** i åtgärdsgruppen har sparats.

1. Definiera en lista med åtgärder genom att tillhandahålla varje åtgärd:

    a. **Namn på**: Ange en unik identifierare för den här åtgärden.

    b. **Åtgärdstyp**: Välj e-post/SMS/Push/röst, Logikapp, Webhook, ITSM eller Automation-Runbook.

    c. **Information om**: baserat på typen av, ange ett telefonnummer, e-postadress, webhook URI, Azure-app, ITSM-anslutningen eller Automation-runbook. För ITSM-åtgärden dessutom ange **arbetsobjekt** och kräver att andra fält ITSM-verktyg.

1. Välj **OK** skapa åtgärdsgruppen.

## <a name="manage-your-action-groups"></a>Hantera din åtgärdsgrupper ##
När du har skapat en åtgärdsgrupp den syns i den **åtgärdsgrupper** delen av den **övervakaren** bladet. Välj åtgärdsgrupp som du vill hantera att:

* Lägga till, redigera eller ta bort åtgärder.
* Ta bort åtgärdsgruppen.

## <a name="action-specific-information"></a>Information om specifika
**Azure-app Push** – du kan ha upp till 10 Azure åtgärder i en åtgärdsgrupp. Azure-app-åtgärden stöder endast ServiceHealth aviseringar just nu. En annan tidpunkt som aviseringen kommer att ignoreras. Se [konfigurera aviseringar när en avisering om tjänstens hälsa publiceras](monitoring-activity-log-alerts-on-service-notifications.md).

**E-post** -e-postmeddelanden ska skickas från följande e-postadresser. Kontrollera att din e-filtrering har konfigurerats på rätt sätt
   - azure-noreply@microsoft.com
   - azureemail-noreply@microsoft.com
   - alerts-noreply@mail.windowsazure.com

Du kan ha upp till 1 000 e poståtgärder i en åtgärdsgrupp. Se den [begränsar information frekvensbegränsningen](./monitoring-alerts-rate-limiting.md) artikel

**ITSM** – du kan ha upp till 10 ITSM-åtgärder i en åtgärd grupp ITSM-åtgärden kräver en ITSM-anslutningen. Lär dig hur du skapar en [ITSM-anslutningen](../log-analytics/log-analytics-itsmc-overview.md).

**Logic App** – du kan ha upp till 10 Logic App-åtgärder i en åtgärdsgrupp

**Runbook** -du kan ha upp till 10 Runbook-åtgärder i en åtgärd grupp referera till den [Azure-prenumerationstjänsten](../azure-subscription-service-limits.md) för gränser för Runbook-nyttolaster

**SMS** – du kan ha upp till 10 SMS-åtgärder i en grupp finns åtgärd i den [begränsar information frekvensbegränsningen](./monitoring-alerts-rate-limiting.md) artikel se den [SMS Avisera beteende](monitoring-sms-alert-behavior.md) artikeln

**Röst** – du kan ha upp till 10 Voice-åtgärder i en åtgärdsgrupp</dd>
Se den [begränsar information frekvensbegränsningen](./monitoring-alerts-rate-limiting.md) artikel</dd>

**Webhook** – du kan ha upp till 10 Webhook-åtgärder i en åtgärdsgrupp. Logik för omprövning - tidsgränsen för svar är 10 sekunder. Webhook-anrop kommer att göras upp till 2 gånger när följande HTTP-Statuskoder returneras: 408, 429, 503, 504 eller HTTP-slutpunkten inte svarar. Det första återförsöket sker efter 10 sekunder. Andra och sista återförsök sker efter 100 sekunder.

Käll-IP-adressintervall
    - 13.106.57.181
    - 13.106.54.3
    - 13.106.54.19
    - 13.106.38.142
    - 13.106.38.148
    - 13.106.57.196

Att ta emot uppdateringar om ändringar av dessa IP-adresser som vi rekommenderar att du konfigurerar en [hälsoavisering för tjänst](./monitoring-service-notifications.md) som övervakar för informationsmeddelanden om tjänsten åtgärdsgrupper.


## <a name="next-steps"></a>Nästa steg ##
* Läs mer om [SMS Avisera beteende](monitoring-sms-alert-behavior.md).  
* Få en [förståelse för avisering webhook för aktivitetslogg](monitoring-activity-log-alerts-webhook.md).  
* Läs mer om [ITSM-anslutningsprogram](../log-analytics/log-analytics-itsmc-overview.md)
* Läs mer om [hastighetsbegränsning](monitoring-alerts-rate-limiting.md) på aviseringar.
* Hämta en [översikt över aktivitetsloggaviseringar](monitoring-overview-alerts.md), och lär dig hur du får aviseringar.  
* Lär dig hur du [konfigurera aviseringar när en avisering om tjänstens hälsa publiceras](monitoring-activity-log-alerts-on-service-notifications.md).
