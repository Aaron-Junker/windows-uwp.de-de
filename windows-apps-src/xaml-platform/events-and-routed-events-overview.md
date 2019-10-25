---
description: Wir beschreiben das Programmier Konzept von Ereignissen in einer Windows-Runtime-APP, wenn C#Sie, Visual Basic oder C++ Visual Component ExtensionsC++(/CX) als Programmiersprache verwenden, und XAML für Ihre UI-Definition.
title: Übersicht über Ereignisse und Routing Ereignisse
ms.assetid: 34C219E8-3EFB-45BC-8BBD-6FD937698832
ms.date: 07/12/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 759e47348198feedbf7b1e3ee2c0bfc2da1da671
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690400"
---
# <a name="events-and-routed-events-overview"></a>Übersicht über Ereignisse und Routing Ereignisse

**Wichtige APIs**
- [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)
- [**RoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.RoutedEventArgs)

Wir beschreiben das Programmier Konzept von Ereignissen in einer Windows-Runtime-APP, wenn C#Sie, Visual Basic oder C++ Visual Component ExtensionsC++(/CX) als Programmiersprache verwenden, und XAML für Ihre UI-Definition. Sie können Handler für Ereignisse als Teil der Deklarationen für Benutzeroberflächen Elemente in XAML zuweisen, oder Sie können die Handler im Code hinzufügen. Windows-Runtime unterstützt Routing *Ereignisse*: bestimmte Eingabeereignisse und Daten Ereignisse können von Objekten über das Objekt, das das Ereignis ausgelöst hat, verarbeitet werden. Routing Ereignisse sind nützlich, wenn Sie Steuerelement Vorlagen definieren oder Seiten oder Layoutcontainer verwenden.

## <a name="events-as-a-programming-concept"></a>Ereignisse als Programmier Konzept

Im allgemeinen ähneln Ereignis Konzepte beim Programmieren einer Windows-Runtime-App dem Ereignis Modell in den meisten gängigen Programmiersprachen. Wenn Sie bereits wissen, wie Sie mit Microsoft .net C++ oder Ereignissen arbeiten, haben Sie einen Anfang. Sie müssen jedoch nicht wissen, dass es sich bei den Konzepten von Ereignis Modellen um einige grundlegende Aufgaben wie das Anfügen von Handlern handelt.

Wenn Sie, C#Visual Basic oder C++/CX als Programmiersprache verwenden, wird die Benutzeroberfläche in Markup (XAML) definiert. In der XAML-Markup Syntax sind einige der Prinzipien zum Verbinden von Ereignissen zwischen Markup Elementen und Lauf Zeit Code Entitäten mit anderen Webtechnologien vergleichbar, wie z. b. ASP.net oder HTML5.

**Beachten Sie**  der Code, der die Lauf Zeit Logik für eine XAML-definierte Benutzeroberfläche bereitstellt, häufig als *Code Behind* oder die Code-Behind-Datei bezeichnet wird. In den Microsoft Visual Studio projektmappenansichten wird diese Beziehung grafisch dargestellt, wobei die Code-Behind-Datei eine abhängige und eine gebildete Datei und die XAML-Seite ist, auf die Sie verweist.

## <a name="buttonclick-an-introduction-to-events-and-xaml"></a>Button. Click: eine Einführung in Ereignisse und XAML

Eine der gängigsten Programmieraufgaben für eine Windows-Runtime-APP ist das Erfassen von Benutzereingaben in der Benutzeroberfläche. Die Benutzeroberfläche kann z. b. eine Schaltfläche haben, auf die der Benutzer zum Übermitteln von Informationen oder zum Ändern des Zustands klicken muss.

Sie definieren die Benutzeroberfläche für Ihre Windows-Runtime-APP, indem Sie XAML erstellen. Dieser XAML-Code ist in der Regel die Ausgabe einer Entwurfs Oberfläche in Visual Studio. Sie können den XAML-Code auch in einem nur-Text-Editor oder in einem XAML-Editor von Drittanbietern schreiben. Beim Erstellen dieses XAML können Sie Ereignishandler für einzelne Benutzeroberflächen Elemente gleichzeitig übertragen, indem Sie alle anderen XAML-Attribute definieren, die Eigenschaftswerte für dieses Benutzeroberflächen Element festlegen.

Um die Ereignisse in XAML zu übertragen, geben Sie den Zeichen folgen Formular Namen der Handlermethode an, die Sie bereits definiert haben, oder definieren Sie später in Ihrem Code-Behind. Beispielsweise definiert dieses XAML ein [**Schalt**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) Flächen Objekt mit anderen Eigenschaften ([x:Name-Attribut](x-name-attribute.md), [**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.content)), die als Attribute zugewiesen sind, und einen Handler für das [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) -Ereignis der Schaltfläche, indem auf eine Methode mit dem Namen `ShowUpdatesButton_Click`verwiesen wird:

```xaml
<Button x:Name="showUpdatesButton"
  Content="{Binding ShowUpdatesText}"
  Click="ShowUpdatesButton_Click"/>
```

**Tip**-  *Ereignis Verkabelung* ist ein Programmier Begriff. Er bezieht sich auf den Prozess oder Code, mit dem angegeben wird, dass das Vorkommen eines Ereignisses eine benannte Handlermethode aufrufen sollte. In den meisten prozeduralen Code Modellen ist die Ereignis Verknüpfung impliziter oder expliziter "AddHandler"-Code, der das Ereignis und die Methode benennt und in der Regel eine Zielobjekt Instanz umfasst. In XAML ist "AddHandler" implizit, und die Ereignis Verkabelung besteht ausschließlich darin, das Ereignis als Attributnamen eines Objekt Elements zu benennen und den Handler als Wert dieses Attributs zu benennen.

