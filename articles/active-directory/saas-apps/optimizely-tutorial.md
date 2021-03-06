---
title: 'Tutorial: Azure Active Directory-Integration mit Optimizely | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Optimizely konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 28ef03e1-9aad-4301-af97-d94e853edc74
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 534a373024556db86d0553b68ce3f847ff085486
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56210712"
---
# <a name="tutorial-azure-active-directory-integration-with-optimizely"></a>Tutorial: Azure Active Directory-Integration mit Optimizely

In diesem Tutorial erfahren Sie, wie Sie Optimizely Learn in Azure Active Directory (Azure AD) integrieren.

Die Integration von Optimizely in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Optimizely hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Optimizely anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Optimizely konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Optimizely-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung.
Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Optimizely aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-optimizely-from-the-gallery"></a>Hinzufügen von Optimizely aus dem Katalog

Zum Konfigurieren der Integration von Optimizely in Azure AD müssen Sie Optimizely aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um Optimizely aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![ANWENDUNGEN][3]

4. Geben Sie im Suchfeld als Suchbegriff **Optimizely**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/optimizely-tutorial/tutorial_optimizely_search.png)

5. Wählen Sie im Ergebnisbereich die Option **Optimizely** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/optimizely-tutorial/tutorial_optimizely_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Optimizely basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Optimizely als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Optimizely muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert des **Benutzernamens** in Optimizely zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Optimizely müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. **[Erstellen eines Optimizely-Testbenutzers](#creating-an-optimizely-test-user)**, um eine Entsprechung von Britta Simon in Optimizely zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Optimizely-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Optimizely die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Optimizely** auf **Einmaliges Anmelden**.

    ![Configure single sign-on][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Configure single sign-on](./media/optimizely-tutorial/tutorial_optimizely_samlbase.png)

3. Führen Sie die folgenden Schritte auf der Seite **Domäne und URLs für Optimizely** aus:

    ![Configure single sign-on](./media/optimizely-tutorial/tutorial_optimizely_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://app.optimizely.net/<instance name>`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `urn:auth0:optimizely:contoso`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Sie aktualisieren den Wert mit der tatsächlichen Anmelde-URL und dem tatsächlichen Bezeichner. Dies wird später in diesem Tutorial beschrieben.

4. Die Anwendung Optimizely erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie für diese Anwendung die folgenden Ansprüche. Sie können die Werte dieser Attribute über den Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:
    
    ![Configure single sign-on](./media/optimizely-tutorial/tutorial_optimizely_attribute.png)
    
5. Aktivieren Sie im Abschnitt **Benutzerattribute** das Kontrollkästchen **Alle weiteren Benutzerattribute anzeigen und bearbeiten**, um die Attribute zu erweitern. Führen Sie die folgenden Schritte für jedes der angezeigten Attribute aus:

    | Attributname | Attributwert |
    | ---------------| --------------- |
    | E-Mail | user.mail |

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Configure single sign-on](./media/optimizely-tutorial/tutorial_attribute_04.png)

    ![Configure single sign-on](./media/optimizely-tutorial/tutorial_attribute_05.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten **Attributnamen** ein.

    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.

    d. Klicken Sie auf **OK**.

6. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Configure single sign-on](./media/optimizely-tutorial/tutorial_optimizely_certificate.png)

7. Klicken Sie auf die Schaltfläche **Save** .

    ![Configure single sign-on](./media/optimizely-tutorial/tutorial_general_400.png)

8. Klicken Sie im Abschnitt **Optimizely-Konfiguration** auf **Optimizely konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Configure single sign-on](./media/optimizely-tutorial/tutorial_optimizely_configure.png)

9. Um einmaliges Anmelden auf der Seite von **Optimizely** zu konfigurieren, wenden Sie sich an Ihren Kundenbetreuer von Optimizely, und stellen Sie das heruntergeladene **Zertifikat (Base64)** und die **SAML-Dienst-URL für einmaliges Anmelden** bereit.

10. Als Antwort auf Ihre E-Mail stellt Optimizely Ihnen die Anmelde-URL (SP-initiiertes SSO) und den Bezeichner (ID der Dienstanbieterentität).

    a. Kopieren Sie die von Optimizely bereitgestellte **SP-initiierte SSO-URL**, und fügen Sie sie im Azure-Portal im Abschnitt **Domäne und URLs für Optimizely** im Textfeld **Anmelde-URL** ein.

    b. Kopieren Sie die von Optimizely bereitgestellte **ID der Dienstanbieterentität**, und fügen Sie sie im Azure-Portal im Abschnitt **Domäne und URLs für Optimizely** im Textfeld **Bezeichner** ein.

11. Melden Sie sich in einem anderen Browserfenster in Ihrer Optimizely-Anwendung als Administrator an.

12. Klicken Sie in der rechten oberen Ecke auf den Namen Ihres Kontos und dann auf **Account Settings**.

    ![Azure AD – einmaliges Anmelden](./media/optimizely-tutorial/tutorial_optimizely_09.png)

13. Aktivieren Sie auf der Registerkarte „Account“ das Kontrollkästchen **Enable SSO** (SSO aktivieren), das Sie im Abschnitt **Overview (Übersicht)** unter „Single Sign On“ finden.
  
    ![Azure AD – einmaliges Anmelden](./media/optimizely-tutorial/tutorial_optimizely_10.png)

14. Klicken Sie unten auf der Seite auf **Speichern**.

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/optimizely-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/optimizely-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/optimizely-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/optimizely-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**.

### <a name="creating-an-optimizely-test-user"></a>Erstellen eines Optimizely-Testbenutzers

In diesem Abschnitt erstellen Sie in Optimizely einen Benutzer mit dem Namen Britta Simon.

1. Klicken Sie auf der Startseite auf **Collaborators**.

2. Klicken Sie auf **New Collaborator**, um einen neuen Projektmitarbeiter zum Projekt hinzuzufügen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/optimizely-tutorial/create_aaduser_10.png)

3. Geben Sie die E-Mail-Adresse ein, und weisen Sie ihr eine Rolle zu. Klicken Sie auf **Einladen**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/optimizely-tutorial/create_aaduser_11.png)

4. Der Benutzer erhält eine E-Mail-Einladung. Der Benutzer muss sich mit der E-Mail-Adresse bei Optimizely anmelden.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf Optimizely gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon zu Optimizely zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **Optimizely**aus.

    ![Configure single sign-on](./media/optimizely-tutorial/tutorial_optimizely_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Optimizely“ klicken, sollten Sie automatisch bei Ihrer Optimizely-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/optimizely-tutorial/tutorial_general_01.png
[2]: ./media/optimizely-tutorial/tutorial_general_02.png
[3]: ./media/optimizely-tutorial/tutorial_general_03.png
[4]: ./media/optimizely-tutorial/tutorial_general_04.png

[100]: ./media/optimizely-tutorial/tutorial_general_100.png

[200]: ./media/optimizely-tutorial/tutorial_general_200.png
[201]: ./media/optimizely-tutorial/tutorial_general_201.png
[202]: ./media/optimizely-tutorial/tutorial_general_202.png
[203]: ./media/optimizely-tutorial/tutorial_general_203.png
