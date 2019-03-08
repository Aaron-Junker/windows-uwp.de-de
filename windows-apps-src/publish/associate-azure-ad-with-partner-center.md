---
Description: Zum Hinzufügen und Verwalten von Kontobenutzern, müssen Sie zuerst Ihr Partner Center-Konto in Ihrer Organisation Azure Active Directory zuordnen.
title: Zuordnen von Azure Active Directory mit Ihrem Partner Center-Konto
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Azure Ad, Azure-Mandant, AAD-Mandant, Azure AD-Mandant, Mandantenverwaltung, Mandanten
ms.localizationpriority: medium
ms.openlocfilehash: aacfdb0044fa9b9368ecbd032629ed5e572ece99
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605315"
---
# <a name="associate-azure-active-directory-with-your-partner-center-account"></a>Zuordnen von Azure Active Directory mit Ihrem Partner Center-Konto

Um [hinzufügen und Verwalten von Kontobenutzern](add-users-groups-and-azure-ad-applications.md), müssen Sie zuerst Ihr Partner Center-Konto zuordnen, in Ihrer Organisation Azure Active Directory. 

[Partner Center](https://partner.microsoft.com/dashboard) nutzt Azure AD für die Nutzung mehrerer Benutzerkonten Zugriff und Verwaltung. Wenn in Ihrer Organisation bereits mit Office 365 oder anderen Unternehmensdiensten von Microsoft gearbeitet wird, verfügen Sie bereits über Azure AD. Andernfalls können Sie ein neues erstellen Azure AD-Mandanten aus in Partner Center ohne zusätzliche Kosten.

> [!TIP]
> Dieses Thema bezieht sich auf das Windows-apps-Entwicklerprogramm in [Partner Center](https://partner.microsoft.com/dashboard), aber ein Mandant zuordnen und Verwalten von Benutzern funktioniert auf ähnliche Weise für Konten in der Windows-Desktop-Anwendung (finden Sie unter [Windows Desktop-Anwendungsprogramm](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users) für Weitere Informationen) und in das Windows Hardware Developer-Programm (, in denen Verweise auf die **Manager** Rolle gilt auch für Hardware-Konten mit den **Administrator**  Rolle; Siehe [Dashboard Verwaltung](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-administration) Informationen).

Ein einzelnes Azure AD-Mandanten kann mehrere Partner Center-Konten zugeordnet werden. Sie müssen nur eine Azure AD-Mandanten Ihrem Partner Center-Konto zugeordnet sind, um mehrere Kontobenutzer hinzufügen, aber haben auch die Möglichkeit, mehrere Azure AD-Mandanten mit einem einzelnen Partner Center-Konto hinzuzufügen. Jeder Benutzer mit der **Manager** Rolle im Partner Center-Konto haben die Option zum Hinzufügen und Entfernen von Azure AD-Mandanten aus dem-Konto.

> [!IMPORTANT]
> Nachdem Sie Azure AD-Mandanten mit Ihrem Partner Center-Konto zuordnen, um hinzufügen und Verwalten von Kontobenutzern-Mandanten, müssen zum Partner Center als ein Benutzer im gleichen Mandanten anmelden, wer hat die **Manager** Rolle.


## <a name="associate-your-partner-center-account-with-your-organizations-existing-azure-ad-tenant"></a>Weisen Sie Ihr Partner Center-Konto mit Azure AD-Mandanten Ihrer Organisation

Gehen folgendermaßen Sie vor, um das Partner Center-Konto verknüpft werden, wenn Ihre Organisation bereits Azure AD verwendet, haben.

1.  Von [Partner Center](https://partner.microsoft.com/dashboard), wählen Sie das Zahnradsymbol (in der Nähe der oberen rechten Ecke des Dashboards), und wählen Sie dann **entwicklereinstellungen**. In der **Einstellungen** , wählen Sie im Menü **Mandanten**.
2.  Wählen Sie **Zuordnen von Azure AD mit Ihrem Partner Center-Konto**.
3.  Geben Sie Ihre Azure AD-Anmeldeinformationen für den Mandanten ein, den Sie zuordnen möchten.
4.  Überprüfen Sie den Organisations- und den Domänennamen für den Azure AD-Mandant. Wählen Sie zum Abschließen der Zuordnung **Bestätigen** aus.
5.  Wenn die Zuordnung erfolgreich ist, dann sind Sie bereit zum Hinzufügen und Verwalten von Kontobenutzern in die **Benutzer** in Partner Center im Abschnitt.

> [!IMPORTANT]
> Um neue Benutzer zu erstellen oder andere Änderungen an Ihrem Azure AD vorzunehmen, müssen Sie sich mit einem Konto beim Azure AD-Mandanten anmelden, das über [über globale Administratorberechtigungen](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) für den Mandanten verfügt. Sie benötigen keinen globalen Administrator eine Zugriffsberechtigung zum Zuordnen des Mandanten oder zum Hinzufügen von Benutzern, die bereits in diesem Mandanten mit Ihrem Partner Center-Konto.


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account"></a>Erstellen Sie einen völlig neuen Azure AD mit Ihrem Partner Center-Konto zuordnen

Wenn Sie einen neuen Azure AD einrichten, um mit Ihrem Partner Center-Konto verknüpfen möchten, gehen Sie wie folgt vor.

1.  Von [Partner Center](https://partner.microsoft.com/dashboard), wählen Sie das Zahnradsymbol (in der Nähe der oberen rechten Ecke des Dashboards), und wählen Sie dann **entwicklereinstellungen**. In der **Einstellungen** , wählen Sie im Menü **Mandanten**.
2.  Wählen Sie **Neues Azure AD erstellen**.
3.  Geben Sie die Verzeichnisinformationen für das neue Azure AD ein:
    - **Domänenname**: Der eindeutige Name, mit denen der Azure AD-Domäne, zusammen mit ". onmicrosoft.com". Wenn Sie beispielsweise „beispiel“ eingegeben haben, wäre Ihre Azure AD-Domäne „beispiel.onmicrosoft.com“.
    - **E-Mail des Kontakts**: Eine-e-Mail-Adresse, in denen wir Sie zu Ihrem Konto bei Bedarf kontaktieren können.
    - **Globaler Administrator Benutzerkontoinformationen**: Der Vorname, letzte Name, Benutzername und Kennwort, das Sie für das neue globale Administratorkonto verwenden möchten.
4.  Klicken Sie auf **Erstellen**, um die neue Domäne und die Kontoinformationen zu bestätigen.
5.  Melden Sie sich mit dem neuen Azure AD-Benutzernamen und -Kennwort als globaler Administrator an, um [zusätzliche Kontobenutzer hinzuzufügen und zu verwalten](add-users-groups-and-azure-ad-applications.md).


## <a name="manage-azure-ad-tenant-associations"></a>Verwalten von Azure AD-Mandantenzuordnungen

Nachdem Sie Azure AD-Mandanten mit Ihrem Partner Center-Konto verknüpft haben, können Sie das Hinzufügen von neuer Mandanten oder Entfernen von vorhandenen Mandanten aus der **Mandanten** Seite.


### <a name="add-multiple-azure-ad-tenants-to-your-partner-center-account"></a>Hinzufügen von mehreren Azure AD-Mandanten mit Ihrem Partner Center-Konto

Jeder Benutzer mit der **Manager** Rolle für einen Partner Center-Konto kann Azure AD-Mandanten mit dem Konto zuordnen.

Wählen Sie zum Zuordnen eines neuen Mandanten **Associate another Azure AD tenant** aus und befolgen Sie die oben angegebenen Schritte. Beachten Sie, dass Sie zur Angabe Ihrer Anmeldeinformationen in dem Azure AD-Mandanten aufgefordert werden, den Sie verknüpfen möchten.


### <a name="remove-an-azure-ad-tenant-from-your-partner-center-account"></a>Entfernen Sie Azure AD-Mandanten aus Ihrem Partner Center-Konto

Jeder Benutzer mit der **Manager** Rolle für einen Partner Center-Konto kann Azure AD-Mandanten aus dem Konto entfernen.

> [!IMPORTANT]
> Wenn Sie einen Mandanten entfernen, werden alle Benutzer, die der Partner Center-Konto, aus diesem Mandanten hinzugefügt wurden, nicht mehr mit dem Konto anmelden können. 

Um einen Mandanten zu entfernen, finden Sie seinen Namen auf die **Mandanten** Seite (in **Kontoeinstellungen**), und wählen Sie dann **entfernen**. Sie werden aufgefordert, zu bestätigen, dass Sie den Mandanten entfernen möchten. Sobald Sie dies tun, keine Benutzer in diesem Mandanten werden in das Partner Center-Konto anmelden können, und alle Berechtigungen, die Sie für diese Benutzer konfiguriert haben, entfernt werden.

> [!TIP]
> Ein Mandant kann nicht entfernt werden, wenn Sie mit einem Konto im gleichen Mandanten derzeit in Partner Center angemeldet sind. Um einen Mandanten zu entfernen, müssen Sie sich anmelden im Partner Center als ein **Manager** für einen anderen Mandanten, die dem Konto zugeordnet ist. Wenn nur ein Mandanten dem Konto zugeordnet ist, kann dieser Mandant nur nach der Anmeldung mithilfe des Microsoft-Kontos entfernt werden, das das Konto eröffnete.


