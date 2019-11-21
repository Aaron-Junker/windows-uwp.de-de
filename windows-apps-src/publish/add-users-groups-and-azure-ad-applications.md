---
Description: You can add users, groups, and Azure AD applications to your Partner Center account.
title: Add users, groups, and Azure AD applications to your Partner Center account
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, azure ad application, aad, user, group, multiple users, multi-user
ms.localizationpriority: medium
ms.openlocfilehash: 41467f51e02f3cc700e3759f33d6fd6eea3ac7a6
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260079"
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-partner-center-account"></a>Add users, groups, and Azure AD applications to your Partner Center account

The **Users** section of [Partner Center](https://partner.microsoft.com/dashboard) (under **Account settings**) lets you use Azure Active Directory to add users to your Partner Center account. Jedem Benutzer wird eine Rolle (oder benutzerdefinierte Berechtigungen) zugewiesen, mit der er bestimmte Zugriffsberechtigungen für das Konto erhält. You can also add [groups of users](#groups) and [Azure AD applications](#azure-ad-applications) to grant them access to your Partner Center account.

Nachdem Benutzer auf das Konto hinzugefügt wurden, können Sie [Kontodetails bearbeiten](#edit), [Rollen und Berechtigungen](set-custom-permissions-for-account-users.md) ändern oder [Benutzer entfernen](#remove).

> [!IMPORTANT]
> In order to add users to your account, you must first [associate your Partner Center account with your organization's Azure Active Directory tenant](associate-azure-ad-with-partner-center.md). 

When adding users, you will need to specify their access to your Partner Center account by assigning them a [role or set of custom permissions](set-custom-permissions-for-account-users.md). 

Keep in mind that all Partner Center users (including groups and Azure AD applications) must have an active account in [an Azure AD tenant that is associated with your Partner Center account](associate-azure-ad-with-partner-center.md). Die Benutzerverwaltung erfolgt pro Mandant. Sie müssen sich mit einem Managerkonto auf dem Mandanten anmelden, wenn Sie Benutzer hinzufügen oder bearbeiten möchten. Creating a new user in Partner Center will also create an account for that user in the Azure AD tenant to which you are signed in, and making changes to a user's name in Partner Center will make the same changes in your organization's Azure AD tenant.

> [!NOTE]
> If your organization uses [directory integration](https://docs.microsoft.com/previous-versions/azure/azure-services/jj573653(v=azure.100)?redirectedfrom=MSDN) to sync the on-premises directory service with your Azure AD, you won't be able to create new users, groups, or Azure AD applications in Partner Center. You (or another admin in your on-premises directory) will need to create them directly in the on-premises directory before you'll be able to see and add them in Partner Center.


<span id="users" />

## <a name="add-users-to-your-partner-center-account"></a>Add users to your Partner Center account

To add users to your Partner Center account, go to the **Users** page in **Account settings** and select **Add users.** Sie müssen mit einem Managerkonto auf dem Azure AD-Mandanten angemeldet sein, auf dem Sie arbeiten möchten. 

### <a name="add-existing-users"></a>Hinzufügen vorhandener Benutzer 

You can select users who already exist in your organization's tenant and give them access to your Partner Center account. 

<span id="from-directory" />

1.  Select the gear icon (near the upper right corner of Partner Center) and then select **Developer settings**. In the **Settings** menu, select **Users**.
2.  Wählen Sie auf der Seite **Benutzer** **Benutzer hinzufügen** aus. 
3.  Wählen Sie in der angezeigten Liste einen oder mehrere Benutzer aus. Im Suchfeld können Sie nach bestimmten Benutzern suchen.
    > [!TIP]
    > If you select more than one user to add to your Partner Center account, you must assign them the same role or set of custom permissions. Wiederholen Sie zum Hinzufügen mehrerer Benutzer mit anderen Rollenberechtigungen die unten beschriebenen Schritte für alle Rollen oder benutzerdefinierte Berechtigungen.
4.  Wenn Sie die Benutzer ausgewählt haben, klicken Sie auf **Ausgewählte hinzufügen**.
5.  Geben Sie im Abschnitt **Rollen** an, welche [Rollen oder angepassten Berechtigungen](set-custom-permissions-for-account-users.md) Sie für die ausgewählten Benutzer wünschen.
6.  Klicken Sie auf **Speichern**.

### <a name="additional-methods-for-adding-users"></a>Zusätzliche Methoden zum Hinzufügen von Benutzern

If you are signed in with a Manager account which also has [global administrator](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) permissions for the Azure AD tenant you're working in, you will have additional options to add users to your Partner Center account. Sie müssen eine der folgenden Optionen auswählen:

-   **Add existing users**: Choose users who already exist in your organization's directory and give them access to your Partner Center account, using the method described above.
-   **Create new users**: Create brand new user accounts to add to both your organization's directory and your Partner Center account
-   **Invite outside users** (Externe Benutzer einladen): Laden Sie Benutzer per E-Mail ein, die derzeit nicht im Verzeichnis der Organisation vorhanden sind. They will be invited to access your Partner Center account, and a new [guest user](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) account will be created for them in your Azure AD tenant.

<span id="new-user" />

### <a name="create-new-users"></a>Erstellen neuer Benutzer

> [!IMPORTANT]
> Sie müssen mit einem globalen Administratorkonto auf Ihrem Azure AD-Mandanten angemeldet sein, um neue Benutzer zu erstellen.

1.  From the **Users** page (under **Account settings**), select **Add users**, then choose **Create new users**.
2.  Geben Sie den Vornamen, den Nachnamen und den Benutzernamen für den neuen Benutzer ein.
3.  Wenn der neue Benutzer ein [Konto als globaler Administrator](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) im Verzeichnis Ihrer Organisation haben soll, markieren Sie das Kontrollkästchen **Diesen Benutzer in Azure AD zum globalen Administrator mit vollständiger Kontrolle über alle Verzeichnisressourcen machen**. Dadurch erhält der Benutzer den vollständigen Zugriff auf alle administrativen Features im Azure AD Ihrer Organisation. They'll be able to add and manage users in your organization's directory (though not in Partner Center, unless you grant the account the appropriate [role/permissions](set-custom-permissions-for-account-users.md)). Wenn Sie dieses Kontrollkästchen markieren, müssen Sie eine **E-Mail-Adresse zur Kennwortwiederherstellung** für den Benutzer angeben.
4.  Wenn Sie das Kontrollkästchen **Diesen Benutzer in Azure AD zum globalen Administrator machen** markiert haben, geben Sie eine E-Mail-Adresse ein, die der Benutzer verwenden kann, um sein Kennwort wiederherzustellen.
5.  Wählen Sie im Abschnitt **Gruppenmitgliedschaft** alle Gruppen aus, denen der neue Benutzer angehören soll.
6.  Geben Sie im Abschnitt **Rollen** an, welche [Rollen oder angepassten Berechtigungen](set-custom-permissions-for-account-users.md) Sie für den ausgewählten Benutzer wünschen.
7.  Klicken Sie auf **Speichern**.
8.  Auf der Bestätigungsseite werden die Anmeldedaten für den neuen Benutzer angezeigt, z. B. ein temporäres Kennwort. Notieren Sie sich diese Informationen, und teilen Sie sie dem neuen Benutzer mit, da Sie nach dem Verlassen dieser Seite nicht mehr auf das temporäre Kennwort zugreifen können.


<span id="email" />

### <a name="invite-outside-users"></a>Einladen externer Benutzer

> [!IMPORTANT]
> Sie müssen auf Ihrem Azure AD-Mandanten mit einem globalen Administratorkonto angemeldet sein, um externe Benutzer einzuladen.

1.  From the **Users** page (under **Account settings**), select **Add users**, then choose **Invite users by email**.
1.  Geben Sie eine oder mehrere E-Mail-Adressen ein (bis zu zehn), die durch Kommas oder Semikolons getrennt sind.
2.  Geben Sie im Abschnitt **Rollen** an, welche [Rollen oder angepassten Berechtigungen](set-custom-permissions-for-account-users.md) Sie für den ausgewählten Benutzer wünschen.
3.  Klicken Sie auf **Speichern**.

Die von Ihnen eingeladenen Benutzer erhalten eine E-Mail-Einladung für Ihr Konto. Es wird ein neues [Gastbenutzer](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)-Konto für sie in Ihrem Azure AD-Mandanten erstellt. Jeder Benutzer muss die Einladung annehmen, bevor er auf Ihr Konto zugreifen kann.

Um die Einladung erneut zu senden, suchen Sie den Benutzer auf Ihrer **Benutzer**-Seite heraus, und wählen Sie seine E-Mail-Adresse (oder den Text **Einladung ausstehend**) aus. Klicken Sie anschließend am unteren Rand der Seite auf **Einladung senden**.

> [!IMPORTANT]
> Outside users that you invite to join your Partner Center account can be assigned the same roles and permissions as other users. Allerdings können externe Benutzern bestimmte Aufgaben in Visual Studio wie z. B. das Assoziieren einer App mit dem Store oder das Erstellen von Paketen zum Hochladen in den Store nicht durchführen. Wenn ein Benutzer diese Aufgaben durchführen muss, wählen Sie **Erstellen von neuen Benutzern** anstelle von **externe Benutzer einladen**. (Wenn Sie diese Benutzer nicht dem vorhandenen Azure AD-Mandanten hinzufügen möchten, können Sie [einen neuen Mandanten erstellen](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account), und anschließend für sie neue Benutzerkonten im Mandanten erstellen.) 


### <a name="changing-a-users-directory-password"></a>Ändern des Verzeichniskennworts eines Benutzers

Wenn ein Benutzer sein Kennwort ändern muss, kann er dies selber tun, wenn Sie ihm beim Erstellen des Benutzerkontos eine **E-Mail-Adresse zur Kennwortwiederherstellung** bereitgestellt haben. Sie können das Kennwort eines Benutzers auch aktualisieren, indem Sie die folgenden Schritte durchführen (wenn Sie sich mit einem globalen Administratorkonto in Ihrem Azure AD-Mandanten angemeldet haben, um das Kennwort des Benutzers zu ändern). Note that this will change the user's password in your Azure AD tenant, along with the password they use to access Partner Center. 

1.  From the **Users** page (under **Account settings**), select the name of the user account that you want to edit.
2.  Select the **Reset password** button at the bottom of the page.
3.  Auf einer Bestätigungsseite werden die Anmeldeinformationen für den Benutzer angezeigt, einschließlich eines temporären Kennworts.

    > [!IMPORTANT]
    >  Be sure to print or copy this info and provide it to the user, as you won't be able to access the temporary password after you leave this page.

<span id="groups" />

## <a name="add-groups-to-your-partner-center-account"></a>Add groups to your Partner Center account

You can add a group from your organization's directory to your Partner Center account. Daraufhin kann jeder Benutzer, der Mitglied dieser Gruppe ist, mit den Berechtigungen der dieser Gruppe zugewiesenen Rolle darauf zugreifen.

### <a name="add-groups-from-your-organizations-directory"></a>Hinzufügen von Gruppen aus dem Verzeichnis der Organisation

1.  Select the gear icon (near the upper right corner of Partner Center) and then select **Developer settings**. In the **Settings** menu, select **Users**.
2. From the **Users** page, select **Add groups**.
2.  Wählen Sie in der angezeigten Liste eine oder mehrere Gruppen aus. Im Suchfeld können Sie nach bestimmten Gruppen suchen.
    > [!TIP]
    > If you select more than one group to add to your Partner Center account, you must assign them the same role or set of custom permissions. Wiederholen Sie zum Hinzufügen mehrerer Gruppen mit anderen Rollenberechtigungen die unten beschriebenen Schritte für alle Rollen oder benutzerdefinierte Berechtigungen.

3.  Wenn Sie die Gruppen ausgewählt haben, klicken Sie auf **Ausgewählte hinzufügen**.
4.  Geben Sie im Abschnitt **Rollen** an, welche [Rollen oder angepassten Berechtigungen](set-custom-permissions-for-account-users.md) Sie für die ausgewählten Gruppen wünschen. All members of the group will be able to access your Partner Center account with the permissions you apply to the group, regardless of the roles/permissions associated with their individual account.
5.  Klicken Sie auf **Speichern**.


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Create a new group account in your organization's directory and add it to your Partner Center account

If you want to grant Partner Center access to a brand new group, you can create a new group in the **Users** section. Note that the new group will be created in your organization's directory, not just in your Partner Center account.

1.  From the **Users** page (under **Developer settings**), click **Add groups**.
2.  On the next page, select **New group**.
3.  Geben Sie den Anzeigenamen für die neue Gruppe ein.
4.  Geben Sie die [Rollen oder angepasste Berechtigungen](set-custom-permissions-for-account-users.md) für die Gruppe an. All members of the group will be able to access your Partner Center account with the permissions you apply to the group, regardless of the roles/permissions associated with their individual account.
5.  Wählen Sie den/die Benutzer, die der neuen Gruppe in der Liste zugewiesen werden sollen, aus der angezeigten Liste aus. Im Suchfeld können Sie nach bestimmten Benutzern suchen.
6.  Wenn Sie mit der Auswahl der Benutzer fertig sind, klicken Sie auf **Ausgewählte hinzufügen**, um diese der neuen Gruppe hinzuzufügen.
7.  Klicken Sie auf **Speichern**.


<span id="azure-ad-applications" />

## <a name="add-azure-ad-applications-to-your-partner-center-account"></a>Add Azure AD applications to your Partner Center account

You can allow applications or services that are part of your organization's Azure AD to access your Partner Center account. Diese Benutzerkonten für die Azure AD-Anwendung können zum Aufrufen der über [Microsoft Store Services](../monetize/using-windows-store-services.md) bereitgestellten REST-APIs genutzt werden.


### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>Hinzufügen von Azure AD-Anwendungen aus dem Verzeichnis Ihrer Organisation

1.  1.  Select the gear icon (near the upper right corner of Partner Center) and then select **Developer settings**. In the **Settings** menu, select **Users**.
2. Wählen Sie auf der Seite **Benutzer** **Azure AD-Anwendungen hinzufügen** aus.
3.  Wählen Sie eine oder Azure AD-Anwendungen aus der angezeigten Liste aus. Mithilfe des Suchfelds können Sie nach bestimmten Azure AD-Anwendungen suchen.
    > [!TIP]
    > If you select more than one Azure AD application to add to your Partner Center account, you must assign them the same role or set of custom permissions. Wiederholen Sie zum Hinzufügen mehrerer Azure AD-Anwendungen mit anderen Rollenberechtigungen die unten beschriebenen Schritte für alle Rollen oder benutzerdefinierten Berechtigungen.

4.  Wenn Sie die Azure AD-Anwendungen ausgewählt haben, klicken Sie auf **Ausgewählte hinzufügen**.
5.  Geben Sie im Abschnitt **Rollen** an, welche [Rollen oder angepassten Berechtigungen](set-custom-permissions-for-account-users.md) Sie für die ausgewählten Azure AD-Anwendungen wünschen.
6.  Klicken Sie auf **Speichern**.


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Create a new Azure AD application account in your organization's directory and add it to your Partner Center account

If you want to grant Partner Center access to a brand new Azure AD application account, you can create one in the **Users** section. Note that this will create a new account in your organization's directory, not just in your Partner Center account.

> [!TIP]
> If you are primarily using this Azure AD application for Partner Center authentication, and don't need users to access it directly, you can enter any valid address for the **Reply URL** and **App ID URI**, as long as those values are not used by any other Azure AD application in your directory.

1.  From the **Users** page (under **Account settings**), select **Add Azure AD applications**.
2.  On the next page, select **New Azure AD application**.
3.  Geben Sie die **Antwort-URL** für die neue Azure AD-App ein. Dies ist die URL, mit der sich Benutzer anmelden und Ihre Azure AD-App verwenden können (wird auch als App-URL oder Anmelde-URL bezeichnet). Die **Antwort-URL** darf nicht länger als 256 Zeichen sein und muss innerhalb des Verzeichnisses eindeutig sein.
4.  Geben Sie den **App-ID-URI** für die neue Azure AD-App ein. Dies ist ein logischer Bezeichner für die Azure AD-App, der beim Senden einer Anforderung für einmaliges Anmelden an Azure AD angezeigt wird. Beachten Sie, dass der **App-ID-URI** für jede Azure AD-App im Verzeichnis eindeutig sein muss und nicht mehr als 256 Zeichen enthalten darf. Weitere Informationen zur **App-ID-URI** finden Sie unter [Integrieren von Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#changing-the-application-registration-to-support-multi-tenant).
5.  Geben Sie im Abschnitt **Rollen** an, welche [Rollen oder angepassten Berechtigungen](set-custom-permissions-for-account-users.md) Sie für die Azure AD-Anwendungen wünschen.
6.  Klicken Sie auf **Speichern**.

Nachdem Sie eine Azure AD-Anwendung hinzugefügt oder erstellt haben, können Sie zum Abschnitt **Benutzer** zurückkehren und den Namen der Anwendung auswählen, um die Einstellungen für die Anwendung zu überprüfen, einschließlich Mandanten-ID, Client-ID, Antwort-URL und App-ID-URI.

> [!NOTE]
> Wenn Sie beabsichtigen, die REST-APIs zu verwenden, die von den [Microsoft Store-Diensten](../monetize/using-windows-store-services.md) bereitgestellt werden, benötigen Sie die auf dieser Seite angezeigten Werte für die Mandanten-ID und die Client-ID, um ein Azure AD-Zugriffstoken abzurufen, das Sie für die Authentifizierung der Aufrufe von Diensten verwenden können.   

<span id="manage-keys" />

### <a name="manage-keys-for-an-azure-ad-application"></a>Verwalten von Schlüsseln für eine Azure AD-App

Wenn die Azure AD-App Daten in Microsoft Azure AD liest und schreibt, benötigt sie einen Schlüssel. You can create keys for an Azure AD application by editing its info in Partner Center. Sie können auch Schlüssel entfernen, die nicht mehr benötigt werden.

1.  From the **Users** page (under **Account settings**), select the name of the Azure AD application.
    > [!TIP]
    > When you click the name of the Azure AD application, you'll see all of the active keys for the Azure AD application, including the date on which the key was created and when it will expire. Klicken Sie auf **Entfernen**, um einen nicht mehr benötigten Schlüssel zu entfernen.

2.  To add a new key, select **Add new key**.
3.  Es wird ein Bildschirm mit den Werten für **Client-ID** und **Schlüssel** angezeigt.
    > [!IMPORTANT]
    > Be sure to print or copy this info, as you won't be able to access it again after you leave this page.

4.  If you want to create more keys, select **Add another key**.

<span id="edit" />

## <a name="edit-a-user-group-or-azure-ad-application"></a>Bearbeiten von Benutzern, Gruppen oder Azure AD-Anwendungen

After you've added users, groups, and/or Azure AD applications to your Partner Center account, you can make changes to their account info. 

> [!IMPORTANT]
> Changes made to [roles or permissions](set-custom-permissions-for-account-users.md) will only affect Partner Center access. All other changes (such as changing a user's name or group membership, or the Reply URL and App ID URI for an Azure AD application) will be reflected in your organization's Azure AD tenant as well as in your Partner Center account. 

1.  From the **Users** page (under **Account settings**), select the name of the user, group, or Azure AD application account that you want to edit.
2.  Nehmen Sie die gewünschten Änderungen vor. Die Elemente, die Sie bearbeiten können, lauten wie folgt:
    -   Sie können den Vornamen, den Nachnamen oder den Benutzernamen eines **Benutzers** bearbeiten. Sie können ebenfalls Gruppen im Abschnitt **Gruppenmitgliedschaft** auswählen oder deaktivieren, um die Gruppenmitgliedschaft zu aktualisieren.
    -   Für eine **Gruppe** können Sie den Namen der Gruppe bearbeiten. (Um Gruppenmitgliedschaft zu aktualisieren, bearbeiten Sie die Benutzer, die Sie der Gruppe hinzufügen oder daraus entfernen möchten und nehmen Sie im Abschnitt **Gruppenmitgliedschaft** Änderungen vor.)
    -   Für eine **Azure AD-Anwendung** können Sie neue Werte für die **Antwort-URL** oder **App-ID-URI** eingeben.
    Remember that these changes will be made in your organization's directory as well as in your Partner Center account.
3.  For changes related to Partner Center access, select or deselect the role(s) that you want to apply, or select **Customize permissions** and make the desired changes. These changes only impact Partner Center access and will not change any permissions within your organization's Azure AD tenant.
3.  Klicken Sie auf **Speichern**.


## <a name="view-history-for-account-users"></a>Verlauf für Kontobenutzer anzeigen

Als Kontobesitzer können Sie den detaillierten Browserverlauf für alle weiteren Benutzer, die Sie dem Konto hinzugefügt haben, anzeigen.

On the **Users** page (under **Account settings**), select the link shown under **Last activity** for the user whose browsing history you’d like to review. Sie können die URLs aller Seiten anzeigen, die der Benutzer in den letzten 30 Tagen besucht habt.

<span id="remove" />

## <a name="remove-users-groups-and-azure-ad-applications"></a>Entfernen von Benutzern, Gruppen und Azure AD-Anwendungen

To remove a user, group, or Azure AD application from your Partner Center account, select the **Remove** link that appears by their name on the **Users** page. After confirming that you want to remove it, that user, group, or Azure AD application will no longer be able to access to your Partner Center account (unless you add it again later).

> [!IMPORTANT]
> Removing a user, group, or Azure AD application means that it will no longer have access to your Partner Center account. Dadurch werden **nicht** die betreffenden Benutzer, Gruppen oder Azure AD-Anwendungen aus dem Verzeichnis der Organisation gelöscht.

 

