---
title: Vanliga frågor och svar om Azure trafikanalys | Microsoft Docs
description: Få svar på några av de vanligaste frågorna om trafikanalys.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/08/2018
ms.author: jdial
ms.openlocfilehash: 69d2d80e40400cc7fa40aeb5a163dce5036905ab
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/18/2018
ms.locfileid: "49402768"
---
# <a name="traffic-analytics-frequently-asked-questions"></a>Vanliga och frågor svar om trafikanalys

Den här artikeln samlar in på samma ställe många av de vanligaste frågorna om trafikanalys i Azure Network Watcher.

## <a name="what-are-the-prerequisites-to-use-traffic-analytics"></a>Vilka är kraven för att använda trafikanalys?

Trafikanalys kräver följande förutsättningar:

- En Network Watcher aktiverat prenumeration.
- Flödesloggar för Nätverkssäkerhetsgruppen (NSG) har aktiverats för NSG: er som du vill övervaka.
- Ett Azure Storage-konto, att lagra rådata flog loggar.
- En Azure Log Analytics-arbetsyta med läs- och skrivbehörighet.

Ditt konto måste uppfylla ett av följande för att aktivera trafikanalys:

- Ditt konto måste tilldelas till någon av följande roller på prenumerationsnivå: kontoadministratör, tjänstadministratör eller delad administratör.
- Kontot måste ha någon av följande roller för rollbaserad åtkomstkontroll (RBAC) prenumerationsområde: ägare, deltagare, läsare eller nätverksdeltagare.
- Om ditt konto inte har tilldelats någon av ovanstående roller, måste det tilldelas till en anpassad roll som tilldelats följande åtgärder på prenumerationsnivån.
            
    - Microsoft.Network/applicationGateways/read
    - Microsoft.Network/connections/read
    - Microsoft.Network/loadBalancers/read 
    - Microsoft.Network/localNetworkGateways/read 
    - Microsoft.Network/networkInterfaces/read 
    - Microsoft.Network/networkSecurityGroups/read 
    - Microsoft.Network/publicIPAddresses/read
    - Microsoft.Network/routeTables/read
    - Microsoft.Network/virtualNetworkGateways/read 
    - Microsoft.Network/virtualNetworks/read
        
Så här kontrollerar roller som är tilldelade till en användare för en prenumeration:

1. Logga in på Azure med hjälp av **Login-AzureRmAccount**. 

2. Välj prenumerationen som krävs med hjälp av **Select-AzureRmSubscription**. 

3. Om du vill visa alla roller som har tilldelats en angiven användare använda **Get-AzureRmRoleAssignment - SignInName [användarens e-postadress] - IncludeClassicAdministrators**. 

Om du inte ser några utdata, kontakta administratören för respektive prenumeration för att få åtkomst till kommandon. Mer information finns i [Hantera rollbaserad åtkomstkontroll med Azure PowerShell](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell).


## <a name="in-which-azure-regions-are-traffic-analytics-available"></a>Där Azure är regioner trafikanalys som är tillgängliga?

Du kan använda trafikanalys för NSG: er i någon av följande stöds regioner: västra centrala USA, östra USA, östra USA 2, norra centrala USA, södra centrala USA, centrala USA, västra USA, västra USA 2, Västeuropa, Nordeuropa, Storbritannien, västra, Storbritannien, södra, Östra Australien, sydöstra Australien och Sydostasien. Log Analytics-arbetsytan måste finnas i det västra centrala USA, östra USA, Västeuropa, Storbritannien, södra, Australien, sydöstra Australien eller regionen Sydostasien.

## <a name="can-the-nsgs-i-enable-flow-logs-for-be-in-different-regions-than-my-workspace"></a>Kan jag aktivera flödet NSG: erna loggar för finnas i olika regioner än Min arbetsyta?

Ja, de här NSG: er kan finnas i olika regioner än din Log Analytics-arbetsyta.

## <a name="can-multiple-nsgs-be-configured-within-a-single-workspace"></a>Kan flera NSG: er konfigureras i en enda arbetsyta?

Ja.

## <a name="can-i-use-an-existing-workspace"></a>Kan jag använda en befintlig arbetsyta?

Ja. Om du väljer en befintlig arbetsyta kan du kontrollera att den har migrerats till det nya frågespråket. Om du inte vill uppgradera arbetsytan måste du skapa en ny. Läs mer om det nya frågespråket [uppgradering av Azure Log Analytics till ny loggsökning](../log-analytics/log-analytics-log-search-upgrade.md).

