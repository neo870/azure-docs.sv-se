---
title: Azure Site Recovery-supportmatrisen för haveriberedskap för virtuella Azure IaaS-datorer mellan Azure-regioner med Azure Site Recovery | Microsoft Docs
description: Sammanfattning av de operativsystem som stöds och konfigurationer för Azure Site Recovery-replikering av virtuella Azure-datorer (VM) från en region till en annan för disaster recovery (DR) behov.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 10/28/2018
ms.author: raynew
ms.openlocfilehash: 5cce3005a0058604136e05d9c3bf9700d5296bf3
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50964089"
---
# <a name="support-matrix-for-replicating-from-one-azure-region-to-another"></a>Stöd matrix för replikering från en Azure-region till en annan

Den här artikeln sammanfattas konfigurationer som stöds och komponenter när du distribuerar haveriberedskap med replikering, redundans och återställning av virtuella Azure-datorer från en Azure-region till en annan, med hjälp av den [Azure Site Recovery](site-recovery-overview.md) service.


## <a name="deployment-method-support"></a>Metoden distributionsstöd

**Distributionsmetod** |  **Stöds / stöds inte**
--- | ---
**Azure Portal** | Stöds
**PowerShell** | [Replikering från Azure till Azure med PowerShell](azure-to-azure-powershell.md)
**REST-API** | Stöds för närvarande inte
**CLI** | Stöds för närvarande inte


## <a name="resource-support"></a>Resursstöd för

**Resursåtgärden** | **Detaljer**
--- | --- | ---
**Flytta valv mellan resursgrupper** | Stöds inte
**Flytta resurser för beräkning/lagringsnätverk mellan resursgrupper** | Stöds ej.<br/><br/> Om du flyttar en virtuell dator eller tillhörande komponenter, till exempel lagringsnätverk/när Virtuellt datorn replikeras, måste du inaktivera och återaktivera replikering för den virtuella datorn.
**Replikera virtuella Azure-datorer från en prenumeration till en annan för katastrofåterställning** | Stöd för inom samma Azure Active Directory-klientorganisation.
**Migrera virtuella datorer mellan regioner i stöds geografiska kluster (inom och mellan prenumerationer)** | Stöd för inom samma Azure Active Directory-klientorganisation.
**Migrera virtuella datorer i samma region** | Stöds ej.

# <a name="region-support"></a>Regionsstöd

Du kan replikera och återställa virtuella datorer mellan alla två regioner inom samma geografiska kluster.

**Geografisk kluster** | **Azure-regioner**
-- | --
Sydamerika | Kanada, östra, Kanada, centrala, södra centrala USA, USA, västra centrala, USA, östra, USA, östra 2, västra USA, västra USA 2, centrala USA, norra centrala USA
Europa | Storbritannien, västra, Storbritannien, södra, Nordeuropa, Västeuropa, Frankrike, centrala och Frankrike, södra
Asien | Södra Indien, centrala Indien, Sydostasien, östra Asien, östra, Japan, Japan västra, centrala Korea, södra
Australien   | Östra Australien, sydöstra Australien, Australien centrala; Australien centrala 2
Azure Government    | Virginia (USA-förvaltad region), Iowa (USA-förvaltad region), USA-förvaltad region Arizona, US GOV Texas, US DOD, östra, US DOD, centrala
Tyskland | Tyskland, centrala, Tyskland, nordöstra
Kina | Östra Kina, norra Kina

>[!NOTE]
>
> För regionen södra Brasilien, kan du replikera och redundansväxla till något av följande: södra centrala USA, västra centrala USA, östra USA, östra USA 2, västra USA, västra USA 2 och USA, norra centrala regioner.

## <a name="cache-storage"></a>Cachelagring

Den här tabellen sammanfattar stödet för cachelagringskonto som används av Site Recovery under replikeringen.

