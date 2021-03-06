---
title: Konfigurera målmiljön för haveriberedskap för lokala fysiska servrar till Azure | Microsoft Docs
description: Den här artikeln beskriver hur du ställer in mål Azure-miljön för haveriberedskap för fysiska servrar med Azure Site Recovery.
author: bsiva
manager: abhemraj
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: bsiva
ms.openlocfilehash: b89d04a6e2fd11a61de8b56690664f6204c208ad
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50209300"
---
# <a name="prepare-target-vmware-to-azure"></a>Förbered mål (VMware till Azure)

Den här artikeln beskriver hur du förbereder Azure-miljön att börja replikera fysiska servrar (x 64) som kör Windows eller Linux till Azure.

## <a name="prerequisites"></a>Förutsättningar

Artikeln förutsätter:
- Du har skapat ett Recovery Services-valv för att skydda din fysiska servrar. Du kan skapa ett Recovery Services-valv från den [Azure-portalen](http://portal.azure.com "Azure-portalen").
- Du har [konfigurering av din lokala miljö](physical-azure-disaster-recovery.md) att replikera fysiska servrar till Azure.

## <a name="prepare-target"></a>Förbered mål

När du har slutfört den **steg 1: Välj skyddsmål** och **steg 2: Förbered källa**, kommer du till **steg3: mål**

![Förbered mål](./media/physical-azure-set-up-target/prepare-target-physical-to-azure.png)

1. **Prenumeration:** från den nedrullningsbara menyn, Välj den prenumeration som du vill replikera dina fysiska servrar till.
2. **Distributionsmodell:** Välj distributionsmodell (klassisk eller Resource Manager)

Baserat på den valda distributionsmodellen, utförs en verifiering för att säkerställa att du har minst ett kompatibelt lagringskonto och virtuellt nätverk i målprenumerationen för replikering och redundans din fysiska servrar till.

När verifieringar slutförs korrekt, klickar du på OK om du vill gå till nästa steg.

Om du inte har en kompatibel Resource Manager-konto eller ett virtuellt nätverk kan du skapa en genom att klicka på den **+ Lagringskonto** eller **+ nätverk** knappar längst upp på sidan.

## <a name="next-steps"></a>Nästa steg
[Konfigurera replikeringsinställningar](vmware-azure-set-up-replication.md).
