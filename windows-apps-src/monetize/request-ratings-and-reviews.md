---
Description: Erfahren Sie mehr über verschiedene Möglichkeiten, wie Sie Kundenprogramm gesteuert ermöglichen können, Ihre APP zu bewerten und zu überprüfen.
title: Anfordern von Bewertungen und Prüfungen für Ihre App
ms.date: 01/22/2019
ms.topic: article
keywords: Windows 10, UWP, Bewertungen, Rezensionen
ms.localizationpriority: medium
ms.openlocfilehash: b167f4cc40ee72e6405436bacee28f2f20b4623c
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210716"
---
# <a name="request-ratings-and-reviews-for-your-app"></a>Anfordern von Bewertungen und Prüfungen für Ihre App

Sie können Code zu Ihrer Universellen Windows-Plattform (UWP)-App hinzufügen, um Ihre Kunden programmgesteuert aufzufordern, Ihre App zu bewerten oder zu rezensieren. Hierfür stehen Ihnen mehrere Möglichkeiten zur Verfügung:
* Sie können ein Bewertungs- und Rezensionsdialogfeld direkt im Kontext Ihrer App anzeigen.
* Sie können programmgesteuert die Bewertungs- und Rezensionsseite für Ihre App im Microsoft Store öffnen.

Wenn Sie bereit sind, Ihre Bewertungen zu analysieren und Daten zu überprüfen, können Sie die Daten im Partner Center anzeigen oder die Microsoft Store Analytics-API verwenden, um diese Daten Programm gesteuert abzurufen.

