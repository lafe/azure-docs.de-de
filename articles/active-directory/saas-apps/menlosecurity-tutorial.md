---
title: 'Tutorial: Azure Active Directory-Integration mit Menlo Security | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Menlo Security konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 9e63fe6b-0ad0-405d-9e41-6a1a40a41df8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e779e4fab06802212514af94cafc1b016b737ca4
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56183903"
---
# <a name="tutorial-azure-active-directory-integration-with-menlo-security"></a>Tutorial: Azure Active Directory-Integration mit Menlo Security

In diesem Tutorial erfahren Sie, wie Sie Menlo Security in Azure Active Directory (Azure AD) integrieren.

Die Integration von Menlo Security in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Menlo Security hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Menlo Security anzumelden (einmaliges Anmelden, Single Sign-On).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zum Integrieren von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Menlo Security konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Menlo Security-Abonnement, das für das einmalige Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Menlo Security aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-menlo-security-from-the-gallery"></a>Hinzufügen von Menlo Security aus dem Katalog
Zum Konfigurieren der Integration von Menlo Security in Azure AD müssen Sie Menlo Security aus dem Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Menlo Security aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![ANWENDUNGEN][3]

1. Geben Sie im Suchfeld den Suchbegriff **Menlo Security**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/menlosecurity-tutorial/tutorial_menlosecurity_search.png)

1. Wählen Sie im Ergebnisbereich die Option **Menlo Security** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/menlosecurity-tutorial/tutorial_menlosecurity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Menlo Security basierend auf einer Testbenutzerin mit dem Namen Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Menlo Security als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Menlo Security muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert für den **Benutzernamen** in Azure AD als Wert für den **Benutzernamen** in Menlo Security zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Menlo Security müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
1. **[Erstellen eines Menlo Security-Testbenutzers](#creating-a-menlo-security-test-user)**, um eine Entsprechung von Britta Simon in Menlo Security zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Menlo Security-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Menlo Security die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Menlo Security** auf **Einmaliges Anmelden**.

    ![Configure single sign-on][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Configure single sign-on](./media/menlosecurity-tutorial/tutorial_menlosecurity_samlbase.png)

1. Führen Sie auf der Seite **Domäne und URLs für Menlo Security** die folgenden Schritte aus:

    ![Configure single sign-on](./media/menlosecurity-tutorial/tutorial_menlosecurity_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<subdomain>.menlosecurity.com/account/login`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<subdomain>.menlosecurity.com/safeview-auth-server/saml/metadata`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Supportteam von Menlo Security Client](https://www.menlosecurity.com/menlo-contact), um diese Werte zu erhalten. 
 
1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Configure single sign-on](./media/menlosecurity-tutorial/tutorial_menlosecurity_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Configure single sign-on](./media/menlosecurity-tutorial/tutorial_general_400.png)

1. Klicken Sie im Abschnitt **Menlo Security-Konfiguration** auf **Menlo Security konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **SAML-Entitäts-ID** und die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Configure single sign-on](./media/menlosecurity-tutorial/tutorial_menlosecurity_configure.png) 

1. Um das einmalige Anmelden auf der Seite von **Menlo Security** zu konfigurieren, melden Sie sich bei der **Menlo Security**-Website als Administrator an.

1. Wechseln Sie unter **Settings** zu **Authentication**, und führen Sie folgende Aktionen aus:
    
    ![Configure single sign-on](./media/menlosecurity-tutorial/menlo_user_setup.png)

    a. Aktivieren Sie das Kontrollkästchen **Enable user authentication using SAML**.

    b. Wählen Sie für **Allow External Access** die Option **Yes**.

    c. Wählen Sie für **SAML Provider** die Option **Azure Active Directory** aus.

    d. **SAML 2.0-Endpunkt**: Fügen Sie die **URL für den SAML-SSO-Dienst** ein, die Sie aus dem Azure-Portal kopiert haben.

    e. **Dienstbezeichner (Aussteller)**: Fügen Sie die **SAML-Entitäts-ID** ein, die Sie aus dem Azure-Portal kopiert haben.

    f. **X.509-Zertifikat**: Öffnen Sie das **Zertifikat (Base64)**, das Sie aus dem Azure-Portal heruntergeladen haben, im Editor, und fügen Sie den Inhalt in dieses Feld ein.

    g. Klicken Sie auf **Speichern** , um die Einstellungen zu speichern.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Dokumentation zu eingebettetem Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/menlosecurity-tutorial/create_aaduser_01.png) 

1. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/menlosecurity-tutorial/create_aaduser_02.png) 

1. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/menlosecurity-tutorial/create_aaduser_03.png) 

1. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/menlosecurity-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**.
 
### <a name="creating-a-menlo-security-test-user"></a>Erstellen eines Menlo Security-Testbenutzers
 
In diesem Abschnitt erstellen Sie in Menlo Security eine Benutzerin namens Britta Simon. Arbeiten Sie mit dem  [Supportteam von Menlo Security Client](https://www.menlosecurity.com/menlo-contact) zusammen, um die Benutzer zur Menlo Security-Plattform hinzuzufügen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können. 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Menlo Security gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon zu Menlo Security zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **Menlo Security**aus.

    ![Configure single sign-on](./media/menlosecurity-tutorial/tutorial_menlosecurity_app.png) 

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für das einmalige Anmelden.

Öffnen Sie ein Browserfenster im „privaten“ oder „Inkognito“-Modus, um eine neue Authentifizierung auszulösen.  Verwenden Sie in Internet Explorer STRG+UMSCHALT+P.  Verwenden Sie in Chrome STRG+UMSCHALT+N.  Browsen Sie im privaten Fenster zu einer geschützten Ressource, und führen Sie eine Azure AD-Anmeldung durch.  Nach der erfolgreichen Anmeldung werden Sie in einer isolierten Sitzung zu der angeforderten Website weitergeleitet.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/menlosecurity-tutorial/tutorial_general_01.png
[2]: ./media/menlosecurity-tutorial/tutorial_general_02.png
[3]: ./media/menlosecurity-tutorial/tutorial_general_03.png
[4]: ./media/menlosecurity-tutorial/tutorial_general_04.png

[100]: ./media/menlosecurity-tutorial/tutorial_general_100.png

[200]: ./media/menlosecurity-tutorial/tutorial_general_200.png
[201]: ./media/menlosecurity-tutorial/tutorial_general_201.png
[202]: ./media/menlosecurity-tutorial/tutorial_general_202.png
[203]: ./media/menlosecurity-tutorial/tutorial_general_203.png

