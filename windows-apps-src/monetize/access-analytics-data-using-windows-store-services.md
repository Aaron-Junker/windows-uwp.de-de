---
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: Use the Microsoft Store analytics API to programmatically retrieve analytics data for apps that are registered to your or your organization''s Windows Partner Center account.
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

Use the *Microsoft Store analytics API* to programmatically retrieve analytics data for apps that are registered to your or your organization's Windows Partner Center account. Mit dieser API können Sie Daten zum Kauf von Apps und Add-Ons (auch als In-App-Produkte (IAPs) bezeichnet), Fehler, App-Bewertungen und App-Rezensionen abrufen. Diese API verwendet Azure Active Directory (Azure AD), um die Aufrufe von Ihrer App oder Ihrem Dienst zu authentifizieren.

Dazu müssen folgende Schritte ausgeführt werden:

1.  Stellen Sie sicher, dass Sie alle [Voraussetzungen](#prerequisites) erfüllt haben.
2.  Vor dem Aufrufen einer Methode in der Microsoft Store-Analyse-API müssen Sie [ein Azure AD-Zugriffstoken anfordern](#obtain-an-azure-ad-access-token). Nach dem Abrufen eines Tokens haben Sie 60 Minuten Zeit, um das Token zum Aufrufen der Microsoft Store-Analyse-API zu nutzen, bevor es abläuft. Nach dem Ablauf des Tokens können Sie ein neues Token generieren.
3.  [Aufrufen der Microsoft Store-Analyse-API](#call-the-windows-store-analytics-api).

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-analytics-api"></a>Schritt 1: Erfüllen der Voraussetzungen für die Verwendung der Microsoft Store-Analyse-API

Stellen Sie sicher, dass Sie die folgenden Voraussetzungen erfüllt haben, bevor Sie mit dem Schreiben von Code zum Aufrufen der Microsoft Store-Analyse-API beginnen:

* Sie (bzw. Ihre Organisation) müssen über ein Azure AD-Verzeichnis und die Berechtigung [Globaler Administrator](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) für das Verzeichnis verfügen. Wenn Sie bereits mit Office 365 oder anderen Business Services von Microsoft arbeiten, verfügen Sie schon über ein Azure AD-Verzeichnis. Otherwise, you can [create a new Azure AD in Partner Center](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) for no additional charge.

* You must associate an Azure AD application with your Partner Center account, retrieve the tenant ID and client ID for the application and generate a key. Die Azure AD-Anwendung stellt die App oder den Dienst dar, aus denen Sie die Microsoft Store-Analyse-API aufrufen möchten. Sie benötigen die Mandanten-ID, die Client-ID und den Schlüssel zum Abrufen eines Azure-AD-Zugriffstokens, das Sie an die API übergeben.
    > [!NOTE]
    > Sie müssen diesen Schritt nur einmal ausführen. Wenn Sie im Besitz der Mandanten-ID, der Client-ID und des Schlüssel sind, können Sie diese Daten jederzeit wiederverwenden, um ein neues Azure AD-Zugriffstoken zu erstellen.

To associate an Azure AD application with your Partner Center account and retrieve the required values:

1.  In Partner Center, [associate your organization's Partner Center account with your organization's Azure AD directory](../publish/associate-azure-ad-with-partner-center.md).

2.  Next, from the **Users** page in the **Account settings** section of Partner Center, [add the Azure AD application](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) that represents the app or service that you will use to access analytics data for your Partner Center account. Weisen Sie dieser Anwendung anschließend die Rolle **Verwalter** zu. If the application doesn't exist yet in your Azure AD directory, you can [create a new Azure AD application in Partner Center](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).

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

For the *tenant\_id* value in the POST URI and the *client\_id* and *client\_secret* parameters, specify the tenant ID, client ID and the key for your application that you retrieved from Partner Center in the previous section. Für den Parameter *resource* müssen Sie ```https://manage.devcenter.microsoft.com``` angeben.

Nachdem das Zugriffstoken abgelaufen ist, können Sie es aktualisieren, indem Sie [diese Anleitung](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens) befolgen.

<span id="call-the-windows-store-analytics-api" />

## <a name="step-3-call-the-microsoft-store-analytics-api"></a>Schritt 3: Aufrufen der Microsoft Store-Analyse-API

Nachdem Sie ein Azure AD-Zugriffstoken abgerufen haben, können Sie die Microsoft Store-Analyse-API aufrufen. Sie müssen das Zugriffstoken an den **Authorization**-Header der einzelnen Methoden übergeben.

### <a name="methods-for-uwp-apps-and-games"></a>Methods for UWP apps and games
The following methods are available for apps and games acquisitions and add-on acquisitions: 

* [Get acquisitions data for your games and apps](acquisitions-data.md)
* [Get add-on acquisitions data for your games and apps](add-on-acquisitions-data.md)

### <a name="methods-for-uwp-apps"></a>Methoden für UWP-Apps 

The following analytics methods are available for UWP apps in Partner Center.

| Szenario       | Methoden      |
|---------------|--------------------|
| Acquisitions, conversions, installs, and usage |  <ul><li>[Get app acquisitions](get-app-acquisitions.md) (legacy)</li><li>[Get app acquisition funnel data](get-acquisition-funnel-data.md) (legacy)</li><li>[Get app conversions by channel](get-app-conversions-by-channel.md)</li><li>[Get add-on acquisitions](get-in-app-acquisitions.md)</li><li>[Get subscription add-on acquisitions](get-subscription-acquisitions.md)</li><li>[Get add-on conversions by channel](get-add-on-conversions-by-channel.md)</li><li>[Get app installs](get-app-installs.md)</li><li>[Get daily app usage](get-app-usage-daily.md)</li><li>[Get monthly app usage](get-app-usage-monthly.md)</li></ul> |
| App-Fehler | <ul><li>[Get error reporting data](get-error-reporting-data.md)</li><li>[Get details for an error in your app](get-details-for-an-error-in-your-app.md)</li><li>[Get the stack trace for an error in your app](get-the-stack-trace-for-an-error-in-your-app.md)</li><li>[Download the CAB file for an error in your app](download-the-cab-file-for-an-error-in-your-app.md)</li></ul> |
| Insights | <ul><li>[Get insights data for your app](get-insights-data-for-your-app.md)</li></ul>  |
| Bewertungen und Prüfungen | <ul><li>[Get app ratings](get-app-ratings.md)</li><li>[Get app reviews](get-app-reviews.md)</li></ul> |
| In-App-Werbung und Anzeigenkampagnen | <ul><li>[Get ad performance data](get-ad-performance-data.md)</li><li>[Get ad campaign performance data](get-ad-campaign-performance-data.md)</li></ul> |

### <a name="methods-for-desktop-applications"></a>Methoden für Desktopanwendungen

Die folgenden Analysemethoden stehen für die Verwendung durch Entwicklerkonten zur Verfügung, die zum [Windows Desktop Application-Programm](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program) gehören.

| Szenario       | Methoden      |
|---------------|--------------------|
| Installiert |  <ul><li>[Get desktop application installs](get-desktop-app-installs.md)</li></ul> |
| Blocks |  <ul><li>[Get upgrade blocks for your desktop application](get-desktop-block-data.md)</li><li>[Get upgrade block details for your desktop application](get-desktop-block-data-details.md)</li></ul> |
| Anwendungsfehler |  <ul><li>[Get error reporting data for your desktop application](get-desktop-application-error-reporting-data.md)</li><li>[Get details for an error in your desktop application](get-details-for-an-error-in-your-desktop-application.md)</li><li>[Get the stack trace for an error in your desktop application](get-the-stack-trace-for-an-error-in-your-desktop-application.md)</li><li>[Download the CAB file for an error in your desktop application](download-the-cab-file-for-an-error-in-your-desktop-application.md)</li></ul> |
| Insights | <ul><li>[Get insights data for your desktop application](get-insights-data-for-your-desktop-app.md)</li></ul>  |

### <a name="methods-for-xbox-live-services"></a>Methoden für Xbox Live-Dienste

Die folgenden zusätzlichen Methoden stehen für Entwicklerkonten mit Spielen zur Verfügung, die [Xbox Live-Dienste](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md) verwenden.

| Szenario       | Methoden      |
|---------------|--------------------|
| Allgemeine Analysen |  <ul><li>[Get Xbox Live analytics data](get-xbox-live-analytics.md)</li><li>[Get Xbox Live achievements data](get-xbox-live-achievements-data.md)</li><li>[Get Xbox Live concurrent usage data](get-xbox-live-concurrent-usage-data.md)</li></ul> |
| Integritätsanalyse |  <ul><li>[Get Xbox Live health data](get-xbox-live-health-data.md)</li></ul> |
| Community-Analyse |  <ul><li>[Get Xbox Live Game Hub data](get-xbox-live-game-hub-data.md)</li><li>[Get Xbox Live club data](get-xbox-live-club-data.md)</li><li>[Get Xbox Live multiplayer data](get-xbox-live-multiplayer-data.md)</li></ul>  |

### <a name="methods-for-xbox-one-games"></a>Methoden für Xbox One-Spiele

The following additional methods are available for use by developer accounts with Xbox One games that were ingested through the Xbox Developer Portal (XDP) and available in the XDP Analytics dashboard.

| Szenario       | Methoden      |
|---------------|--------------------|
| Käufe |  <ul><li>[Get Xbox One game acquisitions](get-xbox-one-game-acquisitions.md)</li><li>[Get Xbox One add-on acquisitions](get-xbox-one-add-on-acquisitions.md)</li></ul> |
| Fehler |  <ul><li>[Get error reporting data for your Xbox One game](get-error-reporting-data-for-your-xbox-one-game.md)</li><li>[Get details for an error in your Xbox One game](get-details-for-an-error-in-your-xbox-one-game.md)</li><li>[Get the stack trace for an error in your Xbox One game](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)</li><li>[Download the CAB file for an error in your Xbox One game](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)</li></ul> |

### <a name="methods-for-hardware-and-drivers"></a>Methoden für Hardware und Treiber

Developer accounts that belong to the [Windows hardware dashboard program](https://docs.microsoft.com/windows-hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) have access to an additional set of methods for retrieving analytics data for hardware and drivers. For more information, see [Hardware dashboard API](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-api).

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
