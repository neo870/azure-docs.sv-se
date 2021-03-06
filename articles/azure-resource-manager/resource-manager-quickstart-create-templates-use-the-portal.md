---
title: Skapa och distribuera en Azure Resource Manager-mall via Azure-portalen | Microsoft Docs
description: Lär dig hur du skapar din första Azure Resource Manager-mall med Azure-portalen och hur du distribuerar den.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/17/2018
ms.topic: quickstart
ms.author: jgao
ms.openlocfilehash: 62bc29495bab3446b6131223110c694e53412db9
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/18/2018
ms.locfileid: "49407647"
---
# <a name="quickstart-create-and-deploy-azure-resource-manager-templates-by-using-the-azure-portal"></a>Snabbstart: Skapa och distribuera Azure Resource Manager-mallar med hjälp av Azure-portalen

Lär dig hur du skapar din första Azure Resource Manager-mall genom att generera en med hjälp av Azure-portalen samt hur du redigerar och distribuerar mallen från Azure-portalen. Resource Manager-mallar är JSON-filer som definierar de resurser du behöver för att distribuera lösningen. Anvisningarna i den här självstudien gör att ett Azure Storage-konto skapas. Du kan använda samma process för att skapa andra Azure-resurser.

Om du inte har en Azure-prenumeration kan du [skapa ett kostnadsfritt konto ](https://azure.microsoft.com/free/) innan du börjar.

## <a name="generate-a-template-using-the-portal"></a>Generera en mall med hjälp av portalen

I det här avsnittet skapar du ett lagringskonto med hjälp av Azure-portalen. Innan du distribuerar lagringskontot har du möjligheten att utforska den mall som genereras av portalen baserat på dina konfigurationer. Du kan spara mallen och använda den igen senare.

1. Logga in på [Azure-portalen](https://portal.azure.com).
2. Välj **Skapa en resurs** > **Lagring** > **Lagringskonto – blob, fil, tabell, kö**.

    ![Skapa ett Azure-lagringskonto med hjälp av Azure-portalen](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-portal.png)
3. Ange följande information. 

    - **Resursgrupp**: Skapa en ny Azure-resursgrupp med ett namn som du väljer. På skärmbilden är namnet på resursgruppen *mystorage1016rg*.
    - **Namn**: Ge lagringskontot ett unikt namn. På skärmbilden är namnet *mystorage1016*.

    Du kan använda standardvärdena för resten av egenskaperna.

    ![Skapa en konfiguration för Azure-lagringskonto med hjälp av Azure-portalen](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account.png)

    > [!NOTE]
    > Vissa av de exporterade mallarna kräver vissa ändringar innan du kan distribuera dem.

4. Välj **Granska + skapa** längst ned på skärmen. 
5. Välj **Ladda ned en mall för automatisering** längst ned på skärmen. Portalen visar den genererade mallen:

    ![Generera en mall från portalen](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template.png)

    Huvudfönsterrutan visar mallen. Det är en JSON-fil med fyra element på den översta nivån – `schema`, `contentVersion`, `parameters` och `resources`. Mer information finns i [Förstå strukturen och syntaxen för Azure Resource Manager-mallar](./resource-group-authoring-templates.md)

    Det finns sex parametrar angivna. En av dem heter **storageAccountName**. Den andra markerade delen visar hur du använder den här parametern i mallen. I nästa avsnitt kan du redigera mallen för att använda ett genererat namn för lagringskontot.

    En Azure-resurs har definierats i mallen. Typen är [Microsoft.Storage/storageAccounts]. Se hur resursen definieras samt definitionsstrukturen.
6. Välj **Ladda ned**. Spara **template.json** från det nedladdade paketet till datorn. I nästa avsnitt använder du ett malldistributionsverktyg för att redigera mallen.
7. Välj fliken **Parameter** för att se de värden som du angav för parametrarna. Anteckna dessa värden, eftersom du behöver dem i nästa avsnitt när du distribuerar mallen.

    ![Generera en mall från portalen](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template-parameters.png)

    Med hjälp av både mallen och parameterfilerna kan du skapa ett Azure-lagringskonto.

## <a name="edit-and-deploy-the-template"></a>Redigera och distribuera mallen

Du kan använda Azure-portalen för att utföra viss grundläggande redigering av mallen. I den här snabbstarten använder du portalverktyget *Malldistribution*. För redigering av mer komplex kod bör du överväga att använda [Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md), som tillhandahåller mer omfattande redigeringsfunktioner.

Azure kräver att varje Azure-tjänst har ett unikt namn. Distributionen misslyckas om du anger namnet på ett lagringskonto som redan finns. För att undvika det här problemet kan du använda mallfunktionsanropet `uniquestring()` för att generera ett unikt lagringskontonamn.

1. I Azure-portalen väljer du **Skapa en resurs**.
2. I **Sök på Marketplace** skriver du **malldistribution** och trycker sedan på **RETUR**.
3. Välj **Malldistribution**.

    ![Azure Resource Manager-mallbibliotek](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-library.png)
4. Välj **Skapa**.
5. Välj alternativet för att **skapa din egen mall i redigeringsprogrammet**.
6. Välj **Läs in fil** och följ sedan anvisningarna för att läsa in template.json som du laddade ned i det sista avsnittet.
7. Lägg till en variabel så som det visas på följande skärmbild:

    ```json
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
    ```
    ![Azure Resource Manager-mallar](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-edit-storage-account-template-revised.png)

    Två funktioner används här: `concat()` och `uniqueString()`.

8. Ta bort parametern för **storageAccountName** som är markerad i föregående skärmbild.
9. Uppdatera namnelementet för resursen **Microsoft.Storage/storageAccounts** för att använda den nyligen definierade variabeln i stället för parametern:

    ```json
    "name": "[variables('storageAccountName')]",
    ```

    Den slutliga mallen bör se ut så här:

    ```json
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "location": {
                "type": "string"
            },
            "accountType": {
                "type": "string"
            },
            "kind": {
                "type": "string"
            },
            "accessTier": {
                "type": "string"
            },
            "supportsHttpsTrafficOnly": {
                "type": "bool"
            }
        },
        "variables": {
            "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
        },
        "resources": [
            {
                "name": "[variables('storageAccountName')]",
                "type": "Microsoft.Storage/storageAccounts",
                "apiVersion": "2018-07-01",
                "location": "[parameters('location')]",
                "properties": {
                    "accessTier": "[parameters('accessTier')]",
                    "supportsHttpsTrafficOnly": "[parameters('supportsHttpsTrafficOnly')]"
                },
                "dependsOn": [],
                "sku": {
                    "name": "[parameters('accountType')]"
                },
                "kind": "[parameters('kind')]"
            }
        ],
        "outputs": {}
    }
    ```
7. Välj **Spara**.
8. Ange följande värden:

    - **Resursgrupp**: Ge resursgruppen ett unikt namn.
    - **Plats**: Välj en plats för resursgruppen.
    - **Plats**: Välj en plats för lagringskontot.  Du kan använda samma plats som för resursgruppen.
    - **Kontotyp**: Ange **Standard_LRS** för den här snabbstarten.
    - **Variant**: Ange **StorageV2** för den här snabbstarten.
    - **Åtkomstnivå**: Ange **Frekvent** för den här snabbstarten.
    - **Https Traffic Only Enabled** (Endast HTTPS-trafik aktiverat).  Välj **true** för den här snabbstarten.
    - **Jag godkänner villkoren ovan**: (välj)

    Här är en skärmbild på en exempeldistribution:

    ![Distribution av Azure Resource Manager-mallar](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-deploy.png)

10. Välj **Köp**.
11. Välj klockikonen (meddelanden) längst upp på skärmen för att se distributionsstatus. Vänta tills distributionen är klar.

    ![Distributionsmeddelande för Azure Resource Manager-mallar](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-portal-notification.png)

12. Välj **Gå till resursgrupp** i meddelandefönstret. Du bör se en skärm som liknar följande:

    ![Resursgrupp för distribution av Azure Resource Manager-mallar](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-portal-deployment-resource-group.png)

    Du kan se att distributionen lyckades och att det bara finns ett lagringskonto i resursgruppen. Lagringskontots namn är en unik sträng som genereras av mallen. Mer information om hur du använder Azure Storage-konton finns i [Snabbstart: Ladda upp, ladda ned och lista blobar med Azure Portal](../storage/blobs/storage-quickstart-blobs-portal.md).

## <a name="clean-up-resources"></a>Rensa resurser

När Azure-resurserna inte längre behövs rensar du de resurser som du har distribuerat genom att ta bort resursgruppen.

1. I Azure-portalen väljer du **Resursgrupp** på den vänstra menyn.
2. Ange resursgruppens namn i fältet **Filtrera efter namn**.
3. Välj resursgruppens namn.  Du bör se lagringskontot i resursgruppen.
4. Välj **Ta bort resursgrupp** på menyn längst upp.

## <a name="next-steps"></a>Nästa steg

I den här självstudien lärde du dig hur du genererar en mall från Azure-portalen och hur du distribuerar mallen med hjälp av portalen. Den mall som användes i den här snabbstarten är en enkel mall med en Azure-resurs. När mallen är komplex är det enklare att använda Visual Studio Code eller Visual Studio för att utveckla mallen. Nästa snabbstart visar också hur du distribuerar mallar med Azure PowerShell och Azure-kommandoradsgränssnittet (CLI).

> [!div class="nextstepaction"]
> [Skapa mallar med hjälp av Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md)
