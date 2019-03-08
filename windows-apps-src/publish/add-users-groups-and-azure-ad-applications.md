---
Description: Sie können es Benutzern, Gruppen und Azure AD-Anwendungen mit Ihrem Partner Center-Konto hinzufügen.
title: Hinzufügen von Benutzern, Gruppen und Azure AD-Anwendungen mit Ihrem Partner Center-Konto
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, Uwp, Azure Ad-Anwendung, Aad, Benutzer, Gruppe, mehrere Benutzer, mehrere Benutzer
ms.localizationpriority: medium
ms.openlocfilehash: 326bb547ac5b0d31f5112d7d5737ddad0d592dd5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610125"
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-partner-center-account"></a>Hinzufügen von Benutzern, Gruppen und Azure AD-Anwendungen mit Ihrem Partner Center-Konto

Die **Benutzer** Abschnitt [Partner Center](https://partner.microsoft.com/dashboard) (unter **Kontoeinstellungen**) können Sie die Azure Active Directory verwenden, um Benutzer mit Ihrem Partner Center-Konto hinzuzufügen. Jedem Benutzer wird eine Rolle (oder benutzerdefinierte Berechtigungen) zugewiesen, mit der er bestimmte Zugriffsberechtigungen für das Konto erhält. Sie können auch hinzufügen [Gruppen von Benutzern](#groups) und [Azure AD-Anwendungen](#azure-ad-applications) gewähren sie Zugriff auf Ihr Partner Center-Konto.

Nachdem Benutzer auf das Konto hinzugefügt wurden, können Sie [Kontodetails bearbeiten](#edit), [Rollen und Berechtigungen](set-custom-permissions-for-account-users.md) ändern oder [Benutzer entfernen](#remove).

> [!IMPORTANT]
> Um Benutzer zu Ihrem Konto hinzufügen zu können, müssen Sie zuerst [weisen Sie Ihr Partner Center-Konto mit Azure Active Directory-Mandanten Ihrer Organisation](associate-azure-ad-with-partner-center.md). 

Wenn Sie Benutzer hinzufügen möchten, müssen Sie benötigen den Zugriff auf Ihr Partner Center-Konto angeben, indem Sie ihnen Zuweisen einer [Rolle oder eine Gruppe von benutzerdefinierten Berechtigungen](set-custom-permissions-for-account-users.md). 

Denken Sie daran, die alle Partner Center-Benutzer (einschließlich der Gruppen und Azure AD-Anwendungen) ein aktives Konto müssen in [Azure AD-Mandanten, die mit Ihrem Partner Center-Konto verknüpft ist](associate-azure-ad-with-partner-center.md). Die Benutzerverwaltung erfolgt pro Mandant. Sie müssen sich mit einem Managerkonto auf dem Mandanten anmelden, wenn Sie Benutzer hinzufügen oder bearbeiten möchten. Erstellen eines neuen Benutzers im Partner Center erstellt auch ein Konto für diesen Benutzer in Azure AD-Mandanten, dem Sie angemeldet sind und vornehmen von Änderungen an den Namen eines Benutzers im Partner Center wird die gleichen Änderungen in Ihrer Organisation Azure AD-Mandanten vornehmen.

> [!NOTE]
> Wenn Ihre Organisation verwendet [Verzeichnisintegration](https://go.microsoft.com/fwlink/p/?LinkID=724033) um den lokalen Verzeichnisdienst mit Ihrem Azure AD synchronisiert werden, nicht möglich, neue Benutzer, Gruppen oder Azure AD-Anwendungen im Partner Center zu erstellen. Sie (oder ein anderer Administrator in Ihrem lokalen Verzeichnis) müssen direkt in das lokale Verzeichnis erstellen, bevor Sie anzeigen, und fügen sie im Partner Center sein müssen.


<span id="users" />

## <a name="add-users-to-your-partner-center-account"></a>Hinzufügen von Benutzern zu Ihrem Partner Center-Konto

Um Benutzer mit Ihrem Partner Center-Konto hinzuzufügen, wechseln Sie zu der **Benutzer** auf der Seite **Kontoeinstellungen** , und wählen Sie **Hinzufügen von Benutzern.** Sie müssen mit einem Managerkonto auf dem Azure AD-Mandanten angemeldet sein, auf dem Sie arbeiten möchten. 

### <a name="add-existing-users"></a>Hinzufügen vorhandener Benutzer 

Sie können die Benutzer auswählen, die bereits in der Mandant Ihrer Organisation vorhanden sind und ihnen Zugriff auf Ihr Partner Center-Konto zu gewähren. 

<span id="from-directory" />

1.  Wählen Sie das Zahnradsymbol (in der Nähe der oberen rechten Ecke des Partner Center), und wählen Sie dann **entwicklereinstellungen**. In der **Einstellungen** , wählen Sie im Menü **Benutzer**.
2.  Wählen Sie auf der Seite **Benutzer****Benutzer hinzufügen** aus. 
3.  Wählen Sie in der angezeigten Liste einen oder mehrere Benutzer aus. Im Suchfeld können Sie nach bestimmten Benutzern suchen.
    > [!TIP]
    > Wenn Sie auf "mehr als einen Benutzer in Ihrem Partner Center-Konto hinzufügen" auswählen, müssen Sie sie der gleichen Rolle oder benutzerdefinierte Berechtigungen zuweisen. Wiederholen Sie zum Hinzufügen mehrerer Benutzer mit anderen Rollenberechtigungen die unten beschriebenen Schritte für alle Rollen oder benutzerdefinierte Berechtigungen.
4.  Wenn Sie die Benutzer ausgewählt haben, klicken Sie auf **Ausgewählte hinzufügen**.
5.  Geben Sie im Abschnitt **Rollen** an, welche [Rollen oder angepassten Berechtigungen](set-custom-permissions-for-account-users.md) Sie für die ausgewählten Benutzer wünschen.
6.  Klicken Sie auf **Speichern**.

### <a name="additional-methods-for-adding-users"></a>Zusätzliche Methoden zum Hinzufügen von Benutzern

Wenn Sie mit einem Manager-Konto angemeldet sind, die ebenfalls [globaler Administrator](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) Berechtigungen für den Azure AD-Mandanten Sie arbeiten, müssen Sie zusätzliche Optionen zum Hinzufügen von Benutzern mit Ihrem Partner Center-Konto. Sie müssen eine der folgenden Optionen auswählen:

-   **Hinzufügen von vorhandenen Benutzern**: Wählen Sie Benutzer, die bereits im Verzeichnis Ihrer Organisation vorhanden, und ermöglichen sie Ihren Zugriff auf Ihr Partner Center-Konto, mit der oben beschriebenen Methode.
-   **Erstellen Sie neue Benutzer**: Erstellen Sie neue Benutzerkonten sowohl des Verzeichnisses Ihrer Organisation hinzu und Ihrem Partner Center-Konto
-   **Externen Benutzer einladen**: Senden Sie, dass e-Mail-Adresse für Benutzer eingeladen werden, die aktuell nicht im Verzeichnis Ihrer Organisation befinden. Sie eingeladen, auf den Zugriff auf Ihr Partner Center-Konto und ein neues [Gastbenutzer](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) Konto wird für sie in Azure AD-Mandanten erstellt werden.

<span id="new-user" />

### <a name="create-new-users"></a>Erstellen neuer Benutzer

> [!IMPORTANT]
> Sie müssen mit einem globalen Administratorkonto auf Ihrem Azure AD-Mandanten angemeldet sein, um neue Benutzer zu erstellen.

1.  Von der **Benutzer** Seite (unter **Kontoeinstellungen**) Option **Hinzufügen von Benutzern**, wählen Sie dann **erstellen Sie neue Benutzer**.
2.  Geben Sie den Vornamen, den Nachnamen und den Benutzernamen für den neuen Benutzer ein.
3.  Wenn der neue Benutzer ein [Konto als globaler Administrator](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) im Verzeichnis Ihrer Organisation haben soll, markieren Sie das Kontrollkästchen **Diesen Benutzer in Azure AD zum globalen Administrator mit vollständiger Kontrolle über alle Verzeichnisressourcen machen**. Dadurch erhält der Benutzer den vollständigen Zugriff auf alle administrativen Features im Azure AD Ihrer Organisation. Sie können zum Hinzufügen und Verwalten von Benutzern im Verzeichnis Ihrer Organisation können (jedoch nicht in Partner Center, es sei denn, Sie dem Konto die entsprechenden gewähren [Rollenberechtigungen/](set-custom-permissions-for-account-users.md)). Wenn Sie dieses Kontrollkästchen markieren, müssen Sie eine **E-Mail-Adresse zur Kennwortwiederherstellung** für den Benutzer angeben.
4.  Wenn Sie das Kontrollkästchen **Diesen Benutzer in Azure AD zum globalen Administrator machen** markiert haben, geben Sie eine E-Mail-Adresse ein, die der Benutzer verwenden kann, um sein Kennwort wiederherzustellen.
5.  Wählen Sie im Abschnitt **Gruppenmitgliedschaft** alle Gruppen aus, denen der neue Benutzer angehören soll.
6.  Geben Sie im Abschnitt **Rollen** an, welche [Rollen oder angepassten Berechtigungen](set-custom-permissions-for-account-users.md) Sie für den ausgewählten Benutzer wünschen.
7.  Klicken Sie auf **Speichern**.
8.  Auf der Bestätigungsseite werden die Anmeldedaten für den neuen Benutzer angezeigt, z. B. ein temporäres Kennwort. Notieren Sie sich diese Informationen, und teilen Sie sie dem neuen Benutzer mit, da Sie nach dem Verlassen dieser Seite nicht mehr auf das temporäre Kennwort zugreifen können.


<span id="email" />

### <a name="invite-outside-users"></a>Einladen externer Benutzer

> [!IMPORTANT]
> Sie müssen auf Ihrem Azure AD-Mandanten mit einem globalen Administratorkonto angemeldet sein, um externe Benutzer einzuladen.

1.  Von der **Benutzer** Seite (unter **Kontoeinstellungen**) Option **Hinzufügen von Benutzern**, wählen Sie dann **Benutzer per e-Mail einladen**.
1.  Geben Sie eine oder mehrere E-Mails Adressen (bis zu zehn) mit Kommata oder Semikola als Trennzeichen ein.
2.  Geben Sie im Abschnitt **Rollen** an, welche [Rollen oder angepassten Berechtigungen](set-custom-permissions-for-account-users.md) Sie für den ausgewählten Benutzer wünschen.
3.  Klicken Sie auf **Speichern**.

Die von Ihnen eingeladenen Benutzer erhalten eine E-Mail-Einladung für Ihr Konto. Es wird ein neues [Gastbenutzer](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)-Konto für sie in Ihrem Azure AD-Mandanten erstellt. Jeder Benutzer muss die Einladung annehmen, bevor er auf Ihr Konto zugreifen kann.

Um die Einladung erneut zu senden, suchen Sie den Benutzer auf Ihrer **Benutzer**-Seite heraus, und wählen Sie seine E-Mail-Adresse (oder den Text **Einladung ausstehend**) aus. Klicken Sie dann am unteren Rand der Seite, auf **Einladung senden**.

> [!IMPORTANT]
> Externe Benutzer, die Sie einladen, beitreten kann Ihr Partner Center-Konto die gleichen Rollen und Berechtigungen wie die anderen Benutzer zugewiesen werden. Allerdings können externe Benutzern bestimmte Aufgaben in Visual Studio wie z. B. das Assoziieren einer App mit dem Store oder das Erstellen von Paketen zum Hochladen in den Store nicht durchführen. Wenn ein Benutzer diese Aufgaben durchführen muss, wählen Sie **Erstellen von neuen Benutzern** anstelle von **externe Benutzer einladen**. (Wenn Sie diese Benutzer nicht dem vorhandenen Azure AD-Mandanten hinzufügen möchten, können Sie [einen neuen Mandanten erstellen](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account), und anschließend für sie neue Benutzerkonten im Mandanten erstellen.) 


### <a name="changing-a-users-directory-password"></a>Ändern des Verzeichniskennworts eines Benutzers

Wenn ein Benutzer sein Kennwort ändern muss, kann er dies selber tun, wenn Sie ihm beim Erstellen des Benutzerkontos eine **E-Mail-Adresse zur Kennwortwiederherstellung** bereitgestellt haben. Sie können das Kennwort eines Benutzers auch aktualisieren, indem Sie die folgenden Schritte durchführen (wenn Sie sich mit einem globalen Administratorkonto in Ihrem Azure AD-Mandanten angemeldet haben, um das Kennwort des Benutzers zu ändern). Beachten Sie, dass dies ändert das Kennwort des Benutzers in Azure AD-Mandanten und dem Kennwort, die sie zum Zugriff auf Partner Center verwenden. 

1.  Von der **Benutzer** Seite (unter **Kontoeinstellungen**), wählen Sie den Namen des Benutzerkontos ein, die Sie bearbeiten möchten.
2.  Wählen Sie die **Zurücksetzen des Kennworts** am unteren Rand der Seite.
3.  Auf einer Bestätigungsseite werden die Anmeldeinformationen für den Benutzer angezeigt, einschließlich eines temporären Kennworts.

    > [!IMPORTANT]
    >  Achten Sie darauf, zu drucken oder kopieren diese Informationen aus, und geben Sie sie für den Benutzer, da Sie nicht auf das temporäre Kennwort zugreifen, nachdem Sie diese Seite verlassen.

<span id="groups" />

## <a name="add-groups-to-your-partner-center-account"></a>Fügen Sie Gruppen mit Ihrem Partner Center-Konto hinzu.

Sie können eine Gruppe aus Ihrem Unternehmensverzeichnis mit Ihrem Partner Center-Konto hinzufügen. Daraufhin kann jeder Benutzer, der Mitglied dieser Gruppe ist, mit den Berechtigungen der dieser Gruppe zugewiesenen Rolle darauf zugreifen.

### <a name="add-groups-from-your-organizations-directory"></a>Hinzufügen von Gruppen aus dem Verzeichnis der Organisation

1.  Wählen Sie das Zahnradsymbol (in der Nähe der oberen rechten Ecke des Partner Center), und wählen Sie dann **entwicklereinstellungen**. In der **Einstellungen** , wählen Sie im Menü **Benutzer**.
2. Von der **Benutzer** Seite **Hinzufügen von Gruppen**.
2.  Wählen Sie in der angezeigten Liste eine oder mehrere Gruppen aus. Im Suchfeld können Sie nach bestimmten Gruppen suchen.
    > [!TIP]
    > Wenn Sie auf "mehr als eine Gruppe mit Ihrem Partner Center-Konto hinzufügen" auswählen, müssen Sie sie der gleichen Rolle oder benutzerdefinierte Berechtigungen zuweisen. Wiederholen Sie zum Hinzufügen mehrerer Gruppen mit anderen Rollenberechtigungen die unten beschriebenen Schritte für alle Rollen oder benutzerdefinierte Berechtigungen.

3.  Wenn Sie die Gruppen ausgewählt haben, klicken Sie auf **Ausgewählte hinzufügen**.
4.  Geben Sie im Abschnitt **Rollen** an, welche [Rollen oder angepassten Berechtigungen](set-custom-permissions-for-account-users.md) Sie für die ausgewählten Gruppen wünschen. Alle Mitglieder der Gruppe werden auf Ihrem Partner Center-Konto mit den Berechtigungen, dass Sie der Gruppe, unabhängig davon, mit ihren persönlichen Konto zugeordneten Rollen/Berechtigungen anwenden.
5.  Klicken Sie auf **Speichern**.


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Erstellen Sie ein neues Gruppenkonto im Verzeichnis Ihrer Organisation und Ihrem Partner Center-Konto hinzufügen

Wenn Sie eine völlig neue Gruppe Partner Center-Zugriff gewähren möchten, können Sie erstellen eine neue Gruppe in der **Benutzer** Abschnitt. Beachten Sie, dass die neue Gruppe im Verzeichnis Ihrer Organisation, nicht nur in Ihrem Partner Center-Konto erstellt wird.

1.  Von der **Benutzer** Seite (unter **entwicklereinstellungen**), klicken Sie auf **Hinzufügen von Gruppen**.
2.  Wählen Sie auf der nächsten Seite **neue Gruppe**.
3.  Geben Sie den Anzeigenamen für die neue Gruppe ein.
4.  Geben Sie die [Rollen oder angepasste Berechtigungen](set-custom-permissions-for-account-users.md) für die Gruppe an. Alle Mitglieder der Gruppe werden auf Ihrem Partner Center-Konto mit den Berechtigungen, dass Sie der Gruppe, unabhängig davon, mit ihren persönlichen Konto zugeordneten Rollen/Berechtigungen anwenden.
5.  Wählen Sie den/die Benutzer, die der neuen Gruppe in der Liste zugewiesen werden sollen, aus der angezeigten Liste aus. Im Suchfeld können Sie nach bestimmten Benutzern suchen.
6.  Wenn Sie mit der Auswahl der Benutzer fertig sind, klicken Sie auf **Ausgewählte hinzufügen**, um diese der neuen Gruppe hinzuzufügen.
7.  Klicken Sie auf **Speichern**.


<span id="azure-ad-applications" />

## <a name="add-azure-ad-applications-to-your-partner-center-account"></a>Hinzufügen von Azure AD-Anwendungen mit Ihrem Partner Center-Konto

Sie können Anwendungen oder Dienste, die Teil von Ihrer Organisation Azure AD Zugriff auf Ihr Partner Center-Konto. Diese Benutzerkonten für die Azure AD-Anwendung können zum Aufrufen der über [Microsoft Store Services](../monetize/using-windows-store-services.md) bereitgestellten REST-APIs genutzt werden.


### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>Hinzufügen von Azure AD-Apps aus Verzeichnis der Organisation

1.  1.  Wählen Sie das Zahnradsymbol (in der Nähe der oberen rechten Ecke des Partner Center), und wählen Sie dann **entwicklereinstellungen**. In der **Einstellungen** , wählen Sie im Menü **Benutzer**.
2. Wählen Sie auf der Seite **Benutzer****Azure AD-Anwendungen hinzufügen** aus.
3.  Wählen Sie eine oder Azure AD-Anwendungen aus der angezeigten Liste aus. Mithilfe des Suchfelds können Sie nach bestimmten Azure AD-Apps suchen.
    > [!TIP]
    > Wenn Sie auf "mehr als eine Azure AD-Anwendung mit Ihrem Partner Center-Konto hinzufügen" auswählen, müssen Sie sie der gleichen Rolle oder benutzerdefinierte Berechtigungen zuweisen. Wiederholen Sie zum Hinzufügen mehrerer Azure AD-Anwendungen mit anderen Rollenberechtigungen die unten beschriebenen Schritte für alle Rollen oder benutzerdefinierten Berechtigungen.

4.  Wenn Sie die Azure AD-Apps ausgewählt haben, klicken Sie auf **Ausgewählte hinzufügen**.
5.  Geben Sie im Abschnitt **Rollen** an, welche [Rollen oder angepassten Berechtigungen](set-custom-permissions-for-account-users.md) Sie für die ausgewählten Azure AD-Anwendungen wünschen.
6.  Klicken Sie auf **Speichern**.


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Erstellen Sie ein neues Azure AD-Anwendung in das Verzeichnis der Organisation und fügen Sie es mit Ihrem Partner Center-Konto hinzu

Wenn Sie Zugriff auf Partner Center mit einem Konto der Marke neuen Azure AD-Anwendung gewähren möchten, können Sie erstellen, in der **Benutzer** Abschnitt. Beachten Sie, dass dies ein neues Konto, im Verzeichnis Ihrer Organisation, nicht nur in Ihrem Partner Center-Konto erstellt wird.

> [!TIP]
> Wenn Sie diese Azure AD-Anwendung in erster Linie für Partner Center-Authentifizierung verwenden und benötigen nicht die Benutzer direkt darauf zugreifen, geben Sie eine beliebige gültige Adresse für den **Antwort-URL** und **App ID-URI**, so lange als diese Werte werden nicht von anderen Azure AD-Anwendung in Ihrem Verzeichnis verwendet.

1.  Von der **Benutzer** Seite (unter **Kontoeinstellungen**) Option **Hinzufügen von Azure AD-Anwendungen**.
2.  Wählen Sie auf der nächsten Seite **New Azure AD-Anwendung**.
3.  Geben Sie die **Antwort-URL** für die neue Azure AD-App ein. Dies ist die URL, mit der sich Benutzer anmelden und Ihre Azure AD-App verwenden können (wird auch als App-URL oder Anmelde-URL bezeichnet). Die **Antwort-URL** darf nicht länger als 256 Zeichen sein und muss innerhalb des Verzeichnisses eindeutig sein.
4.  Geben Sie den **App-ID-URI** für die neue Azure AD-App ein. Dies ist ein logischer Bezeichner für die Azure AD-App, der beim Senden einer Anforderung für einmaliges Anmelden an Azure AD angezeigt wird. Beachten Sie, dass der **App-ID-URI** für jede Azure AD-App im Verzeichnis eindeutig sein muss und nicht mehr als 256 Zeichen enthalten darf. Weitere Informationen zur **App-ID-URI** finden Sie unter [Integrieren von Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#changing-the-application-registration-to-support-multi-tenant).
5.  Geben Sie im Abschnitt **Rollen** an, welche [Rollen oder angepassten Berechtigungen](set-custom-permissions-for-account-users.md) Sie für die Azure AD-Anwendungen wünschen.
6.  Klicken Sie auf **Speichern**.

Nachdem Sie eine Azure AD-Anwendung hinzugefügt oder erstellt haben, können Sie zum Abschnitt **Benutzer** zurückkehren und den Namen der Anwendung auswählen, um die Einstellungen für die Anwendung zu überprüfen, einschließlich Mandanten-ID, Client-ID, Antwort-URL und App-ID-URI.

> [!NOTE]
> Wenn Sie beabsichtigen, die REST-APIs zu verwenden, die von den [Microsoft Store-Diensten](../monetize/using-windows-store-services.md) bereitgestellt werden, benötigen Sie die auf dieser Seite angezeigten Werte für die Mandanten-ID und die Client-ID, um ein Azure AD-Zugriffstoken abzurufen, das Sie für die Authentifizierung der Aufrufe von Diensten verwenden können.   

<span id="manage-keys" />

### <a name="manage-keys-for-an-azure-ad-application"></a>Verwalten von Schlüsseln für eine Azure AD-App

Wenn die Azure AD-Anwendung Daten in Microsoft Azure AD liest und schreibt, benötigt sie einen Schlüssel. Sie können Schlüssel für eine Azure AD-Anwendung erstellen, bearbeiten Sie die Informationen in Partner Center. Sie können auch Schlüssel entfernen, die nicht mehr benötigt werden.

1.  Von der **Benutzer** Seite (unter **Kontoeinstellungen**), wählen Sie den Namen der Azure AD-Anwendung.
    > [!TIP]
    > Wenn Sie den Namen des Azure AD-Anwendung klicken, sehen Sie alle aktiven Schlüssel für die Azure AD-Anwendung, einschließlich des Datums auf der der Schlüssel erstellt wurde und wenn es abläuft. Klicken Sie auf **Entfernen**, um einen nicht mehr benötigten Schlüssel zu entfernen.

2.  Wählen Sie zum Hinzufügen eines neuen Schlüssels **Hinzufügen neuer Schlüssel**.
3.  Es wird ein Bildschirm mit den Werten für **Client-ID** und **Schlüssel** angezeigt.
    > [!IMPORTANT]
    > Achten Sie darauf, drucken oder kopieren Sie diese Informationen, da Sie nicht darauf zugreifen, nachdem Sie diese Seite verlassen können.

4.  Wenn Sie mehrere Schlüssel erstellen möchten, wählen Sie **fügen Sie einen anderen Schlüssel**.

<span id="edit" />

## <a name="edit-a-user-group-or-azure-ad-application"></a>Bearbeiten von Benutzern, Gruppen oder Azure AD-Anwendungen

Nachdem Sie Benutzer, die Gruppen und/oder Azure AD-Anwendungen mit Ihrem Partner Center-Konto hinzugefügt haben, können Sie ihre Kontoinformationen ändern. 

> [!IMPORTANT]
> Änderungen an [Rollen oder Berechtigungen](set-custom-permissions-for-account-users.md) wirkt sich nur auf die Zugriff auf Partner Center. Alle anderen Änderungen (z. B. ändern den Namen eines Benutzers oder der Gruppenmitgliedschaft ist oder die Antwort-URL und die App-ID-URI für eine Azure AD-Anwendung) werden in Ihrer Organisation Azure AD-Mandanten so gut wie für das Partner Center-Konto berücksichtigt. 

1.  Von der **Benutzer** Seite (unter **Kontoeinstellungen**), wählen Sie den Namen des Benutzers, Gruppe oder der Anwendung Azure AD-Konto, das Sie bearbeiten möchten.
2.  Nehmen Sie die gewünschten Änderungen vor. Die Elemente, die Sie bearbeiten können, lauten wie folgt:
    -   Sie können den Vornamen, den Nachnamen oder den Benutzernamen eines **Benutzers** bearbeiten. Sie können ebenfalls Gruppen im Abschnitt **Gruppenmitgliedschaft** auswählen oder deaktivieren, um die Gruppenmitgliedschaft zu aktualisieren.
    -   Für eine **Gruppe** können Sie den Namen der Gruppe bearbeiten. (Um Gruppenmitgliedschaft zu aktualisieren, bearbeiten Sie die Benutzer, die Sie der Gruppe hinzufügen oder daraus entfernen möchten und nehmen Sie im Abschnitt **Gruppenmitgliedschaft** Änderungen vor.)
    -   Für eine **Azure AD-Anwendung** können Sie neue Werte für die **Antwort-URL** oder **App-ID-URI** eingeben.
    Denken Sie daran, dass diese Änderungen im Verzeichnis Ihrer Organisation Sie auch, wie für das Partner Center-Konto gestellt werden.
3.  Damit die Änderungen im Zusammenhang mit Zugriff auf Partner Center, aktivieren oder deaktivieren Sie die Rollen, die Sie verwenden möchten, wenden, oder wählen **Berechtigungen anpassen** und nehmen die gewünschten Änderungen vor. Diese Änderungen wirken sich nur auf Partner Center Zugriff auf und ändert sich nicht auf alle Berechtigungen in Ihrer Organisation Azure AD-Mandanten.
3.  Klicken Sie auf **Speichern**.


## <a name="view-history-for-account-users"></a>Verlauf für Kontobenutzer anzeigen

Als Kontobesitzer können Sie den detaillierten Browserverlauf für alle weiteren Benutzer, die Sie dem Konto hinzugefügt haben, anzeigen.

Auf der **Benutzer** Seite (unter **Kontoeinstellungen**), wählen Sie auf den Link unter **letzte Aktivität** für den Benutzer, dessen Browserverlauf, die Sie überprüfen möchten. Sie können die URLs aller Seiten anzeigen, die der Benutzer in den letzten 30 Tagen besucht habt.

<span id="remove" />

## <a name="remove-users-groups-and-azure-ad-applications"></a>Entfernen von Benutzern, Gruppen und Azure AD-Anwendungen

Wählen Sie zum Entfernen einer Benutzer-, Gruppen-, oder Azure AD-Anwendung aus Ihrem Partner Center-Konto die **entfernen** anhand ihres Namens auf angezeigten Link aus, die **Benutzer** Seite. Nach der Bestätigung, dass Sie sie entfernen möchten, werden diese Benutzer, Gruppe oder Azure AD-Anwendung nicht mehr mit Ihrem Partner Center-Konto zugreifen (es sei denn, Sie es später erneut hinzufügen).

> [!IMPORTANT]
> Entfernen einer Benutzer-, Gruppen-, oder Azure AD-Anwendung bedeutet, dass es nicht mehr mit Ihrem Partner Center-Konto zugreifen kann. Dadurch werden **nicht** die betreffenden Benutzer, Gruppen oder Azure AD-Anwendungen aus dem Verzeichnis der Organisation gelöscht.

 

