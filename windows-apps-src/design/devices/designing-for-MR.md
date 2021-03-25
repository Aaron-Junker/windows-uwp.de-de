---
description: Entwerfen Sie Ihre APP so, dass Sie gut aussieht und gut in gemischter Realität funktioniert.
title: Design für Mixed Reality
ms.assetid: ''
label: Designing for Mixed Reality
template: detail.hbs
isNew: true
keywords: Gemischte Realität, hololens, Erweiterte Realität, Blick, Stimme, Controller
ms.date: 02/26/2021
ms.topic: article
pm-contact: chigy
design-contact: jeffarn
dev-contact: ''
doc-status: ''
ms.localizationpriority: medium
ms.openlocfilehash: 11987ec6e7bd5f97937cc7b9aa86ec9f0fafcca1
ms.sourcegitcommit: e8ea2a36e4f2b9e0326958d226a36dd30c3efa57
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/25/2021
ms.locfileid: "105099821"
---
# <a name="designing-for-mixed-reality"></a>Design für Mixed Reality

Entwerfen Sie Ihre APP so, dass Sie in gemischter Realität gut aussieht, und profitieren Sie von neuen Eingabemethoden.

## <a name="overview"></a>Übersicht

[Gemischte Realität](https://developer.microsoft.com/windows/mixed-reality/mixed_reality) ist das Ergebnis der Mischung der physischen Welt mit der digitalen Welt. Das Spektrum gemischter Reality-Erfahrungen umfasst auf einem extrem Gerät, wie z. b. den hololens (ein Gerät, das vom computergenerierte Inhalte in der realen Welt mischt) und dem anderen eine vollständig immersive Ansicht der virtuellen Realität (wie in einem Windows Mixed Reality-Headset angezeigt). Unter [Typen von Mixed Reality-apps](https://developer.microsoft.com/windows/mixed-reality/types_of_mixed_reality_apps) finden Sie Beispiele dazu, wie sich die Benutzeroberflächen unterscheiden.

Fast alle vorhandenen UWP-apps werden in der gemischten Reality-Umgebung als 2D-apps ohne Änderungen ausgeführt, obwohl die Benutzerumgebung durch die folgenden Anweisungen in diesem Thema verbessert werden kann.

![Gemischte Realität (Ansicht)](images/MR-01.png)

Sowohl das hololens-als auch das Windows Mixed Reality-Headsets unterstützen Anwendungen, die auf der UWP-Plattform ausgeführt werden, und beide unterstützen zwei verschiedene Arten von Funktionen. 

### <a name="2d-vs-immersive-experience"></a>2D und immersive Darstellung

Eine immersive App übernimmt die gesamte Anzeige, die für den Benutzer sichtbar ist, und platziert Sie in der Mitte einer Ansicht, die von der App erstellt wurde. Beispielsweise kann ein immersives Spiel den Benutzer auf der Oberfläche eines fremden Planeten platzieren, oder eine Tour Guide-app könnte den Benutzer in einem Südamerika-Ort platzieren. Zum Erstellen einer immersiven APP sind 3D-Grafiken oder erfasste Stereografische Videos erforderlich. Immersive apps werden häufig mit einer Drittanbieter-Spiel-Engine wie Unity oder mit DirectX entwickelt.

Wenn Sie immersive Apps erstellen, besuchen Sie das [Windows Mixed Reality dev Center](https://developer.microsoft.com/mixed-reality) , um weitere Informationen zu erhalten.

Eine 2D-APP wird als herkömmliches flaches Fenster in der Ansicht des Benutzers ausgeführt. Auf dem hololens bedeutet das, dass eine Ansicht an die Wand oder an einem Punkt im Raum der Benutzer in der Praxis der realen Wohn Räume oder im Büro ist. In einem Windows Mixed Reality-Headset wird die APP an eine Wand in der [Mixed Reality-Startseite](/windows/mixed-reality/enthusiast-guide/your-mixed-reality-home) angeheftet (manchmal auch als " *drehhaus*" bezeichnet).

![Mehrere apps, die in gemischter Realität ausgeführt werden](images/MR-multiple.png)


Diese 2D-apps übernehmen nicht die gesamte Ansicht: Sie werden darin platziert. In der Umgebung können mehrere 2D-apps gleichzeitig vorhanden sein.

Im weiteren Verlauf dieses Themas werden Entwurfs Überlegungen für die 2D-Funktion erläutert.

## <a name="launching-2d-apps"></a>Starten von 2D-apps

![Gemischtes Reality-Startmenü](images/MR-start-options.png)

Alle apps werden über das Startmenü gestartet, es ist aber auch möglich, ein 3D-Objekt zu erstellen, das als App-Start Programm fungiert. Weitere Informationen finden Sie in [diesem Video](https://www.youtube.com/watch?v=TxIslHsEXno) .

## <a name="the-2d-app-input-overview"></a>Übersicht über die 2D-App-Eingabe

Tastaturen und Mäuse werden auf hololens-und Mixed Reality-Plattformen unterstützt. Sie können Tastatur und Maus direkt mit den hololens über Bluetooth koppeln. Mixed Reality-apps unterstützen die Maus und die Tastatur, die mit dem Host Computer verbunden sind. Beides kann in Situationen nützlich sein, in denen eine gute Kontrolle erforderlich ist.

Andere, natürlichere Eingabemethoden werden ebenfalls unterstützt, und diese können besonders nützlich sein, wenn der Benutzer nicht an einem Schreibtisch mit einer echten Tastatur vorsitzt oder wenn eine Feinsteuerung erforderlich ist.

Ohne zusätzliche Hardware oder Codierungen verwenden apps den Blick, und zwar den Vektor, den Ihr Benutzer beim Arbeiten mit 2D-Apps als Mauszeiger ansieht. Sie wird implementiert, als ob ein Mauszeiger auf etwas in der virtuellen Szene zeigt.

Bei einer typischen Interaktion wird der Benutzer ein Steuerelement in Ihrer APP betrachten, wodurch es hervorgehoben wird. Der Benutzer löst eine Aktion aus, indem er entweder eine Bewegung (auf dem hololens) oder einen "Voice"-Befehl verwendet. Wenn der Benutzer ein Texteingabefeld auswählt, wird die Tastatur der Software angezeigt. 


![Die Popup Tastatur in gemischter Realität](images/MR-keyboard.png)

Es ist wichtig zu beachten, dass alle diese Interaktionen automatisch ohne zusätzliche Codierung durchgeführt werden, weil Sie auf der UWP-Plattform ausgeführt werden. Eingaben aus dem hololens-und Mixed Reality-Headset werden als Berührungs Eingaben für die 2D-App angezeigt. Dies bedeutet, dass viele UWP-apps standardmäßig ausgeführt werden und in gemischter Realität verwendet werden können. 

Das heißt, mit einigen zusätzlichen Aufgaben kann die Leistung erheblich verbessert werden. Beispielsweise kann das [sprach Steuer](https://developer.microsoft.com/windows/mixed-reality/voice_design) Element besonders effektiv sein. Sowohl hololens-als auch Mixed Reality-Umgebungen unterstützen Sprachbefehle zum Starten und interagieren mit apps, und die Sprachunterstützung wird als natürliche Erweiterung dieses Ansatzes angezeigt. Weitere Informationen zum Hinzufügen von Sprachunterstützung zu ihrer UWP-App finden Sie unter [sprach Interaktionen]( ../input/speech-interactions.md) . 


### <a name="selecting-the-right-controller"></a>Auswählen des richtigen Controllers

![Gemischte Reality-Motion-Controller](images/MR-controllers.png)

Mehrere neuartige Eingabemethoden wurden speziell für die Verwendung mit gemischter Realität entworfen, insbesondere:

* [Hand Gesten](https://developer.microsoft.com/windows/mixed-reality/gestures) (nur hololens, aber nur zum Starten von 2D-Apps)
* [Gamepad-Unterstützung](https://developer.microsoft.com/windows/mixed-reality/hardware_accessories) (beide Umgebungen)
* [Clicker-Gerät](https://developer.microsoft.com/windows/mixed-reality/hardware_accessories) (nur hololens)
* [Bewegungs Controller](/windows/mixed-reality/motion-controllers) (nur Mixed Reality-Geräte, siehe oben)

Diese Controller sorgen dafür, dass die Interaktion mit virtuellen Objekten naturgemäß und präzise erscheint. Einige der Interaktionen, die Sie kostenlos erhalten. Beispielsweise wird mit der hololens-Option "hololens" oder durch Klicken auf die Windows-Taste oder den-Wert des Motion-Controllers die Eingangs Antwort generiert.

Zu anderen Zeitpunkten sollten Sie Code hinzufügen, um die zusätzlichen Informationen und Eingaben zu nutzen, die zur Verfügung gestellt werden. Beispielsweise können die Motion-Controller verwendet werden, um Objekte mit einer guten Steuerungsebene zu bearbeiten, wenn Sie Code schreiben, der Ihre Position und Schaltfläche in das Konto drückt.

> [!NOTE]
> Zusammenfassung: der Führungs Prinzipal sollte sein, dass der Benutzer immer eine Eingabemethode wie möglich mit einer natürlichen und reibungslosen Eingabemethode bereitstellt.


## <a name="2d-app-design-considerations-functionality"></a>Überlegungen zur 2D-App-Entwicklung: Funktionalität

Beim Erstellen einer UWP-APP, die möglicherweise auf einer gemischten Reality-Plattform verwendet wird, sind einige Dinge zu beachten.

* Drag & Drop funktioniert möglicherweise nicht gut, wenn Sie mit Motion-Controllern, Gamepads oder Gesten verwendet werden. Wenn Ihre Anwendung stark von Drag & Drop abhängig ist, müssen Sie eine alternative Methode zur Unterstützung dieser Aktion bereitstellen, z. b. ein Dialogfeld, das bestätigt, ob Objekte an einen neuen Speicherort verschoben werden.

* Beachten Sie, wie sich Sound geändert hat. Wenn Ihre APP Soundeffekte generiert, scheint die Quelle des Sounds der angeheftet Speicherort Ihrer APP in der virtuellen Welt zu sein. Wenn sich der Benutzer von der app Weg bewegt, verringert sich der Sound. Weitere Informationen finden Sie unter [Spatial Sound](/windows/mixed-reality/spatial-sound) .

* Betrachten Sie das Feld der Ansicht, und stellen Sie die Kosten bereit. Nicht jedes Gerät wird so groß wie ein Computermonitor bereitgestellt. Ausführliche Informationen finden Sie im [Holographic Frame](https://developer.microsoft.com/windows/mixed-reality/holographic_frame) . Außerdem kann es sein, dass der Benutzer eine gewisse Entfernung zu einer laufenden app hat. Das heißt, die APP erscheint an der Wand an einer anderen Stelle auf der Welt (Real oder virtuell). Möglicherweise muss Ihre APP den Benutzern Aufmerksamkeit schenken, oder Sie sollten berücksichtigen, dass die gesamte Ansicht nicht immer sichtbar ist. Popup Benachrichtigungen sind verfügbar, aber eine weitere Möglichkeit, um die Aufmerksamkeit des Benutzers zu erreichen, ist möglicherweise das Generieren einer Sound-oder [sprach](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/SpeechRecognitionAndSynthesis/cs/Scenario_SynthesizeText.xaml.cs) Warnung.

* Einer 2D-APP wird automatisch eine [App-Leiste](https://developer.microsoft.com/windows/mixed-reality/app_bar_and_bounding_box)  zugewiesen, damit Benutzer Sie in der virtuellen Umgebung verschieben und skalieren können. Die Größe der Sichten kann vertikal geändert werden, oder Sie können die Größe des gleichen Seitenverhältnisses ändern.


## <a name="2d-app-design-considerations-uiux"></a>Überlegungen zur 2D-App-Entwicklung: UI/UX

* XAML-Steuerelemente, die das [fließende Entwurfs System](/windows/uwp/design/fluent-design-system/) , z. b. die [Navigationsansicht](../controls-and-patterns/navigationview.md), und Effekte wie z. b. " [Acryl](../style/acrylic.md) " implementieren, funktionieren besonders gut in 2D Mixed Reality-apps

* Testen Sie die Text-und Windows-Größe Ihrer APP in einem Mixed Reality-Gerät oder zumindest im Mixed Reality-Simulator. Ihre APP verfügt über eine standardmäßige Windows-Größe von 853x480 effektiven Pixeln. Verwenden Sie größere Schriftgrößen (eine Punktgröße von ungefähr 32 wird empfohlen), und lesen Sie [Aktualisieren Ihrer vorhandenen universellen App für hololens](https://developer.microsoft.com/windows/mixed-reality/updating_your_existing_universal_app_for_hololens). In der [](https://developer.microsoft.com/windows/mixed-reality/typography) Artikel typografieartikel wird dieses Thema ausführlich behandelt. Bei der Arbeit in Visual Studio gibt es eine XAML-Entwurfs-Editor-Einstellung für eine 57 "hololens 2D-APP, die eine Ansicht mit der richtigen Skala und den richtigen Dimensionen bereitstellt. 

![Der in Mixed Reality-apps angezeigte Text sollte groß sein.](images/MR-text.png)

* [Ihr Blick ist mit der Maus](https://developer.microsoft.com/windows/mixed-reality/gaze_targeting). Wenn der Benutzer etwas ansieht, fungiert es als **Berührungs** Bewegungs Ereignis, sodass das einfache Überprüfen eines Objekts möglicherweise ein unbeabsichtigtes Popup oder eine andere unerwünschte Interaktion auslöst. Sie müssen möglicherweise erkennen, ob die APP derzeit in gemischter Realität ausgeführt wird, und dieses Verhalten ändern. Weitere Informationen finden Sie unten unter **Laufzeitunterstützung**. 

* Wenn ein Benutzer mit einem Bewegungs Controller in Bezug auf etwas oder auf Punkte zeigt, tritt ein Finger **Abdruck Ereignis auf** . Dies besteht aus einem **pointerpoint** , bei dem " **PointerType** " " **berühren**" ist, aber **isincontact** " **false**" ist. Wenn ein Commit ausgeführt wird (z. b. Gamepad eine Schaltfläche, wird ein Clicker-Gerät gedrückt, ein Motion Controller-Befehl gedrückt oder sprach Erkennungs Köpfe "Select"), wird ein **touchpress** angezeigt, wobei der **pointerpoint** mit **isincontact** **true** wird. Weitere Informationen zu diesen Eingabe Ereignissen finden Sie unter [touchinteraktionen](../input/touch-interactions.md) .

* Beachten Sie, dass der Blick nicht so genau ist wie der Mauszeiger. Kleinere Maus Ziele oder Schaltflächen können für Ihre Benutzer zu Frustrationen führen. ändern Sie daher die Größe der Steuerelemente entsprechend. Wenn Sie für die Fingereingabe konzipiert sind, funktionieren Sie in gemischter Realität, aber Sie können einige Schaltflächen zur Laufzeit vergrößern. Weitere Informationen finden [Sie unter Aktualisieren Ihrer vorhandenen universellen App für hololens](https://developer.microsoft.com/windows/mixed-reality/updating_your_existing_universal_app_for_hololens).

* Der hololens definiert die Farbe schwarz als fehlende Beleuchtung. Sie wird einfach nicht gerendert und ermöglicht so die "reale Welt". Die Anwendung sollte nicht schwarz verwenden, wenn dies Verwirrung verursachen würde. In einem Mixed Reality-Headset ist schwarz schwarz.

* Der hololens unterstützt keine Farbschemas in-apps, und der Standardwert ist blau, um die optimale Benutzer Leistung zu gewährleisten. Weitere Hinweise zum Auswählen von Farben finden Sie in [diesem Thema](https://developer.microsoft.com/windows/mixed-reality/color,_light_and_materials) , in dem die Verwendung von Farbe und Material in Mixed Reality-Entwürfen erläutert wird.


## <a name="other-points-to-consider"></a>Weitere zu berücksichtigende Punkte

* Obwohl die [Desktop Bridge](/windows/msix/desktop/source-code-overview) dabei helfen kann, vorhandene (Win32) Desktop-Apps auf Windows 10 und die Microsoft Store zu bringen, können keine apps erstellt werden, die zu diesem Zeitpunkt auf hololens ausgeführt werden. Ab Windows 10, Version 1903, können Win32-Desktop-Apps auf Mixed Reality-Headsets ausgeführt werden.

## <a name="runtime-support"></a>Laufzeitunterstützung

Ihre APP kann ermitteln, ob Sie zur Laufzeit auf einem gemischten Reality-Gerät ausgeführt wird, und diese als Gelegenheit zum Ändern der Größe von Steuerelementen oder auf andere Weise zur Optimierung der APP für die Verwendung auf einem Headset verwenden.

Im folgenden finden Sie einen kurzen Code Abschnitt, der die Größe des Texts in einem XAML-TextBlock-Steuerelement nur dann ändert, wenn die APP auf einem Mixed Reality-Gerät verwendet wird.

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
                textBlock.FontSize = 14;
            }

```





## <a name="related-articles"></a>Verwandte Artikel


* [Aktuelle Einschränkungen für apps, die APIs aus der Shell verwenden](https://developer.microsoft.com/windows/mixed-reality/current_limitations_for_apps_using_apis_from_the_shell)
* [Entwickeln von 2D-apps](https://developer.microsoft.com/windows/mixed-reality/building_2d_apps)
* [Hololens: Entwickeln von UWP-2D-Apps für Microsoft hololens](https://channel9.msdn.com/Events/Build/2016/B854)
* [Bedingtes XAML](../../debug-test-perf/conditional-xaml.md)
