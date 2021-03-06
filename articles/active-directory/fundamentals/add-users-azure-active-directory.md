---
title: Lägga till eller ta bort användare i Azure Active Directory | Microsoft Docs
description: Lär dig mer om att lägga till nya användare eller ta bort befintliga användare med Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/04/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: 782363144a6b1dd87aff515c38588b6ce70b61bd
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46295112"
---
# <a name="how-to-add-or-delete-users-using-azure-active-directory"></a>Så här: lägga till eller ta bort användare som använder Azure Active Directory
Lägga till nya användare eller ta bort befintliga användare från din Azure Active Directory (Azure AD)-klient med hjälp av Azure AD.

## <a name="add-a-new-user"></a>Lägg till en ny användare
Du kan skapa en ny användare med Azure Active Directory.

### <a name="to-add-a-new-user"></a>Lägga till en ny användare
1. Logga in på den [Azure-portalen](https://portal.azure.com/) som en Global administratör eller Användaradministratör för katalogen.

2. Välj **Azure Active Directory**väljer **användare**, och välj sedan **ny användare**.

    ![Användare - sidan för alla användare med nya användaren markerat](media/add-users-azure-active-directory/new-user-all-users-blade.png)

3. På den **användaren** fyller du i informationen som krävs.

    ![Lägg till ny användare, användarsidan med användarinformation](media/add-users-azure-active-directory/new-user-user-blade.png)

    - **Namn (krävs).** Första och sista namnet på den nya användaren. Till exempel Mary Parker.

    - **Användarnamn (krävs).** Användarnamn för den nya användaren. Till exempel mary@contoso.com. 
    
        Domändelen av användarnamnet måste använda antingen det initiala standarddomännamnet, <_domännamn_>. onmicrosoft.com eller ett anpassat domännamn, till exempel contoso.com. Läs mer om hur du skapar ett anpassat domännamn, [lägga till ett anpassat domännamn i Azure Active Directory](add-custom-domain.md).

    - **Profil.** Alternativt kan du lägga till mer information om användaren. Du kan också lägga till användarinformation vid ett senare tillfälle. Läs mer om att lägga till användarinformation [lägga till eller ändra information om användarprofiler](active-directory-users-profile-azure-portal.md).

    - **Grupper.** Du kan också kan du lägga till användaren till en eller flera befintliga grupper. Du kan också lägga till användaren till grupper vid ett senare tillfälle. Mer information om att lägga till användare till grupper finns i [hur du skapar en grundläggande grupp och Lägg till medlemmar](active-directory-groups-create-azure-portal.md).

    - **Katalogroll.** Alternativt kan du lägga till användaren till en directory-roll. Du kan tilldela användare till global administratör, eller till en eller flera av de andra administratörsroller i Azure AD. Mer information om hur du tilldelar roller finns i [tilldela roller till användare](active-directory-users-assign-role-azure-portal.md).

4. Kopiera det automatiskt genererade lösenordet som angavs i den **lösenord** box. Du behöver ge det här lösenordet till användaren för första inloggningsprocess.

5. Välj **Skapa**.

    Användaren skapas och läggs till din Azure AD-klient.

## <a name="add-a-new-user-within-a-hybrid-environment"></a>Lägg till en ny användare i en hybridmiljö
Om du har en miljö med både Azure Active Directory (moln) och Windows Server Active Directory (lokalt), kan du lägga till nya användare genom att synkronisera befintliga konto användardata. Mer information om hybridmiljöer och användare finns i [integrerar dina lokala kataloger med Azure Active Directory](../hybrid/whatis-hybrid-identity.md).

## <a name="delete-a-user"></a>Ta bort en användare
Du kan ta bort en befintlig användare med Azure Active Directory.

### <a name="to-delete-a-user"></a>Ta bort en användare
1. Logga in på den [Azure-portalen](https://portal.azure.com/) med ett konto som Global administratör för katalogen.

2. Välj **Azure Active Directory**väljer **användare**, och sök sedan efter och välj den användare som du vill ta bort från Azure AD-klienten. Till exempel _Mary Parker_.

3. Välj **ta bort användaren**.

    ![Användare - sidan för alla användare med ta bort användaren markerat](media/add-users-azure-active-directory/delete-user-all-users-blade.png)

    Användaren tas bort och inte längre visas på den **användare – alla användare** sidan. Användaren visas på den **borttagna användare** för de närmaste 30 dagarna och kan återställas under den tiden. Mer information om hur du återställer en användare finns i [återställa eller ta bort en nyligen borttagna användare permanent](active-directory-users-restore.md).

    >[!Note]
    >Du måste använda Windows Server Active Directory för att uppdatera identitet, kontaktinformation eller jobbinformation för användare vars auktoritetskälla är Windows Server Active Directory. När du har slutfört uppdateringen måste du vänta tills nästa synkroniseringscykel ska slutföras innan ändringarna visas.

## <a name="next-steps"></a>Nästa steg
När du har lagt till användarna, kan du utföra följande basic-processer:

- [Lägga till eller ändra profilinformation](active-directory-users-profile-azure-portal.md)

- [Tilldela roller till användare](active-directory-users-assign-role-azure-portal.md)

- [Skapa en basgrupp och lägga till medlemmar](active-directory-groups-create-azure-portal.md)

- [Arbeta med dynamiska grupper och användare](../users-groups-roles/groups-create-rule.md)

Eller du kan utföra andra hanteringsuppgifter för användaren, till exempel [att lägga till gästanvändare från en annan katalog](../b2b/what-is-b2b.md) eller [återställa en borttagen användare](active-directory-users-restore.md). Läs mer om andra tillgängliga åtgärder, [Azure Active Directory management supportdokumentation](../users-groups-roles/index.yml).