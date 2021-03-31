---
title: Einrichten des Spieleprojekts
description: Der erste Schritt bei der Entwicklung Ihres Spiels ist das Einrichten eines Projekts in Microsoft Visual Studio. Nachdem Sie ein Projekt speziell für die Spieleentwicklung konfiguriert haben, können Sie es später als Vorlage wieder verwenden.
ms.assetid: 9fde90b3-bf79-bcb3-03b6-d38ab85803f2
ms.date: 06/24/2020
ms.topic: article
keywords: Windows 10, UWP, Games, Setup, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 553069897cc71b45c34159749b7c16bd0af73c7b
ms.sourcegitcommit: 249100d990cd5cf2854c59fa66803b7f83d5db96
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105939095"
---
# <a name="set-up-the-game-project"></a>Einrichten des Spieleprojekts

> [!NOTE]
> Dieses Thema ist Teil der Tutorial-Reihe zum [Erstellen eines einfachen universelle Windows-Plattform (UWP) mit DirectX](tutorial--create-your-first-uwp-directx-game.md) . Das Thema unter diesem Link legt den Kontext für die Reihe fest.

Der erste Schritt bei der Entwicklung Ihres Spiels ist das Erstellen eines Projekts in Microsoft Visual Studio. Nachdem Sie ein Projekt speziell für die Spieleentwicklung konfiguriert haben, können Sie es später als Vorlage wieder verwenden.

## <a name="objectives"></a>Ziele

* Erstellen Sie ein neues Projekt in Visual Studio mithilfe einer Projektvorlage.
* Verstehen Sie den Einstiegspunkt und die Initialisierung des Spiels, indem Sie die Quelldatei für die **App** -Klasse untersuchen.
* Sehen Sie sich die Spiel Schleife an.
* Überprüfen Sie die Datei " **Package. appxmanifest** " des Projekts.

## <a name="create-a-new-project-in-visual-studio"></a>Erstellen eines neuen Projekts in Visual Studio

