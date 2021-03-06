---
title: Tilldela åtkomst till data i Azure Cost Management | Microsoft Docs
description: Den här artikeln vägleder dig även om att tilldela behörighet till Azure Cost Management-data för olika scope.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 11/09/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: a62ecd0201d6b29686abc186ee69f3d26dc5ca4e
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/10/2018
ms.locfileid: "51516293"
---
# <a name="assign-access-to-cost-management-data"></a>Tilldela åtkomst till Cost Management-data

För de flesta användare definiera en kombination av behörigheter som beviljas i Azure-portalen och Enterprise (EA)-portalen en användares åtkomstnivå till data i Azure Cost Management. Den här artikeln vägleder dig även om tilldelar åtkomst till Cost Management-data. När en kombination av behörigheter tilldelas baserat vyer användardata i Cost Management omfattningen som de har åtkomst till och omfattning att de väljer i Azure-portalen.

Omfattningen som en användare väljer används i hela kostnadshantering datakonsolidering och styra åtkomsten till kostnadsinformation. När du använder omfång kan användare inte flerval dem. I stället väljer de ett större område som underordnade omfattningar kavla upp till och sedan de filter ned för att de vill visa. Konsolidera data är viktigt att förstå eftersom vissa användare inte ska ha åtkomst till en överordnad omfattning som underordnade omfattningar slås upp till.

## <a name="cost-management-scopes"></a>Cost Management-scope

Om du vill visa kostnadsdata, måste en användare ha minst skrivskyddad åtkomst till en eller flera av följande omfattningar.

