<properties 
    pageTitle="Tutorial: Azure Active Directory-Integration mit ZScaler Two | Microsoft Azure" 
    description="Hier erfahren Sie, wie Sie ZScaler Two mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen." 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="stevenpo"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="05/25/2016" 
    ms.author="jeedes" />

#Tutorial: Azure Active Directory-Integration mit Zscaler Two

In diesem Tutorial wird die Integration von Azure und Zscaler Two erläutert. Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

-   Ein gültiges Azure-Abonnement
-   Ein Zscaler Two-Abonnement, für das einmaliges Anmelden aktiviert ist
  
Nach Abschluss dieses Tutorials können sich die Zscaler Two zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung auf Ihrer Zscaler Two-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können Sie die [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md) nutzen.
  
Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1.  Aktivieren der Anwendungsintegration für Zscaler Two
2.  Konfigurieren der einmaligen Anmeldung
3.  Konfigurieren von Proxyeinstellungen
4.  Konfigurieren der Benutzerbereitstellung
5.  Zuweisen von Benutzern

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zscaler-two-tutorial/IC800199.png "Einmaliges Anmelden konfigurieren")

##Aktivieren der Anwendungsintegration für Zscaler Two
  
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Zscaler Two aktivieren.

###So aktivieren Sie die Anwendungsintegration für Zscaler Two:

1.  Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory](./media/active-directory-saas-zscaler-two-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

    ![Anwendungen](./media/active-directory-saas-zscaler-two-tutorial/IC700994.png "Anwendungen")

4.  Klicken Sie unten auf der Seite auf **Hinzufügen**.

    ![Anwendung hinzufügen](./media/active-directory-saas-zscaler-two-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

    ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-zscaler-two-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")

6.  Geben Sie im **Suchfeld** das Wort **Zscaler Two** ein.

    ![Anwendungskatalog](./media/active-directory-saas-zscaler-two-tutorial/IC800200.png "Anwendungskatalog")

7.  Wählen Sie im Ergebnisbereich **Zscaler Two** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

    ![Zscaler Two](./media/active-directory-saas-zscaler-two-tutorial/IC800201.png "Zscaler Two")

##Konfigurieren der einmaligen Anmeldung
  
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Zscaler Two zu authentifizieren. Im Rahmen dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikatsdatei in Ihren Zscaler Two-Mandanten hochladen. Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o) (in englischer Sprache) weitere Informationen.

###So konfigurieren Sie einmaliges Anmelden

1.  Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **ZScaler Two** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zscaler-two-tutorial/IC800202.png "Einmaliges Anmelden konfigurieren")

2.  Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Zscaler Two anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zscaler-two-tutorial/IC800203.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Zscaler Two-Anmelde-URL** die URL ein, die die Benutzer zur Anmeldung bei Zscaler Two verwenden, und klicken Sie dann auf **Weiter**.

    ![App-URL konfigurieren](./media/active-directory-saas-zscaler-two-tutorial/IC800204.png "App-URL konfigurieren")

    >[AZURE.NOTE] Sie erhalten bei Bedarf den tatsächlichen Wert für Ihre Umgebung von Ihrem Zscaler Two-Supportteam.

4.  Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für Zscaler Two** auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zscaler-two-tutorial/IC800205.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie sich in einem anderen Webbrowserfenster bei der Zscaler Two-Unternehmenswebsite als Administrator an.

6.  Klicken Sie oben im Menü auf **Verwaltung**.

    ![Verwaltung](./media/active-directory-saas-zscaler-two-tutorial/IC800206.png "Verwaltung")

7.  Klicken Sie unter **Administratoren & Rollen verwalten** auf **Benutzer & Authentifizierung verwalten**.

    ![Benutzer & Authentifizierung verwalten](./media/active-directory-saas-zscaler-two-tutorial/IC800207.png "Benutzer & Authentifizierung verwalten")

8.  Führen Sie im Abschnitt **Auswählen von Authentifizierungsoptionen für Ihre Organisation** die folgenden Schritte aus:

    ![Authentifizierung](./media/active-directory-saas-zscaler-two-tutorial/IC800208.png "Authentifizierung")

    1.  Wählen Sie **Authentifizeren mit der einmaligen Anmeldung für SAML**.
    2.  Klicken Sie auf **Einzelne Parameter der einmaligen Anmeldung für SAML konfigurieren**.

9.  Führen Sie auf der Dialogfeldseite **Einzelne Parameter der einmaligen Anmeldung für SAML konfigurieren** die folgenden Schritte aus, und klicken Sie dann auf **Fertig stellen**.

    ![Einmaliges Anmelden](./media/active-directory-saas-zscaler-two-tutorial/IC800209.png "Einmaliges Anmelden")

    1.  Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für ZScaler Two** den Wert der **Authentifizierungsanforderungs-URL**, und fügen Sie ihn in das Textfeld **URL des SAML-Portals, an das Benutzer zur Authentifizierung weitergeleitet werden** ein.
    2.  Geben Sie im Textfeld **Attribut mit Anmeldenamen** die **NameID** ein.
    3.  Klicken Sie auf **Zscaler pem**, um das heruntergeladene Zertifikat hochzuladen.
    4.  Wählen Sie **Automatische SAML-Bereitstellung aktivieren**.

