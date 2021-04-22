---
title: Einrichten des Spieleprojekts
description: Der erste Schritt bei der Entwicklung Ihres Spiels besteht im Einrichten eines Projekts in Microsoft Visual Studio. Nachdem Sie ein Projekt speziell für die Spieleentwicklung konfiguriert haben, können Sie es später wieder als eine Art Vorlage verwenden.
ms.assetid: 9fde90b3-bf79-bcb3-03b6-d38ab85803f2
ms.date: 06/24/2020
ms.topic: article
keywords: Windows 10, UWP, Spiele, Setup, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: c192a81208c7cf372e27d9fb30233f8391939688
ms.sourcegitcommit: 73ec979ce6b9701e7135fd0541bf932b0847908e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107881703"
---
# <a name="set-up-the-game-project"></a>Einrichten des Spieleprojekts

> [!NOTE]
> Dieses Thema ist Teil der Tutorialreihe Erstellen eines einfachen Universelle Windows-Plattform [(UWP) mit DirectX.](tutorial--create-your-first-uwp-directx-game.md) Das Thema unter diesem Link legt den Kontext für die Reihe fest.

Der erste Schritt bei der Entwicklung Ihres Spiels besteht im Erstellen eines Projekts in Microsoft Visual Studio. Nachdem Sie ein Projekt speziell für die Spieleentwicklung konfiguriert haben, können Sie es später wieder als eine Art Vorlage verwenden.

## <a name="objectives"></a>Ziele

* Erstellen Sie ein neues Projekt in Visual Studio mithilfe einer Projektvorlage.
* Verstehen Sie den Einstiegspunkt und die Initialisierung des Spiels, indem Sie die Quelldatei für die **App-Klasse** untersuchen.
* Sehen Sie sich die Spielschleife an.
* Überprüfen Sie die Datei **package.appxmanifest des** Projekts.

## <a name="create-a-new-project-in-visual-studio"></a>Erstellen eines neuen Projekts in Visual Studio

