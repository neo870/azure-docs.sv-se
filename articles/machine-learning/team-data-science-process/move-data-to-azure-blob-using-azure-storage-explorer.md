---
title: Flytta Data till och från Blob Storage med Azure Storage Explorer | Microsoft Docs
description: Flytta data till och från Azure Blob Storage med Azure Storage Explorer
services: machine-learning,storage
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 10bd283f-0875-4c67-af63-6492270b7656
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: deguhath
ms.openlocfilehash: 295fadd69a86769f74ec45ed3e77e8a5b807e14f
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49392530"
---
# <a name="move-data-to-and-from-azure-blob-storage-using-azure-storage-explorer"></a>Flytta data till och från Azure Blob Storage med Azure Storage Explorer
Azure Storage Explorer är ett kostnadsfritt verktyg från Microsoft som gör det möjligt att arbeta med Azure Storage-data i Windows, macOS och Linux. Det här avsnittet beskriver hur du använder den för att ladda upp och hämta data från Azure blob storage. Verktyget kan laddas ned från [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/).

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Om du använder VM som konfigurerats med skript som tillhandahålls av [Data Science virtuella datorer i Azure](virtual-machines.md), och sedan Azure Storage Explorer är redan installerad på den virtuella datorn.
> 
> [!NOTE]
> En fullständig introduktion till Azure blob storage finns [grunderna i Azure Blob](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) och [Azure Blob-tjänsten](https://msdn.microsoft.com/library/azure/dd179376.aspx).   
> 
> 

## <a name="prerequisites"></a>Förutsättningar
Det här dokumentet förutsätter att du har en Azure-prenumeration, ett lagringskonto och motsvarande lagringskontots åtkomstnyckel för kontot. Innan du laddar upp/ned data, vet du ditt Azure storage-konto och kontonyckeln. 

* Om du vill konfigurera en Azure-prenumeration, se [kostnadsfri utvärderingsmånad](https://azure.microsoft.com/pricing/free-trial/).
* Mer information om hur du skapar ett lagringskonto och för att hämta konto och viktig information, se [om Azure storage-konton](../../storage/common/storage-create-storage-account.md). Skriv ner åtkomstnyckeln för ditt lagringskonto som du behöver den här nyckeln för att ansluta till kontot med Azure Storage Explorer-verktyget.
* Azure Storage Explorer-verktyget kan laddas ned från [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/). Acceptera standardinställningarna under installationen.

<a id="explorer"></a>

## <a name="use-azure-storage-explorer"></a>Använda Azure Storage Explorer
Följande steg beskrivs hur du uppladdning/nedladdning data med hjälp av Azure Storage Explorer. 

1. Starta Microsoft Azure Storage Explorer.
2. Att ta fram den **logga in på ditt konto...**  väljer **Azure kontoinställningar** ikonen och sedan **Lägg till ett konto** och anger du autentiseringsuppgifter. ![Lägg till ett Azure storage-konto](./media/move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3. Att ta fram den **Anslut till Azure Storage** guiden väljer du den **Anslut till Azure storage** ikon. ![Anslut till Azure storage](./media/move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Ange åtkomstnyckel från Azure storage-kontot den **Anslut till Azure Storage** guiden och sedan **nästa**. ![Anslut till Azure storage](./media/move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. Ange lagringskontonamn i den **kontonamn** rutan och välj sedan **nästa**. ![Anslut extern lagring](./media/move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. Storage-konto som har lagts till bör nu visas. Skapa en blob-behållare i ett lagringskonto, högerklicka på den **Blob-behållare** nod i kontot, Välj **skapa Blobbehållare**, och ange ett namn.
7. Välj en målbehållare för att ladda upp data till en behållare, och klicka på den **överför** knappen.![ Storage-konton](./media/move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. Klicka på den **...**  till höger om den **filer** väljer du en eller flera filer att ladda upp från filsystemet och klicka på **överför** att påbörja uppladdningen av filerna.![ Ladda upp filer](./media/move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
9. Att hämta data, att välja blob i behållaren motsvarande för att ladda ned och klicka på **hämta**. ![Ladda ned filer](./media/move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)

