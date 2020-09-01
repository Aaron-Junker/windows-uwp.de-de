---
title: Verwenden von Windows-Runtime Objekten in einer Multithread-Umgebung | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie die .NET Framework Aufrufe von c# und Visual Basic Code an Objekte behandelt, die vom Windows-Runtime oder von Windows-Runtime Komponenten bereitgestellt werden.
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 43ffd28c-c4df-405c-bf5c-29c94e0d142b
keywords: Windows 10, UWP, Timer, Threads
ms.localizationpriority: medium
ms.openlocfilehash: 4287ed5fd5659b7d39d52ada9e3958c592771d59
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164134"
---
# <a name="using-windows-runtime-objects-in-a-multithreaded-environment"></a>Verwenden von Windows-Runtime-Objekten in einer Multithread-Umgebung
In diesem Artikel wird erläutert, wie die .NET Framework Aufrufe von c# und Visual Basic Code an Objekte behandelt, die vom Windows-Runtime oder von Windows-Runtime Komponenten bereitgestellt werden.

In .NET Framework können Sie standardmäßig von mehreren Threads aus ohne besondere Maßnahmen auf beliebige Objekte zugreifen. Sie benötigen dazu lediglich einen Verweis auf das Objekt. In der Windows-Runtime werden solche Objekte als *Agile*bezeichnet. Die meisten Windows-Runtime Klassen sind agil, aber einige Klassen sind nicht, und sogar Agile-Klassen erfordern möglicherweise eine besondere Behandlung.

Nach Möglichkeit behandelt die Common Language Runtime (CLR) Objekte aus anderen Quellen, z. b. die Windows-Runtime, als ob Sie .NET Framework Objekte wären:

- Wenn das Objekt die [iagileobject](/windows/desktop/api/objidl/nn-objidl-iagileobject) -Schnittstelle implementiert oder das [marshalingverhalterattribute](/uwp/api/Windows.Foundation.Metadata.MarshalingBehaviorAttribute) -Attribut mit [marshalingtype. Agile](/uwp/api/Windows.Foundation.Metadata.MarshalingType)aufweist, behandelt die CLR es als Agile.

- Wenn die CLR einen Aufruf vom Thread, aus dem er stammt, zum Threadingkontext des Zielobjekts marshallen kann, tut sie das transparent.

- Wenn das-Objekt über das [marshalingverhalteattribute](/uwp/api/Windows.Foundation.Metadata.MarshalingBehaviorAttribute) -Attribut mit [marshalingtype. None](/uwp/api/Windows.Foundation.Metadata.MarshalingType)verfügt, stellt die-Klasse keine Marshallinginformationen bereit. Die CLR kann den-Befehl nicht Mars Hallen. Daher löst Sie eine [InvalidCastException](/dotnet/api/system.invalidcastexception) -Ausnahme mit einer Meldung aus, die angibt, dass das Objekt nur im Threading Kontext verwendet werden kann, in dem es erstellt wurde.

In den folgenden Abschnitten werden die Auswirkungen dieses Verhaltens auf Objekte aus verschiedenen Quellen beschrieben.

## <a name="objects-from-a-windows-runtime-component-that-is-written-in-c-or-visual-basic"></a>Objekte aus einer Windows-Runtime-Komponente, die in c# oder Visual Basic geschrieben ist
Alle aktivierbaren Typen in der Komponente sind standardmäßig agile.

> [!NOTE]
>  Agilität impliziert nicht zwangsläufig Threadsicherheit. Sowohl im Windows-Runtime als auch in der .NET Framework sind die meisten Klassen nicht Thread sicher, da die Thread Sicherheit eine Leistungssteigerung aufweist und die meisten Objekte nie von mehreren Threads aufgerufen werden. Es ist effizienter, den Zugriff auf einzelne Objekte nur bei Bedarf zu synchronisieren (oder in diesen Fällen threadsichere Klassen zu verwenden).

Wenn Sie eine Windows-Runtime Komponente erstellen, können Sie die Standardeinstellung überschreiben. Weitere Informationen finden Sie unter [ICustomQueryInterface](/dotnet/api/system.runtime.interopservices.icustomqueryinterface) -Schnittstelle und [iagileobject](/windows/desktop/api/objidl/nn-objidl-iagileobject) -Schnittstelle.