10. Führen Sie auf der Dialogseite **Benutzerauthentifizierung konfigurieren** die folgenden Schritte aus:

    ![Verwaltung](./media/active-directory-saas-zscaler-two-tutorial/IC800210.png "Verwaltung")

    1.  Klicken Sie auf **Speichern**.
    2.  Klicken Sie auf **Jetzt aktivieren**.

11. Wählen Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für ZScaler Two** die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Abschließen**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zscaler-two-tutorial/IC800211.png "Einmaliges Anmelden konfigurieren")

##Konfigurieren von Proxyeinstellungen

###So konfigurieren Sie die Proxyeinstellungen in Internet Explorer:

1.  Starten Sie **Internet Explorer**.

2.  Wählen Sie im Menü **Tools** **Internetoptionen**, um das Dialogfeld **Internetoptionen** zu öffnen.

    !["Internetoptionen",](./media/active-directory-saas-zscaler-two-tutorial/IC769492.png ""Internetoptionen",")

3.  Klicken Sie auf die Registerkarte **Verbindungen**.

    ![Verbindungen](./media/active-directory-saas-zscaler-two-tutorial/IC769493.png "Verbindungen")

4.  Klicken Sie zum Öffnen des Dialogfelds **LAN-Einstellungen** auf **LAN-Einstellungen**.

5.  Führen Sie im Abschnitt "Proxyserver" die folgenden Schritte aus:

    ![Proxyserver](./media/active-directory-saas-zscaler-two-tutorial/IC769494.png "Proxyserver")

    1.  Wählen Sie "Proxyserver für LAN verwenden" aus.
    2.  Geben Sie in das Textfeld „Adresse“ **gateway.zscalerone.net** ein.
    3.  Geben Sie im Textfeld „Port“ **80** ein.
    4.  Wählen Sie **Proxyserver für lokale Adressen umgehen**.
    5.  Klicken Sie zum Schließen des Dialogfelds **Local Area Network (LAN) Settings** auf **OK**.

6.  Klicken Sie zum Schließen des Dialogfelds **Internetoptionen** auf **OK**.

##Konfigurieren der Benutzerbereitstellung
  
Damit sich Azure AD-Benutzer bei ZScaler Two anmelden können, müssen sie in ZScaler Two bereitgestellt werden. Im Fall von ZScaler Two ist die Bereitstellung eine manuelle Aufgabe.

###So konfigurieren Sie die Benutzerbereitstellung

1.  Melden Sie sich bei Ihrem **Zscaler**-Mandanten an.

2.  Klicken Sie auf **Verwaltung**.

    ![Verwaltung](./media/active-directory-saas-zscaler-two-tutorial/IC781035.png "Verwaltung")

3.  Klicken Sie auf **Benutzerverwaltung**.

    ![Hinzufügen](./media/active-directory-saas-zscaler-two-tutorial/IC781037.png "Hinzufügen")

4.  Klicken Sie in der Registerkarte **Benutzer** auf **Hinzufügen**.

    ![Hinzufügen](./media/active-directory-saas-zscaler-two-tutorial/IC781037.png "Hinzufügen")

5.  Führen Sie im Abschnitt "Benutzer hinzufügen" die folgenden Schritte aus:

    ![Benutzer hinzufügen](./media/active-directory-saas-zscaler-two-tutorial/IC781038.png "Benutzer hinzufügen")

    1.  Geben Sie die **Benutzer-ID**, den **Benutzeranzeigenamen**, das **Kennwort** und **Kennwort bestätigen** ein, und wählen Sie dann **Gruppen** und die **Abteilung** eines gültigen AAD-Kontos, das Sie bereitstellen möchten.
    2.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von ZScaler Two-Benutzerkonten oder mithilfe der von ZScaler Two bereitgestellten APIs erstellen.

##Zuweisen von Benutzern
  
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

###So weisen Sie ZScaler Two Benutzer zu:

1.  Erstellen Sie im klassischen Azure-Portal ein Testkonto.

2.  Klicken Sie auf der Anwendungsintegrationsseite für **ZScaler Two** auf **Benutzer zuweisen**.

    ![Benutzer zuweisen](./media/active-directory-saas-zscaler-two-tutorial/IC800212.png "Benutzer zuweisen")

3.  Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.

    ![Ja](./media/active-directory-saas-zscaler-two-tutorial/IC767830.png "Ja")
  
Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

<!---HONumber=AcomDC_0525_2016-->