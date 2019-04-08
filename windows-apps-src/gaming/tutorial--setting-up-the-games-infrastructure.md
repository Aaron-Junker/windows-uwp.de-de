---
title: Einrichten des Spieleprojekts
description: Im ersten Schritt für die Erstellung Ihres Spiels richten Sie ein Projekt in Microsoft Visual Studio so ein, dass Sie möglichst wenig Aufwand mit der Bearbeitung der Codeinfrastruktur haben.
ms.assetid: 9fde90b3-bf79-bcb3-03b6-d38ab85803f2
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Einrichtung, directx
ms.localizationpriority: medium
ms.openlocfilehash: 252d7ccb8e50e773a19282afaf19bb18d4c5d5a6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608705"
---
# <a name="set-up-the-game-project"></a>Einrichten des Spieleprojekts

Dieses Thema erläutert das Einrichten eines einfachen UWP-DirectX-Spiels mit den Vorlagen in Visual Studio. Im ersten Schritt für die Erstellung Ihres Spiels richten Sie ein Projekt in Microsoft Visual Studio so ein, dass Sie möglichst wenig Aufwand mit der Bearbeitung der Codeinfrastruktur haben. Lernen Sie, sich eine Menge Zeit und Arbeit ersparen, wenn Sie die richtige Vorlage verwenden und das Projekt speziell für die Spieleentwicklung konfigurieren.

## <a name="objectives"></a>Ziele

* Richten Sie ein Direct3D-Spieleprojekt in Visual Studio mit einer Vorlage ein.
* Verstehen Sie den Haupteinstiegspunkt des Spiels durch Untersuchen der **App**-Quelldateien
* Prüfen Sie die Datei **package.appxmanifest**
* Erfahren Sie, welche Spieleentwicklungstools und Support im Projekt enthalten sind.

## <a name="how-to-set-up-the-game-project"></a>Einrichten des Spieleprojekts

Wenn Sie Erfahrung mit der Entwicklung von UWP haben, empfehlen wir die Verwendung von Vorlagen in Visual Studio, um die grundlegende Codestruktur einzurichten.

