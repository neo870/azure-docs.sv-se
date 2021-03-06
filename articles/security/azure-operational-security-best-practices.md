---
title: Metodtips för Azure driftsäkerhet | Microsoft Docs
description: Den här artikeln innehåller en uppsättning Metodtips för driftsäkerhet för Azure.
services: security
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2018
ms.author: terrylan
ms.openlocfilehash: d005dd01de0dff0136c0a4e9775001dbe018228f
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47035291"
---
# <a name="azure-operational-security-best-practices"></a>Metodtips för Azure driftsäkerhet
Azure driftsäkerhet refererar till de tjänster, kontroller och funktioner som är tillgängliga för användare för att skydda sina data, program och andra resurser i Azure. Azure driftsäkerhet bygger på ett ramverk som innehåller den kunskap som funktioner som är unika för Microsoft, inklusive den [Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl), [Microsoft Security Response Center](https://www.microsoft.com/msrc?rtc=1) programmet och djup medvetenhet om hotlandskapet.

I den här artikeln diskuterar vi en uppsättning rekommenderade säkerhetsmetoder. Dessa metodtips härleds från vår erfarenhet med Azure database-säkerhet och erfarenheter från kunder som dig själv.

För varje rekommenderar förklarar vi:
-   Vad den bästa metoden är
-   Varför du vill aktivera den bästa praxis
-   Vad kan vara resultatet om du inte aktivera den bästa metoden
- Hur du kan lära dig att aktivera ett metodtips

Den här artikeln för Azure Operational säkerhetsmetoder baseras på en konsensus-åsikter och funktioner för Azure-plattformen och funktioner, eftersom de finns när den här artikeln skrevs. Andras åsikter och tekniker som ändras med tiden och den här artikeln kommer att uppdateras regelbundet att återspegla dessa ändringar.

## <a name="monitor-storage-services-for-unexpected-changes-in-behavior"></a>Övervaka lagringstjänster för oväntade ändringar i beteende
Diagnostisera och felsöka problem i ett distribuerat program i en molnmiljö kan vara mer komplex än i traditionella miljöer. Program kan distribueras i en PaaS eller IaaS-infrastruktur lokalt, på en mobil enhet eller i en kombination av dessa miljöer. Ditt programs trafik kan bläddra i offentliga och privata nätverk och ditt program kan använda flera lagringstekniker.

Kontinuerligt bör du övervaka de storage-tjänster som programmet använder för eventuella oväntade ändringar i beteende (till exempel längre svarstider). Använd loggning för att samla in mer detaljerade data och analysera ett problem i detalj. Diagnostikinformation som hämtas från övervakning och loggning hjälper dig att fastställa orsaken till problemet som påträffats i ditt program. Sedan kan du felsöka problemet och fastställa lämpliga åtgärder kan åtgärdas.

[Azure Storage Analytics](../storage/storage-analytics.md) utför loggning och tillhandahåller mätvärden för ett Azure storage-konto. Vi rekommenderar att du använder dessa data för att spåra förfrågningar, analysera användningstrender och diagnostisera problem med ditt lagringskonto.

## <a name="prevent-detect-and-respond-to-threats"></a>Förhindra, upptäcka och svara på hot
[Azure Security Center](../security-center/security-center-intro.md) hjälper dig att förhindra, upptäcka och svara på hot med ökad insyn i (och kontroll över) säkerheten för dina Azure-resurser. Det ger integrerad säkerhet övervaka och hantera principer för alla Azure-prenumerationer, upptäcka hot som kan annars oupptäckta och fungerar med olika säkerhetslösningar.

Security Centers kostnadsfria nivån ger begränsad säkerhet för dina Azure-resurser endast. Standard-nivån utökar funktionerna till lokala och andra moln. Med Security Center Standard kan du hitta och åtgärda säkerhetsproblem, tillämpa åtkomst- och programkontroller för att blockera skadlig aktivitet, upptäcka hot med analys och intelligens och svara snabbt under attacker. Du kan prova Security Center Standard utan kostnad under de första 60 dagarna. Vi rekommenderar att du [publicera din Azure-prenumeration till Security Center Standard](../security-center/security-center-get-started.md).

Du kan använda Security Center för att få en central vy över säkerhetsläget hos alla dina Azure-resurser. Kontrollera att rätt säkerhetskontroller är på plats och korrekt konfigurerad och snabbt identifiera eventuella resurser som behöver åtgärdas på ett ögonblick.

## <a name="monitor-end-to-end-scenario-based-network-monitoring"></a>Övervaka slutpunkt till slutpunkt scenariobaserade nätverksövervakning
Kunder skapar en slutpunkt till slutpunkt-nätverk i Azure genom att kombinera nätverksresurser som ett virtuellt nätverk, ExpressRoute, Application Gateway och belastningsutjämnare. Övervakning är tillgängliga på var och en av nätverksresurserna.

[Azure Network Watcher](../network-watcher/network-watcher-monitoring-overview.md) är en regional tjänst. Använd dess verktyg för diagnostisering och visualisering för att övervaka och diagnostisera villkor på nätverksscenarienivå i, till och från Azure.

Här följer bästa praxis för verktyg för övervakning och tillgänglig.

**Bästa praxis**: automatisera övervakning av fjärrnätverk med infångade paket.   
**Information om**: övervaka och diagnostisera nätverksproblem utan att logga in till dina virtuella datorer med hjälp av Network Watcher. Utlösaren [paketfångsten](../network-watcher/network-watcher-alert-triggered-packet-capture.md) genom att konfigurera aviseringar och få åtkomst till prestandainformation i realtid på paketnivå. När du ser ett problem kan du undersöka i detalj för att få bättre diagnoser.

**Bästa praxis**: få insikt i din nätverkstrafik med flödesloggar.   
**Information om**: skapa trafikmönster för en djupare förståelse för nätverket med hjälp av [network flödesloggar för nätverkssäkerhetsgruppen](../network-watcher/network-watcher-nsg-flow-logging-overview.md). Informationen i flödesloggar kan du samla in data för efterlevnad, granskning och övervakning av din nätverkssäkerhetsprofil.

**Bästa praxis**: diagnostisera VPN-anslutningsproblem.   
**Information om**: Använd Network Watcher till [diagnostisera dina vanligaste problem för VPN-Gateway och anslutning](../network-watcher/network-watcher-diagnose-on-premises-connectivity.md). Du kan inte bara identifiera problemet, men också använda detaljerade loggar för att undersöka.

## <a name="secure-deployment-by-using-proven-devops-tools"></a>Skydda distributionen genom att använda beprövade DevOps-verktyg
Använd följande säkerhetsmetoder för DevOps för att säkerställa att ditt företag och team är produktiva och effektiv.

**Bästa praxis**: automatisera skapande och distribution av tjänster.   
**Information om**: [infrastruktur som kod](https://en.wikipedia.org/wiki/Infrastructure_as_Code) är en uppsättning tekniker och metoder som hjälper IT-proffs kan du ta bort arbetet med dagliga build och hantering av modulära infrastrukturen. Det gör att IT-proffs att skapa och underhålla sina moderna server-miljö på ett sätt som liknar hur programvaruutvecklare att bygga och underhålla programkod.

Du kan använda [Azure Resource Manager](https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/) etablera dina program med hjälp av en deklarativ mall. I samma mall kan du distribuera flera tjänster tillsammans med deras beroenden. Du kan använda samma mall för att upprepade gånger distribuera ditt program i varje fas av programmets livscykel.

**Bästa praxis**: automatiskt skapa och distribuera till Azure-webbappar eller molntjänster.   
**Information om**: du kan konfigurera din teamprojekt i Visual Studio Team Services (VSTS) till [automatiskt skapa och distribuera](https://www.visualstudio.com/docs/build/overview) till Azure-webbappar eller molntjänster. VSTS distribuerar automatiskt binärfilerna när du har gjort en version till Azure efter varje checkar in ny kod. Paketet skapandeprocessen motsvarar kommandot paketet i Visual Studio och publicera stegen är likvärdiga med kommandot Publicera i Visual Studio.

**Bästa praxis**: automatisera release management.   
**Information om**: Visual Studio [Release Management](https://msdn.microsoft.com/library/vs/alm/release/overview) är en lösning för att automatisera distribution av flera steg och hantera lanseringsprocessen. Skapa hanterade kontinuerlig distribution pipelines Frigör snabbt, enkelt och ofta. Du kan automatisera processer versionen med Release Management, och du kan har fördefinierade godkännandearbetsflöden. Distribuera lokalt till molnet, utöka och anpassa efter behov.

**Bästa praxis**: Kontrollera din Apps prestanda innan du startar den eller distribuera uppdateringar till produktion.   
**Information om**: kör molnbaserade [belastningstester](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing) med VSTS till:

- Hitta prestandaproblem i din app.
- Förbättra kvaliteten i distributionen.
- Se till att dina appar alltid är tillgängliga.
- Se till att din app kan hantera trafik för nästa start eller marknadsföring kampanjen.

**Bästa praxis**: övervaka programmets prestanda.   
**Information om**: [Azure Application Insights](../application-insights/app-insights-overview.md) är en utökningsbar (APM) Hanteringstjänst för programprestanda för webbutvecklare på flera plattformar. Du kan använda Application Insights för att övervaka ditt webbprogram. Den identifierar automatiskt prestandaavvikelser. Den innehåller analysverktyg för att hjälpa dig att diagnostisera problem och förstå vad användare faktiskt gör med din app. Den är avsedd för utvecklare och för att hjälpa dig att kontinuerligt förbättra prestanda och användbarhet.

## <a name="mitigate-and-protect-against-ddos"></a>Minimera och skydda mot DDoS
Distribuerade överbelastningsattacker (DDoS) är en typ av angrepp som används för att få slut på resurser. Målet är att påverka programmets tillgänglighet och dess förmåga att hantera legitima begäranden. Dessa attacker blir allt mer sofistikerade och större i storlek och effekt. De kan riktas till valfri slutpunkt som kan nås offentligt via internet.

Utforma och skapa DDoS-skydd kräver planering och design för en mängd olika fellägen.

Följande är rekommenderade metoder för att skapa DDoS anpassningsbara tjänster i Azure.

**Bästa praxis**: se till att säkerhet är en prioritet under hela livscykeln för ett program, från design och implementering till distribution och drift. Program kan ha buggar som gör att en relativt låg volym av begäranden om att använda en massa resurser, vilket resulterar i avbrott i tjänsten.    
**Information om**: för att skydda en tjänst som körs på Microsoft Azure, bör du ha en god förståelse av programarkitekturen och fokusera på den [fem grundpelare för programkvalitet](https://docs.microsoft.com/azure/architecture/guide/pillars). Du bör känna till vanliga trafikvolymer modellen anslutning mellan programmet och andra program och Tjänsteslutpunkter som exponeras för det offentliga internet.

Se till att ett program är tillräckligt flexibel för att hantera DOS-attacker som riktas mot själva programmet är viktigast. Säkerhet och sekretess är inbyggt i Azure-plattformen, från och med den [Security Development Lifecycle (SDL)](https://www.microsoft.com/en-us/sdl). SDL-processen hanterar säkerheten i varje utvecklingsfas och ser till att Azure uppdateras ständigt för att göra det ännu säkrare.

**Bästa praxis**: programmen utformas för [skala horisontellt](https://docs.microsoft.com/azure/architecture/guide/design-principles/scale-out) att uppfylla behovet av en förstärkt belastning, särskilt i händelse av en DDoS-attack. Om ditt program är beroende av en enda instans av en tjänst, skapas en enskild felpunkt. Etablering av flera instanser kan systemet mer skalbart och återhämtningsbart mer.   
**Information om**: för [Azure App Service](../app-service/app-service-value-prop-what-is.md)väljer en [App Service-plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) som erbjuder flera instanser.

Konfigurera var och en av dina roller att använda för Azure Cloud Services, [flera instanser](../cloud-services/cloud-services-choose-me.md).

För [Azure Virtual Machines](../virtual-machines/windows/overview.md), se till att din VM-arkitektur innehåller fler än en virtuell dator och att varje virtuell dator ingår i en [tillgänglighetsuppsättning](../virtual-machines/virtual-machines-windows-manage-availability.md). Vi rekommenderar att du använder virtuella skalningsuppsättningar för funktioner för automatisk skalning.

**Bästa praxis**: Skikta säkerhetsskyddet i ett program minskar risken för angrepp. Implementera säkra utformning för dina program med hjälp av de inbyggda funktionerna i Azure-plattformen.   
**Information om**: risken för angrepp ökar i storlek (ytan) av programmet. Du kan minska ytan med hjälp av listan över tillåtna program ska stänga exponerade IP-adressutrymmet och lyssningsportar som inte behövs på belastningsutjämnarna ([Azure Load Balancer](../load-balancer/load-balancer-get-started-internet-portal.md) och [Azure Application Gateway](../application-gateway/application-gateway-create-probe-portal.md)).

[Nätverkssäkerhetsgrupper](../virtual-network/security-overview.md) är ett annat sätt att minska risken för angrepp. Du kan använda [tjänsttaggar](../virtual-network/security-overview.md#service-tags) och [programsäkerhetsgrupper](../virtual-network/security-overview.md#application-security-groups) att minska komplexiteten för att skapa säkerhetsregler och konfigurera nätverkssäkerhet, som en naturlig förlängning av ett programs struktur.

Du bör distribuera Azure-tjänster i en [virtuellt nätverk](../virtual-network/virtual-networks-overview.md) när det är möjligt. Detta gör att service-resurser kommunicera via privata IP-adresser. Azure-tjänsttrafiken från ett virtuellt nätverk använder offentliga IP-adresser som käll-IP-adresser som standard.

Med hjälp av [tjänstslutpunkter](../virtual-network/virtual-network-service-endpoints-overview.md) växlar tjänsten trafik för att använda privata vnet-adresser som käll-IP-adresser när de får åtkomst till Azure-tjänsten från ett virtuellt nätverk.

Ofta ser vi kundernas lokala resurser komma angripna tillsammans med sina resurser i Azure. Om du ansluter en lokal miljö till Azure, minimera exponering av lokala resurser till det offentliga internet.

Azure har två DDoS [Tjänsterbjudanden](../virtual-network/ddos-protection-overview.md) som ger skydd mot nätverksattacker:

- Grundläggande skydd är integrerad i Azure som standard utan extra kostnad. Ger skydd mot vanliga attacker på nätverkslager via ständig övervakning och i realtid minskning skalbarhet och kapacitet för globalt distribuerade Azure-nätverket. Basic kräver konfiguration eller programmet användarändringar och hjälper dig att skydda alla Azure-tjänster, inklusive PaaS-tjänster som Azure DNS.
- Standard protection tillhandahåller avancerade DDoS-skyddsfunktioner mot nätverksattacker. Det är automatiskt justerade för att skydda din specifika Azure-resurser. Skydd är enkelt att använda under genereringen av virtuella nätverk. Det kan också göras när du har skapat och kräver inga ändringar i programmet eller resursen.

## <a name="next-steps"></a>Nästa steg
Se [säkerhet i Azure-metodtips och mönster](security-best-practices-and-patterns.md) för flera beprövade metoder för att använda när du utforma, distribuera och hantera dina molnlösningar med hjälp av Azure.

Följande resurser är tillgängliga för att tillhandahålla mer allmän information om Azure-säkerhet och relaterade Microsoft-tjänster:
* [Azure-Säkerhetsteamets blogg](https://blogs.msdn.microsoft.com/azuresecurity/) – uppdaterad information på senast inom Azure-säkerhet
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx) – där kan du rapportera säkerhetsproblem i Microsoft, inklusive problem med Azure, eller mejla till secure@microsoft.com
