---
title: Azure Media Services – signalering av Metadata med tidsgräns i liveuppspelning | Microsoft Docs
description: Den här specifikationen beskrivs två lägen som stöds av Media Services för signaling tidsinställd metadata i direktsänd strömning. Detta inkluderar stöd för allmän tidsinställd metadata signaler, samt SCTE 35 signalering för ad splice infogning.
services: media-services
documentationcenter: ''
author: cenkdin
manager: cfowler
editor: johndeu
ms.assetid: 265b94b1-0fb8-493a-90ec-a4244f51ce85
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2018
ms.author: johndeu;
ms.openlocfilehash: 827153300b9cab4ea805689b1e103bea1b334ec9
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51249582"
---
# <a name="signaling-timed-metadata-in-live-streaming"></a>Signalering av Metadata med tidsgräns i liveuppspelning


## <a name="1-introduction"></a>1 introduktion 
För att underlätta insättning av annonser eller anpassade händelser på en klient-spelare, sändningsföretag ofta Kontrollera användning av tidsinställd metadata är inbäddad i videon. Om du vill aktivera dessa scenarier, har Media Services stöd för transport av tidsinställd metadata tillsammans med media från det datum då inmatning för live direktuppspelning kanalen till klientprogrammet.
Den här specifikationen beskrivs två lägen som stöds av Media Services för tidsinställd metadata i live direktuppspelning signaler:

1. [SCTE 35] signalering som heeds rekommenderade metoder som beskrivs av [SCTE 67]

2. En allmän tidsgränsen metadata signalering läge för meddelanden som inte [SCTE 35]

### <a name="12-conformance-notation"></a>1.2 efterlevnadsstatus Notation
Nyckeln orden ”måste”, är ”måste inte”, ”obligatoriskt”, ”skall”, ”skall inte”, ”SHOULD”, ”bör inte”, ”rekommenderade”, ”MAY” och ”valfritt” i det här dokumentet att tolkas som beskrivs i RFC 2119

### <a name="13-terms-used"></a>1.3-termer som används

| Period              | Definition                                                                                                                                                                                                                       |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Presentation-tid | Den tid som en händelse presenteras för ett visningsprogram. Tiden representerar den tidpunkt då på tidslinjen för media som ett visningsprogram ser händelsen. Till exempel är presentation tidpunkten för ett meddelande om SCTE 35 splice_info() kommandot splice_time(). |
| Ankomsttid      | Den tid som ett meddelande anländer. Tiden är vanligtvis separata från dess presentation av händelsen, eftersom händelsemeddelanden skickas förväg presentation av händelsen.                                     |
| Null-optimerade spåra      | Media spåra som inte är kontinuerlig och synkroniseras tid med en över- eller spåra.                                                                                                                                    |
| Ursprung            | Tjänsten Azure Media-direktuppspelning                                                                                                                                                                                                |
| Kanalmottagare      | Azure Media Live Direktuppspelningstjänst                                                                                                                                                                                           |
| HLS               | Apple HTTP Live Streaming-protokollet                                                                                                                                                                                               |
| STRECK              | Dynamisk anpassningsbar strömning via HTTP                                                                                                                                                                                             |
| Smidig            | Smidig strömningsprotokoll                                                                                                                                                                                                        |
| MPEG2-TS          | Strömmar för MPEG-2 Transport                                                                                                                                                                                                         |
| RTMP              | I realtid Multimedia-protokollet                                                                                                                                                                                                    |
| uimsbf            | -Bitars heltal utan tecken, viktigaste först.                                                                                                                                                                                    |

-----------------------

## <a name="2-timed-metadata-ingest"></a>Mata in 2 tidsinställd Metadata
## <a name="21-rtmp-ingest"></a>2.1 RTMP mata in
RTMP stöder tidsinställd metadata signaler som skickas som AMF stack-meddelanden som är inbäddad i RTMP-dataströmmen. Låt dig ledas meddelanden kan skickas en stund innan den faktiska händelsen eller ad-insättning splice måste ske. För att stödja det här scenariot, skickas den faktiska tiden splice eller segment i bota meddelandet. Mer information finns i [AMF0].

I följande tabell beskrivs formatet för AMF meddelande nyttolasten som Media Services kommer att mata in.

Namnet på AMF meddelandet kan användas för att skilja flera händelseströmmar av samma typ.  För [SCTE 35] måste namnet på AMF meddelandet vara ”onAdCue” enligt rekommendationerna i [SCTE 67].  Alla fält som inte anges nedan måste ignoreras så att innovationer i den här designen inte är inaktiv i framtiden.

