---
title: Hämta anslutningssträngen för Azure Event Hubs | Microsoft Docs
description: Hämta en anslutningssträng för Azure Event Hubs
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.date: 10/15/2018
ms.author: shvija
ms.openlocfilehash: bfc82f2dc280c3528f38c9cb466473a76328e552
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/08/2018
ms.locfileid: "51284970"
---
# <a name="get-an-event-hubs-connection-string"></a>Hämta en anslutningssträng för Event Hubs

Om du vill använda Event Hubs, måste du skapa ett namnområde för Event Hubs. Ett namnområde är en gemensam behållare som kan innehålla flera Händelsehubbar / Kafka-avsnitt. Det här namnområdet får du ett unikt [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name). När ett namnområde har skapats kan hämta du anslutningssträngen som krävs för att kommunicera med Event Hubs.

Anslutningssträngen för Azure Event Hubs har följande komponenter som är inbäddad i den,

* FQDN = det fullständiga Domännamnet för EventHubs-namnområdet som du skapade (Detta omfattar namnet för namnområdet EventHubs, följt av servicebus.windows.net)
* SharedAccessKeyName = namnet som du har valt för ditt programs SAS-nycklar
* SharedAccessKey = det genererade värdet för nyckeln.

Mallen anslutningssträngen ut
```
Endpoint=sb://<FQDN>/;SharedAccessKeyName=<KeyName>;SharedAccessKey=<KeyValue>
```

En exempel-anslutningssträng kan se ut `Endpoint=sb://dummynamespace.servicebus.windows.net/;SharedAccessKeyName=DummyAccessKeyName;SharedAccessKey=5dOntTRytoC24opYThisAsit3is2B+OGY1US/fuL3ly=`

Den här artikeln vägleder dig genom olika sätt för att hämta anslutningssträngen.

## <a name="get-connection-string-from-the-portal"></a>Hämta anslutningssträng från portalen

När du har Event Hubs-namnområdet kan översiktsavsnittet i portalen ge dig anslutningssträngen som visas nedan:

![Event Hubs anslutningssträng](./media/event-hubs-get-connection-string/event-hubs-get-connection-string1.png)

När du klickar på länken anslutning sträng i översiktsavsnittet öppnar fliken SAS-principer som du ser i bilden nedan:

![Händelsehubbar SAS-principer](./media/event-hubs-get-connection-string/event-hubs-get-connection-string2.png)

Du kan lägga till en ny SAS-princip och hämta anslutningssträngen eller använda den princip som redan har skapats för dig. När principen öppnas, anslutningssträngen hämtas enligt den nedan bild:

![Hämta anslutningssträngen för Händelsehubbar](./media/event-hubs-get-connection-string/event-hubs-get-connection-string3.png)

## <a name="getting-the-connection-string-with-azure-powershell"></a>Hämta anslutningssträngen med Azure PowerShell
Du kan använda Get-AzureRmEventHubNamespaceKey för att hämta anslutningssträngen för principregeln/ange namnet som visas nedan:

```azurepowershell-interactive
Get-AzureRmEventHubKey -ResourceGroupName dummyresourcegroup -NamespaceName dummynamespace -AuthorizationRuleName RootManageSharedAccessKey
```

Referera till [Azure Event Hubs PowerShell-modulen](https://docs.microsoft.com/powershell/module/azurerm.eventhub/get-azurermeventhubkey) för mer information.

## <a name="getting-the-connection-string-with-azure-cli"></a>Hämta anslutningssträngen med Azure CLI
Du kan använda följande för att hämta anslutningssträngen för namnområdet:

```azurecli-interactive
az eventhubs namespace authorization-rule keys list --resource-group dummyresourcegroup --namespace-name dummynamespace --name RootManageSharedAccessKey
```

Referera till [Azure CLI för Event Hubs](https://docs.microsoft.com/cli/azure/eventhubs) vill veta mer.

## <a name="next-steps"></a>Nästa steg

Du kan lära dig mer om Event Hubs genom att gå till följande länkar:

* [Event Hubs-översikt](event-hubs-what-is-event-hubs.md)
* [Skapa en händelsehubb](event-hubs-create.md)
