---
title: Exempeltopologier för Apache Storm i Azure HDInsight
description: En lista över exempeltopologier för Storm skapas och testas med Apache Storm i HDInsight, inklusive grundläggande C# och Java-topologier och arbeta med Event Hubs.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/30/2018
ms.openlocfilehash: 067065c887ecdac05fa15d897958d521ceb336cc
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51007033"
---
# <a name="example-storm-topologies-and-components-for-apache-storm-on-hdinsight"></a>Exempeltopologier för Storm och komponenter för Apache Storm på HDInsight

Här följer en lista över exempel skapas och underhålls av Microsoft för användning med Apache Storm på HDInsight. De här exemplen omfattar en mängd olika ämnen, från att skapa grundläggande C# och Java-topologier arbetat med Azure-tjänster, till exempel Händelsehubbar, Cosmos DB, SQL-databas, HBase på HDInsight och Azure Storage. Några exempel visar också hur du arbetar med Azure- eller även icke-Microsoft-tekniker, till exempel SignalR och Socket.IO.

| Beskrivning | Visar | / Ramverk för anspråksregelspråket |
|:--- |:--- |:--- |
| [Skriva till Azure Data Lake Store från Apache Storm](apache-storm-write-data-lake-store.md) |Skrivning till Azure Data Lake Store |Java |
| [Event Hub-kanalen och bult källa](https://github.com/apache/storm/tree/master/external/storm-eventhubs) |Källa för Event Hub-kanal och bult |Java |
| [Utveckla Java-baserade topologier för Apache Storm på HDInsight][5797064f] |Maven |Java |
| [Utveckla C#-topologier för Apache Storm på HDInsight med Visual Studio][16fce2d1] |HDInsight Tools för Visual Studio |C#, Java |
| [Bearbeta händelser från Azure Event Hubs med Storm på HDInsight (C#)][844d1d81] |Event Hubs |C# och Java |
| [Process events from Azure Event Hubs with Storm on HDInsight (Java)](https://azure.microsoft.com/resources/samples/hdinsight-java-storm-eventhub/) (Bearbeta händelser från Azure Event Hubs med Storm i HDInsight (Java)) |Event Hubs |Java |
| [Bearbeta fordonssensordata från Event Hubs med Storm på HDInsight][246ee964] |Event Hubs, Cosmos DB, Azure Storage-Blob (WASB) |C#, Java |
| [Extrahering, transformering och inläsning (ETL) från Azure Event Hubs till HBase, med Storm på HDInsight][b4b68194] |Event Hubs, HBase |C# |
| [Mallprojekt C# Storm-topologi för att arbeta med Azure-tjänster från Storm på HDInsight][ce0c02a2] |Event Hubs, Cosmos DB SQL-databas, HBase, SignalR |C#, Java |
| [Skalbarhet prestandamått för att läsa från Azure Event Hubs med Storm på HDInsight][d6c540e3] |Dataflöde för meddelanden, Event Hubs, SQL-databas |C#, Java |
| [Använd Python med Storm på HDInsight](apache-storm-develop-python-topology.md) |Python-komponenter med en topologi som |Python |
| [Använda Kafka med Storm på HDInsight](../hdinsight-apache-storm-with-kafka.md) | Apache Storm läsning och skrivning till Apache Kafka | Java |

> [!WARNING]
> C#-exempel i den här listan har ursprungligen skapats och testats med Windows-baserade HDInsight och maj som inte är fungerar korrekt med Linux-baserade HDInsight-kluster. Linux-baserade kluster använda Mono för att köra .NET-kod och kan ha kompatibilitetsproblem med ramverk och paket som används i det här exemplet.
>
> Linux är det enda operativsystem som används på HDInsight version 3.4 och senare.

### <a name="next-steps"></a>Nästa steg

* [Komma igång med Apache Storm i HDInsight][2b8c3488]
* [Lär dig att distribuera och hantera Storm-topologier med Storm på HDInsight][6eb0d3b8]

[2b8c3488]:apache-storm-tutorial-get-started-linux.md "Lär dig hur du skapar ett Storm på HDInsight-kluster och använda Storm-instrumentpanelen för att distribuera exempeltopologier."
[6eb0d3b8]:apache-storm-deploy-monitor-topology.md "Lär dig mer om att distribuera och hantera topologier med webbaserad Storm-instrumentpanelen och Storm-Användargränssnittet eller HDInsight-verktyg för Visual Studio."
[16fce2d1]:apache-storm-develop-csharp-visual-studio-topology.md "Lär dig mer om att skapa Storm-topologier med hjälp av HDInsight-verktyg för Visual Studio."
[5797064f]:apache-storm-develop-java-topology.md "Lär dig mer om att skapa Storm-topologier i Java, med hjälp av Maven genom att skapa en grundläggande wordcount-topologi."
[844d1d81]:apache-storm-develop-csharp-event-hub-topology.md "Lär dig mer om att läsa och skriva data från Azure Event Hubs med Storm på HDInsight."
[246ee964]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md "Lär dig hur du använder en Storm-topologi kan läsa meddelanden från Azure Event Hubs, läsa dokument från Azure Cosmos DB för refererar till data och spara data till Azure Storage."
[d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "Flera topologier att demonstrera dataflöde när läsning från Azure Event Hubs och lagra till SQL-databas som använder Apache Storm på HDInsight."
[b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Lär dig mer om att läsa data från Azure Event Hubs, sammanställd & Transformera data och sedan lagra den HBase på HDInsight."
[ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "Det här projektet innehåller mallar för kanaler och bultar topologier kan interagera med olika Azure-tjänster som Event Hubs och Cosmos DB SQL-databas."