## <a name="can-my-azure-storage-account-be-in-one-subscription-and-my-log-analytics-workspace-be-in-a-different-subscription"></a>Kan min Azure Storage-konto i en prenumeration och min Log Analytics-arbetsyta är i en annan prenumeration?

Ja, Azure Storage-kontot kan vara i en prenumeration och Log Analytics-arbetsytan kan vara i en annan prenumeration.

## <a name="can-i-store-raw-logs-in-a-different-subscription"></a>Kan jag lagra loggarna i en annan prenumeration?

Nej. Du kan lagra loggarna i alla lagringskonton där en NSG har aktiverats för flödesloggar. Både lagringskontot och på loggarna måste vara i samma prenumeration och region.

## <a name="what-if-i-cant-configure-an-nsg-for-traffic-analytics-due-to-a-not-found-error"></a>Vad händer om jag kan inte konfigurera en NSG för trafikanalys pga ett fel som ”hittades inte”?

Välj en region som stöds. Om du väljer ett område som inte stöds visas felmeddelandet ”hittades inte”. Regioner som stöds anges tidigare i den här artikeln.

## <a name="what-if-i-am-getting-the-status-failed-to-load-under-the-nsg-flow-logs-page"></a>Vad händer om jag får status, ”det gick inte att läsa in”, under sidan NSG flödet loggar?

Microsoft.Insights-providern måste vara registrerad för flow loggning för att fungera korrekt. Om du inte är säker på om Microsoft.Insights-providern är registrerad för din prenumeration ersätter *xxxxx-xxxxx-xxxxxx-xxxx* i följande kommando och kör följande kommandon från PowerShell:

```powershell-interactive
**Select-AzureRmSubscription** -SubscriptionId xxxxx-xxxxx-xxxxxx-xxxx
**Register-AzureRmResourceProvider** -ProviderNamespace Microsoft.Insights
```

## <a name="i-have-configured-the-solution-why-am-i-not-seeing-anything-on-the-dashboard"></a>Jag har konfigurerat lösningen. Varför ser jag inte något på instrumentpanelen?

Instrumentpanelen kan ta upp till 30 minuter att visas första gången. Lösningen måste först aggregera tillräckligt med data att dra användbara slutsatser. Sedan genererar den rapporter. 

## <a name="what-if-i-get-this-message-we-could-not-find-any-data-in-this-workspace-for-selected-time-interval-try-changing-the-time-interval-or-select-a-different-workspace"></a>Vad händer om jag får det här meddelandet: ”Vi kunde inte hitta några data i den här arbetsytan för valda tidsintervallet. Försök att ändra tidsintervallet eller välj en annan arbetsyta ”.?

Prova följande alternativ:
- Ändra tidsintervallet i det övre fältet.
- Välj en annan Log Analytics-arbetsyta i den övre stapeln.
- Försök att öppna trafikanalys efter 30 minuter om det nyligen har aktiverats.
    
