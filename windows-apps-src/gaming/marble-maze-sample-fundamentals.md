---
title: Grundlagen am Beispiel von Marble Maze
description: In diesem Dokument werden die fundamentalen Eigenschaften des Marble Maze-Projekts beschrieben, beispielsweise wie Visual C++ in der Windows-Runtime-Umgebung verwendet wird, wie es erstellt und strukturiert wird und wie es aufgebaut ist.
ms.assetid: 73329b29-62e3-1b36-01db-b7744ee5b4c3
ms.date: 08/22/2017
ms.topic: article
keywords: Windows 10, Uwp, Spiele, Beispiel, Directx, Grundlagen
ms.localizationpriority: medium
ms.openlocfilehash: f04c17609976e8bd8f6c1c6143ed7b992b0bb3c5
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67820618"
---
# <a name="marble-maze-sample-fundamentals"></a>Grundlagen am Beispiel von Marble Maze




In diesem Thema werden die fundamentalen Eigenschaften des Marble Maze-Projekt&mdash;s beschrieben, beispielsweise wie Visual C++ in der Windows Runtime-Umgebung verwendet wird, wie es erstellt und strukturiert wird und wie es aufgebaut ist. Das Thema enthält auch eine Beschreibung verschiedener Konventionen, die im Code verwendet werden.

