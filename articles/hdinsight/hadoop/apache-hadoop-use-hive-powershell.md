---
title: Använda Hadoop Hive med PowerShell i HDInsight - Azure
description: Använd PowerShell för att köra Hive-frågor i Hadoop på HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: hrasheed
ms.openlocfilehash: e02f6b17dff93ce2df1f06ad9681e3086c35300c
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51257875"
---
# <a name="run-hive-queries-using-powershell"></a>Köra Hive-frågor med hjälp av PowerShell
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Det här dokumentet innehåller ett exempel på hur du använder Azure PowerShell i Azure-resursgrupp-läge kör Hive-frågor i ett Hadoop på HDInsight-kluster.

> [!NOTE]
> Det här dokumentet ger inte en detaljerad beskrivning av vad HiveQL-instruktioner som används i exemplen göra. Information om HiveQL som används i det här exemplet finns i [använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md).

## <a name="prerequisites"></a>Förutsättningar

* En Linux-baserat Hadoop i HDInsight-kluster av version 3.4 och senare.

  > [!IMPORTANT]
  > Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare. Mer information finns i [HDInsight-avveckling på Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* En klient med Azure PowerShell.

[!INCLUDE [upgrade-powershell](../../../includes/hdinsight-use-latest-powershell.md)]

## <a name="run-a-hive-query"></a>Köra en Hive-fråga

Azure PowerShell tillhandahåller *cmdletar* som gör det möjligt att köra Hive-frågor via fjärranslutning på HDInsight. Internt cmdletarna göra REST-anrop till [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) på HDInsight-kluster.

När du kör Hive-frågor i ett fjärranslutet HDInsight-kluster används följande cmdletar:

* `Connect-AzureRmAccount`: Autentiserar Azure PowerShell på Azure-prenumerationen.
* `New-AzureRmHDInsightHiveJobDefinition`: Skapar en *jobbet definition* med hjälp av de angivna HiveQL-instruktionerna.
* `Start-AzureRmHDInsightJob`: Skickar jobbdefinitionen till HDInsight och startar jobbet. En *jobbet* objekt returneras.
* `Wait-AzureRmHDInsightJob`: Använder objektet för att kontrollera status för jobbet. Den ska vänta tills jobbet har slutförts eller väntetiden har överskridits.
* `Get-AzureRmHDInsightJobOutput`: Används för att hämta utdata för jobbet.
* `Invoke-AzureRmHDInsightHiveJob`: Används för att köra HiveQL-instruktioner. Den här cmdlet-block frågan har slutförts och returnerar sedan resultatet.
* `Use-AzureRmHDInsightCluster`: Anger det aktuella klustret ska användas för den `Invoke-AzureRmHDInsightHiveJob` kommando.

Följande steg visar hur du kan använda dessa cmdletar för att köra ett jobb i HDInsight-klustret:

1. Med hjälp av en redigerare, spara följande kod som `hivejob.ps1`.

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=5-42)]

2. Öppna ett nytt **Azure PowerShell** Kommandotolken. Ändra kataloger till platsen för den `hivejob.ps1` filen och sedan använda följande kommando för att köra skriptet:

        .\hivejob.ps1

    När skriptet har körts uppmanas du att ange klusternamnet och HTTPS/Admin-autentiseringsuppgifter för klustret. Du kan också uppmanas att logga in på Azure-prenumerationen.

3. När jobbet har slutförts, returnerar den information liknande följande text:

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Som tidigare nämnts, `Invoke-Hive` kan användas för att köra en fråga och vänta tills svaret. Använd följande skript för att se hur Invoke-Hive fungerar:

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=50-71)]

    Utdata ser ut som följande text:

        2012-02-03    18:35:34    SampleClass0    [ERROR]    incorrect    id
        2012-02-03    18:55:54    SampleClass1    [ERROR]    incorrect    id
        2012-02-03    19:25:27    SampleClass4    [ERROR]    incorrect    id

   > [!NOTE]
   > Du kan använda Azure PowerShell för frågor med längre HiveQL **här strängar** cmdlet eller HiveQL skriptfiler. Följande kodfragment visar hur du använder den `Invoke-Hive` cmdlet för att köra en skriptfil för HiveQL. HiveQL skriptfilen måste överföras till wasb: / /.
   >
   > `Invoke-AzureRmHDInsightHiveJob -File "wasb://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
   >
   > Mer information om **här strängar**, se <a href="https://technet.microsoft.com/library/ee692792.aspx" target="_blank">med hjälp av Windows PowerShell här anslutningssträngar</a>.

## <a name="troubleshooting"></a>Felsökning

Om ingen information returneras när jobbet har slutförts kan visa felloggarna. Om du vill visa information om fel för det här jobbet, lägger du till följande i slutet av den `hivejob.ps1` filen, spara den och sedan köra det igen.

```powershell
# Print the output of the Hive job.
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Denna cmdlet returnerar informationen som skrivs till STDERR under bearbetningen av jobbet.

## <a name="summary"></a>Sammanfattning

Som du kan se är det enkelt att köra Hive-frågor i ett HDInsight-kluster, övervaka jobbstatus och hämta utdata i Azure PowerShell.

## <a name="next-steps"></a>Nästa steg

Allmän information om Hive i HDInsight:

* [Använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md)

Information om andra sätt kan du arbeta med Hadoop i HDInsight:

* [Använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md)
* [Använda MapReduce med Hadoop i HDInsight](hdinsight-use-mapreduce.md)
