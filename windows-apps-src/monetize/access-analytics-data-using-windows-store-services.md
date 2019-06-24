---
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: Verwenden Sie den Microsoft Store-Textanalyse-API programmgesteuert abrufen von Analysedaten für apps, die für Ihre oder Ihre Organisation registriert sind '' s Windows Partner Center-Konto.
title: Zugreifen auf Analysedaten mit Store-Diensten
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, UWP, Store-Dienste Microsoft Store-Analyse-API
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5514ea3a0e416ad2a0b7b75084bc66ad057c1a73
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320972"
---
# <a name="access-analytics-data-using-store-services"></a>Zugreifen auf Analysedaten mit Store-Diensten

Verwenden der *Microsoft Store-Textanalyse-API* programmgesteuert abrufen von Analysedaten für apps, die Ihre oder Ihre Organisation Windows Partner Center-Konto registriert sind. Mit dieser API können Sie Daten zum Kauf von Apps und Add-Ons (auch als In-App-Produkte (IAPs) bezeichnet), Fehler, App-Bewertungen und App-Rezensionen abrufen. Diese API verwendet Azure Active Directory (Azure AD), um die Aufrufe von Ihrer App oder Ihrem Dienst zu authentifizieren.

Dazu müssen folgende Schritte ausgeführt werden:

1.  Stellen Sie sicher, dass Sie alle [Voraussetzungen](#prerequisites) erfüllt haben.
2.  Vor dem Aufrufen einer Methode in der Microsoft Store-Analyse-API müssen Sie [ein Azure AD-Zugriffstoken anfordern](#obtain-an-azure-ad-access-token). Nach dem Abrufen eines Tokens haben Sie 60 Minuten Zeit, um das Token zum Aufrufen der Microsoft Store-Analyse-API zu nutzen, bevor es abläuft. Nach dem Ablauf des Tokens können Sie ein neues Token generieren.
3.  [Aufrufen der Microsoft Store-Analyse-API](#call-the-windows-store-analytics-api).

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-analytics-api"></a>Schritt 1: Voraussetzungen Sie für die Verwendung der Microsoft Store-Textanalyse-API

Stellen Sie sicher, dass Sie die folgenden Voraussetzungen erfüllt haben, bevor Sie mit dem Schreiben von Code zum Aufrufen der Microsoft Store-Analyse-API beginnen:

* Sie (bzw. Ihre Organisation) müssen über ein Azure AD-Verzeichnis und die Berechtigung [Globaler Administrator](https://go.microsoft.com/fwlink/?LinkId=746654) für das Verzeichnis verfügen. Wenn Sie bereits mit Office 365 oder anderen Unternehmensdiensten von Microsoft arbeiten, verfügen Sie schon über ein Azure AD-Verzeichnis. Andernfalls können Sie [erstellen Sie einen neuen Azure AD im Partner Center](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) ohne zusätzliche Gebühren.

* Sie müssen eine Azure AD-Anwendung mit Ihrem Partner Center-Konto zuordnen, Abrufen der Mandanten-ID und Client-ID für die Anwendung und einen Schlüssel generieren. Die Azure AD-Anwendung stellt die App oder den Dienst dar, aus denen Sie die Microsoft Store-Analyse-API aufrufen möchten. Sie benötigen die Mandanten-ID, die Client-ID und den Schlüssel zum Abrufen eines Azure AD-Zugriffstokens, das Sie an die API übergeben.
    > [!NOTE]
    > Sie müssen diesen Schritt nur einmal ausführen. Sobald Sie über die Mandanten-ID, Client-ID und den Schlüssel verfügen, können Sie diese Daten jederzeit wiederverwenden, um ein neues Azure AD-Zugriffstoken zu erstellen.

So ordnen Sie eine Azure AD-Anwendung mit Ihrem Partner Center-Konto und Abrufen der erforderlichen Werte:

1.  Im Partner Center [Ihrer Organisation Azure AD-Verzeichnis Ihrer Organisation Partner Center-Konto zuordnen](../publish/associate-azure-ad-with-partner-center.md).

2.  Als Nächstes wird von der **Benutzer** auf der Seite die **Kontoeinstellungen** Partner Center im Abschnitt [Hinzufügen von Azure AD-Anwendung](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) , das darstellt, die app oder Ihres Diensts, die Sie zu verwenden auf die Analytics-Daten für Ihr Partner Center-Konto. Weisen Sie dieser Anwendung anschließend die Rolle **Verwalter** zu. Wenn die Anwendung ist nicht vorhanden, noch Sie in Ihrem Azure AD-Verzeichnis können [erstellen Sie ein neues Azure AD-Anwendung im Partner Center](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).

3.  Wechseln Sie zurück zur Seite **Benutzer**, klicken Sie auf den Namen der Azure AD-Anwendung, um die Anwendungseinstellungen aufzurufen, und kopieren Sie die Werte unter **Mandanten-ID** und **Client-ID**.

4. Klicken Sie auf **Neuen Schlüssel hinzufügen**. Kopieren Sie auf dem folgenden Bildschirm den Wert unter **Schlüssel**. Nach dem Verlassen der Seite können Sie nicht mehr auf diese Informationen zugreifen. Weitere Informationen finden Sie unter [Verwalten von Schlüsseln für eine Azure AD-App](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Schritt 2: Abrufen eines Azure AD-Zugriffstokens

Bevor Sie die Methoden in der Microsoft Store-Analyse-API aufrufen, müssen Sie zuerst ein Azure AD-Zugriffstoken abrufen, das Sie für jede Methode in der API an den **Authorization**-Header übergeben. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Nachdem das Token abgelaufen ist, können Sie es aktualisieren, um es in weiteren Aufrufen an die API zu verwenden.

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

Für die *Mandanten\_Id* Wert in der POST-URI und die *Client\_Id* und *Client\_Geheimnis* den Mandanten zur Angabe von Parametern ID, Client-ID und den Schlüssel für Ihre Anwendung, die Sie vom Partner Center im vorherigen Abschnitt abgerufen haben. Für den Parameter *resource* müssen Sie ```https://manage.devcenter.microsoft.com``` angeben.

Nachdem das Zugriffstoken abgelaufen ist, können Sie es aktualisieren, indem Sie [diese Anleitung](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens) befolgen.

<span id="call-the-windows-store-analytics-api" />

## <a name="step-3-call-the-microsoft-store-analytics-api"></a>Schritt 3: Rufen Sie den Microsoft Store-Textanalyse-API

Nachdem Sie ein Azure AD-Zugriffstoken abgerufen haben, können Sie die Microsoft Store-Analyse-API aufrufen. Sie müssen das Zugriffstoken an den **Authorization**-Header der einzelnen Methoden übergeben.

### <a name="methods-for-uwp-apps-and-games"></a>Methoden für die UWP-apps und Spiele
Für apps und Spiele Übernahmen und Akquisitionen von Add-On-stehen die folgenden Methoden zur Auswahl: 

* [Abrufen von Übernahmen Daten für Ihre Spiele und apps](acquisitions-data.md)
* [Abrufen von Add-On-Akquisitionen Daten für Ihre Spiele und apps](add-on-acquisitions-data.md)

### <a name="methods-for-uwp-apps"></a>Methoden für UWP-Apps 

Die folgenden Analytics-Methoden sind für die UWP-apps in Partner Center verfügbar.

| Szenario       | Methoden      |
|---------------|--------------------|
| Übernahmen, Konvertierungen, Installation und Verwendung |  <ul><li>[Abrufen von app-Akquisitionen](get-app-acquisitions.md) (legacy)</li><li>[Abrufen von trichterdaten für app-Erwerb](get-acquisition-funnel-data.md) (legacy)</li><li>[App-Konvertierungen vom Kanal zu erhalten.](get-app-conversions-by-channel.md)</li><li>[Add-On-Akquisitionen abrufen](get-in-app-acquisitions.md)</li><li>[Abrufen der Abonnement-Add-On Übernahmen](get-subscription-acquisitions.md)</li><li>[Add-On-Konvertierungen vom Kanal zu erhalten.](get-add-on-conversions-by-channel.md)</li><li>[Abrufen von app-Installationen](get-app-installs.md)</li><li>[Erhalten Sie die Daten zur täglichen appnutzung](get-app-usage-daily.md)</li><li>[Monatliche appnutzung, die zu erhalten](get-app-usage-monthly.md)</li></ul> |
| App-Fehler | <ul><li>[Abrufen von Daten für die Fehlerberichterstattung](get-error-reporting-data.md)</li><li>[Abrufen von Details für einen Fehler in Ihrer app](get-details-for-an-error-in-your-app.md)</li><li>[Die stapelüberwachung für einen Fehler in Ihrer app abrufen](get-the-stack-trace-for-an-error-in-your-app.md)</li><li>[Laden Sie die CAB-Datei für einen Fehler in Ihrer app](download-the-cab-file-for-an-error-in-your-app.md)</li></ul> |
| Insights | <ul><li>[Abrufen von Insights-Daten für Ihre app](get-insights-data-for-your-app.md)</li></ul>  |
| Bewertungen und Prüfungen | <ul><li>[Abrufen von app-Bewertungen](get-app-ratings.md)</li><li>[Abrufen von app-Bewertungen](get-app-reviews.md)</li></ul> |
| In-App-Werbung und Anzeigenkampagnen | <ul><li>[Ad-Leistungsdaten abrufen](get-ad-performance-data.md)</li><li>[Ad-Kampagne-Leistungsdaten abrufen](get-ad-campaign-performance-data.md)</li></ul> |

### <a name="methods-for-desktop-applications"></a>Methoden für Desktopanwendungen

Die folgenden Analysemethoden stehen für die Verwendung durch Entwicklerkonten zur Verfügung, die zum [Windows Desktop Application-Programm](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program) gehören.

| Szenario       | Methoden      |
|---------------|--------------------|
| Installiert |  <ul><li>[Abrufen von desktop-Anwendung installiert](get-desktop-app-installs.md)</li></ul> |
| Blöcke |  <ul><li>[Rufen Sie ein Upgrade blockiert für die desktop-Anwendung](get-desktop-block-data.md)</li><li>[Upgrade-Block-Details für die desktop-Anwendung abrufen](get-desktop-block-data-details.md)</li></ul> |
| Anwendungsfehler |  <ul><li>[Abrufen von Daten für die desktop-Anwendung für die Fehlerberichterstattung](get-desktop-application-error-reporting-data.md)</li><li>[Abrufen von Details für einen Fehler in die desktop-Anwendung](get-details-for-an-error-in-your-desktop-application.md)</li><li>[Die stapelüberwachung für einen Fehler in die desktop-Anwendung abrufen.](get-the-stack-trace-for-an-error-in-your-desktop-application.md)</li><li>[Laden Sie die CAB-Datei in die desktop-Anwendung nach einem Fehler](download-the-cab-file-for-an-error-in-your-desktop-application.md)</li></ul> |
| Insights | <ul><li>[Abrufen von Insights-Daten für die desktop-Anwendung](get-insights-data-for-your-desktop-app.md)</li></ul>  |

### <a name="methods-for-xbox-live-services"></a>Methoden für Xbox Live-Dienste

Die folgenden zusätzlichen Methoden stehen für Entwicklerkonten mit Spielen zur Verfügung, die [Xbox Live-Dienste](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md) verwenden.

| Szenario       | Methoden      |
|---------------|--------------------|
| Allgemeine Analysen |  <ul><li>[Abrufen von Xbox Live-Analytics-Daten](get-xbox-live-analytics.md)</li><li>[Abrufen von Daten für Xbox Live Erfolge](get-xbox-live-achievements-data.md)</li><li>[Abrufen von Daten für Xbox Live gleichzeitige Verwendung](get-xbox-live-concurrent-usage-data.md)</li></ul> |
| Integritätsanalyse |  <ul><li>[Abrufen von Xbox Live-Health-Daten](get-xbox-live-health-data.md)</li></ul> |
| Community-Analyse |  <ul><li>[Abrufen von Xbox Live-Spiel Hub-Daten](get-xbox-live-game-hub-data.md)</li><li>[Abrufen von Xbox Live-Club-Daten](get-xbox-live-club-data.md)</li><li>[Abrufen von Xbox Live Multiplayer-Daten](get-xbox-live-multiplayer-data.md)</li></ul>  |

### <a name="methods-for-xbox-one-games"></a>Methoden für Xbox One-Spiele

Die folgenden zusätzlichen Methoden sind verfügbar für die Verwendung von entwicklerkonten mit Xbox One Spiele, die über die Xbox-Entwickler-Portal (XDP) erfasst wurden und in das XDP Analytics-Dashboard verfügbar.

| Szenario       | Methoden      |
|---------------|--------------------|
| Käufe |  <ul><li>[Xbox One Spiele Akquisitionen abrufen](get-xbox-one-game-acquisitions.md)</li><li>[Abrufen von Übernahmen für Xbox One-Add-On](get-xbox-one-add-on-acquisitions.md)</li></ul> |
| Fehler |  <ul><li>[Erhalten Sie die Daten für Ihre Xbox One Fehlerberichterstattung Spiele](get-error-reporting-data-for-your-xbox-one-game.md)</li><li>[Abrufen von Details für einen Fehler in Ihrer Xbox One-Spiel](get-details-for-an-error-in-your-xbox-one-game.md)</li><li>[Die stapelüberwachung für einen Fehler in Ihrer Xbox One Spiel abrufen](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)</li><li>[Laden Sie die CAB-Datei für einen Fehler in Ihrem Spiel Xbox One](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)</li></ul> |

### <a name="methods-for-hardware-and-drivers"></a>Methoden für Hardware und Treiber

Developer-Konten, die zu gehören die [Windows Hardware-Dashboard-Programm](https://docs.microsoft.com/windows-hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) haben Zugriff auf einen zusätzlichen Satz von Methoden zum Abrufen von Analysedaten für Hardware und Treiber. Weitere Informationen finden Sie unter [Hardware-Dashboard-API-](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-api).

## <a name="code-example"></a>Codebeispiel

Im folgenden Codebeispiel wird veranschaulicht, wie Sie ein Azure AD-Zugriffstoken abrufen und die Microsoft Store-Analyse-API aus einer C#-Konsolen-App aufrufen. Wenn Sie dieses Codebeispiel verwenden möchten, weisen Sie die Variablen *TenantId*, *ClientId*, *ClientSecret* und *AppID* den entsprechenden Werten für Ihr Szenario zu. In diesem Beispiel wird das [Json.NET-Paket](https://www.newtonsoft.com/json) von Newtonsoft benötigt, um die von der Microsoft Store-Analyse-API zurückgegebenen JSON-Daten zu deserialisieren.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[AnalyticsApi](./code/StoreServicesExamples_Analytics/cs/Program.cs#AnalyticsApiExample)]

## <a name="error-responses"></a>Fehlerantworten

Die Microsoft Store-Analyse-API gibt Fehlerantworten in einem JSON-Objekt zurück, das Fehlercodes und -meldungen enthält. Im folgenden Beispiel wird eine Fehlerantwort veranschaulicht, die durch einen ungültigen Parameter bewirkt wurde.

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