> [!NOTE]
> Den Beispielcode für dieses Dokument finden Sie im [DirectX-Beispielspiel Marble Maze](https://go.microsoft.com/fwlink/?LinkId=624011).

Es folgen einige wichtige Punkte, die in diesem Dokument erläutert werden und die beim Planen und Entwickeln Ihres UWP-Spiels relevant sind.

-   Verwenden der **DirectX 11-App (universelle Windows - C++/CX)** Vorlage in Visual Studio zum Erstellen Ihrer DirectX-UWP-Spiels.
-   Windows-Runtime stellt Klassen und Schnittstellen bereit, sodass Sie UWP-Apps auf moderne, objektorientierte Art und Weise entwickeln können.
-   Verwenden Sie Objektverweise mit dem Caretzeichen "^"-Symbol zum Verwalten der Lebensdauer von Windows-Runtime-Variablen, [Microsoft::WRL::ComPtr](https://docs.microsoft.com/cpp/windows/comptr-class) zum Verwalten der Lebensdauer von COM-Objekten und [std::shared\_Ptr](https://docs.microsoft.com/cpp/standard-library/shared-ptr-class) oder [std::unique\_Ptr](https://docs.microsoft.com/cpp/standard-library/unique-ptr-class) zum Verwalten der Lebensdauer von allen anderen vom Heap zugewiesenen C++ Objekte.
-   In den meisten Fällen verwenden Sie für die Behandlung unerwarteter Fehler die Ausnahmebehandlung anstelle von Ergebniscodes.
-   Verwenden Sie [SAL-Anmerkungen](https://docs.microsoft.com/visualstudio/code-quality/using-sal-annotations-to-reduce-c-cpp-code-defects) in Kombination mit Codeanalysetools, um Fehler in der App zu finden.

## <a name="creating-the-visual-studio-project"></a>Erstellen des Visual Studio-Projekts


Wenn Sie das Beispiel heruntergeladen und extrahiert haben, können Sie die Projektmappendatei **MarbleMaze_VS2017.sln** (im Ordner **C++** ) öffnen, und der gesamte Code liegt offen vor Ihnen.

Beim Erstellen des Visual Studio-Projekts für Marble Maze haben wir mit einem bereits vorhandenen Projekt begonnen. Jedoch wenn Sie nicht bereits ein vorhandenes Projekt, die die grundlegende Funktionalität bereitstellt, der Ihrer DirectX-UWP-Spiel erfordert verfügen, es wird empfohlen, dass Sie ein Projekt basierend auf der Visual Studio erstellen **DirectX 11-App (universelle Windows - C++/CX)** Vorlage, da es sich um eine grundlegende funktionierende 3D-Anwendung bietet. Gehen Sie hierzu folgendermaßen vor:

1. Wählen Sie in Visual Studio-2019, **Datei > Neu > Projekt...**

2. In der **Erstellen eines neuen Projekts** wählen Sie im Fenster **DirectX 11-App (universelle Windows - C++/CX)** . Wenn Sie diese Option nicht sehen, Sie haben nicht die erforderlichen Komponenten installiert&mdash;finden Sie unter [ändern Visual Studio 2019 durch Hinzufügen oder Entfernen von Workloads und Komponenten](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) Informationen zur Installation zusätzlicher Komponenten .

![Neues Projekt](images/vs2019-marble-maze-sample-fundamentals-1.png)

3. Wählen Sie **Weiter**, und geben Sie dann eine **Projektname**, **Speicherort** für die Dateien gespeichert werden sollen, und ein **Projektmappenname**, und wählen Sie dann auf  **Erstellen Sie**.



Eine wichtige projekteinstellung in der **DirectX 11-App (universelle Windows - C++/CX)** Vorlage ist die **/Zw** auswählen, um die Anwendung die Windows-Runtime-spracherweiterungen verwenden zu können. Sie ist standardmäßig aktiviert, wenn Sie die Visual Studio-Vorlage verwenden. Weitere Informationen zur Vorgehensweise beim Festlegen von Compiler-Optionen in Visual Studio finden Sie unter [Compileroptionen festlegen](https://docs.microsoft.com/cpp/build/reference/setting-compiler-options).

> **Vorsicht**    der **/Zw** Option ist nicht kompatibel mit den Optionen, wie z. B. **"/ CLR"** . Im Fall von **/clr** können Sie demnach über ein und dasselbe Visual C++-Projekt nicht das .NET Framework und die Windows-Runtime erreichen.

 

Jede UWP-app, die Sie aus dem Microsoft Store erwerben, ist in Form eines app-Pakets. Ein App-Paket enthält ein App-Manifest, das wiederum Informationen zur App beinhaltet. Sie können beispielsweise die Funktionen (d. h. den erforderlichen Zugriff auf geschützte Systemressourcen oder Benutzerdaten) Ihrer App angeben. Wenn Sie festlegen, dass für Ihre App bestimmte Funktionen erforderlich sind, verwenden Sie das Paketmanifest, um die erforderlichen Funktionen zu deklarieren. Das Manifest ermöglicht Ihnen auch die Angabe von Projekteigenschaften wie der Rotation unterstützter Geräte, Kachelbildern und Begrüßungsbildschirm. Sie können das Manifest bearbeiten, indem Sie **Package.appxmanifest** in Ihrem Projekt öffnen. Weitere Informationen zu App-Paketen finden Sie unter [Verpacken von Apps](https://docs.microsoft.com/windows/uwp/packaging/index).

##  <a name="building-deploying-and-running-the-game"></a>Erstellen, Bereitstellen und Ausführen des Spiels

Wählen Sie in den Dropdownmenüs oben in Visual Studio links neben der grünen Schaltfläche "Wiedergabe" die Konfiguration Ihrer Bereitstellung aus. Wir empfehlen die Einstellung als **Debuggen** und die Architektur Ihres Geräts auf (**x86** für 32-Bit, **x64** für 64-Bit) und auf **Lokaler Computer** festzulegen. Sie können ebenfalls einen **Remotecomputer** oder ein **Gerät** testen, das über USB verbunden ist. Klicken Sie dann auf die grüne Wiedergabeschaltfläche zum Erstellen und Bereitstellen auf Ihrem Gerät.

![Debuggen; x64; Lokaler Computer](images/marble-maze-sample-fundamentals-2.png)

###  <a name="controlling-the-game"></a>Steuern des Spiels

Für die Steuerung von Marble Maze können Sie die Fingereingabe, den Beschleunigungsmesser, den Xbox One-Controller oder die Maus verwenden.

-   Mithilfe des Steuerkreuzes am Controller können Sie das aktive Menüelement ändern.
-   Zum Auswählen eines Menüelements können Sie die A-oder die Starttaste auf dem Controller oder die Maus verwenden.
-   Über die Fingereingabe, den Beschleunigungsmesser, den linken Ministick oder die Maus können Sie das Labyrinth neigen.
-   Zum Schließen von Menüs wie der Highscore-Tabelle können Sie die A-oder die Starttaste auf dem Controller oder die Maus verwenden.
-   Um das Spiel anzuhalten oder fortzusetzen können Sie die Starttaste am Controller oder die P-Taste auf der Tastatur verwenden.
-   Zum Neustarten des Spiels können Sie die Zurück-Taste am Controller oder die Pos1-Taste auf der Tastatur verwenden.
-   Wenn die Highscore-Tabelle angezeigt wird, können Sie mit der Zurücktaste auf dem Controller oder der Pos1-Taste auf der Tastatur alle Punktergebnisse löschen.

##  <a name="code-conventions"></a>Codekonventionen


Die Windows-Runtime ist eine Programmierschnittstelle, die Sie zum Erstellen von UWP-Apps verwenden können, die nur in einer speziellen Anwendungsumgebung ausgeführt werden. Solche apps verwenden autorisierte Funktionen, Datentypen und Geräte und von den Microsoft Store verteilt werden. Die Windows-Runtime besteht auf der untersten Ebene aus einer binären Anwendungsschnittstelle (Application Binary Interface, ABI). Die ABI ist ein binärer Vertrag auf unterer Ebene, der Windows-Runtime-APIs für mehrere Programmiersprachen zur Verfügung stellt, beispielsweise für JavaScript, die .NET-Sprachen und Visual C++.

Diese Sprachen erfordern für den Aufruf von Windows-Runtime-APIs aus JavaScript und .NET Projektionen, die für die jeweilige Sprachumgebung spezifisch sind. Wenn Sie eine Windows-Runtime-API aus JavaScript oder .NET aufrufen, rufen Sie die Projektion auf, die wiederum die zugrunde liegende ABI-Funktion aufruft. Sie können zwar die ABI-Funktionen direkt aus Standard-C++ aufrufen, jedoch stellt Microsoft auch Projektionen für C++ bereit, da diese die Verwendung der Windows-Runtime-APIs stark vereinfachen und dennoch eine hohe Leistung aufrecht erhalten. Microsoft stellt außerdem Spracherweiterungen für Visual C++ bereit, die spezielle Unterstützung für die Windows-Runtime-Projektionen bieten. Viele dieser Spracherweiterungen ähneln der Syntax für die Sprache C++/CLI. Anstelle einer Zielgruppenadressierung für die Common Language Runtime (CLR) durchzuführen, verwenden systemeigene Apps diese Syntax zum Erreichen der Windows-Runtime. Der Modifizierer in Form einer Objektreferenz, bzw. des Hütchensymbols (^), ist ein wichtiger Teil dieser neuen Syntax, da er die automatische Löschung von Runtime-Objekten anhand einer Referenzzählung ermöglicht. Anstatt Methoden wie [AddRef](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref) und [Release](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-release) zum Verwalten der Lebensdauer eines Windows-Runtime-Objekts aufzurufen, löscht die Runtime das Objekt, wenn keine andere Komponente darauf verweist. Dies ist beispielsweise der Fall, wenn es den Bereich verlässt oder wenn Sie alle Verweise auf **nullptr** festlegen. Ein weiterer wichtiger Aspekt beim Erstellen von UWP-Apps ist das **ref new**-Schlüsselwort. Verwenden Sie **ref new** anstelle von **new**, um Windows-Runtime-Objekte mit Verweiszählung zu erstellen. Weitere Informationen finden Sie unter [Typsystem (C++/CX)](https://docs.microsoft.com/cpp/cppcx/type-system-c-cx).

> [!IMPORTANT]
> Wenn Sie Windows-Runtime-Objekte oder Komponenten für die Windows-Runtime erstellen, müssen Sie nur **^** und **ref new** verwenden. Sie können die C++-Standardsyntax verwenden, wenn Sie Kernanwendungscode schreiben, in dem die Windows-Runtime nicht genutzt wird.

In Marble Maze werden mit **^** und **Microsoft::WRL::ComPtr** zugewiesene Objekte verwaltet und Arbeitsspeicherverluste minimiert. Wir empfehlen die Verwendung ^ zum Verwalten der Lebensdauer von Windows-Runtime-Variablen, **ComPtr** zum Verwalten der Lebensdauer von COM-Variablen (z. B. Wenn Sie DirectX verwenden), und **std::shared\_Ptr** oder **std::unique\_Ptr** zum Verwalten der Lebensdauer von allen anderen vom Heap zugewiesenen C++ Objekte.

 

Weitere Informationen zu den Spracherweiterungen, die für eine C++-UWP-App verfügbar sind, finden Sie unter [Sprachreferenz zu Visual C++ (C++/CX)](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx).

###  <a name="error-handling"></a>Fehlerbehandlung

In Marble Maze werden unerwartete Fehler in erster Linie mithilfe der Ausnahmebehandlung verarbeitet. Obwohl für Spielecode zur Angabe von Fehlern traditionell die Protokollierung oder Fehlercodes verwendet werden, beispielsweise **HRESULT**-Werte, bietet die Ausnahmebehandlung zwei wesentliche Vorteile. Zunächst kann sie das Lesen und Verwalten von Code erleichtern. Aus Codeperspektive stellt die Ausnahmebehandlung ein effizienteres Verfahren dar, um einen Fehler an eine Routine weiterzugeben, die den Fehler behandeln kann. Bei der Verwendung von Fehlercodes muss i. d. R. jede Funktion explizit Fehler weitergeben. Ein zweiter Vorteil besteht darin, dass Sie den Visual Studio-Debugger so konfigurieren können, dass er beim Auftreten einer Ausnahme unterbrochen wird, damit die Ausführung unmittelbar an der Position und im Kontext des Fehlers angehalten wird. Die Windows-Runtime macht ebenfalls extensiven Gebrauch von der Ausnahmebehandlung. Deshalb können Sie durch die Verwendung von Ausnahmebehandlung im Code die gesamte Fehlerbehandlung in einem einzigen Modell zusammenfassen.

Es wird empfohlen, in Ihrem Fehlerbehandlungsmodell die folgenden Konventionen zu verwenden:

-   Kommunizieren Sie unerwartete Fehler anhand von Ausnahmen.
-   Verwenden Sie Ausnahmen nicht zum Steuern des Codeflusses.
-   Fangen Sie nur die Ausnahmen ab, die Sie auf jeden Fall behandeln und beheben können. Fangen Sie andernfalls die Ausnahme nicht ab, und ermöglichen Sie das Beenden der App.
-   Wenn Sie eine DirectX-Routine aufrufen, die **HRESULT** zurückgibt, verwenden Sie die **DX::ThrowIfFailed**-Funktion. Diese Funktion wird in [DirectXHelper.h](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/master/C%2B%2B/Shared/DirectXHelper.h) definiert. **ThrowIfFailed** löst eine Ausnahme aus, wenn die bereitgestellten **HRESULT** einen Fehlercode enthalten. Z. B. **E\_ZEIGER** bewirkt, dass **ThrowIfFailed** auslösen [Platform:: NullReferenceException](https://docs.microsoft.com/cpp/cppcx/platform-nullreferenceexception-class).

    Wenn Sie **ThrowIfFailed** verwenden, platzieren Sie den DirectX-Aufruf in einer separaten Zeile, um die Lesbarkeit des Codes zu verbessern. Dies wird im folgenden Beispiel veranschaulicht.

    ```cpp
    // Identify the physical adapter (GPU or card) this device is running on.
    ComPtr<IDXGIAdapter> dxgiAdapter;
    DX::ThrowIfFailed(
        dxgiDevice->GetAdapter(&dxgiAdapter)
        );
    ```

-   Obwohl empfohlen wird, die Verwendung von **HRESULT** für unerwartete Fehler zu vermeiden, ist es wichtiger, auf die Nutzung der Ausnahmebehandlung zur Steuerung des Codeflusses zu verzichten. Demzufolge wird bevorzugt, bei Bedarf einen **HRESULT**-Rückgabewert zu verwenden, um den Codefluss zu steuern.

###  <a name="sal-annotations"></a>SAL-Anmerkungen

Verwenden Sie SAL-Anmerkungen in Kombination mit Codeanalysetools, um Fehler in der App zu finden.

Mithilfe der Microsoft-Quellcodeanmerkungssprache (Source Code Annotation Language, SAL) können Sie anmerken bzw. beschreiben, wie eine Funktion die zugehörigen Parameter verwendet. SAL-Anmerkungen werden auch zum Beschreiben von Rückgabewerten verwendet. SAL-Anmerkungen können mit dem C/C++-Codeanalysetool verwendet werden, um mögliche Fehler im C- und C++-Quellcode zu finden. Häufige Codierungsfehler, die vom Tool gemeldet werden, beinhalten Pufferüberläufe, nicht initialisierte Speicher, Nullzeiger-Dereferenzierungen und Speicher- und Ressourcenverluste.

Ziehen Sie die **BasicLoader::LoadMesh**-Methode in Erwägung, die in [BasicLoader.h](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/e62d68a85499e208d591d2caefbd9df62af86809/C%2B%2B/Shared/BasicLoader.h) deklariert wird. Diese Methode verwendet `_In_` für die Angabe, dass *filename* ein Eingabeparameter ist (und folglich nur daraus gelesen wird). Zudem verwendet sie `_Out_` für die Angabe, dass *vertexBuffer* und *indexBuffer* und Ausgabeparameter sind (und demzufolge in diese Parameter geschrieben wird). Schlussendlich verwendet die Methode `_Out_opt_`, um anzugeben, dass *vertexCount* und *indexCount* optionale Ausgabeparameter sind (und möglicherweise in diese Parameter geschrieben wird). Da *vertexCount* und *indexCount* optionale Ausgabeparameter sind, dürfen sie **nullptr** sein. Das C/C++-Codeanalysetool untersucht Aufrufe dieser Methode, um sicherzustellen, dass die von ihr übergebenen Parameter diese Kriterien erfüllen.

```cpp
void LoadMesh(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    );
```

Wenn Sie eine Codeanalyse für Ihre App ausführen möchten, wählen Sie auf der Menüleiste die Optionen **Erstellen > Codeanalyse für Lösung ausführen** aus. Weitere Informationen zur Codeanalyse finden Sie unter [Analysieren der C/C++-Codequalität mithilfe der Codeanalyse](https://docs.microsoft.com/visualstudio/code-quality/analyzing-c-cpp-code-quality-by-using-code-analysis).

Die vollständige Liste der verfügbaren Anmerkungen wird in sal.h definiert. Weitere Informationen finden Sie unter [SAL-Anmerkungen](https://docs.microsoft.com/cpp/c-runtime-library/sal-annotations).

## <a name="next-steps"></a>Nächste Schritte


Lesen Sie die Informationen unter [Marble Maze-Anwendungsstruktur](marble-maze-application-structure.md), um zu erfahren, wie der Marble Maze-Anwendungscode strukturiert ist und wodurch sich die Struktur einer DirectX-UWP-App von der einer herkömmlichen Desktopanwendung unterscheidet.

## <a name="related-topics"></a>Verwandte Themen


* [Marble Maze-Anwendungsstruktur](marble-maze-application-structure.md)
* [Entwickeln von Marble Maze, einem UWP-Spiel in C++ und DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