## <a name="objects-from-the-windows-runtime"></a>Objekte aus der Windows-Runtime
Die meisten Klassen in der Windows-Runtime sind Agile, und die CLR behandelt Sie als Agile. Die Dokumentation für diese Klassen listet „MarshalingBehaviorAttribute(Agile)“ bei den Klassenattributen auf. Die Member einiger dieser Agile-Klassen, wie etwa XAML-Steuerelemente, lösen jedoch Ausnahmen aus, wenn sie nicht im UI-Thread aufgerufen werden. Der folgende Code versucht beispielsweise, einen Hintergrundthread zu verwenden, um eine Eigenschaft der angeklickten Schaltfläche festzulegen. Die [Content](/uwp/api/Windows.UI.Xaml.Controls.ContentControl) -Eigenschaft der Schaltfläche löst eine Ausnahme aus.

```csharp
private async void Button_Click_2(object sender, RoutedEventArgs e)
{
    Button b = (Button) sender;
    await Task.Run(() => {
        b.Content += ".";
    });
}
```

```vb
Private Async Sub Button_Click_2(sender As Object, e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    Await Task.Run(Sub()
                       b.Content &= "."
                   End Sub)
End Sub
```

Sie können auf die Schaltfläche sicher zugreifen, indem Sie Ihre [Dispatcher](/uwp/api/Windows.UI.Xaml.DependencyObject) -Eigenschaft oder die- `Dispatcher` Eigenschaft eines beliebigen Objekts verwenden, das im Kontext des UI-Threads vorhanden ist (z. b. die Seite, auf der sich die Schaltfläche befindet). Im folgenden Code wird die [runasync](/uwp/api/Windows.UI.Core.CoreDispatcher) -Methode des [coredispatcher](/uwp/api/Windows.UI.Core.CoreDispatcher) -Objekts verwendet, um den-Befehl für den UI-Thread zu verteilen.

```csharp
private async void Button_Click_2(object sender, RoutedEventArgs e)
{
    Button b = (Button) sender;
    await b.Dispatcher.RunAsync(
        Windows.UI.Core.CoreDispatcherPriority.Normal,
        () => {
            b.Content += ".";
    });
}

```

```vb
Private Async Sub Button_Click_2(sender As Object, e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    Await b.Dispatcher.RunAsync(
        Windows.UI.Core.CoreDispatcherPriority.Normal,
        Sub()
            b.Content &= "."
        End Sub)
End Sub
```

> [!NOTE]
>  Die `Dispatcher` -Eigenschaft löst keine Ausnahme aus, wenn Sie von einem anderen Thread aufgerufen wird.

Die Lebensdauer eines Windows-Runtime Objekts, das im UI-Thread erstellt wird, hängt von der Lebensdauer des Threads ab. Versuchen Sie nicht, auf Objekte in einem UI-Thread zuzugreifen, nachdem das Fenster geschlossen wurde.

Wenn Sie ein eigenes Steuerelement durch Vererbung von einem XAML-Steuerelement oder durch Zusammenstellen einer Sammlung von XAML-Steuerelementen erstellen, ist Ihr Steuerelement agile, da es sich um ein .NET Framework-Objekt handelt. Wenn jedoch Member seiner Basisklasse oder konstituierender Klassen oder aber vererbte Member aufgerufen werden, lösen die betreffenden Member Ausnahmen aus, wenn sie von einem anderen als dem UI-Thread aufgerufen werden.

### <a name="classes-that-cant-be-marshaled"></a>Klassen, die nicht gemarshallt werden können
Windows-Runtime Klassen, die keine Marshallinginformationen bereitstellen, verfügen über das [marshalingverhalteattribute](/uwp/api/Windows.Foundation.Metadata.MarshalingBehaviorAttribute) -Attribut mit [marshalingtype. None](/uwp/api/Windows.Foundation.Metadata.MarshalingType). Die Dokumentation für derartige Klassen listet „MarshalingBehaviorAttribute(None)“ bei den Attributen auf.

