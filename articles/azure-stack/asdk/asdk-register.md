---
title: Registrera ASDK med Azure | Microsoft Docs
description: Beskriver hur du registrerar Azure Stack med Azure för att möjliggöra marketplace-syndikering och användningsrapportering.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 19206163a07964b564300bbed1fed45973c8fc74
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/13/2018
ms.locfileid: "49311073"
---
# <a name="azure-stack-registration"></a>Azure Stack-registrering
Du kan registrera din Azure Stack Development Kit (ASDK)-installation med Azure kan du hämta marketplace från Azure och du ställer in handelsdata rapporterar tillbaka till Microsoft. Registrering krävs för att stödja fullständig Azure Stack-funktioner, inklusive marketplace syndikering. Registrering rekommenderas eftersom det gör att du kan testa viktiga Azure Stack-funktioner som marketplace-syndikering och användningsrapportering. När du har registrerat Azure Stack rapporteras användning till Azure commerce. Du kan se den prenumeration som du använde för registrering. ASDK användare debiteras dock inte för eventuell användning av rapporterar.

Om du inte registrerar din ASDK, kan du se en **aktivering krävs** varning om att registrera din Azure Stack Development Kit. Det här beteendet är förväntat.

## <a name="prerequisites"></a>Förutsättningar
Innan du använder dessa instruktioner för att registrera ASDK med Azure, se till att du har installerat Azure Stack PowerShell och ned Azure Stack-verktyg som beskrivs i den [konfigurationen efter distribution](asdk-post-deploy.md) artikeln.

Dessutom språkläge PowerShell måste anges till **FullLanguageMode** på den dator som används för att registrera ASDK med Azure. För att verifiera att det aktuella språkläget har angetts till full, öppna en upphöjd PowerShell-fönster och kör följande PowerShell-kommandon:

```PowerShell  
$ExecutionContext.SessionState.LanguageMode
```

Kontrollera utdata returnerar **FullLanguageMode**. Om något annat språkläge returneras, registreringen måste köras på en annan dator eller språkläget måste anges till **FullLanguageMode** innan du fortsätter.

## <a name="register-azure-stack-with-azure"></a>Registrera Azure Stack med Azure
Följ dessa steg för att registrera ASDK med Azure.

> [!NOTE]
> De här stegen måste köras från en dator som har åtkomst till privilegierad slutpunkt. För ASDK är som värddator för development kit.

1. Öppna en PowerShell-konsol som administratör.  

2. Kör följande PowerShell-kommandon för att registrera din ASDK-installation med Azure. Du behöver att logga in på både Azure-prenumerationen och den lokala ASDK-installationen. Om du inte har en Azure-prenumeration än kan du [skapa ett kostnadsfritt konto här](https://azure.microsoft.com/free/?b=17.06). Registrera Azure Stack medför utan kostnad på din Azure-prenumeration.<br><br>Om du kör skriptet registrering på fler än en instans av Azure Stack med samma Azure-prenumerationens ID, ange ett unikt namn för registrering när du kör den **Set-AzsRegistration** cmdlet. Den **RegistrationName** parametern har ett standardvärde på **AzureStackRegistration**. Om du använder samma namn på fler än en instans av Azure Stack kan misslyckas skriptet.

    ```PowerShell  
    # Add the Azure cloud subscription environment name. 
    # Supported environment names are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
    Add-AzureRmAccount -EnvironmentName "AzureCloud"

    # Register the Azure Stack resource provider in your Azure subscription
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

    #Import the registration module that was downloaded with the GitHub tools
    Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

    #Register Azure Stack
    $AzureContext = Get-AzureRmContext
    $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
    $RegistrationName = "<unique-registration-name>"
    $UsageReporting = $true # Set to $false if using the Capacity Billing model
    Set-AzsRegistration `
    -PrivilegedEndpointCredential $CloudAdminCred `
    -PrivilegedEndpoint AzS-ERCS01 `
    -BillingModel Development `
    -RegistrationName $RegistrationName `
    -UsageReportingEnabled:$UsageReporting
    ```
3. När skriptet har slutförts bör du se det här meddelandet: **miljön är nu registrerad och aktiverad med hjälp av de angivna parametrarna.**

    ![Din miljö är nu registrerad](media/asdk-register/1.PNG)

## <a name="verify-the-registration-was-successful"></a>Kontrollera registreringen har lyckats
Följ stegen nedan för att verifiera att det lyckades ASDK registreringen med Azure.

1. Logga in på den [Azure Stack-administrationsportalen](https://adminportal.local.azurestack.external).

2. Klicka på **Marketplace Management** > **Lägg till från Azure**.

    ![](media/asdk-register/2.PNG)

3. Om du ser en lista med objekt som är tillgängliga från Azure lyckades aktiveringen.

    ![](media/asdk-register/3.PNG)

## <a name="move-a-registration-resource"></a>Flytta en resurs för registrering
En registrering resurs flyttas mellan resursgrupper i samma prenumeration **är** stöds. Läs mer om hur du flyttar resurser till en ny resursgrupp, [flytta resurser till ny resursgrupp eller prenumeration](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).


## <a name="next-steps"></a>Nästa steg
[Lägg till ett Azure Stack marketplace-objekt](.\.\azure-stack-marketplace.md)
