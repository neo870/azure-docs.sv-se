---
title: Prestandarekommendationer i Azure Database för PostgreSQL
description: Den här artikeln beskriver prestandarekommendationer kan komma in Azure Database för PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/26/2018
ms.openlocfilehash: 46a4e69ecb08276e12ccc197de2d3ad838628b78
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49378609"
---
# <a name="performance-recommendations-in-azure-database-for-postgresql"></a>Prestandarekommendationer i Azure Database för PostgreSQL

**Gäller för:** Azure Database för PostgreSQL 9.6 och 10

> [!IMPORTANT]
> Prestandarekommendationer finns i offentlig förhandsversion.

Funktionen Prestandarekommendationer identifierar de översta index som kan skapas i Azure Database for PostgreSQL-server för att förbättra prestanda. För att skapa indexrekommendationer beaktar funktionen olika egenskaper i databasen, inklusive dess schema och arbetsbelastningen som rapporterats av Query Store. När du implementerar en rekommendation för prestanda, bör kunderna testa prestanda för att utvärdera effekten av ändringarna. 

## <a name="permissions"></a>Behörigheter
**Ägare** eller **deltagare** behörigheter som krävs för att köra analys med funktionen Prestandarekommendationer.

## <a name="performance-recommendations"></a>Prestandarekommendationer
Den [Prestandarekommendationer](concepts-performance-recommendations.md) funktionen analyserar arbetsbelastningar mellan din server för att identifiera index med möjligheter att förbättra prestanda.

Öppna **Prestandarekommendationer** från den **Support och felsökning** delen av menyraden på sidan för Azure portal för din PostgreSQL-server.

![Prestandarekommendationer landningssida](./media/concepts-performance-recommendations/performance-recommendations-landing-page.png)

Välj **analysera** och välj en databas. Detta kommer att börja analysen. Det kan ta flera minuter att slutföra beroende på arbetsbelastningen. När analysen är klar, kommer det att ett meddelande i portalen.

Den **Prestandarekommendationer** i fönstret visas en lista över rekommendationer om några. En rekommendation visas information om den relevanta **databasen**, **tabell**, **kolumnen**, och **Indexstorlek**.

![Ny sida för prestanda-rekommendationer](./media/concepts-performance-recommendations/performance-recommendations-result.png)

Om du vill implementera rekommendationen kopiera frågetexten och köra den från din klient föredrar.

## <a name="next-steps"></a>Nästa steg
- Läs mer om [övervakning och justering](concepts-monitoring.md) i Azure Database för PostgreSQL.

