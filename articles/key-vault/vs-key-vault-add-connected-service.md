---
title: Lägger till stöd för Key Vault i ASP.NET-projektet med Visual Studio | Microsoft Docs
description: Använd den här självstudiekursen för att lära dig hur du lägger till stöd för Key Vault i ett ASP.NET eller ASP.NET Core-webbprogram.
services: key-vault
author: ghogen
manager: douge
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: 9f2adfcbf2d6ca5de79cc787029f5139138b0e52
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50230445"
---
# <a name="add-key-vault-to-your-web-application-by-using-visual-studio-connected-services"></a>Lägg till Key Vault i ditt webbprogram med hjälp av Visual Studio Connected Services

I den här självstudien får lära du dig att enkelt lägga till allt du behöver för att börja använda Azure Key Vault för att hantera dina hemligheter för webbprojekt i Visual Studio om du använder ASP.NET Core eller alla typer av ASP.NET-projekt. Du kan ha Visual Studio automatiskt lägga till alla NuGet-paket och konfigurationsinställningar som du behöver ansluta till Key Vault i Azure med hjälp av funktionen Connected Services i Visual Studio 2017. 

Mer information om ändringarna att Connected Services gör i ditt projekt för att aktivera Key Vault finns [Key Vault Connected Service - vad hände med mitt ASP.NET 4.7.1 projekt](vs-key-vault-aspnet-what-happened.md) eller [Key Vault Connected Service - vad hände med mitt ASP.NET Core-projekt](vs-key-vault-aspnet-core-what-happened.md).

## <a name="prerequisites"></a>Förutsättningar

- **En Azure-prenumeration**. Om du inte har en prenumeration kan du registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).
- **Visual Studio 2017 version 15.7** med arbetsbelastningen **Webbutveckling** installerad. [Ladda ned det nu](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).
- För ASP.NET (inte Core) behöver du utvecklingsverktygen .NET Framework 4.7.1 som inte installeras som standard. Starta installationsprogrammet för Visual Studio för att installera dem, Välj **ändra**, och välj sedan **enskilda komponenter**, sedan till höger, expandera **ASP.NET och webbutveckling**, och välj **utvecklingsverktyg för .NET Framework 4.7.1**.
- En ASP.NET 4.7.1 eller ASP.NET Core 2.0 webbprojekt som är öppna.

## <a name="add-key-vault-support-to-your-project"></a>Lägg till stöd för Key Vault i projektet

1. I **Solution Explorer** väljer du **Lägg till** > **Ansluten tjänst**.
   Sidan Ansluten tjänst visas med tjänster som du kan lägga till i projektet.
1. Meny med olika tjänster, Välj **skydda hemligheter med Azure Key Vault**.

   ![Välj ”säker hemligheter med Azure Key Vault”](media/vs-key-vault-add-connected-service/KeyVaultConnectedService1.PNG)

   Om du har loggat in till Visual Studio och har en Azure-prenumeration som är associerad med ditt konto, visas en sida med en listruta med dina prenumerationer. Kontrollera att du har loggat in Visual Studio och att konton som du har loggat in med samma konto som du använder för din Azure-prenumeration.

1. Välj den prenumeration som du vill använda och välj sedan en ny eller befintlig Key Vault eller Välj länken Redigera för att ändra det automatiskt genererade namnet.

   ![Välj din prenumeration](media/vs-key-vault-add-connected-service/KeyVaultConnectedService3.PNG)

1. Ange namnet som du vill använda för Key Vault.

   ![Byt namn på Key Vault och välj en resursgrupp](media/vs-key-vault-add-connected-service/KeyVaultConnectedService-Edit.PNG)

