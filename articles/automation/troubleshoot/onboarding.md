---
title: Felsöka fel onboarding uppdateringshantering, ändringsspårning och inventering
description: Lär dig att felsöka onboarding med uppdateringshantering, ändringsspårning och inventering lösningar
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 40a1955e88b23ecfb86412b388413b920dd2eb1a
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/18/2018
ms.locfileid: "49407613"
---
# <a name="troubleshoot-errors-when-onboarding-solutions"></a>Felsöka fel när onboarding-lösningar

Fel kan uppstå när onboarding-lösningar som uppdateringshantering eller ändringsspårning och inventering. Den här artikeln beskriver de olika fel som kan uppstå och hur du löser dem.

## <a name="general-errors"></a>Allmänt fel

### <a name="computer-grou-query-format-error"></a>Scenario: ComputerGroupQueryFormatError

#### <a name="issue"></a>Problem

Den här felkoden innebär att sparad sökning dator grupp frågan används för att rikta lösningen inte har formaterats korrekt. 

#### <a name="cause"></a>Orsak

Frågan kan ha ändrats eller den kan ha ändrats av systemet.

#### <a name="resolution"></a>Lösning

Du kan ta bort frågan för den här lösningen och reonboard lösning som återskapar frågan. Frågan finns i din arbetsyta under **sparade sökningar**. Namnet på frågan är **MicrosoftDefaultComputerGroup**, och kategorin för frågan är namnet på den lösning som är associerad med den här frågan. Om flera lösningar är aktiverade i **MicrosoftDefaultComputerGroup** visas flera gånger under **sparade sökningar**.

### <a name="policy-violation"></a>Scenario: PolicyViolation

#### <a name="issue"></a>Problem

Den här felkoden betyder att distributionen misslyckades på grund av överträdelse av en eller flera principer.

#### <a name="cause"></a>Orsak 

En princip är på plats som blockerar åtgärden att slutföras.

#### <a name="resolution"></a>Lösning

För att kunna distribuera lösningen, måste du överväga att ändra den angivna principen. Eftersom det finns många olika typer av principer som kan definieras, beror specifika ändringar som krävs på den princip som har överskridits. Till exempel om en princip har definierats i en resursgrupp som nekas behörighet att ändra innehållet i vissa typer av resurser i resursgruppen, kan du till exempel göra något av följande:

* Ta bort principen helt och hållet.
* Försök att publicera till en annan resursgrupp.
* Ändra principen, av, till exempel:
  * Nytt inriktning principen till en specifik resurs (till exempel ett specifikt Automation-konto).
  * Ändra uppsättningen av resurser principen har konfigurerats för att neka.

Kontrollera meddelanden i det övre högra hörnet i Azure-portalen eller navigera till resursgruppen som innehåller ditt automation-konto och välj **distributioner** under **inställningar** att visa den distribution. Mer information om Azure Policy besök: [översikt över Azure Policy](../../azure-policy/azure-policy-introduction.md?toc=%2fazure%2fautomation%2ftoc.json).

## <a name="mma-extension-failures"></a>MMA-datortillägg

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 

När du distribuerar en lösning distribueras en mängd relaterade resurser. En av dessa resurser är tillägget Microsoft Monitoring Agent eller Log Analytics-agenten för Linux. Dessa är tillägg till virtuella datorer installeras med den virtuella datorn Gästagent som ansvarar för att kommunicera med den konfigurerade Log Analytics-arbetsytan i syfte att senare samordning av hämtning av binärfiler och andra filer som den lösningen som du är beroende av när den körs.
Du vanligtvis får först kännedom om MMA eller Log Analytics-agenten för Linux-installationsfel från ett meddelande som visas i Meddelandehubben. När du klickar på som meddelanden ger ytterligare information om det specifika felet. Navigering till resursen i resursgrupper och sedan till elementet distributioner i den innehåller också information om distributionsfel som uppstått.
Installation av MMA eller Log Analytics-agenten för Linux kan misslyckas av olika orsaker och stegen för att åtgärda de här felen varierar beroende på problemet. Specifika felsökningsanvisningar följer.

I följande avsnitt beskrivs olika problem som kan uppstå när onboarding som orsakar ett fel vid distribution av MMA-tillägget.

### <a name="webclient-exception"></a>Scenario: Ett undantag uppstod under en WebClient-begäran

MMA-tillägget på den virtuella datorn kan inte kommunicera med externa resurser och distributionen misslyckas.

#### <a name="issue"></a>Problem

Här följer några exempel på felmeddelanden som returneras:

```
Please verify the VM has a running VM agent, and can establish outbound connections to Azure storage.
```

```
'Manifest download error from https://<endpoint>/<endpointId>/Microsoft.EnterpriseCloud.Monitoring_MicrosoftMonitoringAgent_australiaeast_manifest.xml. Error: UnknownError. An exception occurred during a WebClient request.
```

#### <a name="cause"></a>Orsak

Några möjliga orsaker till felet är:

* Det finns en proxyserver i den virtuella datorn, som endast tillåter specifika portar.

* En brandväggsinställning har blockerat åtkomsten till de nödvändiga portarna och adresser.

#### <a name="resolution"></a>Lösning

Kontrollera att du har rätt portar och adresser som är öppna för kommunikation. En lista över portar och adresser finns i [planerar nätverket](../automation-hybrid-runbook-worker.md#network-planning).

### <a name="transient-environment-issue"></a>Scenario: Installationen misslyckades på grund av tillfälliga miljö

Installationen av tillägget Microsoft Monitoring Agent misslyckades under distributionen på grund av en annan installation eller åtgärd som blockerar installationen

#### <a name="issue"></a>Problem

Följande är exempel på felmeddelanden som kan returneras:

```
The Microsoft Monitoring Agent failed to install on this machine. Please try to uninstall and reinstall the extension. If the issue persists, please contact support.
```

```
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1618'
```

```
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.2) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.2\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1601'
```

#### <a name="cause"></a>Orsak

Några möjliga orsaker till felet är:

* En annan installation pågår
* Systemet är utlöstes för att starta om när mallen distribueras

#### <a name="resolution"></a>Lösning

Det här felet är ett tillfälligt fel sin natur. Gör om distributionen för att installera tillägget.

### <a name="installation-timeout"></a>Scenario: Tidsgräns för Installation

Installationen av MMA-tillägget kunde inte slutföras på grund av en tidsgräns.

#### <a name="issue"></a>Problem

Följande är ett exempel på ett felmeddelande som kan returneras:

```
Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 15614
```

#### <a name="cause"></a>Orsak

Det här felet beror på den virtuella datorn är hårt belastat under installationen.

### <a name="resolution"></a>Lösning

Försök att installera tillägget MMA när den virtuella datorn har en lägre belastning.

## <a name="next-steps"></a>Nästa steg

Om du inte ser ditt problem eller inte kan lösa problemet besöker du någon av följande kanaler för ytterligare support:

* Få svar från Azure-experter via [Azure-forumen](https://azure.microsoft.com/support/forums/)
* Anslut till [@AzureSupport](https://twitter.com/azuresupport) – det officiella Microsoft Azure-kontot för att förbättra kundtjänstupplevelsen genom att ansluta Azure-communityn till rätt resurser: svar, support och experter.
* Om du behöver mer hjälp kan öppna du en incident i Azure-supporten. Gå till den [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och välj **hämta stöder**.
