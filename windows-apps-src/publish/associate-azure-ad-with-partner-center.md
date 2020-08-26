---
Description: Um Kontobenutzer hinzuzufügen und zu verwalten, müssen Sie Ihr Partner Center-Konto zunächst dem Azure Active Directory Ihrer Organisation zuordnen.
title: Zuordnen von Azure Active Directory zu Ihrem Partner Center-Konto
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Azure AD, Azure-Mandant, AAD-Mandant, Azure AD-Mandant, Mandanten Verwaltung, Mandanten
ms.localizationpriority: medium
ms.openlocfilehash: fa26f4ea3fa91f43dea117ff5ffd2fec87514544
ms.sourcegitcommit: 720413d2053c8d5c5b34d6873740be6e913a4857
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88846760"
---
# <a name="associate-azure-active-directory-with-your-partner-center-account"></a>Zuordnen von Azure Active Directory zu Ihrem Partner Center-Konto

Um [Kontobenutzer hinzuzufügen und zu verwalten](add-users-groups-and-azure-ad-applications.md), müssen Sie Ihr Partner Center-Konto zunächst dem Azure Active Directory Ihrer Organisation zuordnen. 

[Partner Center](https://partner.microsoft.com/dashboard) nutzt Azure AD für den Zugriff und die Verwaltung von mehr Benutzerkonten. Wenn Ihre Organisation bereits Microsoft 365 oder andere Unternehmens Dienste von Microsoft verwendet, verfügen Sie bereits über Azure AD. Andernfalls können Sie ohne zusätzliche Kosten einen neuen Azure AD Mandanten innerhalb von Partner Center erstellen.

> [!TIP]
> Dieses Thema bezieht sich auf das Entwicklerprogramm von Windows-apps in [Partner Center](https://partner.microsoft.com/dashboard), aber die Zuordnung eines Mandanten und der Verwaltung von Benutzern funktioniert ähnlich für Konten im Windows-Desktop Anwendungsprogramm (Weitere Informationen finden Sie unter [Windows-Desktop-Anwendungsprogramm](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users) ) und im Windows-Hardware Entwicklerprogramm (in dem Verweise auf die **Manager** -Rolle [auch für Hardware](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-administration) Konten mit der **Administrator** Rolle gelten).

Ein einzelner Azure AD Mandanten kann mehreren Partner Center-Konten zugeordnet werden. Sie benötigen nur einen Azure AD Mandanten, der Ihrem Partner Center-Konto zugeordnet ist, um mehrere Kontobenutzer hinzuzufügen, aber Sie haben auch die Möglichkeit, einem einzelnen Partner Center-Konto mehrere Azure AD Mandanten hinzuzufügen. Jeder Benutzer mit der **Manager**-Rolle im Partner Center-Konto hat die Möglichkeit, Azure AD-Mandanten im Konto hinzuzufügen und zu entfernen.

> [!IMPORTANT]
> Nachdem Sie Ihr Partner Center-Konto mit Ihrem Azure AD-Mandanten verknüpft haben, müssen Sie sich bei Partner Center als Benutzer im selben Mandanten anmelden, der über die **Manager** -Rolle verfügt, um Kontobenutzer in diesem Mandanten hinzuzufügen und zu verwalten.


## <a name="associate-your-partner-center-account-with-your-organizations-existing-azure-ad-tenant"></a>Verknüpfen Sie Ihr Partner Center-Konto mit dem vorhandenen Azure AD Mandanten Ihrer Organisation.

Wenn Ihre Organisation Azure AD bereits verwendet, führen Sie die folgenden Schritte aus, um Ihr Partner Center-Konto zu verknüpfen.

1.  Wählen Sie im [Partner Center](https://partner.microsoft.com/dashboard)das Zahnrad Symbol (in der Nähe der oberen rechten Ecke des Dashboards), und wählen Sie dann **Developer Settings (Entwicklereinstellungen**) aus. **Wählen Sie**im Menü " **Einstellungen** " die Option Mandanten aus.
2.  Wählen Sie **Azure AD Ihrem Partner Center-Konto zuordnen**aus.
3.  Geben Sie Ihre Azure AD-Anmeldeinformationen für den Mandanten ein, den Sie zuordnen möchten.
4.  Überprüfen Sie die Organisation und den Domänennamen für Ihren Azure AD-Mandanten. Wählen Sie zum Abschließen der Zuordnung **Bestätigen** aus.
5.  Wenn die Zuordnung erfolgreich ist, können Sie anschließend in Partner Center im Abschnitt **Benutzer** Kontobenutzer hinzufügen und verwalten.

> [!IMPORTANT]
> Um neue Benutzer zu erstellen oder andere Änderungen am Azure AD vorzunehmen, müssen Sie sich bei diesem Azure AD Mandanten mit einem Konto anmelden, das über die [Berechtigung "globaler Administrator](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) " für diesen Mandanten verfügt. Sie benötigen jedoch keine globale Administrator Berechtigung, um den Mandanten zuzuordnen, oder Sie können Benutzer, die bereits in diesem Mandanten vorhanden sind, Ihrem Partner Center-Konto hinzufügen.


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account"></a>Erstellen Sie eine neue Azure AD, die Ihrem Partner Center-Konto zugeordnet werden soll.

Wenn Sie einen neuen Azure AD einrichten müssen, der mit Ihrem Partner Center-Konto verknüpft werden soll, führen Sie die folgenden Schritte aus.

1.  Wählen Sie im [Partner Center](https://partner.microsoft.com/dashboard)das Zahnrad Symbol (in der Nähe der oberen rechten Ecke des Dashboards), und wählen Sie dann **Developer Settings (Entwicklereinstellungen**) aus. **Wählen Sie**im Menü " **Einstellungen** " die Option Mandanten aus.
2.  Wählen Sie **neue Azure AD erstellen**aus.
3.  Geben Sie die Verzeichnisinformationen für das neue Azure AD ein:
    - **Domain Name (Domänen Name**): der eindeutige Name, den wir für Ihre Azure AD Domäne verwenden, zusammen mit ". onmicrosoft.com". Wenn Sie beispielsweise „Beispiel“ eingegeben haben, würde Ihre Azure AD-Domäne „Beispiel.onmicrosoft.com“ lauten.
    - **Kontakt-E-Mail**: Eine E-Mail-Adresse, über die wir Sie bei Bedarf bezüglich Ihres Konto kontaktieren können.
    - **Benutzerkontoinformationen für Globaler Administrator**: Der Vorname, Nachname, Benutzername und das Kennwort, die Sie für das neue globale Administratorkonto verwenden möchten.
4.  Klicken Sie auf **Erstellen**, um die neue Domäne und die Kontoinformationen zu bestätigen.
5.  Melden Sie sich mit Ihrem neuen Azure AD globalen Administrator Benutzernamen und-Kennwort an, um [Weitere Kontobenutzer hinzuzufügen und zu verwalten](add-users-groups-and-azure-ad-applications.md).


## <a name="manage-azure-ad-tenant-associations"></a>Verwalten Azure AD Mandanten Zuordnungen

Nachdem Sie Ihrem Partner Center-Konto einen Azure AD Mandanten zugeordnet haben, können Sie neue Mandanten hinzufügen oder vorhandene Mandanten aus der **Mandanten Seite entfernen** .


### <a name="add-multiple-azure-ad-tenants-to-your-partner-center-account"></a>Fügen Sie Ihrem Partner Center-Konto mehrere Azure AD Mandanten hinzu.

Alle Benutzer, die über die **Manager** -Rolle für ein Partner Center-Konto verfügen, können Azure AD Mandanten dem Konto zuordnen.

Wenn Sie einen neuen Mandanten zuordnen möchten, wählen Sie **einen anderen Azure AD Mandanten zuordnen**aus, und führen Sie dann die oben aufgeführten Schritte aus. Beachten Sie, dass Sie aufgefordert werden, Ihre Anmelde Informationen in der Azure AD Mandanten einzugeben, die Sie zuordnen möchten.


### <a name="remove-an-azure-ad-tenant-from-your-partner-center-account"></a>Entfernen eines Azure AD-Mandanten aus Ihrem Partner Center-Konto

Alle Benutzer, die über die **Manager** -Rolle für ein Partner Center-Konto verfügen, können Azure AD Mandanten aus dem Konto entfernen.

> [!IMPORTANT]
> Wenn Sie einen Mandanten entfernen, kann sich kein Benutzer mehr bei dem Konto anmelden, der über diesen Mandanten zum Partner Center-Konto hinzugefügt wurde. 

Um einen Mandanten zu entfernen, suchen Sie seinen Namen **auf der Mandanten** Seite (in den **Kontoeinstellungen**), und wählen Sie dann **Entfernen**aus. Sie werden gefragt, ob Sie den Mandanten wirklich entfernen möchten. Sobald Sie ihn entfernt haben, kann sich kein Benutzer in diesem Mandanten mehr beim Partner Center-Konto anmelden, und alle Berechtigungen, die Sie für diese Benutzer konfiguriert haben, werden entfernt.

> [!TIP]
> Ein Mandant kann nicht entfernt werden, wenn Sie derzeit über ein Konto im betreffenden Mandanten in Partner Center angemeldet sind. Um einen Mandanten zu entfernen, müssen Sie sich in Partner Center als **Manager** für einen anderen Mandanten anmelden, der dem Konto zugeordnet ist. Wenn dem Konto nur ein einziger Mandant zugeordnet, kann dieser Mandant erst nach der Anmeldung bei dem Microsoft-Konto entfernt werden, das das Konto eröffnet hat.


