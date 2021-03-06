---
title: Vad är standardbehörigheterna för användare i Azure Active Directory? | Microsoft Docs
description: Läs mer om de olika användarbehörigheterna som är tillgängliga i Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: lizross
ms.reviewer: vincesm
custom: it-pro
ms.openlocfilehash: 9a9bd33dce4b2be9e15df2745dd25338cdb42702
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/17/2018
ms.locfileid: "45730487"
---
# <a name="what-are-the-default-user-permissions-in-azure-active-directory"></a>Vad är standardbehörigheterna för användare i Azure Active Directory?

Alla användare beviljas en uppsättning standardbehörigheter i Azure Active Directory (AD Azure). En användares behörighet består av typen av användare, deras [rollmedlemskap](https://docs.microsoft.com/azure/active-directory/active-directory-users-assign-role-azure-portal) och deras ägarskap för enskilda objekt. Den här artikeln beskriver dessa standardbehörigheter och innehåller en jämförelse av standardinställningar för medlem och gäst.

## <a name="member-and-guest-users"></a>Medlems- och gästanvändare
Uppsättningen standardbehörigheter beror på om användaren är en medlem på klienten (medlemsanvändare) eller om användaren är en gäst i samband med B2B-samarbete (gästanvändare). Läs mer om B2B-samarbete i [Vad är Azure AD B2B-samarbete?](../b2b/what-is-b2b.md) för mer information om gästanvändare). 
* Medlemsanvändare kan registrera program, hantera sina egna profilbilder och mobila telefonnummer, ändra sina egna lösenord och bjuda in B2B gäster. Dessutom kan användare läsa all kataloginformation (med några få undantag). 
* Azure AD B2B-gästanvändare har begränsade behörigheter för katalogen. Till exempel kan gästanvändare kan inte bläddra i information från klienten utanför sin egen profilinformation. Däremot kan en gästanvändare hämta information om en annan användare genom att ange användarens huvudnamn eller objekt-ID. En gäst kan inte visa någon information om andra klientobjekt, till exempel grupper och program.

Standardbehörigheter för gäster är begränsade som standard. Gäster kan läggas till i administratörsroller som beviljar dem fullständig läs- och skrivbehörighet i rollen. Det finns en ytterligare begränsning, möjligheten för gäster att bjuda in andra gäster. Om du ställer in **Gäster kan bjuda in** på **Nej** kan gäster inte bjuda in andra gäster. Se [Delegera inbjudningar till B2B-samarbete](../b2b/delegate-invitations.md) för att lära dig hur. Om du vill ge gästanvändare samma behörigheter som medlemsanvändare som standard anger du **Gästanvändarnas behörigheter begränsas** till **Nej**. Den här inställningen ger alla behörigheter för medlemsanvändare till gästanvändare som standard och gör att gäster kan läggas till i administrativa roller.

## <a name="compare-member-and-guest-default-permissions"></a>Jämför standardbehörigheter för medlemmar och gästanvändare

**Område** | **Användarbehörigheter för medlem** | **Användarbehörigheter för gäst**
------------ | --------- | ----------
Användare och kontakter | Läsa alla offentliga egenskaper om användare och kontakter<br>Bjuda in gäster<br>Ändra eget lösenord<br>Hantera eget mobilnummer<br>Hantera eget foto<br>Ogiltigförklara egna uppdateringstokens | Läsa egna egenskaper<br>Läs visningsnamn, e-post, inloggningsnamn, foto, användarens huvudnamn och användartypegenskaper för andra användare och kontakter<br>Ändra eget lösenord
Grupper | Skapa säkerhetsgrupper<br>Skapa Office 365-grupper<br>Läsa alla icke-dolda egenskaper i grupper<br>Läsa icke-dolda gruppmeddelanden<br>Läsa dolda Office 365-gruppmedlemskap för domänansluten grupp<br>Hantera egenskaper, ägarskap och medlemskap egna i grupper<br>Lägga till gäster i egna grupper<br>Hantera inställningar för dynamiskt medlemskap<br>Ta bort egna grupper<br>Återställa grupper som ägs av Office 365 | Läsa alla icke-dolda egenskaper i grupper<br>Läsa icke-dolda gruppmeddelanden<br>Läsa dolda Office 365-gruppmedlemskap för domänansluten grupp<br>Hantera egna grupper<br>Lägga till gäster i egna grupper (om detta är tillåtet)<br>Ta bort egna grupper<br>Återställa grupper som ägs av Office 365 
Program | Registrera (Skapa) ett nytt program<br>Skrivskyddade egenskaper för registrerade program och företagsprogram<br>Hantera egenskaper för program, tilldelningar och autentiseringsuppgifter för egna program<br>Skapa eller ta bort programlösenord för användare<br>Ta bort egna program<br>Återställ egna program | Skrivskyddade egenskaper för registrerade program och företagsprogram<br>Hantera egenskaper för program, tilldelningar och autentiseringsuppgifter för egna program<br>Ta bort egna program<br>Återställ egna program
Enheter | Läs alla enhetsegenskaper<br>Läs alla egenskaper för egna enheter<br> | Inga behörigheter<br>Ta bort egna enheter<br>
Katalog | Läs all företagsinformation<br>Läsa alla domäner<br>Läsa alla partnerkontrakt | Läsa visningsnamn och verifierade domäner
Roller och omfattningar | Läsa alla administrativa roller och medlemskap<br>Läsa alla egenskaper och medlemskap i administrativa enheter | Inga behörigheter 
Prenumerationer | Läsa alla prenumerationer<br>Aktivera tjänsteplanmedlem | Inga behörigheter
Principer | Läs alla principegenskaper<br>Hantera alla egenskaper för egna principer | Inga behörigheter

