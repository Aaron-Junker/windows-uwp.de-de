---
Description: Sie können Ihrem Partner Center-Kontobenutzer, Gruppen und Azure AD Anwendungen hinzufügen.
title: Hinzufügen von Benutzern, Gruppen und Azure AD Anwendungen zu Ihrem Partner Center-Konto
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Azure AD-Anwendung, AAD, Benutzer, Gruppe, mehrere Benutzer, mehrere Benutzer
ms.localizationpriority: medium
ms.openlocfilehash: 41467f51e02f3cc700e3759f33d6fd6eea3ac7a6
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210706"
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-partner-center-account"></a>Hinzufügen von Benutzern, Gruppen und Azure AD Anwendungen zu Ihrem Partner Center-Konto

Mit dem Abschnitt " **Benutzer** " von [Partner Center](https://partner.microsoft.com/dashboard) (unter " **Kontoeinstellungen**") können Sie Azure Active Directory zum Hinzufügen von Benutzern zu Ihrem Partner Center-Konto verwenden. Jedem Benutzer wird eine Rolle (oder benutzerdefinierte Berechtigungen) zugewiesen, mit der er bestimmte Zugriffsberechtigungen für das Konto erhält. Sie können auch [Gruppen von Benutzern](#groups) und [Azure AD Anwendungen](#azure-ad-applications) hinzufügen, um Ihnen Zugriff auf Ihr Partner Center-Konto zu gewähren.

Nachdem Benutzer auf das Konto hinzugefügt wurden, können Sie [Kontodetails bearbeiten](#edit), [Rollen und Berechtigungen](set-custom-permissions-for-account-users.md) ändern oder [Benutzer entfernen](#remove).

> [!IMPORTANT]
> zum Hinzufügen von Benutzern zu Ihrem Konto müssen Sie [Ihr Partner Center-Konto zunächst dem Azure Active Directory Mandanten Ihrer Organisation zuordnen](associate-azure-ad-with-partner-center.md). 

Wenn Sie Benutzer hinzufügen, müssen Sie Ihren Zugriff auf Ihr Partner Center-Konto angeben, indem Sie Ihnen eine [Rolle oder einen Satz benutzerdefinierter Berechtigungen](set-custom-permissions-for-account-users.md)zuweisen. 

Beachten Sie, dass alle Partner Center-Benutzer (einschließlich Gruppen und Azure AD Anwendungen) über ein aktives Konto in einem Azure AD-Mandanten verfügen müssen [, der Ihrem Partner Center-Konto zugeordnet ist](associate-azure-ad-with-partner-center.md). Die Benutzerverwaltung erfolgt pro Mandant. Sie müssen sich mit einem Managerkonto auf dem Mandanten anmelden, wenn Sie Benutzer hinzufügen oder bearbeiten möchten. Beim Erstellen eines neuen Benutzers in Partner Center wird auch ein Konto für diesen Benutzer im Azure AD Mandanten erstellt, mit dem Sie sich angemeldet haben. Änderungen an einem Benutzernamen im Partner Center nehmen die gleichen Änderungen im Azure AD Mandanten Ihrer Organisation vor.

> [!NOTE]
> Wenn Ihre Organisation die [Verzeichnisintegration](https://docs.microsoft.com/previous-versions/azure/azure-services/jj573653(v=azure.100)?redirectedfrom=MSDN) verwendet, um den lokalen Verzeichnisdienst mit Ihrem Azure AD zu synchronisieren, können Sie keine neuen Benutzer, Gruppen oder Azure AD Anwendungen in Partner Center erstellen. Sie (oder ein anderer Administrator in Ihrem lokalen Verzeichnis) müssen diese direkt im lokalen Verzeichnis erstellen, bevor Sie Sie in Partner Center anzeigen und hinzufügen können.


<span id="users" />

## <a name="add-users-to-your-partner-center-account"></a>Hinzufügen von Benutzern zu Ihrem Partner Center-Konto

Um Ihrem Partner Center-Kontobenutzer hinzuzufügen, wechseln Sie in den **Kontoeinstellungen** zur Seite **Benutzer** , und wählen Sie **Benutzer hinzufügen aus.** Sie müssen mit einem Managerkonto auf dem Azure AD-Mandanten angemeldet sein, auf dem Sie arbeiten möchten. 

### <a name="add-existing-users"></a>Hinzufügen vorhandener Benutzer 

Sie können Benutzer auswählen, die bereits im Mandanten Ihrer Organisation vorhanden sind, und Ihnen Zugriff auf Ihr Partner Center-Konto erteilt haben. 

<span id="from-directory" />

1.  Wählen Sie das Zahnrad Symbol (in der Nähe der oberen rechten Ecke von Partner Center) aus, und wählen Sie dann **Entwicklereinstellungen**aus. Wählen Sie im Menü " **Einstellungen** " die Option **Benutzer**aus.
2.  Wählen Sie auf der Seite **Benutzer** **Benutzer hinzufügen** aus. 
3.  Wählen Sie in der angezeigten Liste einen oder mehrere Benutzer aus. Im Suchfeld können Sie nach bestimmten Benutzern suchen.
    > [!TIP]
    > Wenn Sie mehr als einen Benutzer auswählen, den Sie Ihrem Partner Center-Konto hinzufügen möchten, müssen Sie Ihnen dieselbe Rolle oder einen Satz benutzerdefinierter Berechtigungen zuweisen. Wiederholen Sie zum Hinzufügen mehrerer Benutzer mit anderen Rollenberechtigungen die unten beschriebenen Schritte für alle Rollen oder benutzerdefinierte Berechtigungen.
4.  Wenn Sie die Benutzer ausgewählt haben, klicken Sie auf **Ausgewählte hinzufügen**.
5.  Geben Sie im Abschnitt **Rollen** an, welche [Rollen oder angepassten Berechtigungen](set-custom-permissions-for-account-users.md) Sie für die ausgewählten Benutzer wünschen.
6.  Klicken Sie auf **Speichern**.

### <a name="additional-methods-for-adding-users"></a>Zusätzliche Methoden zum Hinzufügen von Benutzern

Wenn Sie mit einem Manager-Konto angemeldet sind, das auch über [globale Administrator](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) Berechtigungen für den Azure AD Mandanten verfügt, in dem Sie arbeiten, stehen Ihnen zusätzliche Optionen zum Hinzufügen von Benutzern zu Ihrem Partner Center-Konto zur Verfügung. Sie müssen eine der folgenden Optionen auswählen:

-   **Vorhandene Benutzer hinzufügen**: Wählen Sie Benutzer aus, die bereits im Verzeichnis Ihrer Organisation vorhanden sind, und stellen Sie Ihnen mithilfe der oben beschriebenen Methode Zugriff auf Ihr Partner Center-Konto zur Verfügung.
-   **Neue Benutzer erstellen**: Erstellen Sie neue Benutzerkonten, die dem Verzeichnis Ihrer Organisation und Ihrem Partner Center-Konto hinzugefügt werden sollen.
-   **Invite outside users** (Externe Benutzer einladen): Laden Sie Benutzer per E-Mail ein, die derzeit nicht im Verzeichnis der Organisation vorhanden sind. Sie werden für den Zugriff auf Ihr Partner Center-Konto eingeladen, und in Ihrem Azure AD Mandanten wird ein neues [Gastbenutzer](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) Konto erstellt.

<span id="new-user" />

### <a name="create-new-users"></a>Erstellen neuer Benutzer

> [!IMPORTANT]
> Sie müssen mit einem globalen Administratorkonto auf Ihrem Azure AD-Mandanten angemeldet sein, um neue Benutzer zu erstellen.

1.  Wählen Sie auf der Seite **Benutzer** (unter **Kontoeinstellungen**) die Option **Benutzer hinzufügen**aus, und wählen Sie dann **neue Benutzer erstellen**aus.
2.  Geben Sie den Vornamen, den Nachnamen und den Benutzernamen für den neuen Benutzer ein.
3.  Wenn der neue Benutzer ein [Konto als globaler Administrator](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) im Verzeichnis Ihrer Organisation haben soll, markieren Sie das Kontrollkästchen **Diesen Benutzer in Azure AD zum globalen Administrator mit vollständiger Kontrolle über alle Verzeichnisressourcen machen**. Dadurch erhält der Benutzer den vollständigen Zugriff auf alle administrativen Features im Azure AD Ihrer Organisation. Sie können Benutzer im Verzeichnis Ihrer Organisation hinzufügen und verwalten (aber nicht im Partner Center, es sei denn, Sie gewähren dem Konto die entsprechenden [Rollen/Berechtigungen](set-custom-permissions-for-account-users.md)). Wenn Sie dieses Kontrollkästchen markieren, müssen Sie eine **E-Mail-Adresse zur Kennwortwiederherstellung** für den Benutzer angeben.
4.  Wenn Sie das Kontrollkästchen **Diesen Benutzer in Azure AD zum globalen Administrator machen** markiert haben, geben Sie eine E-Mail-Adresse ein, die der Benutzer verwenden kann, um sein Kennwort wiederherzustellen.
5.  Wählen Sie im Abschnitt **Gruppenmitgliedschaft** alle Gruppen aus, denen der neue Benutzer angehören soll.
6.  Geben Sie im Abschnitt **Rollen** an, welche [Rollen oder angepassten Berechtigungen](set-custom-permissions-for-account-users.md) Sie für den ausgewählten Benutzer wünschen.
7.  Klicken Sie auf **Speichern**.
8.  Auf der Bestätigungsseite werden die Anmeldedaten für den neuen Benutzer angezeigt, z. B. ein temporäres Kennwort. Notieren Sie sich diese Informationen, und teilen Sie sie dem neuen Benutzer mit, da Sie nach dem Verlassen dieser Seite nicht mehr auf das temporäre Kennwort zugreifen können.


<span id="email" />

### <a name="invite-outside-users"></a>Einladen externer Benutzer

> [!IMPORTANT]
> Sie müssen auf Ihrem Azure AD-Mandanten mit einem globalen Administratorkonto angemeldet sein, um externe Benutzer einzuladen.

1.  Wählen Sie auf der Seite **Benutzer** (unter **Kontoeinstellungen**) die Option **Benutzer hinzufügen**aus, und wählen Sie dann **Benutzer per e-Mail einladen**aus.
1.  Geben Sie eine oder mehrere E-Mails Adressen (bis zu zehn) mit Kommata oder Semikola als Trennzeichen ein.
2.  Geben Sie im Abschnitt **Rollen** an, welche [Rollen oder angepassten Berechtigungen](set-custom-permissions-for-account-users.md) Sie für den ausgewählten Benutzer wünschen.
3.  Klicken Sie auf **Speichern**.

Die von Ihnen eingeladenen Benutzer erhalten eine E-Mail-Einladung für Ihr Konto. Es wird ein neues [Gastbenutzer](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)-Konto für sie in Ihrem Azure AD-Mandanten erstellt. Jeder Benutzer muss die Einladung annehmen, bevor er auf Ihr Konto zugreifen kann.

Um die Einladung erneut zu senden, suchen Sie den Benutzer auf Ihrer **Benutzer**-Seite heraus, und wählen Sie seine E-Mail-Adresse (oder den Text **Einladung ausstehend**) aus. Klicken Sie dann am unteren Rand der Seite, auf **Einladung senden**.

> [!IMPORTANT]
> Externe Benutzer, die Sie einladen, Ihrem Partner Center-Konto beizutreten, können dieselben Rollen und Berechtigungen wie andere Benutzer zuweisen. Allerdings können externe Benutzern bestimmte Aufgaben in Visual Studio wie z. B. das Assoziieren einer App mit dem Store oder das Erstellen von Paketen zum Hochladen in den Store nicht durchführen. Wenn ein Benutzer diese Aufgaben durchführen muss, wählen Sie **Erstellen von neuen Benutzern** anstelle von **externe Benutzer einladen**. (Wenn Sie diese Benutzer nicht dem vorhandenen Azure AD-Mandanten hinzufügen möchten, können Sie [einen neuen Mandanten erstellen](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account), und anschließend für sie neue Benutzerkonten im Mandanten erstellen.) 


### <a name="changing-a-users-directory-password"></a>Ändern des Verzeichniskennworts eines Benutzers

Wenn ein Benutzer sein Kennwort ändern muss, kann er dies selber tun, wenn Sie ihm beim Erstellen des Benutzerkontos eine **E-Mail-Adresse zur Kennwortwiederherstellung** bereitgestellt haben. Sie können das Kennwort eines Benutzers auch aktualisieren, indem Sie die folgenden Schritte durchführen (wenn Sie sich mit einem globalen Administratorkonto in Ihrem Azure AD-Mandanten angemeldet haben, um das Kennwort des Benutzers zu ändern). Beachten Sie, dass dadurch das Kennwort des Benutzers in Ihrem Azure AD Mandanten und das Kennwort, das Sie für den Zugriff auf Partner Center verwenden, geändert werden. 

1.  Wählen Sie auf der Seite **Benutzer** (unter **Kontoeinstellungen**) den Namen des Benutzerkontos aus, das Sie bearbeiten möchten.
2.  Wählen Sie unten auf der Seite die Schaltfläche **Kennwort zurücksetzen** aus.
3.  Auf einer Bestätigungsseite werden die Anmeldeinformationen für den Benutzer angezeigt, einschließlich eines temporären Kennworts.

    > [!IMPORTANT]
    >  achten Sie darauf, diese Informationen zu drucken oder zu kopieren und für den Benutzer bereitzustellen, da Sie auf das temporäre Kennwort nicht zugreifen können, nachdem Sie diese Seite verlassen haben.

<span id="groups" />

## <a name="add-groups-to-your-partner-center-account"></a>Hinzufügen von Gruppen zu Ihrem Partner Center-Konto

Sie können eine Gruppe aus dem Verzeichnis Ihrer Organisation Ihrem Partner Center-Konto hinzufügen. Daraufhin kann jeder Benutzer, der Mitglied dieser Gruppe ist, mit den Berechtigungen der dieser Gruppe zugewiesenen Rolle darauf zugreifen.

### <a name="add-groups-from-your-organizations-directory"></a>Hinzufügen von Gruppen aus dem Verzeichnis der Organisation

1.  Wählen Sie das Zahnrad Symbol (in der Nähe der oberen rechten Ecke von Partner Center) aus, und wählen Sie dann **Entwicklereinstellungen**aus. Wählen Sie im Menü " **Einstellungen** " die Option **Benutzer**aus.
2. Wählen Sie auf der Seite **Benutzer** die Option **Gruppen hinzufügen**aus.
2.  Wählen Sie in der angezeigten Liste eine oder mehrere Gruppen aus. Im Suchfeld können Sie nach bestimmten Gruppen suchen.
    > [!TIP]
    > Wenn Sie mehr als eine Gruppe auswählen, die Sie Ihrem Partner Center-Konto hinzufügen möchten, müssen Sie Ihnen dieselbe Rolle oder einen Satz benutzerdefinierter Berechtigungen zuweisen. Wiederholen Sie zum Hinzufügen mehrerer Gruppen mit anderen Rollenberechtigungen die unten beschriebenen Schritte für alle Rollen oder benutzerdefinierte Berechtigungen.

3.  Wenn Sie die Gruppen ausgewählt haben, klicken Sie auf **Ausgewählte hinzufügen**.
4.  Geben Sie im Abschnitt **Rollen** an, welche [Rollen oder angepassten Berechtigungen](set-custom-permissions-for-account-users.md) Sie für die ausgewählten Gruppen wünschen. Alle Mitglieder der Gruppe können auf Ihr Partner Center-Konto mit den Berechtigungen, die Sie für die Gruppe anwenden, zugreifen, unabhängig von den Rollen/Berechtigungen, die dem jeweiligen Konto zugeordnet sind.
5.  Klicken Sie auf **Speichern**.


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Erstellen Sie ein neues Gruppenkonto im Verzeichnis Ihrer Organisation, und fügen Sie es Ihrem Partner Center-Konto hinzu.

Wenn Sie Partner Center Zugriff auf eine neue Gruppe gewähren möchten, können Sie im Abschnitt " **Benutzer** " eine neue Gruppe erstellen. Beachten Sie, dass die neue Gruppe im Verzeichnis Ihrer Organisation, nicht nur in Ihrem Partner Center-Konto erstellt wird.

1.  Klicken Sie auf der Seite " **Benutzer** " (unter " **Entwicklereinstellungen**") auf **Gruppen hinzufügen**.
2.  Wählen Sie auf der nächsten Seite die Option **neue Gruppe**aus.
3.  Geben Sie den Anzeigenamen für die neue Gruppe ein.
4.  Geben Sie die [Rollen oder angepasste Berechtigungen](set-custom-permissions-for-account-users.md) für die Gruppe an. Alle Mitglieder der Gruppe können auf Ihr Partner Center-Konto mit den Berechtigungen, die Sie für die Gruppe anwenden, zugreifen, unabhängig von den Rollen/Berechtigungen, die dem jeweiligen Konto zugeordnet sind.
5.  Wählen Sie den/die Benutzer, die der neuen Gruppe in der Liste zugewiesen werden sollen, aus der angezeigten Liste aus. Im Suchfeld können Sie nach bestimmten Benutzern suchen.
6.  Wenn Sie mit der Auswahl der Benutzer fertig sind, klicken Sie auf **Ausgewählte hinzufügen**, um diese der neuen Gruppe hinzuzufügen.
7.  Klicken Sie auf **Speichern**.


<span id="azure-ad-applications" />

## <a name="add-azure-ad-applications-to-your-partner-center-account"></a>Hinzufügen Azure AD Anwendungen zu Ihrem Partner Center-Konto

Sie können zulassen, dass Anwendungen oder Dienste, die Teil des Azure AD Ihrer Organisation sind, auf Ihr Partner Center-Konto zugreifen können. Diese Benutzerkonten für die Azure AD-Anwendung können zum Aufrufen der über [Microsoft Store Services](../monetize/using-windows-store-services.md) bereitgestellten REST-APIs genutzt werden.


### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>Hinzufügen von Azure AD-Apps aus Verzeichnis der Organisation

1.  1.  Wählen Sie das Zahnrad Symbol (in der Nähe der oberen rechten Ecke von Partner Center) aus, und wählen Sie dann **Entwicklereinstellungen**aus. Wählen Sie im Menü " **Einstellungen** " die Option **Benutzer**aus.
2. Wählen Sie auf der Seite **Benutzer** **Azure AD-Anwendungen hinzufügen** aus.
3.  Wählen Sie eine oder Azure AD-Anwendungen aus der angezeigten Liste aus. Mithilfe des Suchfelds können Sie nach bestimmten Azure AD-Apps suchen.
    > [!TIP]
    > Wenn Sie mehr als eine Azure AD Anwendung auswählen, die Sie Ihrem Partner Center-Konto hinzufügen möchten, müssen Sie Ihnen dieselbe Rolle oder einen Satz benutzerdefinierter Berechtigungen zuweisen. Wiederholen Sie zum Hinzufügen mehrerer Azure AD-Anwendungen mit anderen Rollenberechtigungen die unten beschriebenen Schritte für alle Rollen oder benutzerdefinierten Berechtigungen.

4.  Wenn Sie die Azure AD-Apps ausgewählt haben, klicken Sie auf **Ausgewählte hinzufügen**.
5.  Geben Sie im Abschnitt **Rollen** an, welche [Rollen oder angepassten Berechtigungen](set-custom-permissions-for-account-users.md) Sie für die ausgewählten Azure AD-Anwendungen wünschen.
6.  Klicken Sie auf **Speichern**.


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Erstellen Sie ein neues Azure AD-Anwendungs Konto im Verzeichnis Ihrer Organisation, und fügen Sie es Ihrem Partner Center-Konto hinzu.

Wenn Sie Partner Center Zugriff auf eine neue Azure AD Anwendungs Konto gewähren möchten, können Sie im Abschnitt " **Benutzer** " einen solchen erstellen. Beachten Sie, dass dadurch ein neues Konto im Verzeichnis Ihrer Organisation erstellt wird, nicht nur in Ihrem Partner Center-Konto.

> [!TIP]
> Wenn Sie diese Azure AD Anwendung für die Partner Center-Authentifizierung verwenden und Benutzer nicht direkt darauf zugreifen müssen, können Sie eine gültige Adresse für die **Antwort-URL** und den APP- **ID-URI**eingeben, sofern diese Werte nicht von anderen Azure AD Anwendungen in Ihrem Verzeichnis verwendet werden.

1.  Wählen Sie auf der Seite **Benutzer** (unter **Kontoeinstellungen**) die Option **Azure AD Anwendungen hinzufügen**aus.
2.  Wählen Sie auf der nächsten Seite die Option **neu Azure AD Anwendung**aus.
3.  Geben Sie die **Antwort-URL** für die neue Azure AD-App ein. Dies ist die URL, mit der sich Benutzer anmelden und Ihre Azure AD-App verwenden können (wird auch als App-URL oder Anmelde-URL bezeichnet). Die **Antwort-URL** darf nicht länger als 256 Zeichen sein und muss innerhalb des Verzeichnisses eindeutig sein.
4.  Geben Sie den **App-ID-URI** für die neue Azure AD-App ein. Dies ist ein logischer Bezeichner für die Azure AD-App, der beim Senden einer Anforderung für einmaliges Anmelden an Azure AD angezeigt wird. Beachten Sie, dass der **App-ID-URI** für jede Azure AD-App im Verzeichnis eindeutig sein muss und nicht mehr als 256 Zeichen enthalten darf. Weitere Informationen zur **App-ID-URI** finden Sie unter [Integrieren von Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#changing-the-application-registration-to-support-multi-tenant).
5.  Geben Sie im Abschnitt **Rollen** an, welche [Rollen oder angepassten Berechtigungen](set-custom-permissions-for-account-users.md) Sie für die Azure AD-Anwendungen wünschen.
6.  Klicken Sie auf **Speichern**.

Nachdem Sie eine Azure AD-Anwendung hinzugefügt oder erstellt haben, können Sie zum Abschnitt **Benutzer** zurückkehren und den Namen der Anwendung auswählen, um die Einstellungen für die Anwendung zu überprüfen, einschließlich Mandanten-ID, Client-ID, Antwort-URL und App-ID-URI.

> [!NOTE]
> Wenn Sie beabsichtigen, die REST-APIs zu verwenden, die von den [Microsoft Store-Diensten](../monetize/using-windows-store-services.md) bereitgestellt werden, benötigen Sie die auf dieser Seite angezeigten Werte für die Mandanten-ID und die Client-ID, um ein Azure AD-Zugriffstoken abzurufen, das Sie für die Authentifizierung der Aufrufe von Diensten verwenden können.   

<span id="manage-keys" />

### <a name="manage-keys-for-an-azure-ad-application"></a>Verwalten von Schlüsseln für eine Azure AD-App

Wenn die Azure AD-Anwendung Daten in Microsoft Azure AD liest und schreibt, benötigt sie einen Schlüssel. Sie können Schlüssel für eine Azure AD Anwendung erstellen, indem Sie die zugehörigen Informationen im Partner Center bearbeiten. Sie können auch Schlüssel entfernen, die nicht mehr benötigt werden.

1.  Wählen Sie auf der Seite **Benutzer** (unter **Kontoeinstellungen**) den Namen der Azure AD Anwendung aus.
    > [!TIP]
    > Wenn Sie auf den Namen der Azure AD Anwendung klicken, werden alle aktiven Schlüssel für die Azure AD Anwendung angezeigt, einschließlich des Datums, an dem der Schlüssel erstellt wurde, und der Ablauf Zeitpunkt. Klicken Sie auf **Entfernen**, um einen nicht mehr benötigten Schlüssel zu entfernen.

2.  Wählen Sie zum Hinzufügen eines neuen Schlüssels die Option **neuen Schlüssel hinzufügen**aus.
3.  Es wird ein Bildschirm mit den Werten für **Client-ID** und **Schlüssel** angezeigt.
    > [!IMPORTANT]
    > achten Sie darauf, diese Informationen zu drucken oder zu kopieren, da Sie nicht mehr darauf zugreifen können, nachdem Sie diese Seite verlassen haben.

4.  Wenn Sie weitere Schlüssel erstellen möchten, wählen Sie **einen weiteren Schlüssel hinzufügen**aus.

<span id="edit" />

## <a name="edit-a-user-group-or-azure-ad-application"></a>Bearbeiten von Benutzern, Gruppen oder Azure AD-Anwendungen

Nachdem Sie Ihrem Partner Center-Kontobenutzer, Gruppen und/oder Azure AD Anwendungen hinzugefügt haben, können Sie Änderungen an den Kontoinformationen vornehmen. 

> [!IMPORTANT]
> An [Rollen oder Berechtigungen](set-custom-permissions-for-account-users.md) vorgenommene Änderungen wirken sich nur auf den Zugriff auf Partner Center aus. Alle anderen Änderungen (z. b. das Ändern des Namens oder der Gruppenmitgliedschaft eines Benutzers oder der Antwort-URL und der APP-ID-URI für eine Azure AD Anwendung) werden in Ihrem Unternehmen Azure AD Mandanten sowie in Ihrem Partner Center-Konto widergespiegelt. 

1.  Wählen Sie auf der Seite **Benutzer** (unter **Kontoeinstellungen**) den Namen des Benutzers, der Gruppe oder Azure AD Anwendungs Kontos aus, das Sie bearbeiten möchten.
2.  Nehmen Sie die gewünschten Änderungen vor. Die Elemente, die Sie bearbeiten können, lauten wie folgt:
    -   Sie können den Vornamen, den Nachnamen oder den Benutzernamen eines **Benutzers** bearbeiten. Sie können ebenfalls Gruppen im Abschnitt **Gruppenmitgliedschaft** auswählen oder deaktivieren, um die Gruppenmitgliedschaft zu aktualisieren.
    -   Für eine **Gruppe** können Sie den Namen der Gruppe bearbeiten. (Um Gruppenmitgliedschaft zu aktualisieren, bearbeiten Sie die Benutzer, die Sie der Gruppe hinzufügen oder daraus entfernen möchten und nehmen Sie im Abschnitt **Gruppenmitgliedschaft** Änderungen vor.)
    -   Für eine **Azure AD-Anwendung** können Sie neue Werte für die **Antwort-URL** oder **App-ID-URI** eingeben.
    Beachten Sie, dass diese Änderungen sowohl im Verzeichnis Ihrer Organisation als auch in Ihrem Partner Center-Konto vorgenommen werden.
3.  Für Änderungen im Zusammenhang mit dem Zugriff auf Partner Center aktivieren bzw. deaktivieren Sie die Rollen, die Sie anwenden möchten, oder wählen Sie die Option **Berechtigungen anpassen** aus, und nehmen Sie die gewünschten Änderungen vor. Diese Änderungen wirken sich nur auf den Zugriff auf Partner Center aus und ändern keine Berechtigungen innerhalb des Azure AD Mandanten Ihrer Organisation.
3.  Klicken Sie auf **Speichern**.


## <a name="view-history-for-account-users"></a>Verlauf für Kontobenutzer anzeigen

Als Kontobesitzer können Sie den detaillierten Browserverlauf für alle weiteren Benutzer, die Sie dem Konto hinzugefügt haben, anzeigen.

Wählen Sie auf der Seite **Benutzer** (unter **Kontoeinstellungen**) den Link aus, der unter **Letzte Aktivität** für den Benutzer angezeigt wird, dessen Browserverlauf Sie überprüfen möchten. Sie können die URLs aller Seiten anzeigen, die der Benutzer in den letzten 30 Tagen besucht habt.

<span id="remove" />

## <a name="remove-users-groups-and-azure-ad-applications"></a>Entfernen von Benutzern, Gruppen und Azure AD-Anwendungen

Wenn Sie eine Benutzer-, Gruppen-oder Azure AD Anwendung aus Ihrem Partner Center-Konto entfernen möchten, klicken Sie auf der Seite **Benutzer** auf den Link **Entfernen** , der mit dem Namen angezeigt wird. Nachdem Sie bestätigt haben, dass Sie Sie entfernen möchten, kann der Benutzer, die Gruppe oder Azure AD Anwendung nicht mehr auf Ihr Partner Center-Konto zugreifen (es sei denn, Sie fügen es später erneut hinzu).

> [!IMPORTANT]
> Das Entfernen einer Benutzer-, Gruppen-oder Azure AD Anwendung bedeutet, dass Sie keinen Zugriff mehr auf Ihr Partner Center-Konto hat. Dadurch werden **nicht** die betreffenden Benutzer, Gruppen oder Azure AD-Anwendungen aus dem Verzeichnis der Organisation gelöscht.

 