Sie schreiben den eigentlichen Handler in der Programmiersprache, die Sie für den gesamten Code und Code Behind Ihrer APP verwenden. Wenn Sie das-Attribut `Click="ShowUpdatesButton_Click"`haben, haben Sie einen Vertrag erstellt, der beim Laden der XAML-Datei mit Markup Kompilierung und-Analyse sowohl mit dem XAML-Markup Kompilierungs Schritt in der Buildaktion Ihrer IDE als auch mit der letztlichen XAML-Analyse, wenn die App geladen wird, eine Methode mit dem Namen `ShowUpdatesButton_Click` als Teil der Co Liga. `ShowUpdatesButton_Click` muss eine Methode sein, die eine kompatible Methoden Signatur (basierend auf einem Delegaten) für einen beliebigen Handler des [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) -Ereignisses implementiert. Dieser Code definiert z. b. den `ShowUpdatesButton_Click`-Handler.

```csharp
private void ShowUpdatesButton_Click (object sender, RoutedEventArgs e) 
{
    Button b = sender as Button;
    //more logic to do here...
}
```

```vb
Private Sub ShowUpdatesButton_Click(ByVal sender As Object, ByVal e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    '  more logic to do here...
End Sub
```

```cppwinrt
void winrt::MyNamespace::implementation::BlankPage::ShowUpdatesButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e)
{
    auto b{ sender.as<Windows::UI::Xaml::Controls::Button>() };
    // More logic to do here.
}
```

```cpp
void MyNamespace::BlankPage::ShowUpdatesButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e) 
{
    Button^ b = (Button^) sender;
    //more logic to do here...
}
```

In diesem Beispiel basiert die `ShowUpdatesButton_Click`-Methode auf dem [**RoutedEventHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.routedeventhandler) -Delegaten. Sie wissen, dass dies der zu verwendende Delegat ist, da Sie den Delegaten in der Syntax für die [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) -Methode sehen.

**Tipp**  Visual Studio bietet eine bequeme Möglichkeit, den Ereignishandler zu benennen und die Handlermethode zu definieren, während Sie XAML bearbeiten. Wenn Sie den Attributnamen des Ereignisses im XAML-Text-Editor angeben, warten Sie einen Moment, bis eine Microsoft IntelliSense-Liste angezeigt wird. Wenn Sie in der Liste auf **&lt;neuen Ereignis Handler klicken&gt;** werden Microsoft Visual Studio einen Methodennamen vorschlagen, der auf dem **x:Name** (oder Typnamen) des Elements, dem Ereignis Namen und einem numerischen Suffix basiert. Sie können dann mit der rechten Maustaste auf den Namen des ausgewählten Ereignis Handlers klicken und dann **auf zu Ereignishandler navigieren**klicken. Dadurch navigieren Sie direkt zur neu eingefügten Ereignishandlerdefinition, wie Sie in der Code-Editor-Ansicht Ihrer Code-Behind-Datei für die XAML-Seite angezeigt werden. Der Ereignishandler verfügt bereits über die richtige Signatur, einschließlich des *Absender* Parameters und der Ereignisdaten Klasse, die vom Ereignis verwendet werden. Wenn eine Handlermethode mit der richtigen Signatur bereits im Code-Behind vorhanden ist, wird der Name der Methode in der Dropdown-Dropdown-Datei für die automatische Vervollständigung zusammen mit der Option " **&lt;neuer Ereignishandler&gt;** " angezeigt. Sie können die Tab-Taste auch als Verknüpfung drücken, anstatt auf die IntelliSense-Listenelemente zu klicken.

## <a name="defining-an-event-handler"></a>Definieren eines Ereignis Handlers

Für Objekte, bei denen es sich um Benutzeroberflächen Elemente handelt und die in XAML deklariert sind, wird Ereignishandlercode in der partiellen Klasse definiert, die als Code Behind für eine XAML-Seite fungiert. Ereignishandler sind Methoden, die Sie als Teil der partiellen Klasse schreiben, die mit XAML verknüpft ist. Diese Ereignishandler basieren auf den Delegaten, die von einem bestimmten Ereignis verwendet werden. Die Ereignishandlermethoden können öffentlich oder privat sein. Der Private Zugriff funktioniert, weil der Handler und die Instanz, die von XAML erstellt wurden, letztendlich durch die Codegenerierung verknüpft sind. Im Allgemeinen empfiehlt es sich, die Ereignishandlermethoden in der-Klasse als privat zu gestalten.

**Hinweis**  Ereignishandler für C++ nicht in partiellen Klassen definiert werden, werden Sie im Header als privates Klassenmember deklariert. Die Buildaktionen für C++ ein Projekt sorgen für das Erstellen von Code, der das XAML-Typsystem und das Code C++Behind-Modell für unterstützt.

### <a name="the-sender-parameter-and-event-data"></a>Der *Absender* Parameter und die Ereignisdaten.

Der Handler, den Sie für das-Ereignis schreiben, kann auf zwei Werte zugreifen, die als Eingabe für jeden Fall verfügbar sind, in dem der Handler aufgerufen wird. Der erste derartige Wert ist *Absender*, bei dem es sich um einen Verweis auf das Objekt handelt, an das der Handler angefügt ist. Der *Sender* -Parameter ist als Basis **Objekttyp** typisiert. Eine gängige Methode ist das Umwandeln des *Absenders* in einen präziseren Typ. Diese Technik ist hilfreich, wenn Sie den Status des *Absender* Objekts selbst überprüfen oder ändern möchten. Basierend auf Ihrem eigenen App-Entwurf kennen Sie in der Regel einen Typ, in den der *Absender* sicher umgewandelt werden kann, je nachdem, wo der Handler angefügt ist oder andere Entwurfs Besonderheiten.

