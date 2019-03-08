---
Description: Entwerfen Sie Ihre app, damit sie Ihren vorstellungen entspricht, und gut in Mixed Reality funktioniert.
title: Design für Mixed Reality
ms.assetid: ''
label: Designing for Mixed Reality
template: detail.hbs
isNew: true
keywords: Mixed Reality, Hololens, Erweiterte Realität, anvisieren, Stimme, Controller
ms.date: 02/05/2018
ms.topic: article
pm-contact: chigy
design-contact: jeffarn
dev-contact: ''
doc-status: ''
ms.localizationpriority: medium
ms.openlocfilehash: b988859422a80b31d94a133e36631b078ac7c14e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610495"
---
# <a name="designing-for-mixed-reality"></a>Design für Mixed Reality

Gestalten Sie Ihre Anwendung so, dass sie in Mixed Reality gut aussieht und nutzen Sie neue Eingabemethoden.

## <a name="overview"></a>Übersicht

[Mixed Reality](https://developer.microsoft.com/windows/mixed-reality/mixed_reality) ist das Ergebnis der Verschmelzung der physischen Welt mit der digitalen Welt. Das Spektrum der Mixed Reality-Erfahrungen umfasst zum einen extreme Geräte wie HoloLens (ein Gerät, das computergenerierte Inhalte mit der realen Welt mischt) und zum anderen eine völlig Sicht auf die virtuelle Realität (wie mit einem Windows Mixed Reality-Headset). Siehe [Typen von Mixed Reality-Anwendungen](https://developer.microsoft.com/en-us/windows/mixed-reality/types_of_mixed_reality_apps) für Beispiele zu verschiedenen Erfahrungen.

Fast alle existierenden UWP-Apps laufen in der Mixed Reality-Umgebung ohne Änderungen als 2D-Anwendungen, obwohl die Erfahrung für den Benutzer verbessert werden kann, indem einige der Anleitungen in diesem Thema befolgt werden.

![Mixed Reality-Ansicht](images/MR-01.png)

Sowohl HoloLens als auch die Windows Mixed Reality-Headsets unterstützen Anwendungen, die auf der UWP-Plattform laufen. Beide unterstützen zwei verschiedene Arten von Erfahrungen. 

### <a name="2d-vs-immersive-experience"></a>2D im Vergleich zu Eintauchen

Eine immersive App übernimmt die gesamte für den Benutzer sichtbare Anzeige und stellt sie in den Mittelpunkt einer von der App erstellten Ansicht. Zum Beispiel könnte ein immersives Spiel den Benutzer auf die Oberfläche eines fremden Planeten bringen, oder eine Reiseleiter-App könnte den Beutzer in ein südamerikanisches Dorf bringen. Die Erstellung einer immersiven App erfordert 3D-Grafiken oder aufgenommene stereografische Videos. Immersive Apps werden oft mit einer Drittanbieter-Game-Engine wie Unity oder mit DirectX entwickelt.

Wenn Sie immersive App erstellen, sollten Sie das [Windows Mixed Reality Dev Center](https://developer.microsoft.com/windows/mixed-reality) besuchen, um weitere Informationen zu erhalten.

Eine 2D-App läuft als traditionelles, flaches Fenster im Blickfeld des Benutzers. Auf HoloLens ist dies Fenster an die Wand oder einen Punkt im Raum des eigenen Wohnzimmers oder Büros geheftet. In einem Windows Mixed Reality-Headset wird die App an eine Wand im [Mixed Reality Home](https://docs.microsoft.com/windows/mixed-reality/enthusiast-guide/your-mixed-reality-home) (manchmal auch *Cliff House* genannt) geheftet.

![Mehrere App in Mixed Reality](images/MR-multiple.png)


Diese 2D-Apps übernehmen nicht die gesamte Ansicht: Sie werden in ihr platziert. Mehrere 2D-Apps können gleichzeitig in der Umgebung existieren.

Der Rest dieses Themas behandelt Designüberlegungen für die 2D-Erfahrung.

## <a name="launching-2d-apps"></a>Starten von 2D-Apps

![Mixed Reality-Startmenü](images/MR-start-options.png)

Alle Apps werden über das Startmenü gestartet, aber es ist auch möglich, ein 3D-Objekt zu erstellen, das als App-Launcher fungiert. Details finden Sie in [diesem Video](https://www.youtube.com/watch?v=TxIslHsEXno).

## <a name="the-2d-app-input-overview"></a>Die 2D-App-Input-Übersicht

Tastaturen und Mäuse werden sowohl bei HoloLens als auch bei Mixed Reality-Plattformen unterstützt. Sie können eine Tastatur und Maus über Bluetooth direkt mit HoloLens verbinden. Mixed Reality-Apps unterstützen Maus und Tastatur, die mit dem Host-Computer verbunden sind. Beides kann in Situationen nützlich sein, in denen eine präzise Steuerung erforderlich ist.

Andere, natürlichere Eingabemethoden werden ebenfalls unterstützt. Diese können besonders nützlich sein, wenn der Benutzer nicht an einem Schreibtisch mit einer echten Tastatur vor sich sitzt oder wenn keine präzise Steuerung erforderlich ist.

Ohne zusätzliche Hardware oder Programmierung nutzen Apps bei der Arbeit mit 2D-apps das Anvisieren – den Vektor, in dem der Benutzer schaut – als Mauszeiger. Dies ist so implementiert, als ob ein Mauszeiger über etwas in der virtuellen Szene schwebt.

In einer typischen Interaktion betrachtet Ihr Benutzer ein Steuerelement in Ihrer App, wodurch es hervorgehoben wird. Der Benutzer löst eine Aktion entweder mit einer Geste (bei HoloLens), mit einem Controller oder mit einem Sprachbefehl aus. Wenn der Benutzer ein Texteingabefeld auswählt, erscheint die Softwaretastatur. 


![Die Pop-Up-Tastatur in Mixed Reality](images/MR-keyboard.png)

Es ist wichtig zu beachten, dass diese Interaktionen automatisch und ohne zusätzliche Programmierung Ihrerseits erfolgen. Dies liegt an der Ausführung auf der UWP-Plattform. Die Eingaben von HoloLens und Mixed Reality-Headsets erscheinen für die 2D-App als Touch-Eingabe. Das bedeutet, dass viele UWP-Apps standardmäßig in Mixed Reality laufen und nutzbar sind. 

Allerdings kann die Erfahrung mit etwas mehr Arbeit erheblich verbessert werden. Beispielsweise kann die [Sprachsteuerung](https://developer.microsoft.com/windows/mixed-reality/voice_design) besonders effektiv sein. Sowohl HoloLens als auch Mixed Reality-Umgebungen unterstützen Sprachbefehle zum Starten und Interagieren mit Apps, und die Sprachunterstützung wird als natürliche Erweiterung dieses Ansatzes wahrgenommen. Weitere Informationen zum Hinzufügen von Sprachunterstützung zu Ihrer UWP-App finden Sie unter [Sprachinteraktionen]( https://docs.microsoft.com/windows/uwp/design/input/speech-interactions). 


### <a name="selecting-the-right-controller"></a>Auswahl des richtigen Controllers

![Mixed Reality-Motion-Controller](images/MR-controllers.png)

Mehrere neuartige Eingabemethoden wurden speziell für den Einsatz mit Mixed Reality entwickelt:

* [Handbewegungen](https://developer.microsoft.com/windows/mixed-reality/gestures) (nur HoloLens, aber nur zum Starten von 2D-Apps)
* [Gamepad-Unterstützung](https://developer.microsoft.com/windows/mixed-reality/hardware_accessories) (beide Umgebungen)
* [Klick-Gerät](https://developer.microsoft.com/windows/mixed-reality/hardware_accessories) (nur HoloLens)
* [Motion-Controller](https://developer.microsoft.com/windows/mixed-reality/motion_controllers) (nur Mixed Reality-Geräte, siehe oben)

Diese Controller lassen die Interaktion mit virtuellen Objekten natürlich und präzise erscheinen. Einige der Interaktionen erhalten Sie kostenlos. Beispielsweise wählen Sie die HoloLens Geste oder durch Klicken auf die Motion-Controller-Windows-Schlüssel oder einem Trigger wird die Eingabe Antwort erwarten Sie, die erneut, ohne Codierung ihrerseits generiert.

Manchmal werden Sie trotzdem Code hinzufügen wollen, um die Vorteile der zusätzlichen Informationen und Eingaben zu nutzen. Mit den Motion Controllern können z. B. Objekte präzise manipuliert werden, wenn Sie Code schreiben, der deren Position und Tastendruck berücksichtigt.

> [!NOTE]
> Fazit: Das Leitmotiv sollte sein, dem Anwender immer eine möglichst natürliche und reibungslose Eingabemethode zur Verfügung zu stellen.


## <a name="2d-app-design-considerations-functionality"></a>Direct2D-App-Design-Überlegungen: Funktion

Bei der Erstellung einer UWP-App, die möglicherweise auf einer Mixed Reality-Plattform verwendet wird, sind einige Dinge zu beachten.

* Drag and Drop funktioniert möglicherweise nicht gut, wenn es mit Motion Controllern, Gamepads oder Gesten verwendet wird. Wenn Ihre Anwendung stark von Drag&Drop abhängig ist, müssen Sie eine alternative Methode zur Unterstützung dieser Aktion anbieten, z. B. einen Dialog zur Bestätigung, ob Objekte an einen neuen Ort verschoben werden sollen.

* Seien Sie sich bewusst, wie sich der Klang verändert. Wenn Ihre App Soundeffekte erzeugt, erscheint die Quelle des Sounds als die festgelegte Position Ihrer App in der virtuellen Welt. Wenn sich der Benutzer von der App entfernt, wird der Sound leiser. Siehe [Raumklang](https://developer.microsoft.com/windows/mixed-reality/spatial_sound) für weitere Informationen.

* Berücksichtigen Sie das Sichtfeld und stellen Sie entsprechende Angebote bereit. Nicht jedes Gerät bietet ein so großes Sichtfeld wie ein Computermonitor. Siehe [Holografischer Rahmen](https://developer.microsoft.com/windows/mixed-reality/holographic_frame) für weitere Details. Außerdem kann es sein, dass sich der Benutzer in einiger Entfernung von einer laufenden App befindet. Das heißt, die App kann an einer anderen Stelle der Welt (real oder virtuell) an die Wand geheftet erscheinen. Möglicherweise muss Ihre App die Aufmerksamkeit der Benutzer auf sich ziehen oder berücksichtigen, dass die gesamte Ansicht nicht immer sichtbar ist. Popup-Benachrichtigungen sind verfügbar, aber eine andere Möglichkeit, die Aufmerksamkeit des Benutzers zu erregen, ist die Erzeugung eines Ton- oder [Sprachalarms](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/SpeechRecognitionAndSynthesis/cs/Scenario_SynthesizeText.xaml.cs).

* Eine 2D-App wird automatisch mit einer [App-Leiste](https://developer.microsoft.com/windows/mixed-reality/app_bar_and_bounding_box) versehen, damit der Benutzer sie in der virtuellen Umgebung verschieben und skalieren kann. Die Ansichten können vertikal oder unter Beibehaltung des gleichen Seitenverhältnisses in der Größe verändert werden.


## <a name="2d-app-design-considerations-uiux"></a>Überlegungen zum Entwurf der Direct2D-app: UI/UX

* XAML-Steuerelemente, die das [Fluent Design-System](https://docs.microsoft.com/windows/uwp/design/fluent-design-system/) (z. B. die [Navigationsansicht](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview) und Effekte wie [Acrylic ](https://docs.microsoft.com/windows/uwp/design/style/acrylic)) implementieren, funktionieren besonders gut in 2D-Mixed Reality-Apps.

* Testen Sie die Text- und Fenstergröße Ihrer App in einem Mixed Reality-Gerät oder zumindest im Mixed Reality-Simulator. Ihre App hat eine Standardfenstergröße von 853 x 480 effektiven Pixeln. Verwenden Sie größere Schriftgrößen (eine Punktgröße von ca. 32 wird empfohlen), und lesen Sie [Aktualisieren Ihrer bestehenden universellen Apps für Hololens](https://developer.microsoft.com/windows/mixed-reality/updating_your_existing_universal_app_for_hololens). Der Artikel [Typografie](https://developer.microsoft.com/windows/mixed-reality/typography) behandelt dieses Thema im Detail. Bei der Arbeit in Visual Studio gibt es eine XAML-Design-Editor-Einstellung für eine 57 Zoll HoloLens 2D-App, die eine Ansicht mit dem richtigen Maßstab und den richtigen Abmessungen bietet. 

![Text, der in Mixed Reality-Apps angezeigt wird, sollte groß sein.](images/MR-text.png)

* [Ihr Blick dient als Maus](https://developer.microsoft.com/windows/mixed-reality/gaze_targeting). Wenn der Benutzer etwas ansieht, wird dies wie ein **Touch-Hover**-Ereignis behandelt, sodass ein einfaches Betrachten eines Objekts ein versehentliches Popup oder eine andere unerwünschte Interaktion auslösen kann. Möglicherweise müssen Sie feststellen, ob die App derzeit in Mixed Reality läuft, und dieses Verhalten ändern. Weitere Informationen finden Sie unter **Laufzeitunterstützung** unten. 

* Wenn ein Benutzer mit einem Motion Controller auf etwas zeigt oder etwas anvisiert, wird ein **Touch-Hover**-Ereignis ausgelöst. Dieser besteht aus einem **PointerPoint**, wobei **PointerType****Touch** ist, aber **IsInContact****false** ist. Wenn irgendeine Form der Bestätigung auftritt (z. B. wird eine Taste des Gamepads gedrückt, ein Klick-Gerät gedrückt, ein Motion Controller-Trigger gedrückt oder die Spracherkennung löst „Select” aus), erfolgt ein **touch press**, wobei der **PointerPoint** den **IsInContact**-Wert **true** hat. Weitere Informationen zu diesen Eingabe-Ereignissen finden Sie unter [Touch-Interaktionen](https://docs.microsoft.com/windows/uwp/design/input/touch-interactions).

* Denken Sie daran: das Anvisieren ist nicht so genau wie das Zeigen mit der Maus. Kleinere Mausziele oder Schaltflächen können zu Frustration bei Ihren Benutzern führen. Also passen Sie die Größe der Steuerelemente entsprechend an. Wenn sie für die Touch ausgelegt sind, funktionieren sie in Mixed Reality. Sie können sich jedoch auch dafür entscheiden, einige Schaltflächen zur Laufzeit zu vergrößern. Weitere Informationen finden Sie unter [Aktualisieren Ihrer bestehenden universellen App für Hololens](https://developer.microsoft.com/windows/mixed-reality/updating_your_existing_universal_app_for_hololens).

* HoloLens definiert die Farbe Schwarz als Abwesenheit von Licht. Sie wird einfach nicht gerendert und lässt die „reale Welt” so durchscheinen. Ihre Anwendung sollte kein schwarz verwenden, wenn dies Verwirrung stiften würde. In einem Mixed Reality-Headset ist Schwarz schwarz.

* HoloLens unterstützt keine Farbdesigns in Apps und ist standardmäßig auf „Blau” eingestellt, um den Benutzern ein optimales Erlebnis zu bieten. Weitere Hinweise zur Farbauswahl finden Sie in [diesem Thema](https://developer.microsoft.com/windows/mixed-reality/color,_light_and_materials), das die Verwendung von Farbe und Material in Mixed Reality-Designs behandelt.


## <a name="other-points-to-consider"></a>Weitere zu beachtende Punkte

* Obwohl die [Desktop-Brücke](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root) dabei helfen kann, bestehende (Win32-)Desktop-Apps auf Windows 10 und den Microsoft Store zu aktualisieren, kann sie derzeit keine Apps erstellen, die auf HoloLens oder in Mixed Reality laufen.




## <a name="runtime-support"></a>Laufzeitunterstützung

Ihre App kann zur Laufzeit feststellen, ob sie auf einem Mixed Reality-Gerät läuft, und dies als Gelegenheit nutzen, um die Größe der Steuerelemente zu ändern oder auf andere Weise die App für die Verwendung auf einem Headset zu optimieren.

Hier ist ein kurzes Stück Code, das die Größe des Textes innerhalb eines XAML-TextBlock-Steuerelements nur dann ändert, wenn die App auf einem Mixed Reality Gerät verwendet wird.

```csharp

bool isViewingInMR = Windows.ApplicationModel.Preview.Holographic.HolographicApplicationPreview.IsCurrentViewPresentedOnHolographicDisplay();

            if (isViewingInMR)
            {
                // Running on headset, resize the XAML text
                textBlock.Text = "I'm running in Mixed Reality!";
                textBlock.FontSize = 32;
            }
            else
            {
                // Running on desktop
                textBlock.Text = "I'm running on the desktop.";
                textBlock.FontSize = 16;
            }

```





## <a name="related-articles"></a>Verwandte Artikel


* [Aktuelle Einschränkungen für apps mithilfe von APIs in der Befehlsshell](https://developer.microsoft.com/windows/mixed-reality/current_limitations_for_apps_using_apis_from_the_shell)
* [Erstellen von Direct2D-apps](https://developer.microsoft.com/windows/mixed-reality/building_2d_apps)
* [HoloLens: Erstellen von 2D-UWP-Apps für Microsoft HoloLens](https://channel9.msdn.com/Events/Build/2016/B854)
* [Conditional XAML](https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/conditional-xaml)