> [!NOTE]
> Informationen zum Einrichten von Visual Studio für die C++/WinRT-Entwicklung&mdash;einschließlich Installieren und Verwenden der C++/WinRT Visual Studio-Erweiterung (VSIX) und des NuGet-Pakets (die zusammen die Projektvorlage und Buildunterstützung bereitstellen)&mdash; finden Sie unter [Visual Studio-Unterstützung für C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Installieren Sie zuerst die neueste Version der C++/WinRT-Visual Studio-Erweiterung (VSIX), oder aktualisieren Sie sie auf . siehe Hinweis oben. Erstellen Sie dann in Visual Studio ein neues Projekt basierend auf der **Projektvorlage Core App (C++/WinRT).** Die neueste allgemein verfügbare Version von Windows SDK (d. h. keine Vorschauversion).

## <a name="review-the-app-class-to-understand-iframeworkviewsource-and-iframeworkview"></a>Lesen Sie die **App-Klasse,** um **IFrameworkViewSource und** **IFrameworkView zu verstehen.**

Öffnen Sie in Ihrem Core-App-Projekt die Quellcodedatei `App.cpp` . In gibt es die Implementierung der **App-Klasse,** die die App und ihren Lebenszyklus darstellt. In diesem Fall wissen wir natürlich, dass die App ein Spiel ist. Wir bezeichnen sie jedoch als  App, um allgemein darüber zu sprechen, wie eine Universelle Windows-Plattform-App (UWP) initialisiert wird.

### <a name="the-wwinmain-function"></a>Die wWinMain-Funktion

Die **wWinMain-Funktion** ist der Einstiegspunkt für die App. **wWinMain** sieht wie folgt aus (von `App.cpp` ).

```cppwinrt
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(winrt::make<App>());
}
```

Wir erstellen eine Instanz der **App-Klasse** (dies ist die einzige Instanz von **App,** die erstellt wurde), und übergeben diese an die statische [**CoreApplication.Run-Methode.**](/uwp/api/windows.applicationmodel.core.coreapplication.run) Beachten Sie, dass **CoreApplication.Run** eine [**IFrameworkViewSource-Schnittstelle**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) erwartet. Daher muss die **App-Klasse** diese Schnittstelle implementieren.

In den nächsten beiden Abschnitten dieses Themas werden die Schnittstellen [**IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) und [**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) beschrieben. Diese Schnittstellen (sowie **CoreApplication.Run**) stellen eine Möglichkeit dar, mit der Ihre App Windows mit einem *Ansichtsanbieter* bereitstellen kann. Windows verwendet diesen Ansichtsanbieter, um Ihre App mit der Windows-Shell zu verbinden, sodass Sie Anwendungslebenszyklusereignisse verarbeiten können.

### <a name="the-iframeworkviewsource-interface"></a>Die IFrameworkViewSource-Schnittstelle

Die **App-Klasse** implementiert tatsächlich [**IFrameworkViewSource,**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource)wie Sie in der folgenden Auflistung sehen können.

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

Ein Objekt, das **IFrameworkViewSource** implementiert, ist ein *Ansichtsanbieterfactoryobjekt.* Die Aufgabe dieses Objekts besteht darin, ein *Ansichtsanbieterobjekt* herzustellen und zurückzugeben.

**IFrameworkViewSource** verfügt über die einzelne Methode [**IFrameworkViewSource::CreateView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource.createview). Windows ruft diese Funktion für das Objekt auf, das Sie an **CoreApplication.Run** übergeben. Wie Sie oben sehen können, gibt die **App::CreateView-Implementierung** dieser Methode `*this` zurück. Anders ausgedrückt: Das **App-Objekt** gibt sich selbst zurück. Da **IFrameworkViewSource::CreateView** über den Rückgabewerttyp [**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource)verfügt, muss die **App-Klasse** *diese* Schnittstelle ebenfalls implementieren. Dies können Sie auch in der obigen Auflistung sehen.

### <a name="the-iframeworkview-interface"></a>Die IFrameworkView-Schnittstelle

Ein Objekt, das [**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) implementiert, ist ein *Sichtanbieterobjekt.* Nun haben wir Windows mit diesem Ansichtsanbieter bereitgestellt. Es ist das gleiche **App-Objekt,** das wir in **wWinMain erstellt haben.** Daher dient **die App-Klasse** sowohl als *Ansichtsanbieter-Factory als* auch *als Ansichtsanbieter.*

Windows kann nun die **Implementierungen** der Methoden von **IFrameworkView der App-Klasse aufrufen.** In den Implementierungen dieser Methoden hat Ihre App die Möglichkeit, Aufgaben wie die Initialisierung auszuführen, mit dem Laden der benötigten Ressourcen zu beginnen, die entsprechenden Ereignishandler zu verbinden und das [**CoreWindow**](/uwp/api/windows.ui.core.corewindow) zu empfangen, das Ihre App zum Anzeigen ihrer Ausgabe verwendet.

Ihre Implementierungen der Methoden von **IFrameworkView** werden in der unten gezeigten Reihenfolge aufgerufen.

- [**Initialisieren**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize)
- [**SetWindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow)
- [**Laden**](/uwp/api/windows.applicationmodel.core.iframeworkview.load)
- Das [**CoreApplicationView::Activated-Ereignis**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) wird ausgelöst. Wenn Sie sich also (optional) für die Behandlung dieses Ereignisses registriert haben, wird ihr **OnActivated-Handler** zu diesem Zeitpunkt aufgerufen.
- [**Ausführen**](/uwp/api/windows.applicationmodel.core.iframeworkview.run)
- [**Nicht initialisieren**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize)

Hier sehen Sie das Gerüst der **App-Klasse** (in ), das `App.cpp` die Signaturen dieser Methoden zeigt.

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

Dies war nur eine Einführung in **IFrameworkView.** Unter Definieren des UWP-App-Frameworks des Spiels erfahren Sie mehr über diese Methoden und deren [Implementierung.](tutorial--building-the-games-uwp-app-framework.md)

### <a name="tidy-up-the-project"></a>Aufräumen des Projekts

Das Core App-Projekt, das Sie aus der Projektvorlage erstellt haben, enthält Funktionen, die wir an diesem Punkt aufräumen sollten. Anschließend können wir das Projekt verwenden, um das Spiel mit dem Spielekatalog **(Simple3DGameDX) neu zu erstellen.** Nehmen Sie die folgenden Änderungen an der **App-Klasse** in `App.cpp` vor.

- Löschen Sie die Datenmitglieder.
- Löschen **von OnPointerPressed,** **OnPointerMoved** und **AddVisual**
- Löschen Sie den Code aus **SetWindow**.

Das Projekt wird erstellen und ausgeführt, aber es wird nur eine Volltonfarbe im Clientbereich angezeigt.

## <a name="the-game-loop"></a>Die Spielschleife

Um sich einen Überblick darüber zu verschaffen, wie eine Spielschleife aussieht, sehen Sie sich den Quellcode für das [Simple3DGameDX-Beispielspiel](/samples/microsoft/windows-universal-samples/simple3dgamedx/) an, das Sie heruntergeladen haben.

Die **App-Klasse** verfügt über einen Datenmember vom Typ **GameMain** mit dem Namen *m_main*. Und dieses Element wird in **App::Run** wie folgt verwendet.

```cppwinrt
void Run()
{
    m_main->Run();
}
```

Sie finden **GameMain::Run** in `GameMain.cpp` . Es ist die Hauptschleife des Spiels, und hier sehen Sie eine sehr grobe Gliederung, die die wichtigsten Features zeigt.

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

Im Folgenden finden Sie eine kurze Beschreibung der Aufgaben dieser Hauptspielschleife.

Wenn das Fenster für Ihr Spiel nicht geschlossen ist, senden Sie alle Ereignisse, aktualisieren Sie den Timer, und rendern Und zeigen Sie dann die Ergebnisse der Grafikpipeline an. Es gibt noch viel mehr zu diesen Problemen zu sagen, und wir werden dies in den Themen [Definieren des UWP-App-Frameworks des Spiels,](tutorial--building-the-games-uwp-app-framework.md) [Renderingframework I: Einführung zum Rendern](tutorial--assembling-the-rendering-pipeline.md)und [Renderingframework II: Rendern von Spielen](tutorial-game-rendering.md)tun. Dies ist jedoch die grundlegende Codestruktur eines UWP DirectX-Spiels.

## <a name="review-and-update-the-packageappxmanifest-file"></a>Überprüfen und Aktualisieren der Datei package.appxmanifest

Die Datei **Package.appxmanifest** enthält Metadaten zu einem UWP-Projekt. Diese Metadaten werden zum Packen und Starten Ihres Spiels und zur Übermittlung an die Microsoft Store verwendet. Die Datei enthält auch wichtige Informationen, die das System des Spielers verwendet, um Zugriff auf die Systemressourcen zu gewähren, die das Spiel ausführen muss.

Starten Sie den **Manifest-Designer,** indem Sie **in** Projektmappen-Explorer auf die Datei **Package.appxmanifest** doppelklicken.

![Screenshot des Manifest-Editors "package.appx".](images/simple-dx-game-setup-app-manifest.png)

Weitere Informationen zur Datei **package.appxmanifest** und zum Packen finden Sie unter [Manifest-Designer](/previous-versions/br230259(v=vs.140)). Sehen Sie sich vorerst die Registerkarte **Funktionen** an, und sehen Sie sich die bereitgestellten Optionen an.

![Screenshot mit den Standardfunktionen einer direct3d-App](images/simple-dx-game-setup-capabilities.png)

Wenn Sie nicht die Funktionen auswählen, die Ihr Spiel verwendet, z. B. den Zugriff auf das **Internet** für ein globales High Score Board, können Sie nicht auf die entsprechenden Ressourcen oder Features zugreifen. Wenn Sie ein neues Spiel erstellen, stellen Sie sicher, dass Sie alle Funktionen auswählen, die von APIs benötigt werden, die Ihr Spiel aufruft.

Sehen wir uns nun die restlichen Dateien an, die im **Simple3DGameDX-Beispielspiel** enthalten sind.

## <a name="review-other-important-libraries-and-source-code-files"></a>Überprüfen anderer wichtiger Bibliotheken und Quellcodedateien

Wenn Sie beabsichtigen, eine Art Spielprojektvorlage für sich selbst zu erstellen, damit Sie diese als Ausgangspunkt für zukünftige Projekte wiederverarbeiten können, sollten Sie das `GameMain.h` `GameMain.cpp` [heruntergeladene Simple3DGameDX-Projekt](/samples/microsoft/windows-universal-samples/simple3dgamedx/) kopieren und aus diesem heraus kopieren und ihrem neuen Core-App-Projekt hinzufügen. Untersuchen Sie diese Dateien, erfahren Sie, was sie tun, und entfernen Sie alles, was für **Simple3DGameDX spezifisch ist.** Kommentieren Sie auch alles aus, das von Code abhängt, den Sie noch nicht kopiert haben. Als Beispiel hängt von `GameMain.h` `GameRenderer.h` ab. Sie können die Auskommentierung entfernen, wenn Sie weitere Dateien aus **Simple3DGameDX kopieren.**

Hier ist eine kurze Übersicht über einige der Dateien in **Simple3DGameDX,** die Sie in Ihre Vorlage einlesen können, wenn Sie eine erstellen. In jedem Fall sind diese gleichermaßen wichtig, um zu verstehen, wie **Simple3DGameDX selbst** funktioniert.

|Quelldatei|Dateiordner|BESCHREIBUNG|
|------------------------------|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|DeviceResources.h/.cpp|Versorgungsunternehmen|Definiert die **DeviceResources-Klasse,** die alle DirectX-Geräteressourcen [steuert.](tutorial--assembling-the-rendering-pipeline.md#resource) Definiert auch die **IDeviceNotify-Schnittstelle,** die verwendet wird, um Ihre Anwendung darüber zu benachrichtigen, dass das Grafikadaptergerät verloren gegangen ist oder neu erstellt wurde.|
|DirectXSample.h|Versorgungsunternehmen|Implementiert Hilfsfunktionen wie **ConvertDipsToPixels.** **ConvertDipsToPixels** konvertiert eine Länge in geräteunabhängigen Pixeln (DEVICE-Independent Pixels, DIPs) in eine Länge in physischen Pixeln.|
|GameTimer.h/.cpp|Versorgungsunternehmen|Definiert einen Timer mit hoher Auflösung, der sich besonders für Spiele oder Apps mit interaktivem Rendering eignet.|
|GameRenderer.h/.cpp|Darstellung|Definiert die **GameRenderer-Klasse,** die eine einfache Renderingpipeline implementiert.|
|GameHud.h/.cpp|Darstellung|Definiert eine Klasse zum Rendern einer Kopf-auf-Anzeige (HUD) für das Spiel mit Direct2D und DirectWrite.|
|VertexShader.hlsl und VertexShaderFlat.hlsl|Shader|Enthält den HLSL-Code (High-Level Shader Language) für einfache Vertex-Shader.|
|PixelShader.hlsl und PixelShaderFlat.hlsl|Shader|Enthält den HLSL-Code (High-Level Shader Language) für einfache Pixel-Shader.|
|ConstantBuffers.hlsli|Shader|Enthält Datenstrukturdefinitionen für konstante Puffer und Shaderstrukturen, die zum Übergeben von MVP-Matrizen (Model View-Projection) und Daten pro Scheitelpunkt an den Vertex-Shader verwendet werden.|
|pch.h/.cpp|NICHT ZUTREFFEND|Enthält allgemeine C++/WinRT-, Windows- und DirectX-Includes.|

### <a name="next-steps"></a>Nächste Schritte

An diesem Punkt haben wir gezeigt, wie sie ein neues UWP-Projekt für ein DirectX-Spiel erstellen, einige der darin enthaltenen Teile betrachten und darüber nachdenken, wie dieses Projekt in eine Art wiederverwendbare Vorlage für Spiele umgewandelt werden kann. Wir haben uns auch einige der wichtigen Teile des **Simple3DGameDX-Beispielspiels** angesehen.

Der nächste Abschnitt ist [Definieren des UWP-App-Frameworks des Spiels.](tutorial--building-the-games-uwp-app-framework.md) Dort sehen wir uns genauer an, wie **Simple3DGameDX** funktioniert.