Der zweite Wert sind Ereignisdaten, die im Allgemeinen in Syntax Definitionen als *e* -Parameter angezeigt werden. Sie können ermitteln, welche Eigenschaften für Ereignisdaten verfügbar sind, indem Sie sich den *e* -Parameter des Delegaten ansehen, der für das jeweilige Ereignis, das Sie behandeln, zugewiesen ist, und dann IntelliSense oder Objektkatalog in Visual Studio verwenden. Sie können auch die Windows-Runtime Referenz Dokumentation verwenden.

Für einige Ereignisse sind die spezifischen Eigenschaftswerte der Ereignisdaten so wichtig wie das wissen, dass das Ereignis aufgetreten ist. Dies gilt insbesondere für die Eingabeereignisse. Bei Zeiger Ereignissen kann es wichtig sein, dass die Position des Zeigers bei Auftreten des Ereignisses wichtig ist. Bei Tastatur Ereignissen lösen alle möglichen Tasten [**Kombinationen ein KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) -und [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) -Ereignis aus. Zum Ermitteln des Schlüssels, den ein Benutzer gedrückt hat, müssen Sie auf das [**keyroutedeventargs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs) zugreifen, das für den Ereignishandler verfügbar ist. Weitere Informationen zur Behandlung von Eingabe Ereignissen finden Sie unter [Tastatur Interaktionen](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions) und [Behandeln von Zeiger Eingaben](https://docs.microsoft.com/windows/uwp/input-and-devices/handle-pointer-input). Eingabeereignisse und Eingabe Szenarien haben häufig weitere Überlegungen, die in diesem Thema nicht behandelt werden, z. b. die Zeiger Erfassung für Zeiger Ereignisse und Modifizierertasten und Platt Form Schlüsselcodes für Tastatur Ereignisse.

### <a name="event-handlers-that-use-the-async-pattern"></a>Ereignishandler, die das **Async** -Muster verwenden

In einigen Fällen möchten Sie APIs verwenden, die ein **Async** -Muster in einem Ereignishandler verwenden. Beispielsweise können Sie eine [**Schaltfläche**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) in einer [**appbar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) verwenden, um eine Dateiauswahl anzuzeigen und damit zu interagieren. Viele der APIs für die Dateiauswahl sind jedoch asynchron. Sie müssen in einem **Async**-/awaitable-Bereich aufgerufen werden, und der Compiler erzwingt diesen Vorgang. Sie können also dem Ereignishandler das **Async** -Schlüsselwort hinzufügen, sodass der Handler jetzt **Async** **void**ist. Nun ist es dem Ereignishandler gestattet, **Async**-/awaitable-Aufrufe durchführen.

Ein Beispiel für die Ereignis Behandlung für die Benutzerinteraktion mit dem **Async** -Muster finden Sie unter [Dateizugriff und Picker](https://docs.microsoft.com/previous-versions/windows/apps/jj655411(v=win.10)) (Teil der[Erstellung Ihrer ersten Windows-Runtime C# -App mit oder Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/hh974581(v=win.10)) -Serie). Siehe auch [Aufrufe von asynchronen APIs in C).

## <a name="adding-event-handlers-in-code"></a>Hinzufügen von Ereignis Handlern im Code

XAML ist nicht die einzige Möglichkeit zum Zuweisen eines Ereignis Handlers zu einem Objekt. Zum Hinzufügen von Ereignis Handlern zu einem beliebigen Objekt im Code, einschließlich der Objekte, die in XAML nicht verwendbar sind, können Sie die sprachspezifische Syntax zum Hinzufügen von Ereignis Handlern verwenden.

In C#besteht die Syntax darin, den`+=`-Operator zu verwenden. Sie registrieren den Handler, indem Sie auf den Namen der Ereignishandlermethode auf der rechten Seite des Operators verweisen.

Wenn Sie Code zum Hinzufügen von Ereignis Handlern zu Objekten verwenden, die in der Lauf Zeit Benutzeroberfläche angezeigt werden, besteht eine gängige Vorgehensweise darin, solche Handler als Antwort auf ein Ereignis oder einen Rückruf für die Objekt Lebensdauer (z. b. [**Loaded**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded) oder [**OnApplyTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate)) hinzuzufügen, damit die Ereignishandler auf dem relevante Objekte sind für vom Benutzer initiierte Ereignisse zur Laufzeit bereit. Dieses Beispiel zeigt eine XAML-Gliederung der Seitenstruktur und stellt dann C# die Sprachsyntax zum Hinzufügen eines Ereignis Handlers zu einem Objekt bereit.

```xaml
<Grid x:Name="LayoutRoot" Loaded="LayoutRoot_Loaded">
  <StackPanel>
    <TextBlock Name="textBlock1">Put the pointer over this text</TextBlock>
...
  </StackPanel>
</Grid>
```

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += textBlock1_PointerEntered;
    textBlock1.PointerExited += textBlock1_PointerExited;
}
```

**Beachten Sie**  eine ausführlichere Syntax vorhanden ist. In 2005 wurde C# eine Funktion mit dem Namen delegatinferenz hinzugefügt, mit der ein Compiler die neue Delegatinstanz ableiten und die vorherige, einfachere Syntax aktivieren kann. Die ausführliche Syntax ist funktional identisch mit dem vorherigen Beispiel, erstellt jedoch explizit eine neue Delegatinstanz vor der Registrierung und nutzt somit nicht den Vorteil des delegatinferenz. Diese explizite Syntax ist weniger häufig, kann aber in einigen Codebeispielen weiterhin angezeigt werden.

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += new PointerEventHandler(textBlock1_PointerEntered);
    textBlock1.PointerExited += new MouseEventHandler(textBlock1_PointerExited);
}
```

Es gibt zwei Möglichkeiten für die Visual Basic-Syntax. Eine besteht darin, die C# Syntax parallel anzufügen und Handler direkt an-Instanzen anzufügen. Hierfür ist das **AddHandler** -Schlüsselwort und der **AddressOf** -Operator erforderlich, der den Namen der Handlermethode dereferenziert.

Die andere Option für die Visual Basic Syntax besteht darin, das **Handles** -Schlüsselwort für Ereignishandler zu verwenden. Dieses Verfahren eignet sich für Fälle, in denen für Objekte zur Ladezeit voraussichtlich ein Handler vorhanden ist, die während der gesamten Objekt Lebensdauer beibehalten werden. Die Verwendung von **Handles** für ein Objekt, das in XAML definiert ist, erfordert, dass Sie einen **Namen** / **x:Name**angeben. Dieser Name wird zum Instanzqualifizierer, der für den *instance. Event* -Teil der **Handles** -Syntax benötigt wird. In diesem Fall benötigen Sie keinen Ereignishandler auf Objekt Lebensdauer, um das Anfügen der anderen Ereignishandler zu initiieren. die **Handles** werden bei der Kompilierung der XAML-Seite erstellt.

```vb
Private Sub textBlock1_PointerEntered(ByVal sender As Object, ByVal e As PointerRoutedEventArgs) Handles textBlock1.PointerEntered
' ...
End Sub
```

**Beachten Sie**  Visual Studio und seine XAML-Entwurfs Oberfläche in der Regel das instanzbehandlungsverfahren anstelle des **Handles** -Schlüssel Worts herauf Stufen. Dies liegt daran, dass das Einrichten der ereignishandlerverkabelung in XAML Teil des typischen Designer-Developer-Workflows ist und das **Handles** -Schlüsselwort Verfahren nicht mit dem Verknüpfen der Ereignishandler in XAML kompatibel ist.

In C++/CX verwenden Sie auch die **+=** Syntax, aber es gibt Unterschiede in der Grund C# Legenden Form:

- Es ist kein delegatinferenz vorhanden, deshalb müssen Sie für die Delegatinstanz **ref New** verwenden.
- Der Delegatkonstruktor verfügt über zwei Parameter und erfordert das Zielobjekt als ersten Parameter. Normalerweise geben Sie **diese**Angabe an.
- Der Delegatkonstruktor erfordert die Methoden Adresse als zweiten Parameter, sodass der **&** Reference-Operator dem Methodennamen vorangestellt wird.

```cppwinrt
textBlock1().PointerEntered({this, &MainPage::TextBlock1_PointerEntered });
```

```cpp
textBlock1->PointerEntered += 
ref new PointerEventHandler(this, &BlankPage::textBlock1_PointerEntered);
```

### <a name="removing-event-handlers-in-code"></a>Entfernen von Ereignis Handlern im Code

Es ist in der Regel nicht erforderlich, Ereignishandler im Code zu entfernen, auch wenn Sie Sie im Code hinzugefügt haben. Das Verhalten der Objekt Lebensdauer für die meisten Windows-Runtime Objekte (z. b. Seiten und Steuerelemente) zerstört die Objekte, wenn Sie vom Haupt [**Fenster**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) und der zugehörigen visuellen Struktur getrennt sind, und alle delegatverweise werden ebenfalls zerstört. .NET führt dies durch Garbage Collection und Windows-Runtime mit C++/CX verwendet schwache Verweise standardmäßig.

Es gibt einige seltene Fälle, in denen Sie Ereignishandler explizit entfernen möchten. Diese umfassen:

- Handler, die Sie für statische Ereignisse hinzugefügt haben, sodass die Garbage Collection nicht auf herkömmliche Weise durchgeführt werden kann. Beispiele für statische Ereignisse in der Windows-Runtime-API sind die Ereignisse der Klassen " [**CompositionTarget**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CompositionTarget) " und " [**Clipboard**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.Clipboard) ".
- Testen Sie den Code, in dem die zeitliche Steuerung des Handler direkt erfolgen soll, oder den Code, in dem Sie alte bzw. neue Ereignishandler für ein Ereignis zur Laufzeit austauschen möchten.
- Die Implementierung einer benutzerdefinierten **Remove** -Zugriffsmethode.
- Benutzerdefinierte statische Ereignisse.
- Handler für Seitennavigation.

" [**FrameworkElement. entladen**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.unloaded) " oder " [**Page. navigatedfrom**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom) " sind mögliche Ereignis Trigger, die geeignete Positionen in der Zustands Verwaltung und der Objekt Lebensdauer aufweisen, sodass Sie diese zum Entfernen von Handlern für andere Ereignisse verwenden können.

Beispielsweise können Sie einen Ereignishandler mit dem Namen **textBlock1\_pointereingegeben** aus dem Zielobjekt **textBlock1** mithilfe dieses Codes entfernen.

```csharp
textBlock1.PointerEntered -= textBlock1_PointerEntered;
```

```vb
RemoveHandler textBlock1.PointerEntered, AddressOf textBlock1_PointerEntered
```

Sie können Handler auch für Fälle entfernen, in denen das Ereignis über ein XAML-Attribut hinzugefügt wurde. Dies bedeutet, dass der Handler in generiertem Code hinzugefügt wurde. Dies ist einfacher, wenn Sie einen **namens** Wert für das Element bereitgestellt haben, an das der Handler angefügt wurde, da dies einen Objekt Verweis für Code später bereitstellt. Sie können jedoch auch die Objektstruktur durchlaufen, um den erforderlichen Objekt Verweis in Fällen zu finden, in denen das Objekt keinen **Namen**hat.

Wenn Sie einen Ereignishandler in C++/CX entfernen müssen, benötigen Sie ein Registrierungs Token, das Sie vom Rückgabewert der`+=`-ereignishandlerregistrierung erhalten sollten. Dies liegt daran, dass der Wert, den Sie für die Rechte Seite der `-=` Registrierung in der C++/CX-Syntax verwenden, das Token und nicht der Methodenname ist. Bei C++/CX können Sie keine Handler entfernen, die als XAML-Attribut hinzugefügt wurden, C++da der von/CX generierte Code kein Token speichert.

## <a name="routed-events"></a>Routing Ereignisse

Der Windows-Runtime mit C#, Microsoft Visual Basic oder C++/CX unterstützt das Konzept eines Routing Ereignisses für eine Reihe von Ereignissen, die für die meisten Benutzeroberflächen Elemente vorhanden sind. Diese Ereignisse gelten für Eingabe-und Benutzer Interaktions Szenarios und werden auf der [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) -Basisklasse implementiert. Im folgenden finden Sie eine Liste der Eingabeereignisse, die geroutete Ereignisse sind:

- [**Bringingeviewangeforderten**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)
- [**Charakteristik empfangen**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.characterreceived)
- [**Contextabgeb Rochen**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextcanceled)
- [**Contextrequessiert**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextrequested)
- [**Doppelt abgetippt**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.doubletapped)
- [**DragEnter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragenter)
- [**DragLeave**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragleave)
- [**DragOver**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragover)
- [**DragStart**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragstarting)
- [**Dropdown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.drop)
- [**Dropdown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dropcompleted)
- [**Gettingfocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gettingfocus)
- [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus)
- [**Gehalten**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.holding)
- [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown)
- [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup)
- [**Losingfocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.losingfocus)
- [**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus)
- [**ManipulationCompleted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)
- [**ManipulationDelta**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationdelta)
- [**ManipulationInertiaStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting)
- [**ManipulationStarted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarted)
- [**ManipulationStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarting)
- [**Nofocus candidatefound**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.nofocuscandidatefoundeventargs)
- [**Pointerabgeb Rochen**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercanceled)
- [**Pointercapturelost**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost)
- [**Pointereingetragen**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered)
- [**Pointerexited**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited)
- [**Pointerverschoben**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved)
- [**Pointerpressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed)
- [**Pointerreleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased)
- [**Pointerwheelchanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**PreviewKeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeydown.md)
- [**PreviewKeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeyup.md)
- [**Pointerwheelchanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**Righttippt**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.righttapped)
- [**Abgezweigten**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.tapped)

Ein Routing Ereignis ist ein Ereignis, das potenziell von einem untergeordneten Objekt an jedes der aufeinander folgenden übergeordneten Objekte in einer Objektstruktur weitergegeben (weiter*geleitet*) werden kann. Die XAML-Struktur Ihrer Benutzeroberfläche gleicht dieser Struktur, wobei der Stamm dieser Struktur das Stamm Element in XAML ist. Die wahre Objektstruktur kann etwas von der Schachtelung des XAML-Elements abweichen, da die Objektstruktur keine XAML-sprach Features wie z. b. Eigenschaften Element Tags enthält. Sie können das-Routing *Ereignis als* bubblinkt von einem untergeordneten XAML-Objekt Element vorstellen, das das-Ereignis auslöst, in Richtung des übergeordneten Object-Elements, in dem es enthalten ist. Das Ereignis und seine Ereignisdaten können auf mehreren Objekten entlang der Ereignis Route behandelt werden. Wenn kein Element über Handler verfügt, wird die Route potenziell fortlaufend fortfahren, bis das Stamm Element erreicht ist.

Wenn Sie Webtechnologien wie Dynamic HTML (DHTML) oder HTML5 kennen, sind Sie möglicherweise bereits *mit dem Konzept* des bubblindereignisses vertraut.

Wenn ein Routing Ereignis die Ereignis Route durchläuft, greifen alle angefügten Ereignishandler auf eine freigegebene Instanz von Ereignisdaten zu. Wenn eines der Ereignisdaten von einem Handler beschreibbar ist, werden alle an den Ereignisdaten vorgenommenen Änderungen an den nächsten Handler weitergegeben und können die ursprünglichen Ereignisdaten des Ereignisses nicht mehr darstellen. Wenn ein Ereignis ein Routing Ereignis Verhalten aufweist, enthält die Referenz Dokumentation Hinweise oder andere Anmerkungen zum Routing Verhalten.

### <a name="the-originalsource-property-of-routedeventargs"></a>Die **OriginalSource** -Eigenschaft von **routedebug args**

Wenn ein Ereignis eine Ereignis Route aufblasen, ist der *Absender* nicht mehr dasselbe Objekt wie das Ereignis erbende Objekt. Stattdessen ist *Absender* das Objekt, in dem der aufgerufene Handler angefügt wird.

In manchen Fällen ist der *Absender* nicht interessant, und Sie sind stattdessen an Informationen interessiert, z. b. welche der möglichen untergeordneten Objekte der Zeiger ist, wenn ein Zeiger Ereignis ausgelöst wird, oder welches Objekt in einer größeren Benutzeroberfläche den Fokus hatte, als ein Benutzer eine Tastatur Taste gedrückt hat. In diesen Fällen können Sie den Wert der [**OriginalSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.routedeventargs.originalsource) -Eigenschaft verwenden. An allen Punkten auf der Route meldet **OriginalSource** das ursprüngliche Objekt, das das Ereignis ausgelöst hat, anstelle des Objekts, an das der Handler angefügt ist. Bei [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) -Eingabe Ereignissen ist das ursprüngliche Objekt häufig ein Objekt, das in der XAML-Datei der Benutzeroberflächen Definition auf Seitenebene nicht sofort sichtbar ist. Stattdessen kann es sich bei dem ursprünglichen Quell Objekt um einen Vorlagen Bereich eines Steuer Elements handeln. Wenn der Benutzer beispielsweise den Mauszeiger über den ganz Rand einer [**Schaltfläche**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)bewegt, ist die **OriginalSource** für die meisten Zeiger [**Ereignisse ein Rahmen**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border) Vorlagen Teil in der [**Vorlage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.template), nicht die **Schaltfläche** selbst.

**Tip**  Eingabe Ereignis Blasen ist besonders nützlich, wenn Sie ein Steuerelement mit Vorlagen erstellen. Jedes Steuerelement, das über eine Vorlage verfügt, kann eine neue Vorlage haben, die von seinem Consumer angewendet wird. Der Consumer, der versucht, eine funktionierende Vorlage neu zu erstellen, kann versehentlich einige Ereignis Behandlung ausschließen, die in der Standardvorlage deklariert ist. Sie können die Ereignis Behandlung auf Steuerungsebene weiterhin durch Anfügen von Handlern als Teil der [**OnApplyTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate) -Überschreibung in der Klassendefinition bereitstellen. Anschließend können Sie die Eingabeereignisse abfangen, die für die Instanziierung in den Stamm des Steuer Elements Blasen.

### <a name="the-handled-property"></a>Die **behandelte** Eigenschaft

Mehrere Ereignisdaten Klassen für bestimmte Routing Ereignisse enthalten eine Eigenschaft mit dem Namen **behandelt**. Beispiele finden Sie unter [**pointerroutedebug. behandelte**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.handled), [**keyroutedebug-args. behandelte**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled), [**DragEventArgs. behandelt**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.drageventargs.handled). In allen Fällen ist **behandelt** eine festleg Bare boolesche Eigenschaft.

Das Festlegen der Eigenschaft **behandelt** auf **true** wirkt sich auf das Verhalten des Ereignis Systems aus. Wenn **"** **true**" ist, wird das Routing für die meisten Ereignishandler beendet. das Ereignis wird nicht fortgesetzt, um andere angefügte Handler dieses bestimmten Ereignis Falls zu benachrichtigen. Was "behandelt" bedeutet, dass im Kontext des Ereignisses und wie Ihre APP antwortet, ist für Sie von Bedeutung. Im Grunde ist **behandelt** ein einfaches Protokoll, das es dem App-Code ermöglicht, zu sagen, dass ein Ereignis nicht in einen Container hinein Blasen muss. Ihre APP-Logik übernimmt die erforderlichen Anforderungen. In der Regel müssen Sie jedoch darauf achten, dass Sie keine Ereignisse verarbeiten, die wahrscheinlich Blasen Weise sein sollten, damit das integrierte System oder Steuerelement Verhalten agieren kann. Beispielsweise kann das Verarbeiten von Ereignissen auf niedriger Ebene innerhalb der Teile oder Elemente eines Auswahl Steuer Elements schädlich sein. Das Auswahl Steuerelement sucht möglicherweise nach Eingabe Ereignissen, um zu wissen, dass sich die Auswahl ändern sollte.

Nicht alle Routing Ereignisse können eine Route auf diese Weise abbrechen, und Sie können erkennen, dass Sie über keine **behandelte** Eigenschaft verfügen. Beispielsweise ist " [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus) " und " [**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus) " eine Blase, aber Sie werden immer bis zum Stamm geblasen, und ihre Ereignisdaten Klassen verfügen über keine **behandelte** Eigenschaft, die dieses Verhalten beeinflussen kann.

##  <a name="input-event-handlers-in-controls"></a>Eingabe Ereignishandler in Steuerelementen

Bestimmte Windows-Runtime Steuerelemente verwenden manchmal das **behandelte** Konzept für Eingabeereignisse intern. Dies kann dazu führen, dass ein Eingabe Ereignis nie auftritt, da der Benutzercode ihn nicht verarbeiten kann. Beispielsweise enthält die [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) -Klasse Logik, die das allgemeine [**Eingabe Ereignis absichtlich**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed)behandelt. Dies liegt daran, dass Schaltflächen ein [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) -Ereignis auslösen, das durch Zeiger gesteuerte Eingaben initiiert wird, sowie durch andere Eingabemodi, wie z. b. die EINGABETASTE, mit der die Schaltfläche aufgerufen werden kann, wenn Sie fokussiert ist. Für den Entwurf der **Schaltfläche**der-Klasse wird das unformatierte Eingabe Ereignis konzeptionell behandelt, und Klassenconsumer, wie z. b. der Benutzercode, können stattdessen mit dem Steuerelement für ein **Klick** Ereignis interagieren. Themen für bestimmte Steuerelement Klassen in der Windows-Runtime-API-Referenz wird häufig das Ereignis Behandlungs Verhalten beachten, das von der-Klasse implementiert wird. In einigen Fällen können Sie das Verhalten ändern, indem Sie **auf**_Ereignis_ Methoden überschreiben. Beispielsweise können Sie ändern, wie die abgeleitete [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) -Klasse auf die Tasten Eingabe reagiert, indem Sie [**Control. OnKeyDown überschreiben**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.onkeydown).

##  <a name="registering-handlers-for-already-handled-routed-events"></a>Registrieren von Handlern für bereits behandelte Routing Ereignisse

Früher haben wir gesagt, dass die Einstellung für die Einstellung " **true** **" die meisten** Handler nicht aufzurufen. Die [**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler) -Methode stellt jedoch ein Verfahren bereit, mit dem Sie einen Handler anfügen können, der immer für die Route aufgerufen wird, selbst wenn ein anderer Handler, **der sich zuvor** in der Route befand, in den freigegebenen Ereignisdaten auf **true** festgelegt wurde. Dieses Verfahren ist nützlich, wenn ein von Ihnen verwendete Steuerelement das-Ereignis in der internen Zusammensetzung oder der Steuerungs spezifischen Logik behandelt hat. Sie möchten aber trotzdem von einer Steuerelement Instanz oder ihrer App-Benutzeroberfläche darauf antworten. Verwenden Sie dieses Verfahren jedoch mit Vorsicht, da es dem Zweck der Verarbeitung wider **sprechen und möglich** erweise die beabsichtigten Interaktionen eines Steuer Elements unterbrechen kann.

Nur die Routing Ereignisse, die über einen entsprechenden Routing Ereignis Bezeichner verfügen, können das Ereignis Behandlungsverfahren [**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler) verwenden, da der Bezeichner eine erforderliche Eingabe der **AddHandler** -Methode ist. Eine Liste der Ereignisse, für die Routing Ereignis Bezeichner verfügbar sind, finden Sie in der Referenz Dokumentation für [**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler) . Zum größten Teil ist dies die Liste der weiter oben gezeigten Routing Ereignisse. Die Ausnahme besteht darin, dass die beiden letzten in der Liste: [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus) und [**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus) keinen Routing Ereignis Bezeichner haben, sodass Sie **AddHandler** nicht für diese verwenden können.

## <a name="routed-events-outside-the-visual-tree"></a>Routing Ereignisse außerhalb der visuellen Struktur

Bestimmte Objekte nehmen an einer Beziehung mit der primären visuellen Struktur Teil, die konzeptionell eine Überlagerung der visuellen Hauptelemente aufweist. Diese Objekte sind nicht Teil der üblichen Beziehungen zwischen übergeordneten und untergeordneten Elementen, die alle Strukturelemente mit dem visuellen Stamm verbinden. Dies ist der Fall für jedes angezeigte [**Popup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup) [ **-oder**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToolTip)QuickInfo-Menü. Wenn Sie Routing Ereignisse von einem **Popup** oder einer QuickInfo verarbeiten möchten, platzieren Sie die Handler für bestimmte **UI-** **Elemente, die**sich innerhalb des **Popups** oder der QuickInfo befinden, nicht für das **Popup** -oder QuickInfo **-Element selbst** . Verlassen Sie sich nicht auf das Routing innerhalb von Zusammensetzung, das für **Popup** **-oder** QuickInfo-Inhalte ausgeführt wird. Dies liegt daran, dass das Ereignis Routing für Routing Ereignisse nur entlang der visuellen Hauptstruktur funktioniert. Ein **Popup** oder **eine** QuickInfo wird nicht als übergeordnetes Element von Elementen der Benutzeroberfläche angesehen und empfängt nie das Routing Ereignis, auch wenn es versucht, etwas ähnliches wie der **Popup** -Standard Hintergrund als Erfassungsbereich für Eingabeereignisse zu verwenden.

## <a name="hit-testing-and-input-events"></a>Treffer Tests und Eingabeereignisse

Die Bestimmung, ob und wo ein Element auf der Benutzeroberfläche sichtbar ist, wird als *Treffer Test*bezeichnet. Für Berührungs Aktionen und auch für Interaktions-oder Bearbeitungs Ereignisse, die Folgen einer Berührungs Aktion sind, muss ein Element als Ereignis Quelle für den Treffer Test sichtbar sein und das Ereignis auslösen, das der Aktion zugeordnet ist. Andernfalls übergibt die Aktion das-Element an alle zugrunde liegenden Elemente oder übergeordneten Elemente in der visuellen Struktur, die mit dieser Eingabe interagieren können. Es gibt mehrere Faktoren, die den Treffer Test beeinflussen, Sie können jedoch ermitteln, ob ein bestimmtes Element Eingabeereignisse auslösen kann, indem Sie dessen [**IsHitTestVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.ishittestvisible) -Eigenschaft überprüfen. Diese Eigenschaft gibt nur **dann true** zurück, wenn das Element diese Kriterien erfüllt:

- Der [**Visibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) -Eigenschafts Wert des Elements ist [**sichtbar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Visibility).
- Der Wert für den **Background** -oder **Fill** -Eigenschafts Wert des Elements ist nicht **null**. Ein **null** - [**Pinsel**](/uwp/api/Windows.UI.Xaml.Media.Brush) Wert führt zu Transparenz und Treffer Tests. (Um ein Element transparent zu machen, aber auch für testable zu erreichen, verwenden Sie einen [**transparenten**](https://docs.microsoft.com/uwp/api/windows.ui.colors.transparent) Pinsel anstelle von **null**.)

**Beachten Sie**, dass  **Background** und **Fill** nicht durch [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)definiert werden und stattdessen von verschiedenen abgeleiteten Klassen wie z. b. [**Steuer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control) Element und [**Form**](/uwp/api/Windows.UI.Xaml.Shapes.Shape)definiert werden. Die Auswirkungen von Pinsel, die Sie für die Vordergrund-und Hintergrund Eigenschaften verwenden, sind jedoch für Treffer Tests und Eingabeereignisse identisch, unabhängig davon, welche Unterklasse die Eigenschaften implementiert.

- Wenn das Element ein Steuerelement ist, muss sein [**isaktivierter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.isenabled) Eigenschafts Wert **true**lauten.
- Das-Element muss über tatsächliche Dimensionen im Layout verfügen. Ein Element, bei dem entweder [**ActualHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight) und [**ActualWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) 0 sind, werden keine Eingabeereignisse ausgelöst.

Einige Steuerelemente verfügen über spezielle Regeln für Treffer Tests. [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) hat z. b. keine **Background** -Eigenschaft, ist jedoch nach wie vor in dem gesamten Bereich der Dimensionen nach dem prüfbar. [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) -und [**Media Element**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) -Steuerelemente werden über die definierten Rechteck Dimensionen getestet, unabhängig von transparentem Inhalt, wie z. b. Alphakanal in der angezeigten Medien Quelldatei. [**WebView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) -Steuerelemente haben ein spezielles Treffer Testverhalten, da die Eingabe von den gehosteten HTML-und Fire Script-Ereignissen behandelt werden kann.

Die meisten [**Panel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Panel) - [**Klassen und-** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border) Rahmen sind im eigenen Hintergrund nicht als Treffer fähig, können aber weiterhin die Benutzereingabe Ereignisse verarbeiten, die von den darin enthaltenen Elementen weitergeleitet werden.

Sie können bestimmen, welche Elemente sich an derselben Position wie ein Benutzereingabe Ereignis befinden, unabhängig davon, ob die Elemente einen Treffer Test durchlaufen. Um dies zu erreichen, müssen Sie die Methode [**findelta ementsinhostkoordinaten**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates) aufrufen. Wie der Name schon sagt, findet diese Methode die Elemente an einer Position relativ zu einem angegebenen Host Element. Allerdings können angewendete Transformationen und Layoutänderungen das relative Koordinatensystem eines Elements anpassen und somit beeinflussen, welche Elemente an einer bestimmten Position gefunden werden.

## <a name="commanding"></a>Befehls

Eine kleine Anzahl von Elementen der Benutzeroberfläche unterstützt die *Befehls*Zeile. Die Befehlszeilenschnittstelle verwendet Eingabe bezogene Routing Ereignisse in der zugrunde liegenden Implementierung und ermöglicht die Verarbeitung verwandter Benutzeroberflächen Eingaben (eine bestimmte Zeiger Aktion, eine bestimmte Zugriffstaste) durch Aufrufen eines einzelnen Befehls Handlers. Wenn die Befehlszeile für ein UI-Element verfügbar ist, sollten Sie Ihre Befehls-APIs anstelle von diskreten Eingabe Ereignissen verwenden. In der Regel verwenden Sie einen **Bindungs** Verweis in Eigenschaften einer Klasse, die das Ansichts Modell für Daten definiert. Die Eigenschaften enthalten benannte Befehle, die das sprachspezifische **ICommand** -Befehls Muster implementieren. Weitere Informationen finden Sie unter [**ButtonBase. Command**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command).

## <a name="custom-events-in-the-windows-runtime"></a>Benutzerdefinierte Ereignisse in der Windows-Runtime

Zum Definieren von benutzerdefinierten Ereignissen hängt die Art und Weise, wie Sie das Ereignis hinzufügen, und was dies für den Klassen Entwurf bedeutet, stark von der verwendeten Programmiersprache ab.

- Für C# und Visual Basic Sie ein CLR-Ereignis definieren. Sie können das standardmäßige .NET-Ereignis Muster verwenden, solange Sie keine benutzerdefinierten Accessoren verwenden (**Add**/**Remove**). Weitere Tipps:
    - Für den Ereignishandler empfiehlt es sich, [**System. EventHandler<TEventArgs>** ](https://docs.microsoft.com/dotnet/api/system.eventhandler-1) zu verwenden, da es über eine integrierte Übersetzung in den Windows-Runtime generischen Ereignis Delegaten [**EventHandler<T>** ](https://docs.microsoft.com/uwp/api/windows.foundation.eventhandler)verfügt.
    - Basieren Sie nicht auf " [**System. EventArgs**](https://docs.microsoft.com/dotnet/api/system.eventargs) " der Ereignisdaten Klasse, da Sie nicht in die Windows-Runtime übersetzt wird. Verwenden Sie überhaupt eine vorhandene Ereignisdaten Klasse oder keine Basisklasse.
    - Wenn Sie benutzerdefinierte Accessoren verwenden, finden Sie weitere Informationen [unter benutzerdefinierte Ereignisse und Ereignisaccessoren in Windows-Runtime-Komponenten](https://docs.microsoft.com/previous-versions/windows/apps/hh972883(v=vs.140)).
    - Wenn Sie nicht klar sind, was das standardmäßige .NET-Ereignis Muster ist, finden Sie weitere Informationen unter [Definieren von Ereignissen für benutzerdefinierte Silverlight-Klassen](https://docs.microsoft.com/previous-versions/windows/). Dies wird für Microsoft Silverlight geschrieben, aber es ist immer noch eine gute Summe aus dem Code und den Konzepten für das standardmäßige .NET-Ereignis Muster.
- Informationen C++zu/CX finden Sie unter [Events (C++/CX)](https://docs.microsoft.com/cpp/cppcx/events-c-cx).
    - Verwenden Sie benannte Verweise auch für Ihre eigenen Verwendungen von benutzerdefinierten Ereignissen. Verwenden Sie Lambda nicht für benutzerdefinierte Ereignisse. es kann einen Zirkel Verweis erstellen.

Sie können kein benutzerdefiniertes Routing Ereignis für Windows-Runtime deklarieren. Routing Ereignisse sind auf den Satz beschränkt, der aus der Windows-Runtime stammt.

Die Definition eines benutzerdefinierten Ereignisses erfolgt in der Regel im Rahmen der Übung zum Definieren eines benutzerdefinierten Steuer Elements. Es ist ein gängiges Muster, eine Abhängigkeits Eigenschaft zu haben, die einen Rückruf für die Eigenschaften Änderung hat, und auch ein benutzerdefiniertes Ereignis zu definieren, das in einigen oder allen Fällen durch den Rückruf der Abhängigkeits Eigenschaft ausgelöst wird. Consumer Ihres Steuer Elements haben keinen Zugriff auf den von Ihnen definierten, von der Eigenschaft geänderten Rückruf, aber ein Benachrichtigungs Ereignis ist das nächste beste. Weitere Informationen finden Sie unter [benutzerdefinierte Abhängigkeits Eigenschaften](custom-dependency-properties.md).

## <a name="related-topics"></a>Verwandte Themen

* [Übersicht über XAML](xaml-overview.md)
* [Schnellstart: berühren von Eingaben](https://docs.microsoft.com/previous-versions/windows/apps/hh465387(v=win.10))
* [Tastatur Interaktionen](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions)
* [.Net-Ereignisse und-Delegaten](https://go.microsoft.com/fwlink/p/?linkid=214364)
* [Erstellen von Windows-Runtime-Komponenten](https://docs.microsoft.com/previous-versions/windows/apps/hh441572(v=vs.140))
* [**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler)
