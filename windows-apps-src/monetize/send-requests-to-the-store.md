---
author: mcleanbyron
Description: "Sie können mithilfe der SendRequestAsync-Methode Anforderungen an den Windows Store für Vorgänge senden, für die im Windows SDK noch keine API zur Verfügung steht."
title: Senden von Anforderungen an den Windows Store
ms.assetid: 070B9CA4-6D70-4116-9B18-FBF246716EF0
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, UWP, StoreRequestHelper, SendRequestAsync
ms.openlocfilehash: a949b3c93bb5b4a056f9e1fa4679e8ddca05d899
ms.sourcegitcommit: a9e4be98688b3a6125fd5dd126190fcfcd764f95
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/21/2017
---
# <a name="send-requests-to-the-windows-store"></a>Senden von Anforderungen an den Windows Store

Ab Windows10, Version 1607, bietet das Windows SDK im [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store)-Namespace APIs für Store-bezogene Vorgänge (z.B. In-App-Einkäufe). Obwohl die Dienste, die den Store unterstützen, zwischen den verschiedenen Betriebssystemversionen kontinuierlich aktualisiert, erweitert und verbessert werden, werden neue APIs in der Regel nur bei Hauptversionen des Betriebssystems zum Windows SDK hinzugefügt.

Unsere [SendRequestAsync](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreRequestHelper#Windows_Services_Store_StoreRequestHelper_SendRequestAsync_Windows_Services_Store_StoreContext_System_UInt32_System_String_)-Methode ist eine flexible Lösung zur Bereitstellung neuer Store-Vorgänge für UWP (Universelle Windows-Plattform)-Apps vor der Veröffentlichung einer neuen Windows SDK-Version. Sie können mithilfe dieser Methode Anforderungen an den Windows Store für Vorgänge senden, für die in der neuesten Windows SDK-Version noch keine entsprechende API zur Verfügung steht.

> [!NOTE]
> Die **SendRequestAsync**-Methode ist nur für Apps für Windows 10, Version 1607 oder höher verfügbar. Einige Anforderungen, die von dieser Methode unterstützt werden, werden nur in Versionen nach Windows10, Version 1607 unterstützt.

**SendRequestAsync** ist eine statische Methode der [StoreRequestHelper](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper)-Klasse. Um diese Methode aufzurufen, müssen Sie die folgenden Informationen an diese Methode übergeben:
* Ein [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext)-Objekt, das Informationen zum Benutzer bereitstellt, für den Sie den Vorgang ausführen möchten. Weitere Informationen zu diesem Objekt finden Sie unter [Erste Schrittemit der StoreContext-Klasse](in-app-purchases-and-trials.md#get-started-with-the-storecontext-class).
* Eine Ganzzahl, die die Anforderung identifiziert, die Sie an den Store senden möchten.
* Wenn die Anforderung keine Argumente unterstützt, können Sie auch eine Zeichenfolge im JSON-Format übergeben, die die Argumente enthält, die zusammen mit der Anforderung übergeben werden sollen.

Das folgende Beispiel veranschaulicht, wie diese Methode aufgerufen wird. In diesem Beispiel werden using-Anweisungen für die Namespaces **Windows.Services.Store** und **System.Threading.Tasks** benötigt.

```csharp
public async Task<bool> AddUserToFlightGroup()
{
    StoreSendRequestResult result = await StoreRequestHelper.SendRequestAsync(
        StoreContext.GetDefault(), 8,
        "{ \"type\": \"AddToFlightGroup\", \"parameters\": \"{ \"flightGroupId\": \"your group ID\" }\" }");

    if (result.ExtendedError == null)
    {
        return true;
    }

    return false;
}
```

Informationen zu den Anforderungen, die derzeit für die **SendRequestAsync**-Methode verfügbar sind, finden Sie in den folgenden Abschnitten. Dieser Artikel wird aktualisiert, wenn Unterstützung für neue Anforderungen hinzugefügt wird.

## <a name="requests-for-flight-group-scenarios"></a>Anforderungen für Test-Flight-Gruppenszenarien

> [!IMPORTANT]
> Alle in diesem Abschnittbeschriebenen Anforderungen für Test-Flight-Gruppen sind derzeit für die meisten Entwicklerkonten nicht verfügbar. Diese Anforderungen werden fehlschlagen, es sei denn, Ihr Entwicklerkonto wird speziell von Microsoft bereitgestellt.

Die **SendRequestAsync**-Methode unterstützt eine Reihe von Anforderungen für Test-Flight-Gruppenszenarien wie das Hinzufügen von Benutzern oder Geräten zu einer Test-Flight-Gruppe. Um diese Anforderungen zu senden, übergeben Sie den Wert7 oder8 an den *RequestKind*-Parameter und die Zeichenfolge im JSON-Format an den *ParametersAsJson*-Parameter, der die Anforderung angibt, die Sie zusammen mit allen zugehörigen Argumenten übermitteln möchten. Diese *RequestKind*-Werte unterscheiden sich folgendermaßen:

|  RequestKind-Wert  |  Beschreibung  |
|----------------------|---------------|
|  7                   |  Die Anforderungen werden im Kontext des aktuellen Geräts ausgeführt. Dieser Wert kann nur unter Windows10, Version 1703 oder höher verwendet werden.  |
|  8                   |  Die Anforderungen werden im Kontext des Benutzers ausgeführt, der aktuell beim Store angemeldet ist. Dieser Wert kann unter Windows10, Version 1607 oder höher verwendet werden.  |

Die folgenden Anforderungen für Test-Flight-Gruppen werden derzeit implementiert.

### <a name="retrieve-remote-variables-for-the-highest-ranked-flight-group"></a>Abrufen von Remotevariablen für Test-Flight-Gruppen mit dem höchsten Rang

> [!IMPORTANT]
> Diese Anforderung ist derzeit für die meisten Entwicklerkonten nicht verfügbar. Diese Anforderung wird fehlschlagen, es sei denn, Ihr Entwicklerkonto wird speziell von Microsoft bereitgestellt.

Diese Anforderung ruft die Remotevariablen für die Test-Flight-Gruppe mit dem höchsten Rang für den aktuellen Benutzer oder das aktuelle Gerät ab. Um diese Anforderung zu senden, übergeben Sie die folgenden Informationen an den *RequestKind*- und *ParametersAsJson*-Parameter der **SendRequestAsync**-Methode.

|  Parameter  |  Beschreibung  |
|----------------------|---------------|
|  *requestKind*                   |  Geben Sie die Ziffer7 ein, um die Test-Flight-Gruppe mit dem höchsten Rang für das Gerät zurückzugeben, oder geben Sie8 ein, um die Test-Flight-Gruppe mit dem höchsten Rang für den aktuellen Benutzer und das aktuelle Gerät zurückzugeben. Wir empfehlen die Verwendung des Werts8 für den *RequestKind*-Parameter, da dieser Wert die Test-Flight-Gruppe mit dem höchsten Rang innerhalb der Mitgliedschaft für den aktuellen Benutzer und das aktuelle Gerät zurückgibt.  |
|  *parametersAsJson*                   |  Übergeben Sie eine Zeichenfolge im JSON-Format, die die im folgenden Beispiel angezeigten Daten enthält.  |

Das folgende Beispiel zeigt das Format der JSON-Daten, die an den *ParametersAsJson*-Parameter übergeben werden sollen. Das Feld *type* muss der Zeichenfolge *GetRemoteVariables* zugewiesen werden. Weisen Sie das Feld *projectId* der ID des Projekts zu, bei dem Sie die Remotevariablen im Windows Dev Center-Dashboard definiert haben.

```json
{ 
    "type": "GetRemoteVariables", 
    "parameters": "{ \"projectId\": \"your project ID\" }" 
}
```

Nach Übermittlung dieser Anforderung enthält die [Response](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult#Windows_Services_Store_StoreSendRequestResult_Response)-Eigenschaft des [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult)-Rückgabewerts eine Zeichenfolge im JSON-Format mit den folgenden Feldern:

|  Feld  |  Beschreibung  |
|----------------------|---------------|
|  *anonymous*                   |  Ein boolescher Wert, wobei **true** angibt, dass die Identität des Benutzers oder Geräts in der Anforderung nicht vorhanden war; **false** bedeutet, dass die Identität des Benutzers oder Geräts in der Anforderung vorhanden war.  |
|  *name*                   |  Eine Zeichenfolge, die den Namen der Test-Flight-Gruppe mit dem höchsten Rang enthält, der das Gerät oder der Benutzer angehört.  |
|  *settings*                   |  Ein Wörterbuch mit Schlüssel-Wert-Paaren, die den Namen und den Wert der Remotevariablen enthalten, die der Entwickler für die Test-Flight-Gruppe konfiguriert hat.  |

Das folgende Beispiel zeigt einen Rückgabewert für diese Anforderung.

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

### <a name="add-the-current-device-or-user-to-a-flight-group"></a>Hinzufügen des aktuellen Geräts oder Benutzers zu einer Test-Flight-Gruppe

> [!IMPORTANT]
> Diese Anforderung ist derzeit für die meisten Entwicklerkonten nicht verfügbar. Diese Anforderung wird fehlschlagen, es sei denn, Ihr Entwicklerkonto wird speziell von Microsoft bereitgestellt.

Um diese Anforderung zu senden, übergeben Sie die folgenden Informationen an den *RequestKind*- und *ParametersAsJson*-Parameter der **SendRequestAsync**-Methode.

|  Parameter  |  Beschreibung  |
|----------------------|---------------|
|  *requestKind*                   |  Geben Sie den Wert7 ein, um das Gerät zu einer Test-Flight-Gruppe hinzufügen; geben Sie alternativ den Wert8 ein, um den Benutzer hinzuzufügen, der derzeit im Store einer Test-Flight-Gruppe angemeldet ist.  |
|  *parametersAsJson*                   |  Übergeben Sie eine Zeichenfolge im JSON-Format, die die im folgenden Beispiel angezeigten Daten enthält.  |

Das folgende Beispiel zeigt das Format der JSON-Daten, die an den *ParametersAsJson*-Parameter übergeben werden sollen. Das Feld *type* muss der Zeichenfolge *AddToFlightGroup* zugewiesen werden. Weisen Sie das Feld *FlightGroupId* der ID der Test-Flight-Gruppe zu, zu der Sie das Gerät oder den Benutzer hinzufügen möchten.

```json
{ 
    "type": "AddToFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

Wenn ein Fehler bei der Anforderung vorliegt, enthält die [HttpStatusCode](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult#Windows_Services_Store_StoreSendRequestResult_HttpStatusCode)-Eigenschaft des [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult)-Rückgabewerts den Antwortcode.

### <a name="remove-the-current-device-or-user-from-a-flight-group"></a>Entfernen des aktuellen Geräts oder Benutzers aus einer Test-Flight-Gruppe

> [!IMPORTANT]
> Diese Anforderung ist derzeit für die meisten Entwicklerkonten nicht verfügbar. Diese Anforderung wird fehlschlagen, es sei denn, Ihr Entwicklerkonto wird speziell von Microsoft bereitgestellt.

Um diese Anforderung zu senden, übergeben Sie die folgenden Informationen an die *RequestKind*- und *ParametersAsJson*-Parameter der **SendRequestAsync**-Methode.

|  Parameter  |  Beschreibung  |
|----------------------|---------------|
|  *requestKind*                   |  Geben Sie den Wert7 ein, um das Gerät aus einer Test-Flight-Gruppe zu entfernen; geben Sie alternativ den Wert8 ein, um den Benutzer zu entfernen, der derzeit im Store einer Test-Flight-Gruppe angemeldet ist.  |
|  *parametersAsJson*                   |  Übergeben Sie eine Zeichenfolge im JSON-Format, die die im folgenden Beispiel angezeigten Daten enthält.  |

Das folgende Beispiel zeigt das Format der JSON-Daten, die an den *ParametersAsJson*-Parameter übergeben werden sollen. Das Feld *type* muss der Zeichenfolge *RemoveFromFlightGroup* zugewiesen werden. Weisen Sie das Feld *flightGroupId* der ID der Test-Flight-Gruppe zu, aus der Sie das Gerät oder den Benutzer entfernen möchten.

```json
{ 
    "type": "RemoveFromFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

Wenn ein Fehler bei der Anforderung vorliegt, enthält die [HttpStatusCode](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult#Windows_Services_Store_StoreSendRequestResult_HttpStatusCode)-Eigenschaft des [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult)-Rückgabewerts den Antwortcode.

## <a name="related-topics"></a>Verwandte Themen

* [SendRequestAsync](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreRequestHelper#Windows_Services_Store_StoreRequestHelper_SendRequestAsync_Windows_Services_Store_StoreContext_System_UInt32_System_String_)
