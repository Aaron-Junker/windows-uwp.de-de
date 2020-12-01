---
description: Ein agiles Objekt ist ein Objekt, auf das von jedem Thread aus zugegriffen werden kann. C#/WinRT bietet Unterstützung für Agile-Verweise, wenn Sie ein nicht-Agile-Objekt auf sichere Weise über mehrere Apartments hinweg Mars Hallen müssen.
title: Agile-Objekte mit c#/WinRT
ms.date: 11/17/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a963f1a9918e6732028ad74903b096a38a14ec8f
ms.sourcegitcommit: 3b10880007fe9e29ea2b9305fe62ced239d974be
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2020
ms.locfileid: "96355231"
---
# <a name="agile-objects-in-cwinrt"></a>Agile-Objekte in c#/WinRT

Die meisten Windows-Runtime Klassen sind *agil*, d. h., auf Sie kann von allen Threads in unterschiedlichen Apartments zugegriffen werden C#-/WinRT Typen, die Sie erstellen, sind standardmäßig Agile, und es ist nicht möglich, dieses Verhalten für diese Typen zu entscheiden.

Die projizierten c#-/WinRT Typen (die Windows-Runtime Typen enthalten, die von der Windows SDK und der WinUI-Bibliothek bereitgestellt werden) können jedoch nicht agil sein. Viele Typen, die UI-Objekte darstellen, sind z. b. nicht Agile. Wenn Sie nicht Agile-Typen nutzen, müssen Sie das Threading Modell und das Marshallingverhalten berücksichtigen. C#/WinRT bietet Unterstützung für Agile-Verweise, wenn Sie ein nicht-Agile-Objekt auf sichere Weise über mehrere Apartments hinweg Mars Hallen müssen.

> [!NOTE]
> Windows-Runtime basiert auf COM. Im Sinne von COM wird eine agile Klasse mit `ThreadingModel` = *Both* registriert. Weitere Informationen zu COM-Threadingmodellen finden Sie unter [Verstehen und Verwenden von COM-Threadingmodellen](/previous-versions/ms809971(v=msdn.10)).

## <a name="check-for-agile-support"></a>Auf Agile-Unterstützung überprüfen

Um zu überprüfen, ob ein Windows-Runtime Objekt Agile ist, verwenden Sie den folgenden Code, um zu bestimmen, ob das Objekt die Schnittstelle [iagileobject](/windows/desktop/api/objidl/nn-objidl-iagileobject) unterstützt.

```csharp
var queryAgileObject = testObject.As<IAgileObject>();

if (queryAgileObject != null) {
    // testObject is agile.
}
```

## <a name="create-an-agile-reference"></a>Erstellen eines Agile-Verweises

Zum Erstellen eines Agile-Verweises für ein nicht-Agile-Objekt können Sie die- `AsAgile` Erweiterungsmethode verwenden. `AsAgile` ist eine generische Erweiterungsmethode, die auf jeden projizierten c#-/WinRT-Typ angewendet werden kann. Wenn der Typ kein projizierter Typ ist, wird eine Ausnahme ausgelöst. Hier ist ein Beispiel für die Verwendung eines [PopupMenu](/uwp/api/Windows.UI.Popups.PopupMenu) -Objekts, bei dem es sich um einen nicht agilen Typ aus dem Windows SDK handelt.

```csharp
var nonAgileObj = new Windows.UI.Popups.PopupMenu();
AgileReference<Windows.UI.Popups.PopupMenu> agileReference = nonAgileObj.AsAgile();
```

Sie können jetzt `agileReference` an einen Thread in einem anderen Apartment übergeben und dort verwenden.

```csharp
await Task.Run(() => {
        Windows.UI.Popups.PopupMenu nonAgileObjAgain = agileReference.Get()
    });
```