> [!NOTE]
> Informationen zum Einrichten von Visual Studio für die C++/WinRT-Entwicklung&mdash;einschließlich Installieren und Verwenden der C++/WinRT Visual Studio-Erweiterung (VSIX) und des NuGet-Pakets (die zusammen die Projektvorlage und Buildunterstützung bereitstellen)&mdash; finden Sie unter [Visual Studio-Unterstützung für C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Installieren Sie zunächst die neueste Version von C++/WinRT Visual Studio-Erweiterung (VSIX), und installieren Sie Sie. Siehe den obigen Hinweis. Erstellen Sie dann in Visual Studio ein neues Projekt, das auf der Projektvorlage **Core app (C++/WinRT)** basiert. Die neueste allgemein verfügbare Version von Windows SDK (d. h. keine Vorschauversion).

## <a name="review-the-app-class-to-understand-iframeworkviewsource-and-iframeworkview"></a>Überprüfen Sie die **App** -Klasse, um **iframeworkviewsource** und **iframeworkview** zu verstehen.

Öffnen Sie in Ihrem Core-App-Projekt die Quell Code Datei `App.cpp` . In gibt es die Implementierung der **App** -Klasse, die die APP und ihren Lebenszyklus darstellt. In diesem Fall wissen wir natürlich, dass die APP ein Spiel ist. Wir werden jedoch als *App* bezeichnet, um im Allgemeinen zu erfahren, wie eine universelle Windows-Plattform-app (UWP) initialisiert wird.

### <a name="the-wwinmain-function"></a>Die wWinMain-Funktion

Die **wWinMain** -Funktion ist der Einstiegspunkt für die app. So sieht **wWinMain** aus (aus `App.cpp` ).

```cppwinrt
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(winrt::make<App>());
}
```

Wir erstellen eine Instanz der **App** -Klasse (Hierbei handelt es sich um die einzige Instanz der erstellten **App** ), und wir übergeben diese an die statische [**coreapplication. Run**](/uwp/api/windows.applicationmodel.core.coreapplication.run) -Methode. Beachten Sie, dass **coreapplication. Run** eine [**iframeworkviewsource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) -Schnittstelle erwartet. Daher muss die **App** -Klasse diese Schnittstelle implementieren.

In den nächsten beiden Abschnitten dieses Themas werden die Schnittstellen [**iframeworkviewsource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) und [**iframeworkview**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) beschrieben. Diese Schnittstellen (und **coreapplication. Run**) stellen eine Methode dar, mit der Ihre APP Windows mit einem *Ansichts Anbieter* bereitstellen kann. Windows verwendet diesen Ansichts Anbieter zum Verbinden Ihrer APP mit der Windows-Shell, damit Sie Ereignisse des Anwendungslebenszyklus behandeln können.

### <a name="the-iframeworkviewsource-interface"></a>Die iframeworkviewsource-Schnittstelle

Die **App** -Klasse implementiert [**iframeworkviewsource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource)tatsächlich, wie Sie in der folgenden Liste sehen können.

```cppwinrt
struct App : winrt::implements<App, IFrameworkViewSource, IFrameworkView>
{
    ...
    IFrameworkView CreateView()
    {
        return *this;
    }
    ...
}
```

Ein Objekt, das **iframeworkviewsource** implementiert, ist ein *Ansichts Anbieter-Factory* -Objekt. Die Aufgabe dieses Objekts besteht darin, ein *Ansichts Anbieter* Objekt zu produzieren und zurückzugeben.

**Iframeworkviewsource** verfügt über die einzige Methode [**iframeworkviewsource:: up view**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource.createview). Windows ruft diese Funktion für das-Objekt auf, das Sie an **coreapplication. Run** übergeben. Wie Sie oben sehen können, gibt die **App:: up view** -Implementierung dieser Methode zurück `*this` . Anders ausgedrückt: das **App** -Objekt gibt sich selbst zurück. Da **iframeworkviewsource:: aufgabenview** den Rückgabe Werttyp [**iframeworkview**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource)aufweist, muss die **App** -Klasse *diese* Schnittstelle ebenfalls implementieren. Und Sie können sehen, dass dies in der obigen Liste der Fall ist.

### <a name="the-iframeworkview-interface"></a>Die iframeworkview-Schnittstelle

Ein Objekt, das [**iframeworkview**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) implementiert, ist ein *Ansichts Anbieter* Objekt. Wir haben jetzt Windows mit diesem Ansichts Anbieter bereitgestellt. Es ist das gleiche **App** -Objekt, das wir in **wWinMain** erstellt haben. Daher fungiert die **App** -Klasse sowohl als *Ansichts Anbieterfactory* als auch als *Ansichts Anbieter*.

Jetzt kann Windows die Implementierungen der **App** -Klasse der Methoden von **iframeworkview** aufzurufen. In den Implementierungen dieser Methoden hat Ihre APP die Möglichkeit, Aufgaben auszuführen, wie z. b. die Initialisierung, das Laden der benötigten Ressourcen, das Herstellen einer Verbindung mit den entsprechenden Ereignis Handlern und das Empfangen von [**corewindow**](/uwp/api/windows.ui.core.corewindow) , das Ihre APP zum Anzeigen der Ausgabe verwendet.

Ihre Implementierungen der Methoden von **iframeworkview** werden in der unten gezeigten Reihenfolge aufgerufen.

- [**Initialisieren**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize)
- [**SetWindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow)
- [**Laden**](/uwp/api/windows.applicationmodel.core.iframeworkview.load)
- Das [**coreapplicationview:: aktivierte**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) -Ereignis wird ausgelöst. Wenn Sie (optional) registriert haben, um dieses Ereignis zu behandeln, wird der **onaktivierte** Handler zu diesem Zeitpunkt aufgerufen.
- [**Ausführen**](/uwp/api/windows.applicationmodel.core.iframeworkview.run)
- [**Uninitialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize)

Und hier ist das Gerüst der **App** -Klasse (in `App.cpp` ), die die Signaturen dieser Methoden anzeigt.

```cppwinrt
struct App : winrt::implements<App, IFrameworkViewSource, IFrameworkView>
{
    ...
    void Initialize(Windows::ApplicationModel::Core::CoreApplicationView const& applicationView) { ... }
    void SetWindow(Windows::UI::Core::CoreWindow const& window) { ... }
    void Load(winrt::hstring const& entryPoint) { ... }
    void OnActivated(
        Windows::ApplicationModel::Core::CoreApplicationView const& applicationView,
        Windows::ApplicationModel::Activation::IActivatedEventArgs const& args) { ... }
    void Run() { ... }
    void Uninitialize() { ... }
    ...
}
```

Dies war nur eine Einführung in **iframeworkview**. In [definieren Sie das UWP-App-Framework des Spiels](tutorial--building-the-games-uwp-app-framework.md)ausführlich mit diesen Methoden und deren Implementierung.

### <a name="tidy-up-the-project"></a>Bereinigen des Projekts

Das Core-App-Projekt, das Sie aus der Projektvorlage erstellt haben, enthält Funktionen, die Sie an diesem Punkt Aufräumen sollten. Danach können wir das Projekt zum erneuten Erstellen des Spiels "Shooting Gallery" (**Simple3DGameDX**) verwenden. Nehmen Sie die folgenden Änderungen an der **App** -Klasse in vor `App.cpp` .

- Löschen Sie die zugehörigen Datenmember.
- " **Onpointerpressed**", " **onpointerpressed**" und " **addvisual** " Löschen
- Löschen Sie den Code aus **SetWindow**.

Das Projekt wird erstellt und ausgeführt, aber es wird nur eine voll Tonfarbe im Client Bereich angezeigt.

## <a name="the-game-loop"></a>Die Spielschleife

Um eine Vorstellung davon zu erhalten, wie eine Spiel Schleife aussieht, sehen Sie sich den Quellcode für das [Simple3DGameDX](/samples/microsoft/windows-universal-samples/simple3dgamedx/) -Beispiel Spiel an, das Sie heruntergeladen haben.

Die **App** -Klasse verfügt über einen Datenmember mit dem Namen *m_main* vom Typ **gamemain**. Dieser Member wird in **App:: Run** wie diesem verwendet.

```cppwinrt
void Run()
{
    m_main->Run();
}
```

Sie finden **gamemain:: Run** in `GameMain.cpp` . Dabei handelt es sich um die Hauptschleife des Spiels, und hier finden Sie einen sehr groben Überblick über die wichtigsten Features.

```cppwinrt
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
            Update();
            m_renderer->Render();
            m_deviceResources->Present();
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

Im folgenden finden Sie eine kurze Beschreibung der Funktionsweise dieser Hauptspiel Schleife.

Wenn das Fenster für das Spiel nicht geschlossen ist, verteilen Sie alle Ereignisse, aktualisieren Sie den Timer, und zeigen Sie die Ergebnisse der Grafik Pipeline an. Es gibt noch viele weitere Informationen über diese Probleme, und dies wird in den Themen [Definieren des UWP-App-Frameworks von spielen](tutorial--building-the-games-uwp-app-framework.md), [Rendern von Framework I: Einführung in Rendering](tutorial--assembling-the-rendering-pipeline.md)und [Rendering Framework II: Game Rendering](tutorial-game-rendering.md). Dies ist jedoch die grundlegende Code Struktur eines UWP DirectX-Spiels.

## <a name="review-and-update-the-packageappxmanifest-file"></a>Überprüfen und aktualisieren Sie die Datei "Package. appxmanifest".

Die Datei " **Package. appxmanifest** " enthält Metadaten zu einem UWP-Projekt. Diese Metadaten werden zum Verpacken und Starten Ihres Spiels sowie für die Übermittlung an die Microsoft Store verwendet. Die Datei enthält auch wichtige Informationen, die das System des Players verwendet, um den Zugriff auf die Systemressourcen zu ermöglichen, die das Spiel ausführen muss.

Starten Sie den **Manifest-Designer** , indem Sie in **Projektmappen-Explorer** auf die Datei " **Package. appxmanifest** " doppelklicken.

![Screenshot des Paket. AppX-Manifest-Editors.](images/simple-dx-game-setup-app-manifest.png)

Weitere Informationen zur Datei **package.appxmanifest** und zum Packen finden Sie unter [Manifest-Designer](/previous-versions/br230259(v=vs.140)). Sehen Sie sich nun die Registerkarte " **Funktionen** " an, und sehen Sie sich die bereitgestellten Optionen an.

![Screenshot mit den Standardfunktionen einer Direct3D-app.](images/simple-dx-game-setup-capabilities.png)

Wenn Sie die Funktionen, die Ihr Spiel verwendet, nicht auswählen, z. b. den Zugriff auf das **Internet** für Global High Score Board, sind Sie nicht in der Lage, auf die entsprechenden Ressourcen und Features zuzugreifen. Wenn Sie ein neues Spiel erstellen, stellen Sie sicher, dass Sie alle Funktionen auswählen, die von den von Ihrem Spiel benötigten APIs benötigt werden.

Sehen wir uns nun die restlichen Dateien an, die mit der Vorlage **DirectX 11-app (universelle Windows-APP)** geliefert werden.

## <a name="review-other-important-libraries-and-source-code-files"></a>Andere wichtige Bibliotheken und Quell Code Dateien überprüfen

Wenn Sie beabsichtigen, eine Art von Spiel Projektvorlage für sich selbst zu erstellen, sodass Sie diese als Ausgangspunkt für zukünftige Projekte wieder verwenden können, sollten Sie `GameMain.h` `GameMain.cpp` das [Simple3DGameDX](/samples/microsoft/windows-universal-samples/simple3dgamedx/) -Projekt, das Sie heruntergeladen haben, kopieren und aus dem neuen Core-App-Projekt hinzufügen. Überprüfen Sie diese Dateien, erfahren Sie, was Sie tun, und entfernen Sie alles, was für **Simple3DGameDX** spezifisch ist. Kommentieren Sie auch alle Elemente aus, die von Code abhängen, den Sie noch nicht kopiert haben. Ein Beispiel hierfür `GameMain.h` hängt von ab `GameRenderer.h` . Sie können die Auskommentierung aufheben, wenn Sie weitere Dateien aus **Simple3DGameDX** kopieren.

Im folgenden finden Sie eine kurze Übersicht über einige der Dateien in **Simple3DGameDX** , die Sie für Ihre Vorlage nützlich finden, wenn Sie eine erstellen. In jedem Fall sind diese gleichermaßen wichtig, um zu verstehen, wie **Simple3DGameDX** selbst funktioniert.

|Quelldatei|Dateiordner|BESCHREIBUNG|
|------------------------------|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|Deviceresources. h/. cpp|Versorgungsunternehmen|Definiert die **deviceresources** -Klasse, mit der alle DirectX- [Geräte Ressourcen](tutorial--assembling-the-rendering-pipeline.md#resource)gesteuert werden. Definiert auch die **idevicenotify** -Schnittstelle, die verwendet wird, um Ihre Anwendung zu benachrichtigen, dass das Grafikadapter Gerät verloren gegangen ist oder neu erstellt wurde.|
|DirectXSample.h|Versorgungsunternehmen|Implementiert Hilfsfunktionen, z. **b. convertdipstopixels**. **Convertdipstopixels** konvertiert eine Länge in geräteunabhängigen Pixeln (Dips) in eine Länge in physischen Pixeln.|
|Gametimer. h/. cpp|Versorgungsunternehmen|Definiert einen Timer mit hoher Auflösung, der sich besonders für Spiele oder Apps mit interaktivem Rendering eignet.|
|GameRenderer.h/.cpp|Darstellung|Definiert die **gamerenderer** -Klasse, die eine einfache Renderingpipeline implementiert.|
|Gamehud. h/. cpp|Darstellung|Definiert eine Klasse zum Rendering einer Heads-Up-Anzeige (HUD) für das Spiel mithilfe von Direct2D und DirectWrite.|
|Vertexshader. HLSL und vertexshaderflat. HLSL|Shader|Enthält den HLSL-Code (High-Level Shader Language) für grundlegende Scheitelpunkt-Shader.|
|Pixelshader. HLSL und pixelshaderflat. HLSL|Shader|Enthält den HLSL-Code (High-Level Shader Language) für grundlegende Pixel-Shader.|
|Constantbuffers. hlsli|Shader|Enthält Datenstruktur Definitionen für konstante Puffer und shaderstrukturen, die verwendet werden, um MVP-Matrizen (Model-View-Projection) und pro-Vertex-Daten an den Vertex-Shader zu übergeben.|
|pch.h/.cpp|–|Enthält allgemeine C++/WinRT-, Windows-und DirectX-includes.|

### <a name="next-steps"></a>Nächste Schritte

An dieser Stelle haben wir gezeigt, wie Sie ein neues UWP-Projekt für ein DirectX-Spiel erstellen, sich einige der darin aufgeführten Teile ansehen und überlegen, wie Sie dieses Projekt in eine wiederverwendbare Vorlage für Spiele umwandeln können. Wir haben auch einige wichtige Bestandteile des **Simple3DGameDX** -Beispiel Spiels untersucht.

Im nächsten Abschnitt wird [das UWP-App-Framework des Spiels definiert](tutorial--building-the-games-uwp-app-framework.md). Hier sehen wir uns genauer an, wie **Simple3DGameDX** funktioniert.