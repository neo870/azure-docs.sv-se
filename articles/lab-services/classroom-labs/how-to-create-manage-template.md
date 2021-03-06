---
title: Hantera en mall med ett klassrumslabb i Azure Lab Services | Microsoft Docs
description: Lär dig hur du skapar och hanterar en klassrum labbmall i Azure Lab Services.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: spelluru
ms.openlocfilehash: 3ecbef3b3063ceb413b852f8000b44a85d28d08e
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142512"
---
# <a name="create-and-manage-a-classroom-template-in-azure-lab-services"></a>Skapa och hantera en klassrum-mall i Azure Lab Services
En mall i ett labb är en basavbildning av en virtuell dator som alla användares virtuella datorer skapas från. Konfigurera mallen för virtuella datorer så att den är konfigurerad med exakt det som du vill förse labbanvändarna med. Du kan ange ett namn och en beskrivning av mallen som visas för labbanvändarna. Sedan kan publicera du mallen för att tillgängliggöra instanser av VM-mallen för dina labbanvändare. När du publicerar en mall skapar Azure Lab Services virtuella datorer i labbet med hjälp av mallen. Antalet virtuella datorer som skapas i den här processen är samma som det högsta antalet användare som tillåts i labbet. Du kan definiera antalet i användningsprincipen för labbet. Alla virtuella datorer har samma konfiguration som mallen.

Den här artikeln beskriver hur du skapar och hanterar en mall för virtuell dator i ett klassrumslabb för Azure Lab Services. 

## <a name="publish-a-template-while-creating-a-classroom-lab"></a>Publicera en mall när du skapar ett klassrumslabb
Först, kan du konfigurera och publicera en mall när du skapar ett klassrumslabb.

