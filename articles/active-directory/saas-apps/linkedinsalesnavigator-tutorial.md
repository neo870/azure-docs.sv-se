---
title: 'Självstudier: Azure Active Directory-integration med LinkedIn Sales Navigator | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och LinkedInSalesNavigator.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 7a9fa8f3-d611-4ffe-8d50-04e9586b24da
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: jeedes
ms.openlocfilehash: f0e34a614251cf11c9547d749fef58dfa8ca623a
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39425205"
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-sales-navigator"></a>Självstudier: Azure Active Directory-integration med LinkedIn Sales Navigator

I den här självstudien får lära du att integrera LinkedIn Sales Navigator med Azure Active Directory (AD Azure).

Integrera LinkedIn Sales Navigator med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till LinkedIn Sales Navigator
- Du kan aktivera användarna att automatiskt få loggat in på LinkedIn Sales Navigator (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton på en central plats – Azure portal

Om du vill veta mer om integrering av SaaS-app med Azure AD, Bläddra [vad är programåtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Förutsättningar

Om du vill konfigurera Azure AD-integrering med LinkedIn Sales Navigator, behöver du följande objekt:

- En Azure AD-prenumeration
- En LinkedIn Sales Navigator enkel inloggning aktiverad prenumeration

> [!NOTE]
> Om du vill testa stegen i den här självstudien rekommenderar vi inte med hjälp av en produktionsmiljö.

Om du vill testa stegen i den här självstudien bör du följa dessa rekommendationer:

- Undvik att använda i produktionsmiljön, om det inte behövs.
- Om du inte har en Azure AD-utvärderingsmiljö kan du få en månads utvärdering [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I den här självstudien kan du testa Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här självstudien består av två viktigaste byggstenarna:

1. Att lägga till LinkedIn Sales Navigator från galleriet
1. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-linkedin-sales-navigator-from-the-gallery"></a>Att lägga till LinkedIn Sales Navigator från galleriet
Om du vill konfigurera integreringen av LinkedIn Sales Navigator till Azure AD, som du behöver lägga till LinkedIn Sales Navigator från galleriet i din lista över hanterade SaaS-appar.

**Utför följande steg för att lägga till LinkedIn Sales Navigator från galleriet:**

1. I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Active Directory][1]

1. Gå till **företagsprogram**. Gå till **alla program**.

    ![Program][2]
    
1. Klicka på **nytt program** knappen överst i dialogrutan.

    ![Program][3]

1. I sökrutan skriver **LinkedIn Sales Navigator**.

    ![Skapa en Azure AD-användare för testning](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_search.png)

1. I resultatpanelen väljer **LinkedIn Sales Navigator**, och klicka sedan på **Lägg till** för att lägga till programmet.

    ![Skapa en Azure AD-användare för testning](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet ska du konfigurera och testa Azure AD enkel inloggning med LinkedIn Sales Navigator baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning att fungera, behöver Azure AD du veta vad användaren motsvarighet i LinkedIn Sales Navigator är till en användare i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och relaterade användaren i LinkedIn Sales Navigator upprättas.

Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i LinkedIn Sales Navigator.

Om du vill konfigurera och testa Azure AD enkel inloggning med LinkedIn Sales Navigator, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  – om du vill ge användarna använda den här funktionen.
1. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  – om du vill testa Azure AD enkel inloggning med Britta Simon.
1. **[Skapa en testanvändare LinkedIn Sales Navigator](#creating-a-linkedin-sales-navigator-test-user)**  – du har en motsvarighet för Britta Simon i LinkedIn Sales Navigator som är länkad till en Azure AD-representation av användaren.
1. **[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  – om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
1. **[Testa enkel inloggning](#testing-single-sign-on)**  – om du vill kontrollera om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet ska du aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt LinkedIn Sales Navigator-program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med LinkedIn Sales Navigator:**

1. I Azure-portalen på den **LinkedIn Sales Navigator** program integration-sidan klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

1. På den **enkel inloggning** dialogrutan i **läge** Välj **SAML-baserad inloggning** att aktivera enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_samlbase.png)

1. I ett annat webbläsarfönster inloggning till din **LinkedIn Sales Navigator** webbplats som administratör.

1. I **Kontocenter**, klickar du på **globala inställningar** under **inställningar**. Markera också **Sales Navigator** från den nedrullningsbara listan.

    ![Konfigurera enkel inloggning](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_01.png)

1. Klicka på **eller klicka här om du vill läsa in och kopiera enskilda fält i formuläret** och kopiera **entitets-Id** och **Assertion konsument Access (ACS) Url**.

    ![Konfigurera enkel inloggning](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_031.png)

1. På Azure-portalen under **LinkedIn Sales Navigator domän och URL: er** avsnittet, utför följande steg om du vill konfigurera programmet i **IDP** initierade läge.

    ![Konfigurera enkel inloggning](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url1.png)

    a. I den **identifierare** textrutan anger du den **entitets-ID** kopieras från LinkedIn-portalen 

    b. I den **svars-URL** textrutan anger du den **Assertion konsument Access (ACS) Url** kopieras från LinkedIn-portalen

1. Kontrollera **visa avancerade URL-inställningar**, om du vill konfigurera programmet i **SP** initierade läge.

    ![Konfigurera enkel inloggning](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url2.png)

    I den **inloggnings-URL** textrutan skriver du värdet med följande mönster: `https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`

1. Din **LinkedIn Sales Navigator** program som förväntar SAML-intyg i ett visst format, vilket kräver att du kan lägga till anpassade attributmappningar i SAML-tokenattribut konfigurationen. Följande skärmbild visar ett exempel. Standardvärdet för **användaridentifierare** är **user.userprincipalname** men LinkedIn Sales Navigator förväntar sig den mappas med användarens e-postadress. Du kan använda **user.mail** attribut i listan eller Använd rätt attribut-värde baserat på konfigurationen för din organisation. 

    ![Konfigurera enkel inloggning](./media/linkedinsalesnavigator-tutorial/updateusermail.png)
    
1. I **användarattribut** klickar du på **visa och redigera alla andra användarattribut** och ange attribut. Användaren behöver för att lägga till fyra anspråk med namnet **e-post**, **avdelning**, **firstname**, och **lastname** och värdet är mappas med **user.mail**, **user.department**, **user.givenname**, och **user.surname** respektive

    | Attributnamn | Attributvärde |
    | --- | --- |    
    | e-post| User.Mail |
    | Avdelning| User.Department |
    | Förnamn| User.givenName |
    | Efternamn| User.surname |
    
    ![Skapa en Azure AD-användare för testning](./media/linkedinsalesnavigator-tutorial/userattribute.png)
    
    a. Klicka på **lägga till attributet** att öppna dialogrutan attribut.
    
    ![Skapa en Azure AD-användare för testning](./media/linkedinsalesnavigator-tutorial/tutorial_attribute_04.png)
    
    ![Skapa en Azure AD-användare för testning](./media/linkedinsalesnavigator-tutorial/tutorial_attribute_05.png)
   
    b. I den **namn** textrutan skriver du attributnamnet som visas för den raden.
    
    c. Från den **värdet** anger attributvärdet som visas för den raden.
    
    d. Klicka på **Ok**

1. Utför följande steg på den **namn** attributet -

    a. Klicka på attribut så öppnas den **redigera attributet** fönster.

    ![Konfigurera enkel inloggning](./media/linkedinsalesnavigator-tutorial/url_update.png)

    b. Ta bort URL-värdet från den **namnområde**.
    
    c. Klicka på **Ok** att spara inställningen.

1. På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara XML-filen på datorn.

    ![Konfigurera enkel inloggning](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_certificate.png) 

1. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/linkedinsalesnavigator-tutorial/tutorial_general_400.png)

1. Gå till **LinkedIn administratörsinställningar** avsnittet. Klicka på **ladda upp XML-filen** att överföra Metadata-XML-filen som du har hämtat från Azure-portalen.

    ![Konfigurera enkel inloggning](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_metadata_03.png)

1. Klicka på **på** att aktivera enkel inloggning. SSO status ändras från **inte ansluten** till **ansluten**

    ![Konfigurera enkel inloggning](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_05.png)


> [!TIP]
> Du kan läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du ställer in appen!  När du lägger till den här appen från den **Active Directory > företagsprogram** bara klickar du på den **enkel inloggning** fliken och komma åt den inbäddade dokumentationen genom den  **Konfigurationen** avsnittet längst ned. Du kan läsa mer om här funktionen embedded-dokumentation: [Azure AD embedded-dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en Azure AD-användare för testning
Målet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.

![Skapa en Azure AD-användare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I den **Azure-portalen**, i det vänstra navigeringsfönstret klickar du på **Azure Active Directory** ikon.

    ![Skapa en Azure AD-användare för testning](./media/linkedinsalesnavigator-tutorial/create_aaduser_01.png) 

1. Gå till **användare och grupper** och klicka på **alla användare**.
    
    ![Skapa en Azure AD-användare för testning](./media/linkedinsalesnavigator-tutorial/create_aaduser_02.png) 

1. Överst i dialogrutan klickar du på **Lägg till** att öppna den **användaren** dialogrutan.
 
    ![Skapa en Azure AD-användare för testning](./media/linkedinsalesnavigator-tutorial/create_aaduser_03.png) 

1. På den **användaren** dialogrutan utför följande steg:
 
    ![Skapa en Azure AD-användare för testning](./media/linkedinsalesnavigator-tutorial/create_aaduser_04.png) 

    a. I den **namn** textrutan typ **BrittaSimon**.

    b. I den **användarnamn** textrutan skriver den **e-postadress** av BrittaSimon.

    c. Välj **visa lösenord** och anteckna värdet för den **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-linkedin-sales-navigator-test-user"></a>Skapa en testanvändare LinkedIn Sales Navigator

Länkade Sales Navigator programmet stöder bara i tid (JIT) användaretablering och -autentiserade användare skapas automatiskt i programmet. Aktivera **automatiskt tilldela licenser** tilldela en licens till användaren.
   
   ![Skapa en Azure AD-användare för testning](./media/linkedinsalesnavigator-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet ska aktivera du Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till LinkedIn Sales Navigator.

![Tilldela användare][200] 

**Om du vill tilldela Britta Simon på LinkedIn Sales Navigator, utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** klickar **alla program**.

    ![Tilldela användare][201] 

1. I listan med program väljer **LinkedIn Sales Navigator**.

    ![Konfigurera enkel inloggning](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_app.png) 

1. I menyn till vänster, klickar du på **användare och grupper**.

    ![Tilldela användare][202] 

1. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg till tilldelning** dialogrutan.

    ![Tilldela användare][203]

1. På **användare och grupper** dialogrutan **Britta Simon** på listan användare.

1. Klicka på **Välj** knappen **användare och grupper** dialogrutan.

1. Klicka på **tilldela** knappen **Lägg till tilldelning** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet ska testa du Azure AD enkel inloggning för konfigurationen med hjälp av åtkomstpanelen.

När du klickar på LinkedIn Sales Navigator-panelen i åtkomstpanelen, bör du omdirigeras till organisationens sida där du måste ange din personliga LinkedIn-kontoinformation. Den länkar ditt personliga konto med ditt företag LinkedIn-konto. Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över guider om hur du integrerar SaaS-appar med Azure Active Directory](tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_01.png
[2]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_02.png
[3]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_03.png
[4]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_04.png

[100]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_100.png

[200]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_200.png
[201]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_201.png
[202]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_202.png
[203]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_203.png

