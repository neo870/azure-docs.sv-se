---
title: Skala identifiering och en utvärdering med hjälp av Azure Migrate | Microsoft Docs
description: Beskriver hur du avgör stort antal lokala datorer med hjälp av Azure Migrate-tjänsten.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: raynew
ms.openlocfilehash: b5685640a55e2ce52a202c341cb293fe9315ab14
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50240196"
---
# <a name="discover-and-assess-a-large-vmware-environment"></a>Upptäck och utvärdera en stor VMware-miljö

Azure Migrate har en gräns på 1500 datorer per projekt, den här artikeln beskrivs hur du avgör stort antal lokala virtuella datorer (VM) med hjälp av [Azure Migrate](migrate-overview.md).   

## <a name="prerequisites"></a>Förutsättningar

- **VMware**: de virtuella datorer som du planerar att migrera måste hanteras av vCenter Server version 5.5, 6.0 eller 6.5. Du måste även en ESXi-värd som kör version 5.0 eller senare för att distribuera den Virtuella insamlardatorn.
- **vCenter-kontot**: du behöver ett konto för skrivskyddad åtkomst till vCenter-servern. Azure Migrate använder kontot till att identifiera de lokala virtuella datorerna.
- **Behörigheter**: I vCenter-servern, behöver du behörighet att skapa en virtuell dator genom att importera en fil i OVA-formatet.
- **Inställningar för statistik**: det här kravet gäller endast för den [enstaka identifiering modellen](https://docs.microsoft.com/azure/migrate/concepts-collector#discovery-methods). För identifiering av enstaka modellen ska statistikinställningarna för vCenter-servern vara inställd på nivå 3 innan du påbörjar distributionen. Statistik är att vara satt till 3 för varje dag, vecka och månad datainsamlingen. Om kompatibilitetsnivå är lägre än 3 för någon av de tre datainsamlingen, fungerar utvärderingen, men prestandadata för lagring och nätverk samlas inte in. Storleksrekommendationer som baseras på prestandadata för CPU och minne och konfigurationsdata för disk och nätverkskort.

### <a name="set-up-permissions"></a>Konfigurera behörigheter

Azure Migrate måste ha åtkomst till VMware-servrar för att automatiskt kunna identifiera virtuella datorer för utvärdering. VMware-kontot måste ha följande behörigheter:

- Användartyp: Minst en skrivskyddad användare
- Behörigheter: Datacenter-objekt –> Sprid till underordnat objekt, roll = skrivskyddad
- Information: Användaren tilldelas på datacenternivå och har åtkomst till alla objekt i datacentret.
- Om du vill begränsa åtkomsten tilldelar du rollen Ingen åtkomst med Sprid till underordnat objekt till underordnade objekt (vSphere-värdar, datalager, virtuella datorer och nätverk).

Om du distribuerar i en miljö organisationer, är här ett sätt att konfigurera detta:

1.  Skapa en användare per klient och använder [RBAC](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal), tilldela läsbehörighet till alla de virtuella datorerna tillhör en viss klient. Sedan Använd dessa autentiseringsuppgifter för identifiering. RBAC säkerställer att motsvarande vCenter-användaren har åtkomst till endast klientspecifik virtuella datorer.
2. Du konfigurerar RBAC för olika klientanvändare enligt beskrivningen i följande exempel för användaren nr 1 och 2 för användaren:

    - I **användarnamn** och **lösenord**, anger du de skrivskyddade kontoautentiseringsuppgifter som insamlaren använder för att identifiera virtuella datorer i
    - Datacenter1 - ge läsbehörighet till 1 användare och användare #2. Inte Sprid dessa behörigheter till alla underordnade objekt eftersom du ska ange behörigheter för enskilda virtuella datorer.

      - VM1 (klient #1) (skrivskyddad behörighet till användare #1)
      - VM2 (klient #1) (skrivskyddad behörighet till användare #1)
      - VM3 (klient #2) (skrivskyddad behörighet till användare #2)
      - VM4 (klient #2) (skrivskyddad behörighet till användare #2)

   - Om du utför identifieringen med autentiseringsuppgifter för användare #1 kan sedan identifieras endast VM1 och VM2.

## <a name="plan-your-migration-projects-and-discoveries"></a>Planera ditt migreringsprojekt och identifieringar

En enda Azure Migrate collector stöder identifiering från flera vCenter-servrar (efter varandra) och stöder flera migreringsprojekt identifieringen (efter varandra).

Insamlaren vid enstaka identifiering fungerar i fire and -forget-modell, när identifiering är klar kan du använda samma insamlaren för att samla in data från en annan vCenter-Server eller skicka det till en annan migreringsprojekt. Vid kontinuerlig identifiering, är en enhet ansluten till ett enda projekt så att du inte kan använda samma insamlaren för att utlösa en andra identifiering.

Planera dina upptäckter och utvärderingar som baseras på följande begränsningar:

| **Entitet** | **Gränsen för datorn** |
| ---------- | ----------------- |
| Project    | 1,500             |
| Identifiering  | 1,500             |
| Utvärdering | 1,500             |

Tänk på dessa överväganden:

- När du gör en identifiering med hjälp av Azure Migrate collector kan du ange identifieringsomfånget till en vCenter Server-mapp, datacenter, kluster eller en värddator.
- Om du vill göra mer än en identifiering, verifiera i vCenter Server som de virtuella datorerna som du vill identifiera i mappar, Datacenter, kluster eller värdar som har stöd för begränsning av 1 500 datorer.
- Vi rekommenderar att för utvärdering, du behåller datorer med beroenden inom samma projekt och utvärdering. Kontrollera att beroende datorer finns i samma mapp, datacenter eller kluster för utvärdering i vCenter Server.

Beroende på ditt scenario kan du dela upp dina identifieringar enligt nedan:

### <a name="multiple-vcenter-servers-with-less-than-1500-vms"></a>Flera vCenter-servrar med mindre än 1 500 virtuella datorer
Om du har flera vCenter-servrar i din miljö och det totala antalet virtuella datorer är mindre än 1500, kan du använda följande metod baserat på ditt scenario:

**Enstaka identifiering:** använder en enda insamlare och en enda migration-projekt för att identifiera alla virtuella datorer mellan alla vCenter-servrar. Eftersom enstaka identifiering insamlaren identifierar en vCenter-Server i taget, kan du köra samma insamlaren mot alla vCenter-servrar, efter varandra och peka insamlaren på samma migreringsprojekt. När alla identifieringar är klar kan skapa du sedan utvärderingar för datorer.

**Kontinuerlig identifiering:** vid kontinuerlig identifiering, en enhet kan anslutas till bara ett enda projekt. Så behöver du distribuera en installation för var och en av vCenter-servrar och skapa ett projekt för varje installation och utlösare identifieringar.

### <a name="multiple-vcenter-servers-with-more-than-1500-vms"></a>Flera vCenter-servrar med mer än 1 500 virtuella datorer

Om du har flera vCenter-servrar med mindre än 1 500 virtuella datorer per vCenter-servern, men mer än 1 500 virtuella datorer mellan alla vCenter-servrar kan behöva du skapa flera migration-projekt (en migration-projekt kan innehålla endast 1 500 virtuella datorer). Du kan åstadkomma detta genom att skapa ett migreringsprojekt per vCenter-servern och dela identifieringar.

**Enstaka identifiering:** du kan använda en enda insamlare för att identifiera varje vCenter-Server (efter varandra). Om du vill identifieringar att starta på samma gång, kan du också distribuera flera enheter och köra identifieringar parallellt.

**Kontinuerlig identifiering:** måste du skapa flera insamlaren installationer (en för varje vCenter-Server) och ansluta varje enhet till en identifiering för projektet och utlöser därefter.

### <a name="more-than-1500-machines-in-a-single-vcenter-server"></a>Mer än 1500 datorer i en enda vCenter-Server

Om du har fler än 1 500 virtuella datorer i en enda vCenter-Server, måste du dela upp identifieringen i flera migreringsprojekt. Om du vill dela identifieringar, kan du utnyttja fältet omfång i installationen och anger värden, kluster, mapp eller datacenter som du vill identifiera. Exempel: Om du har två mappar i vCenter Server, en med 1000 virtuella datorer (Mapp1) och andra med 800 virtuella datorer (mapp2) du kan använda fältet omfattning för att dela upptäckter mellan dessa mappar.

**Enstaka identifiering:** du kan använda samma insamlaren för att utlösa båda identifieringar. I den första identifieringen kan du ange Mapp1 som omfång och peka den första migreringsprojekt när den första identifieringen är klar, du kan använda samma insamlaren genom att ändra sitt omfång till Mapp2 och migrering projektinformation till andra migreringsprojekt och göra andra identifiering.

**Kontinuerlig identifiering:** i det här fallet måste du skapa två insamlaren enheter, för den första insamlardatorn, ange omfång som mapp1 och ansluter den till det första migreringsprojektet. Du kan parallellt påbörja Upptäckten av Mapp2 med andra insamlingsprogrammet och ansluter den till andra migreringsprojekt.

### <a name="multi-tenant-environment"></a>Miljö för flera innehavare

Om du har en miljö som delas mellan klienter och du inte vill identifiera de virtuella datorerna i en klient i en annan klient prenumeration, kan du använda fältet omfång i insamlingsprogrammet att definiera omfattningen av identifieringen. Om klienterna delar värdar, skapar du autentiseringsuppgifter som har skrivskyddad åtkomst till endast de virtuella datorerna som hör till specifik klient och använda den här autentiseringsuppgiften i insamlingsprogrammet och ange omfång som värden för att göra identifieringen.

## <a name="discover-on-premises-environment"></a>Identifiera lokala miljö

Du kan sedan starta identifiering av lokala virtuella datorer när du är klar med din plan:

### <a name="create-a-project"></a>Skapa ett projekt

Skapa ett Azure Migrate-projekt utifrån dina behov:

1. I Azure-portalen väljer du **Skapa en resurs**.
2. Sök efter **Azure Migrate** och välj tjänsten **Azure Migrate** i sökresultaten. Välj sedan **Skapa**.
3. Ange ett projektnamn och Azure-prenumerationen för projektet.
4. Skapa en ny resursgrupp.
5. Ange platsen där du vill skapa projektet och välj sedan **skapa**. Observera att du kan fortfarande utvärdera dina virtuella datorer för en annan målplats. Den angivna platsen för projektet används för att lagra de metadata som samlats in från lokala virtuella datorer.

### <a name="set-up-the-collector-appliance"></a>Konfigurera insamlingsprogrammet

Azure Migrate skapar en lokal virtuell dator som kallas för insamlarprogram. Den här virtuella datorn identifierar lokala virtuella VMware-datorer och skickar den metadata om dem till Azure Migrate-tjänsten. Om du vill konfigurera insamlingsprogrammet hämta OVA-filen och importera den till en lokal vCenter Server-instansen.

#### <a name="download-the-collector-appliance"></a>Hämta insamlingsprogrammet

Om du har flera projekt kan behöva du hämta insamlingsprogrammet bara en gång till vCenter Server. När du laddar ned och konfigurera installationen kan du köra den för varje projekt och anger du unika projekt-ID och nyckel.

1. I Azure Migrate-projektet klickar du på **Komma igång** > **Identifiera och utvärdera** > **Identifiera datorer**.
2. I **identifiera datorer**, finns det två alternativ för installationen, klicka på **hämta** för att ladda ned rätt program baserat på dina inställningar.

    a. **Engångsidentifiering:** Tillämpningen för den här modellen kommunicerar med vCenter Server för att samla in metadata om de virtuella datorerna. För insamling av prestandadata för de virtuella datorerna, förlitar den sig på historiska prestandadata som lagras i vCenter Server och samlar in prestandahistoriken för den senaste månaden. I den här modellen samlar Azure Migrate in genomsnittlig beräkning (jämfört med högsta beräkning) för varje mått, [läs mer](https://docs.microsoft.com/azure/migrate/concepts-collector#what-data-is-collected). Eftersom det är en enstaka identifiering återspeglas inte ändringar i den lokala miljön när identifieringen är klar. Om du vill att ändringarna ska återspeglas behöver göra en ny identifiering av samma miljö för samma projekt.

    b. **Kontinuerlig identifiering:** Installation för den här modellen profilerar kontinuerligt den lokala miljön för att samla in användningsdata i realtid för varje virtuell dator. I den här modellen samlas högsta beräkningar in för varje mått (processoranvändning, minnesanvändning osv.). Den här modellen är inte beroende av statistikinställningarna för vCenter Server för insamling av prestandadata. Du kan stoppa kontinuerlig profileringen när som helst från programmet.

    > [!NOTE]
    > Funktionen kontinuerlig identifiering är i förhandsversion.

3. I **kopiera projektautentiseringsuppgifterna**, kopiera-ID och nyckel för projektet. Du behöver dem när du konfigurerar insamlaren.


#### <a name="verify-the-collector-appliance"></a>Kontrollera insamlingsprogrammet

Kontrollera att OVA-filen är säker innan du distribuerar den:

1. Öppna ett kommandofönster för administratör på den dator som du laddade ned filen till.

2. Kör följande kommando för att generera en hash för OVA-filen:

   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

   Exempel på användning: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```

3. Se till att den genererade hashen matchar följande inställningar.

#### <a name="one-time-discovery"></a>Engångsidentifiering

För OVA-version 1.0.9.15 (släppt på 10/23/2018)

**Algoritm** | **Hash-värde**
--- | ---
MD5 | e9ef16b0c837638c506b5fc0ef75ebfa
SHA1 | 37b4b1e92b3c6ac2782ff5258450df6686c89864
SHA256 | 8a86fc17f69b69968eb20a5c4c288c194cdcffb4ee6568d85ae5ba96835559ba

För OVA-version 1.0.9.14 (släppt på 8/24/2018)

**Algoritm** | **Hash-värde**
--- | ---
MD5 | 6d8446c0eeba3de3ecc9bc3713f9c8bd
SHA1 | e9f5bdfdd1a746c11910ed917511b5d91b9f939f
SHA256 | 7f7636d0959379502dfbda19b8e3f47f3a4744ee9453fc9ce548e6682a66f13c

För OVA-version 1.0.9.12

**Algoritm** | **Hash-värde**
--- | ---
MD5 | d0363e5d1b377a8eb08843cf034ac28a
SHA1 | df4a0ada64bfa59c37acf521d15dcabe7f3f716b
SHA256 | f677b6c255e3d4d529315a31b5947edfe46f45e4eb4dbc8019d68d1d1b337c2e

För OVA-version 1.0.9.8

**Algoritm** | **Hash-värde**
--- | ---
MD5 | b5d9f0caf15ca357ac0563468c2e6251
SHA1 | d6179b5bfe84e123fabd37f8a1e4930839eeb0e5
SHA256 | 09c68b168719cb93bd439ea6a5fe21a3b01beec0e15b84204857061ca5b116ff

För OVA-version 1.0.9.7

**Algoritm** | **Hash-värde**
--- | ---
MD5 | d5b6a03701203ff556fa78694d6d7c35
SHA1 | f039feaa10dccd811c3d22d9a59fb83d0b01151e
SHA256 | e5e997c003e29036f62bf3fdce96acd4a271799211a84b34b35dfd290e9bea9c

#### <a name="continuous-discovery"></a>Kontinuerlig identifiering

För OVA-version 1.0.10.4

**Algoritm** | **Hash-värde**
--- | ---
MD5 | 2ca5b1b93ee0675ca794dd3fd216e13d
SHA1 | 8c46a52b18d36e91daeae62f412f5cb2a8198ee5
SHA256 | 3b3dec0f995b3dd3c6ba218d436be003a687710abab9fcd17d4bdc90a11276be

### <a name="create-the-collector-vm"></a>Skapa den virtuella insamlardatorn

Importera den nedladdade filen till vCenter Server:

1. I vSphere-klientkonsolen väljer **filen** > **distribuera OVF-mall**.

    ![Distribuera OVF](./media/how-to-scale-assessment/vcenter-wizard.png)

2. I guiden Distribuera OVF-mall > **källa**, ange platsen för OVA-filen.
3. I **Namn** och **Plats** anger du ett eget namn för den virtuella insamlardatorn och lagerobjektet där den virtuella datorn kommer att finnas.
4. I **Värd/kluster** anger du den värd eller det kluster där den virtuella insamlardatorn körs.
5. Ange lagringsplats för den virtuella insamlardatorn i lagringen.
6. I **Diskformat** anger du disktyp och storlek.
7. I **Nätverksmappning** anger du det nätverk som den virtuella insamlardatorn kommer att ansluta till. Nätverket måste ha en Internetanslutning för att skicka metadata till Azure.
8. Granska och bekräfta inställningarna och välj sedan **Slutför**.

### <a name="identify-the-id-and-key-for-each-project"></a>Identifiera-ID och nyckel för varje projekt

Om du har flera projekt måste du identifiera-ID och nyckel för var och en. Du behöver nyckeln när du kör insamlaren att identifiera de virtuella datorerna.

1. I projektet, väljer **komma igång** > **identifiera och utvärdera** > **identifiera datorer**.
2. I **kopiera projektautentiseringsuppgifterna**, kopiera-ID och nyckel för projektet.
    ![Kopiera projektautentiseringsuppgifterna](./media/how-to-scale-assessment/copy-project-credentials.png)

### <a name="set-the-vcenter-statistics-level"></a>Ange statistiknivån

Insamlingsprogrammet identifieras följande statiska metadata om de valda virtuella datorerna.

1. VM-visningsnamn (på vCenter)
2. Virtuella datorns lager sökväg (värd/mapp i vCenter)
3. IP-adress
4. MAC-adress
5. Operativsystem
5. Antal kärnor, diskar, nätverkskort
6. Minnesstorlek, diskstorlekar
7. Och prestandaräknarna för den virtuella datorn, disk och nätverk som anges i tabellen nedan.

I följande tabell visas de exakta prestandaräknare som samlas in och visar också utvärderingsresultat som påverkas om en särskild räknare inte har samlats in för enstaka identifiering.

För kontinuerlig identifiering samma prestandaräknare som samlas in i realtid (20 sekunder intervall), så det finns inga beroende på statistiknivå. Installationen sedan samlar upp 20 sekunder exemplen för att skapa en enskild datapunkt för varje kvart genom att välja det högsta värdet 20 sekunder exemplen och skickar det till Azure.

|Räknare                                  |Nivå    |Nivå per enhet  |Utvärdering av påverkan                               |
|-----------------------------------------|---------|------------------|------------------------------------------------|
|CPU.Usage.Average                        | 1       |Ej tillämpligt                |Rekommenderad storlek och kostnad                    |
|Mem.Usage.Average                        | 1       |Ej tillämpligt                |Rekommenderad storlek och kostnad                    |
|virtualDisk.read.average                 | 2       |2                 |Diskens storlek, kostnaden för lagring och VM-storlek         |
|virtualDisk.write.average                | 2       |2                 |Diskens storlek, kostnaden för lagring och VM-storlek         |
|virtualDisk.numberReadAveraged.average   | 1       |3                 |Diskens storlek, kostnaden för lagring och VM-storlek         |
|virtualDisk.numberWriteAveraged.average  | 1       |3                 |Diskens storlek, kostnaden för lagring och VM-storlek         |
|NET.Received.Average                     | 2       |3                 |VM-storlek och nätverk kostnad                        |
|NET.Transmitted.Average                  | 2       |3                 |VM-storlek och nätverk kostnad                        |

> [!WARNING]
> För enstaka identifiering om du precis har en högre nivå för statistik, ska det ta upp till en dag att generera prestandaräknare. Därför rekommenderar vi att du kör identifieringen efter en dag. Vänta minst en dag när du har startat identifieringen att profilera miljön och sedan skapa utvärderingar för identifiering av kontinuerlig-modellen.

### <a name="run-the-collector-to-discover-vms"></a>Kör insamlaren för att identifiera virtuella datorer

För varje typ av upptäckt du behöver utföra, kör insamlaren för att identifiera virtuella datorer i det nödvändiga omfånget. Kör identifieringar som efter varandra. Samtidiga identifieringar stöds inte och varje identifiering måste ha ett annat omfång.

1.  Högerklicka på den virtuella datorn i vSphere-klientkonsolen > **Öppna konsol**.
2.  Ange språk, tidszon och lösenordsinställningar för produkten.
3.  På skrivbordet, Välj den **kör insamlare** genväg.
4.  Öppna i Azure Migrate-insamlaren **konfigurera förhandskraven** och sedan:

    a. Acceptera licensvillkoren och läs informationen från tredje part.

    Insamlaren kontrollerar att den virtuella datorn har Internetåtkomst.

    b. Om den virtuella datorn har åtkomst till internet via en proxyserver, väljer **proxyinställningar**, och anger proxyadress och lyssningsport. Ange autentiseringsuppgifter om proxyn kräver autentisering.

    Insamlaren kontrollerar att insamlingstjänsten körs. Tjänsten installeras som standard på den virtuella insamlardatorn.

    c. Ladda ned och installera VMware PowerCLI.

5.  Gör följande i **Specify vCenter Server details** (Ange vCenter Server-information):
    - Ange namn (FQDN) eller IP-adress för vCenter-servern.
    - I **användarnamn** och **lösenord**, anger du de skrivskyddade kontoautentiseringsuppgifter som insamlaren använder för att identifiera virtuella datorer i vCenter Server.
    - I **Välj omfattning** väljer du en omfattning för identifieringen av virtuella datorer. Insamlaren kan identifiera virtuella datorer i angivet omfång. Omfånget kan anges till en viss mapp, ett datacenter eller ett kluster. Det får inte innehålla fler än 1 000 virtuella datorer.

6.  I **ange migreringsprojekt**, ange ID och nyckel för projektet. Om du inte kopierade dem öppnar du Azure-portalen från den Virtuella insamlardatorn. På projektets **översikt** väljer **identifiera datorer** och kopierar värdena.  
7.  I **visa insamlingsförloppet**, övervaka identifieringsprocessen och kontrollera att metadata som samlas in från de virtuella datorerna är i omfattningen. Insamlaren visar en ungefärlig identifieringstid.


#### <a name="verify-vms-in-the-portal"></a>Verifiera virtuella datorer i portalen

För engångsidentifiering beror identifieringstiden på hur många virtuella datorer du identifierar. Normalt tar 100 virtuella datorer när insamlaren är klar att köra det cirka en timme och datainsamling att slutföra. Du kan skapa utvärderingar (både prestandabaserade och som lokala utvärderingar) direkt när identifieringen är klar.

För kontinuerlig identifiering (i förhandsversion) kommer insamlaren kontinuerligt profilera den lokala miljön och fortsätta skicka prestandadata med ett intervall på en timma. Du kan granska datorer i portalen en timme efter att identifieringen startades. Vi rekommenderar starkt att vänta minst en dag innan du skapar några prestandabaserade utvärderingar för de virtuella datorerna.

1. I migrationsprojektet klickar du på **Hantera** > **Datorer**.
2. Kontrollera att de virtuella datorer som du vill identifiera visas i portalen.


## <a name="next-steps"></a>Nästa steg

- Lär dig hur du [skapa en grupp](how-to-create-a-group.md) för utvärdering.
- [Läs mer](concepts-assessment-calculation.md) om hur utvärderingar beräknas.