1. Gå till [webbplatsen för Azure Lab Services](https://labs.azure.com). 
2. Välj **Logga in** och ange dina autentiseringsuppgifter. Azure Lab Services har stöd för organisationskonton och Microsoft-konton. 
3. Utför följande i fönstret **Nytt labb**: 
    1. Ange ett **namn** på ditt labb. 
    2. Ange det högsta **antalet användare** som är tillåtet i labbet. 
    6. Välj **Spara**.

        ![Skapa ett klassrumslabb](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. På sidan för att **välja specifikationer för virtuell dator** utför du följande steg:
    1. Välj en **storlek** för virtuella datorer (VM) som skapas i labbet. 
    2. Välj den **region** där du vill att de virtuella datorerna ska skapas. 
    3. Välj den **VM-avbildning**  som ska användas för att skapa virtuella datorer i labbet. 
    4. Välj **Nästa**.

        ![Ange VM-specifikationer](../media/tutorial-setup-classroom-lab/select-vm-specifications.png)    
5. På sidan **Ange autentiseringsuppgifter** anger du standardautentiseringsuppgifter för alla virtuella datorer i labbet. 
    1. Ange **namnet på användaren** för alla virtuella datorer i labbet.
    2. Ange **lösenordet** för användaren. 

        > [!IMPORTANT]
        > Anteckna namnet och lösenordet. De kommer inte att visas igen.
    3. Välj **Skapa**. 

        ![Ange autentiseringsuppgifter](../media/tutorial-setup-classroom-lab/set-credentials.png)
6. På sidan **Konfigurera mall** kan du se statusen för labbskapandeprocessen. Det tar upp till 20 minuter att skapa mallen i labbet. 

    ![Konfigurera mall](../media/tutorial-setup-classroom-lab/configure-template.png)
7. När konfigurationen av mallen har slutförts visas följande sida: 

    ![Konfigurera mallsidan när det är klart](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)
8. Följande steg är valfria i den här självstudien: 
    1. Starta mallen för den virtuella datorn genom att välja **Starta**.
    2. Anslut till mallen för den virtuella datorn genom att välja **Anslut**. 
    3. Installera och konfigurera programvaran på mallen för den virtuella datorn. 
    4. **Stoppa** den virtuella datorn.  
    5. Ange en **beskrivning** för mallen

        ![Nästa på sidan Konfigurera mall](../media/tutorial-setup-classroom-lab/configure-template-next.png)
9. Välj **Nästa** på mallsidan. 
10. På sidan **Publicera mallen** utför du någon av följande åtgärder. 
    1. Om du vill publicera mallen omedelbart markerar du kryssrutan för *I understand I can't modify the template after publishing (Jag förstår att jag inte kan ändra mallen efter publicering). Den här processen kan bara göras en gång och den kan ta upp till en timme*. Välj sedan **Publicera**.  

        > [!WARNING]
        > När du väl har publicerat kan du inte ångra publiceringen. 
    2. Om du vill publicera senare väljer **Spara till senare**. Du kan publicera mallen för den virtuella datorn när guiden har slutförts. Mer information om hur du konfigurerar och publicerar när guiden slutförts, se mer information om hur du konfigurerar och publicerar när guiden slutförts finns i [publicerar du mallen](#publish-the-template) i avsnittet den [hantera klassrum labs ](how-to-manage-classroom-labs.md) artikeln.

        ![Publicera mall](../media/tutorial-setup-classroom-lab/publish-template.png)
11. Du ser **förloppet för publiceringen**  av mallen. Den här processen kan ta upp till en timma. 

    ![Publicera mall – förlopp](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. Du ser följande sida när mallen har publicerats. Välj **Done** (Klar).

    ![Publicera mall – lyckades](../media/tutorial-setup-classroom-lab/publish-success.png)
1. **Instrumentpanelen** för labbet visas. 
    
    ![Instrumentpanel för klassrumslabb](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)

## <a name="set-up-a-template-after-creating-a-lab"></a>Konfigurera en mall när du har skapat ett labb 
Du kan också ställa in en mall när du har skapat labbet.   

### <a name="set-template-title-and-description"></a>Ange mall rubrik och beskrivning
1. I avsnittet **Mall** väljer du **Redigera** (pennikonen) för mallen. 
2. I fönstret **Användarvy** anger du en **rubrik** för mallen.
3. Ange en **beskrivning** för mallen.
4. Välj **Spara**.

    ![Beskrivning av klassrumslabb](../media/how-to-manage-classroom-labs/lab-description.png)

### <a name="set-up-the-template-vm"></a>Konfigurera den virtuella malldatorn
 Du ansluter till den virtuella malldatorn och installerar program som krävs på den innan du gör den tillgänglig för dina studenter. 

1. Vänta tills den virtuella malldatorn är klar. När den är klar bör knappen **Starta** aktiveras. För att starta den virtuella datorn väljer du **Starta**.

    ![Starta den virtuella malldatorn](../media/tutorial-setup-classroom-lab/start-template-vm.png)
1. För att ansluta till den virtuella datorn väljer du **Anslut** och följer instruktionerna. 

    ![Ansluta till den virtuella malldatorn](../media/tutorial-setup-classroom-lab/connect-template-vm.png)
1. Installera all programvara som krävs för studenterna ska kunna utföra labbet (till exempel Visual Studio, Azure Storage Explorer osv.). 
2. Koppla från (stäng fjärrskrivbordssessionen) den virtuella malldatorn. 
3. **Stoppa** den virtuella malldatorn genom att välja **Stoppa**. 

    ![Stoppa den virtuella malldatorn](../media/tutorial-setup-classroom-lab/stop-template-vm.png)


## <a name="publish-the-template"></a>Publicera mallen 
Om du inte publicerar mallen när du har skapat labbet kan publicera du den senare. Innan du publicerar, kanske du vill ansluta till VM-mallen och uppdatera den med programvara. När du publicerar en mall skapar Azure Lab Services virtuella datorer i labbet med hjälp av mallen. Antalet virtuella datorer som skapas i den här processen är samma som det högsta antalet användare som tillåts i labbet. Du kan definiera antalet i användningsprincipen för labbet. Alla virtuella datorer har samma konfiguration som mallen. 

1. Välj **Publicera** i avsnittet **Mall**. 

    ![Publicera den virtuella malldatorn](../media/tutorial-setup-classroom-lab/public-access.png)
1. På den **publicerar du mallen** meddelanderutan, granska meddelandet och välj **publicera**. Den här processen kan ta lite tid beroende på hur många virtuella datorer som skapas, vilket är samma som det antal användare som tillåts i labbet.
    
    > [!IMPORTANT]
    > När en mall har publicerats kan publiceringen inte tas bort. Du kan publicera om mallen dock. 
4. Växla till sidan **Virtuella datorer** och bekräfta att du ser virtuella datorer som är i tillståndet **Otilldelad**. De här virtuella datorerna har inte tilldelats till studenter ännu. 

    ![Virtuella datorer](../media/tutorial-setup-classroom-lab/virtual-machines.png)
5. Vänta tills de virtuella datorerna har skapats. De måste vara i tillstånd **Stoppad**. Du kan starta en virtuell dator för studenter, ansluta till den virtuella datorn, stoppa den virtuella datorn och ta bort den virtuella datorn på den här sidan. Du kan starta dem på den här sidan eller låta studenterna starta de virtuella datorerna. 

    ![Virtuella datorer i stoppat tillstånd](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)


## <a name="republish-the-template"></a>Publicera om mallen 
När du har publicerat en mall kan du fortfarande ansluta till VM-mallen, uppdatera den och sedan publicera den igen. När du publicerar en mall för virtuell dator, återskapas alla användare VMs avbrytas och de baserat på den uppdaterade mallen. 

1. På instrumentpanelsidan över ditt labb väljer **publicera** i mallavsnittet. 

    ![Publicera knappen](../media/how-to-create-manage-template/republish-button.png)
2. På den **publicera om mallen** meddelanderutan, läser du igenom texten och väljer **publicera** att fortsätta. Annars väljer **Avbryt**. 

    ![Publicera meddelandet mall](../media/how-to-create-manage-template/republish-template-message.png)
3. Du ser status för den publicera på nytt på panelen i den **mall** avsnittet.

    ![Status för att publicera](../media/how-to-create-manage-template/republish-status-publishing.png)
4. När mallen har publicerats kan den här inställningen **publicerad**. 

    ![Publicerar om den lyckades](../media/how-to-create-manage-template/republish-success.png)

## <a name="next-steps"></a>Nästa steg
Kom igång med att konfigurera ett testlabb med Azure Lab Services:

- [Konfigurera ett klassrumslabb](how-to-manage-classroom-labs.md)
- [Konfigurera ett labb](../tutorial-create-custom-lab.md)