**Inställning** | **Detaljer**
--- | ---
Generell användning V2-lagringskonton (frekvent och lågfrekvent nivå) | Stöds ej. | Begränsningen finns för cachelagring eftersom transaktionskostnader för V2 är betydligt högre än V1-lagringskonton.
Azure Storage-brandväggar för virtuella nätverk  | Nej | Att tillåta åtkomst till specifika Azure-nätverk på cachelagringskonton som används för att lagra replikerade data stöds inte.



## <a name="replicated-machine-operating-systems"></a>Replikerade datorn operativsystem

Site Recovery har stöd för replikering av virtuella Azure-datorer som kör operativsystemen som anges i det här avsnittet.

### <a name="windows"></a>Windows

**Operativsystem** | **Detaljer**
--- | ---
Windows Server 2016  | Server Core, Server med Skrivbordsmiljö
Windows Server 2012 R2 |
Windows Server 2012 |
Windows Server 2008 R2 | Med SP1 eller senare

#### <a name="linux"></a>Linux

**Operativsystem** | **Detaljer**
--- | ---
Red Hat Enterprise Linux | 6.7, 6.8, 6,9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4 och 7.5   
CentOS | 6.5, 6.6, 6.7, 6.8, 6,9, 6.10, 7.0, 7.1, 7.2, 7.3,7.4, 7.5
Ubuntu 14.04 LTS Server | [Stöds kernel-versioner](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
Ubuntu 16.04 LTS Server | [Stöds kernel-version](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)<br/><br/> Ubuntu-servrar som använder lösenordsbaserad autentisering och logga in och cloud-init-paketet för att konfigurera virtuella datorer, kan ha lösenordsbaserad inloggning inaktiverad på redundans (beroende på cloudinit-konfiguration). Lösenordsbaserad inloggning kan återaktiveras på den virtuella datorn genom att återställa lösenordet från stödet > Felsökning > menyn Inställningar (på den redundansväxlade virtuella datorn i Azure-portalen.
Debian 7 | [Stöds kernel-versioner](#supported-debian-kernel-versions-for-azure-virtual-machines)
Debian 8 | [Stöds kernel-versioner)](#supported-debian-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 12 | SP1, SP2, SP3. [Stöds kernel-versioner](#supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 11 | SP3<br/><br/> Uppgradering av datorer som replikeras från SP3 till SP4 stöds inte. Om en replikerad dator har uppgraderats, måste du inaktivera replikering och återaktivera replikering efter uppgraderingen.
SUSE Linux Enterprise Server 11 | SP4
Oracle Linux | 6.4, 6.5, 6.6, 6.7<br/><br/> Köra antingen kompatibel Red Hat-kernel eller Unbreakable Enterprise Kernel version 3 (UEK3).


#### <a name="supported-ubuntu-kernel-versions-for-azure-virtual-machines"></a>Ubuntu kernel-versioner som stöds för Azure-datorer

**Versionen** | **Mobilitetstjänstversionen** | **Kernelversion** |
--- | --- | --- |
14.04 LTS | 9.19 | 3.13.0-24-Generic till 3.13.0-153-generic,<br/>3.16.0-25-Generic till 3.16.0-77-generic,<br/>3.19.0-18-Generic till 3.19.0-80-generic,<br/>4.2.0-18-Generic till 4.2.0-42-generic,<br/>4.4.0-21-Generic till 4.4.0-131-generic |
14.04 LTS | 9.18 | 3.13.0-24-Generic till 3.13.0-151-generic,<br/>3.16.0-25-Generic till 3.16.0-77-generic,<br/>3.19.0-18-Generic till 3.19.0-80-generic,<br/>4.2.0-18-Generic till 4.2.0-42-generic,<br/>4.4.0-21-Generic till 4.4.0-128-generic |
14.04 LTS | 9.17 | 3.13.0-24-Generic till 3.13.0-147-generic,<br/>3.16.0-25-Generic till 3.16.0-77-generic,<br/>3.19.0-18-Generic till 3.19.0-80-generic,<br/>4.2.0-18-Generic till 4.2.0-42-generic,<br/>4.4.0-21-Generic till 4.4.0-124-generic |
14.04 LTS | 9.16 | 3.13.0-24-Generic till 3.13.0-144-generic,<br/>3.16.0-25-Generic till 3.16.0-77-generic,<br/>3.19.0-18-Generic till 3.19.0-80-generic,<br/>4.2.0-18-Generic till 4.2.0-42-generic,<br/>4.4.0-21-Generic till 4.4.0-119-generic |
|||
16.04 LTS | 9.19 | 4.4.0-21-Generic till 4.4.0-131-generic,<br/>4.8.0-34-Generic till 4.8.0-58-generic,<br/>4.10.0-14-Generic till 4.10.0-42-generic,<br/>4.11.0-13-Generic till 4.11.0-14-generic,<br/>4.13.0-16-Generic till 4.13.0-45-generic,<br/>4.15.0-13-Generic till 4.15.0-30-generic<br/>4.11.0-1009-Azure till 4.11.0-1016-azure,<br/>4.13.0-1005-Azure till 4.13.0-1018-azure <br/>4.15.0-1012-Azure till 4.15.0-1019-azure|
16.04 LTS | 9.18 | 4.4.0-21-Generic till 4.4.0-128-generic,<br/>4.8.0-34-Generic till 4.8.0-58-generic,<br/>4.10.0-14-Generic till 4.10.0-42-generic,<br/>4.11.0-13-Generic till 4.11.0-14-generic,<br/>4.13.0-16-Generic till 4.13.0-45-generic,<br/>4.11.0-1009-Azure till 4.11.0-1016-azure,<br/>4.13.0-1005-Azure till 4.13.0-1018-azure |
16.04 LTS | 9.17 | 4.4.0-21-Generic till 4.4.0-124-generic,<br/>4.8.0-34-Generic till 4.8.0-58-generic,<br/>4.10.0-14-Generic till 4.10.0-42-generic,<br/>4.11.0-13-Generic till 4.11.0-14-generic,<br/>4.13.0-16-Generic till 4.13.0-41-generic,<br/>4.11.0-1009-Azure till 4.11.0-1016-azure,<br/>4.13.0-1005-Azure till 4.13.0-1016-azure |
16.04 LTS | 9.16 | 4.4.0-21-Generic till 4.4.0-119-generic,<br/>4.8.0-34-Generic till 4.8.0-58-generic,<br/>4.10.0-14-Generic till 4.10.0-42-generic,<br/>4.11.0-13-Generic till 4.11.0-14-generic,<br/>4.13.0-16-Generic till 4.13.0-38-generic,<br/>4.11.0-1009-Azure till 4.11.0-1016-azure,<br/>4.13.0-1005-Azure till 4.13.0-1012-azure |


#### <a name="supported-debian-kernel-versions-for-azure-virtual-machines"></a>Debian kernel-versioner som stöds för virtuella Azure-datorer

**Versionen** | **Mobilitetstjänstversionen** | **Kernelversion** |
--- | --- | --- |
Debian 7 | 9.17,9.18,9.19 | 3.2.0-4-amd64 till 3.2.0-6-amd64, 3.16.0-0.bpo.4-amd64 |
Debian 7 | 9.16 | 3.2.0-4-amd64 till 3.2.0-5-amd64, 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | 9.19 | 3.16.0-4-amd64 till 3.16.0-6-amd64 4.9.0-0.bpo.4-amd64 till 4.9.0-0.bpo.7-amd64 |
Debian 8 | 9.17, 9.18 | 3.16.0-4-amd64 till 3.16.0-6-amd64 4.9.0-0.bpo.4-amd64 till 4.9.0-0.bpo.6-amd64 |
Debian 8 | 9.16 | 3.16.0-4-amd64 till 3.16.0-5-amd64 4.9.0-0.bpo.4-amd64 till 4.9.0-0.bpo.5-amd64 |

#### <a name="supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines"></a>SUSE Linux Enterprise Server 12 kernel-versioner som stöds för Azure-datorer

**Versionen** | **Mobilitetstjänstversionen** | **Kernelversion** |
--- | --- | --- |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3) | 9.19 | SP1 3.12.49-11-default till 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default till 3.12.74-60.64.93-default</br></br> SP2 4.4.21-69-default till 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default till 4.4.121-92.80-default</br></br>SP3 4.4.73-5-default till 4.4.140-94.42-default |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3) | 9.18 | SP1 3.12.49-11-default till 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default till 3.12.74-60.64.93-default</br></br> SP2 4.4.21-69-default till 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default till 4.4.121-92.80-default</br></br>SP3 4.4.73-5-default till 4.4.138-94.39-default |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3) | 9.17 | SP1 3.12.49-11-default till 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default till 3.12.74-60.64.88-default</br></br> SP2 4.4.21-69-default till 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default</br></br>SP3 4.4.73-5-default till 4.4.126-94.22-default |

