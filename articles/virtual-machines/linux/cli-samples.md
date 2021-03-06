---
title: Azure CLI-exempel | Microsoft Docs
description: Azure CLI-exempel
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/08/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 8d573b5a0dafaab5eea98f05163ff99e7ccd3760
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/18/2018
ms.locfileid: "49409092"
---
# <a name="azure-cli-samples-for-linux-virtual-machines"></a>Azure CLI-exempel för virtuella Linux-datorer

Följande tabell innehåller länkar till bash-skript som skapats med hjälp av Azure CLI.

| | |
|---|---|
|**Skapa virtuella datorer**||
| [Skapa en virtuell dator](./../scripts/virtual-machines-linux-cli-sample-create-vm-quick-create.md?toc=%2fcli%2fazure%2ftoc.json) | Skapar en Linux-dator med minimal konfiguration. |
| [Skapa en fullständigt konfigurerad virtuell dator](./../scripts/virtual-machines-linux-cli-sample-create-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Skapar en resursgrupp, virtuell dator och alla relaterade resurser.|
| [Skapa virtuella datorer med hög tillgänglighet](./../scripts/virtual-machines-linux-cli-sample-nlb.md?toc=%2fcli%2fazure%2ftoc.json) | Skapar flera virtuella datorer med hög tillgänglighet och belastningsutjämnade konfiguration. |
| [Skapa en virtuell dator och kör konfigurationsskript](./../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md?toc=%2fcli%2fazure%2ftoc.json) | Skapar en virtuell dator och använder tillägget för anpassat skript för Azure för att installera NGINX. |
| [Skapa en virtuell dator med WordPress installerad](./../scripts/virtual-machines-linux-cli-sample-create-vm-wordpress.md?toc=%2fcli%2fazure%2ftoc.json) | Skapar en virtuell dator och använder tillägget för anpassat skript för Azure för att installera WordPress. |
| [Skapa en virtuell dator från en hanterad OS-disk](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json) | Skapar en virtuell dator genom att koppla en befintlig hanterad Disk som OS-disk. |
| [Skapa en virtuell dator från en ögonblicksbild](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) | Skapar en virtuell dator från en ögonblicksbild genom att först skapa en hanterad disk från en ögonblicksbild och sedan den nya hanterade disken som OS-disk. |
|**Hantera lagring**||
| [Skapa hanterad disk från en virtuell Hårddisk](../scripts/virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json) | Skapar en hanterad disk från en specialiserad virtuell hårddisk (VHD) som en OS-disk eller från en data-VHD som datadisk.  |
| [Skapa en hanterad disk från en ögonblicksbild](../scripts/virtual-machines-linux-cli-sample-create-managed-disk-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) | Skapar en hanterad disk från en ögonblicksbild. |
| [Kopiera hanterade diskar till samma eller en annan prenumeration](../scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Kopior hanterad disk till samma eller en annan prenumeration men i samma region som överordnat hanterad disk. 
| [Exportera en ögonblicksbild som VHD till ett lagringskonto](../scripts/virtual-machines-linux-cli-sample-copy-snapshot-to-storage-account.md?toc=%2fcli%2fmodule%2ftoc.json) | Exporterar en hanterad ögonblicksbild som VHD till ett lagringskonto i annan region. |
| [Exportera den virtuella Hårddisken för en hanterad disk till ett lagringskonto](../scripts/virtual-machines-linux-cli-sample-copy-managed-disks-vhd.md?toc=%2fcli%2fmodule%2ftoc.json) | Exporterar underliggande VHD från en hanterad disk till ett lagringskonto i annan region. |
| [Kopiera ögonblicksbild till samma eller en annan prenumeration](../scripts/virtual-machines-linux-cli-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Kopierar ögonblicksbilden till samma eller en annan prenumeration men i samma region som den överordnade ögonblicksbilden. |
|**Virtuella datorer**||
| [Säkra nätverkstrafik mellan virtuella datorer](./../scripts/virtual-machines-linux-cli-sample-create-vm-nsg.md?toc=%2fcli%2fazure%2ftoc.json) | Skapar två virtuella datorer och alla relaterade resurser som en intern och extern nätverkssäkerhetsgrupper (NSG). |
|**Skydda virtuella datorer**||
| [Kryptera en virtuell dator- och datadiskar](./../scripts/virtual-machines-linux-cli-sample-encrypt-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Skapar en Azure Key Vault, en krypteringsnyckel och ett huvudnamn för tjänsten och sedan krypterar en virtuell dator. |
|**Övervaka virtuella datorer**||
| [Övervaka en virtuell dator med Azure Log Analytics](./../scripts/virtual-machines-linux-cli-sample-create-vm-oms.md?toc=%2fcli%2fazure%2ftoc.json) | Skapar en virtuell dator, installerar Log Analytics-agenten och registrerar den virtuella datorn i en Log Analytics-arbetsyta.  |
|**Felsökning av virtuella datorer**||
| [Felsöka en operativsystemdisk för virtuella datorer](./../scripts/virtual-machines-linux-cli-sample-mount-os-disk.md?toc=%2fcli%2fazure%2ftoc.json) | Monterar operativsystemets disk från en virtuell dator som en datadisk på en andra virtuell dator. |
| | |
