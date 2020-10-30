---
description: Sie können Ihrem Partner Center-Kontobenutzer, Gruppen und Azure AD Anwendungen hinzufügen.
title: Hinzufügen von Benutzern, Gruppen und Azure AD-Anwendungen zu Ihrem Partner Center-Konto
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Azure AD-Anwendung, AAD, Benutzer, Gruppe, mehrere Benutzer, mehrere Benutzer
ms.localizationpriority: medium
ms.openlocfilehash: 59f3a69399b33164688da3c80c781828802662f5
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032883"
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-partner-center-account"></a>Hinzufügen von Benutzern, Gruppen und Azure AD-Anwendungen zu Ihrem Partner Center-Konto

Mit dem Abschnitt " **Benutzer** " von [Partner Center](https://partner.microsoft.com/dashboard) (unter " **Kontoeinstellungen** ") können Sie Azure Active Directory zum Hinzufügen von Benutzern zu Ihrem Partner Center-Konto verwenden. Jedem Benutzer wird eine Rolle (oder ein Satz benutzerdefinierter Berechtigungen) zugewiesen, die den Zugriff auf das Konto definiert. Sie können auch [Gruppen von Benutzern](#groups) und [Azure AD Anwendungen](#azure-ad-applications) hinzufügen, um Ihnen Zugriff auf Ihr Partner Center-Konto zu gewähren.

Nachdem Benutzer zum Konto hinzugefügt wurden, können Sie [Konto Details bearbeiten](#edit), [Rollen und Berechtigungen](set-custom-permissions-for-account-users.md)ändern oder [Benutzer entfernen](#remove).

> [!IMPORTANT]
> Zum Hinzufügen von Benutzern zu Ihrem Konto müssen Sie [Ihr Partner Center-Konto zunächst dem Azure Active Directory Mandanten Ihrer Organisation zuordnen](associate-azure-ad-with-partner-center.md). 

Wenn Sie Benutzer hinzufügen, müssen Sie Ihren Zugriff auf Ihr Partner Center-Konto angeben, indem Sie Ihnen eine [Rolle oder einen Satz benutzerdefinierter Berechtigungen](set-custom-permissions-for-account-users.md)zuweisen. 

Beachten Sie, dass alle Partner Center-Benutzer (einschließlich Gruppen und Azure AD Anwendungen) über ein aktives Konto in einem Azure AD-Mandanten verfügen müssen [, der Ihrem Partner Center-Konto zugeordnet ist](associate-azure-ad-with-partner-center.md). Die Benutzerverwaltung erfolgt in jeweils einem Mandanten. Sie müssen sich mit einem Manager-Konto für den Mandanten anmelden, in dem Sie Benutzer hinzufügen oder bearbeiten möchten. Beim Erstellen eines neuen Benutzers in Partner Center wird auch ein Konto für diesen Benutzer im Azure AD Mandanten erstellt, mit dem Sie sich angemeldet haben. Änderungen an einem Benutzernamen im Partner Center nehmen die gleichen Änderungen im Azure AD Mandanten Ihrer Organisation vor.

> [!NOTE]
> Wenn Ihre Organisation [Verzeichnisintegration](/previous-versions/azure/azure-services/jj573653(v=azure.100)) verwendet, um den lokalen Verzeichnisdienst mit Ihrem Azure AD zu synchronisieren, können Sie keine neuen Benutzer, Gruppen oder Azure AD-Anwendungen in Partner Center erstellen. Diese müssen Sie (oder ein anderer Administrator in Ihrem lokalen Verzeichnis) direkt im lokalen Verzeichnis erstellen, bevor Sie sie im Partner Center anzeigen und hinzufügen können.


<span id="users" />

## <a name="add-users-to-your-partner-center-account"></a>Hinzufügen von Benutzern zu Ihrem Partner Center-Konto

Um Ihrem Partner Center-Kontobenutzer hinzuzufügen, wechseln Sie in den **Kontoeinstellungen** zur Seite **Benutzer** , und wählen Sie **Benutzer hinzufügen aus.** Sie müssen mit einem Manager-Konto für den Azure AD Mandanten angemeldet sein, in dem Sie arbeiten möchten. 

### <a name="add-existing-users"></a>Hinzufügen vorhandener Benutzer 

Sie können Benutzer auswählen, die bereits im Mandanten Ihrer Organisation vorhanden sind, und Ihnen Zugriff auf Ihr Partner Center-Konto erteilt haben. 

<span id="from-directory" />

1.  Wählen Sie das Zahnrad Symbol (in der Nähe der oberen rechten Ecke von Partner Center) aus, und wählen Sie dann **Entwicklereinstellungen** aus. Wählen Sie im Menü " **Einstellungen** " die Option **Benutzer** aus.
2.  Wählen Sie auf der Seite **Benutzer** die Option **Benutzer hinzufügen** aus. 
3.  Wählen Sie einen oder mehrere Benutzer aus der angezeigten Liste aus. Über das Suchfeld können Sie bestimmte Benutzer suchen.
    > [!TIP]
    > Wenn Sie mehr als einen Benutzer auswählen, den Sie Ihrem Partner Center-Konto hinzufügen möchten, müssen Sie Ihnen dieselbe Rolle oder einen Satz benutzerdefinierter Berechtigungen zuweisen. Wenn Sie mehrere Benutzer mit unterschiedlichen Rollen/Berechtigungen hinzufügen möchten, wiederholen Sie die Schritte unten für jede Rolle oder jeden Satz benutzerdefinierter Berechtigungen.
4.  Wenn Sie die Benutzer ausgewählt haben, klicken Sie auf **Ausgewählte hinzufügen** .
5.  Geben Sie im Abschnitt **Rollen** die [Rollen oder benutzerdefinierten Berechtigungen](set-custom-permissions-for-account-users.md) für die ausgewählten Benutzer an.
6.  Klicken Sie auf **Speichern** .

### <a name="additional-methods-for-adding-users"></a>Zusätzliche Methoden zum Hinzufügen von Benutzern

Wenn Sie mit einem Manager-Konto angemeldet sind, das auch über [globale Administrator](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) Berechtigungen für den Azure AD Mandanten verfügt, in dem Sie arbeiten, stehen Ihnen zusätzliche Optionen zum Hinzufügen von Benutzern zu Ihrem Partner Center-Konto zur Verfügung. Sie müssen eine der folgenden Aktionen auswählen:

-   **Vorhandene Benutzer hinzufügen** : Wählen Sie Benutzer aus, die bereits im Verzeichnis Ihrer Organisation vorhanden sind, und stellen Sie Ihnen mithilfe der oben beschriebenen Methode Zugriff auf Ihr Partner Center-Konto zur Verfügung.
-   **Neue Benutzer erstellen** : Erstellen Sie neue Benutzerkonten, die dem Verzeichnis Ihrer Organisation und Ihrem Partner Center-Konto hinzugefügt werden sollen.
-   **Einladen außerhalb von Benutzern** : Senden von e-Mail-Einladungen an Benutzer, die sich derzeit nicht im Verzeichnis Ihrer Organisation befinden. Sie werden für den Zugriff auf Ihr Partner Center-Konto eingeladen, und in Ihrem Azure AD Mandanten wird ein neues [Gastbenutzer](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) Konto erstellt.

<span id="new-user" />

### <a name="create-new-users"></a>Erstellen neuer Benutzer

> [!IMPORTANT]
> Sie müssen in Ihrem Azure AD-Mandanten mit einem globalen Administrator Konto angemeldet sein, um neue Benutzer erstellen zu können.

1.  Wählen Sie auf der Seite **Benutzer** (unter **Kontoeinstellungen** ) die Option **Benutzer hinzufügen** aus, und wählen Sie dann **neue Benutzer erstellen** aus.
2.  Geben Sie den Vornamen, den Nachnamen und den Benutzernamen für den neuen Benutzer ein.
3.  Wenn Sie möchten, dass der neue Benutzer über ein [globales Administrator Konto](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) im Verzeichnis Ihrer Organisation verfügt, aktivieren Sie das Kontrollkästchen **diesen Benutzer in Ihrem Azure AD als globaler Administrator festlegen, mit vollständiger Kontrolle über alle Verzeichnis Ressourcen** . Dadurch erhält der Benutzer vollständigen Zugriff auf alle Administratorfunktionen in Ihrem Unternehmens-Azure AD. Sie können Benutzer im Verzeichnis Ihrer Organisation hinzufügen und verwalten (aber nicht im Partner Center, es sei denn, Sie gewähren dem Konto die entsprechenden [Rollen/Berechtigungen](set-custom-permissions-for-account-users.md)). Wenn Sie dieses Kontrollkästchen markieren, müssen Sie eine **E-Mail-Adresse zur Kennwortwiederherstellung** für den Benutzer angeben.
4.  Wenn Sie das Kontrollkästchen aktiviert haben, um **diesen Benutzer zu einem globalen Administrator in Ihrem Azure AD zu machen** , geben Sie eine e-Mail ein, die der Benutzer verwenden kann, wenn er sein Kennwort wiederherstellen muss.
5.  Wählen Sie im Abschnitt **Gruppenmitgliedschaft** alle Gruppen aus, zu denen der neue Benutzer gehören soll.
6.  Geben Sie im Abschnitt **Rollen** die [Rollen oder benutzerdefinierten Berechtigungen](set-custom-permissions-for-account-users.md) für den Benutzer an.
7.  Klicken Sie auf **Speichern** .
8.  Auf der Bestätigungsseite werden die Anmeldedaten für den neuen Benutzer angezeigt, z. B. ein temporäres Kennwort. Notieren Sie sich diese Informationen, und teilen Sie sie dem neuen Benutzer mit, da Sie nach dem Verlassen dieser Seite nicht mehr auf das temporäre Kennwort zugreifen können.


<span id="email" />

### <a name="invite-outside-users"></a>Einladen außerhalb von Benutzern

> [!IMPORTANT]
> Sie müssen in Ihrem Azure AD Mandanten mit einem globalen Administrator Konto angemeldet sein, um außerhalb von Benutzern eingeladen zu werden.

1.  Wählen Sie auf der Seite **Benutzer** (unter **Kontoeinstellungen** ) die Option **Benutzer hinzufügen** aus, und wählen Sie dann **Benutzer per e-Mail einladen** aus.
1.  Geben Sie eine oder mehrere E-Mail-Adressen (bis zu 10) getrennt durch Kommas oder Semikolons ein.
2.  Geben Sie im Abschnitt **Rollen** die [Rollen oder benutzerdefinierten Berechtigungen](set-custom-permissions-for-account-users.md) für den Benutzer an.
3.  Klicken Sie auf **Speichern** .

Die von Ihnen geladenen Benutzer erhalten eine e-Mail-Einladung zum beitreten zum Konto, und in Ihrem Azure AD Mandanten wird ein neues [Gastbenutzer](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) Konto erstellt. Jeder Benutzer muss zunächst die Einladung akzeptieren, bevor er auf Ihr Konto zugreifen kann.

Wenn Sie eine Einladung erneut senden müssen, suchen Sie den Benutzer auf der Seite " **Benutzer** ", und wählen Sie die zugehörige e-Mail-Adresse (oder den Text, der die **Einladung steht aus** ) Klicken Sie dann am unteren Rand der Seite, auf **Einladung senden** .

> [!IMPORTANT]
> Externe Benutzer, die Sie einladen, Ihrem Partner Center-Konto beizutreten, können dieselben Rollen und Berechtigungen wie andere Benutzer zuweisen. Externe Benutzer können jedoch bestimmte Aufgaben in Visual Studio nicht durchführen, z. b. das Zuordnen einer App zum Store oder das Erstellen von Paketen zum Hochladen in den Speicher. Wenn ein Benutzer diese Aufgaben ausführen muss, wählen Sie **neue Benutzer erstellen** anstelle von **außerhalb der Benutzer einladen** aus. (Wenn Sie diese Benutzer nicht zu Ihrem vorhandenen Azure AD Mandanten hinzufügen möchten, können Sie [einen neuen Mandanten erstellen](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account)und dann neue Benutzerkonten für diese Benutzer in diesem Mandanten erstellen.) 


### <a name="changing-a-users-directory-password"></a>Ändern des Verzeichniskennworts eines Benutzers

Wenn einer Ihrer Benutzer Ihr Kennwort ändern muss, können Sie dies selbst tun, wenn Sie beim Erstellen des Benutzerkontos eine e-Mail zur Kenn **Wort Wiederherstellung** angegeben haben. Sie können das Kennwort eines Benutzers auch aktualisieren, indem Sie die folgenden Schritte ausführen (wenn Sie in Ihrem Azure AD Mandanten mit einem globalen Administrator Konto angemeldet sind, um das Kennwort eines Benutzers zu ändern). Beachten Sie, dass hierdurch das Kennwort des Benutzers in Ihrem Azure AD-Mandanten sowie das Kennwort geändert werden, das er zum Zugriff auf Partner Center verwendet. 

1.  Wählen Sie auf der Seite **Benutzer** (unter **Kontoeinstellungen** ) den Namen des Benutzerkontos aus, das Sie bearbeiten möchten.
2.  Wählen Sie unten auf der Seite die Schaltfläche **Kennwort zurücksetzen** aus.
3.  Auf einer Bestätigungsseite werden die Anmeldeinformationen für den Benutzer angezeigt, einschließlich eines temporären Kennworts.

    > [!IMPORTANT]
    >  Da Sie nach Verlassen dieser Seite nicht mehr auf das temporäre Kennwort zugreifen können, sollten Sie diese Informationen unbedingt drucken oder kopieren und dem Benutzer zur Verfügung stellen.

<span id="groups" />

## <a name="add-groups-to-your-partner-center-account"></a>Hinzufügen von Gruppen zu Ihrem Partner Center-Konto

Sie können eine Gruppe aus dem Verzeichnis Ihrer Organisation Ihrem Partner Center-Konto hinzufügen. Wenn Sie dies tun, kann jeder Benutzer, der Mitglied dieser Gruppe ist, darauf zugreifen, mit den Berechtigungen, die der zugewiesenen Rolle der Gruppe zugeordnet sind.

### <a name="add-groups-from-your-organizations-directory"></a>Hinzufügen von Gruppen aus dem Verzeichnis der Organisation

1.  Wählen Sie das Zahnrad Symbol (in der Nähe der oberen rechten Ecke von Partner Center) aus, und wählen Sie dann **Entwicklereinstellungen** aus. Wählen Sie im Menü " **Einstellungen** " die Option **Benutzer** aus.
2. Wählen Sie auf der Seite **Benutzer** die Option **Gruppen hinzufügen** aus.
2.  Wählen Sie eine oder mehrere Gruppen aus der angezeigten Liste aus. Über das Suchfeld können Sie bestimmte Gruppen suchen.
    > [!TIP]
    > Wenn Sie mehrere Gruppen auswählen, die Ihrem Partner Center-Konto hinzugefügt werden sollen, müssen Sie diesen dieselbe Rolle oder denselben Satz benutzerdefinierter Berechtigungen zuweisen. Wenn Sie mehrere Gruppen mit unterschiedlichen Rollen/Berechtigungen hinzufügen möchten, wiederholen Sie die nachfolgenden Schritte für jede Rolle oder jeden Satz benutzerdefinierter Berechtigungen.

3.  Wenn Sie die Gruppen ausgewählt haben, klicken Sie auf **Ausgewählte hinzufügen** .
4.  Geben Sie im Abschnitt **Rollen** die [Rollen oder benutzerdefinierten Berechtigungen](set-custom-permissions-for-account-users.md) für die ausgewählten Gruppen an. Alle Mitglieder der Gruppe können auf Ihr Partner Center-Konto mit den Berechtigungen, die Sie für die Gruppe anwenden, zugreifen, unabhängig von den Rollen/Berechtigungen, die dem jeweiligen Konto zugeordnet sind.
5.  Klicken Sie auf **Speichern** .


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Erstellen Sie ein neues Gruppenkonto im Verzeichnis Ihrer Organisation, und fügen Sie es Ihrem Partner Center-Konto hinzu.

Wenn Sie Partner Center Zugriff auf eine neue Gruppe gewähren möchten, können Sie im Abschnitt " **Benutzer** " eine neue Gruppe erstellen. Beachten Sie, dass die neue Gruppe im Verzeichnis Ihrer Organisation, nicht nur in Ihrem Partner Center-Konto erstellt wird.

1.  Klicken Sie auf der Seite " **Benutzer** " (unter " **Entwicklereinstellungen** ") auf **Gruppen hinzufügen** .
2.  Wählen Sie auf der nächsten Seite **Neue Gruppe** aus.
3.  Geben Sie den Anzeigenamen für die neue Gruppe ein.
4.  Geben Sie die [Rollen oder benutzerdefinierten Berechtigungen](set-custom-permissions-for-account-users.md) für die Gruppe an. Alle Mitglieder der Gruppe können auf Ihr Partner Center-Konto mit den Berechtigungen, die Sie für die Gruppe anwenden, zugreifen, unabhängig von den Rollen/Berechtigungen, die dem jeweiligen Konto zugeordnet sind.
5.  Wählen Sie in der angezeigten Liste die Benutzer aus, die der neuen Gruppe zugewiesen werden sollen. Über das Suchfeld können Sie bestimmte Benutzer suchen.
6.  Wenn Sie alle Benutzer ausgewählt haben, klicken Sie auf **Ausgewählte hinzufügen** , um sie der neuen Gruppe hinzuzufügen.
7.  Klicken Sie auf **Speichern** .


<span id="azure-ad-applications" />

## <a name="add-azure-ad-applications-to-your-partner-center-account"></a>Hinzufügen Azure AD Anwendungen zu Ihrem Partner Center-Konto

Sie können zulassen, dass Anwendungen oder Dienste, die Teil des Azure AD Ihrer Organisation sind, auf Ihr Partner Center-Konto zugreifen können. Diese Azure AD Anwendungs Benutzerkonten können verwendet werden, um die Rest-APIs aufzurufen, die von den [Microsoft Store Diensten](../monetize/using-windows-store-services.md)bereitgestellt werden.


### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>Hinzufügen von Azure AD-Apps aus Verzeichnis der Organisation

1.  1.  Wählen Sie das Zahnrad Symbol (in der Nähe der oberen rechten Ecke von Partner Center) aus, und wählen Sie dann **Entwicklereinstellungen** aus. Wählen Sie im Menü " **Einstellungen** " die Option **Benutzer** aus.
2. Wählen Sie **Users** auf der Seite Benutzer **Azure AD Anwendungen hinzufügen** aus.
3.  Wählen Sie eine oder mehrere Azure AD-Anwendungen aus der angezeigten Liste aus. Über das Suchfeld können Sie bestimmte Azure AD-Anwendungen suchen.
    > [!TIP]
    > Wenn Sie mehrere Azure AD-Anwendungen auswählen, die Ihrem Partner Center-Konto hinzugefügt werden sollen, müssen Sie diesen dieselbe Rolle oder denselben Satz benutzerdefinierter Berechtigungen zuweisen. Wenn Sie mehrere Azure AD Anwendungen mit unterschiedlichen Rollen/Berechtigungen hinzufügen möchten, wiederholen Sie die Schritte unten für jede Rolle oder jeden Satz benutzerdefinierter Berechtigungen.

4.  Wenn Sie alle Azure AD-Anwendungen ausgewählt haben, klicken Sie auf **Ausgewählte hinzufügen** .
5.  Geben Sie im Abschnitt **Rollen** die [Rollen oder benutzerdefinierten Berechtigungen](set-custom-permissions-for-account-users.md) für die ausgewählten Azure AD Anwendung (en) an.
6.  Klicken Sie auf **Speichern** .


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Erstellen Sie ein neues Azure AD-Anwendungs Konto im Verzeichnis Ihrer Organisation, und fügen Sie es Ihrem Partner Center-Konto hinzu.

Wenn Sie Partner Center Zugriff auf eine neue Azure AD Anwendungs Konto gewähren möchten, können Sie im Abschnitt " **Benutzer** " einen solchen erstellen. Beachten Sie, dass dadurch ein neues Konto im Verzeichnis Ihrer Organisation erstellt wird, nicht nur in Ihrem Partner Center-Konto.

> [!TIP]
> Wenn Sie diese Azure AD-Anwendung in erster Linie für die Partner Center-Authentifizierung verwenden und die Benutzer keinen direkten Zugriff benötigen, können Sie alle gültigen Adressen für die **Antwort-URL** und den **App-ID-URI** eingeben, sofern diese Werte noch nicht von einer anderen Azure AD-Anwendung in Ihrem Verzeichnis verwendet werden.

1.  Wählen Sie auf der Seite **Benutzer** (unter **Kontoeinstellungen** ) die Option **Azure AD-Anwendungen hinzufügen** aus.
2.  Wählen Sie auf der nächsten Seite **Neue Azure AD-Anwendung** aus.
3.  Geben Sie die **Antwort-URL** für die neue Azure AD-Anwendung ein. Dies ist die URL, über die sich die Benutzer anmelden und Ihre Azure AD-Anwendung verwenden können (auch bekannt als App-URL oder Anmelde-URL). Die **Antwort-URL** darf nicht länger als 256 Zeichen sein und muss in Ihrem Verzeichnis eindeutig sein.
4.  Geben Sie den **App-ID-URI** für die neue Azure AD-Anwendung ein. Dies ist ein logischer Bezeichner für die Azure AD-Anwendung, der beim Senden einer Anforderung für einmaliges Anmelden an Azure AD angezeigt wird. Beachten Sie, dass der **App-ID-URI** für jede Azure AD-App im Verzeichnis eindeutig sein muss und nicht mehr als 256 Zeichen enthalten darf. Weitere Informationen zum APP- **ID-URI** finden Sie unter [integrieren von Anwendungen in Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications#changing-the-application-registration-to-support-multi-tenant).
5.  Geben Sie im Abschnitt **Rollen** die [Rollen oder benutzerdefinierten Berechtigungen](set-custom-permissions-for-account-users.md) für die Azure AD Anwendung an.
6.  Klicken Sie auf **Speichern** .

Nachdem Sie eine Azure AD-Anwendung hinzugefügt oder erstellt haben, können Sie zum Abschnitt **Benutzer** zurückkehren und den Anwendungsnamen auswählen, um die Einstellungen für die Anwendung zu überprüfen, zum Beispiel die Mandanten-ID, Client-ID, Antwort-URL und App-ID-URI.

> [!NOTE]
> Wenn Sie beabsichtigen, die Rest-APIs zu verwenden, die von den [Microsoft Store-Diensten](../monetize/using-windows-store-services.md)bereitgestellt werden, benötigen Sie die auf dieser Seite angezeigten Werte für Mandanten-ID und Client-ID, um ein Azure AD Zugriffs Token zu erhalten, mit dem Sie die Aufrufe an-Dienste authentifizieren können.   

<span id="manage-keys" />

### <a name="manage-keys-for-an-azure-ad-application"></a>Verwalten von Schlüsseln für eine Azure AD-Anwendung

Wenn Ihre Azure AD-Anwendung Daten in Microsoft Azure AD liest und schreibt, benötigt sie einen Schlüssel. Sie können Schlüssel für eine Azure AD Anwendung erstellen, indem Sie die zugehörigen Informationen im Partner Center bearbeiten. Außerdem können Sie nicht mehr benötigte Schlüssel entfernen.

1.  Wählen Sie auf der Seite **Benutzer** (unter **Kontoeinstellungen** ) den Namen der Azure AD-Anwendung aus.
    > [!TIP]
    > Wenn Sie auf den Namen der Azure AD Anwendung klicken, werden alle aktiven Schlüssel für die Azure AD Anwendung angezeigt, einschließlich des Datums, an dem der Schlüssel erstellt wurde, und der Ablauf Zeitpunkt. Klicken Sie auf **Entfernen** , um einen nicht mehr benötigten Schlüssel zu entfernen.

2.  Wählen Sie zum Hinzufügen eines neuen Schlüssels **Neuen Schlüssel hinzufügen** aus.
3.  Sie sehen einen Bildschirm, der die **Client-ID** und die **Schlüssel** Werte anzeigt.
    > [!IMPORTANT]
    > Achten Sie darauf, diese Informationen zu drucken oder zu kopieren, da Sie nicht mehr darauf zugreifen können, nachdem Sie diese Seite verlassen haben.

4.  Wenn Sie weitere Schlüssel erstellen möchten, wählen Sie **Weiteren Schlüssel hinzufügen** aus.

<span id="edit" />

## <a name="edit-a-user-group-or-azure-ad-application"></a>Bearbeiten einer Benutzer-, Gruppen-oder Azure AD Anwendung

Nachdem Sie Ihrem Partner Center-Kontobenutzer, Gruppen und/oder Azure AD Anwendungen hinzugefügt haben, können Sie Änderungen an den Kontoinformationen vornehmen. 

> [!IMPORTANT]
> An [Rollen oder Berechtigungen](set-custom-permissions-for-account-users.md) vorgenommene Änderungen wirken sich nur auf den Zugriff auf Partner Center aus. Alle anderen Änderungen (z. b. das Ändern des Namens oder der Gruppenmitgliedschaft eines Benutzers oder der Antwort-URL und der APP-ID-URI für eine Azure AD Anwendung) werden in Ihrem Unternehmen Azure AD Mandanten sowie in Ihrem Partner Center-Konto widergespiegelt. 

1.  Wählen Sie auf der Seite **Benutzer** (unter **Kontoeinstellungen** ) den Namen des Benutzers, der Gruppe oder Azure AD Anwendungs Kontos aus, das Sie bearbeiten möchten.
2.  Nehmen Sie die gewünschten Änderungen vor. Die Elemente, die Sie bearbeiten können, lauten wie folgt:
    -   Für einen **Benutzer** können Sie den Vornamen, den Nachnamen oder den Benutzernamen des Benutzers bearbeiten. Sie können im Abschnitt **Gruppenmitgliedschaft** auch Gruppen auswählen oder deaktivieren, um deren Gruppenmitgliedschaft zu aktualisieren.
    -   Bei einer **Gruppe** können Sie den Namen der Gruppe bearbeiten. (Um die Gruppenmitgliedschaft zu aktualisieren, bearbeiten Sie die Benutzer, die Sie der Gruppe hinzufügen oder entfernen möchten, und nehmen Sie Änderungen am Abschnitt **Gruppenmitgliedschaft** vor.)
    -   Für eine **Azure AD-Anwendung** können Sie neue Werte für die **Antwort-URL** oder den APP- **ID-URI** eingeben.
    Beachten Sie, dass diese Änderungen sowohl im Verzeichnis Ihrer Organisation als auch in Ihrem Partner Center-Konto vorgenommen werden.
3.  Für Änderungen im Zusammenhang mit dem Zugriff auf Partner Center aktivieren bzw. deaktivieren Sie die Rollen, die Sie anwenden möchten, oder wählen Sie die Option **Berechtigungen anpassen** aus, und nehmen Sie die gewünschten Änderungen vor. Diese Änderungen wirken sich nur auf den Zugriff auf Partner Center aus und ändern keine Berechtigungen innerhalb des Azure AD Mandanten Ihrer Organisation.
3.  Klicken Sie auf **Speichern** .


## <a name="view-history-for-account-users"></a>Verlauf für Kontobenutzer anzeigen

Als Konto Besitzer können Sie den detaillierten Browserverlauf für alle weiteren Benutzer anzeigen, die Sie dem Konto hinzugefügt haben.

Wählen Sie auf der Seite **Benutzer** (unter **Kontoeinstellungen** ) den Link aus, der unter **Letzte Aktivität** für den Benutzer angezeigt wird, dessen Browserverlauf Sie überprüfen möchten. Sie können die URLs für alle Seiten anzeigen, die der Benutzer in den letzten 30 Tagen besucht hat.

<span id="remove" />

## <a name="remove-users-groups-and-azure-ad-applications"></a>Entfernen von Benutzern, Gruppen und Azure AD Anwendungen

Wenn Sie eine Benutzer-, Gruppen-oder Azure AD Anwendung aus Ihrem Partner Center-Konto entfernen möchten, klicken Sie auf der Seite **Benutzer** auf den Link **Entfernen** , der mit dem Namen angezeigt wird. Nachdem Sie bestätigt haben, dass Sie Sie entfernen möchten, kann der Benutzer, die Gruppe oder Azure AD Anwendung nicht mehr auf Ihr Partner Center-Konto zugreifen (es sei denn, Sie fügen es später erneut hinzu).

> [!IMPORTANT]
> Das Entfernen einer Benutzer-, Gruppen-oder Azure AD Anwendung bedeutet, dass Sie keinen Zugriff mehr auf Ihr Partner Center-Konto hat. Der Benutzer, die Gruppe oder Azure AD Anwendung wird **nicht** aus dem Verzeichnis Ihrer Organisation gelöscht.

 
