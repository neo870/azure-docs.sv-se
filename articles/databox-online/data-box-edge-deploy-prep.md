---
title: Självstudie om att förbereda Azure-portalen för att distribuera Data Box Edge | Microsoft Docs
description: Den första självstudien om hur du distribuerar Azure Data Box Edge handlar om hur du förbereder Azure-portalen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/08/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to prepare the portal to deploy Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 35ac28d687c8bc6636a7d8e10f54ffb5b219a776
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167474"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-edge-preview"></a>Självstudier: Förbereda distributionen av Azure Data Box Edge (förhandsversion)


Det här är den första självstudien i serien med distributionssjälvstudier som krävs för en fullständig Azure Data Box Edge-distribution. Den här självstudien beskriver hur du förbereder Azure-portalen för att distribuera Data Box Edge-resursen. 

Du måste ha administratörsbehörighet för att utföra installationen och konfigurationen. Portalförberedelserna tar mindre än tio minuter.

I den här guiden får du lära dig att:

> [!div class="checklist"]
> * Skapa en ny resurs
> * Hämta aktiveringsnyckeln

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.


> [!IMPORTANT]
> Data Box Edge är i förhandsversion. Granska [Azures användningsvillkor för förhandsversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) innan du beställer och distribuerar den här lösningen. 

### <a name="get-started"></a>Kom igång

Gå igenom följande självstudier i angiven ordning när du ska distribuera Data Box Edge.

| **#** | **I det här steget** | **Använd de här dokumenten** |
| --- | --- | --- | 
| 1. |**[Förbereda Azure-portalen för Data Box Edge](data-box-edge-deploy-prep.md)** |Skapa och konfigurera din Data Box Edge-resurs innan du installerar en fysisk Data Box Edge-enhet. |
| 2. |**[Installera Data Box Edge](data-box-edge-deploy-install.md)**|Packa upp, rackmontera och kabelanslut den fysiska Data Box Edge-enheten.  |
| 3. |**[Ansluta, konfigurera och aktivera Data Box Edge](data-box-edge-deploy-connect-setup-activate.md)** |Anslut till det lokala webbgränssnittet, slutför installationen av enheten och aktivera enheten. Enheten är redo att konfigurera SMB- eller NFS-resurser.  |
| 4. |**[Överföra data med Data Box Edge](data-box-edge-deploy-add-shares.md)** |Lägg till resurser och anslut till resurser via SMB eller NFS. |
| 5. |**[Transformera data med Data Box Edge](data-box-edge-deploy-configure-compute.md)** |Konfigurera Edge-moduler på enheten för att transformera data då de flyttas till Azure. |

Nu kan du börja konfigurera Azure-portalen.

## <a name="prerequisites"></a>Nödvändiga komponenter

Här hittar du konfigurationskraven för Data Box Edge-resursen, Data Box Edge-enheten och datacenternätverket.

### <a name="for-the-data-box-edge-resource"></a>För Data Box Edge-resursen

Innan du börjar ska du kontrollera att:

* Din Microsoft Azure-prenumeration måste vara aktiverad för Data Box Edge-resursen.
* Du har ditt Microsoft Azure lagringskonto med autentiseringsuppgifter.

### <a name="for-the-data-box-edge-device"></a>För Data Box Edge-enheten

Innan du distribuerar en fysisk enhet kontrollerar du att:

- Du har en 1 U-plats tillgänglig i ett standardrack på 19 tum i datacentret för att rackmontera enheten. 
- Se till att du har tillgång till en plan, stabil och jämn arbetsyta när enheten kan stå på ett säkert sätt.
- Verifiera att webbplatsen där du tänker konfigurera enheten har standardnätström från en oberoende källa eller en strömfördelare på racket med avbrottsfri kraftfälla.
- Du har tillgång till en fysisk enhet.


### <a name="for-the-datacenter-network"></a>För datacenternätverket

Innan du börjar ska du kontrollera att:

* Nätverket i datacentret konfigureras enligt nätverkskraven för din Data Box Edge-enhet. Mer information finns i avsnittet med [systemkrav för Data Box Edge](data-box-gateway-system-requirements.md).

* Din Data Box Edge har en dedikerad Internetbandbredd på minst 20 Mbit/s som alltid är tillgänglig. Den här bandbredden ska inte delas med andra program. Om du använder nätverksbegränsning rekommenderar vi att du använder en Internetbandbredd på 32 Mbit/s eller mer för att begränsningen ska fungera.

## <a name="create-a-new-resource"></a>Skapa en ny resurs

Utför följande steg för att skapa en ny Data Box Edge-resurs. 

Om du har en befintlig Data Box Edge-resurs för hantering av virtuella enheter hoppar du över det här steget och går till [Hämta aktiveringsnyckeln](#get-the-activation-key).

Utför följande steg på Azure-portalen för att skapa en Data Box-resurs.

1. Använd dina Microsoft Azure-autentiseringsuppgifter för att logga in på Azure-förhandsgranskningsportalen på följande URL: [https://aka.ms/databox-edge](https://aka.ms/databox-edge). 

2. Välj den prenumeration som du vill använda för förhandsversionen av Data Box Edge. Välj den region där du vill distribuera Data Box Edge-resursen. I alternativet för **Data Box Edge** klickar du på **Skapa**.

    ![Söka efter Data Box Edge-tjänst](media/data-box-edge-deploy-prep/data-box-edge-sku.png)

3. Ange eller välj följande information för den nya resursen.
    
    |Inställning  |Värde  |
    |---------|---------|
    |Resursnamn   | Ett eget namn som identifierar resursen.<br>Resursnamnet innehåller mellan 2 och 50 tecken som består av bokstäver, siffror och bindestreck.<br> Namnet börjar och slutar med en bokstav eller en siffra.        |
    |Prenumeration    |Prenumerationen är kopplad till ditt faktureringskonto. |
    |Resursgrupp  |Välj en befintlig grupp eller skapa en ny grupp.<br>Läs mer om [Azure-resursgrupper](../azure-resource-manager/resource-group-overview.md).     |
    |Plats     |För den här versionen är följande regioner tillgängliga: USA, östra; USA 2, västra; Asien, sydöstra; och Europa, västra. <br> Välj den plats som är närmast den geografiska region där du vill distribuera enheten.|
    
    ![Skapa Data Box Edge-resurs](media/data-box-edge-deploy-prep/data-box-edge-resource.png)
    
4. Klicka på **OK**.
 
Det tar några minuter att skapa resursen. När resursen har skapats får du ett meddelande.


## <a name="get-the-activation-key"></a>Hämta aktiveringsnyckeln

När Data Box Edge-resursen är igång och körs måste du hämta aktiveringsnyckeln. Den här nyckeln används för att aktivera och ansluta Data Box Edge-enheten med resursen. Du kan hämta den här nyckeln nu när du befinner dig på Azure-portalen.

1. Klicka på den resurs som du skapade och klicka sedan på **Översikt**.

2. Klicka på **Generera nyckel** för att skapa en aktiveringsnyckel. Klicka på kopieringsikonen för att kopiera nyckeln och spara den för senare användning.

    ![Hämta aktiveringsnyckeln](media/data-box-edge-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - Aktiveringsnyckeln upphör att gälla tre dagar efter att den skapats. 
> - Om nyckeln har upphört att gälla genererar du en ny nyckel. Den äldre nyckeln är inte giltig.

## <a name="next-steps"></a>Nästa steg

I den här kursen har du lärt dig om Data Box Edge-ämnen som att:

> [!div class="checklist"]
> * Skapa en ny resurs
> * Hämta aktiveringsnyckeln

Gå vidare till nästa självstudie och lär dig hur du installerar Data Box Edge. 

> [!div class="nextstepaction"]
> [Installera Data Box Edge](./data-box-edge-deploy-install.md)



