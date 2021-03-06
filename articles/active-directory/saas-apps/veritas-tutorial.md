---
title: 'Tutorial: Azure Active Directory-Integration mit Veritas Enterprise Vault.cloud SSO | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Veritas Enterprise Vault.cloud SSO konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 70adbcd8c25b3acb4408447070d3b0397d258847
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56167455"
---
# <a name="tutorial-azure-active-directory-integration-with-veritas-enterprise-vaultcloud-sso"></a>Tutorial: Azure Active Directory-Integration mit Veritas Enterprise Vault.cloud SSO

In diesem Tutorial erfahren Sie, wie Sie Veritas Enterprise Vault.cloud SSO in Azure Active Directory (Azure AD) integrieren.

Die Integration von Enterprise Vault.cloud SSO in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Veritas Enterprise Vault.cloud SSO hat.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Veritas Enterprise Vault.cloud SSO anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Veritas Enterprise Vault.cloud SSO konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Veritas Enterprise Vault.cloud SSO-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Veritas Enterprise Vault.cloud SSO aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-veritas-enterprise-vaultcloud-sso-from-the-gallery"></a>Hinzufügen von Veritas Enterprise Vault.cloud SSO aus dem Katalog
Zum Konfigurieren der Integration von Veritas Enterprise Vault.cloud SSO in Azure AD müssen Sie Veritas Enterprise Vault.cloud SSO aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um Veritas Enterprise Vault.cloud SSO aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![ANWENDUNGEN][3]

1. Geben Sie in das Suchfeld **Veritas Enterprise Vault.cloud SSO**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/veritas-tutorial/tutorial_veritas_search.png)

1. Wählen Sie im Ergebnisbereich die Option **Veritas Enterprise Vault.cloud SSO** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/veritas-tutorial/tutorial_veritas_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens Britta Simon das einmalige Anmelden von Azure AD mit Veritas Enterprise Vault.cloud SSO.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Veritas Enterprise Vault.cloud SSO als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Veritas Enterprise Vault.cloud SSO muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Veritas Enterprise Vault.cloud SSO den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD mit Veritas Enterprise Vault.cloud SSO müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
1. **[Erstellen eines Veritas Enterprise Vault.cloud SSO-Testbenutzers](#creating-a-veritas-enterprise-vaultcloud-sso-test-user)**, um eine Entsprechung von Britta Simon in Veritas Enterprise Vault.cloud SSO zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Veritas Enterprise Vault.cloud SSO-Anwendung.

**Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Veritas Enterprise Vault.cloud SSO zu konfigurieren:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Veritas Enterprise Vault.cloud SSO** auf **Einmaliges Anmelden**.

    ![Configure single sign-on][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Configure single sign-on](./media/veritas-tutorial/tutorial_veritas_samlbase.png)

1. Führen Sie auf der Seite **Domäne und URLs für Veritas Enterprise Vault.cloud SSO** die folgenden Schritte aus:

    ![Configure single sign-on](./media/veritas-tutorial/tutorial_veritas_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://personal.ap.archive.veritas.com/CID=<CUSTOMERID>`.

    b. Verwenden Sie im Textfeld **Bezeichner** die richtige URL für das Rechenzentrum.

    | Datacenter| URL |
    |----------|----|
    | Nordamerika| `https://auth.lax.archivecloud.net` |
    | Europa | `https://auth.ams.archivecloud.net` |
    | Asien-Pazifik| `https://auth.syd.archivecloud.net`|

    c. Verwenden Sie im Textfeld **Antwort-URL** die richtige URL für das Rechenzentrum.

    | Datacenter| URL |
    |----------|----|
    | Nordamerika| `https://auth.lax.archivecloud.net` |
    | Europa | `https://auth.ams.archivecloud.net` |
    | Asien-Pazifik| `https://auth.syd.archivecloud.net`|
    
    > [!NOTE] 
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Wenden Sie sich an das [Clientsupportteam von Veritas Enterprise Vault.cloud SSO](https://www.veritas.com/support/.html), um diesen Wert zu erhalten. 

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Configure single sign-on](./media/veritas-tutorial/tutorial_veritas_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Configure single sign-on](./media/veritas-tutorial/tutorial_general_400.png)

1. Klicken Sie im Abschnitt **Veritas Enterprise Vault.cloud SSO-Konfiguration** auf **Veritas Enterprise Vault.cloud SSO konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Configure single sign-on](./media/veritas-tutorial/tutorial_veritas_configure.png) 

1. Zum Konfigurieren des einmaligen Anmeldens bei **Veritas Enterprise Vault.cloud SSO** müssen Sie das heruntergeladene **Zertifikat (Base64)** und die **SAML-Dienst-URL für einmaliges Anmelden** an das [Supportteam von Veritas Enterprise Vault.cloud SSO](https://www.veritas.com/support/.html) senden.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Dokumentation zu eingebettetem Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/veritas-tutorial/create_aaduser_01.png) 

1. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/veritas-tutorial/create_aaduser_02.png) 

1. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/veritas-tutorial/create_aaduser_03.png) 

1. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/veritas-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**.
 
### <a name="creating-a-veritas-enterprise-vaultcloud-sso-test-user"></a>Erstellen eines Testbenutzers für Veritas Enterprise Vault.cloud SSO

In diesem Abschnitt erstellen Sie in Enterprise Vault.cloud SSO einen Benutzer mit dem Namen „Britta Simon“. Wenden Sie sich an das  [Supportteam von Veritas Enterprise Vault.cloud SSO](https://www.veritas.com/support/.html), um die Benutzer zur Veritas Enterprise Vault.cloud SSO-Plattform hinzuzufügen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Veritas Enterprise Vault.cloud SSO gewähren.

![Benutzer zuweisen][200] 

**Führen Sie die folgenden Schritte aus, um Britta Simon Veritas Enterprise Vault.cloud SSO zuzuweisen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste die Option **Veritas Enterprise Vault.cloud SSO** aus.

    ![Configure single sign-on](./media/veritas-tutorial/tutorial_veritas_app.png) 

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Veritas Enterprise Vault.cloud SSO“ klicken, sollten Sie automatisch bei Ihrer Veritas Enterprise Vault.cloud SSO-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/veritas-tutorial/tutorial_general_01.png
[2]: ./media/veritas-tutorial/tutorial_general_02.png
[3]: ./media/veritas-tutorial/tutorial_general_03.png
[4]: ./media/veritas-tutorial/tutorial_general_04.png

[100]: ./media/veritas-tutorial/tutorial_general_100.png

[200]: ./media/veritas-tutorial/tutorial_general_200.png
[201]: ./media/veritas-tutorial/tutorial_general_201.png
[202]: ./media/veritas-tutorial/tutorial_general_202.png
[203]: ./media/veritas-tutorial/tutorial_general_203.png