## <a name="replicated-machines---linux-file-systemguest-storage"></a>Replikerade datorer - Linux system/gäst fillagring

* Filsystem: ext3 ext4, ReiserFS (Suse Linux Enterprise Server endast), XFS
* Volymhanterare: LVM2
* Programvaran MultiPath: enheten Mapper


## <a name="replicated-machines---compute-settings"></a>Replikerade datorer - inställningarna för beräkning

**Inställning** | **Support** | **Detaljer**
--- | --- | ---
Storlek | Alla Azure VM-storlekar med minst 2 CPU-kärnor och 1 GB RAM-minne | Kontrollera [Azure VM-storlekar](../virtual-machines/windows/sizes.md).
Tillgänglighetsuppsättningar | Stöds | Om du aktiverar replikering för en virtuell Azure-dator med standardalternativen skapas en tillgänglighetsuppsättning automatiskt baserat på de nationella inställningarna för källan. Du kan ändra dessa inställningar.
Tillgänglighetszoner | Stöds inte | Du kan inte för närvarande replikera virtuella datorer som distribueras i tillgänglighetszoner.
Hybrid Use-förmånen (HUB) | Stöds | Om den Virtuella källdatorn har en HUB licens aktiverad, ett redundanstest eller redundans använder virtuell dator också HUB-licens.
Skalningsuppsättning för virtuella datorer | Stöds inte |
Azure galleriavbildningar - Microsoft publicerat | Stöds | Om den virtuella datorn kör ett operativsystem som stöds.
Azure-galleriet-avbildningar – från tredje part publicerats | Stöds | Om den virtuella datorn kör ett operativsystem som stöds.
Anpassade avbildningar - från tredje part publicerats | Stöds | Om den virtuella datorn kör ett operativsystem som stöds.
Virtuella datorer migreras med hjälp av Site Recovery | Stöds | Om en VMware-VM eller fysiska datorn har migrerats till Azure med Site Recovery, måste du avinstallera den äldre versionen av mobilitetstjänsten som körs på datorn och starta om datorn innan du replikerar till en annan Azure-region.

