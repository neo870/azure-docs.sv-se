---
title: Gästoperativsystem som stöds för Azure Stack | Microsoft Docs
description: Dessa gästoperativsystem kan användas på Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: be4d9b3ea7e5715d7c3a4df11b7e8bab4d1d4ca5
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47405605"
---
# <a name="guest-operating-systems-supported-on-azure-stack"></a>Gästoperativsystem som stöds på Azure Stack

*Gäller för: integrerade Azure Stack-system och Azure Stack Development Kit*

## <a name="windows"></a>Windows

Azure Stack stöd för de Windows-gästoperativsystem som visas i följande tabell:

| Operativsystem | Beskrivning | Tillgänglig på Marketplace |
| --- | --- | --- | --- | --- | --- |
| Windows Server, version 1709 | 64-bitars | Core med behållare |
| Windows Server 2016 | 64-bitars |  Datacenter, Datacenter-kärna, Datacenter med behållare |
| Windows Server 2012 R2 | 64-bitars |  Datacenter |
| Windows Server 2012 | 64-bitars |  Datacenter |
| Windows Server 2008 R2 SP1 | 64-bitars |  Datacenter |
| Windows Server 2008 SP2 | 64-bitars |  Ta med din egen avbildning |
| Windows 10 *(se anmärkning 1)* | 64-bitars, Pro och Enterprise | Ta med din egen avbildning |

> [!NOTE]
> Om du vill distribuera Windows 10-klientoperativsystem på Azure Stack, måste du ha [Windows licenser per användare](https://www.microsoft.com/en-us/Licensing/product-licensing/windows10.aspx) eller köper via ett kvalificerat Multitenant-värden ([QMTH](https://www.microsoft.com/en-us/CloudandHosting/licensing_sca.aspx)).

Marketplace-avbildningar är tillgängliga för betalning som du-användning eller BYOL (EA/SPLA) licensiering. Användning av både på en enda Azure Stack-instans stöds inte. Lägger in en lämplig version av gästagenten i avbildningen under distributionen av Azure Stack.

Datacenter-versionerna är tillgängliga i marketplace för att ladda ned; kunder kan använda sina egna server-avbildningar, inklusive andra versioner. Windows-klientavbildningar är inte tillgängliga i Marketplace.

## <a name="linux"></a>Linux

Linux-distributioner som är listat som är tillgänglig i Marketplace innehåller den nödvändiga Windows Azure Linux Agent (WALA). Om du överför en egen avbildning till Azure Stack, följer du riktlinjerna i [Lägg till Linux-avbildningar till Azure Stack](azure-stack-linux.md).

> [!NOTE]
> Anpassade avbildningar ska byggas med den senaste versionen för offentliga WALA. Versioner som är äldre än 2.2.18 kanske inte fungerar korrekt på Azure Stack.
>
> [cloud-init](https://cloud-init.io/) stöds inte på Azure Stack just nu.

| Distribution | Beskrivning | Utgivare | Marketplace |
| --- | --- | --- | --- | --- | --- |
| CentOS-baserade 6,9 | 64-bitars | Rogue Wave | Ja |
| CentOS-baserade 7.4 | 64-bitars | Rogue Wave | Ja |
| ClearLinux | 64-bitars | ClearLinux.org | Ja |
| Behållaren Linux |  64-bitars | CoreOS | Stable |
| Debian 8 ”Jessie” | 64-bitars | credativ |  Ja |
| Debian 9 ”Stretch” | 64-bitars | credativ | Ja |
| Red Hat Enterprise Linux 7.x | 64-bitars | Red Hat |Ta med din egen avbildning |
| SLES 11SP4 | 64-bitars | SUSE | Ja |
| SLES 12SP3 | 64-bitars | SUSE | Ja |
| Ubuntu 14.04-LTS | 64-bitars | Canonical | Ja |
| Ubuntu 16.04-LTS | 64-bitars | Canonical | Ja |
| Ubuntu 18.04-LTS | 64-bitars | Canonical | Ja |

Red Hat Enterprise Linux supportinformation i [Red Hat och Azure Stack: vanliga frågor och svar](https://access.redhat.com/articles/3413531).

## <a name="next-steps"></a>Nästa steg

Mer information om Azure Stack Marketplace finns i följande artiklar:

[Hämta Marketplace-objekt](azure-stack-download-azure-marketplace-item.md)  
[Skapa och publicera ett Marketplace-objekt](azure-stack-create-and-publish-marketplace-item.md)