Om problemet kvarstår kan du ställa frågor i den [User voice-forumet](https://feedback.azure.com/forums/217313-networking?category_id=195844).

## <a name="what-if-i-get-this-message-analyzing-your-nsg-flow-logs-for-the-first-time-this-process-may-take-20-30-minutes-to-complete-check-back-after-some-time-2-if-the-above-step-doesnt-work-and-your-workspace-is-under-the-free-sku-then-check-your-workspace-usage-here-to-validate-over-quota-else-refer-to-faqs-for-further-information"></a>Vad händer om jag får det här meddelandet: ”analyserar flödesloggarna för Nätverkssäkerhetsgrupper flow loggar för första gången. Den här processen kan ta 20 – 30 minuter att slutföra. Komma tillbaka efter en stund. 2) om ovanstående steg inte fungerar och din arbetsytan tillhör kostnadsfritt SKU: N, kontrollera din arbetsyta användningsdata här för att verifiera över kvot, eller Läs vanliga frågor och svar för ytterligare information ”.?

Du kanske ser det här meddelandet eftersom:
- Trafikanalys nyligen har aktiverats och kan inte ännu har sammanställd tillräckligt med data att dra användbara slutsatser.
- Du använder den kostnadsfria versionen av Log Analytics-arbetsytan och det överskrider kvotgränserna. Du kan behöva använda en arbetsyta med en större kapacitet.
    
Om problemet kvarstår kan du ställa frågor i den [User voice-forumet](https://feedback.azure.com/forums/217313-networking?category_id=195844).
    
## <a name="what-if-i-get-this-message-looks-like-we-have-resources-data-topology-and-no-flows-information-meanwhile-click-here-to-see-resources-data-and-refer-to-faqs-for-further-information"></a>Vad händer om jag får det här meddelandet: ”Det verkar finnas resursdata (topologi) men ingen information om flöden. Under tiden kan klicka här om du vill visa resursdata och referera till vanliga frågor och svar för ytterligare information ”.?

Du ser informationen om resurser på instrumentpanelen; men det finns ingen flow-statistik. Data kan inte finnas på grund av inga kommunikationsflöden mellan resurser. Vänta tills 60 minuter och kontrollera status. Om problemet kvarstår och du vet att det inte finns kommunikationsflöden mellan resurser, ställa frågor i den [User voice-forumet](https://feedback.azure.com/forums/217313-networking?category_id=195844).

## <a name="can-i-configure-traffic-analytics-using-powershell-or-an-azure-resource-manager-template-or-client"></a>Kan jag konfigurera trafikanalys med hjälp av PowerShell eller Azure Resource Manager-mall eller klient?

Du kan konfigurera trafikanalys med hjälp av Windows PowerShell från version 6.2.1 och senare. För att konfigurera flödesloggar och trafikanalys för en specifik NSG med hjälp av cmdleten Set, se [Set-AzureRmNetworkWatcherConfigFlowLog](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermnetworkwatcherconfigflowlog?view=azurermps-6.3.0). Om du vill hämta flödesloggar och status för trafikanalys för en specifik NSG, se [Get-AzureRmNetworkWatcherFlowLogStatus](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworkwatcherflowlogstatus?view=azurermps-6.3.0).

För närvarande kan använda du inte en Azure Resource Manager-mall för att konfigurera trafikanalys.

Se följande exempel för att konfigurera trafikanalys genom att använda en Azure Resource Manager-klienten.

**Ange cmdlet-exempel:**
```
#Requestbody parameters
$TAtargetUri ="/subscriptions/<NSG subscription id>/resourceGroups/<NSG resource group name>/providers/Microsoft.Network/networkSecurityGroups/<name of NSG>"
$TAstorageId = "/subscriptions/<storage subscription id>/resourcegroups/<storage resource group name> /providers/microsoft.storage/storageaccounts/<storage account name>"
$networkWatcherResourceGroupName = "<network watcher resource group name>"
$networkWatcherName = "<network watcher name>"

$requestBody = 
@"
{
    'targetResourceId': '${TAtargetUri}',
    'properties': 
    {
        'storageId': '${TAstorageId}',
        'enabled': '<true to enable flow log or false to disable flow log>',
        'retentionPolicy' : 
        {
            days: <enter number of days like to retail flow logs in storage account>,
            enabled: <true to enable retention or false to disable retention>
        }
    },
    'flowAnalyticsConfiguration':
    {
                'networkWatcherFlowAnalyticsConfiguration':
      {
        'enabled':,<true to enable traffic analytics or false to disable traffic analytics>
        'workspaceId':'bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb',
        'workspaceRegion':'<workspace region>',
        'workspaceResourceId':'/subscriptions/<workspace subscription id>/resourcegroups/<workspace resource group name>/providers/microsoft.operationalinsights/workspaces/<workspace name>'
        
      }

    }
}
"@
$apiversion = "2016-09-01"

armclient login
armclient post "https://management.azure.com/subscriptions/<NSG subscription id>/resourceGroups/<network watcher resource group name>/providers/Microsoft.Network/networkWatchers/<network watcher name>/configureFlowlog?api-version=${apiversion}" $requestBody
```
**Hämta cmdlet-exempel:**
```
#Requestbody parameters
$TAtargetUri ="/subscriptions/<NSG subscription id>/resourceGroups/<NSG resource group name>/providers/Microsoft.Network/networkSecurityGroups/<NSG name>"


$requestBody = 
@"
{
    'targetResourceId': '${TAtargetUri}'
}
“@


armclient login
armclient post "https://management.azure.com/subscriptions/<NSG subscription id>/resourceGroups/<network watcher resource group name>/providers/Microsoft.Network/networkWatchers/<network watcher name>/queryFlowLogStatus?api-version=${apiversion}" $requestBody
```



## <a name="how-is-traffic-analytics-priced"></a>Hur prissätts trafikanalys?

Trafikanalys mäts. Den Avläsning av programvara baseras på bearbetningen av loggdata för flödet av tjänsten och lagra den resulterande förbättrad loggar i Log Analytics-arbetsytan. 

Till exempel enligt den [prisavtal](https://azure.microsoft.com/pricing/details/network-watcher/), överväger USA, västra centrala regionen, om flödesloggarna data som lagras i ett lagringskonto som bearbetas av trafikanalys är 10 GB och förbättrad loggar som matas in i Log Analytics-arbetsyta är 1 GB används tillämpliga kostnader är: 10 x 2.3$ + 1 x 2.76$ = 25.76$

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-geo-map-view"></a>Hur kan navigera genom att använda tangentbordet i kartvyn geo?

Sidan geo kartan innehåller två huvudavsnitt:
    
- **Banderoll**: banderoll överst på geo-mappning innehåller knappar för att välja trafikfilter distribution (till exempel distribution, trafik från länder och skadliga). När du väljer en knapp, tillämpas respektive filtret på kartan. Om du väljer knappen Active visar kartan active datacenter i distributionen.
- **Kartan**: nedan på banderollen avsnittet kartan visar fördelning av trafik mellan Azure-datacenter och länder/regioner.
    
### <a name="keyboard-navigation-on-the-banner"></a>Tangentbordsnavigering i popup-meddelandet
    
- Som standard är val på sidan för geo-karta för popup-meddelandet ”Azure domänkontrollanter”.
- Om du vill flytta till ett annat filter kan du använda antingen den `Tab` eller `Right arrow` nyckel. Om du vill flytta bakåt kan du använda antingen den `Shift+Tab` eller `Left arrow` nyckel. Vidarebefordra navigering från vänster till höger, följt av längst upp och ned.
- Tryck på `Enter` eller `Down` pilknapp för att tillämpa det valda filtret. Baserat på filterval och distribution av är en eller flera noder under avsnittet kartan markerade.
- Om du vill växla mellan banderoll och kartan, trycker du på `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-map"></a>Tangentbordsnavigering på kartan
    
- När du har valt filter i popup-meddelandet och tryckt `Ctrl+F6`, fokus flyttas till en av de markerade noderna (**Azure-datacenter** eller **Land/Region**) i kartvyn.
- Om du vill flytta till andra noder som är markerade på kartan, kan du använda antingen `Tab` eller `Right arrow` nyckel för vidarebefordra flytt. Använd `Shift+Tab` eller `Left arrow` nyckel för bakåtkompatibilitet flytt.
- Välj vilken nod som helst markerade på kartan genom att använda den `Enter` eller `Down arrow` nyckel.
- På val av dessa noder fokus flyttas till den **Information verktygslådan** för noden. Som standard fokus flyttas till knappen stängda på den **Information verktygslådan**. Att flytta ytterligare i den **Box** visa, använda `Right arrow` och `Left arrow` för att flytta framåt och bakåt, respektive. Att trycka på `Enter` har samma effekt som att välja knappen fokus i den **Information verktygslådan**.
- När du trycker på `Tab` medan fokus ligger på den **Information verktygslådan**, fokus flyttas till slutpunkter i samma kontinent som den valda noden. Använd den `Right arrow` och `Left arrow` för att flytta via dessa slutpunkter.
- Om du vill flytta till andra flow-slutpunkter eller kontinenten kluster, använda `Tab` för vidarebefordra förflyttning och `Shift+Tab` för bakåtkompatibilitet flytt.
- När fokus ligger på **kontinent kluster**, använda den `Enter` eller `Down` för att markera slutpunkterna i det kontinent-klustret. Om du vill flytta via slutpunkter och på Stäng på informationsrutan i kontinenten klustret, kan du använda antingen den `Right arrow` eller `Left arrow` nyckel för framåt och bakåt förflyttning respektive. Du kan använda på valfri slutpunkt `Shift+L` att växla till raden anslutningen från den valda noden till slutpunkten. Du kan trycka på `Shift+L` igen för att flytta till den valda slutpunkten.
        
### <a name="keyboard-navigation-at-any-stage"></a>Tangentbordsnavigering under alla stadier
    
- `Esc` döljer det expanderade urvalet.
- Den `Up arrow` nyckel utför samma åtgärd som `Esc`. Den `Down arrow` nyckel utför samma åtgärd som `Enter`.
- Använd `Shift+Plus` att zooma in, och `Shift+Minus` att zooma ut.

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-virtual-network-topology-view"></a>Hur kan jag för att navigera med hjälp av tangentbordet i virtuellt nätverk topologiska vyn?

Sidan virtuellt nätverk topologin innehåller två avsnitt:
    
- **Banderoll**: banderoll överst på den virtuella nätverk-topologin innehåller knappar för att välja distribution trafikfilter (till exempel anslutna virtuella nätverk, frånkopplade virtuella nätverk och offentliga IP-adresser). När du väljer en knapp, tillämpas respektive filtret på topologin. Om du väljer knappen Active visar topologin de aktiva virtuella nätverken i distributionen.
- **Topologi**: nedan banderoll, i avsnittet om topologi visar fördelning av trafik mellan virtuella nätverk.
    
### <a name="keyboard-navigation-on-the-banner"></a>Tangentbordsnavigering i popup-meddelandet
    
- Som standard är val på sidan virtuellt nätverk topologi för popup-meddelandet ”anslutna virtuella nätverk”.
- Flytta till ett annat filter genom att använda den `Tab` för att flytta framåt. Flytta bakåt genom att använda den `Shift+Tab` nyckel. Vidarebefordra navigering från vänster till höger, följt av längst upp och ned.
- Tryck på `Enter` att tillämpa det valda filtret. Baserat på filterval och distribution av är en eller flera noder (virtuella nätverk) under avsnittet topologi markerade.
- Om du vill växla mellan popup-meddelandet och topologin, trycker du på `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-topology"></a>Tangentbordsnavigering på topologin
    
- När du har valt filter i popup-meddelandet och tryckt `Ctrl+F6`, fokus flyttas till en av de markerade noderna (**VNet**) i vyn topologi.
- Flytta till andra noder som är markerade i topologiska vyn genom att använda den `Shift+Right arrow` nyckel för vidarebefordra flytt. 
- På markerade noder fokus flyttas till den **Information verktygslådan** för noden. Som standard fokus flyttas till den **mer** knappen på den **Information verktygslådan**. Att flytta ytterligare i den **Box** visa, använda den `Right arrow` och `Left arrow` för att flytta framåt och bakåt, respektive. Att trycka på `Enter` har samma effekt som att välja knappen fokus i den **Information verktygslådan**.
- På val av dessa noder, du kan gå till alla dess anslutningar, en i taget genom att trycka på den `Shift+Left arrow` nyckel. Fokus flyttas till den **Information verktygslådan** anslutning. När som helst fokus kan flyttas tillbaka till noden genom att trycka på `Shift+Right arrow` igen.
    

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-subnet-topology-view"></a>Hur kan jag för att navigera med hjälp av tangentbordet i undernätet topologiska vyn?

Sidan virtuella undernätverk topologin innehåller två huvudavsnitt:
    
- **Banderoll**: banderoll överst på den virtuella undernätverk topologin innehåller knappar för att välja trafikfilter distribution (till exempel aktiv, medel och Gateway-undernät). När du väljer en knapp, tillämpas respektive filtret på topologin. Om du väljer knappen Active visar topologin active virtuellt undernätverk i distributionen.
- **Topologi**: nedan banderoll, i avsnittet om topologi visar fördelning av trafik mellan virtuella undernätverk.
    
### <a name="keyboard-navigation-on-the-banner"></a>Tangentbordsnavigering i popup-meddelandet
    
- Som standard är val på sidan virtuellt undernätverk topologi för popup-meddelandet ”undernät”-filter.
- Flytta till ett annat filter genom att använda den `Tab` för att flytta framåt. Flytta bakåt genom att använda den `Shift+Tab` nyckel. Vidarebefordra navigering från vänster till höger, följt av längst upp och ned.
- Tryck på `Enter` att tillämpa det valda filtret. Baserat på filterval och distribution av är en eller flera noder (undernät) under avsnittet topologi markerade.
- Om du vill växla mellan popup-meddelandet och topologin, trycker du på `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-topology"></a>Tangentbordsnavigering på topologin
    
- När du har valt filter i popup-meddelandet och tryckt `Ctrl+F6`, fokus flyttas till en av de markerade noderna (**undernät**) i vyn topologi.
- Flytta till andra noder som är markerade i topologiska vyn genom att använda den `Shift+Right arrow` nyckel för vidarebefordra flytt. 
- På markerade noder fokus flyttas till den **Information verktygslådan** för noden. Som standard fokus flyttas till den **mer** knappen på den **Information verktygslådan**. Att flytta ytterligare i den **Box** visa, använda `Right arrow` och `Left arrow` för att flytta framåt och bakåt, respektive. Att trycka på `Enter` har samma effekt som att välja knappen fokus i den **Information verktygslådan**.
- På val av dessa noder, du kan gå till alla dess anslutningar, en i taget genom att trycka på `Shift+Left arrow` nyckel. Fokus flyttas till den **Information verktygslådan** anslutning. När som helst fokus kan flyttas tillbaka till noden genom att trycka på `Shift+Right arrow` igen.    

