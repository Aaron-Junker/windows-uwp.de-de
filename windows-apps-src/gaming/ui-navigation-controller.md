---
title: Benutzeroberflächen-Navigationscontroller
description: Verwenden Sie die Windows.Gaming.Input-Benutzeroberflächen-Navigationscontroller-APIs zum Erkennen und Lesen verschiedener Arten von Eingabegeräten für die Navigation auf der Benutzeroberfläche.
ms.assetid: 5A14926D-8C2E-4DE8-AAFB-BEEB9BFE91A5
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Games, UI, Navigation
ms.localizationpriority: medium
ms.openlocfilehash: 7cf5369bd01fbcf95c5af7bddc7055958cc50299
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159084"
---
# <a name="ui-navigation-controller"></a>Benutzeroberflächen-Navigationscontroller

Diese Seite beschreibt die Grundlagen der Programmierung für Benutzeroberflächen-Navigationsgeräte mit [Windows.Gaming.Input.UINavigationController][uinavigationcontroller] und zugehörigen APIs für die Universelle Windows-Plattform (UWP).

Auf dieser Seite erhalten Sie Informationen zu folgenden Vorgängen:
* Erstellen einer Liste verbundener Benutzeroberflächen-Navigationsgeräte und deren Benutzer
* Erkennen, dass ein Navigationsgerät hinzugefügt oder entfernt wurde
* Lesen von Eingaben von einem oder mehreren Benutzeroberflächen-Navigationsgeräten
* Verhalten von Gamepads und Arcade-Joysticks als Navigationsgeräte

## <a name="ui-navigation-controller-overview"></a>Übersicht über Benutzeroberflächen-Navigationsgeräte

Fast alle Spiele verfügen neben dem Gameplay zumindest über etwas Benutzeroberfläche, auch wenn es sich dabei nur um Pregame-Menüs oder In-Game-Dialogfelder handelt. Die Spieler müssen auf dieser Benutzeroberfläche unabhängig vom gewählten Eingabegerät navigieren können. Daher müssen die Entwickler spezifische Unterstützung für alle Arten von Eingabegeräten hinzufügen und können so auch Inkonsistenzen zwischen Spielen und Eingabegeräten einführen, die die Spieler verwirren. Aus diesen Gründen wurde die [UINavigationController][]-API erstellt.

Benutzeroberflächen-Navigationscontroller sind _logische_ Eingabegeräte, die zur Bereitstellung eines Vokabulars gängiger Benutzeroberflächen-Navigationsbefehle dienen, die von einer Vielzahl von _physischen_ Eingabegeräten unterstützt werden können. Ein _Benutzeroberflächen-Navigationscontroller_ ist nur eine andere Betrachtungsweise für ein physisches Eingabegerät. Wir verwenden _Navigationsgerät_ für alle physischen Eingabegeräte, die als Navigationscontroller betrachtet werden. Durch Programmieren für ein Navigationsgerät statt für bestimmte Eingabegeräte verhindern die Entwickler das Problem, verschiedene Eingabegeräte unterstützen zu müssen, und erzielen standardmäßig Konsistenz.

Da die Anzahl und Vielzahl der von jedem Typ von Eingabegeräten unterstützten Steuerelemente enorm variieren kann und da bestimmte Eingabegeräte möglicherweise einen umfangreicheren Satz von Navigationsbefehlen unterstützen, teilt die Navigationscontrollerschnittstelle das Vokabular der Befehle in einen _erforderlichen Satz_, der die gängigsten und wesentlichen Befehle enthält, und einen _optionalen Satz_ ein, der praktische, aber nicht erforderliche Befehle enthält. Alle Navigationsgeräte unterstützen jeden Befehl im _erforderlichen Satz_ und unterstützen möglicherweise alle, einige oder keine Befehle im _optionalen Satz_.

### <a name="required-set"></a>Erforderlicher Satz

Navigationsgeräte müssen alle Navigationsbefehle im _erforderlichen Satz_ unterstützen. Dies sind die Richtungsbefehle (nach oben, unten, links und rechts), Ansicht, Menü, Akzeptieren und Abbrechen.

