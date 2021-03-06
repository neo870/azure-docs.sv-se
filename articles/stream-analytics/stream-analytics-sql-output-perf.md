---
title: Azure Stream Analytics-utdata till Azure SQL Database
description: Läs om skickar data till SQL Azure från Azure Stream Analytics och uppnå högre hastigheter för skrivning.
services: stream-analytics
author: chetang
ms.author: chetang
manager: katicad
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/21/2018
ms.openlocfilehash: 623d03c96866392ef245fb924cbf6600e7850ffe
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47058034"
---
# <a name="azure-stream-analytics-output-to-azure-sql-database"></a>Azure Stream Analytics-utdata till Azure SQL Database

Den här artikeln beskrivs tips för att uppnå bättre prestanda för skrivåtgärder dataflöde när du läser in data till SQL Azure Database med Azure Stream Analytics.

SQL-utdata i Azure Stream Analytics har stöd för skrivning parallellt som ett alternativ. Det här alternativet möjliggör [fullständigt parallella](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#embarrassingly-parallel-jobs) jobb-topologier, där flera partitioner i utdata skrivs till tabellen parallellt. Aktivera det här alternativet i Azure Stream Analytics kanske men inte är tillräckliga för att uppnå högre genomströmning, eftersom den avsevärt beror på din konfiguration för SQL Azure-databas och tabellschemat. Valet av index, klustring nyckel, fyllningsfaktor för index och komprimering påverka tid att läsa in tabeller. Läs mer om hur du optimerar dina SQL Azure database för att förbättra fråga och läsa in prestanda baserat på interna prestandamått [SQL database-prestandaråd](https://docs.microsoft.com/azure/sql-database/sql-database-performance-guidance).

Här följer några konfigurationer inom varje tjänst som bidrar till att förbättra hela dataflödet för din lösning.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

- **Ärv partitionering** – den här SQL utdata configuration alternativet aktiverar ärver partitioneringsschemat av din föregående frågesteg eller indata. Alternativet är aktiverat, skriver till en diskbaserad tabell och gör att en [fullständigt parallella](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#embarrassingly-parallel-jobs) topologi för ditt jobb förvänta dig att se bättre genomströmning. Partitionering redan automatiskt sker för många andra [matar ut](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#partitions-in-sources-and-sinks). Tabellen låsning (TABLOCK) har också inaktiverats för bulkinfogningar som gjorts med det här alternativet.

> [!NOTE] 
> När det finns fler än 8 inkommande partitioner, kanske ärver indata partitioneringsschema inte är det bästa valet. Den här maxgräns observerades i en tabell med en enda identitetskolumn och ett grupperat index. Baserat på schemat och val av index, kan din observationer variera.

- **Batchstorlek** -konfiguration av SQL-utdata kan du ange den maximala batchstorleken i Azure Stream Analytics SQL utdata baserat på typen av mål tabell/arbetsbelastningen. Batch är det maximala antalet poster som skickas med varje bulk insert transaktion. I klustrade columnstore-index, batch-storlekar runt [100 kB](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-data-loading-guidance) tillåter flera parallellisering, minimal loggning och låsa optimeringar. I diskbaserade tabeller vara 10K (standard) eller lägre optimala för din lösning som högre batch-storlekar kan utlösa Lås eskalering under bulkinfogningar.

- **Ange meddelande justering** – om du har optimerats med ärver partitionering och batch-storlek, ökar antalet inkommande händelser per meddelande per partition hjälper dig att skicka ytterligare upp genomströmning för skrivning. Indatameddelande justering kan batch-storlekar i Azure Stream Analytics vara upp till den angivna storleken för Batch, vilket ger bättre genomflöde. Detta kan uppnås med hjälp av [komprimering](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-define-inputs) eller större meddelande storlekar som är tillgängliga i Premium-SKU för EventHub.

## <a name="sql-azure"></a>SQL Azure

- **Partitionerade tabeller och index** – med hjälp av en [partitionerade](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes?view=sql-server-2017) kan avsevärt minska contentions mellan SQL-tabell och partitionerade index för tabellen med samma kolumn som din partitionsnyckel (till exempel PartitionId) partitioner under skrivåtgärder. För en partitionerad tabell måste du skapa en [partitionsfunktioner](https://docs.microsoft.com/sql/t-sql/statements/create-partition-function-transact-sql?view=sql-server-2017) och en [partitionsschema](https://docs.microsoft.com/sql/t-sql/statements/create-partition-scheme-transact-sql?view=sql-server-2017) på den primära filgruppen. Detta ökar också tillgängligheten för befintliga data medan nya data läses. Logg-i/o-begränsning kan träffa baserat på antalet partitioner som kan utökas genom att uppgradera SKU: N.

- **Undvika unika nyckelfel** – om du får [flera felet varningsmeddelanden](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-common-troubleshooting-issues#handle-duplicate-records-in-azure-sql-database-output) i aktivitetsloggen för Azure Stream Analytics, se till att jobbet inte påverkas av unik begränsning överträdelser som kan inträffa under återställning fall. Detta kan undvikas genom att ange den [IGNORERA\_Duplicera\_nyckel](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-common-troubleshooting-issues#handle-duplicate-records-in-azure-sql-database-output) alternativet på ditt index.

## <a name="azure-data-factory-and-in-memory-tables"></a>Azure Data Factory och InMemory-tabeller

- **InMemory-tabell som temporär tabell** – [InMemory-tabeller](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) tillåta för mycket hög hastighet Datainläsningen men data som ska få plats i minnet. Prestandamått show massinläsning från en InMemory tabell till en diskbaserad tabell är ungefär 10 gånger snabbare än direkt bulk infoga med en enda skrivare till diskbaserad-tabell med en identity-kolumn och ett grupperat index. Om du vill utnyttja det här bulk insert prestanda ställer du in en [kopieringsjobbet med Azure Data Factory](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-database) som kopierar data från InMemory-tabell till diskbaserad tabell.

## <a name="summary"></a>Sammanfattning

Sammanfattningsvis med partitionerade utdata-funktionen i Azure Stream Analytics för SQL-utdata justerade parallellisering för dina jobb med en partitionerad tabell i SQL Azure bör du få betydande dataflödet förbättringar. Utnyttjar Azure Data Factory för att samordna dataförflyttning från en InMemory-tabell i diskbaserade tabeller kan ge storleksordning genomflödesvinsterna. Om så är möjligt, kan förbättra meddelande densitet också vara en viktig faktor förbättra hela dataflödet.
