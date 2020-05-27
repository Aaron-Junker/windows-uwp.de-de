---
Description: Sie können die sendrequestasync-Methode verwenden, um Anforderungen an die Microsoft Store für Vorgänge zu senden, für die noch keine API in der Windows SDK verfügbar ist.
title: Senden von Anforderungen an den Microsoft Store
ms.assetid: 070B9CA4-6D70-4116-9B18-FBF246716EF0
ms.date: 03/22/2018
ms.topic: article
keywords: Windows 10, UWP, storerequesthelper, sendrequestasync
ms.localizationpriority: medium
ms.openlocfilehash: 810c546eb0ee0263dcb50b3ce58e593ad294435c
ms.sourcegitcommit: 577a54d36145f91c8ade8e4509d4edddd8319137
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2020
ms.locfileid: "83867330"
---
# <a name="send-requests-to-the-microsoft-store"></a>Senden von Anforderungen an den Microsoft Store

Ab Windows 10, Version 1607, bietet das Windows SDK APIs für speicherbezogene Vorgänge (z. b. in-App-Käufe) im [Windows. Services. Store](https://docs.microsoft.com/uwp/api/windows.services.store) -Namespace. Obwohl die Dienste, die den Speicher unterstützen, ständig aktualisiert, erweitert und zwischen den Betriebssystemversionen verbessert werden, werden neue APIs in der Regel nur während der Hauptversionen des Betriebssystems dem Windows SDK hinzugefügt.

Wir stellen die [sendrequestasync](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper.sendrequestasync) -Methode als flexible Möglichkeit bereit, neue Speichervorgänge für universelle Windows-Plattform-Apps (UWP) verfügbar zu machen, bevor eine neue Version der Windows SDK veröffentlicht wird. Sie können diese Methode zum Senden von Anforderungen an den Store für neue Vorgänge verwenden, für die noch keine entsprechende API in der neuesten Version der Windows SDK verfügbar ist.

> [!NOTE]
> Die **sendrequestasync** -Methode ist nur für apps verfügbar, die auf Windows 10, Version 1607 oder höher ausgerichtet sind. Einige der Anforderungen, die von dieser Methode unterstützt werden, werden nur in Releases nach Windows 10, Version 1607, unterstützt.

**Sendrequestasync** ist eine statische Methode der [storerequesthelper](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper) -Klasse. Um diese Methode aufzurufen, müssen Sie die folgenden Informationen an die-Methode übergeben:
* Ein [storecontext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) -Objekt, das Informationen über den Benutzer bereitstellt, für den Sie den Vorgang ausführen möchten. Weitere Informationen zu diesem Objekt finden Sie unter [Get Started with the storecontext Class](in-app-purchases-and-trials.md#get-started-with-the-storecontext-class).
* Eine ganze Zahl, die die Anforderung identifiziert, die Sie an den Speicher senden möchten.
* Wenn die Anforderung Argumente unterstützt, können Sie auch eine JSON-formatierte Zeichenfolge übergeben, die die Argumente enthält, die zusammen mit der Anforderung übergeben werden.

Das folgende Beispiel veranschaulicht, wie die Methode aufgerufen wird. Dieses Beispiel erfordert die Verwendung von-Anweisungen für die Namespaces " **Windows. Services. Store** " und " **System. Threading. Tasks** ".

```csharp
public async Task<bool> AddUserToFlightGroup()
{
    StoreSendRequestResult result = await StoreRequestHelper.SendRequestAsync(
        StoreContext.GetDefault(), 8,
        "{ \"type\": \"AddToFlightGroup\", \"parameters\": { \"flightGroupId\": \"your group ID\" } }");

    if (result.ExtendedError == null)
    {
        return true;
    }

    return false;
}
```

In den folgenden Abschnitten finden Sie Informationen zu den Anforderungen, die derzeit über die **sendrequestasync** -Methode verfügbar sind. Wir aktualisieren diesen Artikel, wenn die Unterstützung für neue Anforderungen hinzugefügt wird.

## <a name="request-for-in-app-ratings-and-reviews"></a>Anfordern von in-App-Bewertungen und-Reviews

Sie können Programm gesteuert ein Dialogfeld von ihrer app starten, das Ihren Kunden auffordert, Ihre APP zu bewerten und eine Überprüfung zu übermitteln, indem Sie die ganze Zahl 16 der Anforderung an die **sendrequestasync** -Methode übergeben. Weitere Informationen finden Sie unter [Anzeigen eines Bewertungs-und Überprüfungs Dialogfelds in Ihrer APP](request-ratings-and-reviews.md#show-a-rating-and-review-dialog-in-your-app).

## <a name="requests-for-flight-group-scenarios"></a>Anforderungen für fluggruppen Szenarien

> [!IMPORTANT]
> Alle in diesem Abschnitt beschriebenen fluggruppen Anforderungen sind für die meisten Entwickler Konten zurzeit nicht verfügbar. Diese Anforderungen schlagen fehl, wenn Ihr Entwicklerkonto nicht speziell von Microsoft bereitgestellt wird.

Die **sendrequestasync** -Methode unterstützt eine Reihe von Anforderungen für fluggruppen Szenarien, wie z. b. das Hinzufügen eines Benutzers oder eines Geräts zu einer Fluggruppe. Um diese Anforderungen zu übermitteln, übergeben Sie den Wert 7 oder 8 an den *requestkind* -Parameter zusammen mit einer JSON-formatierten Zeichenfolge an den *parametersasjson* -Parameter, der die Anforderung angibt, die Sie zusammen mit den zugehörigen Argumenten übermitteln möchten. Diese *requestkind* -Werte unterscheiden sich wie folgt.

|  Anforderungs Kind Wert  |  BESCHREIBUNG  |
|----------------------|---------------|
|  7                   |  Die Anforderungen werden im Kontext des aktuellen Geräts ausgeführt. Dieser Wert kann nur unter Windows 10, Version 1703 oder höher, verwendet werden.  |
|  8                   |  Die Anforderungen werden im Kontext des Benutzers ausgeführt, der zurzeit beim Store angemeldet ist. Dieser Wert kann unter Windows 10, Version 1607 oder höher, verwendet werden.  |

Die folgenden fluggruppen Anforderungen sind zurzeit implementiert.

### <a name="retrieve-remote-variables-for-the-highest-ranked-flight-group"></a>Abrufen von Remote Variablen für die Fluggruppe mit der höchsten Rangfolge

> [!IMPORTANT]
> Diese Anforderung ist für die meisten Entwickler Konten zurzeit nicht verfügbar. Diese Anforderung schlägt fehl, es sei denn, Ihr Entwicklerkonto wird speziell von Microsoft bereitgestellt.

Mit dieser Anforderung werden die Remote Variablen für die Fluggruppe mit der höchsten Rangfolge für den aktuellen Benutzer oder das aktuelle Gerät abgerufen. Um diese Anforderung zu senden, übergeben Sie die folgenden Informationen an die Parameter " *requestkind* " und " *parametersasjson* " der **sendrequestasync** -Methode.

|  Parameter  |  BESCHREIBUNG  |
|----------------------|---------------|
|  *requestkind*                   |  Geben Sie 7 an, um die Fluggruppe mit der höchsten Rangfolge für das Gerät zurückzugeben, oder geben Sie 8 an, um die mit der höchsten Rangfolge für den aktuellen Benutzer und das Gerät zurückzugeben. Es wird empfohlen, den Wert 8 für den *requestkind* -Parameter zu verwenden, da dieser Wert für den aktuellen Benutzer und das Gerät die Gruppe mit der höchsten Rangfolge über die Mitgliedschaft zurückgibt.  |
|  *parametersasjson*                   |  Übergeben Sie eine JSON-formatierte Zeichenfolge, die die im folgenden Beispiel gezeigten Daten enthält.  |

Das folgende Beispiel zeigt das Format der JSON-Daten, die an *parametersasjson*übergeben werden. Das *typanfeld* muss der Zeichenfolge *getremotevariables*zugewiesen werden. Weisen Sie das Feld *ProjectId* der ID des Projekts zu, in dem Sie die Remote Variablen im Partner Center definiert haben.

```json
{ 
    "type": "GetRemoteVariables", 
    "parameters": "{ \"projectId\": \"your project ID\" }" 
}
```

Nachdem Sie diese Anforderung gesendet haben, enthält die [Response](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.Response) -Eigenschaft des [storesendrequestresult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) -Rückgabewerts eine JSON-formatierte Zeichenfolge mit den folgenden Feldern.

|  Feld  |  BESCHREIBUNG  |
|----------------------|---------------|
|  *Anonymous*                   |  Ein boolescher Wert, wobei **true** angibt, dass die Benutzer-oder Geräte Identität nicht in der Anforderung enthalten war, und **false** gibt an, dass die Benutzer-oder Geräte Identität in der Anforderung vorhanden war.  |
|  *name*                   |  Eine Zeichenfolge, die den Namen der Fluggruppe mit der höchsten Rangfolge enthält, zu der das Gerät oder der Benutzer gehört.  |
|  *settings*                   |  Ein Wörterbuch von Schlüssel-Wert-Paaren, das den Namen und den Wert der Remote Variablen enthält, die der Entwickler für die Fluggruppe konfiguriert hat.  |

Im folgenden Beispiel wird ein Rückgabewert für diese Anforderung veranschaulicht.

```json
{ 
  "anonymous": false, 
  "name": "Insider Slow",
  "settings":
  {
      "Audience": "Slow"
      ...
  }
}
```

### <a name="add-the-current-device-or-user-to-a-flight-group"></a>Hinzufügen des aktuellen Geräts oder Benutzers zu einer Fluggruppe

> [!IMPORTANT]
> Diese Anforderung ist für die meisten Entwickler Konten zurzeit nicht verfügbar. Diese Anforderung schlägt fehl, es sei denn, Ihr Entwicklerkonto wird speziell von Microsoft bereitgestellt.

Um diese Anforderung zu senden, übergeben Sie die folgenden Informationen an die Parameter " *requestkind* " und " *parametersasjson* " der **sendrequestasync** -Methode.

|  Parameter  |  BESCHREIBUNG  |
|----------------------|---------------|
|  *requestkind*                   |  Geben Sie 7 an, um das Gerät einer Fluggruppe hinzuzufügen, oder geben Sie 8 an, um den Benutzer, der zurzeit beim Store angemeldet ist, einer Fluggruppe hinzuzufügen.  |
|  *parametersasjson*                   |  Übergeben Sie eine JSON-formatierte Zeichenfolge, die die im folgenden Beispiel gezeigten Daten enthält.  |

Das folgende Beispiel zeigt das Format der JSON-Daten, die an *parametersasjson*übergeben werden. Das *typanfeld* muss der Zeichenfolge *adddeflightgroup*zugewiesen werden. Weisen Sie das Feld *flightgroupid* der ID der Fluggruppe zu, der Sie das Gerät oder den Benutzer hinzufügen möchten.

```json
{ 
    "type": "AddToFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

Wenn bei der Anforderung ein Fehler auftritt, enthält die [httpstatencode](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.HttpStatusCode) -Eigenschaft des Rückgabewerts [storesendrequestresult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) den Antwort Code.

### <a name="remove-the-current-device-or-user-from-a-flight-group"></a>Aktuelles Gerät oder Benutzer aus einer Fluggruppe entfernen

> [!IMPORTANT]
> Diese Anforderung ist für die meisten Entwickler Konten zurzeit nicht verfügbar. Diese Anforderung schlägt fehl, es sei denn, Ihr Entwicklerkonto wird speziell von Microsoft bereitgestellt.

Um diese Anforderung zu senden, übergeben Sie die folgenden Informationen an die Parameter " *requestkind* " und " *parametersasjson* " der **sendrequestasync** -Methode.

|  Parameter  |  BESCHREIBUNG  |
|----------------------|---------------|
|  *requestkind*                   |  Geben Sie 7 an, um das Gerät aus einer Fluggruppe zu entfernen, oder geben Sie 8 an, um den Benutzer zu entfernen, der derzeit bei dem Store aus einer Fluggruppe angemeldet ist.  |
|  *parametersasjson*                   |  Übergeben Sie eine JSON-formatierte Zeichenfolge, die die im folgenden Beispiel gezeigten Daten enthält.  |

Das folgende Beispiel zeigt das Format der JSON-Daten, die an *parametersasjson*übergeben werden. Das *typanfeld* muss der Zeichenfolge *removefromflightgroup*zugewiesen werden. Weisen Sie das Feld *flightgroupid* der ID der Fluggruppe zu, aus der Sie das Gerät oder den Benutzer entfernen möchten.

```json
{ 
    "type": "RemoveFromFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

Wenn bei der Anforderung ein Fehler auftritt, enthält die [httpstatencode](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.HttpStatusCode) -Eigenschaft des Rückgabewerts [storesendrequestresult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) den Antwort Code.

## <a name="related-topics"></a>Zugehörige Themen

* [Anzeigen des Dialog Felds "Bewertung und Überprüfung" in Ihrer APP](request-ratings-and-reviews.md#show-a-rating-and-review-dialog-in-your-app)
* [Sendrequestasync](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper.sendrequestasync)
