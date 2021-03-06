---
title: Förbereda Azure för haveriberedskap för lokala datorer med Azure Site Recovery | Microsoft Docs
description: Lär dig hur du förbereder Azure för haveriberedskap för lokala datorer med hjälp av Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 10/29/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: c00dd8cd412d79093e4ae1fd7ea30a3071c9b54f
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50210252"
---
# <a name="prepare-azure-resources-for-disaster-recovery-of-on-premises-machines"></a>Förbereda Azure-resurser för haveriberedskap för lokala datorer

 [Azure Site Recovery](site-recovery-overview.md) bidrar till din BCDR-strategi för affärskontinuitet och haveriberedskap genom att hålla dina företagsprogram igång och köra dem vid planerade och oplanerade avbrott. Site Recovery hanterar och samordnar haveriberedskap för lokala datorer och virtuella Azure-datorer, inklusive replikering, redundans och återställning.

Den här artikeln är den första kursen i en serie som illustrerar hur du ställer in haveriberedskap för lokala virtuella datorer. Det är relevant oavsett om du skyddar lokala virtuella VMware-datorer, virtuella Hyper-V-datorer eller fysiska servrar.

> [!NOTE]
> Självstudiekurser är utformade för att visa den enklaste distributionsvägen för ett scenario. De använder standardalternativ där så är möjligt och visar inte alla möjliga inställningar och sökvägar. Detaljerade instruktioner finns i **instruktionsavsnittet** för motsvarande scenario.

Den här artikeln visar hur du förbereder Azure-komponenter när du vill replikera lokala virtuella datorer (Hyper-V eller VMware) eller fysiska Windows-/Linux-servrar till Azure. I den här guiden får du lära dig att:

> [!div class="checklist"]
> * Kontrollera att ditt Azure-konto har replikeringsbehörighet.
> * Skapa ett Azure-lagringskonto. Avbildningar av replikerade datorer lagras i den.
> * Skapa ett Recovery Services-valv. Ett valv innehåller metadata och konfigurationsinformation för virtuella datorer och andra replikeringskomponenter.
> * Skapa ett Azure-nätverk. När de virtuella Azure-datorerna skapats efter redundansen, är de anslutna till det här Azure-nätverket.

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/) innan du börjar.

## <a name="sign-in-to-azure"></a>Logga in på Azure

