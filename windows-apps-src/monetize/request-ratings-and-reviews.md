---
Description: Erfahren Sie mehr über mehrere Methoden können Sie programmgesteuert Kunden zu bewerten, und überprüfen Ihre app aktivieren.
title: Anfordern von Bewertungen und Prüfungen für Ihre App
ms.date: 01/22/2019
ms.topic: article
keywords: Windows 10, UWP, Bewertungen, Rezensionen
ms.localizationpriority: medium
ms.openlocfilehash: b167f4cc40ee72e6405436bacee28f2f20b4623c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601305"
---
# <a name="request-ratings-and-reviews-for-your-app"></a>Anfordern von Bewertungen und Prüfungen für Ihre App

Sie können Code zu Ihrer Universellen Windows-Plattform (UWP)-App hinzufügen, um Ihre Kunden programmgesteuert aufzufordern, Ihre App zu bewerten oder zu rezensieren. Hierfür stehen Ihnen mehrere Möglichkeiten zur Verfügung:
* Sie können ein Bewertungs- und Rezensionsdialogfeld direkt im Kontext Ihrer App anzeigen.
* Sie können programmgesteuert die Bewertungs- und Rezensionsseite für Ihre App im Microsoft Store öffnen.

Wenn Sie Ihre Bewertungen und Rezensionen Daten analysieren möchten, können Sie Anzeigen der Daten im Partner Center oder verwenden den Microsoft Store-Textanalyse-API, um diese Daten programmgesteuert abzurufen.

> [!IMPORTANT]
> Wenn Sie eine Bewertung-Funktion in Ihrer app hinzufügen, müssen alle Bewertungen der Benutzer auf die Store Bewertung die Mechanismen, unabhängig von Star-Bewertung ausgewählten senden. Wenn Sie Feedback oder Kommentare von Benutzern sammeln, muss offensichtlich sein, dass er sich nicht auf die app-Bewertung oder zugriffsüberprüfungen in den Store bezieht, sondern direkt an den app-Entwickler gesendet. Weitere Informationen zu verwandten finden Sie die Developer-Verhaltenskodex [Fraudulent oder unehrliche Aktivitäten](https://docs.microsoft.com/legal/windows/agreements/store-developer-code-of-conduct#3-fraudulent-or-dishonest-activities).

## <a name="show-a-rating-and-review-dialog-in-your-app"></a>Ein Bewertungs- und Rezensionsdialogfeld in Ihrer App anzeigen

Um programmgesteuert ein Dialogfeld aus Ihrer app anzuzeigen, die bittet Ihr Kunde, bewerten Ihre app und senden eine Überprüfung, rufen die [RequestRateAndReviewAppAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestrateandreviewappasync) -Methode in der die [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) Namespace. 

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

Die **RequestRateAndReviewAppAsync** Methode wurde in Windows 10, Version 1809, eingeführt und kann nur in Projekten, die als Ziel verwendet werden **Windows 10 Oktober 2018 Update (10.0; Erstellen Sie 17763)** oder eine neuere Version in Visual Studio.

### <a name="response-data-for-the-rating-and-review-request"></a>Antwortdaten für die Bewertungs- und Rezensionsanforderung

Nach dem Senden der Anforderung an die Bewertung angezeigt, und überprüfen im Dialogfeld die [ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storerateandreviewresult.extendedjsondata) Eigenschaft der [StoreRateAndReviewResult](https://docs.microsoft.com/uwp/api/windows.services.store.storerateandreviewresult) -Klasse enthält eine JSON-formatierte Zeichenfolge, der angibt, ob die Anforderung war erfolgreich.

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

| Feld          | Beschreibung                                                                                                                                   |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| *status*       | Eine Zeichenfolge, die angibt, ob der Kunde eine Bewertung oder Rezension erfolgreich gesendet hat. Die unterstützten Werte sind **success** und **aborted**. |
| *data*         | Ein Objekt, das nur einen booleschen Wert mit dem Namen *aktualisiert* enthält. Dieser Wert gibt an, ob der Kunde eine vorhandene Bewertung oder Rezension aktualisiert hat. Das *data*-Objekt ist nur in den Erfolgsantworten enthalten. |
| *errorDetails* | Eine Zeichenfolge, die Fehlerdetails für die Anforderung enthält.                                                                                     |

## <a name="launch-the-rating-and-review-page-for-your-app-in-the-store"></a>Die Bewertungs- und Rezensionsseite für Ihre App im Store starten

Wenn Sie programmgesteuert eine Bewertungs- und Rezensionsseite für Ihre App im Store öffnen möchten, können Sie die [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync)-Methode mit dem ```ms-windows-store://review```-URI-Schema verwenden, wie in diesem Codebeispiel gezeigt wird.

```csharp
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://review/?ProductId=9WZDNCRFHVJL"));
```

Weitere Informationen finden Sie unter [Die Microsoft Store App starten](../launch-resume/launch-store-app.md).

## <a name="analyze-your-ratings-and-reviews-data"></a>Ihre Bewertungs- und Rezensionsdaten analysieren

Sie haben mehrere Optionen, um die Bewertungs- und Rezensionsdaten von Ihren Kunden zu analysieren:
* Sie können die [überprüft](../publish/reviews-report.md) Bericht im Partner Center, um die Bewertungen und Rezensionen Ihrer Kunden finden Sie unter. Sie können diesen Bericht auch herunterladen, um ihn offline zu sehen.
* Sie können die [Abrufen von App-Bewertungen](get-app-ratings.md)- und [Abrufen von App-Rezensionen](get-app-reviews.md)-Methoden in der Store-Analyse-API zum programmgesteuerten Abrufen der Bewertungen und Prüfungen von Ihren Kunden im JSON-Format verwenden.

## <a name="related-topics"></a>Verwandte Themen

* [Senden Sie Anforderungen an den Store](send-requests-to-the-store.md)
* [Starten Sie die Microsoft Store-app](../launch-resume/launch-store-app.md)
* [Rezensionsbericht](../publish/reviews-report.md)
