---
title: Hantera uppdateringar och korrigeringar för dina virtuella Windows-datorer i Azure
description: I den här artikeln finns en översikt över hur du använder Azure Automations uppdateringshantering för att hantera uppdateringar och korrigeringsfiler för dina virtuella Azure-datorer i Windows.
services: automation
author: zjalexander
ms.service: automation
ms.component: update-management
ms.topic: tutorial
ms.date: 09/18/2018
ms.author: zachal
ms.custom: mvc
ms.openlocfilehash: 6046781f59b64dcec4769686a2acd710c7b68965
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49987315"
---
# <a name="manage-windows-updates-by-using-azure-automation"></a>Hantera Windows-uppdateringar med hjälp av Azure Automation

Du kan använda uppdateringshanteringen för att hantera uppdateringar och korrigeringar av virtuella datorer. I den här självstudien får du lära dig hur du utvärderar statusen för tillgängliga uppdateringar snabbt, planerar installation av uppdateringar som krävs, granskar distributionsresultat och skapar en avisering för att verifiera uppdateringar.

Prisinformation finns i [Automation-priser för uppdateringshantering](https://azure.microsoft.com/pricing/details/automation/).

I den här guiden får du lära dig att:

> [!div class="checklist"]
> * Publicera en virtuell dator för hantering av uppdateringar
> * Visa en uppdateringsbedömning
> * Konfigurera aviseringar
> * Schemalägga en uppdateringsdistribution
> * Visa resultatet av en distribution

## <a name="prerequisites"></a>Nödvändiga komponenter

För att slutföra den här kursen behöver du:

* En Azure-prenumeration. Om du inte redan har ett konto kan du [aktivera dina månatliga Azure-krediter för Visual Studio-prenumeranter ](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Ett [Azure Automation-konto](automation-offering-get-started.md) för bevakaren samt runbooks-flöden för åtgärd och bevakaraktivitet.
* En [virtuell dator](../virtual-machines/windows/quick-create-portal.md) som du vill publicera.

## <a name="sign-in-to-azure"></a>Logga in på Azure

Logga in på Azure Portal på https://portal.azure.com.

## <a name="enable-update-management"></a>Aktivera uppdateringshantering

Först aktiverar du uppdateringshantering på din virtuella datorn för att kunna genomföra den här självstudien:

1. I Azure Portal, i vänstermenyn, väljer du **Virtuella datorer**. Välj en virtuell dator i listan.
2. På sidan för virtuell dator under **ÅTGÄRDER** väljer du **Uppdateringshantering**. Fönstret **Aktivera uppdateringshantering** öppnas.

Verifieringen utförs för att fastställa om uppdateringshantering är aktiverat för den här virtuella datorn. Verifieringen söker efter en Log Analytics-arbetsyta och ett länkat Azure Automation-konto, och om lösningen för uppdateringshantering är i arbetsytan.

En [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json)-arbetsyta används för att samla in data som genereras av funktioner och tjänster som uppdateringshantering. Arbetsytan tillhandahåller en enda plats för att granska och analysera data från flera källor.

Verifieringsprocessen kontrollerar också om den virtuella datorn har etablerats med MMA och Automation Hybrid Runbook Worker. Den här agenten används för att kommunicera med Azure Automation och hämta information om uppdateringsstatus. Agenten kräver att port 443 till öppnas för att kommunicera med Azure Automation-tjänsten och för att ladda ner uppdateringar.

Om några av följande krav saknades under publiceringen läggs de till automatiskt:

* [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json)-arbetsyta
* Ett [Automation-konto](./automation-offering-get-started.md)
* En [Hybrid Runbook Worker](./automation-hybrid-runbook-worker.md) (aktiverad på den virtuella datorn)

Under **Uppdateringshantering**, ange platsen, Log Analytics-arbetsytan och Automation-kontot som ska användas. Välj sedan **Aktivera**. Om dessa alternativ inte är tillgängliga, innebär det att en annan automation-lösning är aktiverad för den virtuella datorn. I så fall måste samma arbetsyta och Automation-konto användas.

![Aktivera fönstret Uppdateringshanteringslösning](./media/automation-tutorial-update-management/manageupdates-update-enable.png)

Det kan ta några minuter att aktivera lösningen. Under tiden ska du inte stänga webbläsaren. När lösningen har aktiverats flödar information om saknade uppdateringar på den virtuella datorn till Log Analytics. Det kan ta mellan 30 minuter och 6 timmar innan data blir tillgängliga för analys.

## <a name="view-update-assessment"></a>Visa kontroll av uppdateringar

När uppdateringshantering är aktiverat visas fönstret **Uppdateringshantering**. Om några uppdateringar saknas visas en lista med uppdateringar som saknas på fliken  **Uppdateringar som saknas**.

Under **INFORMATIONSLÄNKEN** välj uppdateringslänken för att öppna supportartikeln för uppdateringen i ett nytt fönster. Du kan lära dig viktig information om uppdateringen i det här fönstret.

![Visa uppdateringsstatus](./media/automation-tutorial-update-management/manageupdates-view-status-win.png)

Klicka någon annanstans på uppdateringen för att öppna fönstret **Loggsökning** för den markerade uppdateringen. Frågan för loggsökningen är fördefinierad för den specifika uppdateringen. Du kan ändra den här frågan eller skapa en egen fråga om du vill visa detaljerad information om uppdateringarna som har distribuerats eller som saknas i din miljö.

![Visa uppdateringsstatus](./media/automation-tutorial-update-management/logsearch.png)

## <a name="configure-alerts"></a>Konfigurera varningar

I det här steget lär du dig att ställa in en avisering som meddelar dig när uppdateringar har distribuerats via en Log Analytics-fråga eller genom att spåra huvud-runbook för uppdateringshantering för distributioner som har misslyckats.

### <a name="alert-conditions"></a>Aviseringsvillkor

Det finns olika aviseringsvillkor som måste definieras för varje typ av avisering.

#### <a name="log-analytics-query-alert"></a>Log Analytics-frågeavisering

Du kan skapa en avisering baserad på en Log Analytics-fråga för lyckade distributioner. För misslyckade distributioner kan du använda stegen för [Runbook-aviseringen](#runbook-alert) och få aviseringar när något går fel med den huvud-runbook som orkestrerar distributionen av uppdateringar. Du kan skriva en anpassad fråga för ytterligare aviseringar och omfattar många olika scenarier.

I Azure Portal går du till **Övervaka**, och väljer sedan **Skapa avisering**.

Under **1. Definiera aviseringstillstånd** , klicka på **Välj mål**. Under **Filtrera efter resurstyp** väljer du **Log Analytics**. Välj Log Analytics-arbetsytan och välj sedan **Klar**.

![Skapa avisering](./media/automation-tutorial-update-management/create-alert.png)

Välj **Lägg till villkor**.

Under **Konfigurera signallogik**, i tabellen, välj **Anpassad loggsökning**. Ange sedan följande fråga i textrutan **Sökfråga**:

```loganalytics
UpdateRunProgress
| where InstallationStatus == 'Succeeded'
| where TimeGenerated > now(-10m)
| summarize by UpdateRunName, Computer
```
Den här frågan returnerar datorer och uppdateringens körnamn som slutförts under den tidsperiod som angetts.

Under **Aviseringslogik**, för **Tröskelvärde**, ange **1**. När du är klar väljer du **Klar**.

![Konfigurera signallogiken](./media/automation-tutorial-update-management/signal-logic.png)

#### <a name="runbook-alert"></a>Runbook-avisering

För misslyckade distributioner måste du avisera om felet i din master-runbook.
I Azure Portal går du till **Övervaka**, och väljer sedan **Skapa avisering**.

Under **1. Definiera aviseringstillstånd** , klicka på **Välj mål**. Under **Filtrera efter resurstyp** väljer du **Automation-konton**. Välj ditt Automation-konto och välj **Klar**.

För **Namn på runbook** klickar du på **\+**-tecknet och skriver in **Patch-MicrosoftOMSComputers** som anpassat namn. För **Status** väljer du **Misslyckad** eller klickar på **\+**-tecknet och skriver **Misslyckad**.

![Konfigurera signallogiken för runbook-flöden](./media/automation-tutorial-update-management/signal-logic-runbook.png)

Under **Aviseringslogik**, för **Tröskelvärde**, ange **1**. När du är klar väljer du **Klar**.

### <a name="alert-details"></a>Aviseringsinformation

Under **2. Definiera aviseringsinformation**, ange ett namn och en beskrivning för aviseringen. Ange **allvarlighetsgrad** till **Information (allvarlighetsgrad 2)** för en lyckad körning eller **Information (allvarlighetsgrad 1)** för en misslyckad körning.

![Konfigurera signallogiken](./media/automation-tutorial-update-management/define-alert-details.png)

Under **3. Definiera åtgärdsgrupp**, välj **Ny åtgärdsgrupp**. En åtgärdsgrupp är en grupp av åtgärder som kan användas i flera aviseringar. Dessa åtgärder kan inkludera, men är inte begränsade till, e-postmeddelanden, runbooks, webhooks och mycket mer. Läs mer om åtgärdsgrupper i [Skapa och hantera åtgärdsgrupper](../monitoring-and-diagnostics/monitoring-action-groups.md).

I rutan **Åtgärdsgruppnamn** anger du ett namn för aviseringen och ett kortnamn. Det korta namnet används i stället för ett fullständigt åtgärdsgruppnamn när meddelanden skickas med den här gruppen.

Under **Åtgärder**, ange ett namn för åtgärden, såsom **e-postmeddelanden**. Under **ÅTGÄRDSTYP**, välj **e-post/SMS/Push/röst**. Under **INFORMATION** väljer du **Redigera detaljer**.

I rutan **e-post/SMS/Push/röst**, ange ett namn. Välj kryssrutan **e-post** och ange sedan en giltig e-postadress.

![Konfigurera en e-poståtgärdsgrupp](./media/automation-tutorial-update-management/configure-email-action-group.png)

I rutan **e-post/SMS/Push/röst**, välj **OK**. I fönstret **Lägg till åtgärdsgrupp**, välj **OK**.

Anpassa föremål för aviserings-e-postmeddelandet under **Skapa regel**, under **Anpassa åtgärder**, välj **e-postmeddelandets ämne**. När du är klar väljer du **Skapa varningsregel**. Varningen berättar när en distribution lyckas och vilka datorer som var en del av denna uppdaterade distributionskörning.

## <a name="schedule-an-update-deployment"></a>Schemalägga en uppdateringsdistribution

För att installera uppdateringar schemalägger du en distribution som passar ditt schema och servicefönster. Du kan välja vilka uppdateringstyper som ska tas med i distributionen. Du kan till exempel ta med kritiska uppdateringar eller säkerhetsuppdateringar och exkludera samlade uppdateringar.

Schemalägg en ny uppdateringsdistribution för den virtuella datorn, gå till **Uppdateringshantering** och välj sedan **Distribution av schemauppdatering**.

Under **Ny uppdateringsdistribution** anger du följande information:

* **Namn**: Ange ett unikt namn på uppdateringsdistributionen.

* **Operativsystem**: Välj operativsystem som mål för uppdateringsdistributionen.

* **Grupper att uppdatera (förhandsversion)**: definiera en fråga som baseras på en kombination av prenumeration, resursgrupper, platser och taggar för att skapa en dynamisk grupp med virtuella Azure-datorer som ska ingå i din distribution. Mer information finns i [Dynamiska grupper](automation-update-management.md#using-dynamic-groups)

* **Datorer som ska uppdateras**: Välj en sparad sökning eller en importerad grupp, eller välj Dator i listrutan och välj enskilda datorer. Om du väljer **Datorer** visas beredskapen för datorn i kolumnen **Uppdatera agentberedskap**. Mer om de olika metoderna för att skapa datorgrupper i Log Analytics finns i dokumentationen om [datorgrupper i Log Analytics](../log-analytics/log-analytics-computer-groups.md)

* **Uppdatera klassificering**: Välj vilka typer av programvara som ska tas med i uppdateringsdistributionen. Låt alla typer vara markerade för den här självstudien.

  Klassificeringstyper:

   |Operativsystem  |Typ  |
   |---------|---------|
   |Windows     | Kritiska uppdateringar</br>Säkerhetsuppdateringar</br>Samlade uppdateringar</br>Funktionspaket</br>Service pack</br>Definitionsuppdateringar</br>Verktyg</br>Uppdateringar        |
   |Linux     | Kritiska uppdateringar och säkerhetsuppdateringar</br>Övriga uppdateringar       |

   En beskrivning av klassificeringstyper finns i [uppdatera klassificeringar](automation-update-management.md#update-classifications).

* **Uppdateringar att inkludera/exkludera** – detta öppnar sidan **Inkludera/exkludera**. Uppdateringar som ska inkluderas eller exkluderas visas på en separat flik. Mer information om hur inkludering hanteras och finns i [inkluderingsbeteende](automation-update-management.md#inclusion-behavior)

* **Schemainställningar**: Sidan **Schemainställningar** öppnas. Starttiden är som standard 30 minuter efter den aktuella tiden. Du kan ange starttiden till helst från 10 minuter i framtiden.

   Du kan också ange om distributionen ska ske en gång eller ange ett schema med återkommande tider. Under **Återkommande**, välj **En gång**. Lämna standardvärdet till 1 dag och välj **OK**. Då ställs ett återkommande schema in.

* **Skript före och efter**: Välj skript som ska köras före och efter distributionen. Mer information finns i [Hantera skript före och efter](pre-post-scripts.md).
* **Underhållsperiod (minuter)**: Låt standardvärdet stå kvar. Du kan ange tidsfönstret som du vill att distributionen av uppdateringen ska ske inom. Den här inställningen hjälper till att säkerställa att ändringarna utförs inom ditt definierade servicefönster.

* **Omstartsalternativ**: Den här inställningen avgör hur omstarter ska hanteras. De tillgängliga alternativen är:
  * Starta om vid behov (standard)
  * Starta alltid om
  * Starta aldrig om
  * Endast omstart – uppdateringar installeras inte

När du är klar med att konfigurera schemat väljer du **Skapa**.

![Inställningsfönster för uppdateringsschema](./media/automation-tutorial-update-management/manageupdates-schedule-win.png)

Du kommer tillbaka till statusinstrumentpanelen. Välj **Schemalagda uppdateringsdistributioner** om du vill visa distributionsschemat du har skapat.

> [!NOTE]
> Uppdateringshantering har stöd för distribution av förstapartsuppdateringar och förnedladdning av korrigeringar. Detta kräver ändringar på de system som korrigeras. Information om hur du konfigurerar dessa inställningar på systemet finns i avsnittet om [support för förstapart och förnedladdning](automation-update-management.md#firstparty-predownload).

## <a name="view-results-of-an-update-deployment"></a>Visa resultat för en uppdateringsdistribution

När den schemalagda distributionen startar kan du se status för distributionen på fliken **Uppdateringsdistributioner** under **Uppdateringshantering**. Statusen är **Pågår** när distributionen körs. När distributionen är klar, om den lyckas, ändras statusen till **Lyckades**. Om det uppstod några fel med en eller flera uppdateringar i distributionen visas status **Misslyckades delvis**.

Välj den slutförda uppdateringsdistributionen för att visa instrumentpanelen för distributionen.

![Statusinstrumentpanel för Uppdatera distribution för en specifik distribution](./media/automation-tutorial-update-management/manageupdates-view-results.png)

Under **Uppdateringsresultat** visas en sammanfattning av det totala antalet uppdateringar och distributionsresultat för den virtuella datorn. Tabellen till höger visar detaljer för varje uppdatering och installationsresultaten.

I följande lista visas tillgängliga värden:

* **Inget försök har gjorts**: Uppdateringen installerades inte eftersom det inte fanns tillräckligt med tid utifrån det underhållsfönster som definierats.
* **Lyckades**: Uppdateringen lyckades.
* **Misslyckades**: Uppdateringen misslyckades.

Välj **Alla loggar** om du vill se alla loggposter som har skapats för distributionen.

Välj **Utdata** om du vill se jobbströmmen för den runbook som ansvarar för att hantera uppdateringsdistributionen på den virtuella måldatorn.

Välj **Fel** om du vill se detaljerad information om fel som uppstått vid distributionen.

När uppdateringsdistributionen av är klar skickas e-post som liknar följande exempel för att visa resultatet av distributionen:

![Konfigurera e-poståtgärdsgrupp](./media/automation-tutorial-update-management/email-notification.png)

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen lärde du dig att:

> [!div class="checklist"]
> * Publicera en virtuell dator för hantering av uppdateringar
> * Visa en uppdateringsbedömning
> * Konfigurera aviseringar
> * Schemalägga en uppdateringsdistribution
> * Visa resultatet av en distribution

Fortsätt till översikten för uppdateringshanteringslösningen.

> [!div class="nextstepaction"]
> [Uppdateringshanteringslösning](../operations-management-suite/oms-solution-update-management.md?toc=%2fazure%2fautomation%2ftoc.json)