### <a name="signal-syntax"></a>Signal-Syntax
För RTMP enkelt läge stöder Media Services ett enda AMF stack-meddelande som kallas ”onAdCue” med följande format:

### <a name="simple-mode"></a>Enkelt läge

| Fältnamn | Fälttyp | Krävs? | Beskrivningar                                                                                                             |
|------------|------------|----------|--------------------------------------------------------------------------------------------------------------------------|
| Låt dig ledas        | Sträng     | Krävs | Händelsemeddelandet.  Vara skall ”SpliceOut” för att ange ett enkelt läge splice.                                              |
| id         | Sträng     | Krävs | En unik identifierare som beskriver splice eller segment. Identifierar den här instansen av meddelandet                            |
| Varaktighet   | Tal     | Krävs | Varaktigheten för splice. Enheterna är fraktionell.                                                                |
| elapsed    | Tal     | Valfri | När signalen upprepas för att stödja Kolla in, det här fältet ska vara presentation tiden som har förflutit sedan splice påbörjades. Enheterna är fraktionell. När du använder enkelt läge måste får det här värdet inte överstiga splice ursprungliga varaktighet.                                                  |
| time       | Tal     | Krävs | Skall vara tidpunkten för splice, tidpunkt för presentation. Enheterna är fraktionell.                                     |

---------------------------

### <a name="scte-35-mode"></a>SCTE 35 läge

| Fältnamn | Fälttyp | Krävs? | Beskrivningar                                                                                                             |
|------------|------------|----------|--------------------------------------------------------------------------------------------------------------------------|
| Låt dig ledas        | Sträng     | Krävs | Händelsemeddelandet.  För [SCTE 35] meddelanden måste vara base64 (IETF RFC 4648) binär kodade splice_info_section() för meddelanden som ska skickas till HLS, Smooth och Dash klienter i enlighet med [SCTE 67].                                              |
| typ       | Sträng     | Krävs | En URN eller en URL som anger meddelande-schemat. till exempel ”urn: exempel: signalering: 1.0”.  Detta måste vara ”urn: scte:scte35:2013a:bin” för att meddelanden ska skickas till HLS, Smooth och Dash klienter i enlighet med [SCTE 67] för [SCTE 35].  |
| id         | Sträng     | Krävs | En unik identifierare som beskriver splice eller segment. Identifierar den här instansen av meddelandet.  Meddelanden med motsvarande semantik bör ha samma värde.|
| Varaktighet   | Tal     | Krävs | Varaktigheten för händelsen eller ad splice-segmentet, om det är möjligt. Om det är okänd, vara värdet 0.                                                                 |
| elapsed    | Tal     | Valfri | Om ad-signal [SCTE 35] upprepas för att Kolla in och vara det här fältet presentation tiden som har förflutit sedan splice påbörjades. Enheterna är fraktionell. Det här värdet får överstiga den ursprungliga angivna varaktigheten splice eller segment i [SCTE 35]-läge.                                                  |
| time       | Tal     | Krävs | Presentation-tid för händelsen eller ad splice.  Presentation starttid och varaktighet ska justeras med Stream åtkomst punkter (SAP) av typen 1 eller 2, enligt definitionen i [ISO-14496-12] bilaga. Tid och varaktighet ska justeras med segment gränser för HLS utgående data. Presentation-tid och varaktighet för olika händelsemeddelanden inom samma händelseströmmen får inte överlappa varandra. Enheterna är fraktionell.

---------------------------

#### <a name="211-cancelation-and-updates"></a>2.1.1 säger upp avtalet och uppdateringar

Meddelanden kan har avbrutits eller uppdateras genom att skicka flera meddelanden med samma presentation tid och -ID. Presentation-tid och ID identifiera unikt händelsen och det sista meddelandet som togs emot för en specifik presentation tid som uppfyller före distributionen begränsningar är meddelandet som agerar på. Den uppdaterade händelsen ersätter alla tidigare mottagna meddelanden. Före distributionen begränsningen är fyra sekunder. Meddelanden som tas emot minst fyra sekunder innan presentation W3wp.

## <a name="22-fragmented-mp4-ingest-smooth-streaming"></a>2.2 inmatning av fragmenterad MP4 (Smooth Streaming)
Se [LIVE-FMP4] för inmatning av krav på live stream. Följande avsnitt innehåller information om hämtning av metadata för tidsinställd presentation.  Tidsinställd presentation metadata matas in som ett null-optimerade spår, som definieras i båda Live Manifest rutan Server (se MS-SSTR) och rutan film (moov).  Varje sparse fragment som består av en film Fragment (moof) och Media Data (”mdat”), där rutan ”mdat” är det binära meddelandet.

