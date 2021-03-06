---
title: Skapa och hantera Azure Machine Learning-tjänstens arbetsytor
description: Lär dig mer om att skapa, visa och ta bort Azure Machine Learning-tjänstens arbetsytor i Azure-portalen.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: shipatel
author: shivp950
ms.date: 09/24/2018
ms.openlocfilehash: 2d62e2688c6c17cc96798cb1a560808f6da9c6a9
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51242712"
---
# <a name="create-and-manage-azure-machine-learning-service-workspaces"></a>Skapa och hantera Azure Machine Learning-tjänstens arbetsytor

I den här artikeln får du skapa, visa och ta bort [ **Azure Machine Learning-tjänstens arbetsytor** ](concept-azure-machine-learning-architecture.md#workspace) i Azure-portalen för [Azure Machine Learning-tjänsten](overview-what-is-azure-ml.md).  Du kan också skapa och ta bort arbetsytor [med hjälp av CLI](reference-azure-machine-learning-cli.md) eller [med Python-koden](https://aka.ms/aml-sdk).

Du behöver en Azure-prenumeration för att skapa en arbetsyta. Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

## <a name="create-a-workspace"></a>Skapa en arbetsyta 

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="view-a-workspace"></a>Visa en arbetsyta

1. I det övre vänstra hörnet i portalen, väljer **alla tjänster**. 

1. I den **alla tjänster** filter och skriver **Machine Learning-tjänstens arbetsyta**.  

   ![Sök efter Azure Machine Learning-tjänstens arbetsyta](media/how-to-manage-workspace/allservices-search1.png)

1. Filtrera resultaten och välj **Machine Learning-tjänstens arbetsyta** att visa en lista över dina arbetsytor. 

   ![Sök efter Azure Machine Learning-tjänstens arbetsyta](media/how-to-manage-workspace/allservices-search.PNG)

1. Titta igenom listan över arbetsytor hittades. Du kan filtrera baserat på prenumerationen, resursgrupper och platser.  

   ![Azure Machine Learning-arbetsyta Tjänstlista](media/how-to-manage-workspace/allservices_view_workspace.PNG)

1. Välj den arbetsyta som du nyss skapade om du vill visa dess egenskaper.

   ![PNG](media/how-to-manage-workspace/allservices_view_workspace_full.PNG)

## <a name="delete-a-workspace"></a>Ta bort en arbetsyta

Använd knappen Ta bort överst på arbetsytan som du vill ta bort.

  ![PNG](media/how-to-manage-workspace/delete-workspace.png)


## <a name="clean-up-resources"></a>Rensa resurser 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Nästa steg

Följ den kompletta självstudiekursen lär du dig hur du använder en arbetsyta för att skapa, träna och distribuera modeller med Azure Machine Learning-tjänsten.

> [!div class="nextstepaction"]
> [Självstudie: Träna modeller](tutorial-train-models-with-aml.md)
