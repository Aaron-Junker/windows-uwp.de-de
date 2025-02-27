---
title: Das App-Objekt und DirectX
description: Für die Universelle Windows-Plattform (UWP) mit DirectX-Spielen werden nur wenige der UI-Elemente und -objekte der Windows-Benutzeroberfläche genutzt.
ms.assetid: 46f92156-29f8-d65e-2587-7ba1de5b48a6
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, DirectX, App-Objekt
ms.localizationpriority: medium
ms.openlocfilehash: 08d2039bd7b3b8aa248acca31615d34635929aa1
ms.sourcegitcommit: 48702934676ae366fd46b7d952396c5e2fb2cbbe
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2021
ms.locfileid: "97927774"
---
# <a name="the-app-object-and-directx"></a>Das App-Objekt und DirectX

Für die Universelle Windows-Plattform (UWP) mit DirectX-Spielen werden nur wenige der UI-Elemente und -objekte der Windows-Benutzeroberfläche genutzt. Da sie auf einer niedrigeren Ebene des Windows-Runtime-Stapels ausgeführt werden, müssen sie stattdessen auf eine grundlegendere Weise mit dem Benutzeroberflächenframework interagieren, und zwar indem sie direkt auf das App-Objekt zugreifen und mit diesem interagieren. Im Folgenden erfahren Sie, zu welchem Zeitpunkt und auf welche Weise eine solche Interaktion erfolgt und wie Sie dieses Modell als DirectX-Entwickler beim Entwickeln von UWP-Apps effizient nutzen können.

Informationen zu unbekannten Grafik Begriffen oder Konzepten, die beim Lesen auftreten, finden Sie im [Direct3D-Grafik Glossar](../graphics-concepts/index.md) .

## <a name="the-important-core-user-interface-namespaces"></a>Die wichtigen Benutzeroberflächen-Hauptnamespaces

Zunächst sind die Windows-Runtime-Namespaces zu erwähnen, die Sie (mit **using**) in Ihre UWP-App einschließen müssen. Darauf gehen wir unten noch ausführlicher ein.

-   [**Windows. applicationmodel. Core**](/uwp/api/Windows.ApplicationModel.Core)
-   [**Windows. applicationmodel. Activation**](/uwp/api/Windows.ApplicationModel.Activation)
-   [**Windows. UI. Core**](/uwp/api/Windows.UI.Core)
-   [**Windows.System**](/uwp/api/Windows.System)
-   [**Windows.Foundation**](/uwp/api/Windows.Foundation)

> [!NOTE]
> Wenn Sie keine UWP-App entwickeln, verwenden Sie die Benutzeroberflächen Komponenten, die in den JavaScript-oder XAML-spezifischen Bibliotheken und Namespaces bereitgestellt werden, anstelle der Typen, die in diesen Namespaces bereitgestellt werden.

## <a name="the-windows-runtime-app-object"></a>Das Windows-Runtime-App-Objekt

Sie möchten in Ihrer UWP-App ein Fenster und einen Ansichtsanbieter abrufen, von dem Sie wiederum eine Ansicht abrufen und mit dem Sie Ihre Swapchain (Ihre Anzeigepuffer) verbinden können. Sie können diese Ansicht auch in die fensterspezifischen Ereignisse für die ausgeführte App einbinden. Um das übergeordnete Fenster für das App-Objekt zu erhalten, das durch den [**corewindow**](/uwp/api/Windows.UI.Core.CoreWindow) -Typ definiert ist, erstellen Sie einen Typ, der [**iframeworkviewsource**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkViewSource)implementiert. Ein [C++/WinRT](../cpp-and-winrt-apis/index.md) -Codebeispiel, das zeigt, wie **iframeworkviewsource** implementiert wird, finden Sie unter [Komposition Native Interoperation with DirectX and Direct2D](../composition/composition-native-interop.md).

Hier finden Sie die grundlegenden Schritte zum Einrichten eines Fensters mithilfe des Core-Benutzeroberflächen-Frameworks.

