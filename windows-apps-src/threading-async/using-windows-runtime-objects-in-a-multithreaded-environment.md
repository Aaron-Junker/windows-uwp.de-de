---
title: Verwenden von Windows-Runtime-Objekten in einer Multithread-Umgebung | Microsoft Docs
description: In diesem Artikel wird beschrieben, wie das .NET Framework Aufrufe von C#- und Visual Basic-Code auf Objekte verarbeitet, die von der Windows-Runtime oder von Komponenten für Windows-Runtime bereitgestellt werden.
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 43ffd28c-c4df-405c-bf5c-29c94e0d142b
keywords: Windows 10, UWP, Timer, Threads
ms.localizationpriority: medium
ms.openlocfilehash: f11207a774b1ffcebde95e316634592020e6ed49
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631215"
---
# <a name="using-windows-runtime-objects-in-a-multithreaded-environment"></a>Verwenden von Windows-Runtime-Objekten in einer Multithread-Umgebung
In diesem Artikel wird beschrieben, wie das .NET Framework Aufrufe von C#- und Visual Basic-Code auf Objekte verarbeitet, die von der Windows-Runtime oder von Komponenten für Windows-Runtime bereitgestellt werden.

Im .NET Framework können Sie standardmäßig ohne besondere Verarbeitung über mehrere Threads auf alle Objekte zugreifen. Sie benötigen lediglich einen Verweis auf das Objekt. Solche Objekte werden in der Windows-Runtime als *agil* bezeichnet. Die meisten Windows-Runtime-Klassen sind agil, einige jedoch nicht, und selbst agile Klassen erfordern möglicherweise eine besondere Verarbeitung.

Wann immer möglich, behandelt die Common Language Runtime (CLR) Objekte aus anderen Quellen, wie beispielsweise der Windows-Runtime, so, als wären sie .NET Framework-Objekte:

