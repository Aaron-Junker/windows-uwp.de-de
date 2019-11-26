---
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: Verwenden Sie die Microsoft Store Analytics-API zum programmgesteuerten Abrufen von Analysedaten für apps, die für das Windows Partner Center-Konto Ihrer Organisation registriert sind.
title: Zugreifen auf Analysedaten mit Store-Diensten
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, UWP, Store-Dienste Microsoft Store-Analyse-API
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 71c59049b76219d6f9360748e9ca11ea84542e47
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259318"
---
# <a name="access-analytics-data-using-store-services"></a>Zugreifen auf Analysedaten mit Store-Diensten

Verwenden Sie die *Microsoft Store Analytics-API* zum programmgesteuerten Abrufen von Analysedaten für apps, die im Windows Partner Center-Konto Ihrer Organisation registriert sind. Mit dieser API können Sie Daten zum Kauf von Apps und Add-Ons (auch als In-App-Produkte (IAPs) bezeichnet), Fehler, App-Bewertungen und App-Rezensionen abrufen. Diese API verwendet Azure Active Directory (Azure AD), um die Aufrufe von Ihrer App oder Ihrem Dienst zu authentifizieren.

Dazu müssen folgende Schritte ausgeführt werden:

1.  Stellen Sie sicher, dass Sie alle [Voraussetzungen](#prerequisites) erfüllt haben.
2.  Vor dem Aufrufen einer Methode in der Microsoft Store-Analyse-API müssen Sie [ein Azure AD-Zugriffstoken anfordern](#obtain-an-azure-ad-access-token). Nach dem Abrufen eines Tokens haben Sie 60 Minuten Zeit, um das Token zum Aufrufen der Microsoft Store-Analyse-API zu nutzen, bevor es abläuft. Nach dem Ablauf des Tokens können Sie ein neues Token generieren.
3.  [Aufrufen der Microsoft Store-Analyse-API](#call-the-windows-store-analytics-api).

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-analytics-api"></a>Schritt 1: Erfüllen der Voraussetzungen für die Verwendung der Microsoft Store-Analyse-API

Stellen Sie sicher, dass Sie die folgenden Voraussetzungen erfüllt haben, bevor Sie mit dem Schreiben von Code zum Aufrufen der Microsoft Store-Analyse-API beginnen:

* Sie (bzw. Ihre Organisation) müssen über ein Azure AD-Verzeichnis und die Berechtigung [Globaler Administrator](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) für das Verzeichnis verfügen. Wenn Sie bereits mit Office 365 oder anderen Business Services von Microsoft arbeiten, verfügen Sie schon über ein Azure AD-Verzeichnis. Andernfalls können Sie [eine neue Azure AD im Partner Center erstellen](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) , ohne dass zusätzliche Kosten anfallen.

* Sie müssen Ihrem Partner Center-Konto eine Azure AD Anwendung zuordnen, die Mandanten-ID und die Client-ID für die Anwendung abrufen und einen Schlüssel generieren. Die Azure AD-Anwendung stellt die App oder den Dienst dar, aus denen Sie die Microsoft Store-Analyse-API aufrufen möchten. Sie benötigen die Mandanten-ID, die Client-ID und den Schlüssel zum Abrufen eines Azure-AD-Zugriffstokens, das Sie an die API übergeben.
    > [!NOTE]
    > Sie müssen diesen Schritt nur einmal ausführen. Wenn Sie im Besitz der Mandanten-ID, der Client-ID und des Schlüssel sind, können Sie diese Daten jederzeit wiederverwenden, um ein neues Azure AD-Zugriffstoken zu erstellen.

So ordnen Sie eine Azure AD Anwendung Ihrem Partner Center-Konto zu und rufen die erforderlichen Werte ab:

1.  Ordnen Sie in Partner Center das [Partner Center-Konto Ihrer Organisation dem Azure AD-Verzeichnis Ihrer Organisation zu](../publish/associate-azure-ad-with-partner-center.md).

2.  Fügen Sie als nächstes auf der Seite " **Benutzer** " im Abschnitt " **Kontoeinstellungen** " von Partner Center [die Azure AD Anwendung hinzu](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) , die die APP oder den Dienst darstellt, die Sie für den Zugriff auf Analysedaten für Ihr Partner Center-Konto verwenden. Weisen Sie dieser Anwendung anschließend die Rolle **Verwalter** zu. Wenn die Anwendung noch nicht in Ihrem Azure AD Verzeichnis vorhanden ist, können Sie [eine neue Azure AD Anwendung im Partner Center erstellen](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).

3.  Wechseln Sie zurück zur Seite **Benutzer**, klicken Sie auf den Namen der Azure AD-Anwendung, um die Anwendungseinstellungen aufzurufen, und kopieren Sie die Werte unter **Mandanten-ID** und **Client-ID**.

4. Klicken Sie auf **Neuen Schlüssel hinzufügen**. Kopieren Sie auf dem folgenden Bildschirm den Wert unter **Schlüssel**. Nach dem Verlassen der Seite können Sie nicht mehr auf diese Informationen zugreifen. Weitere Informationen finden Sie unter [Verwalten von Schlüsseln für eine Azure AD-App](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Schritt 2: Abrufen eines Azure AD-Zugriffstokens

Bevor Sie die Methoden in der Microsoft Store-Analyse-API aufrufen, müssen Sie zuerst ein Azure AD-Zugriffstoken abrufen, das Sie für jede Methode in der API an den **Authorization**-Header übergeben. Nachdem Sie ein Zugriffstoken abgerufen haben, können Sie es 60 Minuten lang verwenden, bevor es abläuft. Nachdem das Token abgelaufen ist, können Sie es aktualisieren, um es in weiteren Aufrufen an die API zu verwenden.

Befolgen Sie zum Abrufen des Zugriffstokens die Anweisungen unter [Aufrufe zwischen Diensten mithilfe von Clientanmeldeinformationen](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/), um eine HTTP POST-Anforderung an den ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```-Endpunkt zu senden. Hier ist ein Beispiel für eine Anforderung angegeben.

```json
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

Geben Sie für den Mandanten *\_ID* -Wert im Post-URI und die Parameter " *Client\_ID* " und " *Client\_Secret* " die Mandanten-ID, die Client-ID und den Schlüssel für die Anwendung an, die Sie im vorherigen Abschnitt aus Partner Center abgerufen haben. Für den Parameter *resource* müssen Sie ```https://manage.devcenter.microsoft.com``` angeben.

Nachdem das Zugriffstoken abgelaufen ist, können Sie es aktualisieren, indem Sie [diese Anleitung](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens) befolgen.

<span id="call-the-windows-store-analytics-api" />

## <a name="step-3-call-the-microsoft-store-analytics-api"></a>Schritt 3: Aufrufen der Microsoft Store-Analyse-API

Nachdem Sie ein Azure AD-Zugriffstoken abgerufen haben, können Sie die Microsoft Store-Analyse-API aufrufen. Sie müssen das Zugriffstoken an den **Authorization**-Header der einzelnen Methoden übergeben.

### <a name="methods-for-uwp-apps-and-games"></a>Methoden für UWP-apps und Spiele
Die folgenden Methoden sind für apps und Spiele Käufe und Add-on-Käufe verfügbar: 

* [Erhalten von Erstellungs Daten für Ihre Spiele und Apps](acquisitions-data.md)
* [Holen Sie sich Add-on-Erstellungs Daten für Ihre Spiele und Apps](add-on-acquisitions-data.md)

### <a name="methods-for-uwp-apps"></a>Methoden für UWP-Apps 

Die folgenden Analysemethoden sind für UWP-apps im Partner Center verfügbar.

| Szenario       | Methoden      |
|---------------|--------------------|
| Akquisitionen, Konvertierungen, Installationen und Verwendung |  <ul><li>[App-Käufe erhalten](get-app-acquisitions.md) (Legacy)</li><li>Abrufen von [Trichter Daten](get-acquisition-funnel-data.md) für die APP-Beschaffung (Legacy)</li><li>[App-Konvertierungen nach Kanal erhalten](get-app-conversions-by-channel.md)</li><li>[Add-on-Akquisitionen erhalten](get-in-app-acquisitions.md)</li><li>[Abonnement-Add-on-Käufe erhalten](get-subscription-acquisitions.md)</li><li>[Add-on-Konvertierungen nach Kanal](get-add-on-conversions-by-channel.md)</li><li>[App-Installationen erhalten](get-app-installs.md)</li><li>[Tägliche App-Nutzung erhalten](get-app-usage-daily.md)</li><li>[Monatliche App-Nutzung erhalten](get-app-usage-monthly.md)</li></ul> |
| App-Fehler | <ul><li>[Fehler Berichterstattungs Daten erhalten](get-error-reporting-data.md)</li><li>[Details zu einem Fehler in Ihrer APP erhalten](get-details-for-an-error-in-your-app.md)</li><li>[Stapel Überwachung für einen Fehler in Ihrer APP erhalten](get-the-stack-trace-for-an-error-in-your-app.md)</li><li>[Herunterladen der CAB-Datei für einen Fehler in Ihrer APP](download-the-cab-file-for-an-error-in-your-app.md)</li></ul> |
| Aufschluss | <ul><li>[Erhalten von Insights-Daten für Ihre APP](get-insights-data-for-your-app.md)</li></ul>  |
| Bewertungen und Prüfungen | <ul><li>[APP-Bewertungen erhalten](get-app-ratings.md)</li><li>[App-Reviews erhalten](get-app-reviews.md)</li></ul> |
| In-App-Werbung und Anzeigenkampagnen | <ul><li>[Anzeigen von Leistungsdaten](get-ad-performance-data.md)</li><li>[Leistungsdaten der Werbekampagne erhalten](get-ad-campaign-performance-data.md)</li></ul> |

### <a name="methods-for-desktop-applications"></a>Methoden für Desktopanwendungen

Die folgenden Analysemethoden stehen für die Verwendung durch Entwicklerkonten zur Verfügung, die zum [Windows Desktop Application-Programm](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program) gehören.

| Szenario       | Methoden      |
|---------------|--------------------|
| Installiert |  <ul><li>[Installieren von Desktop Anwendungen](get-desktop-app-installs.md)</li></ul> |
| Blöcke |  <ul><li>[Aktualisieren Sie upgradeblöcke für Ihre Desktop Anwendung.](get-desktop-block-data.md)</li><li>[Aktualisierungs Block Details für Ihre Desktop Anwendung](get-desktop-block-data-details.md)</li></ul> |
| Anwendungsfehler |  <ul><li>[Get-Fehlerberichts Daten für Ihre Desktop Anwendung](get-desktop-application-error-reporting-data.md)</li><li>[Details zu einem Fehler in der Desktop Anwendung erhalten](get-details-for-an-error-in-your-desktop-application.md)</li><li>[Stapel Überwachung für einen Fehler in der Desktop Anwendung erhalten](get-the-stack-trace-for-an-error-in-your-desktop-application.md)</li><li>[Herunterladen der CAB-Datei für einen Fehler in der Desktop Anwendung](download-the-cab-file-for-an-error-in-your-desktop-application.md)</li></ul> |
| Aufschluss | <ul><li>[Erhalten von Insights-Daten für Ihre Desktop Anwendung](get-insights-data-for-your-desktop-app.md)</li></ul>  |

### <a name="methods-for-xbox-live-services"></a>Methoden für Xbox Live-Dienste

Die folgenden zusätzlichen Methoden stehen für Entwicklerkonten mit Spielen zur Verfügung, die [Xbox Live-Dienste](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md) verwenden.

| Szenario       | Methoden      |
|---------------|--------------------|
| Allgemeine Analysen |  <ul><li>[Erhalten von Xbox Live Analytics-Daten](get-xbox-live-analytics.md)</li><li>[Erhalten von Xbox Live-Erstellungs Daten](get-xbox-live-achievements-data.md)</li><li>[Online-Daten zur gleichzeitigen Verwendung von Xbox Live](get-xbox-live-concurrent-usage-data.md)</li></ul> |
| Integritätsanalyse |  <ul><li>[Erhalten von Xbox Live Health-Daten](get-xbox-live-health-data.md)</li></ul> |
| Community-Analyse |  <ul><li>[Get Xbox Live Game Hub-Daten](get-xbox-live-game-hub-data.md)</li><li>[Xbox Live Club-Daten erhalten](get-xbox-live-club-data.md)</li><li>[Xbox Live-multiplayerdaten erhalten](get-xbox-live-multiplayer-data.md)</li></ul>  |

### <a name="methods-for-xbox-one-games"></a>Methoden für Xbox One-Spiele

Die folgenden zusätzlichen Methoden können von Entwickler Konten mit Xbox One-spielen verwendet werden, die über das Xbox Developer Portal (XDP) erfasst wurden und im XDP-Analyse Dashboard verfügbar sind.

| Szenario       | Methoden      |
|---------------|--------------------|
| Käufe |  <ul><li>[Holen Sie sich die Xbox One-Spiele](get-xbox-one-game-acquisitions.md)</li><li>[Holen Sie sich eine Get-on-Übernahme von Xbox One](get-xbox-one-add-on-acquisitions.md)</li></ul> |
| Fehler |  <ul><li>[Fehler Berichterstattungs Daten für Ihr Xbox One-Spiel](get-error-reporting-data-for-your-xbox-one-game.md)</li><li>[Details zu einem Fehler in Ihrem Xbox One-Spiel](get-details-for-an-error-in-your-xbox-one-game.md)</li><li>[Erhalten Sie die Stapel Überwachung für einen Fehler in Ihrem Xbox One-Spiel.](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)</li><li>[Herunterladen der CAB-Datei für einen Fehler in Ihrem Xbox One-Spiel](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)</li></ul> |

### <a name="methods-for-hardware-and-drivers"></a>Methoden für Hardware und Treiber

Entwickler Konten, die zum [Windows-Hardware-dashboardprogramm](https://docs.microsoft.com/windows-hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) gehören, haben Zugriff auf einen zusätzlichen Satz von Methoden zum Abrufen von Analysedaten für Hardware und Treiber. Weitere Informationen finden Sie unter [Hardware Dashboard-API](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-api).

## <a name="code-example"></a>Codebeispiel

Im folgenden Codebeispiel wird veranschaulicht, wie Sie ein Azure AD-Zugriffstoken abrufen und die Microsoft Store-Analyse-API aus einer C#-Konsolen-App aufrufen. Wenn Sie dieses Codebeispiel verwenden möchten, weisen Sie den Variablen *tenantId*, *clientId*, *clientSecret* und *appID* die entsprechenden Werte für Ihr Szenario zu. In diesem Beispiel wird das [Json.NET-Paket](https://www.newtonsoft.com/json) von Newtonsoft benötigt, um die von der Microsoft Store-Analyse-API zurückgegebenen JSON-Daten zu deserialisieren.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[AnalyticsApi](./code/StoreServicesExamples_Analytics/cs/Program.cs#AnalyticsApiExample)]

## <a name="error-responses"></a>Fehlerantworten

Die Microsoft Store-Analyse-API gibt Fehlerantworten in einem JSON-Objekt zurück, das Fehlercodes und -meldungen enthält. Im folgenden Beispiel wird eine Fehlerantwort veranschaulicht, die durch einen ungültigen Parameter verursacht wurde.

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