1. Välj en befintlig resursgrupp eller välja att skapa en ny med ett automatiskt genererade unika namnet.  Om du vill skapa en ny grupp med ett annat namn kan du använda den [Azure-portalen](https://portal.azure.com), och sedan stänga sidan och startas om för att läsa in listan över resursgrupper.
1. Välj den region där du vill skapa Key Vault. Om ditt webbprogram finns i Azure, väljer du den region som är värd för webbprogram för optimala prestanda.
1. Välj en prismodell. Mer information finns i [priser för Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).
1. Välj OK om du vill acceptera konfigurationsalternativ.
1. Välj **Lägg till** att skapa Key Vault. Skapandeprocessen misslyckas om du väljer ett namn som redan används.  Om detta händer, Använd den **redigera** länk för att byta namn på Key Vault och försök igen.

   ![Att lägga till ansluten tjänst i projektet](media/vs-key-vault-add-connected-service/KeyVaultConnectedService4.PNG)

1. Lägg nu till en hemlighet i Key Vault i Azure. Gå till rätt plats i portalen genom att klicka på länken för hantera hemligheter som lagras i den här Key Vault. Om du har stängt sidan eller projektet du kan navigera till den i den [Azure-portalen](https://portal.azure.com) genom att välja **alla tjänster**under **Security**, Välj **Key Vault**, välj sedan det Nyckelvalv som du skapade.

   ![Gå till portalen](media/vs-key-vault-add-connected-service/manage-secrets-link.jpg)

1. Valvet du skapade i avsnittet Key Vault för nyckeln, Välj **hemligheter**, sedan **generera/importera**.

   ![Generera/importera en hemlighet](media/vs-key-vault-add-connected-service/generate-secrets.jpg)

1. Ange en hemlighet, till exempel ”MinHemlighet” och ge den ett värde som ett test, och välj sedan den **skapa** knappen.

   ![Skapa en hemlighet](media/vs-key-vault-add-connected-service/create-a-secret.jpg)

1. (valfritt) Ange en annan hemlighet, men nu placerar den i en kategori genom att namnge den ”hemligheter--MinHemlighet”. Den här syntaxen anger en kategori ”hemligheter” som innehåller en hemlighet ”MinHemlighet”.
 
Du kan nu komma åt dina hemligheter i kod. Nästa steg är olika beroende på om du använder ASP.NET 4.7.1 eller ASP.NET Core.

## <a name="access-your-secrets-in-code"></a>Få åtkomst till dina hemligheter i kod

1. Installera de här två nuget-paket [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) och [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) NuGet-bibliotek.

2. Öppna filen Program.cs och uppdatera koden med följande kod: 
```
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
           WebHost.CreateDefaultBuilder(args)
               .ConfigureAppConfiguration((ctx, builder) =>
               {
                   var keyVaultEndpoint = GetKeyVaultEndpoint();
                   if (!string.IsNullOrEmpty(keyVaultEndpoint))
                   {
                       var azureServiceTokenProvider = new AzureServiceTokenProvider();
                       var keyVaultClient = new KeyVaultClient(
                           new KeyVaultClient.AuthenticationCallback(
                               azureServiceTokenProvider.KeyVaultTokenCallback));
                       builder.AddAzureKeyVault(
                           keyVaultEndpoint, keyVaultClient, new DefaultKeyVaultSecretManager());
                   }
               }
            ).UseStartup<Startup>()
             .Build();

        private static string GetKeyVaultEndpoint() => "https://<YourKeyVaultName>.vault.azure.net";
    }
```
3. Nästa gång du öppnar About.cshtml.cs fil och skriva följande kod
    1. Innehåller en referens till Microsoft.Extensions.Configuration av detta med hjälp av instruktionen    
        ```
        using Microsoft.Extensions.Configuration
        ```
    2. Lägg till den här konstruktorn
        ```
        public AboutModel(IConfiguration configuration)
        {
            _configuration = configuration;
        }
        ```
    3. Uppdatera metoden OnGet. Uppdatera värdet för platshållaren som visas här med det hemliga namnet som du skapade i kommandona ovan
        ```
        public void OnGet()
        {
            //Message = "Your application description page.";
            Message = "My key val = " + _configuration["<YourSecretNameThatWasCreatedAbove>"];
        }
        ```

Kör appen lokalt genom att bläddra till om sida. Du bör din hemliga värdet som hämtas

## <a name="clean-up-resources"></a>Rensa resurser

När den inte längre behövs tar du bort resursgruppen. Detta tar bort Nyckelvalvet och relaterade resurser. Så här tar du bort resursgruppen via portalen:

1. Skriv namnet på resursgruppen i rutan Sök längst upp i portalen. När du ser resursgruppen du använde i den här snabbstarten bland sökresultaten väljer du den.
2. Välj **Ta bort resursgrupp**.
3. I rutan **SKRIV RESURSGRUPPSNAMNET:** skriver du namnet på resursgruppen och väljer **Ta bort**.

## <a name="next-steps"></a>Nästa steg

Läs mer om Key Vault-utveckling genom att läsa den [Utvecklarguide för Key Vault](key-vault-developers-guide.md)