---
title: 'Tutorial: Azure Active Directory-Integration mit Front | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Front konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 88270b6d-2571-434a-b139-b6ccc3a2b19f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7edc02f6291decf28c4b3d27277545b9ad31c537
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56183648"
---
# <a name="tutorial-azure-active-directory-integration-with-front"></a>Tutorial: Azure Active Directory-Integration mit Front

In diesem Tutorial erfahren Sie, wie Sie Front in Azure Active Directory (Azure AD) integrieren.

Die Integration von Front in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Front hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Front anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Front konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Front-Abonnement, das für das einmalige Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Front aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-front-from-the-gallery"></a>Hinzufügen von Front aus dem Katalog
Zum Konfigurieren der Integration von Front in Azure AD müssen Sie Front aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Front aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

1. Geben Sie im Suchfeld **Front** ein, wählen Sie im Ergebnisbereich **Front** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Front in der Ergebnisliste](./media/front-tutorial/tutorial_front_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Front mithilfe einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Front als Pendant eines Benutzers in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Front muss eine Linkbeziehung eingerichtet werden.

Weisen Sie den Wert für **Benutzername** aus Azure AD in Front als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Front müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
1. **[Erstellen eines Front-Testbenutzers](#create-a-front-test-user)**, um ein Pendant von Britta Simon in Front zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Front-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Front die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Front** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/front-tutorial/tutorial_front_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für Front** die folgenden Schritte durch:

    ![Configure single sign-on](./media/front-tutorial/tutorial_front_url1.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<companyname>.frontapp.com`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<companyname>.frontapp.com/sso/saml/callback`
     
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL. Diese werden später in diesem Tutorial erläutert. Wenden Sie sich alternativ an das [Front-Client-Supportteam](mailto:support@frontapp.com), um diese Werte zu erhalten. 

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Configure single sign-on](./media/front-tutorial/tutorial_front_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Configure single sign-on](./media/front-tutorial/tutorial_general_400.png)
    
1. Klicken Sie im Abschnitt **Front-Konfiguration** auf **Front konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Configure single sign-on](./media/front-tutorial/tutorial_front_configure.png) 

1. Melden Sie sich bei Ihrem Front-Mandanten als Administrator an.

1. Wechseln Sie zu **Settings (Zahnradsymbol unten auf der linken Randleiste) > Preferences** (Einstellungen > Voreinstellungen).
   
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/front-tutorial/tutorial_front_000.png)

1. Klicken Sie auf den Link **Single Sign On** .
   
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/front-tutorial/tutorial_front_001.png)

1. Wählen Sie in der Dropdownliste für **Einmaliges Anmelden** den Eintrag **SAML** aus.
   
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/front-tutorial/tutorial_front_002.png)

1. Geben Sie im Textfeld **Einstiegspunkt** den Wert für die **Dienst-URL für einmaliges Anmelden** aus dem Konfigurations-Assistenten der Azure AD-Anwendung ein.
    
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/front-tutorial/tutorial_front_003.png)

1. Öffnen Sie die heruntergeladene Datei **Zertifikat (Base64)** in Editor, kopieren Sie den Inhalt in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **Signaturzertifikat** ein.
    
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/front-tutorial/tutorial_front_004.png)

1. Führen Sie im Abschnitt **Service provider settings** die folgenden Schritte aus:

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/front-tutorial/tutorial_front_005.png)

    a. Kopieren Sie den Wert von **Entity ID**, und fügen Sie ihn im Azure-Portal im Abschnitt **Domänen und URLs für Front** in das Textfeld **Bezeichner** ein.

    b. Kopieren Sie den Wert von **ACS-URL**, und fügen Sie ihn im Azure-Portal im Abschnitt **Domänen und URLs für Front** in das Textfeld **Antwort-URL** ein.
    
1. Klicken Sie auf die Schaltfläche **Save** .

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Dokumentation zu eingebettetem Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/front-tutorial/create_aaduser_01.png)

1. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/front-tutorial/create_aaduser_02.png)

1. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/front-tutorial/create_aaduser_03.png)

1. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/front-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-a-front-test-user"></a>Erstellen eines Front-Testbenutzers

In diesem Abschnitt erstellen Sie in Front einen Benutzer namens Britta Simon. Arbeiten Sie mit dem  [Supportteam für den Front-Client](mailto:support@frontapp.com) zusammen, um die Benutzer zur Front-Plattform hinzuzufügen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Front gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon Front zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste den Eintrag **Front** aus.

    ![Front-Link in der Anwendungsliste](./media/front-tutorial/tutorial_front_app.png)  

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Front“ klicken, sollten Sie automatisch bei Ihrer Front-Anwendung angemeldet werden. 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/front-tutorial/tutorial_general_01.png
[2]: ./media/front-tutorial/tutorial_general_02.png
[3]: ./media/front-tutorial/tutorial_general_03.png
[4]: ./media/front-tutorial/tutorial_general_04.png

[100]: ./media/front-tutorial/tutorial_general_100.png

[200]: ./media/front-tutorial/tutorial_general_200.png
[201]: ./media/front-tutorial/tutorial_general_201.png
[202]: ./media/front-tutorial/tutorial_general_202.png
[203]: ./media/front-tutorial/tutorial_general_203.png