1.  Erstellen Sie einen Typ, der [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) implementiert. Dies ist Ihre Ansicht.

    Definieren Sie in diesem Typ Folgendes:

    -   Eine [**Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize)-Methode, die eine Instanz von [**CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) als Parameter annimmt. Sie können eine Instanz dieses Typs abrufen, indem Sie [**CoreApplication.CreateNewView**](/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) aufrufen. Das App-Objekt ruft die Methode beim Starten der App auf.
    -   Eine [**SetWindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow)-Methode, die eine Instanz von [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) als Parameter annimmt. Sie können eine Instanz dieses Typs abrufen, indem Sie auf die [**CoreWindow**](/uwp/api/windows.applicationmodel.core.coreapplicationview.corewindow)-Eigenschaft in der neuen [**CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)-Instanz zugreifen.
    -   Eine [**Load**](/uwp/api/windows.applicationmodel.core.iframeworkview.load)-Methode, die eine Zeichenfolge für einen Einstiegspunkt als einzigen Parameter annimmt. Das App-Objekt stellt den Einstiegspunkt bereit, wenn Sie diese Methode aufrufen. Hier richten Sie Ressourcen ein. Hier erstellen Sie Ihre Geräteressourcen. Das App-Objekt ruft die Methode beim Starten der App auf.
    -   Eine [**Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run)-Methode, die das [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)-Objekt aktiviert und den Fensterereignisverteiler startet. Der Aufruf durch das App-Objekt erfolgt, wenn der Prozess der App gestartet wird.
    -   Eine [**Uninitialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize)-Methode, die die im Aufruf von [**Load**](/uwp/api/windows.applicationmodel.core.iframeworkview.load) eingerichteten Ressourcen bereinigt. Das App-Objekt ruft diese Methode beim Schließen der App auf.

2.  Erstellen Sie einen Typ, der [**IFrameworkViewSource**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkViewSource) implementiert. Dies ist Ihr Ansichtsanbieter.

    Definieren Sie in diesem Typ Folgendes:

    -   Eine Methode mit dem Namen [**CreateView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource.createview), die eine Instanz Ihrer [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView)-Implementierung zurückgibt, die in Schritt 1 erstellt wurde.

3.  Übergeben Sie eine Instanz des Ansichts Anbieters an [**coreapplication. Run**](/uwp/api/windows.applicationmodel.core.coreapplication.run) von **Main**.

Da Sie jetzt mit den Grundlagen vertraut sind, betrachten Sie nun weitere verfügbare Optionen, mit denen Sie diesen Ansatz ausweiten können.

## <a name="core-user-interface-types"></a>Zentrale Benutzeroberflächentypen

Im Folgenden werden weitere zentrale Benutzeroberflächentypen in der Windows-Runtime vorgestellt, die möglicherweise hilfreich sein können:

-   [**Windows. applicationmodel. Core. coreapplicationview**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)
-   [**Windows.UI.Core.CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)
-   [**Windows.UI.Core.CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher)

Mit diesen Typen können Sie auf die Ansicht der App zugreifen, insbesondere auf die Bits, mit denen die Inhalte des übergeordneten Fensters der App gezeichnet werden, und Sie können die für dieses Fenster ausgelösten Ereignisse behandeln. Der Prozess des App-Fensters befindet sich in einem *Anwendungs-Single-Threaded-Apartment* (ASTA), das isoliert ist und alle Rückrufe behandelt.

Die Ansicht Ihrer App wird durch den Ansichtsanbieter für das App-Fenster generiert und in den meisten Fällen durch ein spezifisches Frameworkpaket oder direkt vom System implementiert, sodass Sie die Implementierung nicht selbst ausführen müssen. Für DirectX müssen Sie wie zuvor beschrieben einen kompakten Ansichtsanbieter implementieren. Zwischen den folgenden Komponenten und Verhalten besteht eine spezifische 1:1-Beziehung:

-   Die Ansicht einer App, die durch den [**CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)-Typ dargestellt wird und die die Methode(n) zum Aktualisieren des Fensters definiert.
-   Ein ASTA, dessen Attribute das Threadingverhalten der App definieren. Sie können keine Instanzen von STA-attributierten COM-Typen für ein ASTA erstellen.
-   Ein Ansichtsanbieter, den Ihre App vom System erhält oder der von Ihnen implementiert wird.
-   Ein übergeordnetes Fenster, das durch den [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)-Typ dargestellt wird.
-   Quellentnahme für alle Aktivierungsereignisse. Sowohl Ansichten als auch Fenster verfügen über separate Aktivierungsereignisse.

Dies lässt sich wie folgt zusammenfassen: Das App-Objekt stellt eine Ansichtsanbieterfactory bereit. Es erstellt einen Ansichtsanbieter und instanziiert ein übergeordnetes Fenster für die App. Der Ansichtsanbieter definiert die Ansicht der App für das übergeordnete Fenster der App. Im Folgenden wird genauer auf die Ansicht und das übergeordnete Fenster eingegangen.

