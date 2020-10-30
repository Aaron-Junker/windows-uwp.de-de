---
description: Erfahren Sie mehr über verschiedene Möglichkeiten, wie Sie Kundenprogramm gesteuert ermöglichen können, Ihre APP zu bewerten und zu überprüfen.
title: Anfordern von Bewertungen und Überprüfungen für Ihre APP
ms.date: 01/22/2019
ms.topic: article
keywords: Windows 10, UWP, Bewertungen, Reviews
ms.localizationpriority: medium
ms.openlocfilehash: 9dbc33eaaf3adcb05a6ad37e2f54ceec4769f530
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034403"
---
# <a name="request-ratings-and-reviews-for-your-app"></a>Anfordern von Bewertungen und Überprüfungen für Ihre APP

Sie können Ihrer APP für universelle Windows-Plattform (UWP) Code hinzufügen, um Ihre Kundenprogramm gesteuert aufzufordern, Ihre APP zu bewerten oder zu überprüfen. Es gibt mehrere Möglichkeiten, dies zu erreichen:
* Sie können ein Dialogfeld für die Bewertung und Überprüfung direkt im Kontext ihrer App anzeigen.
* Sie können die Seite Bewertung und Überprüfung für Ihre APP Programm gesteuert in der Microsoft Store öffnen.

Wenn Sie bereit sind, Ihre Bewertungen zu analysieren und Daten zu überprüfen, können Sie die Daten im Partner Center anzeigen oder die Microsoft Store Analytics-API verwenden, um diese Daten Programm gesteuert abzurufen.

> [!IMPORTANT]
> Wenn Sie in Ihrer APP eine Bewertungsfunktion hinzufügen, müssen alle Überprüfungen den Benutzer unabhängig von der gewählten Star-Bewertung an die Bewertungsmechanismen des Stores senden. Wenn Sie Feedback oder Kommentare von Benutzern erfassen, muss klar sein, dass es nicht mit der APP-Bewertung oder Überprüfungen im Store verknüpft ist, sondern direkt an den App-Entwickler gesendet wird. Weitere Informationen im Zusammenhang mit [betrügerischen oder unehrlichen Aktivitäten](/legal/windows/agreements/store-developer-code-of-conduct#3-fraudulent-or-dishonest-activities)finden Sie im Entwickler-Verhaltenskodex.

## <a name="show-a-rating-and-review-dialog-in-your-app"></a>Anzeigen des Dialog Felds "Bewertung und Überprüfung" in Ihrer APP

Um Programm gesteuert einen Dialog in Ihrer APP anzuzeigen, der Ihren Kunden auffordert, Ihre APP zu bewerten und einen Review zu übermitteln, rufen Sie die [requestrateandreviewappasync](/uwp/api/windows.services.store.storecontext.requestrateandreviewappasync) -Methode im [Windows. Services. Store](/uwp/api/windows.services.store) -Namespace auf. 

> [!IMPORTANT]
> Die Anforderung zum Anzeigen des Dialog Felds Bewertung und Überprüfung muss im UI-Thread ihrer app aufgerufen werden.

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

### <a name="response-data-for-the-rating-and-review-request"></a>Antwortdaten für die Bewertungs-und Überprüfungs Anforderung

Nachdem Sie die Anforderung zum Anzeigen des Dialog Felds "Bewertung und Überprüfung" eingereicht haben, enthält die [extendedjsondata](/uwp/api/windows.services.store.storerateandreviewresult.extendedjsondata) -Eigenschaft der [storerateandreviewresult](/uwp/api/windows.services.store.storerateandreviewresult) -Klasse eine JSON-formatierte Zeichenfolge, die angibt, ob die Anforderung erfolgreich war.

Im folgenden Beispiel wird der Rückgabewert für diese Anforderung veranschaulicht, nachdem der Kunde eine Bewertung oder Überprüfung erfolgreich übermittelt hat.

```json
{ 
  "status": "success", 
  "data": {
    "updated": false
  },
  "errorDetails": "Success"
}
```

Im folgenden Beispiel wird der Rückgabewert für diese Anforderung veranschaulicht, nachdem der Kunde entschieden hat, keine Bewertung oder Überprüfung zu senden.

```json
{ 
  "status": "aborted", 
  "errorDetails": "Navigation was unsuccessful"
}
```

In der folgenden Tabelle werden die Felder in der JSON-formatierten Daten Zeichenfolge beschrieben.

| Feld          | BESCHREIBUNG                                                                                                                                   |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| *status*       | Eine Zeichenfolge, die angibt, ob der Kunde eine Bewertung oder Überprüfung erfolgreich übermittelt hat. Die unterstützten Werte sind **Erfolg** und werden **abgebrochen** . |
| *data*         | Ein-Objekt, das einen einzelnen booleschen Wert mit dem Namen " *aktualisiert* " enthält. Dieser Wert gibt an, ob der Kunde eine vorhandene Bewertung oder Überprüfung aktualisiert hat. Das *Daten* Objekt ist nur in Erfolgs Antworten enthalten. |
| *errorDetails* | Eine Zeichenfolge, die die Fehlerdetails für die Anforderung enthält.                                                                                     |

## <a name="launch-the-rating-and-review-page-for-your-app-in-the-store"></a>Starten Sie die Seite "Bewertung und Überprüfung" für Ihre APP im Store.

Wenn Sie die Seite "Bewertung und Überprüfung" für Ihre APP im Store Programm gesteuert öffnen möchten, können Sie die [launchuriasync](/uwp/api/windows.system.launcher.launchuriasync) -Methode mit dem URI-Schema verwenden, ```ms-windows-store://review``` wie in diesem Codebeispiel gezeigt.

```csharp
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://review/?ProductId=9WZDNCRFHVJL"));
```

Weitere Informationen finden Sie unter [Starten der Microsoft Store-App](../launch-resume/launch-store-app.md).

## <a name="analyze-your-ratings-and-reviews-data"></a>Analysieren von Bewertungen und Überprüfen von Daten

Zum Analysieren der Bewertungen und zum Überprüfen von Daten von ihren Kunden stehen Ihnen mehrere Optionen zur Verfügung:
* Sie können den Bericht [Reviews](../publish/reviews-report.md) im Partner Center verwenden, um die Bewertungen und Bewertungen ihrer Kunden anzuzeigen. Sie können diesen Bericht auch herunterladen, um ihn offline anzuzeigen.
* Sie können die Methoden " [Get App Ratings](get-app-ratings.md) " und " [Get App Reviews](get-app-reviews.md) " in der Store Analytics-API verwenden, um die Bewertungen und Reviews von ihren Kunden im JSON-Format Programm gesteuert abzurufen.

## <a name="related-topics"></a>Verwandte Themen

* [Senden von Anfragen an den Store](send-requests-to-the-store.md)
* [Starten der Microsoft Store-App](../launch-resume/launch-store-app.md)
* [Bericht „Rezensionen“](../publish/reviews-report.md)