Logga in på [Azure-portalen](http://portal.azure.com).

## <a name="verify-account-permissions"></a>Kontrollera kontobehörigheten

Om du nyligen skapade ditt kostnadsfria Azure-konto är du administratör för prenumerationen. Om du inte är administratör för prenumerationen kan du tillsammans med administratören tilldela de behörigheter som du behöver. Om du vill aktivera replikering för en ny virtuell dator måste du ha behörighet för att:

- Skapa en virtuell dator i den valda resursgruppen.
- Skapa en virtuell dator i det valda virtuella nätverket.
- Skriva till det valda lagringskontot.

För att slutföra dessa uppgifter måste ditt konto tilldelas en inbyggd roll som virtuell datordeltagare. För att hantera Site Recovery-åtgärder i ett valv, måste ditt konto också tilldelas en inbyggd roll som Site Recovery-deltagare.

## <a name="create-a-storage-account"></a>skapar ett lagringskonto

Avbildningar av replikerade datorer lagras i Azure-lagringen. Virtuella Azure-datorer skapas från minnet vid redundansväxling från lokalt till Azure. Lagringskontot måste finnas i samma region som Recovery Services-valvet. Vi använder Västeuropa i den här självstudiekursen.

1. I [Azure-portalen](https://portal.azure.com) väljer du **Skapa en resurs** > **Storage** > **Storage Account – blob, file, table, queue**.
2. I **Skapa lagringskonto** anger du ett namn för kontot. I de här självstudierna använder vi namnet **contosovmsacct1910171607**. Namnet måste vara unikt i Azure och vara mellan 3 och 24 tecken långt. Det får endast bestå av siffror och gemener.
3. För **Distributionsmodell** väljer du **Resource Manager**.
4. I **Typ av konto** väljer du **Minne (generell användning v1)**. Välj inte blobblagring.
5. I **Replikering** väljer du standardinställningen **Read-access geo-redundant storage** för lagringsredundans. Vi låter **Säker överföring krävs** vara **Inaktiverat**.
6. I **Prestanda** väljer du **Standard** och i **Åtkomstnivå** väljer du standardalternativet **Frekvent**.
7. I **Prenumeration** väljer du den prenumeration som du vill skapa det nya lagringskontot i.
8. Ange ett nytt namn på resursgruppen i **Resursgrupp**. En Azure-resursgrupp är en logisk container där Azure-resurser distribueras och hanteras. I de här självstudierna använder vi namnet **ContosoRG**.
9. Välj den geografiska platsen för ditt lagringskonto för **Plats**. 

   ![skapar ett lagringskonto](media/tutorial-prepare-azure/create-storageacct.png)

9. Skapa lagringskontot genom att välja **Skapa**.

## <a name="create-a-recovery-services-vault"></a>skapar ett Recovery Services-valv

1. I Azure-portalen markerar du **Skapa en resurs** > **Storage** > **Backup och Site Recovery (OMS)**.
2. I **Namn** anger du ett eget namn som identifierar valvet. I de här självstudierna använder vi namnet **ContosoVMVault**.
3. I **Resursgruppen** använder vi **contosoRG**.
4. I **Plats**. Använder vi **Västeuropa**.
5. För att snabbt komma åt valvet från instrumentpanelen väljer du **Fäst på instrumentpanelen** > **Skapa**.

   ![Skapa ett nytt valv](./media/tutorial-prepare-azure/new-vault-settings.png)

   Det nya valvet visas på **Instrumentpanelen** > **Alla resurser** och på huvudsidan för **Recovery Services-valv**.

## <a name="set-up-an-azure-network"></a>Skapa ett Azure-nätverk

När de virtuella Azure-datorerna har skapats från minnet efter redundansen, är de anslutna till det här nätverket.

1. I [Azure Portal](https://portal.azure.com) markerar du **Skapa en resurs** > **Nätverk** > **Virtuellt nätverk**.
2. Låt **Resource Manager** vara valt som distributionsmodell.
3. I **Namn** anger du ett nätverksnamn. Namnet måste vara unikt inom Azure-resursgruppen. Vi använder **ContosASRnet** i den här självinlärningskursen.
4. Ange resursgruppen där nätverket kommer att skapas. Vi använder den befintliga resursgruppen **contosoRG**.
5. I **Adressintervall** anger du nätverksadressintervallet **10.0.0.0/24**. I det här nätverket använder vi inte ett undernät.
6. I **Prenumeration** väljer du den prenumeration där du vill skapa nätverket.
7. För **Plats** väljer du **Europa, västra**. Nätverket måste finnas i samma region som Recovery Services-valvet.
8. Vi lämnar standardalternativen för grundläggande DDoS-skydd utan tjänstslutpunkten i nätverket.
9. Klicka på **Skapa**.

   ![Skapa ett virtuellt nätverk](media/tutorial-prepare-azure/create-network.png)

   Det tar några sekunder att skapa ditt virtuella nätverk. När det har skapats ser du det i instrumentpanelen för Azure Portal.

## <a name="useful-links"></a>Användbara länkar

- [Lär dig om](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) Azure-nätverk.
- [Lär dig om](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts) typer av Azure-lagring.
- [Lär dig om](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs#read-access-geo-redundant-storage) lagringsredundans och [säker överföring](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) för lagring.



## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Förbereda den lokala VMware-infrastrukturen på haveriberedskap till Azure](tutorial-prepare-on-premises-vmware.md)