## <a name="to-restrict-the-default-permissions-for-member-users"></a>Så här begränsar du standardbehörigheter för medlemsanvändare

Standardbehörigheter för medlemmsanvändare kan begränsas på följande sätt.

Behörighet | Förklaring av inställning
---------- | ------------
Möjlighet att skapa säkerhetsgrupper | När den här inställningen är inställd på Nej kan användare inte skapa säkerhetsgrupper. Globala administratörer eller användarkontoadministratörer kan fortfarande skapa säkerhetsgrupper. Se [Azure Active Directory-cmdletar för att konfigurera gruppinställningar](../users-groups-roles/groups-settings-cmdlets.md) för att lära dig hur du ska gå tillväga.
Får skapa Office 365-grupper | Om det här alternativet är Nej får användare inte skapa Office 365-grupper. Om det här alternativet är Vissa får en uppsättning användare skapa Office 365-grupper. Globala administratörer eller användarkontoadministratörer kommer fortfarande att kunna skapa grupper för Office 365. Se [Azure Active Directory-cmdletar för att konfigurera gruppinställningar](../users-groups-roles/groups-settings-cmdlets.md) för att lära dig hur du ska gå tillväga.
Begränsa åtkomst till Azure AD-administrationsportalen | Ange det här alternativet på Nej förhindrar användare från att komma åt Azure Active Directory.
Möjlighet att läsa andra användare | Den här inställningen är endast tillgänglig i PowerShell. Om den är $false får inga icke-administratörer läsa användarinformation från katalogen. Detta förhindrar inte läsning av användarinformation i andra Microsofttjänster, till exempel Exchange Online. Den här inställningen är avsedd för särskilda omständigheter och det rekommenderas inte att ställa in den på $false.

## <a name="object-ownership"></a>Objektägarskap

### <a name="application-registration-owner-permissions"></a>Ägarbehörigheter för programregistrering
När en användare registrerar ett program, läggs de automatiskt till som ägare till programmet. De kan hantera metadata för programmet som ägare, till exempel namn och behörigheter för appen. De kan också hantera klient-specifik konfiguration av program, till exempel SSO-konfiguration och användartilldelningar. En ägare kan också lägga till eller ta bort andra ägare. Till skillnad från globala administratörer kan ägare bara hantera program som de äger.

<!-- ### Enterprise application owner permissions

When a user adds a new enterprise application, they are automatically added as an owner for the tenant-specific configuration of the application. As an owner, they can manage the tenant-specific configuration of the application, such as the SSO configuration, provisioning, and user assignments. An owner can also add or remove other owners. Unlike Global Administrators, owners can manage only the applications they own. <!--To assign an enterprise application owner, see *Assigning Owners for an Application*.-->

### <a name="group-owner-permissions"></a>Gruppägares behörigheter

När en användare skapar en grupp, läggs de automatiskt som en ägare till gruppen. Som ägare kan de hantera gruppens egenskaper, till exempel namn, samt hantera medlemskap. En ägare kan också lägga till eller ta bort andra ägare. Till skillnad från globala administratörer och användarkontoadministratörer kan ägare bara hantera grupper som de äger. Om du vill tilldela en gruppägare, se [Hantera ägare för en grupp](active-directory-accessmanagement-managing-group-owners.md).

## <a name="next-steps"></a>Nästa steg

* Mer information om hur du tilldelar administratörsroller i Azure AD finns [tilldela en användare till administratörsroller i Azure Active Directory](active-directory-users-assign-role-azure-portal.md)
* Mer information om hur resursåtkomsten hanteras i Microsoft Azure finns i [Förstå resursåtkomst i Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Mer information om hur Azure Active Directory relaterar till din Azure-prenumeration finns i [Hur Azure-prenumerationer är associerade med Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
* [Hantera användare](add-users-azure-active-directory.md)