#### <a name="221-live-server-manifest-box"></a>2.2.1 Realtidsserver Manifest Box
Null-optimerade spåra måste deklareras i rutan Live Server Manifest med en \<textstream\> posten och måste innehålla de följande attribut:

| **Attributnamn** | **Fälttyp** | **Krävs?** | **Beskrivning**                                                                                                                                                                                                                                                 |
|--------------------|----------------|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| systemBitrate      | Tal         | Krävs      | MÅSTE vara ”0”, som anger ett spår med okänd, variabel bithastighet.                                                                                                                                                                                                 |
| parentTrackName    | Sträng         | Krävs      | MÅSTE vara namnet på den överordnade kurs, som är null-optimerade spåra tidskoderna tidsskalan justerad. Den överordnade-kursen får inte vara ett null-optimerade spår.                                                                                                                    |
| manifestOutput     | Boolesk        | Krävs      | MÅSTE vara ”sant”, som anger att null-optimerade spåra bäddas in manifestet Smooth klienten.                                                                                                                                                               |
| Undertyp            | Sträng         | Krävs      | Vara måste fyra tecken code ”DATA”.                                                                                                                                                                                                                         |
| Schema             | Sträng         | Krävs      | MÅSTE vara en URN eller URL identifierar meddelandet planen. till exempel ”urn: exempel: signalering: 1.0”. Detta måste vara ”urn: scte:scte35:2013a:bin” för att meddelanden ska skickas till HLS, Smooth och Dash klienter i enlighet med [SCTE 67] för [SCTE 35]. |
| TrackName          | Sträng         | Krävs      | MÅSTE vara namnet på den null-optimerade kursen. TrackName kan användas för att skilja flera händelseströmmar med samma schema. Varje unik händelseström måste ha ett unikt namn.                                                                           |
| tidsskalan          | Tal         | Valfri      | MÅSTE vara tidsskalan i den överordnade kursen.                                                                                                                                                                                                                      |

-------------------------------------

### <a name="222-movie-box"></a>2.2.2 film Box

Rutan film (moov) följer Live Manifest rutan Server som en del av stream-rubriken för ett null-optimerade spår.

Rutan ”moov' ska innehålla en **TrackHeaderBox (tkhd)** rutan enligt definitionen i [ISO-14496-12] med följande begränsningar:

| **Fältnamn** | **Fälttyp**          | **Krävs?** | **Beskrivning**                                                                                                |
|----------------|-------------------------|---------------|----------------------------------------------------------------------------------------------------------------|
| Varaktighet       | 64-bitars heltal utan tecken | Krävs      | BÖR vara 0, eftersom rutan spåra har noll exempel och den totala varaktigheten för exempel i rutan spåra är 0. |
-------------------------------------

Rutan ”moov' ska innehålla en **HandlerBox (hdlr)** enligt definitionen i [ISO-14496-12] med följande begränsningar:

| **Fältnamn** | **Fälttyp**          | **Krävs?** | **Beskrivning**   |
|----------------|-------------------------|---------------|-------------------|
| handler_type   | 32-bitars heltal utan tecken | Krävs      | SKA vara ”metadata”. |
-------------------------------------

Rutan 'stsd' ska innehålla en MetaDataSampleEntry ruta med ett kodning namn som definierats i [ISO-14496-12].  För SCTE 35 meddelanden ska kodning namnet ”scte”.

### <a name="223-movie-fragment-box-and-media-data-box"></a>2.2.3 film Fragment och Media Data

Null-optimerade spåra fragment består av en film Fragment (moof) och en Media Data (mdat).

Rutan MovieFragmentBox (moof) måste innehålla en **TrackFragmentExtendedHeaderBox (uuid)** rutan enligt definitionen i [MS-SSTR] med följande fält:

| **Fältnamn**         | **Fälttyp**          | **Krävs?** | **Beskrivning**                                                                               |
|------------------------|-------------------------|---------------|-----------------------------------------------------------------------------------------------|
| fragment_absolute_time | 64-bitars heltal utan tecken | Krävs      | MÅSTE vara ankomsttid för händelsen. Det här värdet justerar meddelandet med den överordnade kursen.   |
| fragment_duration      | 64-bitars heltal utan tecken | Krävs      | MÅSTE vara varaktigheten för händelsen. Varaktigheten kan vara noll för att indikera att varaktigheten är okänd. |

------------------------------------


Rutan MediaDataBox (mdat) måste ha följande format:

