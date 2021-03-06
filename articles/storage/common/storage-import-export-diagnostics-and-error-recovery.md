---
title: Diagnostik och felåterställning för Azure Import/Export-jobb | Microsoft Docs
description: Lär dig hur du aktiverar utförlig loggning för Microsoft Azure Import/Export service jobb.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: 2a54752f933b91265d0aa8add61ca0707615931b
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/06/2018
ms.locfileid: "39526335"
---
# <a name="diagnostics-and-error-recovery-for-azure-importexport-jobs"></a>Diagnostik och felåterställning för Azure Import/Export-jobb
Tjänsten Azure Import/Export skapar en fellogg i det associerade lagringskontot för varje enhet som bearbetas. Du kan också aktivera utförlig loggning genom att ange den `LogLevel` egenskap `Verbose` när du anropar den [placera jobbet](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) eller [uppdatering jobbegenskaper](/rest/api/storageimportexport/jobs#Jobs_Update) åtgärder.

 Som standard, loggarna skrivs till en behållare med namnet `waimportexport`. Du kan ange ett annat namn genom att ange den `DiagnosticsPath` egenskapen när du anropar den `Put Job` eller `Update Job Properties` åtgärder. Loggfilerna lagras som blockblobar med följande namngivningskonvention: `waies/jobname_driveid_timestamp_logtype.xml`.

 Du kan hämta URI för loggar för ett jobb genom att anropa den [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) igen. URI för utförliga loggen returneras i de `VerboseLogUri` egenskapen för varje enhet medan URI för felloggen returneras i de `ErrorLogUri` egenskapen.

Du kan använda loggningsdata för att identifiera följande problem.

## <a name="drive-errors"></a>Enhet-fel

Följande objekt är klassificerade som enheten fel:

-   Fel i åtkomst till eller läsa manifestfilen

-   Felaktig BitLocker-nycklar

-   Enheten skrivning/fel

## <a name="blob-errors"></a>BLOB-fel

Följande objekt är klassificerade som blob-fel:

-   Felaktig eller motstridig blob eller namn

-   Filer som saknas

-   Det gick inte att hitta BLOB

-   Trunkerade filer (filer på disken är mindre än vad som angetts i manifestet)

-   Skadad fil innehåll (för importjobb som har identifierats med en felaktig matchning av MD5 kontrollsumma)

-   Skadat blob-metadata och egenskapen-filer som (identifieras med en felaktig matchning av MD5 kontrollsumma)

-   Felaktigt schema för blobegenskaper och/eller -metadatafiler

Det kan finnas fall där vissa delar av ett import eller export-jobb inte slutförs, medan övergripande jobbet slutförs fortfarande. I det här fallet du kan antingen ladda upp eller hämta saknade delar av data över nätverket, eller skapa ett nytt jobb för att överföra data. Se den [referensguiden för Azure Import/Export-verktyget](storage-import-export-tool-how-to-v1.md) information om hur du reparera data över nätverket.

## <a name="next-steps"></a>Nästa steg

* [Med hjälp av REST-API för Import/Export-tjänsten](storage-import-export-using-the-rest-api.md)