## <a name="coreapplicationview-behaviors-and-properties"></a>CoreApplicationView – Verhalten und Eigenschaften

[**CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) stellt die aktuelle App-Ansicht dar. Das App-Singleton erstellt die App-Ansicht während der Initialisierung; die Ansicht bleibt jedoch so lange ruhend, bis sie aktiviert wird. Sie können das [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) abrufen, in dem die Ansicht angezeigt wird, indem Sie auf die zugehörige [**CoreApplicationView.CoreWindow**](/uwp/api/windows.applicationmodel.core.coreapplicationview.corewindow)-Eigenschaft zugreifen: Sie können Aktivierungs- und Deaktivierungsereignisse für die Ansicht behandeln, indem Sie Delegaten beim [**CoreApplicationView.Activated**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated)-Ereignis registrieren.

## <a name="corewindow-behaviors-and-properties"></a>CoreWindow – Verhalten und Eigenschaften

Das übergeordnete Fenster, das eine [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)-Instanz darstellt, wird erstellt und an den Ansichtsanbieter übergeben, wenn das App-Objekt initialisiert wird. Wenn die App über ein anzuzeigendes Fenster verfügt, wird dieses angezeigt. Andernfalls wird lediglich die Ansicht initialisiert.

[**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) stellt eine Reihe von Ereignissen bereit, die spezifisch für das Eingabeverhalten und das grundlegende Fensterverhalten sind. Sie können diese Ereignisse behandeln, indem Sie eigene Delegaten für diese registrieren.

Sie können auch den Fenster Ereignis Verteiler für das Fenster abrufen, indem Sie auf die [**corewindow. Verteiler**](/uwp/api/windows.ui.core.corewindow.dispatcher) -Eigenschaft zugreifen, die eine Instanz von [**coredispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher)bereitstellt.

## <a name="coredispatcher-behaviors-and-properties"></a>CoreDispatcher – Verhalten und Eigenschaften

Sie können das Threadingverhalten der Ereignisverteilung für ein Fenster mit dem [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher)-Typ bestimmen. Bei diesem Typ gibt es eine besonders wichtige Methode: die [**CoreDispatcher.ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents)-Methode, die die Fensterereignisverarbeitung startet. Wenn Sie diese Methode mit der falschen Option für Ihre App aufrufen, kann dies zu allen möglichen unerwarteten Verhaltensweisen bei der Ereignisverarbeitung führen.

| CoreProcessEventsOption-Option | BESCHREIBUNG |
|--------------------------------|-------------|
| [**CoreProcessEventsOption.ProcessOneAndAllPending**](/uwp/api/Windows.UI.Core.CoreProcessEventsOption) | Verteilen Sie alle derzeit in der Warteschlange verfügbaren Ereignisse. Wenn keine Ereignisse ausstehen, warten Sie auf das nächste neue Ereignis. |
| [**CoreProcessEventsOption.ProcessOneIfPresent**](/uwp/api/Windows.UI.Core.CoreProcessEventsOption) | Verteilen Sie ein Ereignis, wenn es in der Warteschlange aussteht. Wenn keine Ereignisse ausstehen, warten Sie nicht, dass ein neues Ereignis ausgelöst wird, sondern kehren Sie stattdessen sofort zurück. |
| [**CoreProcessEventsOption.ProcessUntilQuit**](/uwp/api/Windows.UI.Core.CoreProcessEventsOption) | Warten Sie auf neue Ereignisse, und verteilen Sie alle verfügbaren Ereignisse. Setzen Sie dieses Verhalten fort, bis das Fenster geschlossen wird oder die Anwendung die [**Close**](/uwp/api/windows.ui.core.corewindow.close)-Methode für die [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)-Instanz aufruft. |
| [**CoreProcessEventsOption.ProcessAllIfPresent**](/uwp/api/Windows.UI.Core.CoreProcessEventsOption) | Verteilen Sie alle derzeit in der Warteschlange verfügbaren Ereignisse. Wenn keine Ereignisse ausstehen, kehren Sie sofort zurück. |

UWP mit DirectX muss die [**CoreProcessEventsOption.ProcessAllIfPresent**](/uwp/api/Windows.UI.Core.CoreProcessEventsOption)-Option verwenden, um blockierendes Verhalten zu verhindern, das Grafikaktualisierungen beeinträchtigen könnte.

## <a name="asta-considerations-for-directx-devs"></a>Überlegungen zu ASTA für DirectX-Entwickler

