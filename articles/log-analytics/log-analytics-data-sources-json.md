---
title: Samla in anpassade JSON-data i Log Analytics | Microsoft Docs
description: Anpassade JSON-datakällor kan samlas in i Log Analytics med Log Analytics-agenten för Linux.  Dessa anpassade datakällor kan vara enkla skript som returnerar JSON, till exempel curl eller någon av Fluentds över 300 plugin-program. Den här artikeln beskrivs den konfiguration som krävs för den här Datasamlingen.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 9e54a586ac20835078ae3a3734122b0db34ea69e
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51006761"
---
# <a name="collecting-custom-json-data-sources-with-the-log-analytics-agent-for-linux-in-log-analytics"></a>Samla in anpassade JSON-datakällor med Log Analytics-agenten för Linux i Log Analytics
[!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)]
Anpassade JSON-datakällor kan samlas in i Log Analytics med Log Analytics-agenten för Linux.  Dessa anpassade datakällor kan vara enkla skript som returnerar JSON som [curl](https://curl.haxx.se/) eller någon av [Fluentds över 300 plugin-program](http://www.fluentd.org/plugins/all). Den här artikeln beskrivs den konfiguration som krävs för den här Datasamlingen.

> [!NOTE]
> Log Analytics-agenten för Linux v1.1.0-217 + krävs för anpassad JSON-Data

## <a name="configuration"></a>Konfiguration

### <a name="configure-input-plugin"></a>Konfigurera inkommande plugin-programmet

Om du vill samla in JSON-data i Log Analytics, lägger du till `oms.api.` i början av en FluentD-tagg i en indata-plugin-programmet.

Till exempel följande är en separat konfigurationsfil `exec-json.conf` i `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`.  Här används plugin-programmet FluentD `exec` att köra ett curl-kommando med 30 sekunders mellanrum.  Utdata från det här kommandot samlas in av plugin-programmet för JSON-utdata.

```
<source>
  type exec
  command 'curl localhost/json.output'
  format json
  tag oms.api.httpresponse
  run_interval 30s
</source>

<match oms.api.httpresponse>
  type out_oms_api
  log_level info

  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms_api_httpresponse*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
```
Konfigurationsfilen har lagts till `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/` kräver för att ha ägarskapet ändrats med följande kommando.

`sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/exec-json.conf`

### <a name="configure-output-plugin"></a>Konfigurera utdata-plugin-programmet 
Lägg till följande konfiguration för utdata-plugin-programmet i den huvudsakliga konfigurationen av `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` eller som en separat konfigurationsfil placeras i `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`

```
<match oms.api.**>
  type out_oms_api
  log_level info

  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms_api*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
```

### <a name="restart-log-analytics-agent-for-linux"></a>Starta om Log Analytics-agenten för Linux
Starta om Log Analytics-agenten för Linux-tjänsten med följande kommando.

    sudo /opt/microsoft/omsagent/bin/service_control restart 

## <a name="output"></a>Resultat
Data kommer att samlas in i Log Analytics med en typ av post för `<FLUENTD_TAG>_CL`.

Till exempel anpassade taggar `tag oms.api.tomcat` i Log Analytics med en typ av post för `tomcat_CL`.  Du kan hämta alla poster i den här typen med följande loggsökning.

    Type=tomcat_CL

Kapslad JSON-data källor som stöds, men indexeras baserat på överordnad fältet. Till exempel följande JSON-data returneras från en Log Analytics-sökningen som `tag_s : "[{ "a":"1", "b":"2" }]`.

```
{
    "tag": [{
        "a":"1",
        "b":"2"
    }]
}
```


## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [loggsökningar](log-analytics-queries.md) att analysera data som samlas in från datakällor och lösningar. 
 
