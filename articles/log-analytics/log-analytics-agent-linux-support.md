---
title: Felsöka Azure Log Analytics-agenten för Linux | Microsoft Docs
description: Beskriv problem, orsaker och upplösning för de vanligaste problemen med Log Analytics-agenten för Linux.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 2720ecece86133bf551ab42c415b2e9a87420e93
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51237833"
---
# <a name="how-to-troubleshoot-issues-with-the-log-analytics-agent-for-linux"></a>Så här felsöker du problem med Log Analytics-agenten för Linux 

Du får hjälp med att felsöka fel du kan uppleva med Log Analytics-agenten för Linux och föreslår lösningar för att lösa dem.

Om ingen av de här stegen fungerar för dig finns också stöd för följande kanaler:

* Kunder med Premier support-förmåner kan öppna en supportbegäran med [Premier](https://premier.microsoft.com/).
* Kunder med supportavtal för Azure kan öppna en supportbegäran [i Azure-portalen](https://manage.windowsazure.com/?getsupport=true).
* Diagnostisera OMI problem med den [OMI felsökningsguide för](https://github.com/Microsoft/omi/blob/master/Unix/doc/diagnose-omi-problems.md).
* Filen en [GitHub-ärende](https://github.com/Microsoft/OMS-Agent-for-Linux/issues).
* Gå till sidan för Log Analytics Feedback om du vill granska skickade idéer och buggar [ http://aka.ms/opinsightsfeedback ](https://aka.ms/opinsightsfeedback) eller skicka en ny.  

## <a name="important-log-locations-and-log-collector-tool"></a>Viktiga loggfilernas placering och Logginsamlaren verktyg

 Fil | Sökväg
 ---- | -----
 Log Analytics-agenten för Linux-loggfil | `/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log `
 Log agentkonfigurationsfilen för log Analytics | `/var/opt/microsoft/omsconfig/omsconfig.log`

 Vi rekommenderar att du använder våra insamlaren loggverktyget för att hämta viktiga loggar för felsökning eller innan du skickar in ett GitHub-ärende. Du kan läsa mer om verktyget och hur du kör den [här](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/tools/LogCollector/OMS_Linux_Agent_Log_Collector.md).

## <a name="important-configuration-files"></a>Viktiga konfigurationsfiler

 Kategori | Filsökväg
 ----- | -----
 Syslog | `/etc/syslog-ng/syslog-ng.conf` eller `/etc/rsyslog.conf` eller `/etc/rsyslog.d/95-omsagent.conf`
 Prestanda, Nagios, Zabbix, Log Analytics-utdata och allmän agent | `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`
 Ytterligare konfigurationer | `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/*.conf`

 >[!NOTE]
 >Redigera konfigurationsfiler för prestandaräknare och Syslog skrivs över om samlingen konfigureras från den [data-menyn Avancerade inställningar för Log Analytics](log-analytics-data-sources.md#configuring-data-sources) i Azure-portalen för din arbetsyta. Om du vill inaktivera konfigurationen för alla agenter, inaktiverar du insamling från Log Analytics **avancerade inställningar** eller för en enskild agent kör du följande:  
> `sudo su omsagent -c /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable`

## <a name="installation-error-codes"></a>Felkoder för klientinstallationer

| Felkod | Betydelse |
| --- | --- |
| NOT_DEFINED | Eftersom nödvändiga beroenden är installerade, installeras inte auoms auditd-plugin-programmet | Installation av auoms misslyckades, installera paketet auditd. |
| 2 | Ogiltigt alternativ har angetts för shell-paket. Kör `sudo sh ./omsagent-*.universal*.sh --help` för användning |
| 3 | Inget alternativ som angetts för shell-paket. Kör `sudo sh ./omsagent-*.universal*.sh --help` för användning. |
| 4 | Ogiltigt paket skriver eller ogiltiga proxyinställningar; omsagent -*rpm*.sh paket kan endast installeras på RPM-baserade system och omsagent -*deb*.sh paket kan endast installeras på Debian-baserade system. Det är det rekommenderas att du använder universal installationsprogrammet från den [senaste versionen](log-analytics-quick-collect-linux-computer.md#install-the-agent-for-linux). Även [granska](#issue:-unable-to-connect-through-proxy-to-log-analytics) att kontrollera dina proxyinställningar. |
| 5 | Shell-paket måste köras som rot eller så uppstod 403-fel returneras under publiceringen. Kör kommandot med hjälp av `sudo`. |
| 6 | Ogiltig paketera arkitektur eller så uppstod fel 200-felet som returnerades under publiceringen; omsagent -*x64.sh paket kan endast installeras på 64-bitars system och omsagent -* x86.sh paket kan endast installeras på 32-bitars system. Ladda ned rätt paket för din arkitektur från den [senaste versionen](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/latest). |
| 17 | Det gick inte att installera OMS-paketet. Titta igenom kommandoutdata till rot-felet. |
| 19 | Det gick inte att installera OMI-paketet. Titta igenom kommandoutdata till rot-felet. |
| 20 | Det gick inte att installera SCX-paketet. Titta igenom kommandoutdata till rot-felet. |
| 21 | Det gick inte att installera providern kits. Titta igenom kommandoutdata till rot-felet. |
| 22 | Det gick inte att installera paketet tillsammans. Titta igenom kommandoutdata för rot-fel |
| 23 | SCX- eller OMI-paketet redan installerad. Använd `--upgrade` i stället för `--install` att installera paketet shell. |
| 30 | Intern paket-fel. Filen en [GitHub-ärende](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) med information från utdata. |
| 55 | Stöds inte openssl-version eller kan inte ansluta till Log Analytics-tjänsten eller dpkg är låst eller saknas curl program. |
| 61 | Saknas Python ctypes-bibliotek. Installera Python-bibliotek för ctypes eller paket (python-ctypes). |
| 62 | Saknas tar program, installera tar. |
| 63 | Program som saknas sed, installera sed. |
| 64 | Saknas curl-program, installera curl. |
| 65 | Saknas gpg-program, installera gpg. |

## <a name="onboarding-error-codes"></a>Onboarding-felkoder

| Felkod | Betydelse |
| --- | --- |
| 2 | Ogiltigt alternativ anges i omsadmin-skriptet. Kör `sudo sh /opt/microsoft/omsagent/bin/omsadmin.sh -h` för användning. |
| 3 | Ogiltig konfiguration som anges i omsadmin-skriptet. Kör `sudo sh /opt/microsoft/omsagent/bin/omsadmin.sh -h` för användning. |
| 4 | Ogiltig proxy som anges i omsadmin-skriptet. Kontrollera proxyn och finns i vår [dokumentation för att använda en HTTP-proxy](log-analytics-concept-hybrid.md#network-firewall-requirements). |
| 5 | 403 HTTP-fel togs emot från Log Analytics-tjänsten. Visa fullständiga utdata av skriptet omsadmin för information. |
| 6 | Icke-200 HTTP-fel togs emot från Log Analytics-tjänsten. Visa fullständiga utdata av skriptet omsadmin för information. |
| 7 | Det går inte att ansluta till Log Analytics-tjänsten. Visa fullständiga utdata av skriptet omsadmin för information. |
| 8 | Fel vid registrering till Log Analytics-arbetsyta. Visa fullständiga utdata av skriptet omsadmin för information. |
| 30 | Internt skriptfel. Filen en [GitHub-ärende](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) med information från utdata. |
| 31 | Fel vid generering agent-ID. Filen en [GitHub-ärende](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) med information från utdata. |
| 32 | Fel vid generering av certifikat. Visa fullständiga utdata av skriptet omsadmin för information. |
| 33 | Fel vid generering av metaconfiguration för omsconfig. Filen en [GitHub-ärende](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) med information från utdata. |
| 34 | Metaconfiguration generation skript finns inte. Försök registrering med `sudo sh /opt/microsoft/omsagent/bin/omsadmin.sh -w <Workspace ID> -s <Workspace Key>`. |

## <a name="enable-debug-logging"></a>Aktivera felsökningsloggning
### <a name="oms-output-plugin-debug"></a>Felsökning för OMS utdata-plugin-programmet
 FluentD gör för plugin-programmet-specifika loggningsnivåer så att du kan ange olika loggningsnivåerna för indata och utdata. Om du vill ange en annan logg för OMS-utdata, redigera allmän agentkonfiguration på `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`.  

 I OMS utdata plugin-programmet, före utgången av konfigurationsfilen ändrar den `log_level` egenskap från `info` till `debug`:

 ```
 <match oms.** docker.**>
  type out_oms
  log_level debug
  num_threads 5
  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
 ```

Felsökningsloggning kan du se gruppbaserade överföringar till Log Analytics-tjänsten med typen, antalet dataobjekt och åtgången tid för att skicka:

*Exempel aktiverat felsökningsloggen:*

```
Success sending oms.nagios x 1 in 0.14s
Success sending oms.omi x 4 in 0.52s
Success sending oms.syslog.authpriv.info x 1 in 0.91s
```

### <a name="verbose-output"></a>Utförliga utdata
Istället för att använda plugin-programmet för OMS utdata du kan också mata ut dataobjekt direkt till `stdout`, som är synliga i Log Analytics-agenten för Linux-loggfil.

I Log Analytics Allmänt agentkonfigurationsfilen på `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, kommentera ut OMS utdata-plugin-programmet genom att lägga till en `#` framför varje rad:

```
#<match oms.** docker.**>
#  type out_oms
#  log_level info
#  num_threads 5
#  buffer_chunk_limit 5m
#  buffer_type file
#  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms*.buffer
#  buffer_queue_limit 10
#  flush_interval 20s
#  retry_limit 10
#  retry_wait 30s
#</match>
```

Ta bort kommentarerna i följande avsnitt nedan utdata-plugin-programmet genom att ta bort den `#` framför varje rad:

```
<match **>
  type stdout
</match>
```

## <a name="issue-unable-to-connect-through-proxy-to-log-analytics"></a>Problem: Det gick inte att ansluta via proxy till Log Analytics

### <a name="probable-causes"></a>Troliga orsaker
* Proxyn som anges under publiceringen var felaktig
* Log Analytics och Azure Automation-tjänstslutpunkter är inte tillåten i ditt datacenter 

### <a name="resolution"></a>Lösning
1. Reonboard till Log Analytics-tjänsten med Log Analytics-agenten för Linux med hjälp av följande kommando med alternativet `-v` aktiverat. Det gör att utförliga utdata för den agent som ansluter via proxy till Log Analytics-tjänsten. 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <Workspace ID> -s <Workspace Key> -p <Proxy Conf> -v`

2. Läs avsnittet [uppdatera proxyinställningar](log-analytics-agent-manage.md#update-proxy-settings) att verifiera att du har konfigurerat agenten för kommunikation via en proxyserver korrekt.    
* Kontrollera att följande slutpunkter i Log Analytics är vitlistade:

    |Agentresurs| Portar | Riktning |
    |------|---------|----------|  
    |*.ods.opinsights.azure.com | Port 443| Inkommande och utgående |  
    |*.oms.opinsights.azure.com | Port 443| Inkommande och utgående |  
    |*.blob.core.windows.net | Port 443| Inkommande och utgående |  
    |*.azure-automation.net | Port 443| Inkommande och utgående | 

## <a name="issue-you-receive-a-403-error-when-trying-to-onboard"></a>Problem: Du får ett 403-fel vid försök att publicera

### <a name="probable-causes"></a>Troliga orsaker
* Datum och tid stämmer på Linux-Server 
* Arbetsyte-ID och Arbetsytenyckel används är inte rätt

### <a name="resolution"></a>Lösning

1. Kontrollera tiden på Linux-server med kommandot datum. Om tiden är +/-15 minuter efter den aktuella tiden, misslyckas onboarding. Till rätt detta Uppdatera datum och/eller tidszonen för din Linux-server. 
2. Kontrollera att du har installerat den senaste versionen av Log Analytics-agenten för Linux.  Den senaste versionen nu meddelar dig om tidsförskjutningsintervallet som orsakar onboarding felet.
3. Reonboard med rätt arbetsyte-ID och Arbetsytenyckel följa installationsinstruktionerna tidigare i den här artikeln.

## <a name="issue-you-see-a-500-and-404-error-in-the-log-file-right-after-onboarding"></a>Problem: Du ser ett 500 och 404-fel i loggfilen direkt efter integreringen
Det här är ett känt problem som uppstår vid första överföring av Linux-data till Log Analytics-arbetsytan. Detta påverkar inte data som skickas eller service-upplevelse.

## <a name="issue-you-are-not-seeing-any-data-in-the-azure-portal"></a>Problem: Du inte ser några data i Azure portal

### <a name="probable-causes"></a>Troliga orsaker

- Kom igång med Log Analytics-tjänsten misslyckades
- Anslutning till Log Analytics-tjänsten är blockerad
- Log Analytics-agenten för Linux-data som säkerhetskopieras

### <a name="resolution"></a>Lösning
1. Kontrollera om onboarding Log Analytics-tjänsten lyckades genom att kontrollera om det finns följande fil: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. Reonboard med hjälp av den `omsadmin.sh` kommandoradsinstruktioner
3. Om du använder en proxyserver, referera till proxy Lösningssteg som angavs tidigare.
4. I vissa fall när Log Analytics-agenten för Linux inte kan kommunicera med tjänsten data på agenten är i kö till den fullständiga buffertstorleken, vilket är 50 MB. Agenten ska startas genom att köra följande kommando: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`. 

    >[!NOTE]
    >Det här problemet löses i agenten version 1.1.0-28 och senare.


## <a name="issue-you-are-not-seeing-forwarded-syslog-messages"></a>Problem: Du ser inte vidarebefordrade Syslog-meddelanden 

### <a name="probable-causes"></a>Troliga orsaker
* Konfigurationen tillämpas på Linux-servern tillåter inte insamling av skickade anläggningar och/eller loggningsnivåer
* Syslog vidarebefordras inte korrekt till Linux-servern
* Hur många meddelanden som vidarebefordras per sekund behövs för för den grundläggande konfigurationen av Log Analytics-agenten för Linux för att hantera

### <a name="resolution"></a>Lösning
* Kontrollera konfigurationen i Log Analytics-arbetsytan för Syslog innehåller alla funktioner och rätt loggningsnivåer. Granska [konfigurera sysloginsamling i Azure portal](log-analytics-data-sources-syslog.md#configure-syslog-in-the-azure-portal)
* Kontrollera interna syslog-meddelanden Daemon (`rsyslog`, `syslog-ng`) kan ta emot vidarebefordrade meddelanden
* Kontrollera brandväggsinställningarna för på Syslog-servern för att säkerställa att meddelanden inte blockeras
* Simulera en Syslog-meddelande till Log Analytics med hjälp av `logger` kommando
  * `logger -p local0.err "This is my test message"`

## <a name="issue-you-are-receiving-errno-address-already-in-use-in-omsagent-log-file"></a>Problem: Du får Errno adress redan används i omsagent loggfil
Om du ser `[error]: unexpected error error_class=Errno::EADDRINUSE error=#<Errno::EADDRINUSE: Address already in use - bind(2) for "127.0.0.1" port 25224>` i omsagent.log.

### <a name="probable-causes"></a>Troliga orsaker
Det här felet indikerar att Linux-diagnostiktillägget (LAD) installeras sida vid sida med Log Analytics Linux VM-tillägget och den använder samma port för syslog-datainsamling som omsagent.

### <a name="resolution"></a>Lösning
1. Kör följande kommandon (Observera att 25224 är ett exempel och det är möjligt att i din miljö du ser ett annat portnummer som används av LAD) som rot:

    ```
    /opt/microsoft/omsagent/bin/configure_syslog.sh configure LAD 25229

    sed -i -e 's/25224/25229/' /etc/opt/microsoft/omsagent/LAD/conf/omsagent.d/syslog.conf
    ```

    Sedan måste du redigera rätt `rsyslogd` eller `syslog_ng` config filen och ändra LAD-relaterade konfigurationen att skriva till port 25229.

2. Om Virtuellt datorn körs `rsyslogd`, filen som ska ändras är: `/etc/rsyslog.d/95-omsagent.conf` (om den finns, annars `/etc/rsyslog`). Om Virtuellt datorn körs `syslog_ng`, filen som ska ändras är: `/etc/syslog-ng/syslog-ng.conf`.
3. Starta om omsagent `sudo /opt/microsoft/omsagent/bin/service_control restart`.
4. Starta om syslog-tjänsten.

## <a name="issue-you-are-unable-to-uninstall-omsagent-using-purge-option"></a>Problem: Du kan inte avinstallera omsagent alternativet Rensa

### <a name="probable-causes"></a>Troliga orsaker

* Linux-Diagnostiktillägget installeras
* Linux-Diagnostiktillägget har installeras och avinstalleras, men du fortfarande ser ett fel om omsagent som används av mdsd och kan inte tas bort.

### <a name="resolution"></a>Lösning
1. Avinstallera Linux-Diagnostiktillägget (LAD).
2. Ta bort Linux-Diagnostiktillägget filer från datorn om de finns på följande plats: `/var/lib/waagent/Microsoft.Azure.Diagnostics.LinuxDiagnostic-<version>/` och `/var/opt/microsoft/omsagent/LAD/`.

## <a name="issue-you-cannot-see-data-any-nagios-data"></a>Problem: Du kan inte se data Nagios data 

### <a name="probable-causes"></a>Troliga orsaker
* Omsagent användaren har inte behörighet att läsa från Nagios-loggfil
* Nagios käll- och filter har inte uncommented från omsagent.conf fil

### <a name="resolution"></a>Lösning
1. Lägg till omsagent användare att läsa från Nagios-filen genom att följa dessa [instruktioner](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#nagios-alerts).
2. I Log Analytics-agenten för Linux Allmänt konfigurationsfilen på `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, se till att **både** Nagios käll- och filter är uncommented.

    ```
    <source>
      type tail
      path /var/log/nagios/nagios.log
      format none
      tag oms.nagios
    </source>

    <filter oms.nagios>
      type filter_nagios_log
    </filter>
    ```

## <a name="issue-you-are-not-seeing-any-linux-data"></a>Problem: Du inte ser några data i Linux 

### <a name="probable-causes"></a>Troliga orsaker
* Kom igång med Log Analytics-tjänsten misslyckades
* Anslutning till Log Analytics-tjänsten är blockerad
* Virtuella datorn har startats om
* OMI-paketet har manuellt uppgraderats till en senare version jämfört med vad som har installerats med Log Analytics-agenten för Linux-paket
* DSC-resurs loggar *gick inte att hitta klassen* fel i `omsconfig.log` loggfil
* Log Analytics-agenten för data som säkerhetskopieras
* DSC-loggar *aktuella konfigurationen finns inte. Kör Start-DscConfiguration-kommando med - sökvägsparameter att ange en konfigurationsfil och skapa en aktuell konfiguration först.* i `omsconfig.log` loggfil, men inga loggmeddelande finns om `PerformRequiredConfigurationChecks` åtgärder.

### <a name="resolution"></a>Lösning
1. Installera alla beroenden som auditd-paketet.
2. Kontrollera om Log Analytics-tjänsten lyckades genom att kontrollera om det finns följande fil: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`.  Om den inte, har reonboard med hjälp av kommandoraden omsadmin.sh [instruktioner](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line).
4. Om du använder en proxy, kontrollera proxy felsökning stegen ovan.
5. I vissa Azure distributionssystem startar omid OMI server daemon inte efter att den virtuella datorn startas om. Detta leder inte se granskning eller ChangeTracking UpdateManagement lösning-relaterade data. Lösningen är att manuellt starta omi-servern genom att köra `sudo /opt/omi/bin/service_control restart`.
6. Efter OMI paketet manuellt till en nyare version, måste den startas om manuellt för Log Analytics-agenten ska fortsätta att fungera. Det här steget är obligatoriskt för vissa distributioner där OMI-servern inte startar automatiskt när den har uppgraderats. Kör `sudo /opt/omi/bin/service_control restart` att starta om OMI.
7. Om du ser DSC-resurs *gick inte att hitta klassen* fel i omsconfig.log, kör `sudo /opt/omi/bin/service_control restart`.
8. I vissa fall när Log Analytics-agenten för Linux inte kan kommunicera med Log Analytics-tjänsten data på agenten har säkerhetskopierats till den fullständiga buffertstorleken: 50 MB. Agenten ska startas genom att köra följande kommando `/opt/microsoft/omsagent/bin/service_control restart`.

    >[!NOTE]
    >Det här problemet löses i agenten version 1.1.0-28 eller senare
    >

* Om `omsconfig.log` loggfilen inte indikerar att `PerformRequiredConfigurationChecks` åtgärder körs regelbundet på systemet, det kan finnas ett problem med cron-jobb/tjänsten. Kontrollera att det finns ett cron-jobb `/etc/cron.d/OMSConsistencyInvoker`. Om det behövs kör du följande kommandon för att skapa cron-jobb:

    ```
    mkdir -p /etc/cron.d/
    echo "*/15 * * * * omsagent /opt/omi/bin/OMSConsistencyInvoker >/dev/null 2>&1" | sudo tee /etc/cron.d/OMSConsistencyInvoker
    ```

    Kontrollera också cron-tjänsten körs. Du kan använda `service cron status` med Debian, Ubuntu, SUSE, eller `service crond status` med RHEL, CentOS, Oracle Linux för att kontrollera status för den här tjänsten. Om tjänsten inte finns, kan du installera binärfilerna och starta tjänsten med hjälp av följande:

    **Ubuntu/Debian**

    ```
    # To Install the service binaries
    sudo apt-get install -y cron
    # To start the service
    sudo service cron start
    ```

    **SUSE**

    ```
    # To Install the service binaries
    sudo zypper in cron -y
    # To start the service
    sudo systemctl enable cron
    sudo systemctl start cron
    ```

    **RHEL/CeonOS**

    ```
    # To Install the service binaries
    sudo yum install -y crond
    # To start the service
    sudo service crond start
    ```

    **Oracle Linux**

    ```
    # To Install the service binaries
    sudo yum install -y cronie
    # To start the service
    sudo service crond start
    ```

## <a name="issue-when-configuring-collection-from-the-portal-for-syslog-or-linux-performance-counters-the-settings-are-not-applied"></a>Problem: När du konfigurerar samlingen från portalen för Syslog- eller Linux-prestandaräknare, gäller inte inställningarna

### <a name="probable-causes"></a>Troliga orsaker
* Log Analytics-agenten för Linux har inte hämtas den senaste konfigurationen
* De ändrade i portalen har inte tillämpats

### <a name="resolution"></a>Lösning
**Bakgrund:** `omsconfig` är Log Analytics-agenten för Linux-konfiguration-agenten som söker efter nya portal-sida-konfigurationen var femte minut. Den här konfigurationen tillämpas sedan på Log Analytics-agenten för Linux-konfigurationsfiler finns på /etc/opt/microsoft/omsagent/conf/omsagent.conf.

* I vissa fall kanske inte Log Analytics-agenten för Linux-konfiguration-agenten kan kommunicera med tjänsten Portalkonfiguration, vilket resulterar i senaste konfiguration som inte används.
  1. Kontrollera att den `omsconfig` agenten är installerad genom att köra `dpkg --list omsconfig` eller `rpm -qi omsconfig`.  Om det inte är installerat installerar du om den senaste versionen av Log Analytics-agenten för Linux.

  2. Kontrollera att den `omsconfig` agenten kan kommunicera med Log Analytics-tjänsten genom att köra följande kommando `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'`. Det här kommandot returnerar konfigurationen agenten tar emot från tjänsten, inklusive Syslog-inställningar, prestandaräknare för Linux och anpassade loggar. Om kommandot misslyckas kör du följande kommando `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py'`. Detta kommando tvingar fram omsconfig agenten att kommunicera med Log Analytics-tjänsten och hämta den senaste konfigurationen.

## <a name="issue-you-are-not-seeing-any-custom-log-data"></a>Problem: Du ser inte alla anpassade loggdata 

### <a name="probable-causes"></a>Troliga orsaker
* Det gick inte att tjänsten Log Analytics.
* Inställningen **gäller följande konfiguration för Mina Linux-servrar** inte har valts.
* omsconfig har inte hämtas den senaste konfigurationen för den anpassade loggen från tjänsten.
* Log Analytics-agenten för Linux-användare `omsagent` inte kan komma åt den anpassade loggen bero på behörigheter eller inte kunde hittas.  Du kan se följande fel:
 * `[DATETIME] [warn]: file not found. Continuing without tailing it.`
 * `[DATETIME] [error]: file not accessible by omsagent.`
* Kända problem med konkurrenstillstånd fast i Log Analytics-agenten för Linux-version 1.1.0-217

### <a name="resolution"></a>Lösning
1. Kontrollera Kom igång med Log Analytics har lyckats genom att kontrollera om det finns följande fil: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`. Om inte, antingen:  

  1. Reonboard omsadmin.sh från kommandoraden [instruktioner](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line).
  2. Under **avancerade inställningar** i Azure-portalen kontrollerar du att inställningen **gäller följande konfiguration för Mina Linux-servrar** är aktiverad.  

2. Kontrollera att den `omsconfig` agenten kan kommunicera med Log Analytics-tjänsten genom att köra följande kommando `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'`.  Det här kommandot returnerar konfigurationen agenten tar emot från tjänsten, inklusive Syslog-inställningar, prestandaräknare för Linux och anpassade loggar. Om kommandot misslyckas kör du följande kommando `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py`. Detta kommando tvingar fram omsconfig agenten att kommunicera med Log Analytics-tjänsten och hämta den senaste konfigurationen.

**Bakgrund:** i stället för Log Analytics-agenten för Linux körs som en privilegierad användare - `root`, agenten körs som den `omsagent` användaren. I de flesta fall måste explicit behörighet beviljas till den här användaren för att vissa filer som ska läsas. Att bevilja behörighet till `omsagent` användare, kör följande kommandon:

1. Lägg till den `omsagent` användare till specifika grupp `sudo usermod -a -G <GROUPNAME> <USERNAME>`
2. Bevilja universal läsbehörighet till den nödvändiga filen `sudo chmod -R ugo+rx <FILE DIRECTORY>`

Det finns ett känt problem med ett konkurrenstillstånd med Log Analytics-agenten för Linux tidigare version än 1.1.0-217. När du har uppdaterat till den senaste agenten, kör du följande kommando för att hämta den senaste versionen av plugin-programmet utdata `sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`.

## <a name="issue-you-are-trying-to-reonboard-to-a-new-workspace"></a>Problem: Du försöker att reonboard till en ny arbetsyta
När du försöker reonboard en agent till en ny arbetsyta, måste konfigurationen för logganalys-agenten rensas innan reonboarding. Om du vill rensa gammal konfiguration från agenten kan köra shell-paket med `--purge`

```
sudo sh ./omsagent-*.universal.x64.sh --purge
```
Eller

```
sudo sh ./onboard_agent.sh --purge
```

Du kan fortsätta reonboard när du har använt den `--purge` alternativet

## <a name="log-analytics-agent-extension-in-the-azure-portal-is-marked-with-a-failed-state-provisioning-failed"></a>Log Analytics agent-tillägget i Azure-portalen är märkt med ett felaktigt tillstånd: Etableringen misslyckades

### <a name="probable-causes"></a>Troliga orsaker
* Log Analytics-agenten har tagits bort från operativsystemet
* Log Analytics agent-tjänsten inte är tillgängligt, inaktiverat eller inte konfigurerat

### <a name="resolution"></a>Lösning 
Utför följande steg för att åtgärda problemet.
1. Ta bort tillägg från Azure-portalen.
2. Installera agenten efter den [instruktioner](log-analytics-quick-collect-linux-computer.md).
3. Starta om agenten genom att köra följande kommando: `sudo /opt/microsoft/omsagent/bin/service_control restart`.
* Vänta några minuter och Etableringsstatus ändras till **etablering lyckades**.


## <a name="issue-the-log-analytics-agent-upgrade-on-demand"></a>Problem: Uppgradera Log Analytics-agenten på begäran

### <a name="probable-causes"></a>Troliga orsaker

Paket för Log Analytics-agenten på värden är inaktuell.

### <a name="resolution"></a>Lösning 
Utför följande steg för att åtgärda problemet.

1. Kontrollera den senaste versionen på [sidan](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/).
2. Ladda ned installationsskriptet (1.4.2-124 som exempel version):

    ```
    wget https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.2-124/omsagent-1.4.2-124.universal.x64.sh
    ```

3. Uppgradera paket genom att köra `sudo sh ./omsagent-*.universal.x64.sh --upgrade`.
