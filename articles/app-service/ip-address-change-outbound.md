---
title: Så här förbereder du för en utgående IP-adressändring - Azure
description: Om din utgående IP-adress ska ändras, lär du dig vad du gör så att din app fortsätter att fungera efter ändringen.
services: app-service\web
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.topic: article
ms.date: 06/28/2018
ms.author: cephalin
ms.openlocfilehash: 2355fa5e3e7fd83b5d9f22d87c19f8152659fc97
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/13/2018
ms.locfileid: "49309368"
---
# <a name="how-to-prepare-for-an-outbound-ip-address-change"></a>Så här förbereder du för en utgående IP-adressändring

Om du har fått ett meddelande som ändrar utgående IP-adresserna för Azure App Service-appen följer du anvisningarna i den här artikeln.

## <a name="determine-if-you-have-to-do-anything"></a>Avgöra om du behöver göra något

* Alternativ 1: Om din App Service-app inte använder IP-filtrering, en explicit inkluderingslistan eller särskild hantering av utgående trafik som routning eller brandväggen, ingen åtgärd krävs.

* Alternativ 2: Om appen har särskild hantering för utgående IP-adresser (se exemplen nedan), lägga till nya utgående IP-adresser, oavsett var befintliga visas. Inte ersätta de befintliga IP-adresserna. Du hittar nya utgående IP-adresser genom att följa anvisningarna i nästa avsnitt.

  Till exempel en utgående IP-adress uttryckligen kan ingå i en brandvägg utanför din app eller en extern betalningstjänsten kan ha en lista med tillåtna som innehåller den utgående IP-adressen för din app. Om din utgående adress har konfigurerats i en lista var som helst utanför din app, som behöver ändras.

## <a name="find-the-outbound-ip-addresses-in-the-azure-portal"></a>Hitta de utgående IP-adresserna i Azure portal

Nya utgående IP-adresser visas i portalen innan de träder i kraft. När Azure startar med hjälp av nya används inte längre gamla. Endast en uppsättning i taget används, så att posterna i listor över måste ha både gamla och nya IP-adresser för att förhindra att ett avbrott när växeln händer. 

1.  Öppna [Azure-portalen](https://portal.azure.com).

2.  Välj i den vänstra navigeringsmenyn **Apptjänster**.

3.  Välj din App Service-app i listan.

4.  Om appen är en funktionsapp, se [funktionen app utgående IP-adresser](../azure-functions/ip-addresses.md#find-outbound-ip-addresses).

4.  Under den **inställningar** rubrik, klickar du på **egenskaper** i det vänstra navigeringsfönstret och hitta avsnittet märkta **utgående IP-adresser**.

5. Kopiera IP-adresser och lägga till dem i din särskild hantering av utgående trafik, till exempel ett filter eller listan över tillåtna. Ta inte bort de befintliga IP-adresserna i listan.

## <a name="next-steps"></a>Nästa steg

Den här artikeln beskrivs hur du förbereder en ändring av IP-adress som initierades av Azure. Mer information om IP-adresser i Azure App Service finns i [inkommande och utgående IP-adresser i Azure App Service](app-service-ip-addresses.md).
