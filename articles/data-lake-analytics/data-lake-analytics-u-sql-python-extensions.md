---
title: Utöka U-SQL-skript med Python i Azure Data Lake Analytics
description: Lär dig hur du kör Python-kod i U-SQL-skript med hjälp av Azure Data Lake Analytics
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: c1c74e5e-3e4a-41ab-9e3f-e9085da1d315
ms.topic: conceptual
ms.date: 06/20/2017
ms.openlocfilehash: b2179f9d81a2dad877c8ae58471f7440eb9edbe7
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43050999"
---
# <a name="extend-u-sql-scripts-with-python-code-in-azure-data-lake-analytics"></a>Utöka U-SQL-skript med Python-kod i Azure Data Lake Analytics

## <a name="prerequisites"></a>Förutsättningar

Innan du börjar måste du kontrollera Python-tillägg installeras i Azure Data Lake Analytics-kontot.

* Navigera till dig Data Lake Analytics-konto i Azure portal
* I den vänstra menyn under **komma igång** klickar du på **exempelskript**
* Klicka på **installera U-SQL-tilläggen** sedan **OK**

## <a name="overview"></a>Översikt 

Python-tillägg för U-SQL att utvecklare kan utföra massivt parallell körning av Python-kod. I följande exempel visas de grundläggande stegen:

* Använd den `REFERENCE ASSEMBLY` -uttrycket för att aktivera Python-tillägg för U-SQL-skript
* Med hjälp av den `REDUCE` åtgärden att partitionera inkommande data på en nyckel
* Python-tillägg för U-SQL är en inbyggd reducer (`Extension.Python.Reducer`) som kör Python-kod på varje brytpunkt som tilldelats reducer
* U-SQL-skriptet innehåller en inbäddad Python-kod som har en funktion som kallas `usqlml_main` som accepterar en pandas-DataFrame som indata och returnerar en pandas-DataFrame som utdata.

--

    REFERENCE ASSEMBLY [ExtPython];

    DECLARE @myScript = @"
    def get_mentions(tweet):
        return ';'.join( ( w[1:] for w in tweet.split() if w[0]=='@' ) )

    def usqlml_main(df):
        del df['time']
        del df['author']
        df['mentions'] = df.tweet.apply(get_mentions)
        del df['tweet']
        return df
    ";

    @t  = 
        SELECT * FROM 
           (VALUES
               ("D1","T1","A1","@foo Hello World @bar"),
               ("D2","T2","A2","@baz Hello World @beer")
           ) AS 
               D( date, time, author, tweet );

    @m  =
        REDUCE @t ON date
        PRODUCE date string, mentions string
        USING new Extension.Python.Reducer(pyScript:@myScript);

    OUTPUT @m
        TO "/tweetmentions.csv"
        USING Outputters.Csv();

## <a name="how-python-integrates-with-u-sql"></a>Hur Python kan integreras med U-SQL

### <a name="datatypes"></a>Datatyper

* Konverteras som strängvärden och numeriska kolumner från U-SQL-är mellan Pandas och U-SQL
* U-SQL null-värden konverteras till och från Pandas `NA` värden

### <a name="schemas"></a>Scheman

* Index vektorer i Pandas stöds inte i U-SQL. Alla indata bildrutor i Python-funktionen har alltid en 64-bitars numeriskt index mellan 0 och hur många rader minus 1. 
* U-SQL-datauppsättningar kan inte ha duplicerade kolumnnamn
* U-SQL datauppsättningar kolumnnamn som inte är strängar. 

### <a name="python-versions"></a>Python-versioner
Endast Python 3.5.1 (kompilerats för Windows) stöds. 

### <a name="standard-python-modules"></a>Standard Python-moduler
Alla Python-moduler som standard ingår.

### <a name="additional-python-modules"></a>Ytterligare Python-moduler
Förutom de Python-biblioteken som standard ingår flera vanliga python-bibliotek:

    pandas
    numpy
    numexpr

### <a name="exception-messages"></a>Undantag meddelanden
För närvarande ett undantag i Python-koden som visas som allmän hörn felet. U-SQL-jobb felmeddelanden visas i framtiden, Undantagsmeddelande Python.

### <a name="input-and-output-size-limitations"></a>Indata och utdata begränsningar
Varje hörn har en begränsad mängd minne som tilldelats den. Denna gräns är för närvarande 6 GB för en AU. Eftersom inkommande och utgående dataramar måste finnas i minnet i Python-koden, får inte den totala storleken för indata och utdata överskrida 6 GB.

## <a name="see-also"></a>Se också
* [Översikt över Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [Med hjälp av U-SQL-fönstrets funktioner för Azure Data Lake Analytics-jobb](data-lake-analytics-use-window-functions.md)
* [Använda Azure Data Lake Tools för Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md)