Der folgende Code erstellt ein [cameracaptureui](/uwp/api/Windows.Media.Capture.CameraCaptureUI) -Objekt im UI-Thread und versucht dann, eine Eigenschaft des Objekts aus einem Thread Pool Thread festzulegen. Die CLR kann den-Befehl nicht Mars Hallen und löst eine [System. InvalidCastException](/dotnet/api/system.invalidcastexception) -Ausnahme mit einer Meldung aus, die angibt, dass das Objekt nur im Threading Kontext verwendet werden kann, in dem es erstellt wurde.

```csharp
Windows.Media.Capture.CameraCaptureUI ccui;

private async void Button_Click_1(object sender, RoutedEventArgs e)
{
    ccui = new Windows.Media.Capture.CameraCaptureUI();

    await Task.Run(() => {
        ccui.PhotoSettings.AllowCropping = true;
    });
}

```

```vb
Private ccui As Windows.Media.Capture.CameraCaptureUI

Private Async Sub Button_Click_1(sender As Object, e As RoutedEventArgs)
    ccui = New Windows.Media.Capture.CameraCaptureUI()

    Await Task.Run(Sub()
                       ccui.PhotoSettings.AllowCropping = True
                   End Sub)
End Sub
```

Die Dokumentation für [cameracaptureui](/uwp/api/Windows.Media.Capture.CameraCaptureUI) listet auch "threadingattribute (STA)" unter den Attributen der Klasse auf, da es in einem Single Thread-Kontext (z. b. dem UI-Thread) erstellt werden muss.

Wenn Sie von einem anderen Thread aus auf das [cameracaptureui](/uwp/api/Windows.Media.Capture.CameraCaptureUI) -Objekt zugreifen möchten, können Sie das [coredispatcher](/uwp/api/Windows.UI.Core.CoreDispatcher) -Objekt für den UI-Thread Zwischenspeichern und später verwenden, um den-Befehl für diesen Thread zu verteilen. Alternativ können Sie den Dispatcher eines XAML-Objekts abrufen, wie etwa der Seite, wie im folgenden Codebeispiel zu sehen.

```csharp
Windows.Media.Capture.CameraCaptureUI ccui;

private async void Button_Click_3(object sender, RoutedEventArgs e)
{
    ccui = new Windows.Media.Capture.CameraCaptureUI();

    await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () => {
            ccui.PhotoSettings.AllowCropping = true;
        });
}

```

```vb
Dim ccui As Windows.Media.Capture.CameraCaptureUI

Private Async Sub Button_Click_3(sender As Object, e As RoutedEventArgs)

    ccui = New Windows.Media.Capture.CameraCaptureUI()

    Await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                                Sub()
                                    ccui.PhotoSettings.AllowCropping = True
                                End Sub)
End Sub
```

## <a name="objects-from-a-windows-runtime-component-that-is-written-in-c"></a>Objekte aus einer Windows-Runtime-Komponente, die in C++ geschrieben ist
Standardmäßig sind alle aktivierbaren Klassen in der Komponente agile. C++ erlaubt jedoch die Steuerung der Threadingmodelle und des Marshallingverhaltens in erheblichem Umfang. Wie weiter oben in diesem Artikel beschrieben, erkennt die CLR Agile-Klassen, versucht, Aufrufe zu Mars Hallen, wenn Klassen nicht Agile sind, und löst eine [System. InvalidCastException](/dotnet/api/system.invalidcastexception) -Ausnahme aus, wenn eine Klasse keine Marshallinginformationen hat.

Bei Objekten, die im UI-Thread ausgeführt werden und Ausnahmen auslösen, wenn Sie von einem anderen Thread als dem UI-Thread aufgerufen werden, können Sie den Aufruf mit dem [coredispatcher](/uwp/api/Windows.UI.Core.CoreDispatcher) -Objekt des UI-Threads senden.

## <a name="see-also"></a>Weitere Informationen
[Leitfaden für C#](/dotnet/csharp/)

[Leitfaden für Visual Basic](/dotnet/visual-basic/)