---
title: Partitionering och skalning i Azure Cosmos DB | Microsoft Docs
description: Lär dig mer om hur partitionering fungerar i Azure Cosmos DB, hur du konfigurerar partitionering och partitions-nycklar och hur du väljer rätt Partitionsnyckeln för ditt program.
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: na
ms.topic: conceptual
ms.date: 05/24/2017
ms.author: rafats
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7e3c15ce201cee88d400b9d5fb9272b584293a2f
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34797443"
---
# <a name="partitioning-in-azure-cosmos-db-using-the-sql-api"></a>Partitionering i Azure Cosmos-databasen med SQL-API

[Microsoft Azure Cosmos DB](../cosmos-db/introduction.md) är en global distribuerad databas med flera modeller tjänst som hjälper dig uppnå snabb och förutsägbar prestanda och skalning sömlöst tillsammans med ditt program som de växer. 

Den här artikeln innehåller en översikt över hur du arbetar med partitionering av Cosmos-DB-behållare med SQL-API. Se [partitionering och teckenbredden](../cosmos-db/partition-data.md) en översikt över begrepp och bästa praxis för partitionering med Azure Cosmos DB API: er. 

Hämta projektet från att komma igång med kod [Github](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark). 

När du har läst den här artikeln kommer du att kunna svara på följande frågor:   

* Hur fungerar partitionering arbete i Azure Cosmos DB?
* Hur konfigurerar jag partitionering i Azure Cosmos DB
* Vad är partitionsnycklar och hur jag välja rätt Partitionsnyckeln för Mina program?

Hämta projektet från att komma igång med kod [Azure Cosmos DB prestanda testa drivrutinen Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark). 

<!-- placeholder until we have a permanent solution-->
<a name="partition-keys"></a>
<a name="single-partition-and-partitioned-collections"></a>
<a name="migrating-from-single-partition"></a>

## <a name="partition-keys"></a>Partitionsnycklar

Ange partition viktiga definition i form av en JSON-sökvägen i SQL-API. I följande tabell visas exempel på partitionen viktiga definitioner och de värden som motsvarar varje. Partitionsnyckeln har angetts som en sökväg, till exempel `/department` representerar egenskapen avdelning. 

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Partitionsnyckeln</strong></p></td>
            <td valign="top"><p><strong>Beskrivning</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>/Department</p></td>
            <td valign="top"><p>Motsvarar värdet för doc.department där doc är objektet.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ Egenskaper/namn</p></td>
            <td valign="top"><p>Motsvarar värdet för doc.properties.name där doc är objekt (kapslade egenskap).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ID</p></td>
            <td valign="top"><p>Motsvarar värdet för doc.id (id och partitions nyckel är samma egenskap).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ ”avdelningsnamn”</p></td>
            <td valign="top"><p>Motsvarar värdet för doc [”” avdelningsnamn] där doc är objektet.</p></td>
        </tr>
    </tbody>
</table>

> [!NOTE]
> Syntaxen för partitionsnyckel liknar sökvägen för indexering princip sökvägar med den viktigaste skillnaden sökvägen motsvarar egenskapen i stället för värdet, dvs. det finns inga jokertecken i slutet. Exempelvis anger du/avdelning /? Om du vill indexera värden under avdelning, men ange /department som definitionen för partitionen. Partitionsnyckeln är implicit indexerad och kan inte uteslutas från att indexera med indexering princip åsidosättningar.
> 
> 

Nu ska vi titta på hur valet av partitionsnyckel påverkar prestanda för ditt program.

## <a name="working-with-the-azure-cosmos-db-sdks"></a>Arbeta med Azure Cosmos DB-SDK
Azure Cosmos-DB tillagt stöd för automatisk partitionering med [REST API-version 2015-12-16](/rest/api/cosmos-db/). För att kunna skapa partitionerade behållare, måste du hämta SDK-versioner 1.6.0 eller senare på en av SDK plattformar som stöds (.NET, Node.js, Java, Python, MongoDB). 

### <a name="creating-containers"></a>Skapa behållare
I följande exempel visas en .NET-fragment för att skapa en behållare för att lagra telemetri enhetsdata för 20 000 frågeenheter per sekund genomströmning. SDK anger OfferThroughput-värde (som i sin tur anger den `x-ms-offer-throughput` huvudet i begäran i REST-API). Här anger du den `/deviceId` som partitionsnyckel. Valet av partitionsnyckel sparas tillsammans med resten av metadata för behållaren som namn och indexprincip.

För det här exemplet du valt `deviceId` eftersom du vet att (a) det finns ett stort antal enheter, skriver kan vara jämnt över partitioner och så att du kan skala databasen om du vill föra in stora mängder data och (b) många förfrågningar som hämtar den senaste avläsningen för en enhet är begränsade till en enda deviceId och kan hämtas från en enda partition.

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
await client.CreateDatabaseAsync(new Database { Id = "db" });

// Container for device telemetry. Here the property deviceId will be used as the partition key to 
// spread across partitions. Configured for 10K RU/s throughput and an indexing policy that supports 
// sorting against any number or string property.
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 20000 });
```

Den här metoden gör en REST-API-anrop för att Cosmos-DB och tjänsten etablerar ett antal partitioner baserat på det begärda genomflödet. Du kan ändra genomflödet av en behållare eller en uppsättning behållare prestandan behov utvecklas. 

### <a name="reading-and-writing-items"></a>Läsning och skrivning av objekt
Nu ska vi infoga data i Cosmos DB. Här är ett exempel på klass som innehåller en enhet som läser och ett anrop till CreateDocumentAsync att infoga en ny enhet som läses in en behållare. Följande är ett exempel kodblock som utnyttjar SQL-API:

```csharp
public class DeviceReading
{
    [JsonProperty("id")]
    public string Id;

