---
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: Verwenden Sie die Microsoft Store Analytics-API zum programmgesteuerten Abrufen von Analysedaten für apps, die für das Windows Partner Center-Konto Ihrer Organisation registriert sind.
title: Zugreifen auf Analytics-Daten mithilfe von Store Services
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store Analytics-API
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8becf9149d0afa888d0024619df06f2103c7bf8b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158644"
---
# <a name="access-analytics-data-using-store-services"></a>Zugreifen auf Analytics-Daten mithilfe von Store Services

Verwenden Sie die *Microsoft Store Analytics-API* zum programmgesteuerten Abrufen von Analysedaten für apps, die im Windows Partner Center-Konto Ihrer Organisation registriert sind. Mit dieser API können Sie Daten zum Kauf von Apps und Add-Ons (auch als In-App-Produkte (IAPs) bezeichnet), Fehler, App-Bewertungen und App-Rezensionen abrufen. Diese API verwendet Azure Active Directory (Azure AD), um die Aufrufe von Ihrer App oder Ihrem Dienst zu authentifizieren.

Dazu müssen folgende Schritte ausgeführt werden:

1.  Stellen Sie sicher, dass Sie alle [Voraussetzungen](#prerequisites) erfüllt haben.
2.  Bevor Sie eine Methode in der Microsoft Store Analytics-API aufrufen, [erhalten Sie ein Azure AD Zugriffs Token](#obtain-an-azure-ad-access-token). Nachdem Sie ein Token erhalten haben, haben Sie 60 Minuten Zeit, dieses Token in Aufrufen der Microsoft Store Analytics-API zu verwenden, bevor das Token abläuft. Nach dem Ablauf des Tokens können Sie ein neues Token generieren.
3.  [Ruft die Microsoft Store Analytics-API](#call-the-windows-store-analytics-api)auf.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-analytics-api"></a>Schritt 1: Voraussetzungen für die Verwendung der Microsoft Store Analytics-API

Bevor Sie mit dem Schreiben von Code beginnen, um die Microsoft Store Analytics-API aufzurufen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind.

* Sie (oder Ihre Organisation) müssen über ein Azure AD-Verzeichnis verfügen, und Ihnen müssen die Berechtigungen [globaler Administrator](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) für das Verzeichnis gewährt worden sein. Wenn Sie Microsoft 365 oder andere Unternehmensdienste von Microsoft verwenden, verfügen Sie bereits über ein Azure AD-Verzeichnis. Andernfalls können Sie [eine neue Azure AD im Partner Center erstellen](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) , ohne dass zusätzliche Kosten anfallen.

* Sie müssen Ihrem Partner Center-Konto eine Azure AD Anwendung zuordnen, die Mandanten-ID und die Client-ID für die Anwendung abrufen und einen Schlüssel generieren. Die Azure AD-Anwendung stellt die APP oder den Dienst dar, von dem aus Sie die Microsoft Store Analytics-API aufzurufen möchten. Sie benötigen die Mandanten-ID, die Client-ID und den Schlüssel, um ein Azure AD-Zugriffstoken zu erhalten, das Sie an die API übergeben.
    > [!NOTE]
    > Sie müssen diese Aufgabe nur einmal ausführen. Nachdem Sie über die Mandanten-ID, die Client-ID und den Schlüssel verfügen, können Sie diese jederzeit wiederverwenden, wenn Sie ein neues Azure AD-Zugriffstoken erstellen müssen.

So ordnen Sie eine Azure AD Anwendung Ihrem Partner Center-Konto zu und rufen die erforderlichen Werte ab:

1.  Verknüpfen Sie in Partner Center [das Partner Center-Konto Ihres Unternehmens mit dem Azure AD-Verzeichnis](../publish/associate-azure-ad-with-partner-center.md) Ihrer Organisation.

2.  Fügen Sie als nächstes auf der Seite " **Benutzer** " im Abschnitt " **Kontoeinstellungen** " von Partner Center [die Azure AD Anwendung hinzu](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) , die die APP oder den Dienst darstellt, die Sie für den Zugriff auf Analysedaten für Ihr Partner Center-Konto verwenden. Stellen Sie sicher, dass Sie dieser Anwendung die Rolle **Manager** zuweisen. Wenn die Anwendung noch nicht in Ihrem Azure AD-Verzeichnis vorhanden ist, können Sie [in Partner Center eine neue Azure AD-Anwendung erstellen](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).

3.  Kehren Sie zur Seite **Benutzer** zurück, klicken Sie auf den Namen Ihrer Azure AD-Anwendung, um die Anwendungseinstellungen zu öffnen, und schreiben Sie die Werte **Mandanten-ID** und **Client-ID** auf.

4. Klicken Sie auf **Neuen Schlüssel hinzufügen**. Notieren Sie auf dem folgenden Bildschirm den Wert von **Schlüssel**. Nachdem Sie diese Seite verlassen haben, können Sie nicht mehr auf diese Informationen zugreifen. Weitere Informationen finden Sie unter [Verwalten von Schlüsseln für eine Azure AD-Anwendung](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Schritt 2: Abrufen eines Azure AD-Zugriffstokens

Bevor Sie eine der Methoden in der Microsoft Store Analytics-API aufrufen, müssen Sie zuerst ein Azure AD Zugriffs Token abrufen, das Sie an den **Autorisierungs** Header jeder Methode in der API übergeben. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Nachdem das Token abgelaufen ist, können Sie es aktualisieren, um es in weiteren Aufrufen an die API zu verwenden.

Befolgen Sie zum Abrufen des Zugriffstokens die Anweisungen unter [Aufrufe zwischen Diensten mithilfe von Clientanmeldeinformationen](/azure/active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow), um eine HTTP POST-Anforderung an den ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```-Endpunkt zu senden. Hier ist ein Beispiel für eine Anforderung angegeben.

```json
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

Geben Sie als Mandanten- * \_ ID* im Post-URI und in den Parametern für die *Client- \_ ID* und den * \_ geheimen Client* Schlüssel die Mandanten-ID, die Client-ID und den Schlüssel für die Anwendung an, die Sie im vorherigen Abschnitt aus Partner Center abgerufen haben. Für den Parameter *resource* müssen Sie ```https://manage.devcenter.microsoft.com``` angeben.

Nachdem das Zugriffstoken abgelaufen ist, können Sie es aktualisieren, indem Sie [diese Anleitung](/azure/active-directory/azuread-dev/v1-protocols-oauth-code#refreshing-the-access-tokens) befolgen.

<span id="call-the-windows-store-analytics-api" />

## <a name="step-3-call-the-microsoft-store-analytics-api"></a>Schritt 3: Abrufen der Microsoft Store Analytics-API

Nachdem Sie über ein Azure AD Zugriffs Token verfügen, können Sie die Microsoft Store Analytics-API aufrufen. Sie müssen das Zugriffstoken an den **Authorization**-Header der einzelnen Methoden übergeben.

### <a name="methods-for-uwp-apps-and-games"></a>Methoden für UWP-apps und Spiele
Die folgenden Methoden sind für apps und Spiele Käufe und Add-on-Käufe verfügbar: 

* [Abrufen von Kaufdaten für Ihre Spiele und Apps](acquisitions-data.md)
* [Abrufen von Add-On-Kaufdaten für Ihre Spiele und Apps](add-on-acquisitions-data.md)

### <a name="methods-for-uwp-apps"></a>Methoden für UWP-apps 

Die folgenden Analysemethoden sind für UWP-apps im Partner Center verfügbar.

| Szenario       | Methoden      |
|---------------|--------------------|
| Akquisitionen, Konvertierungen, Installationen und Verwendung |  <ul><li>[App-Käufe erhalten](get-app-acquisitions.md) (Legacy)</li><li>Abrufen von [Trichter Daten](get-acquisition-funnel-data.md) für die APP-Beschaffung (Legacy)</li><li>[Abrufen von App-Konvertierungen nach Kanal](get-app-conversions-by-channel.md)</li><li>[Abrufen von Add-On-Käufen](get-in-app-acquisitions.md)</li><li>[Abrufen von Add-On-Käufen für Abonnements](get-subscription-acquisitions.md)</li><li>[Abrufen von Add-On-Konvertierungen nach Kanal](get-add-on-conversions-by-channel.md)</li><li>[Abrufen von App-Installationen](get-app-installs.md)</li><li>[Abrufen der täglichen App-Nutzung](get-app-usage-daily.md)</li><li>[Abrufen der monatlichen App-Nutzung](get-app-usage-monthly.md)</li></ul> |
| App-Fehler | <ul><li>[Abrufen von Fehlerberichtsdaten](get-error-reporting-data.md)</li><li>[Abrufen von Details zu einem Fehler in Ihrer App](get-details-for-an-error-in-your-app.md)</li><li>[Abrufen der Stapelüberwachung für einen Fehler in Ihrer App](get-the-stack-trace-for-an-error-in-your-app.md)</li><li>[Herunterladen der CAB-Datei bei einem Fehler in der App](download-the-cab-file-for-an-error-in-your-app.md)</li></ul> |
| Einblicke | <ul><li>[Abrufen von internen Daten über die App](get-insights-data-for-your-app.md)</li></ul>  |
| Bewertungen und Rezensionen | <ul><li>[Abrufen von App-Bewertungen](get-app-ratings.md)</li><li>[Abrufen von App-Rezensionen](get-app-reviews.md)</li></ul> |
| In-App-Werbeeinblendungen und Werbekampagnen | <ul><li>[Abrufen von Anzeigenleistungsdaten](get-ad-performance-data.md)</li><li>[Abrufen der Leistungsdaten einer Anzeigenkampagne](get-ad-campaign-performance-data.md)</li></ul> |

### <a name="methods-for-desktop-applications"></a>Methoden für Desktop Anwendungen

Die folgenden Analysemethoden können von Entwickler Konten verwendet werden, die zum [Windows-Desktop Anwendungsprogramm](/windows/desktop/appxpkg/windows-desktop-application-program)gehören.

| Szenario       | Methoden      |
|---------------|--------------------|
| Video |  <ul><li>[Abrufen von Desktopanwendungsinstallationen](get-desktop-app-installs.md)</li></ul> |
| Blöcke |  <ul><li>[Abrufen von Upgradeblockierungen für Ihre Desktopanwendung](get-desktop-block-data.md)</li><li>[Abrufen von Details zur Upgradeblockierung für Ihre Desktopanwendung](get-desktop-block-data-details.md)</li></ul> |
| Anwendungsfehler |  <ul><li>[Abrufen von Fehlerberichtsdaten für Ihre Desktopanwendung](get-desktop-application-error-reporting-data.md)</li><li>[Abrufen von Details zu einem Fehler in Ihrer Desktopanwendung](get-details-for-an-error-in-your-desktop-application.md)</li><li>[Abrufen der Stapelüberwachung für einen Fehler in Ihrer Desktopanwendung](get-the-stack-trace-for-an-error-in-your-desktop-application.md)</li><li>[Herunterladen der CAB-Datei bei einem Fehler in der Desktopanwendung](download-the-cab-file-for-an-error-in-your-desktop-application.md)</li></ul> |
| Einblicke | <ul><li>[Abrufen von Analysedaten für Ihre Desktopanwendung](get-insights-data-for-your-desktop-app.md)</li></ul>  |

### <a name="methods-for-xbox-live-services"></a>Methoden für Xbox Live-Dienste

Die folgenden zusätzlichen Methoden sind für Entwickler Konten mit Spielen verfügbar, die [Xbox Live-Dienste](/gaming/xbox-live/developer-program-overview.md)verwenden.

| Szenario       | Methoden      |
|---------------|--------------------|
| Allgemeine Analysen |  <ul><li>[Abrufen von Xbox Live-Analysedaten](get-xbox-live-analytics.md)</li><li>[Abrufen von Xbox Live-Erfolgsdaten](get-xbox-live-achievements-data.md)</li><li>[Abrufen von Xbox Live-Daten zur gleichzeitigen Nutzung](get-xbox-live-concurrent-usage-data.md)</li></ul> |
| Integritäts Analyse |  <ul><li>[Abrufen von Xbox Live-Integritätsdaten](get-xbox-live-health-data.md)</li></ul> |
| Community Analytics |  <ul><li>[Abrufen von Xbox Live Spielehubdaten](get-xbox-live-game-hub-data.md)</li><li>[Abrufen von Xbox Live-Clubdaten](get-xbox-live-club-data.md)</li><li>[Abrufen von Xbox Live Multiplayerdaten](get-xbox-live-multiplayer-data.md)</li></ul>  |

### <a name="methods-for-hardware-and-drivers"></a>Methoden für Hardware und Treiber

Entwickler Konten, die zum [Windows-Hardware-dashboardprogramm](/windows-hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) gehören, haben Zugriff auf einen zusätzlichen Satz von Methoden zum Abrufen von Analysedaten für Hardware und Treiber. Weitere Informationen finden Sie unter [Hardware Dashboard-API](/windows-hardware/drivers/dashboard/dashboard-api).

## <a name="code-example"></a>Codebeispiel

Im folgenden Codebeispiel wird veranschaulicht, wie Sie ein Azure AD Zugriffs Token abrufen und die Microsoft Store Analytics-API aus einer c#-Konsolen-App aufrufen. Wenn Sie dieses Codebeispiel verwenden möchten, weisen Sie die Variablen *TenantId*, *ClientId*, *ClientSecret* und *AppID* den entsprechenden Werten für Ihr Szenario zu. Für dieses Beispiel ist es erforderlich, dass das [JSON.net-Paket](https://www.newtonsoft.com/json) von newtonsoft die von der Microsoft Store Analytics-API zurückgegebenen JSON-Daten deserialisiert.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[AnalyticsApi](./code/StoreServicesExamples_Analytics/cs/Program.cs#AnalyticsApiExample)]

## <a name="error-responses"></a>Fehlercodes

Die Microsoft Store Analytics-API gibt Fehler Antworten in einem JSON-Objekt zurück, das Fehlercodes und Meldungen enthält. Im folgenden Beispiel wird eine Fehlerantwort veranschaulicht, die durch einen ungültigen Parameter bewirkt wurde.

```json
{
    "code":"BadRequest",
    "data":[],
    "details":[],
    "innererror":{
        "code":"InvalidQueryParameters",
        "data":[
            "top parameter cannot be more than 10000"
        ],
        "details":[],
        "message":"One or More Query Parameters has invalid values.",
        "source":"AnalyticsAPI"
    },
    "message":"The calling client sent a bad request to the service.",
    "source":"AnalyticsAPI"
}
```