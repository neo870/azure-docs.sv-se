---
title: Installera Jupyter lokalt och Anslut till Spark i Azure HDInsight
description: Lär dig mer om att installera Jupyter-anteckningsbok lokalt på datorn och ansluter den till ett Apache Spark-kluster.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/28/2017
ms.author: hrasheed
ms.openlocfilehash: 9bab98d8009704d570dc4b03a97b9cbc5d085b4c
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51007560"
---
# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-on-hdinsight"></a>Installera Jupyter notebook på datorn och ansluta till Apache Spark i HDInsight

I den här artikeln lär du dig hur du installerar Jupyter-anteckningsbok med anpassade PySpark (för Python) och Spark (för Scala) kernlar med Spark magic och ansluter den bärbara datorn till ett HDInsight-kluster. Det kan finnas flera anledningar till att installera Jupyter på den lokala datorn och det kan finnas några utmaningar samt. Mer information om detta finns i avsnittet [Varför bör jag installera Jupyter på datorn](#why-should-i-install-jupyter-on-my-computer) i slutet av den här artikeln.

Det finns tre viktiga steg när du installerar Jupyter- och Spark-magic på datorn.

* Installera Jupyter-anteckningsbok
* Installera dessa kernlar PySpark och Spark med Spark-magic
* Konfigurera Spark magic för att komma åt Spark-kluster i HDInsight

Mer information om anpassade kernlar och den Spark magin som är tillgängliga för Jupyter-anteckningsböcker med HDInsight-kluster finns i [Kernlar som är tillgängliga för Jupyter-anteckningsböcker med Apache Spark Linux-kluster i HDInsight](apache-spark-jupyter-notebook-kernels.md).

## <a name="prerequisites"></a>Förutsättningar
De förutsättningar som anges här är inte för att installera Jupyter. Det här är för Jupyter-anteckningsboken för att ansluta till ett HDInsight-kluster när anteckningsboken har installerats.

* En Azure-prenumeration. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Ett Apache Spark-kluster i HDInsight. Anvisningar finns i [Skapa Apache Spark-kluster i Azure HDInsight](apache-spark-jupyter-spark-sql.md).

## <a name="install-jupyter-notebook-on-your-computer"></a>Installera Jupyter notebook på datorn

Du måste installera Python innan du kan installera Jupyter-anteckningsböcker. Både Python och Jupyter är tillgängliga som en del av den [Anaconda distribution](https://www.continuum.io/downloads). När du installerar Anaconda, installerar du en distribution av Python. När Anaconda har installerats kan du lägga till Jupyter-installationen genom att köra rätt kommandon.

1. Ladda ned den [Anaconda installer](https://www.continuum.io/downloads) för din plattform och kör installationen. När du kör installationsguiden, kontrollera att du väljer alternativet att lägga till Anaconda i PATH-variabeln.
1. Kör följande kommando för att installera Jupyter.

        conda install jupyter

    Läs mer om hur du installerar Jupyter [installera Jupyter med Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).

## <a name="install-the-kernels-and-spark-magic"></a>Installera kernlar och Spark magic

Anvisningar om hur du installerar Spark-magic PySpark och Spark-kärnor, följer du installationsanvisningarna i den [sparkmagic dokumentation](https://github.com/jupyter-incubator/sparkmagic#installation) på GitHub. Det första steget i Spark magic dokumentationen uppmanar dig att installera Spark magic. Ersätt det första steget i länken med följande kommandon, beroende på vilken version av HDInsight-kluster som du ansluter till. Efter det återstående åtgärderna i magic Spark-dokumentationen. Om du vill installera olika kärnor måste du utföra steg3 i avsnittet Spark magic installation instruktioner.

* För kluster v3.4 installera sparkmagic 0.2.3 genom att köra `pip install sparkmagic==0.2.3`

* För kluster v3.5 och v3.6, installera sparkmagic 0.11.2 genom att köra `pip install sparkmagic==0.11.2`

## <a name="configure-spark-magic-to-connect-to-hdinsight-spark-cluster"></a>Konfigurera Spark magic att ansluta till HDInsight Spark-kluster

I det här avsnittet kan du konfigurera Spark-magic som du har installerat tidigare för att ansluta till ett Apache Spark-kluster som du måste redan har skapat i Azure HDInsight.

1. Jupyter Konfigurationsinformationen lagras vanligtvis i arbetskatalogen för användare. Skriv följande kommandon för att hitta din hemkatalog på alla OS-plattformar.

    Starta Python-gränssnittet. I ett kommandofönster, skriver du följande:

        python

    Ange följande kommando för att ta reda på arbetskatalogen på Python-gränssnittet.

        import os
        print(os.path.expanduser('~'))

1. Gå till arbetskatalogen och skapa en mapp med namnet **.sparkmagic** om den inte redan finns.
1. I mappen, skapar du en fil med namnet **config.json** och Lägg till följande JSON-kodfragmentet i den.

        {
          "kernel_python_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          },
          "kernel_scala_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          }
        }

1. Ersätt **{USERNAME}**, **{CLUSTERDNSNAME}**, och **{BASE64ENCODEDPASSWORD}** med lämpliga värden. Du kan använda ett antal verktyg i favorit programmeringsspråk eller online för att generera en base64-kodad lösenord för det faktiska lösenordet.

1. Konfigurera rätt pulsslagsinställningar i `config.json`. Du bör lägga till de här inställningarna på samma nivå som den `kernel_python_credentials` och `kernel_scala_credentials` kodfragment din har lagts till tidigare. Ett exempel på hur och var du vill lägga till inställningar för pulsslag finns i den här [exempel config.json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).

    * För `sparkmagic 0.2.3` (v3.4), kluster omfattar:

            "should_heartbeat": true,
            "heartbeat_refresh_seconds": 5,
            "heartbeat_retry_seconds": 1

    * För `sparkmagic 0.11.2` (kluster v3.5 och v3.6) inkluderar:

            "heartbeat_refresh_seconds": 5,
            "livy_server_heartbeat_timeout_seconds": 60,
            "heartbeat_retry_seconds": 1

    >[!TIP]
    >Det sänds pulsslag för att säkerställa att sessioner inte sprids. När en dator försätts i viloläge eller är avstängd, sänds pulsslag, vilket resulterar i sessionen som rensas. För kluster v3.4 om du vill inaktivera den här funktionen kan du ange Livy-config `livy.server.interactive.heartbeat.timeout` till `0` från Ambari UI. För kluster v3.5, om du inte anger 3,5 konfigurationen ovan tas sessionen inte bort.

1. Starta Jupyter. Använd följande kommando i Kommandotolken.

        jupyter notebook

1. Kontrollera att du kan ansluta till klustret med Jupyter-anteckningsboken och att du kan använda den Spark magin som är tillgängliga med dessa kernlar. Utför följande steg.

    a. Skapa en ny anteckningsbok. I det högra hörnet, klickar du på **New**. Du bör se standardkernel **Python2** och de nya kernlar som du installerar **PySpark** och **Spark**. Klicka på **PySpark**.

    ![Kernlar i Jupyter-anteckningsbok](./media/apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Kernlar i Jupyter-anteckningsbok")

    b. Kör följande kodavsnitt.

        %%sql
        SELECT * FROM hivesampletable LIMIT 5

    Om du har hämta utdata, testas din anslutning till HDInsight-kluster.

    >[!TIP]
    >Om du vill uppdatera konfigurationen av anteckningsboken för att ansluta till ett annat kluster måste du uppdatera config.json med den nya uppsättningen värden, som visas i steg3 ovan.

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Varför bör jag installera Jupyter på datorn?
Det kan finnas flera skäl till varför du kanske vill installera Jupyter på datorn och ansluter den till en Spark-kluster i HDInsight.

* Även om Jupyter notebooks finns redan på Spark-kluster i Azure HDInsight, installera Jupyter på datorn ger dig möjlighet att skapa dina anteckningsböcker lokalt, testa programmet mot ett aktivt kluster och sedan ladda upp den anteckningsböcker till klustret. Om du vill ladda upp anteckningsböckerna till klustret, kan du ladda upp dem med Jupyter-anteckningsboken som körs eller klustret eller spara dem i mappen /HdiNotebooks i lagringskontot som associerats med klustret. Mer information om hur anteckningsböcker lagras i klustret finns i [där lagras Jupyter-anteckningsböcker](apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?
* Med de bärbara datorerna som är tillgänglig lokalt, kan du ansluta till olika Spark-kluster utifrån behov.
* Du kan använda GitHub för att implementera ett källkontrollsystem och har versionskontroll för de bärbara datorerna. Du kan också ha en samarbetsmiljö där flera användare kan arbeta med samma anteckningsbok.
* Du kan arbeta med bärbara datorer lokalt utan även att ett kluster. Du behöver bara ett kluster att testa dina anteckningsböcker mot, inte för att manuellt hantera dina anteckningsböcker eller en utvecklingsmiljö.
* Det kan vara enklare att konfigurera din egen lokala utvecklingsmiljö än att konfigurera Jupyter-installationen på klustret.  Du kan dra nytta av all programvara som du har installerat lokalt utan att konfigurera ett eller flera fjärranslutna kluster.

> [!WARNING]
> Med Jupyter installerat på din lokala dator, kan flera användare köra samma anteckningsbok i samma Spark-klustret på samma gång. I detta fall skapas flera Livy-sessioner. Om du får problem och vill felsöka det blir en komplicerad uppgift att spåra vilken Livy-session som hör till vilken användare.
>
>

## <a name="seealso"></a>Se även
* [Översikt: Apache Spark i Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenarier
* [Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg](apache-spark-use-bi-tools.md)
* [Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data](apache-spark-ipython-notebook-machine-learning.md)
* [Spark med Machine Learning: Använda Spark i HDInsight för att förutsäga resultatet av en livsmedelskontroll](apache-spark-machine-learning-mllib-ipython.md)
* [Webbplatslogganalys med Spark i HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Skapa och köra program
* [Skapa ett fristående program med hjälp av Scala](apache-spark-create-standalone-application.md)
* [Köra jobb via fjärranslutning på ett Spark-kluster med Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Verktyg och tillägg
* [Använda HDInsight Tools-plugin för IntelliJ IDEA till att skapa och skicka Spark Scala-appar](apache-spark-intellij-tool-plugin.md)
* [Använda HDInsight Tools-plugin för IntelliJ IDEA till att felsöka Spark-program via fjärranslutning](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight](apache-spark-zeppelin-notebook.md)
* [Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Använda externa paket med Jupyter-anteckningsböcker](apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a>Hantera resurser
* [Hantera resurser för Apache Spark-klustret i Azure HDInsight](apache-spark-resource-manager.md)
* [Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight](apache-spark-job-debugging.md)
