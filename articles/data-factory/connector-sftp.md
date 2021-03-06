---
title: Kopiera data från SFTP-servern med hjälp av Azure Data Factory | Microsoft Docs
description: Läs mer om MySQL-anslutningen i Azure Data Factory som låter dig kopiera data från en SFTP-server till ett dataarkiv som stöds som en mottagare.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/27/2018
ms.author: jingwang
ms.openlocfilehash: 3425558ac1ffa9e8d5146a5126f01c4ac55050dc
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37049638"
---
# <a name="copy-data-from-sftp-server-using-azure-data-factory"></a>Kopiera data från SFTP-servern med hjälp av Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1](v1/data-factory-sftp-connector.md)
> * [Aktuell version](connector-sftp.md)

Den här artikeln beskrivs hur du använder aktiviteten kopiera i Azure Data Factory för att kopiera data från en SFTP-server. Den bygger på den [kopiera aktivitet översikt](copy-activity-overview.md) artikel som presenterar en allmän översikt över kopieringsaktiviteten.

## <a name="supported-capabilities"></a>Funktioner som stöds

Du kan kopiera data från SFTP-servern till några stöds sink-datalagret. En lista över datalager som stöds som källor/sänkor av kopieringsaktiviteten, finns det [stöds datalager](copy-activity-overview.md#supported-data-stores-and-formats) tabell.

Mer specifikt stöder den här SFTP-anslutningen:

- Kopiera filer med **grundläggande** eller **SshPublicKey** autentisering.
- Filer som kopieras-är eller parsning filer med den [filformat och komprimering codec stöds](supported-file-formats-and-compression-codecs.md).

## <a name="get-started"></a>Kom igång

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Följande avsnitt innehåller information om egenskaper som används för att definiera Data Factory entiteter till SFTP.

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper

Följande egenskaper stöds för SFTP länkade tjänsten:

| Egenskap  | Beskrivning | Krävs |
|:--- |:--- |:--- |
| typ | Egenskapen type måste anges till: **Sftp**. |Ja |
| värd | Namn eller IP-adress till SFTP-server. |Ja |
| port | Port som avlyssnas SFTP-servern.<br/>Tillåtna värden är: heltal, standardvärdet är **22**. |Nej |
| skipHostKeyValidation | Ange om du vill hoppa över värden viktiga validering.<br/>Tillåtna värden är: **SANT**, **FALSKT** (standard).  | Nej |
| hostKeyFingerprint | Ange fingeravtryck för värdnyckeln. | Ja om ”skipHostKeyValidation” är inställd på false.  |
| authenticationType | Ange autentiseringstypen.<br/>Tillåtna värden är: **grundläggande**, **SshPublicKey**. Referera till [använder grundläggande autentisering](#using-basic-authentication) och [med hjälp av SSH autentisering med offentlig nyckel](#using-ssh-public-key-authentication) respektive avsnitt på fler egenskaper och JSON-exempel. |Ja |
| connectVia | Den [integrering Runtime](concepts-integration-runtime.md) som används för att ansluta till datalagret. Du kan använda Azure Integration Runtime eller Self-hosted integrering Runtime (om datalager finns i privat nätverk). Om inget anges används standard-Azure Integration Runtime. |Nej |

### <a name="using-basic-authentication"></a>Med grundläggande autentisering

Om du vill använda grundläggande autentisering för egenskapen ”authenticationType” **grundläggande**, och ange följande egenskaper förutom SFTP kopplingen generiska som introducerades i det sista avsnittet:

| Egenskap  | Beskrivning | Krävs |
|:--- |:--- |:--- |
| Användarnamn | Användare som har åtkomst till SFTP-servern. |Ja |
| lösenord | Lösenord för användare (användarnamn). Markera det här fältet som en SecureString lagra den på ett säkert sätt i Data Factory eller [referera en hemlighet som lagras i Azure Key Vault](store-credentials-in-key-vault.md). | Ja |

**Exempel:**

```json
{
    "apiVersion": "2017-09-01-preview",
    "name": "SftpLinkedService",
    "type": "linkedservices",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<sftp server>",
            "port": 22,
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="using-ssh-public-key-authentication"></a>Med hjälp av autentisering med SSH offentlig nyckel

Om du vill använda autentisering med SSH offentlig nyckel för egenskapen ”authenticationType” som **SshPublicKey**, och ange följande egenskaper förutom SFTP kopplingen generiska som introducerades i det sista avsnittet:

| Egenskap  | Beskrivning | Krävs |
|:--- |:--- |:--- |
| Användarnamn | Användare som har åtkomst till SFTP-server |Ja |
| privateKeyPath | Ange absolut sökväg till den fil för privat nyckel som har åtkomst till Integration Runtime. Gäller endast när automatisk värdbaserade Integration Runtime har angetts i ”connectVia”. | Ange antingen det `privateKeyPath` eller `privateKeyContent`.  |
| privateKeyContent | Base64-kodade innehåll för SSH privat nyckel. Privata SSH-nyckeln ska vara OpenSSH-format. Markera det här fältet som en SecureString lagra den på ett säkert sätt i Data Factory eller [referera en hemlighet som lagras i Azure Key Vault](store-credentials-in-key-vault.md). | Ange antingen det `privateKeyPath` eller `privateKeyContent`. |
| Lösenfrasen | Ange pass frasen/lösenord för att dekryptera den privata nyckeln om nyckelfilen skyddas av ett lösenord. Markera det här fältet som en SecureString lagra den på ett säkert sätt i Data Factory eller [referera en hemlighet som lagras i Azure Key Vault](store-credentials-in-key-vault.md). | Ja om filen för privata nyckeln skyddas av ett lösenord. |

> [!NOTE]
> SFTP-anslutningen har stöd för RSA/DSA OpenSSH-nyckel. Kontrollera att innehållet nyckelfil som börjar med ”---BEGIN [RSA/DSA] privata NYCKELN---”. Om filen för privat nyckel är en ppk-fil, Använd Putty verktyget vid konvertering från .ppk till OpenSSH-format. 

**Exempel 1: SshPublicKey autentisering med hjälp av privat nyckel filsökväg**

```json
{
    "apiVersion": "2017-09-01-preview",
    "name": "SftpLinkedService",
    "type": "Linkedservices",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<sftp server>",
            "port": 22,
            "skipHostKeyValidation": true,
            "authenticationType": "SshPublicKey",
            "userName": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": {
                "type": "SecureString",
                "value": "<pass phrase>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Exempel 2: SshPublicKey autentisering med hjälp av privat nyckel innehåll**

```json
{
    "apiVersion": "2017-09-01-preview",
    "name": "SftpLinkedService",
    "type": "Linkedservices",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<sftp server>",
            "port": 22,
            "skipHostKeyValidation": true,
            "authenticationType": "SshPublicKey",
            "userName": "<username>",
            "privateKeyContent": {
                "type": "SecureString",
                "value": "<base64 string of the private key content>"
            },
            "passPhrase": {
                "type": "SecureString",
                "value": "<pass phrase>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Egenskaper för datamängd

En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera datauppsättningar finns i artikeln datauppsättningar. Det här avsnittet innehåller en lista över egenskaper som stöds av SFTP dataset.

Ange typegenskapen för dataset för att kopiera data från SFTP, **filresursen**. Följande egenskaper stöds:

| Egenskap  | Beskrivning | Krävs |
|:--- |:--- |:--- |
| typ | Egenskapen type för dataset måste anges till: **filresursen** |Ja |
| folderPath | Sökvägen till mappen. Wildcard-filter stöds inte. Till exempel: mappen/undermappen / |Ja |
| fileName |  **Namn eller jokertecken-filtret** för alla filer under den angivna ”folderPath”. Om du inte anger ett värde för den här egenskapen dataset pekar på alla filer i mappen. <br/><br/>För filter som tillåts är jokertecken: `*` (matchar noll eller flera tecken) och `?` (matchar noll eller valfritt tecken).<br/>-Exempel 1: `"fileName": "*.csv"`<br/>-Exempel 2: `"fileName": "???20180427.txt"`<br/>Använd `^` att undanta om din faktiska filnamnet innehåller jokertecken eller den här escape-tecken i. |Nej |
| Format | Om du vill **kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppa över avsnittet format i både inkommande och utgående dataset-definitioner.<br/><br/>Om du vill tolka filer med ett specifikt format format för följande filtyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**,  **OrcFormat**, **ParquetFormat**. Ange den **typen** egenskap under format till ett av dessa värden. Mer information finns i [textformat](supported-file-formats-and-compression-codecs.md#text-format), [Json-Format](supported-file-formats-and-compression-codecs.md#json-format), [Avro-formatet](supported-file-formats-and-compression-codecs.md#avro-format), [Orc Format](supported-file-formats-and-compression-codecs.md#orc-format), och [parkettgolv Format](supported-file-formats-and-compression-codecs.md#parquet-format) avsnitt. |Nej (endast för binära kopiera scenario) |
| komprimering | Ange typ och kompression för data. Mer information finns i [stöds filformat och komprimering codec](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**.<br/>Nivåer som stöds är: **Optimal** och **snabbast**. |Nej |

>[!TIP]
>Kopiera alla filer i en mapp genom att ange **folderPath** endast.<br>Om du vill kopiera en enstaka fil med ett angivet namn, ange **folderPath** med Mappdel och **fileName** med filnamn.<br>Om du vill kopiera en delmängd av filer på en mapp, ange **folderPath** med Mappdel och **fileName** med jokerteckenfiltret.

>[!NOTE]
>Om du använde ”fileFilter”-egenskapen för filfilter fortfarande stöds som-är, medan du rekommenderas för att använda den nya filter kapaciteten som lagts till ”fileName” framöver.

**Exempel:**

```json
{
    "apiVersion": "2017-09-01-preview",
    "name": "SFTPDataset",
    "type": "Datasets",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<SFTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "myfile.csv.gz",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet

En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera aktiviteter finns i [Pipelines](concepts-pipelines-activities.md) artikel. Det här avsnittet innehåller en lista över egenskaper som stöds av SFTP källa.

### <a name="sftp-as-source"></a>SFTP som källa

Om du vill kopiera data från SFTP, anger du källa i kopieringsaktiviteten till **FileSystemSource**. Följande egenskaper stöds i kopieringsaktiviteten **källa** avsnitt:

| Egenskap  | Beskrivning | Krävs |
|:--- |:--- |:--- |
| typ | Egenskapen type för aktiviteten kopieringskälla måste anges till: **FileSystemSource** |Ja |
| rekursiva | Anger om data läses rekursivt från undermappar eller endast från den angivna mappen. Obs när rekursiv är inställd på true och mottagare är filbaserad lagring, tom mapp/underåtgärder-folder kommer inte att kopieras/skapas på mottagare.<br/>Tillåtna värden är: **SANT** (standard), **FALSKT** | Nej |

**Exempel:**

```json
"activities":[
    {
        "name": "CopyFromSFTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SFTP input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "FileSystemSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```


## <a name="next-steps"></a>Nästa steg
En lista över datakällor som stöds som källor och sänkor av kopieringsaktiviteten i Azure Data Factory finns [stöds datalager](copy-activity-overview.md##supported-data-stores-and-formats).
