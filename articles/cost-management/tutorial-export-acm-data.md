---
title: Självstudie – Skapa och hantera exporterade data från Azure Cost Management | Microsoft Docs
description: Den här artikeln visar hur du kan skapa en hantera exporterade Azure Cost Management-data så att du kan använda dem i externa system.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/21/2018
ms.topic: tutorial
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 0765e79b90eed49742f5eead33063907eb1db1f4
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47030854"
---
# <a name="tutorial-create-and-manage-exported-data"></a>Självstudie: Skapa och hantera exporterade data

Om du har läst självstudien om kostnadsanalys är du bekant med att manuellt ladda ned Cost Management-data. Du kan dock skapa en dagligen återkommande aktivitet uppgift som automatiskt exporterar Cost Management-data till Azure Storage varje dag. Exporterade data är i CSV-format och innehåller all information som samlas in av Cost Management. Du kan sedan använda exporterade data i Azure Storage med externa system och kombinera dem med dina egna anpassade data. Och du kan använda din exporterade data i ett externt system, till exempel en instrumentpanel eller något annat ekonomisystem.

Exemplen i den här självstudien vägleder dig genom export av Cost Management-data och kontrollerar sedan att data har exporterats.

I den här guiden får du lära dig att:

> [!div class="checklist"]
> * Skapa en daglig export
> * Kontrollera att data samlas in

## <a name="prerequisites"></a>Nödvändiga komponenter

Dataexport är tillgängligt för alla [Enterprise-avtalskunder (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/). Följande Azure-behörigheter stöds per prenumeration för dataexport efter användare och grupp:

- Ägare – kan skapa, ändra eller ta bort schemalagda exporter för en prenumeration.
- Deltagare – kan skapa, ändra eller ta bort sina egna schemalagda exporter. Kan ändra namnet på schemalagda exporter som skapats av andra.
- Läsare – kan schemalägga exporter som läsaren har behörighet till.

För Azure Storage-konton:
- Det krävs skrivbehörigheter för att ändra det konfigurerade lagringskontot, oavsett behörigheter för exporten.
- Ditt Azure Storage-konto måste konfigureras för blob- eller fillagring.

## <a name="sign-in-to-azure"></a>Logga in på Azure
Logga in på Azure Portal på [https://portal.azure.com](https://portal.azure.com/).

## <a name="create-a-daily-export"></a>Skapa en daglig export

Cost Management + fakturering &gt; välj en prenumeration eller en resursgrupp i en prenumeration &gt; Exportera &gt; **Lägg till**.

Skriv ett namn för export och ange prenumeration, Azure Storage-konto, container och fillagringskatalog eller blog-container och klicka sedan på **Skapa**.

![Ny export](./media/tutorial-export-acm-data/new-export01.png)

Den nya exporten visas i listan över exporter. Som standard är nya exporter aktiverade, och de körs varje dag. Om du vill inaktivera eller ta bort en schemalagd export klickar du på valfritt objekt i listan och klickar sedan på antingen **Inaktivera** eller **Ta bort**.

Först kan det ta en till två timmar innan exporten körs. Dock kan det ta upp till fyra timmar innan data visas i de exporterade filerna.

## <a name="verify-that-data-is-collected"></a>Kontrollera att data samlas in

Du kan enkelt kontrollera att dina Cost Management-data samlas in och visa den exporterade CSV-filen med hjälp av Azure Storage Explorer.

I exportlistan klickar du på lagringskontots namn. På sidan för lagringskontot klickar du på Öppna i Explorer. Om du ser en bekräftelseruta klickar du på **Ja** för att öppna filen i Azure Storage Explorer.

![Sida för lagringskonto](./media/tutorial-export-acm-data/storage-account-page.png)

I Storage Explorer navigerar du till den container som du vill öppna och väljer den mapp som motsvarar aktuell månad. En lista med CSV-filer visas. Välj en och klicka sedan på **Öppna**.

![Storage Explorer](./media/tutorial-export-acm-data/storage-explorer.png)

Filen öppnas med det program eller den app som är inställt på att öppna CSV-filnamnstillägg. Här är ett exempel i Excel.

![Exempeldata för export](./media/tutorial-export-acm-data/example-export-data.png)

## <a name="access-exported-data-from-other-systems"></a>Komma åt exporterade data från andra system

Ett av syftena med att exportera dina Cost Management-data är att komma åt data från externa system. Du använder kanske ett instrumentpanelssystem eller något annat ekonomisystem. Sådana system varierar mycket, så det vore inte praktiskt att visa ett exempel.  Dock kan du komma igång med att få åtkomst till data från dina program i [Introduktion till Azure Storage](../storage/common/storage-introduction.md).

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen lärde du dig att:

> [!div class="checklist"]
> * Skapa en daglig export
> * Kontrollera att data samlas in

Gå vidare till nästa självstudie för att optimera och förbättra effektiviteten genom att identifiera inaktiva och underutnyttjade resurser.

> [!div class="nextstepaction"]
> [Granska och arbeta med optimeringsrekommendationer](tutorial-acm-opt-recommendations.md)
