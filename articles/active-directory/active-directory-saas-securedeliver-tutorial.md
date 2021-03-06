<properties
	pageTitle="Tutorial: Azure Active Directory-Integration mit Novatus | Microsoft Azure"
	description="Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und SECURE DELIVER konfigurieren."
	services="active-directory"
	documentationCenter=""
	authors="jeevansd"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/07/2016"
	ms.author="jeedes"/>


# Tutorial: Azure Active Directory-Integration mit SECURE DELIVER

Dieses Tutorial soll Ihnen zeigen, wie Sie SECURE DELIVER in Azure Active Directory (Azure AD) integrieren. Die Integration von SECURE DELIVER in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf SECURE DELIVER hat
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei SECURE DELIVER anzumelden (einmaliges Anmelden)
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## Voraussetzungen

Um die Azure AD-Integration mit SECURE DELIVER konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure-Abonnement
- Ein SECURE DELIVER-Abonnement, für das das einmalige Anmelden aktiviert ist


> [AZURE.NOTE] Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.


## Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Azure AD in einer Testumgebung zu testen. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von SECURE DELIVER aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## Hinzufügen von SECURE DELIVER aus dem Katalog
Zum Konfigurieren der Integration von SECURE DELIVER in Azure AD müssen Sie SECURE DELIVER aus dem Katalog zu der Liste der verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um SECURE DELIVER aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

	![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

	![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen**.

	![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

	![Anwendungen][4]

6. Geben Sie im Suchfeld als Suchbegriff **SECURE DELIVER** ein.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_01.png)

7. Wählen Sie im Ergebnisbereich **SECURE DELIVER** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
	
	![App-Logo und -Name im Katalog](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_06.png)

##  Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt soll anhand eines Testbenutzers namens Britta Simon veranschaulicht werden, wie das einmalige Anmelden bei Azure AD in SECURE DELIVER konfiguriert und getestet werden kann.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in SECURE DELIVER als Gegenbenutzer zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SECURE DELIVER muss eine Linkbeziehung eingerichtet werden. Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert des **Benutzernamens** in SECURE DELIVER zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei SECURE DELIVER müssen Sie die folgenden Schritte ausführen:

1. **[Konfigurieren von Azure AD – einmaliges Anmelden](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines SECURE DELIVER-Testbenutzers](#creating-a-secure-deliver-test-user)**, um eine Entsprechung von Britta Simon in SECURE DELIVER zu erstellen, die mit ihrer Darstellung in Azure AD verknüpft ist.
5. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### Konfigurieren des einmaligen Anmeldens von Azure AD

Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden von Azure AD im klassischen Azure-Portal zu aktivieren und in Ihrer SECURE DELIVER-Anwendung zu konfigurieren.



**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in SECURE DELIVER die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **SECURE DELIVER** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.

	![Einmaliges Anmelden konfigurieren][6]

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei SECURE DELIVER anmelden?** die Option **Azure AD – einmaliges Anmelden**, und klicken Sie dann auf **Weiter**.
 
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_03.png)

3. Führen Sie auf der Seite **App-Einstellungen konfigurieren** die folgenden Schritte aus, und klicken Sie dann auf **Weiter**:
 
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_04.png)

    a. Geben Sie im Textfeld **Anmelde-URL** die von Ihren Benutzern für die Anmeldung bei Ihrer SECURE DELIVER-Anwendung verwendete URL in folgendem Format ein: **https://i-securedeliver.jp/sd/<Unternehmensname>/jsf/login/sso**.

    b. Wenden Sie sich über [iw-sd-support@fujifilm.com](mailto:iw-sd-support@fujifilm.com) an das SECURE DELIVER-Supportteam, wenn Sie den Wert Ihres URL-Mandanten nicht kennen.

	c. Geben Sie im Textfeld **Bezeichner** die Mandanten-URL ein.

	d. Klicken Sie auf **Weiter**.


4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für bei SECURE DELIVER** die folgenden Schritte aus, und klicken Sie dann auf **Weiter**:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_05.png)

    a. Klicken Sie auf **Zertifikat herunterladen** und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5. Wenden Sie sich über [iw-sd-support@fujifilm.com](mailto:iw-sd-support@fujifilm.com) an das Supportteam von SECURE DELIVER, um SSO (Single Sign-On, einmaliges Anmelden) für Ihre Anwendung konfigurieren zu lassen, und stellen Sie Folgendes bereit:
	
	• Die heruntergeladene Zertifikatdatei

	• Die **Entitäts-ID**

	• Die **Dienst-URL für einmaliges Anmelden**

	• Die **Dienst-URL für einmaliges Abmelden**



6. Wählen Sie im klassischen Azure-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.

	![Azure AD – einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.
  
	![Azure AD – einmaliges Anmelden][11]




### Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im klassischen Azure-Portal eine Testbenutzerin namens Britta Simon erstellt.

![Azure AD-Benutzer erstellen][20]

**Um einen SECURE DELIVER-Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_09.png)

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
  
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_03.png)

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_04.png)

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_05.png)

    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.

    b. Geben Sie in das Textfeld **Benutzername** den Text **BrittaSimon** ein.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_06.png)

    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.

    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_07.png)

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:

	![Erstellen einesAzure AD-Testbenutzers](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_08.png)

    a. Notieren Sie den Wert von **Neues Kennwort**.

    b. Klicken Sie auf **Fertig stellen**.



### Erstellen eines SECURE DELIVER-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in SECURE DELIVER. Arbeiten Sie mit dem Supportteam von SECURE DELIVER zusammen, um Benutzer im SECURE DELIVER-Konto hinzuzufügen.

> [AZURE.NOTE] Wenn Sie einen Benutzer manuell erstellen müssen, setzen Sie sich mit dem Supportteam von SECURE DELIVER in Verbindung.


### Zuweisen des Azure AD-Testbenutzers

Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens von Azure zu ermöglichen, indem sie Zugriff auf SECURE DELIVER erhält.

![Benutzer zuweisen][200]

**Um Britta Simon SECURE DELIVER zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

	![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **SECURE DELIVER** aus.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_50.png)

1. Klicken Sie im oberen Menü auf **Benutzer**.

	![Benutzer zuweisen][203]

1. Wählen Sie in der Benutzerliste **Britta Simon** aus.

2. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.

	![Benutzer zuweisen][205]



### Testen der einmaligen Anmeldung

In diesem Abschnitt soll Ihre Azure AD-Konfiguration für das einmalige Anmelden mithilfe des Zugriffsbereichs getestet werden. Wenn Sie im Zugriffsbereich auf die Kachel SECURE DELIVER klicken, sollten Sie automatisch bei Ihrer SECURE DELIVER-Anwendung angemeldet werden.


## Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0713_2016-->