> [!IMPORTANT]
> Wenn Sie in Ihrer APP eine Bewertungsfunktion hinzufügen, müssen alle Überprüfungen den Benutzer unabhängig von der gewählten Star-Bewertung an die Bewertungsmechanismen des Stores senden. Wenn Sie Feedback oder Kommentare von Benutzern erfassen, muss klar sein, dass es nicht mit der APP-Bewertung oder Überprüfungen im Store verknüpft ist, sondern direkt an den App-Entwickler gesendet wird. Weitere Informationen im Zusammenhang mit [betrügerischen oder unehrlichen Aktivitäten](https://docs.microsoft.com/legal/windows/agreements/store-developer-code-of-conduct#3-fraudulent-or-dishonest-activities)finden Sie im Entwickler-Verhaltenskodex.

## <a name="show-a-rating-and-review-dialog-in-your-app"></a>Ein Bewertungs- und Rezensionsdialogfeld in Ihrer App anzeigen

Um Programm gesteuert einen Dialog in Ihrer APP anzuzeigen, der Ihren Kunden auffordert, Ihre APP zu bewerten und einen Review zu übermitteln, rufen Sie die [requestrateandreviewappasync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestrateandreviewappasync) -Methode im [Windows. Services. Store](https://docs.microsoft.com/uwp/api/windows.services.store) -Namespace auf. 

> [!IMPORTANT]
> Die Anforderung zum Anzeigen des Bewertungs- und Rezensionsdialogfelds muss im UI-Thread in Ihrer App aufgerufen werden.

```csharp
using Windows.ApplicationModel.Store;

private StoreContext _storeContext;

public async Task Initialize()
{
    if (App.IsMultiUserApp) // pseudo-code
    {
        IReadOnlyList<User> users = await User.FindAllAsync();
        User firstUser = users[0];
        _storeContext = StoreContext.GetForUser(firstUser);
    }
    else
    {
        _storeContext = StoreContext.GetDefault();
    }
}

private async Task PromptUserToRateApp()
{
    // Check if we’ve recently prompted user to review, we don’t want to bother user too often and only between version changes
    if (HaveWePromptedUserInPastThreeMonths())  // pseudo-code
    {
        return;
    }

    StoreRateAndReviewResult result = await 
        _storeContext.RequestRateAndReviewAppAsync();

    // Check status
    switch (result.Status)
    { 
        case StoreRateAndReviewStatus.Succeeded:
            // Was this an updated review or a new review, if Updated is false it means it was a users first time reviewing
            if (result.UpdatedExistingRatingOrReview)
            {
                // This was an updated review thank user
                ThankUserForReview(); // pseudo-code
            }
            else
            {
                // This was a new review, thank user for reviewing and give some free in app tokens
                ThankUserForReviewAndGrantTokens(); // pseudo-code
            }
            // Keep track that we prompted user and don’t do it again for a while
            SetUserHasBeenPrompted(); // pseudo-code
            break;

        case StoreRateAndReviewStatus.CanceledByUser:
            // Keep track that we prompted user and don’t prompt again for a while
            SetUserHasBeenPrompted(); // pseudo-code

            break;

        case StoreRateAndReviewStatus.NetworkError:
            // User is probably not connected, so we’ll try again, but keep track so we don’t try too often
            SetUserHasBeenPromptedButHadNetworkError(); // pseudo-code

            break;

        // Something else went wrong
        case StoreRateAndReviewStatus.OtherError:
        default:
            // Log error, passing in ExtendedJsonData however it will be empty for now
            LogError(result.ExtendedError, result.ExtendedJsonData); // pseudo-code
            break;
    }
}
```

Die **requestrateandreviewappasync** -Methode wurde in Windows 10, Version 1809, eingeführt und kann nur in Projekten verwendet werden, die auf das **Windows 10-Update vom Oktober 2018 abzielen (10,0; Build 17763)** oder eine spätere Version in Visual Studio.

### <a name="response-data-for-the-rating-and-review-request"></a>Antwortdaten für die Bewertungs- und Rezensionsanforderung

Nachdem Sie die Anforderung zum Anzeigen des Dialog Felds "Bewertung und Überprüfung" eingereicht haben, enthält die [extendedjsondata](https://docs.microsoft.com/uwp/api/windows.services.store.storerateandreviewresult.extendedjsondata) -Eigenschaft der [storerateandreviewresult](https://docs.microsoft.com/uwp/api/windows.services.store.storerateandreviewresult) -Klasse eine JSON-formatierte Zeichenfolge, die angibt, ob die Anforderung erfolgreich war.

Das folgende Beispiel veranschaulicht den Rückgabewert für diese Anforderung, nachdem der Kunde erfolgreich eine Bewertung oder Rezension übermittelt hat.

```json
{ 
  "status": "success", 
  "data": {
    "updated": false
  },
  "errorDetails": "Success"
}
```

Das folgende Beispiel veranschaulicht den Rückgabewert für diese Anforderung, nachdem der Kunde gewählt hat, keine Bewertung oder Rezension zu übermitteln.

```json
{ 
  "status": "aborted", 
  "errorDetails": "Navigation was unsuccessful"
}
```

In der folgenden Tabelle werden die Felder in der Zeichenfolge im JSON-Format erläutert.

| Field          | Beschreibung                                                                                                                                   |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| *Stands*       | Eine Zeichenfolge, die angibt, ob der Kunde eine Bewertung oder Rezension erfolgreich gesendet hat. Die unterstützten Werte sind **success** und **aborted**. |
| *Vorrats*         | Ein Objekt, das nur einen booleschen Wert mit dem Namen *aktualisiert* enthält. Dieser Wert gibt an, ob der Kunde eine vorhandene Bewertung oder Rezension aktualisiert hat. Das *data*-Objekt ist nur in den Erfolgsantworten enthalten. |
| *errorDetails* | Eine Zeichenfolge, die Fehlerdetails für die Anforderung enthält.                                                                                     |

## <a name="launch-the-rating-and-review-page-for-your-app-in-the-store"></a>Die Bewertungs- und Rezensionsseite für Ihre App im Store starten

Wenn Sie programmgesteuert eine Bewertungs- und Rezensionsseite für Ihre App im Store öffnen möchten, können Sie die [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync)-Methode mit dem ```ms-windows-store://review```-URI-Schema verwenden, wie in diesem Codebeispiel gezeigt wird.

```csharp
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://review/?ProductId=9WZDNCRFHVJL"));
```

Weitere Informationen finden Sie unter [Die Microsoft Store App starten](../launch-resume/launch-store-app.md).

## <a name="analyze-your-ratings-and-reviews-data"></a>Ihre Bewertungs- und Rezensionsdaten analysieren

Sie haben mehrere Optionen, um die Bewertungs- und Rezensionsdaten von Ihren Kunden zu analysieren:
* Sie können den Bericht [Reviews](../publish/reviews-report.md) im Partner Center verwenden, um die Bewertungen und Bewertungen ihrer Kunden anzuzeigen. Sie können diesen Bericht auch herunterladen, um ihn offline zu sehen.
* Sie können die [Abrufen von App-Bewertungen](get-app-ratings.md)- und [Abrufen von App-Rezensionen](get-app-reviews.md)-Methoden in der Store-Analyse-API zum programmgesteuerten Abrufen der Bewertungen und Prüfungen von Ihren Kunden im JSON-Format verwenden.

## <a name="related-topics"></a>Verwandte Themen

* [Anforderungen an den Speicher senden](send-requests-to-the-store.md)
* [Starten der Microsoft Store-App](../launch-resume/launch-store-app.md)
* [Rezensionsbericht](../publish/reviews-report.md)