- Wenn das Objekt die Schnittstelle [IAgileObject](https://msdn.microsoft.com/library/Hh802476.aspx) implementiert oder das Attribut [MarshalingBehaviorAttribute](https://go.microsoft.com/fwlink/p/?LinkId=256022) mit [MarshalingType.Agile](https://go.microsoft.com/fwlink/p/?LinkId=256023) aufweist, wird es von der CLR so behandelt, als wäre es agil.

- Wenn CLR einen Aufruf über den Thread marshallen kann, in dem er zum Threadingkontext des Zielobjekts gemacht wurde, geschieht dies transparent.

- Wenn das Objekt über das Attribut [MarshalingBehaviorAttribute](https://go.microsoft.com/fwlink/p/?LinkId=256022) mit [MarshalingType.None](https://go.microsoft.com/fwlink/p/?LinkId=256023) verfügt, stellt die Klasse keine Marshalling-Informationen bereit. Die CLR kann den Aufruf nicht marshallen, daher wird eine [InvalidCastException](/dotnet/api/system.invalidcastexception)-Ausnahme mit der Meldung ausgelöst, dass das Objekt nur in dem Threadingkontext verwendet werden kann, in dem es erstellt wurde.

In den folgenden Abschnitten werden die Auswirkungen dieses Verhaltens auf Objekte aus verschiedenen Quellen beschrieben.

## <a name="objects-from-a-windows-runtime-component-that-is-written-in-c-or-visual-basic"></a>Objekte aus einer in C# oder Visual Basic geschriebenen Windows-Runtime-Komponente
Alle Typen in der Komponente, die aktiviert werden können, sind standardmäßig agil.

> [!NOTE]
>  Agilität bedeutet nicht, dass Threadsicherheit gegeben ist. Sowohl in der Windows-Runtime als auch im .NET Framework sind die meisten Klassen nicht threadsicher, da Threadsicherheit mit Leistungseinbußen verbunden ist und die meisten Objekte nie von mehreren Threads aufgerufen werden. Es ist effizienter, nur bei Bedarf den Zugriff auf einzelne Objekte zu synchronisieren (oder threadsichere Klassen zu verwenden).

Wenn Sie eine Windows-Runtime-Komponente erstellen, können Sie die Standardeinstellung überschreiben. Informationen hierzu finden Sie in den Abschnitten [ICustomQueryInterface](/dotnet/api/system.runtime.interopservices.icustomqueryinterface)-Schnittstelle und [IAgileObject](https://msdn.microsoft.com/library/Hh802476.aspx)-Schnittstelle.

## <a name="objects-from-the-windows-runtime"></a>Objekte aus der Windows-Runtime
Die meisten Klassen in der Windows-Runtime sind agil, und die CLR behandelt diese als agil. In der Dokumentation für diese Klassen wird „MarshalingBehaviorAttribute(Agile)” unter den Klassenattributen genannt. Die Elemente in einigen dieser agilen Klassen, wie beispielsweise XAML-Steuerelemente, lösen jedoch Ausnahmen aus, wenn sie nicht für den UI-Thread aufgerufen werden. Der folgende Code versucht beispielsweise, einen Hintergrundthread zu verwenden, um eine Eigenschaft der Schaltfläche festzulegen, auf die geklickt wurde. Die [Content](https://go.microsoft.com/fwlink/p/?LinkId=256025)-Eigenschaft der Schaltfläche löst eine Ausnahme aus.

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

Ein sicherer Zugriff auf die Schaltfläche ist bei Verwendung ihrer [Dispatcher](https://go.microsoft.com/fwlink/p/?LinkId=256026)-Eigenschaft oder der `Dispatcher`-Eigenschaft eines beliebigen Objekts im Kontext des UI-Threads (beispielsweise die Seite, auf der sich die Schaltfläche befindet) möglich. Der folgende Code verwendet die Methode [RunAsync](https://go.microsoft.com/fwlink/p/?LinkId=256030) des [CoreDispatcher](https://go.microsoft.com/fwlink/p/?LinkId=256029)-Objekts, um den Aufruf für den UI-Thread zu verteilen.

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
>  Die `Dispatcher`-Eigenschaft löst keine Ausnahme aus, wenn sie von einem anderen Thread aufgerufen wird.

Die Lebensdauer eines Windows-Runtime-Objekts, das in dem UI-Thread erstellt wird, wird durch die Lebensdauer des Threads begrenzt. Versuchen Sie nicht, auf Objekte in einem UI-Thread zuzugreifen, nachdem das Fenster geschlossen wurde.

Wenn Sie ein eigenes Steuerelement erstellen, indem Sie ein XAML-Steuerelement übernehmen oder eine Reihe von XAML-Steuerelementen erstellen, ist Ihr Steuerelement agil, da es sich um ein .NET Framework-Objekt handelt. Wenn es jedoch Elemente seiner Basisklasse oder Bestandteilklassen aufruft oder wenn Sie übernommene Elemente aufrufen, lösen diese Elemente Ausnahmen aus, wenn sie von einem anderen Thread als dem UI-Thread aufgerufen werden.

### <a name="classes-that-cant-be-marshaled"></a>Klassen, die nicht gemarshallt werden kann
Windows-Runtime-Klassen, die keine Marshalling Informationen bereitstellen, weisen das Attribut [MarshalingBehaviorAttribute](https://go.microsoft.com/fwlink/p/?LinkId=256022) mit [MarshalingType.None](https://go.microsoft.com/fwlink/p/?LinkId=256023) auf. In der Dokumentation für solche Klassen wird „MarshalingBehaviorAttribute(None)” unter den Attributen genannt.

Der folgende Code erstellt ein [CameraCaptureUI](https://go.microsoft.com/fwlink/p/?LinkId=256027)-Objekt im UI-Thread und versucht anschließend, eine Eigenschaft für das Objekt aus einem Threadpoolthread festzulegen. Die CLR kann den Aufruf nicht marshallen und löst eine [System.InvalidCastException](/dotnet/api/system.invalidcastexception)-Ausnahme mit der Meldung aus, dass das Objekt nur in dem Threadingkontext verwendet werden kann, in dem es erstellt wurde.

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

In der Dokumentation für [CameraCaptureUI](https://go.microsoft.com/fwlink/p/?LinkId=256027) wird auch „ThreadingAttribute(STA)” unter den Attributen der Klasse genannt, da es in einem Singlethread-Kontext erstellt werden muss, wie beispielsweise dem UI-Thread.

Wenn Sie von einem anderen Thread auf das [CameraCaptureUI](https://go.microsoft.com/fwlink/p/?LinkId=256027)-Objekt zugreifen möchten, können Sie das [CoreDispatcher](https://go.microsoft.com/fwlink/p/?LinkId=256029)-Objekt für den UI-Thread zwischenspeichern und später verwenden, um den Aufruf für diesen Thread zu verteilen. Sie können den Dispatcher auch über ein XAML-Objekt wie beispielsweise die Seite abrufen, wie im folgenden Code dargestellt.

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

## <a name="objects-from-a-windows-runtime-component-that-is-written-in-c"></a>Objekte aus einer in C++ geschriebenen Windows-Runtime-Komponente
Standardmäßig sind die Klassen in der Komponente, die aktiviert werden kann, agil. C++ lässt jedoch eine umfassende Möglichkeit der Steuerung von Threadingmodellen und Marshallingverhalten zu. Wie weiter oben in diesem Artikel beschrieben, erkennt die CLR agile Klassen, sie versucht, Aufrufe zu marshallen, wenn Klassen nicht agil sind, und sie löst eine [System.InvalidCastException](/dotnet/api/system.invalidcastexception)-Ausnahme aus, wenn eine Klasse keine Marshalling-Informationen aufweist.

Für Objekte, die im UI-Thread ausgeführt werden und Ausnahmen auslösen, wenn sie von einem anderen Thread als dem UI-Thread aufgerufen werden, können Sie das [CoreDispatcher](https://go.microsoft.com/fwlink/p/?LinkId=256029)-Objekt des UI-Threads verwenden, um den Aufruf zu verteilen.

## <a name="see-also"></a>Weitere Informationen
[C#-Handbuch](/dotnet/csharp/)

[Leitfaden für Visual Basic](/dotnet/visual-basic/)