| **Fältnamn**          | **Fälttyp**                   | **Krävs?** | **Beskrivning**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-------------------------|----------------------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| version                 | 32-bitars heltal utan tecken (uimsbf) | Krävs      | Anger formatet för innehållet i rutan ”mdat'. Okänd versioner kommer att ignoreras. För närvarande är den enda versionen som stöds 1.                                                                                                                                                                                                                                                                                                                                                      |
| id                      | 32-bitars heltal utan tecken (uimsbf) | Krävs      | Identifierar den här instansen av meddelandet. Meddelanden med motsvarande semantik skall har samma värde. bearbetning av alla meddelanderutan för en händelse med samma id är tillräckliga.                                                                                                                                                                                                                                                                                                            |
| presentation_time_delta | 32-bitars heltal utan tecken (uimsbf) | Krävs      | Summan av fragment_absolute_time, anges i TrackFragmentExtendedHeaderBox och presentation_time_delta måste vara presentation tid för händelse. Presentation starttid och varaktighet ska justeras med Stream åtkomst punkter (SAP) av typen 1 eller 2, enligt definitionen i [ISO-14496-12] bilaga. Tid och varaktighet ska justeras med segment gränser för HLS utgående data. Presentation-tid och varaktighet för olika händelsemeddelanden inom samma händelseströmmen får inte överlappa varandra. |
| meddelande                 | bytematris                       | Krävs      | Händelsemeddelandet. För [SCTE 35] meddelanden är meddelandet binära splice_info_section(), även om [SCTE 67] rekommenderar något annat. Detta måste vara splice_info_section() för meddelanden som ska skickas till HLS, Smooth och Dash klienter i enlighet med [SCTE 67] för [SCTE 35]. För [SCTE 35] meddelanden, binär splice_info_section() nyttolasten för rutan ”mdat” och det är inte base64-kodad.                                                            |

------------------------------


### <a name="224-cancelation-and-updates"></a>2.2.4 säger upp avtalet och uppdateringar
Meddelanden kan har avbrutits eller uppdateras genom att skicka flera meddelanden med samma presentation tid och -ID.  Presentation-tid och ID identifiera unikt händelsen. Det sista meddelandet som togs emot för en specifik presentation tid, som uppfyller före distributionen begränsningar är meddelandet som agerar på. Uppdaterade meddelandet ersätter alla tidigare mottagna meddelanden.  Före distributionen begränsningen är fyra sekunder. Meddelanden som tas emot minst fyra sekunder innan presentation W3wp. 


## <a name="3-timed-metadata-delivery"></a>3 tidsgränsen Metadata leverans

Stream händelsedata är täckande för Media Services. Media Services Överför bara tre typer av information mellan slutpunkten för inmatning och klient-slutpunkt. Följande egenskaper levereras till klienten, i enlighet med [SCTE 67] och/eller klienten Direktuppspelningsprotokoll:

1.  Schema – en URN eller en URL som anger schemat för meddelandet.

2.  Presentation tid – presentation-tid för händelse på tidslinjen media.

3.  Varaktighet – varaktigheten för händelsen.

4.  ID – en valfri Unik identifierare för händelsen.

5.  Meddelandet – händelsedata.


## <a name="31-smooth-streaming-delivery"></a>3.1 smooth strömmande leverans

Referera till sparse spåra hantering av information i [MS-SSTR].

#### <a name="smooth-client-manifest-example"></a>Smidig klienten Manifest-exempel
~~~ xml
<?xml version=”1.0” encoding=”utf-8”?>
<SmoothStreamingMedia MajorVersion=”2” MinorVersion=”0” TimeScale=”10000000” IsLive=”true” Duration=”0”
  LookAheadFragmentCount=”2” DVRWindowLength=”6000000000”>

  <StreamIndex Type=”video” Name=”video” Subtype=”” Chunks=”0” TimeScale=”10000000”
    Url=”QualityLevels({bitrate})/Fragments(video={start time})”>
    <QualityLevel Index=”0” Bitrate=”230000”
      CodecPrivateData=”250000010FC3460B50878A0B5821FF878780490800704704DC0000010E5A67F840” FourCC=”WVC1”
      MaxWidth=”364” MaxHeight=”272”/>
    <QualityLevel Index=”1” Bitrate=”305000”
      CodecPrivateData=”250000010FC3480B50878A0B5821FF87878049080894E4A7640000010E5A67F840” FourCC=”WVC1”
      MaxWidth=”364” MaxHeight=”272”/>
    <c t=”0” d=”20000000” r=”300” />
  </StreamIndex>
  <StreamIndex Type=”audio” Name=”audio” Subtype=”” Chunks=”0” TimeScale=”10000000”
    Url=”QualityLevels({bitrate})/Fragments(audio={start time})”>
    <QualityLevel Index=”0” Bitrate=”96000” CodecPrivateData=”1000030000000000000000000000E00042C0”
      FourCC=”WMAP” AudioTag=”354” Channels=”2” SamplingRate=”44100” BitsPerSample=”16” PacketSize=”4459”/>
    <c t=”0” d=”20000000” r=”300” />
  </StreamIndex>
  <StreamIndex Type=”text” Name=”scte35-event-stream” Subtype=”DATA” Chunks=”0” TimeScale=”10000000”
    ParentStreamIndex=”video” ManifestOutput=”true” 
    Url=”QualityLevels({bitrate})/Fragments(captions={start time})”>
    <QualityLevel Index=”0” Bitrate=”0” CodecPrivateData=”” FourCC=””>
      <CustomAttributes>
        <Attribute Name=”Scheme” Value=”urn:scte:scte35:2013a:bin”/>
      </CustomAttributes>
    </QualityLevel>
    <!-- <f> contains base-64 encoded splice_info_section() message 
    <c t=”600000000” d=”300000000”>