## <a name="replicated-machines---disk-actions"></a>Replikerade datorer - disk åtgärder

**Åtgärd** | **Detaljer**
-- | ---
Ändra storlek på disk på den replikerade virtuella datorn | Stöds
Lägg till en disk till en replikerad virtuell dator | Stöds ej.<br/><br/> Du måste inaktivera replikering för den virtuella datorn, Lägg till disk och aktiverar sedan replikeringen igen.

## <a name="replicated-machines---storage"></a>Replikerade datorer - lagring

Den här tabellen sammanfattas stöd för Azure VM OS-disk, datadisk och tillfällig disk.

- Det är viktigt att Observera diskgränser för virtuella datorer och mål för [Linux](../virtual-machines/linux/disk-scalability-targets.md) och [Windows](../virtual-machines/windows/disk-scalability-targets.md) virtuella datorer för att undvika eventuella prestandaproblem.
- Om du distribuerar med standardinställningarna, skapar Site Recovery automatiskt diskar och lagringskonton baserat på inställningar för datakälla.
- Om du har ändrat kan du kontrollera att du följer riktlinjerna.

**Komponent** | **Support** | **Detaljer**
--- | --- | ---
Maximal storlek för OS-disk | 2 048 GB | [Läs mer](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms) om VM-diskar.
Temporär disk | Stöds inte | Den temporära disken utelämnas alltid från replikering.<br/><br/> Inte alla beständiga data på den temporära disken. [Läs mer](../virtual-machines/windows/about-disks-and-vhds.md#temporary-disk).
Maximal Datastorlek för disk | 4 095 GB |
Maxantalet för data-disk | Upp till 64 i enlighet med stöd för en viss Azure VM-storlek | [Läs mer](../virtual-machines/windows/sizes.md) om VM-storlekar.
Förändringstakten för data-disk | Högst 10 Mbit/s per disk för premiumlagring. Högst 2 Mbit/s per disk för standardlagring. | Om de genomsnittliga dataändringshastighet på disken är kontinuerligt högre än tillåtna, replikering ifatt inte.<br/><br/>  Om längsta har överskridits sporadiskt replikering kan komma ifatt, men du kan se något fördröjda återställningspunkter.
Datadisk - standardlagringskonto | Stöds |
Datadisk - premium storage-konto | Stöds | Om en virtuell dator har diskar som är fördelade på premium- och standardlagringskonton, kan du välja ett annat mållagringskonto för varje disk, så du får samma lagringskonfiguration för i målregionen.
Hanterad disk - standard | Stöd i Azure-regioner där Azure Site Recovery stöds. |  
Hanterad disk - premium | Stöd i Azure-regioner där Azure Site Recovery stöds. |
Redundans | LRS och GRS stöds.<br/><br/> ZRS stöds inte.
Frekventa och lågfrekventa lagring | Stöds inte | VM-diskar stöds inte på frekventa och lågfrekventa lagring
Lagringsutrymmen | Stöds |         
Kryptering i vila (SSE) | Stöds | SSE är standardinställningen på storage-konton.   
Azure Disk Encryption (ADE) för Windows OS | Virtuella datorer som har aktiverats för [kryptering med Azure AD-app](https://aka.ms/ade-aad-app) stöds |
Azure Disk Encryption (ADE) för Linux OS | Stöds inte |
Frekvent Lägg till/ta bort disk | Stöds inte | Om du lägger till eller ta bort datadisk på den virtuella datorn, måste du inaktivera replikering och aktiverar replikering igen för den virtuella datorn.
Uteslut disk | Stöds inte|   Temporär disk är undantagen som standard.
Lagringsdirigering  | Stöds inte|
Skalbar filserver  | Stöds inte|
LRS | Stöds |
GRS | Stöds |
RA-GRS | Stöds |
ZRS | Stöds inte |  
Frekventa och lågfrekventa lagring | Stöds inte | Virtuella diskar stöds inte på frekventa och lågfrekventa lagring
Azure Storage-brandväggar för virtuella nätverk  | Ja | Om du är att begränsa åtkomst till virtuellt nätverk till storage-konton, kontrollerar du att betrodda Microsoft-tjänster har åtkomst till lagringskontot.
Storage-konton för generell användning V2 (både frekvent och lågfrekvent nivå) | Nej | Transaktionen kostnaderna ökar avsevärt jämfört med generell användning V1-lagringskonton

>[!IMPORTANT]
> Se till att du ser Virtuella disken skalbarhets- och prestandamål för [Linux](../virtual-machines/linux/disk-scalability-targets.md) eller [Windows](../virtual-machines/windows/disk-scalability-targets.md) virtuella datorer för att undvika eventuella prestandaproblem. Om du följer standardinställningarna skapar Site Recovery begärda diskar och lagringskonton baserat på käll-konfigurationen. Om du anpassar och välja egna inställningar, kontrollerar du att du följer disk-mål för skalbarhet och prestanda för dina virtuella källdatorer.

## <a name="replicated-machines---networking"></a>Replikerade datorer - nätverk
**Konfiguration** | **Support** | **Detaljer**
--- | --- | ---
Nätverkskort | Högsta antal som stöds för en viss Azure VM-storlek | Nätverkskort skapas när den virtuella datorn skapas under en redundansväxling.<br/><br/> Antal nätverkskort på den virtuella datorn som redundansväxlingen beror på antalet nätverkskort på den Virtuella källdatorn när replikering har aktiverats. Om du lägger till eller ta bort ett nätverkskort efter att ha aktiverat replikering, kan den inte påverkar antalet nätverkskort på den replikerade virtuella datorn efter redundans.
Internet-lastbalanserare | Stöds | Associera förkonfigurerade belastningsutjämnaren med hjälp av en Azure Automation-skript i en återställningsplan.
Den interna belastningsutjämnaren | Stöds | Associera förkonfigurerade belastningsutjämnaren med hjälp av en Azure Automation-skript i en återställningsplan.
Offentlig IP-adress | Stöds | Koppla en befintlig offentlig IP-adress till nätverkskortet. Eller skapa en offentlig IP-adress och associera den med nätverkskortet med hjälp av en Azure Automation-skript i en återställningsplan.
NSG på nätverkskortet | Stöds | Associera NSG med nätverkskortet med hjälp av en Azure Automation-skript i en återställningsplan.  
NSG på undernätet | Stöds | Koppla NSG: N med undernätet med hjälp av en Azure Automation-skript i en återställningsplan.
Reserverad (statiska) IP-adress | Stöds | Om nätverkskortet på den Virtuella källdatorn har en statisk IP-adress och målundernätet har den samma IP-adressen som är tillgängliga, den är tilldelad till den redundansväxlade virtuella datorn.<br/><br/> Om målundernätet inte har den samma IP-adressen som är tillgängliga, är en av de tillgängliga IP-adresserna i undernätet reserverad för den virtuella datorn.<br/><br/> Du kan också ange en fast IP-adress och nätmask i **replikerade objekt** > **inställningar** > **beräkning och nätverk**  >  **Nätverksgränssnitt**.
Dynamisk IP-adress | Stöds | Om nätverkskortet på källan har dynamiska IP-adresser, är nätverkskortet på den redundansväxlade virtuella datorn också dynamiskt som standard.<br/><br/> Du kan ändra detta till en fast IP-adress om det behövs.
Traffic Manager     | Stöds | Du kan förkonfigurera Traffic Manager så att trafiken dirigeras till slutpunkten i källregionen regelbundet och till slutpunkten i målregionen vid redundans.
Azure DNS | Stöds |
Anpassad DNS  | Stöds |    
Via oautentiserad proxyserver | Stöds | Referera till [nätverk vägledningsdokumentet.](site-recovery-azure-to-azure-networking-guidance.md)    
Autentiserad Proxy | Stöds inte | Om den virtuella datorn använder en autentiserad proxyserver för utgående anslutningar, kan inte replikeras med Azure Site Recovery.    
VPN för plats till plats med en lokal (med eller utan ExpressRoute)| Stöds | Se till att Udr och NSG: er konfigureras så att Site recovery trafiken inte dirigeras till den lokala. Referera till [nätverk vägledningsdokumentet.](site-recovery-azure-to-azure-networking-guidance.md)  
Anslutning mellan virtuella nätverk | Stöds | Referera till [nätverk vägledningsdokumentet.](site-recovery-azure-to-azure-networking-guidance.md)  
Slutpunkter för virtuellt nätverk | Stöds | Om du är att begränsa åtkomst till virtuellt nätverk till storage-konton, kontrollerar du att betrodda Microsoft-tjänster har åtkomst till lagringskontot. 
Accelererat nätverk | Stöds | Accelererat nätverk måste vara aktiverat på den Virtuella källdatorn. [Läs mer](azure-vm-disaster-recovery-with-accelerated-networking.md).



## <a name="next-steps"></a>Nästa steg
- Läs [nätverk vägledning för att replikera virtuella Azure-datorer](site-recovery-azure-to-azure-networking-guidance.md).
- Distribuera återställning av [replikering av virtuella Azure-datorer](site-recovery-azure-to-azure.md).
