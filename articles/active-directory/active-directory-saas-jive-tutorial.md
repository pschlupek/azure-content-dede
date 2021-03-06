<properties
	pageTitle="Tutorial: Azure Active Directory-Integration mit Jive | Microsoft Azure"
	description="Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Jive konfigurieren."
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
	ms.date="06/07/2016"
	ms.author="jeedes"/>


# Tutorial: Azure Active Directory-Integration mit Jive

In diesem Tutorial erfahren Sie, wie Sie Jive in Azure Active Directory (Azure AD) integrieren.

Die Integration von Jive in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Jive haben soll.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Jive anzumelden (Sigle Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## Voraussetzungen

Um die Azure AD-Integration mit Jive konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges Jive-Abonnement


> [AZURE.NOTE] Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.


## Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Jive über den Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## Hinzufügen von Jive über den Katalog
Um die Integration von Jive in Azure AD zu konfigurieren, müssen Sie Jive über den Katalog zu Ihrer Liste mit verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um Jive über den Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

	![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

	![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen**.

	![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

	![Anwendungen][4]

6. Geben Sie im Suchfeld die Zeichenfolge **Jive** ein.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-jive-tutorial/tutorial_jive_01.png)
7. Wählen Sie im Ergebnisbereich **Jive** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-jive-tutorial/tutorial_jive_02.png)


##  Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Jive mithilfe eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Jive als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Jive muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Benutzernamen in Azure AD dem Benutzernamen in Jive zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Jive müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren von Azure AD – einmaliges Anmelden](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Jive-Testbenutzers](#creating-a-jive-test-user)**, um in Jive einen Gegenpart von Britta Simon zu erhalten, der mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Konfigurieren der Benutzerbereitstellung](#configuring-user-provisioning)**, um anzugeben, wie die Bereitstellung von Active Directory-Benutzerkonten für Jive aktiviert werden soll.
5. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
6. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### Konfigurieren des einmaligen Anmeldens von Azure AD

Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden von Azure AD im klassischen Azure-Portal zu aktivieren und in Ihrer Jive-Anwendung zu konfigurieren.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Jive die folgenden Schritte aus:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **Jive** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
	 
	![Einmaliges Anmelden konfigurieren][6]

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Jive anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-jive-tutorial/tutorial_jive_03.png)

3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-jive-tutorial/tutorial_jive_04.png)

    a. Geben Sie im Textfeld **Anmelde-URL** die URL ein, die von Ihren Benutzern zur Anmeldung bei Ihrer Jive-Anwendung verwendet wird. Verwenden Sie dabei das folgende Muster: **https://\<Kundenname>.jivecustom.com**.
	
	b. Klicken Sie auf **Weiter**.
 
4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für Jive** die folgenden Schritte aus:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-jive-tutorial/tutorial_jive_05.png)

    a. Klicken Sie auf **Zertifikat herunterladen** und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5. Melden Sie sich bei Ihrem Jive-Mandanten als Administrator an.

6. Klicken Sie im Menü am oberen Rand auf **Saml**.

	![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-jive-tutorial/tutorial_jive_002.png)

	a. Wählen auf der Registerkarte **Allgemein** die Option **Aktiviert** aus.

	b. Klicken Sie auf die Schaltfläche **Save all saml settings** (Alle Saml-Einstellungen speichern).

7. Navigieren Sie zur Registerkarte **Idp Metadata** (IDP-Metadaten).

	![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-jive-tutorial/tutorial_jive_003.png)

	a. Kopieren Sie den Inhalt der heruntergeladenen XML-Metadatendatei, und fügen Sie ihn in das Textfeld **Identity Provider (IDP) Metadata** (IDP-Metadaten) ein.

	b. Klicken Sie auf die Schaltfläche **Save all saml settings** (Alle Saml-Einstellungen speichern).

8. Navigieren Sie zur Registerkarte **User Attribute Mapping** (Benutzerattributzuordnung).

	![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-jive-tutorial/tutorial_jive_004.png)

	a. Fügen Sie im Textfeld **E-Mail** den zuvor kopierten Attributnamen des Werts **mail** ein.

	b. Fügen Sie im Textfeld **Vorname** den zuvor kopierten Attributnamen des Werts **givenname** ein.

	c. Fügen Sie im Textfeld **Nachname** den zuvor kopierten Attributnamen des Werts **surname** ein.
	
9. Wählen Sie im klassischen Portal die Bestätigung für die Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**. ![Azure AD – einmaliges Anmelden][10]

10. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Abschließen**. ![Azure AD – einmaliges Anmelden][11]


### Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im klassischen Portal einen Testbenutzer mit dem Namen Britta Simon.


![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-jive-tutorial/create_aaduser_09.png)

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie im Menü oben auf **Benutzer**, um die Liste der Benutzer anzuzeigen.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-jive-tutorial/create_aaduser_03.png)

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-jive-tutorial/create_aaduser_04.png)

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus: ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-jive-tutorial/create_aaduser_05.png)

    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.

    b. Geben Sie in das Textfeld **Benutzername** den Text **BrittaSimon** ein.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus: ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-jive-tutorial/create_aaduser_06.png)

    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.

    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-jive-tutorial/create_aaduser_07.png)

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-jive-tutorial/create_aaduser_08.png)

    a. Notieren Sie den Wert von **Neues Kennwort**.

    b. Klicken Sie auf **Fertig stellen**.



###Erstellen eines Jive-Testbenutzers

In diesem Abschnitt erstellen Sie in Jive einen Benutzer namens Britta Simon. Lassen Sie sich beim Hinzufügen der Benutzer ggf. vom Jive-Supportteam unterstützen.


###Konfigurieren der Benutzerbereitstellung
  
In diesem Abschnitt wird erläutert, wie Sie die Bereitstellung von Active Directory-Benutzerkonten für Jive aktivieren. Im Rahmen dieses Verfahrens werden Sie zur Angabe eines Benutzersicherheitstokens aufgefordert, das Sie von „Jive.com“ anfordern müssen.
  
Der folgende Screenshot zeigt ein Beispiel für das entsprechende Dialogfeld in Azure AD:

![Benutzerbereitstellung konfigurieren](./media/active-directory-saas-jive-tutorial/IC698794.png "Benutzerbereitstellung konfigurieren")

####So konfigurieren Sie die Benutzerbereitstellung

1.  Klicken Sie im Azure-Verwaltungsportal auf der Anwendungsintegrationsseite für **Jive** auf **Benutzerbereitstellung konfigurieren**, um das Dialogfeld **Benutzerbereitstellung konfigurieren** zu öffnen.

2.  Legen Sie auf der Seite **Geben Sie Ihre Jive-Anmeldeinformationen ein, um die automatische Benutzerbereitstellung zu aktivieren** die folgenden Konfigurationseinstellungen fest:

    1.  Geben Sie im Textfeld für den **Jive-Admin-Benutzernamen** den Namen eines Jive-Kontos ein, dem das Profil **Systemadministrator** in „Jive.com“ zugewiesen ist.

    2.  Geben Sie im Feld **Jive-Administratorkennwort** das Kennwort für das Konto ein.

    3.  Geben Sie im Textfeld **Jive-Mandanten-URL** die URL des Jive-Mandanten ein.

        >[AZURE.NOTE] Die Jive-Mandanten-URL ist die URL, die von Ihrer Organisation zum Anmelden bei Jive verwendet wird. In der Regel hat die URL folgendes Format: **www.<organisation>.jive.com**.

    4.  Klicken Sie auf **Überprüfen**, um die Konfiguration zu überprüfen.

    5.  Klicken Sie auf **Weiter**, um die Bestätigungsseite zu öffnen.

3.  Klicken Sie auf der Bestätigungsseite auf das Häkchen, um die Konfiguration zu speichern.
  
Sie können nun ein Testkonto erstellen und nach 10 Minuten überprüfen, ob das Konto mit „Jive.com“ synchronisiert wurde.




### Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Jive gewähren.

![Benutzer zuweisen][200]

**Führen Sie die folgenden Schritte aus, um Britta Simon Jive zuzuweisen:**

1. Klicken Sie in der Verzeichnisansicht des klassischen Portals auf der oberen Menüleiste auf **Anwendungen**, um die Anwendungsansicht zu öffnen.

	![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste die Option **Jive** aus.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-jive-tutorial/tutorial_jive_50.png)

3. Klicken Sie im oberen Menü auf **Benutzer**.

	![Benutzer zuweisen][203]

4. Wählen Sie in der Benutzerliste **Britta Simon** aus.

5. Klicken Sie auf der Symbolleiste am unteren Rand auf **Zuweisen**.

	![Benutzer zuweisen][205]


### Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Jive“ klicken, sollten Sie automatisch bei Ihrer Jive-Anwendung angemeldet werden.


## Weitere Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-jive-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jive-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jive-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jive-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-jive-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-jive-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-jive-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-jive-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jive-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jive-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-jive-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-jive-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-jive-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0720_2016-->