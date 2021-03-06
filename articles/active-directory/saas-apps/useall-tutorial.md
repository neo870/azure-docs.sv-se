---
title: 'Självstudier: Azure Active Directory-integration med Useall | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Useall.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8dd9e452-a5b6-4a16-a97c-b60211ea6b95
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: jeedes
ms.openlocfilehash: 8deacddc9dcc7571ea725143fa9a9c002c1602b1
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/01/2018
ms.locfileid: "50743340"
---
# <a name="tutorial-azure-active-directory-integration-with-useall"></a>Självstudier: Azure Active Directory-integration med Useall

I den här självstudien får du lära dig hur du integrerar Useall med Azure Active Directory (AD Azure).

Integrera Useall med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till Useall.
- Du kan aktivera användarna att automatiskt få loggat in på Useall (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton på en central plats – Azure portal.

Om du vill veta mer om integrering av SaaS-app med Azure AD finns i [vad är programåtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Förutsättningar

Om du vill konfigurera Azure AD-integrering med Useall, behöver du följande objekt:

- En Azure AD-prenumeration
- En Useall enkel inloggning aktiverat prenumeration

> [!NOTE]
> Om du vill testa stegen i den här självstudien rekommenderar vi inte med hjälp av en produktionsmiljö.

Om du vill testa stegen i den här självstudien bör du följa dessa rekommendationer:

- Använd inte din produktionsmiljö, om det inte behövs.
- Om du inte har en Azure AD-utvärderingsmiljö, kan du [få en månads utvärdering](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning

I den här självstudien kan du testa Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här självstudien består av två viktigaste byggstenarna:

1. Att lägga till Useall från galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-useall-from-the-gallery"></a>Att lägga till Useall från galleriet

För att konfigurera integrering av Useall i Azure AD, som du behöver lägga till Useall från galleriet i din lista över hanterade SaaS-appar.

**Utför följande steg för att lägga till Useall från galleriet:**

1. I den **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.

    ![Azure Active Directory-knappen][1]

2. Gå till **företagsprogram**. Gå till **alla program**.

    ![Bladet för Enterprise-program][2]

3. Lägg till nytt program, klicka på **nytt program** knappen överst i dialogrutan.

    ![Knappen Nytt program][3]

4. I sökrutan skriver **Useall**väljer **Useall** resultatet panelen klickar **Lägg till** för att lägga till programmet.

    ![Useall i resultatlistan](./media/useall-tutorial/tutorial_useall_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet ska du konfigurera och testa Azure AD enkel inloggning med Useall baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning att fungera, behöver Azure AD du veta vad användaren motsvarighet i Useall är till en användare i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och relaterade användaren i Useall upprättas.

Om du vill konfigurera och testa Azure AD enkel inloggning med Useall, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  – om du vill ge användarna använda den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  – om du vill testa Azure AD enkel inloggning med Britta Simon.
3. **[Skapa testanvändare Useall](#creating-useall-test-user)**  – du har en motsvarighet för Britta Simon i Useall som är länkad till en Azure AD-representation av användaren.
4. **[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  – om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  – om du vill kontrollera om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Useall program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med Useall:**

1. I Azure-portalen på den **Useall** program integration-sidan klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning för länken][4]

2. På den **väljer du en metod för enkel inloggning** dialogrutan klickar du på **Välj** för **SAML** läge för att aktivera enkel inloggning.

    ![Konfigurera enkel inloggning](common/tutorial_general_301.png)

3. På den **ange in enkel inloggning med SAML** klickar du på **redigera** ikonen för att öppna **SAML grundkonfiguration** dialogrutan.

    ![Konfigurera enkel inloggning](common/editconfigure.png)

4. På den **SAML grundkonfiguration** avsnittet, utför följande steg:

    ![Useall domän och URL: er med enkel inloggning för information](./media/useall-tutorial/tutorial_useall_url.png)

    a. I den **inloggnings-URL** textrutan anger du ett URL med hjälp av följande mönster: `https://<SUBDOMAIN>.useall.com.br/tenant/useall`

    b. I den **identifierare** textrutan anger du ett URL med hjälp av följande mönster: `https://<SUBDOMAIN>.useall.com.br/tenant/apiuseall/saml2`

    > [!NOTE]
    > Dessa värden är inte verkliga. Uppdatera dessa värden med faktiska inloggnings-URL och identifierare. Kontakta [Useall supportteamet](mailto:luizotavio@useall.com.br) att hämta dessa värden.

5. På den **ange in enkel inloggning med SAML** sidan den **SAML-signeringscertifikat** klickar du på kopieringsknappen för att kopiera **Appfederationsmetadata** och spara den på din dator.

    ![Länk för hämtning av certifikat](./media/useall-tutorial/tutorial_useall_certificate.png)

6. Att konfigurera enkel inloggning på **Useall** sida, som du behöver skicka de hämtade **Appfederationsmetadata** till [Useall supportteamet](mailto:luizotavio@useall.com.br). De ställer du in SAML SSO ansluta till korrekt inställda på båda sidorna.

### <a name="creating-an-azure-ad-test-user"></a>Skapa en Azure AD-användare för testning

Målet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.

1. I Azure-portalen, i den vänstra rutan väljer **Azure Active Directory**väljer **användare**, och välj sedan **alla användare**.

    ![Skapa en Azure AD-användare][100]

2. Välj **ny användare** överst på skärmen.

    ![Skapa en Azure AD-användare för testning](common/create_aaduser_01.png)

3. Utför följande steg i egenskaperna för användaren.

    ![Skapa en Azure AD-användare för testning](common/create_aaduser_02.png)

    a. I den **namn** anger **BrittaSimon**.
  
    b. I den **användarnamn** skriver **brittasimon@yourcompanydomain.extension**  
    Till exempel, BrittaSimon@contoso.com

    c. Välj **egenskaper**väljer den **Show lösenord** kryssrutan och sedan skriva ned det värde som visas i rutan lösenord.

    d. Välj **Skapa**.
  
### <a name="creating-useall-test-user"></a>Skapa Useall testanvändare

I det här avsnittet skapar du en användare som kallas Britta Simon i Useall. Arbeta med [Useall supportteamet](mailto:luizotavio@useall.com.br) att lägga till användare i Useall-plattformen. Användare måste skapas och aktiveras innan du använder enkel inloggning.

### <a name="assigning-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet ska aktivera du Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Useall.

1. I Azure-portalen väljer du **företagsprogram**väljer **alla program**.

    ![Tilldela användare][201]

2. I listan med program väljer **Useall**.

    ![Konfigurera enkel inloggning](./media/useall-tutorial/tutorial_useall_app.png)

3. I menyn till vänster, klickar du på **användare och grupper**.

    ![Tilldela användare][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg till tilldelning** dialogrutan.

    ![Tilldela användare][203]

5. I den **användare och grupper** dialogrutan Välj **Britta Simon** i listan över användare och klicka på den **Välj** längst ned på skärmen.

6. I den **Lägg till tilldelning** dialogrutan Välj den **tilldela** knappen.

### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet ska testa du Azure AD enkel inloggning för konfigurationen med hjälp av åtkomstpanelen.

När du klickar på panelen Useall i åtkomstpanelen du bör få automatiskt loggat in på ditt Useall program.
Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över guider om hur du integrerar SaaS-appar med Azure Active Directory](tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[200]: common/tutorial_general_200.png
[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png