<f>PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTUwMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</f>
    </c>
    <c t=”1200000000” d=”400000000”>      <f>PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTYyMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</f>
    </c>
  </StreamIndex>
</SmoothStreamingMedia> 
~~~ 

## <a name="32--apple-hls-delivery"></a>3.2 Apple HLS leverans
Tidsinställda metadata för Apple HTTP Live Streaming (HLS) kan bäddas in listan segment i en anpassad M3U-tagg.  Programlagret kan parsa M3U spelningslistan och bearbeta M3U taggar. Azure Media Services ska bädda in tidsinställd metadata i taggen EXT-X-stack som definierats i [HLS].  EXT-X-stack-taggen är för närvarande används av DynaMux för meddelanden av typen ADI3.  Ange attributen för taggen EXT-X-stack för att stödja SCTE 35 och icke SCTE 35 meddelanden, enligt nedan:

| **Attributnamn** | **Typ**                      | **Krävs?**                             | **Beskrivning**                                                                                                                                                                                                                                                                      |
|--------------------|-------------------------------|-------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| LÅT DIG LEDAS                | sträng inom citattecken                 | Krävs                                  | Meddelandet kodade som en base64-sträng som beskrivs i [IETF RFC 4648](http://tools.ietf.org/html/rfc4648). Detta är base64-kodad splice_info_section() för [SCTE 35] meddelanden.                                                                                                |
| TYP               | sträng inom citattecken                 | Krävs                                  | En URN eller en URL som anger meddelande-schemat. till exempel ”urn: exempel: signalering: 1.0”. För [SCTE 35] tar typen särskilda värdet ”scte35”.                                                                                                                                |
| ID                 | sträng inom citattecken                 | Krävs                                  | En unik identifierare för händelsen. Om det ID: T inte anges när meddelandet matas genererar Azure Media Services ett unikt id.                                                                                                                                          |
| VARAKTIGHET           | decimal flyttalsnummer | Krävs                                  | Varaktighet för händelsen. Om det är okänd, vara värdet 0. Enheterna är factional sekunder.                                                                                                                                                                                           |
| FÖRFLUTEN TID            | decimal flyttalsnummer | Valfritt, men krävs för skjutfönster | När signalen upprepas för ett skjutfönster presentation, måste det här fältet vara presentation tiden som har förflutit sedan händelsen påbörjades. Enheterna är fraktionell. Det här värdet kan överskrida den ursprungliga angivna varaktigheten splice eller segment. |
| TIME               | decimal flyttalsnummer | Krävs                                  | Presentation-tid för händelse. Enheterna är fraktionell.                                                                                                                                                                                                                    |


HLS player programnivån använder typen för att identifiera formatet för meddelandet, avkoda meddelandet, konvertera tid som krävs och bearbeta händelsen.  Händelserna är tidsinställningen synkroniserad högst i listan segment av överordnade-spåra enligt tidsstämpel för händelsen.  De infogas före det närmaste segmentet (#EXTINF tagg).

#### <a name="hls-segment-playlist-example"></a>HLS Segment spellistan exempel
~~~
#EXTM3U
#EXT-X-VERSION:4
#EXT-X-ALLOW-CACHE:NO
#EXT-X-MEDIA-SEQUENCE:0
#EXT-X-TARGETDURATION:6
#EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
#EXTINF:6.000000,no-desc
Fragments(video=0,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=60000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
#EXT-X-CUE: ID=”metadata-12.000000”,TYPE=”urn:example:signaling:1.0”,TIME=”12.000000”, DURATION=”18.000000”,CUE=”HrwOi8vYmWVkaWEvhhaWFRlRDa=”
Fragments(video=120000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=180000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=240000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=300000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=360000000,format=m3u8-aapl)
#EXT-X-CUE: ID=”metadata-42.000000”,TYPE=”urn:example:signaling:1.0”,TIME=”42.000000”, DURATION=”60.000000”,CUE=”PD94bWwgdm0iMS4wIiBlbmNvpD4=”
#EXTINF:6.000000,no-desc
Fragments(video=420000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=480000000,format=m3u8-aapl)
…
~~~

#### <a name="hls-message-handling"></a>HLS-meddelandehantering

Händelser signaleras i listan segment av varje video och ljud spår. Positionen för taggen EXT-X-stack måste alltid vara antingen omedelbart innan de första HLS segmentera (för splice ut eller segment start) eller omedelbart efter de senaste HLS segmentera (för splice i eller segment slutet) till som dess tid och varaktighet attribut refererar, vilket krävs av [ HLS].

När ett skjutfönster presentation aktiveras ska EXT-X-stack-taggen upprepas tillräckligt ofta som splice eller segment alltid är fullständigt beskrivs i listan segment och attributet FÖRFLUTEN måste användas för att ange hur lång tid splice eller segmentet har varit aktiv, som krävs av [HLS].

När ett skjutfönster presentation aktiveras tas EXT-X-stack-taggar bort från listan segment när mediet gången som de refererar till har återställts från skjutfönster för presentation.

## <a name="33--dash-delivery"></a>3.3 DASH leverans
[DASH] finns tre sätt att signalen händelser:

1.  Händelse uppstår i MPD
2.  Händelser signalerade band i Representation (med hjälp av meddelanderuta för händelse (emsg)
3.  En kombination av både 1 och 2

Händelse uppstår i MPD är användbara för VOD-direktuppspelning eftersom klienter har åtkomst till alla händelser, omedelbart när MPD har hämtats. In-band-lösningen är användbar för direktsänd strömning eftersom klienter inte behöver ladda ned MPD igen. För tidsbaserade segmentering anger klienten URL-Adressen för nästa segment genom att lägga till varaktighet och tidsstämpeln för det aktuella segmentet. Om som begärande misslyckas klienten förutsätter en avvikelse och hämtar MPD, men annars fortsätter utan att hämta MPD för direktuppspelning.

Azure Media Services gör båda signalering i MPD och in-band-signalering med meddelanderutan händelse.

#### <a name="mpd-signaling"></a>MPD-signalering

Händelser signaleras i MPD med hjälp av EventStream-elementet som visas i Period-elementet.

EventStream-elementet har följande attribut:

| **Attributnamn** | **Typ**                | **Krävs?** | **Beskrivning**                                                                                                                                                                                                                                                                                   |
|--------------------|-------------------------|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| scheme_id_uri      | sträng                  | Krävs      | Identifierar schemat för meddelandet. Schemat har angetts till värdet för attributet schemat i rutan Live Server Manifest. Värdet ska vara en URN eller en URL som anger meddelande-schemat. till exempel ”urn: exempel: signalering: 1.0”.                                                                |
| värde              | sträng                  | Valfri      | En ytterligare strängvärde som används av ägarna av schemat för att anpassa semantiken för meddelandet. Värdet måste anges till namnet på händelseströmmen (trackName för Smooth mata in eller AMF meddelandenamn för RTMP mata in) för att skilja flera händelseströmmar med samma schema. |
| Tidsskala          | 32-bitars heltal utan tecken | Krävs      | Tidsskalan i ticken per sekund, i fälten gånger och varaktighet i rutan ”emsg”.                                                                                                                                                                                                       |


Noll eller flera händelsen finns även i EventStream-elementet och de har följande attribut:

| **Attributnamn**  | **Typ**                | **Krävs?** | **Beskrivning**                                                                                                                                                                                                             |
|---------------------|-------------------------|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| presentation_time   | 64-bitars heltal utan tecken | Valfri      | MÅSTE vara mediet presentation tid för händelse i förhållande till början av perioden. Presentation starttid och varaktighet ska justeras med Stream åtkomst punkter (SAP) av typen 1 eller 2, enligt definitionen i [ISO-14496-12] bilaga. |
| Varaktighet            | 32-bitars heltal utan tecken | Valfri      | Varaktighet för händelsen. Detta måste utelämnas om varaktigheten är okänt.                                                                                                                                                 |
| id                  | 32-bitars heltal utan tecken | Valfri      | Identifierar den här instansen av meddelandet. Meddelanden med motsvarande semantik bör ha samma värde. Om det ID: T inte anges när meddelandet matas genererar Azure Media Services ett unikt id.             |
| Händelsen elementvärde | sträng                  | Krävs      | Händelsemeddelandet som en base64-sträng som beskrivs i [IETF RFC 4648](http://tools.ietf.org/html/rfc4648).                                                                                                                   |

#### <a name="xml-syntax-and-example-for-dash-manifest-mpd-signaling"></a>XML-Syntax och exempel för DASH manifest (MPD) signal

~~~ xml
<!-- XML Syntax -->
<xs:complexType name=”EventStreamType”>
  <xs:sequence>
    <xs:element name=”Event” type=”EventType” minOccurs=”0” maxOccurs=”unbounded”/>
    <xs:any namespace=”##other” processContents=”lax” minOccurs=”0” maxOccurs=”unbounded”/>
  </xs:sequence>
  <xs:attribute ref=”xlink:href”/>
  <xs:attribute ref=”xlink:actuate” default=”onRequest”/>
  <xs:attribute name=”schemeIdUri” type=”xs:anyURI” use=”required”/>
  <xs:attribute name=”value” type=”xs:string”/>
  <xs:attribute name=”timescale” type=”xs:unsignedInt”/>
</xs:complexType>
<!-- Event -->
<xs:complexType name=”EventType”>
  <xs:sequence>
    <xs:any namespace=”##other” processContents=”lax” minOccurs=”0” maxOccurs=”unbounded”/>
  </xs:sequence>
  <xs:attribute name=”presentationTime” type=”xs:unsignedLong” default=”0”/>
  <xs:attribute name=”duration” type=”xs:unsignedLong”/>
  <xs:attribute name=”id” type=”xs:unsignedInt”/>
  <xs:anyAttribute namespace=”##other” processContents=”lax”/>
</xs:complexType><?xml version=”1.0” encoding=”utf-8”?>


<!-- Example Section in MPD -->

<EventStream schemeIdUri=”urn:example:signaling:1.0” timescale=”1000” value=”player-statistics”>
  <Event presentationTime=”0” duration=”10000” id=”0”> PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTUwMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</Event>
  <Event presentationTime=”20000” duration=”10000” id=”1”> PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTYyMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</Event>
</EventStream>
~~~

>[!NOTE]
>Observera att presentationTime presentation tid för händelse, inte ankomsttid för meddelandet.
>

### <a name="431-in-band-event-message-box-signaling"></a>4.3.1 in-band-händelsen meddelande Box-signalering
En in-band-händelseström kräver MPD ha ett InbandEventStream element på nivån anpassning ange.  Det här elementet har en obligatorisk schemeIdUri attribut och valfritt tidsskalan attribut, som även visas i meddelanderutan (emsg).  Händelsen meddelanderutor med schema-ID som inte har definierats i MPD bör inte vara närvarande. Om en DASH-klienten upptäcker en händelse meddelanderutan med ett schema som inte har definierats i MPD, förväntas klienten att den ska ignoreras.
Rutan händelsemeddelandet (emsg) ger signalering för allmänna händelser relaterade till media presentation tid. Om det finns placeras någon ”emsg”-rutan innan alla 'moof ”-rutan.

### <a name="dash-event-message-box-emsg"></a>Meddelanderuta för DASH Event 'emsg'
~~~
Box Type: `emsg’
Container: Segment
Mandatory: No
Quantity: Zero or more
~~~

~~~ c
aligned(8) class DASHEventMessageBox extends FullBox(‘emsg’, version = 0, flags =0) 
{
    string scheme_id_uri;
    string value;
    unsigned int(32) timescale;
    unsigned int(32) presentation_time_delta;
    unsigned int(32) event_duration;
    unsigned int(32) id;
    unsigned int(8) message_data[];
}
~~~

Fälten för DASHEventMessageBox definieras nedan:

| **Fältnamn**          | **Fälttyp**          | **Krävs?** | **Beskrivning**                                                                                                                                                                                                                                                                                                                                                    |
|-------------------------|-------------------------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| scheme_id_uri           | sträng                  | Krävs      | Identifierar schemat för meddelandet. Schemat har angetts till värdet för attributet schemat i rutan Live Server Manifest. Värdet ska vara en URN eller en URL som anger meddelande-schemat. till exempel ”urn: exempel: signalering: 1.0”. För [SCTE 35] tar detta särskilda värdet ”urn: scte:scte35:2013a:bin”, även om [SCTE 67] rekommenderar något annat. |
| Värde                   | sträng                  | Krävs      | En ytterligare strängvärde som används av ägarna av schemat för att anpassa semantiken för meddelandet. För att skilja flera händelseströmmar med samma schema, ställs värdet till namnet på händelseströmmen (trackName för Smooth mata in eller AMF meddelandenamn för RTMP mata in).                                                                  |
| Tidsskala               | 32-bitars heltal utan tecken | Krävs      | Tidsskalan i ticken per sekund, i fälten gånger och varaktighet i rutan ”emsg”.                                                                                                                                                                                                                                                                        |
| Presentation_time_delta | 32-bitars heltal utan tecken | Krävs      | Det media presentation tidsspann presentation tid för händelse och den tidigaste tidpunkt presentation i det här segmentet. Presentation starttid och varaktighet ska justeras med Stream åtkomst punkter (SAP) av typen 1 eller 2, enligt definitionen i [ISO-14496-12] bilaga.                                                                                            |
| event_duration          | 32-bitars heltal utan tecken | Krävs      | Varaktigheten för händelsen eller 0xFFFFFFFF att indikera en okänd varaktighet.                                                                                                                                                                                                                                                                                          |
| Id                      | 32-bitars heltal utan tecken | Krävs      | Identifierar den här instansen av meddelandet. Meddelanden med motsvarande semantik bör ha samma värde. Om det ID: T inte anges när meddelandet matas genererar Azure Media Services ett unikt id.                                                                                                                                                    |
| Message_data            | bytematris              | Krävs      | Händelsemeddelandet. För [SCTE 35] meddelanden är meddelandedata binära splice_info_section(), även om [SCTE 67] rekommenderar något annat.                                                                                                                                                                                                                                 |

### <a name="332-dash-message-handling"></a>3.3.2 bindestreck meddelandehantering

Signaleras händelser i bandbredd, i rutan ”emsg” för både video- och ljudspår.  Signalering sker för alla segmentera begäranden som presentation_time_delta är 15 sekunder eller mindre. När ett skjutfönster presentation aktiveras tas händelsemeddelanden bort från MPD när summan av tid och varaktighet för händelsemeddelandet är mindre än tidpunkten för media-data i manifestet.  Med andra ord tas i händelsemeddelanden bort från manifestet när media tiden som de avser har återställts från skjutfönster för presentation.

## <a name="4-scte-35-ingest"></a>4 SCTE-35 mata in

[SCTE 35] meddelanden är matas in i binärt format med hjälp av schemat **”urn: scte:scte35:2013a:bin”** Smooth inmatning och typen **”scte35”** RTMP inmatning. Att underlätta konverteringen av [SCTE 35] tidpunkten, som baseras på MPEG-2 transport stream presentation tidsstämplar (punkter), en mappning mellan punkter (pts_time + pts_adjustment av splice_time()) och media tidslinjen tillhandahålls av händelse presentation tid ( fältet fragment_absolute_time för Smooth mata in och fältet för RTMP mata in). Mappningen är nödvändigt eftersom punkter 33-bitarsvärde som samlar in över ungefär var 26.5 timme.

Smooth Streaming mata in kräver att Media Data Box (mdat) måste innehålla den **splice_info_section()** definieras i tabell 8-1 i [SCTE 35]. RTMP hämta, låt dig ledas attributet meddelandets AMF har angetts till base64encoded **splice_info_section()**. När meddelandena har formatet som beskrivs ovan, skickas de till HLS, Smooth och Dash klienter i enlighet med [SCTE 67].


## <a name="5-references"></a>5-referenser

**[SCTE 35]**  ANSI/SCTE 35 2013a – Program för Digital infogning köa meddelande kabelanslutning 2013a

**[SCTE 67]**  ANSI/SCTE 67 2014 – rekommenderas för SCTE 35: Program för Digital infogning köa meddelande för kabel

**[DASH]**  ISO/IEC 23009-1 2014 – informationsteknik – dynamisk anpassningsbar strömning via HTTP (DASH) – del 1: Media Presentation beskrivning och segment format, 2nd edition

**[HLS]**  [”HTTP Live Streaming”, draft-pantos-http-live-streaming-14, den 14 oktober 2014](http://tools.ietf.org/html/draft-pantos-http-live-streaming-14)

**[MS-SSTR]**  [”Microsoft Smooth Streaming-protokollet”, den 15 maj 2014](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-SSTR%5d.pdf)

**[AMF0]**  [”Åtgärd meddelande Format AMF0”](http://download.macromedia.com/pub/labs/amf/amf0_spec_121207.pdf)

**[LIVE FMP4]**  [Specifikation för azure Media Services fragmenterad MP4 Live-inmatning](https://docs.microsoft.com/azure/media-services/media-services-fmp4-live-ingest-overview)

**[ISO-14496-12]**  ISO/IEC 14496-12: grundläggande media för en del 12 ISO-formatet, fjärde utgåvan 2012-07-15.

**[RTMP]**  [”Adobes i realtid meddelandeprotokoll”, 21 December 2012](https://www.adobe.com/devnet/rtmp.html) 

------------------------------------------

## <a name="next-steps"></a>Nästa steg
Visa Media Services utbildningsvägar.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