Die Richtungsbefehle sind für die primäre [XY-Fokusnavigation](../design/input/gamepad-and-remote-interactions.md#xy-focus-navigation-and-interaction) zwischen einzelnen Benutzeroberflächenelementen vorgesehen. Die Ansichts- und Menübefehle dienen zum Anzeigen von (kurzzeitigen oder modalen) Informationen zum Gameplay und zum Wechseln zwischen Gameplay und Menü. Die Befehle „Akzeptieren“ und „Abbrechen“ sind für zustimmende (Ja) und ablehnende (Nein) Antworten vorgesehen.

In der folgenden Tabelle werden diese Befehle und ihre beabsichtigte Nutzung mit Beispielen zusammengefasst.
| Get-Help | Beabsichtigte Verwendung
| -------:| ---------------
|      Nach oben | XY-Fokusnavigation nach oben
|    Nach unten | XY-Fokusnavigation nach unten
|    Left | XY-Fokusnavigation nach links
|   Right | XY-Fokusnavigation nach rechts
|    Sicht | Gameplay-Informationen anzeigen _(Anzeigetafel, Spielstatistik, Ziele, Welt- oder Bereichskarte)_
|    Menü | Hauptmenü/Pause _(Einstellungen, Status, Geräte, Bestand, Pause)_
|  Akzeptieren | Positive Antwort _(Akzeptieren, Weiter, Bestätigen, Starten, Ja)_
|  Abbrechen | Negative Antwort _(Ablehnen, Umkehren, Verweigern, Anhalten, Nein)_


### <a name="optional-set"></a>Optionaler Satz

Navigationsgeräte können alle, einige oder keine Navigationsbefehle im _optionalen Satz_ unterstützen. Hierbei handelt es sich um Paging- (nach oben, unten, links und rechts), Bildlauf- (nach oben, unten, links und rechts) und Kontextbefehle (Kontext 1 bis 4).

Die Kontextbefehle sind explizit für anwendungsspezifische Befehle und Tastenkombinationen für die Navigation vorgesehen. Die Paging- und Bildlaufbefehle sind für die schnelle Navigation zwischen Seiten oder Gruppen von Benutzeroberflächenelementen und für die differenzierte Navigation innerhalb von Benutzeroberflächenelementen vorgesehen.

In der folgenden Tabelle werden diese Befehle und ihre beabsichtigte Nutzung zusammengefasst.
|     Get-Help | Beabsichtigte Verwendung
| -----------:| ------------
|      BILD-AUF | Nach oben (zur oberen bzw. vorherigen vertikalen Seite oder Gruppe) wechseln
|    BILD-AB | Nach unten (zur unteren/nächsten vertikalen Seite oder Gruppe) wechseln
|    PageLeft | Nach links (zur linken bzw. vorherigen horizontalen Seite oder Gruppe) wechseln
|   PageRight | Nach rechts (zur rechten bzw. nächsten horizontalen Seite oder Gruppe) wechseln
|    ScrollUp | Bildlauf nach oben (im markierten Benutzeroberflächenelement oder der aktuellen Gruppe)
|  ScrollDown | Bildlauf nach unten (im markierten Benutzeroberflächenelement oder der aktuellen Gruppe)
|  ScrollLeft | Bildlauf nach links (im markierten Benutzeroberflächenelement oder der aktuellen Gruppe)
| ScrollRight | Bildlauf nach rechts (im markierten Benutzeroberflächenelement oder der aktuellen Gruppe)
|    Context1 | Erste Kontextaktion
|    Context2 | Zweite Kontextaktion
|    Context3 | Dritte Kontextaktion
|    Context4 | Vierte Kontextaktion

> **Hinweis** Auch wenn ein Spiel beliebig auf einen Befehl reagieren kann und sich eine tatsächliche Funktion von der vorgesehenen Verwendung unterscheiden darf, sollte überraschendes Verhalten vermieden werden. Ändern Sie insbesondere nicht die tatsächliche Funktion eines Befehls, wenn Sie dessen vorgesehene Nutzung benötigen. Weisen Sie neue Funktionen dem Befehl zu, bei dem es am sinnvollsten erscheint, und weisen Sie die gegenteiligen Funktionen gegenteiligen Befehlen zu, wie etwa BildAuf/BildAb. Überlegen Sie schließlich, welche Befehle von allen Eingabegeräten unterstützt werden und welche Steuerelemente ihnen zugeordnet sind, und stellen Sie dadurch sicher, dass kritische Befehle über jedes Gerät aufgerufen werden können.

## <a name="gamepad-arcade-stick-and-racing-wheel-navigation"></a>Navigation per Gamepad, Arcade-Joystick und Rennlenkrad

Alle vom Windows.Gaming.Input-Namespace unterstützten Eingabegeräte sind Benutzeroberflächen-Navigationsgeräte.

Die folgende Tabelle fasst zusammen, wie der _erforderliche Satz_ der Navigationsbefehle verschiedenen Eingabegeräten zugeordnet wird.

| Navigationsbefehl | Gamepad-Eingabe                       | Eingabe per Arcade-Joystick | Rennlenkrad-Eingabe |
| ------------------:| ----------------------------------- | ------------------ | ------------------ |
|                 Nach oben | Linker Ministick nach oben/Steuerkreuz nach oben       | Joystick nach oben           | Steuerkreuz nach oben           |
|               Nach unten | Linker Ministick nach unten/Steuerkreuz nach unten   | Joystick nach unten         | Steuerkreuz nach unten         |
|               Left | Linker Ministick nach links/Steuerkreuz nach links   | Joystick nach links         | Steuerkreuz nach links         |
|              Right | Linker Ministick nach rechts/Steuerkreuz nach rechts | Joystick nach rechts        | Steuerkreuz nach rechts        |
|               Sicht | Ansicht-Taste                         | Ansicht-Taste        | Ansicht-Taste        |
|               Menü | Menü-Taste                         | Menü-Taste        | Menü-Taste        |
|             Akzeptieren | A-Taste                            | Taste für Aktion 1    | A-Taste           |
|             Abbrechen | B-Taste                            | Taste für Aktion 2    | B-Taste           |

Die folgende Tabelle fasst zusammen, wie der _optionale Satz_ der Navigationsbefehle verschiedenen Eingabegeräten zugeordnet wird.

| Navigationsbefehl | Gamepad-Eingabe          | Eingabe per Arcade-Joystick | Rennlenkrad-Eingabe    |
| ------------------:| ---------------------- | ------------------ | --------------------- |
|             BILD-AUF | Linker Trigger           | _nicht unterstützt_    | _variiert_              |
|           BILD-AB | Rechter Trigger          | _nicht unterstützt_    | _variiert_              |
|           PageLeft | Linker Bumper            | _nicht unterstützt_    | _variiert_              |
|          PageRight | Rechter Bumper           | _nicht unterstützt_    | _variiert_              |
|           ScrollUp | Rechter Ministick nach oben    | _nicht unterstützt_    | _variiert_              |
|         ScrollDown | Rechter Ministick nach unten  | _nicht unterstützt_    | _variiert_              |
|         ScrollLeft | Rechter Ministick nach links  | _nicht unterstützt_    | _variiert_              |
|        ScrollRight | Rechter Ministick nach rechts | _nicht unterstützt_    | _variiert_              |
|           Context1 | X-Taste               | _nicht unterstützt_    | X-Taste (_gängig_) |
|           Context2 | Y-Taste               | _nicht unterstützt_    | Y-Schaltfläche (_häufig_) |
|           Context3 | Linken Ministick drücken  | _nicht unterstützt_    | _variiert_              |
|           Context4 | Rechten Ministick drücken | _nicht unterstützt_    | _variiert_              |


## <a name="detect-and-track-ui-navigation-controllers"></a>Benutzeroberflächen-Navigationscontroller erkennen und nachverfolgen

Auch wenn Benutzeroberflächen-Navigationscontroller logische Eingabegeräte sind, sind sie eine Darstellung eines physischen Geräts und werden auf diese Weise vom System verwaltet. Sie müssen sie nicht erstellen oder initialisieren. Das System stellt eine Liste der verbundenen Benutzeroberflächen-Navigationscontroller und Ereignisse bereit, um Sie zu benachrichtigen, wenn ein Benutzeroberflächen-Navigationscontroller hinzugefügt oder entfernt wird.

### <a name="the-ui-navigation-controllers-list"></a>Die Liste der Benutzeroberflächen-Navigationscontroller

Die [UINavigationController][]-Klasse stellt die statische Eigenschaft [UINavigationControllers][] bereit, bei der es sich um eine schreibgeschützte Liste der Benutzeroberflächen-Navigationsgeräte handelt, die zurzeit verbunden sind. Da Sie möglicherweise nur an einigen der verbundenen Navigationsgeräte interessiert sind, wird empfohlen, dass Sie eine eigene Sammlung verwalten, statt über die Eigenschaft `UINavigationControllers` auf diese zuzugreifen.

Im folgenden Beispiel werden alle verbundenen Benutzeroberflächen-Navigationscontroller in eine neue Sammlung kopiert.
```cpp
auto myNavigationControllers = ref new Vector<UINavigationController^>();

for (auto device : UINavigationController::UINavigationControllers)
{
    // This code assumes that you're interested in all navigation controllers.
    myNavigationControllers->Append(device);
}
```

### <a name="adding-and-removing-ui-navigation-controllers"></a>Hinzufügen und Entfernen von Benutzeroberflächen-Navigationscontrollern

Wenn ein Benutzeroberflächen-Navigationscontroller hinzugefügt oder entfernt wird, werden die Ereignisse [UINavigationControllerAdded][] und [UINavigationControllerRemoved][] ausgelöst. Sie können einen Ereignishandler für diese Ereignisse zum Nachverfolgen der Navigationsgeräte registrieren, die zurzeit verbunden sind.

Im folgenden Beispiel wird die Nachverfolgung eines Benutzeroberflächen-Navigationsgeräts gestartet, das hinzugefügt wurde.
```cpp
UINavigationController::UINavigationControllerAdded += ref new EventHandler<UINavigationController^>(Platform::Object^, UINavigationController^ args)
{
    // This code assumes that you're interested in all new navigation controllers.
    myNavigationControllers->Append(args);
}
```

Im folgenden Beispiel wird die Nachverfolgung eines entfernen Arcade-Joysticks beendet.
```cpp
UINavigationController::UINavigationControllerRemoved += ref new EventHandler<UINavigationController^>(Platform::Object^, UINavigationController^ args)
{
    unsigned int indexRemoved;

    if(myNavigationControllers->IndexOf(args, &indexRemoved))
    {
        myNavigationControllers->RemoveAt(indexRemoved);
    }
}
```

### <a name="users-and-headsets"></a>Benutzer und Headsets

Jedes Navigationsgerät kann einem Benutzerkonto zugeordnet werden, um die Identität mit der Eingabe zu verknüpfen. Sie können einen Kopfhörer anhängen, um Sprachchat oder die Navigationsfunktionen zu erleichtern. Weitere Informationen zu Benutzern und Headsets finden Sie unter [Nachverfolgen von Benutzern und ihren Geräten](input-practices-for-games.md#tracking-users-and-their-devices) und [Headsets](headset.md).

## <a name="reading-the-ui-navigation-controller"></a>Lesen des Benutzeroberflächen-Navigationscontrollers

Nachdem Sie das Benutzeroberflächen-Navigationsgerät identifiziert haben, für das Sie sich interessieren, können Sie Eingaben von ihm erfassen. Anders als im Fall anderer Eingaben, die Sie möglicherweise kennen, teilen Navigationsgeräte Zustandsänderungen jedoch nicht durch das Auslösen von Ereignissen mit. Stattdessen müssen Sie den aktuellen Status regelmäßig lesen, indem Sie ihn _abrufen_.

### <a name="polling-the-ui-navigation-controller"></a>Abrufen des Benutzeroberflächen-Navigationscontrollers

Beim Abruf wird eine Momentaufnahme des Navigationsgeräts an einem bestimmten Zeitpunkt erfasst. Dieser Ansatz zum Erfassen von Eingaben ist für die meisten Spiele geeignet, da deren Logik in der Regel in einer deterministischen Schleife ausgeführt wird und nicht ereignisgesteuert ist. Es ist in der Regel auch einfacher, Befehle in Spielen anhand von Eingaben zu interpretieren, die zusammen erfasst werden, als anhand zahlreicher Eingaben, die über die Zeit erfasst werden.

Sie fragen ein Navigationsgerät durch Aufrufen von [UINavigationController.GetCurrentReading][getcurrentreading] ab. Diese Funktion gibt [UINavigationReading][] zurück, das den Status des Navigationsgeräts enthält.

```cpp
auto navigationController = myNavigationControllers[0];

UINavigationReading reading = navigationController->GetCurrentReading();
```

### <a name="reading-the-buttons"></a>Lesen der Tastenwerte

Alle Benutzeroberflächen-Navigationstasten stellen einen booleschen Wert bereit, der angibt, ob sie gedrückt (nach unten) oder nicht gedrückt werden (nach oben). Aus Effizienzgründen werden die Tastenwerte nicht als einzelne boolesche Werte dargestellt. Stattdessen werden sie alle in eines von zwei Bitfeldern gepackt, die durch die Enumerationen [RequiredUINavigationButtons][] und [OptionalUINavigationButtons][] dargestellt werden.

Die Schaltflächen im _erforderlichen Satz_ werden von der `RequiredButtons`-Eigenschaft der [UINavigationReading][]-Struktur gelesen, die Schaltflächen im _optionalen_ werden von der `OptionalButtons`-Eigenschaft gelesen. Da diese Eigenschaften Bitfelder sind, wird eine bitweise Maskierung verwendet, um den Wert der Taste zu isolieren, an der Sie interessiert sind. Die Taste ist gedrückt (unten), wenn das entsprechende Bit festgelegt ist. Andernfalls ist sie nicht gedrückt (oben).

Im folgenden Beispiel wird ermittelt, ob die Schaltfläche „Annehmen“ im _erforderlichen Satz_ gedrückt ist.
```cpp
if (RequiredUINavigationButtons::Accept == (reading.RequiredButtons & RequiredUINavigationButtons::Accept))
{
    // Accept is pressed
}
```

Im folgenden Beispiel wird ermittelt, ob die Schaltfläche „Annehmen“ im _erforderlichen Satz_ nicht gedrückt ist.
```cpp
if (RequiredUINavigationButtons::None == (reading.RequiredButtons & RequiredUINavigationButtons::Accept))
{
    // Accept is released (not pressed)
}
```

Verwenden Sie die `OptionalButtons`-Eigenschaft und die `OptionalUINavigationButtons`-Enumeration beim Lesen von Schaltflächen im _optionalen Satz_.

Im folgenden Beispiel wird bestimmt, ob die Context1-Taste im _optionalen Satz_ gedrückt ist.
```cpp
if (OptionalUINavigationButtons::Context1 == (reading.OptionalButtons & OptionalUINavigationButtons::Context1))
{
    // Context 1 is pressed
}
```

In einigen Fällen möchten Sie möglicherweise ermitteln, ob eine Taste von „Gedrückt“ zu „Losgelassen“ oder von „Losgelassen“ zu „Gedrückt“ wechselt, ob mehrere Tasten gedrückt oder losgelassen werden, oder ob verschiedene Tasten in einer bestimmten Weise angeordnet sind – einige sind gedrückt, andere nicht. Weitere Informationen zum Ermitteln dieser Bedingungen finden Sie unter [Erkennen von Tastenübergängen](input-practices-for-games.md#detecting-button-transitions) und [Erkennen von komplexen Tastenanordungen](input-practices-for-games.md#detecting-complex-button-arrangements).


## <a name="run-the-ui-navigation-controller-sample"></a>Beispiel zum Ausführen des Benutzeroberflächen-Navigationscontrollers

Das [InputInterfacingUWP-Beispiel _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/InputInterfacingUWP) veranschaulicht, wie sich verschiedene Eingabegeräte als Benutzeroberflächen-Navigationscontroller verhalten.

## <a name="see-also"></a>Weitere Informationen
[Windows. Gaming. Input. Gamepad][] 
 [Windows. Gaming. Input. arcadestick][] 
 [Windows. Gaming. Input. racingwheel][] 
 [Windows. Gaming. Input. igamecontroller][]


[Windows.Gaming.Input]: /uwp/api/Windows.Gaming.Input
[Windows.Gaming.Input.Gamepad]: /uwp/api/Windows.Gaming.Input.Gamepad
[Windows. Gaming. Input. arcadestick]: /uwp/api/Windows.Gaming.Input.ArcadeStick
[Windows. Gaming. Input. racingwheel]: /uwp/api/Windows.Gaming.Input.RacingWheel
[Windows.Gaming.Input.IGameController]: /uwp/api/Windows.Gaming.Input.IGameController
[uinavigationcontroller]: /uwp/api/Windows.Gaming.Input.UINavigationController
[uinavigationcontrollers]: /uwp/api/Windows.Gaming.Input.UINavigationController
[uinavigationcontrolleradded]: /uwp/api/Windows.Gaming.Input.UINavigationController
[uinavigationcontrollerremoved]: /uwp/api/Windows.Gaming.Input.UINavigationController
[getcurrentreading]: /uwp/api/Windows.Gaming.Input.UINavigationController
[uinavigationreading]: /uwp/api/Windows.Gaming.Input.UINavigationReading
[requireduinavigationbuttons]: /uwp/api/Windows.Gaming.Input.RequiredUINavigationButtons
[optionaluinavigationbuttons]: /uwp/api/Windows.Gaming.Input.OptionalUINavigationButtons