| **Omfång** | **Definieras på** | **Behörighet att visa data** | **Nödvändig EA-inställning** | **Konsoliderar data till** |
| --- | --- | --- | --- | --- |
| Faktureringskonto<sup>1</sup> | [https://ea.azure.com](https://ea.azure.com/) | Företagsadministratör | Ingen | Alla prenumerationer från Enterprise-avtalet |
| Avdelning | [https://ea.azure.com](https://ea.azure.com/) | Avdelningsadministratör | **Visa debiteringar DA** aktiverat | Alla prenumerationer som tillhör ett registreringskonto som är länkade till avdelningen |
| Registreringskonto<sup>2</sup> | [https://ea.azure.com](https://ea.azure.com/) | Kontoägare | **Visa debiteringar AO** aktiverat | Alla prenumerationer från registreringskontot |
| Hanteringsgrupp | [https://portal.azure.com](https://portal.azure.com/) | Cost Management-läsare (eller Läsare) | **Visa debiteringar AO** aktiverat | Alla prenumerationer under hanteringsgruppen |
| Prenumeration | [https://portal.azure.com](https://portal.azure.com/) | Cost Management-läsare (eller Läsare) | **Visa debiteringar AO** aktiverat | Alla resurser/resursgrupper i prenumerationen |
| Resursgrupp | [https://portal.azure.com](https://portal.azure.com/) | Cost Management-läsare (eller Läsare) | **Visa debiteringar AO** aktiverat | Alla resurser i resursgruppen |

<sup>1</sup> faktureringskontot också kallas Enterprise-avtal eller registrering.

<sup>2</sup> registreringskontot kallas också ägare.

## <a name="enable-access-to-costs-in-the-ea-portal"></a>Aktivera åtkomst till kostnader i EA-portalen

Fakturering kontoomfånget kräver den **DA visa debiteringar** alternativet **aktiverad** i EA-portalen. Alla omfång kräver den **AO visa debiteringar** alternativet **aktiverad** i EA-portalen.

För att aktivera ett alternativ:

1. Logga in på EA-portalen på [ https://ea.azure.com ](https://ea.azure.com) med ett enterprise-administratörskonto.
2. Välj **hantera** i den vänstra rutan.
3. För kostnadshantering scope som du vill ge åtkomst för att aktivera alternativet kostnad till **DA visa debiteringar** och/eller **AO visa debiteringar**.  
    ![Registrering fliken visar DA och AO vy debiterar alternativ](./media/assign-access-acm-data/ea-portal-enrollment-tab.png)

När Visningsalternativ för kostnad är aktiverade, måste de flesta omfång också rollbaserad åtkomstkontroll (RBAC) behörighet konfigureras i Azure-portalen.

## <a name="enterprise-administrator-role"></a>Enterprise-administratörsrollen

En företagsadministratör har åtkomst till faktureringskonto (Enterprise Agreement/registrering) och alla andra scope, som är underordnade omfattningar som standard. Enterprise-administratören tilldelar åtkomst till omfång för andra användare. Som bästa praxis för affärskontinuitet, bör du alltid ha två användare med administratörsbehörighet för företag. I följande avsnitt är hanteringspaketen exempel på enterprise-administratören tilldela åtkomst till omfattningar för andra användare.

## <a name="assign-billing-account-scope-access"></a>Tilldela fakturering kontoåtkomst omfång

Åtkomst till faktureringen kontoomfånget kräver administratörsbehörighet för företag i EA-portalen. Enterprise-administratörer har behörighet att visa kostnaderna över hela EA-avtal eller flera registreringar. Ingen åtgärd krävs i Azure-portalen för fakturering kontoomfånget.

1. Logga in på EA-portalen på [ https://ea.azure.com ](https://ea.azure.com) med ett enterprise-administratörskonto.
2. Välj **hantera** i den vänstra rutan.
3. På den **registrering** väljer du en registreringen som du vill hantera.  
    ![EA-portalen](./media/assign-access-acm-data/ea-portal.png)
4. Klicka på **+ Lägg till administratör**.
5. Välj autentiseringstyp och ange användarens e-postadress i rutan Lägg till administratör.
6. Om användaren ska ha skrivskyddad åtkomst till data för kostnader och användning, under **skrivskyddad**väljer **Ja**.  Annars väljer **nr**.
7. Klicka på **Lägg till** att skapa kontot.  
    ![Lägg till administratör box](./media/assign-access-acm-data/add-admin.png)

Det kan ta upp till 30 minuter innan den nya användaren kan komma åt data i Cost Management.

### <a name="assign-department-scope-access"></a>Tilldela avdelning begränsa åtkomsten

Åtkomst till området avdelning kräver avdelning (DA visa debiteringar) administratörsåtkomst i EA-portalen. Avdelning administratörer har behörighet att visa kostnader och användningsdata som är associerat med en avdelning eller till flera avdelningar.  Data för avdelningen innehåller alla prenumerationer som hör till ett konto för enhetsregistreringshanterare som är länkade till avdelningen. Ingen åtgärd krävs i Azure-portalen.

1. Logga in på EA-portalen på [ https://ea.azure.com ](https://ea.azure.com) med ett enterprise-administratörskonto.
2. Välj **hantera** i den vänstra rutan.
3. På den **registrering** väljer du en registreringen som du vill hantera.
4. Klicka på den **avdelning** fliken och klicka sedan på **Lägg till administratör**.
5. Välj autentiseringstyp och Skriv användarens e-postadress i rutan Lägg till avdelningen administratör.
6. Om användaren ska ha skrivskyddad åtkomst till data för kostnader och användning, under **skrivskyddad**väljer **Ja**.  Annars väljer **nr**.
7. Välj de avdelningar som du vill bevilja avdelning administrativ behörighet till.
8. Klicka på **Lägg till** att skapa kontot.  
    ![Lägg till administratör rutan för avdelning](./media/assign-access-acm-data/add-depart-admin.png)

## <a name="assign-enrollment-account-scope-access"></a>Tilldela registreringsåtkomst konto omfång

Åtkomst till kontoomfånget registreringen kräver kontoåtkomst ägare (AO visa debiteringar) i EA-portalen. Ägare kan visa kostnader och användningsdata som är associerad med ett konto för enhetsregistreringshanterare. Konto för enhetsregistreringshanterare innehåller alla Azure-prenumerationer som är kopplad till registreringen. Ingen åtgärd krävs i Azure-portalen.

1. Logga in på EA-portalen på [ https://ea.azure.com ](https://ea.azure.com) med ett enterprise-administratörskonto.
2. Välj **hantera** i den vänstra rutan.
3. På den **registrering** väljer du en registreringen som du vill hantera.
4. Klicka på den **konto** fliken och klicka sedan på **Lägg till konto**.
5. I rutan Lägg till konto väljer du den **avdelning** du associerar kontot eller lämna den som otilldelade.
6. Välj autentiseringstyp och ange namnet på kontot.
7. Skriv användarens e-postadress och du kan också ange kostnadsstället.
8. Klicka på **Lägg till** att skapa kontot.  
    ![Lägg till konto box](./media/assign-access-acm-data/add-account.png)

## <a name="assign-management-group-scope-access"></a>Tilldela åtkomst till hanteringsgruppen omfång

Åtkomst till en grupp hanteringsomfång kräver minst Cost Management Reader (eller läsare) behörighet. Du kan konfigurera behörighet till hanteringsgrupp i Azure-portalen. Du måste ha minst behörigheten deltagare till hanteringsgruppen för att aktivera åtkomst för andra. Och du måste också har aktiverat den **AO visa debiteringar** i EA-portalen.

1. Logga in på Azure Portal på [http://portal.azure.com](http://portal.azure.com).
2. Välj **alla tjänster** i sidopanelen, söker du efter _hanteringsgrupper_och välj sedan **hanteringsgrupper**.
3. Välj hanteringsgruppen i hierarkin.
4. Bredvid namnet på hanteringsgruppen, klickar du på **information**.
5. Välj **åtkomstkontroll (IAM)** i den vänstra rutan.
6. Klicka på **Lägg till**.
7. Under **rollen**väljer **Cost Management läsare**.
8. Under **tilldela åtkomst till**väljer **Azure AD-användare, grupp eller program**.
9. Tilldela åtkomst genom att söka efter och välj sedan användaren.
10. Klicka på **Spara**.  
    ![Lägg till behörigheter](./media/assign-access-acm-data/add-permissions.png)

## <a name="assign-subscription-scope-access"></a>Tilldela omfång prenumerationsåtkomst

Åtkomst till en prenumeration måste minst Cost Management Reader (eller läsare) behörighet. Du kan konfigurera behörighet till en prenumeration på Azure-portalen. Du måste ha minst deltagarbehörighet för prenumerationen för att aktivera åtkomst för andra. Och du måste också har aktiverat den **AO visa debiteringar** i EA-portalen.

1. Logga in på Azure Portal på [http://portal.azure.com](http://portal.azure.com).
2. Välj **alla tjänster** i sidopanelen, söker du efter _prenumerationer_och välj sedan **prenumerationer**.
3. Välj din prenumeration.
4. Välj **åtkomstkontroll (IAM)** i den vänstra rutan.
5. Klicka på **Lägg till**.
6. Under **rollen**väljer **Cost Management läsare**.
7. Under **tilldela åtkomst till**väljer **Azure AD-användare, grupp eller program**.
8. Tilldela åtkomst genom att söka efter och välj sedan användaren.
9. Klicka på **Spara**.

## <a name="assign-resource-group-scope-access"></a>Tilldela resursåtkomst grupp omfång

Åtkomst till en resursgrupp måste minst Cost Management Reader (eller läsare) behörighet. Du kan konfigurera behörigheter för en resursgrupp i Azure-portalen. Du måste ha minst behörigheten deltagare till resursgruppen för att aktivera åtkomst för andra. Och du måste också har aktiverat den **AO visa debiteringar** i EA-portalen.

1. Logga in på Azure Portal på [http://portal.azure.com](http://portal.azure.com).
2. Välj **alla tjänster** i sidopanelen, söker du efter _resursgrupper_och välj sedan **resursgrupper**.
3. Välj din resursgrupp.
4. Välj **åtkomstkontroll (IAM)** i den vänstra rutan.
5. Klicka på **Lägg till**.
6. Under **rollen**väljer **Cost Management läsare**.
7. Under **tilldela åtkomst till**väljer **Azure AD-användare, grupp eller program**.
8. Tilldela åtkomst genom att söka efter och välj sedan användaren.
9. Klicka på **Spara**.

## <a name="next-steps"></a>Nästa steg

- Om du inte har redan slutfört den första snabbstarten för kostnadshantering, kan du läsa den på [börja analysera kostnaderna](quick-acm-cost-analysis.md).