>[!Note]
>Dieser Artikel ist Teil einer Lernprogrammreihe basierend auf einem Beispiel für ein Spiel. Sie erhalten den neuesten Code unter [Direct3D-Spielbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Dieses Beispiel gehört zu einer großen Sammlung von UWP-Featurebeispielen. Anweisungen zum Herunterladen des Beispiels finden Sie unter [Abrufen der UWP-Beispiele von GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

### <a name="use-directx-template-to-create-a-project"></a>Verwenden Sie DirectX-Vorlagen, um ein Projekt zu erstellen

Eine Visual Studio-Vorlage ist eine Sammlung von Einstellungen und Codedateien, die abhängig von der bevorzugten Sprache und Technologie auf eine bestimmte Art von App ausgerichtet sind. In Microsoft Visual Studio 2017 finden Sie eine Reihe von Vorlagen, die erheblich Spiele- und Grafik-app-Entwicklung vereinfachen können. Ohne Verwendung einer Vorlage müssen Sie einen Großteil des grundlegenden Rendering- und Anzeigeframeworks für die Grafik selbst entwickeln, was insbesondere für neue Spieleentwickler recht mühsam sein kann.

Die richtige Vorlage für dieses Tutorial ist die Vorlage **DirectX 11 App (Universal Windows)**. 

Schritte zum Erstellen eines DirectX 11-Spieleprojekts in Visual Studio 11:
1.  Wählen Sie **Datei...** &gt; **Neue** &gt; **Projekt...**
2.  Wählen Sie im linken Bereich **installiert** &gt; **Vorlagen** &gt; **Visual C++** &gt; **Windows Universal**
3.  Wählen Sie im mittleren Bereich **DirectX 11-App (universelle Windows-App)** aus
4.  Geben Sie Ihrem Spieleprojekt einen Namen, und klicken Sie auf **OK**.

![Bildschirmfoto, das zeigt, wie die DirectX11-Vorlage zum Erstellen eines neuen Spieleprojekts ausgewählt wird](images/simple-dx-game-setup-new-project.png)

Diese Vorlage enthält das grundlegende Framework für eine UWP-App, für die DirectX mit C++ verwendet wird. Drücken Sie F5, um die App zu erstellen und auszuführen. Ist das nicht ein hübscher blauer Bildschirm? Mit der Vorlage werden mehrere Codedateien erstellt, die die grundlegenden Funktionen für eine UWP-App mit DirectX und C++ enthalten.

## <a name="review-the-apps-main-entry-point-by-understanding-iframeworkview"></a>Überprüfen Sie den Haupteinstiegspunkt der App, indem Sie IFrameworkView verstehen

Die **App**-Klasse beerbt die **IFrameworkView**-Klasse.

### <a name="inspect-apph"></a>Überprüfen Sie **App.h**.

Wir betrachten kurz, wie die 5-Methoden in **App.h** &mdash; [ **initialisieren**](https://msdn.microsoft.com/library/windows/apps/hh700495), [ **"SetWindow"** ](https://msdn.microsoft.com/library/windows/apps/hh700509), [ **Load**](https://msdn.microsoft.com/library/windows/apps/hh700501), [ **ausführen**](https://msdn.microsoft.com/library/windows/apps/hh700505), und [ **deinitialisieren** ](https://msdn.microsoft.com/library/windows/apps/hh700523) beim Implementieren der [ **"iframeworkview"** ](https://msdn.microsoft.com/library/windows/apps/hh700469) Schnittstelle, die ein sichtanbieter definiert. Diese Methoden werden vom App-Singleton ausgeführt, der beim Spielstart erstellt wird. Sie laden alle vom Spiel benötigten Ressourcen und stellen eine Verbindung zwischen den entsprechenden Ereignishandlern her.

```cpp
    // Main entry point for our app. Connects the app with the Windows shell and handle application lifecycle events.
    ref class App sealed : public Windows::ApplicationModel::Core::IFrameworkView
    {
    public:
        App();

        // IFrameworkView Methods.
        virtual void Initialize(Windows::ApplicationModel::Core::CoreApplicationView^ applicationView);
        virtual void SetWindow(Windows::UI::Core::CoreWindow^ window);
        virtual void Load(Platform::String^ entryPoint);
        virtual void Run();
        virtual void Uninitialize();

    protected:
        ...
    };
```

### <a name="inspect-appcpp"></a>Überprüfen Sie **App.cpp**.

Die **main**-Methode befindet sich in der Quelldatei **App.cpp**:

```cpp
//The main function is only used to initialize our IFrameworkView class.
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}
```

Die Methode erstellt eine Instanz des Direct3D-Ansichtsanbieters aus der Ansichtsanbieterfactory (**Direct3DApplicationSource**, definiert in **App.h**) und übergibt sie zur Ausführung an das App-Singleton ([**CoreApplication::Run**](https://msdn.microsoft.com/library/windows/apps/hh700469)). Das bedeutet, dass sich der Ausgangspunkt für Ihr Spiel im Implementierungscode der [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505)-Methode (in diesem Fall: **App::Run**) befindet. 

Suchen Sie die **App::Run**-Methode in **App.cpp**. Hier ist der Code:

```cpp
//This method is called after the window becomes active.
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

Was hat diese Methode: Wenn das Fenster für Ihr Spiel nicht geschlossen ist, alle Ereignisse sendet, aktualisiert den Timer, klicken Sie dann rendert und präsentiert die Ergebnisse Ihrer Pipeline Grafiken. Beschäftigen wir uns dies ausführlicher [definieren Sie das UWP-app-Framework](tutorial--building-the-games-uwp-app-framework.md), [Rendering-Framework I: Einführung in Rendering](tutorial--assembling-the-rendering-pipeline.md), und [Rendering-Framework II: Rendern von Spielen](tutorial-game-rendering.md). Damit dürften Sie ein gewisses Gespür für die grundlegende Codestruktur eines UWP-DirectX-Spiels entwickelt haben.

## <a name="review-and-update-the-packageappxmanifest-file"></a>Prüfen und Aktualisieren der Datei „package.appxmanifest“


Die Vorlage hat aber noch mehr zu bieten als nur Codedateien. Die Datei **package.appxmanifest** enthält Metadaten für Ihr Projekt. Diese werden zum Packen und Starten des Spiels sowie zum Übermitteln des Spiels an den Microsoft Store verwendet. Darüber hinaus enthält sie wichtige Infos, mit deren Hilfe das System des Spielers den Zugriff auf die zum Ausführen des Spiels benötigten Systemressourcen ermöglicht.

Starten Sie den **Manifest-Designer**. Doppelklicken Sie hierzu im **Projektmappen-Explorer** auf die Datei **package.appxmanifest**.

![Screenshot des Manifest-Editor „package.appx“](images/simple-dx-game-setup-app-manifest.png)

Weitere Informationen zur Datei **package.appxmanifest** und zum Packen finden Sie unter [Manifest-Designer](https://msdn.microsoft.com/library/windows/apps/br230259.aspx). Nun widmen wir uns aber erst einmal der Registerkarte **Funktionen** und den dort zur Verfügung stehenden Optionen.

![Screenshot der Standardfunktionen einer Direct3D-App](images/simple-dx-game-setup-capabilities.png)

Wenn Sie die von Ihrem Spiel genutzten Funktionen (etwa den Zugriff auf das **Internet** für eine globale Bestenliste) nicht auswählen, können Sie nicht auf die entsprechenden Ressourcen oder Features zugreifen. Achten Sie beim Erstellen eines neuen Spiels darauf, die Funktionen auszuwählen, die Ihr Spiel für die Ausführung benötigt.

Kommen wir nun zu den restlichen Dateien der Vorlage **DirectX 11-App (Universelle Windows-App)**.

## <a name="review-the-included-libraries-and-headers"></a>Anzeigen der enthaltenen Bibliotheken und Header

Ein paar Dateien haben wir uns für den Schluss aufgehoben. Diese Dateien bieten zusätzliche Tools und Unterstützung für die Entwicklung von Direct3D-Spielen. Die vollständige Liste der Dateien, die im grundlegenden DirectX-Spieleprojekts enthalten ist, finden Sie unter [DirectX-Spielprojektvorlagen](user-interface.md#template-structure).

| Quelldatei der Vorlage         | Dateiordner            | Beschreibung |
|------------------------------|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DeviceResources.h/.cpp       | Allgemein                 | Definiert ein Klassenobjekt, das [Geräteressourcen](tutorial--assembling-the-rendering-pipeline.md#resource) von DirectX steuert. Darüber hinaus enthält es eine Schnittstelle für die Anwendung, die DeviceResources besitzt, um benachrichtigt zu werden, wenn das Gerät verloren geht oder erstellt wird.                                                |
| DirectXHelper.h              | Allgemein                 | Implementiert Methoden wie z. B. **DX::ThrowIfFailed**, **ReadDataAsync**, und ** ConvertDipsToPixels. **DX::ThrowIfFailed**konvertiert die von DirectX Win32-APIs zurückgegebenen HRESULT-Fehlerwerte in Ausnahmen der Windows-Runtime konvertiert. Verwenden Sie diese Methode, um einen Haltepunkt zum Debuggen von DirectX-Fehlern zu setzen. Weitere Informationen finden Sie unter [ThrowIfFailed](https://github.com/Microsoft/DirectXTK/wiki/ThrowIfFailed). **ReadDataAsync** liest asynchron aus einer binären Datei. **ConvertDipsToPixels** konvertiert eine Länge in geräteunabhängige Pixel (DIPs) auf eine Länge in physische Pixel. |
| StepTimer.h                  | Allgemein                 | Definiert einen Timer mit hoher Auflösung, der sich besonders für Spiele oder Apps mit interaktivem Rendering eignet.   |
| Sample3DSceneRenderer.h/.cpp | Inhalt                | Definiert ein Klassenobjekt, um eine grundlegende Renderingpipeline zu instanziieren. Erstellt eine einfache Renderimplementierung, die eine Direct3D-Swapchain und einen Grafikadapter mit Ihrem UWP-DirectX-Spiel verbindet.   |
| SampleFPSTextRenderer.h/.cpp | Inhalt                | Definiert ein Klassenobjekt, das den aktuellen FPS-Wert mit Direct2D und DirectWrite rechts unten auf dem Bildschirm rendert.  |
| SamplePixelShader.hlsl       | Inhalt                | Enthält den Code der High-Level-Shader-Language (HLSL) für einen sehr einfachen Pixel-Shader.                                            |
| SampleVertexShader.hlsl      | Inhalt                | Enthält den Code der High-Level Shader Language (HLSL) für einen sehr einfachen Vertex-Shader.                                           |
| ShaderStructures.h           | Inhalt                | Enthält die Shader-Strukturen, die zum Senden von MVP-Matrizen und Vertex-Daten an den Vertexshader verwendet werden können.  |
| pch.h/.cpp                   | Main                   | Enthält alles, was das Windows-System für die von einer Direct3D-App genutzten APIs (einschließlich DirectX 11-APIs) enthält.| 

### <a name="next-steps"></a>Nächste Schritte

Hier haben Sie gelernt, wie Sie ein UWP-DirectX-Spieleprojekts mithilfe der **DirectX 11-App (Universal Windows)**-Vorlage erstellen und haben einige Komponenten und Dateien eingeführt, die von diesem Projekt bereitgestellt werden.

Der nächste Abschnitt ist [Definieren des UWP-App-Frameworks für das Spiel](tutorial--building-the-games-uwp-app-framework.md). Wir untersuchen, wie dieses Spiel viele der Konzepte und Komponenten verwendet und erweitert, die die Vorlage bereitstellt.

 

 