    [JsonProperty("deviceId")]
    public string DeviceId;

    [JsonConverter(typeof(IsoDateTimeConverter))]
    [JsonProperty("readingTime")]
    public DateTime ReadingTime;

    [JsonProperty("metricType")]
    public string MetricType;

    [JsonProperty("unit")]
    public string Unit;

    [JsonProperty("metricValue")]
    public double MetricValue;
  }

// Create a document. Here the partition key is extracted as "XMS-0001" based on the collection definition
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new DeviceReading
    {
        Id = "XMS-001-FE24C",
        DeviceId = "XMS-0001",
        MetricType = "Temperature",
        MetricValue = 105.00,
        Unit = "Fahrenheit",
        ReadingTime = DateTime.UtcNow
    });
```

Vi läsa objekt av sin partitionsnyckel och id, uppdatera och som ett sista steg kan ta bort den av partitionsnyckel och ID: t. Läsningar innehåller ett PartitionKey-värde (som motsvarar den `x-ms-documentdb-partitionkey` huvudet i begäran i REST-API).

```csharp
// Read document. Needs the partition key and the ID to be specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;

// Update the document. Partition key is not required, again extracted from the document
reading.MetricValue = 104;
reading.ReadingTime = DateTime.UtcNow;

await client.ReplaceDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  reading);

// Delete document. Needs partition key
await client.DeleteDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

### <a name="querying-partitioned-containers"></a>Frågar partitionerade behållare
När du begär data i partitionerade behållare dirigerar Cosmos DB automatiskt frågan till partitioner som motsvarar de värdena för partitionen som angetts i filtret (om det finns några). Den här frågan dirigeras till exempel enbart till den partition som innehåller partitionsnyckeln XMS-0001.

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
Följande fråga har inget filter på partitionsnyckeln (DeviceId) och är utspridd till alla partitioner där den körs mot partitionens index. Du måste ange EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` i REST-API) har SDK, för att köra en fråga i partitioner.

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

Har stöd för cosmos DB [mängdfunktionerna](sql-api-sql-query.md#Aggregates) `COUNT`, `MIN`, `MAX`,, och `AVG` över partitionerad behållare med SQL som börjar med SDK: er 1.12.0 och högre. Frågor måste innehålla en sammanställd operator och måste innehålla ett värde i projektionen.

### <a name="parallel-query-execution"></a>Parallell frågekörning
SDK: er 1.9.0 för Cosmos DB och högre support parallell körning frågealternativ, så att du kan utföra låg latens frågor mot partitionerade samlingar, även när de behöver touch ett stort antal partitioner. Följande fråga är till exempel konfigurerad för att köras parallellt över partitioner.

```csharp
// Cross-partition Order By Queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
Du kan hantera parallell frågekörning genom att justera följande parametrar:

* Genom att ange `MaxDegreeOfParallelism`, du kan styra i vilken grad av parallellitet som är maximalt antal samtidiga anslutningar till behållarens partitioner. Om du anger egenskapen 1 hanteras grad av parallellitet av SDK. Om den `MaxDegreeOfParallelism` är inte angiven eller ange värdet 0, vilket är standardvärdet, finns en nätverksanslutning till behållarens partitioner.
* Genom att ange `MaxBufferedItemCount`, kan du avväga frågesvarstider och minnesanvändning på klientsidan. Om du utelämnar den här parametern eller ange egenskapen till -1, hanteras antal objekt som buffras under parallell frågekörning av SDK.

Med samma status för samlingen, returnerar en parallell fråga resultat i samma ordning som vid seriell körning. När du utför fråga cross-partition som innehåller sortering (ORDER BY och/eller TOP), Azure Cosmos DB SDK skickar fråga parallellt över partitioner och sammanfogar delvis sorterade resulterar i klientsidan inga globalt beställda resultat.

### <a name="executing-stored-procedures"></a>Köra lagrade procedurer
Du kan också köra atomiska transaktioner mot dokument med samma enhets-ID till exempel om du underhålla mängdfunktioner eller det aktuella tillståndet för en enhet i ett enskilt objekt. 

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```
   
I nästa avsnitt titta på hur du kan flytta till partitionerade behållare från enskild partition behållare.

## <a name="next-steps"></a>Nästa steg
Den här artikeln som en översikt över hur du arbetar med partitionering av Azure Cosmos DB behållare med SQL-API. Se även [partitionering och teckenbredden](../cosmos-db/partition-data.md) en översikt över begrepp och bästa praxis för partitionering med Azure Cosmos DB API: er. 

* Utföra skalnings- och prestandatester med Azure Cosmos DB. Se [prestanda och Skalningstester med Azure Cosmos DB](performance-testing.md) ett exempel.
* Komma igång med programmeringen med den [SDK](sql-api-sdk-dotnet.md) eller [REST API](/rest/api/cosmos-db/)
* Lär dig mer om [etablerat dataflöde i Azure Cosmos DB](request-units.md)