Das App-Objekt, das die Laufzeitdarstellung Ihrer UWP- und DirectX-App definiert, verwendet das ASTA-Threadingmodell (Anwendungs-Single-Threaded-Apartment), um die UI-Ansichten der App zu hosten. Wenn Sie eine UWP- und DirectX-App entwickeln, sind Sie mit den Eigenschaften eines ASTAs vertraut, da alle Threads, die von Ihrer UWP- und DirectX-App gesendet werden, die [**Windows::System::Threading**](/uwp/api/Windows.System.Threading)-APIs oder [**CoreWindow::CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) verwenden müssen. (Sie können das [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)-Objekt für das ASTA abrufen, indem Sie [**CoreWindow::GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread) in Ihrer App aufrufen.)

Wichtig ist, dass Sie als Entwickler einer UWP DirectX-App wissen, dass Sie Ihren app-Thread aktivieren müssen, um MTA-Threads zu senden, indem Sie **Platform:: MTAThread** on **Main ()** festlegen.

```cpp
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto myDXAppSource = ref new MyDXAppSource(); // your view provider factory 
    CoreApplication::Run(myDXAppSource);
    return 0;
}
```

Wenn das App-Objekt für Ihre UWP-DirectX-App aktiviert wird, erstellt es ein ASTA, das für die UI-Ansicht verwendet wird. Der neue ASTA-Thread ruft die Factory für Ihren Ansichtsanbieter auf, um den Ansichtsanbieter für Ihr App-Objekt zu erstellen. Der Code für Ihren Ansichtsanbieter wird dann in dem ASTA-Thread ausgeführt.

Alle Threads, die aus einem ASTA ausgegliedert werden, müssen sich außerdem in einem MTA befinden. Beachten Sie, dass es durch ausgelagerte MTA-Threads weiterhin zu Eintrittsinvarianzen und somit zu Sperren kommen kann.

Wenn Sie vorhandenen Code zur Ausführung in einem ASTA-Thread portieren, sollten Sie folgende Punkte berücksichtigen:

-   Das Verhalten von Wartegrundtypen wie [**CoWaitForMultipleObjects**](/windows/win32/api/combaseapi/nf-combaseapi-cowaitformultipleobjects) unterscheidet sich je nachdem, ob es sich um einen ASTA oder einen STA handelt.
-   Die gebundene Schleife eines COM-Aufrufs wird in einem ASTA anders ausgeführt. Solange ein ausgehender Aufruf aktiv ist, können Sie keine Aufrufe ohne entsprechenden Bezug mehr empfangen. Beispielsweise führt das folgende Verhalten zu einer ASTA-Sperre (und zum unmittelbaren Absturz der App):
    1.  Das ASTA ruft ein MTA-Objekt auf und übergibt den Schnittstellenzeiger P1.
    2.  Dasselbe MTA-Objekt wird später vom ASTA aufgerufen. Das MTA-Objekt ruft P1 vor der Rückgabe an das ASTA auf.
    3.  Der Schnittstellenzeiger P1 kann aufgrund der Blockierung durch einen Aufruf ohne Bezug nicht auf das ASTA zugreifen. Der MTA-Thread wird jedoch bei dem Versuch, P1 aufzurufen, blockiert.

    Sie können dieses Problem wie folgt beheben:
    -   Verwenden Sie das **async**-Muster, das in der Parallel Patterns Library (PPLTasks.h) definiert wurde.
    -   Rufen Sie [**CoreDispatcher::ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents) im ASTA Ihrer App (dem Hauptthread Ihrer App) so früh wie möglich auf, um zufällige Aufrufe zuzulassen.

    Sie können sich daher nicht auf die unmittelbare Übermittlung von Aufrufen ohne Bezug zum ASTA Ihrer App verlassen. Weitere Informationen zu Async-aufrufen finden Sie unter [asynchrone Programmierung in C++](../threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps.md).

Im Allgemeinen sollten Sie bei der Entwicklung Ihrer UWP-App den [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) für [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) und [**CoreDispatcher::ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents) Ihrer App verwenden, um alle UI-Threads zu behandeln, anstatt eigene MTA-Threads zu erstellen und zu verwalten. Wenn Sie einen separaten Thread benötigen, der nicht mit dem **CoreDispatcher** behandelt werden kann, verwenden Sie asynchrone Muster, und beachten Sie die bereits erwähnten Ratschläge zur Vermeidung von Problemen mit Einstiegsinvarianzen.