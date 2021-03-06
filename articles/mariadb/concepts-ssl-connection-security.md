---
title: SSL-anslutning för Azure Database for MariaDB
description: Information för att konfigurera Azure Database for MariaDB och associerade program att använda normalt SSL-anslutningar
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 1d0b27a8fd7e3882a73624fa1b668ac602a85e6b
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/03/2018
ms.locfileid: "48249510"
---
# <a name="ssl-connectivity-in-azure-database-for-mariadb"></a>SSL-anslutning i Azure Database for MariaDB
Azure Database for MariaDB stöder anslutning av din databasserver för klientprogram som använder Secure Sockets Layer (SSL). Framtvingande av SSL-anslutningar mellan databasservern och klientprogrammen hjälper till att skydda mot ”man in the middle”-attacker genom att kryptera dataströmmen mellan servern och programmet.

## <a name="default-settings"></a>Standardinställningar
Som standard ska databastjänsten konfigureras för att kräva SSL-anslutningar när du ansluter till MariaDB.  Vi rekommenderar att undvika att inaktivera SSL-alternativet när det är möjligt.

När du etablerar en ny Azure-databas för MariaDB-servern via Azure portal och CLI är tillämpning av SSL-anslutningar aktiverat som standard.

Anslutningssträngar för olika programmeringsspråk visas i Azure-portalen. Dessa anslutningssträngar omfattar de obligatoriska SSL-parametrarna för att ansluta till databasen. Välj din server i Azure-portalen. Under den **inställningar** väljer du den **anslutningssträngar**. SSL-parametern varierar beroende på anslutningen, till exempel ”ssl = true” eller ”sslmode = kräver” eller ”sslmode = krävs” eller andra ändringar.

Om du vill lära dig mer om att aktivera eller inaktivera SSL-anslutning när du utvecklar program, som avser [hur du konfigurerar SSL](howto-configure-ssl.md).

## <a name="next-steps"></a>Nästa steg
- Läs mer om [serverbrandväggsregler](concepts-firewall-rules.md)
- Lär dig hur du [Konfigurera SSL](howto-configure-ssl.md).
