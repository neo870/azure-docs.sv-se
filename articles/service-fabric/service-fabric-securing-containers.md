---
title: Importera certifikat till en behållare som körs på Azure Service Fabric | Microsoft Docs
description: Lär dig att importera certifikatfiler till en tjänst för Service Fabric-behållare.
services: service-fabric
documentationcenter: .net
author: TylerMSFT
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: twhitney, subramar
ms.openlocfilehash: d49c16741f581b2ad09dc173e8380fdf77391dbe
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/09/2018
ms.locfileid: "51299069"
---
# <a name="import-a-certificate-file-into-a-container-running-on-service-fabric"></a>Importera en certifikatfil till en behållare som körs på Service Fabric

Du kan skydda dina behållartjänster genom att ange ett certifikat. Service Fabric tillhandahåller en mekanism för tjänster i en behållare för att få åtkomst till ett certifikat som installeras på noderna i ett Windows- eller Linux-kluster (version 5.7 eller högre). Certifikatet måste installeras i LocalMachine på alla noder i klustret. Certifikatinformationen har angetts i manifestet under den `ContainerHostPolicies` tagg som i följande fragment visas:

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
    <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[Thumbprint1]"/>
    <CertificateRef Name="MyCert2" X509FindValue="[Thumbprint2]"/>
 ```

Windows-kluster, när programmet startas, körningen läser certifikat och genererar en PFX-filen och lösenordet för varje certifikat. Det här PFX-filen och lösenordet är tillgängliga i behållaren med hjälp av följande miljövariabler: 

* Certificates_ServicePackageName_CodePackageName_CertName_PFX
* Certificates_ServicePackageName_CodePackageName_CertName_Password

För Linux-kluster kopieras certifikat (PEM) över från arkivet som anges av X509StoreName till behållaren. Motsvarande miljövariabler i Linux är:

* Certificates_ServicePackageName_CodePackageName_CertName_PEM
* Certificates_ServicePackageName_CodePackageName_CertName_PrivateKey

Om du redan har certifikaten i formuläret och vill ha åtkomst till den i behållaren du också skapa ett paket för data i din app-paket och anger du följande i programmanifestet:

```xml
<ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
  <CertificateRef Name="MyCert1" DataPackageRef="[DataPackageName]" DataPackageVersion="[Version]" RelativePath="[Relative Path to certificate inside DataPackage]" Password="[password]" IsPasswordEncrypted="[true/false]"/>
 ```

Behållartjänst eller process ansvarar för att importera certifikatdatabasen till behållaren. Du kan använda för att importera certifikatet `setupentrypoint.sh` skript eller köra anpassad kod i behållaren processen. Här är exempelkod i C# för att importera PFX-filen:

```csharp
string certificateFilePath = Environment.GetEnvironmentVariable("Certificates_MyServicePackage_NodeContainerService.Code_MyCert1_PFX");
string passwordFilePath = Environment.GetEnvironmentVariable("Certificates_MyServicePackage_NodeContainerService.Code_MyCert1_Password");
X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
string password = File.ReadAllLines(passwordFilePath, Encoding.Default)[0];
password = password.Replace("\0", string.Empty);
X509Certificate2 cert = new X509Certificate2(certificateFilePath, password, X509KeyStorageFlags.MachineKeySet | X509KeyStorageFlags.PersistKeySet);
store.Open(OpenFlags.ReadWrite);
store.Add(cert);
store.Close();
```
Den här PFX-certifikat kan användas för att autentisera programmet eller tjänsten eller säker kommunikation med andra tjänster. Som standard är filerna ACLed endast för SYSTEM. Du kan ACL den till andra konton som krävs av tjänsten.

Som ett nästa steg kan du läsa följande artiklar:

* [Distribuera en Windows-behållare till Service Fabric i Windows Server 2016](service-fabric-get-started-containers.md)
* [Distribuera en Docker-behållare till Service Fabric i Linux](service-fabric-get-started-containers-linux.md)
