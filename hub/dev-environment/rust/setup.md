---
title: Einrichten Ihrer Entwicklungsumgebung unter Windows für Rust
description: Einrichten Ihrer Entwicklungsumgebung für Anfänger, die unter Windows mit Rust entwickeln möchten.
author: stevewhims
ms.author: stwhi
manager: jken
ms.topic: article
keywords: Rust, Windows 10, Microsoft, Rust lernen, Rust unter Windows für Anfänger, Rust mit VS Code
ms.localizationpriority: medium
ms.date: 03/04/2021
ms.openlocfilehash: 5aac8dd9b9f760f6e1ed49ff0246e44c400d72c4
ms.sourcegitcommit: 85b9a5fc16f4486bc23b4ec8f4fae5ab6211a066
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102194156"
---
# <a name="set-up-your-dev-environment-on-windows-for-rust"></a>Einrichten Ihrer Entwicklungsumgebung unter Windows für Rust

Der [Überblick über die Entwicklung unter Windows mit Rust](overview.md) enthielt Informationen zu Rust an sich sowie zu einigen der Hauptkomponenten von Rust. In diesem Thema wird die Entwicklungsumgebung eingerichtet.

Wir empfehlen, die Rust-Entwicklung unter Windows durchzuführen. Falls Sie jedoch lieber Linux zum lokalen Kompilieren und Testen verwenden möchten, ist auch das [Windows-Subsystem für Linux (WSL)](/windows/wsl/about) eine Option für die Entwicklung mit Rust.

## <a name="install-visual-studio-recommended-or-the-microsoft-c-build-tools"></a>Installieren von Visual Studio (empfohlen) oder Microsoft Build Tools für C++

Unter Windows sind für Rust bestimmte C++-Buildtools erforderlich.

Sie können entweder [Microsoft Build Tools für C++](https://visualstudio.microsoft.com/visual-cpp-build-tools/) herunterladen oder einfach [Microsoft Visual Studio](https://visualstudio.microsoft.com/downloads/) installieren (empfohlen).

> [!NOTE]
> Wir verwenden nicht Visual Studio, sondern Visual Studio Code als unsere integrierte Entwicklungsumgebung (Integrated Development Environment, IDE) für Rust. Sie können Visual Studio aber trotzdem kostenfrei installieren. Die Community-Edition ist für Schüler/Studenten, Open-Source-Mitwirkende und Einzelpersonen kostenlos.

Bei der Installation von Visual Studio sollten mehrere Windows-Workloads ausgewählt werden. Hierzu zählen **.NET-Desktopentwicklung**, **Desktopentwicklung mit C++** und **Entwicklung für die universelle Windows-Plattform**. Zwar kann es sein, dass Sie nicht alle drei benötigen, es ist aber nicht unwahrscheinlich, dass sich eine Abhängigkeit ergibt, die sie erforderlich macht. Daher ist es einfacher, gleich alle drei auszuwählen.

Bei neuen Rust-Projekten wird standardmäßig Git verwendet. Fügen Sie daher auch die Einzelkomponente **Git für Windows** hinzu. (Verwenden Sie das Suchfeld, um nach dem Namen zu suchen.)

![„.NET-Desktopentwicklung“, „Desktopentwicklung mit C++“ und „Entwicklung für die universelle Windows-Plattform“](../../images/rust-vs-workloads.png)

## <a name="install-rust"></a>Installieren von Rust

Installieren Sie als Nächstes Rust [über die Rust-Website](https://www.rust-lang.org/tools/install). Die Website erkennt, dass Sie Windows verwenden, und bietet 64 Bit- und 32-Bit-Installationsprogramme des Tools `rustup` für Windows sowie eine Anleitung für die Installation von Rust im [Windows-Subsystem für Linux (WSL)](/windows/wsl/about).

> [!TIP]
> Rust funktioniert unter Windows sehr gut. Daher gibt es eigentlich keinen Grund für die WSL-Route (es sei denn, Sie möchten unter Linux lokal kompilieren und testen). Da Sie über Windows verfügen, empfiehlt es sich, einfach das Installationsprogramm `rustup` für Windows (64 Bit) auszuführen. Anschließend können Sie mithilfe von Rust Apps *für* Windows schreiben.

Nach Abschluss der Rust-Installation können Sie mit Rust programmieren. Sie verfügen allerdings noch nicht über eine praktische IDE. (Entsprechende Informationen finden Sie im Anschluss im Abschnitt [Installieren von Visual Studio Code](#install-visual-studio-code).) Und Sie können noch keine Windows-APIs aufrufen. Sie können aber eine Eingabeaufforderung (etwa die **x64 Native Tools-Eingabeaufforderung für VS** oder `cmd.exe`) starten und den Befehl `cargo --version` ausführen. Wird eine Versionsnummer ausgegeben, ist Rust ordnungsgemäß installiert.

Sollten Sie sich fragen, was es mit dem oben verwendeten Suchbegriff `cargo` auf sich hat: *Cargo* ist der Name des Tools in der Rust-Entwicklungsumgebung, mit dem Ihre Projekte (genauer: *Pakete*) und deren Abhängigkeiten verwaltet und erstellt werden.

Und falls Sie sich an dieser Stelle trotz fehlender IDE direkt in die Programmierung stürzen möchten, empfehlen wir das Kapitel [Hello, World!](https://doc.rust-lang.org/book/ch01-02-hello-world.html) des Buchs zur Rust-Programmiersprache auf der Rust-Website.

## <a name="install-visual-studio-code"></a>Installieren von Visual Studio Code

Mit Visual Studio Code (VS Code) als Text-Editor/integrierte Entwicklungsumgebung (Integrated Development Environment, IDE) können Sie Sprachdienste wie Codevervollständigung, Syntaxhervorhebung, Formatierung und Debuggen nutzen.

VS Code enthält auch ein [integriertes Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal), mit dem Sie Befehlszeilenargumente ausgeben können (beispielsweise für Cargo-Befehle).

1. Laden Sie zunächst [Visual Studio Code für Windows](https://code.visualstudio.com) herunter, und installieren Sie es.

2. Installieren Sie im Anschluss an die VS Code-Installation die *Erweiterung* **rust-analyzer**. Dazu können Sie entweder die [Erweiterung „rust-analyzer“ aus Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=matklad.rust-analyzer) installieren oder VS Code öffnen und im Erweiterungsmenü (STRG+UMSCHALT+X) nach **rust-analyzer** suchen.

3. Installieren Sie zur Unterstützung beim Debuggen die Erweiterung **CodeLLDB**. Dazu können Sie entweder die [Erweiterung „CodeLLDB“ aus Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb) installieren oder VS Code öffnen und im Erweiterungsmenü (STRG+UMSCHALT+X) nach **CodeLLDB** suchen.

   > [!NOTE]
   > Eine Alternative zur Erweiterung **CodeLLDB** für die Debugunterstützung ist die Microsoft-Erweiterung **C/C++** . Die Erweiterung **C/C++** ist nicht so gut in die IDE integriert wie **CodeLLDB**. Dafür bietet die Erweiterung **C/C++** allerdings bessere Debuginformationen. Daher empfiehlt es sich gegebenenfalls, diese Erweiterung bereitzuhalten, falls sie benötigt wird.
   >
   > Dazu können Sie entweder die [Erweiterung „C/C++“ aus Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) installieren oder VS Code öffnen und im Erweiterungsmenü (STRG+UMSCHALT+X) nach **C/C++** suchen.

4. Wenn Sie das Terminal in VS Code öffnen möchten, wählen Sie **Ansicht** > **Terminal** aus, oder verwenden Sie alternativ die Tastenkombination **STRG+`** (mit dem Graviszeichen). Das Standardterminal ist PowerShell.

## <a name="hello-world-tutorial-rust-with-vs-code"></a>Hello, world! Tutorial (Rust mit VS Code)

In diesem Abschnitt können Sie sich anhand einer einfachen App vom Typ „Hello, world!“ mit Rust vertraut machen.

1. Starten Sie zunächst eine Eingabeaufforderung (etwa die **x64 Native Tools-Eingabeaufforderung für VS** oder `cmd.exe`), und wechseln Sie mithilfe von `cd` zu einem Ordner, in dem Sie Ihre Rust-Projekte speichern möchten.

2. Führen Sie anschließend den folgenden Befehl für Cargo aus, um ein neues Rust-Projekt zu erstellen:

   ```console
   cargo new first_rust_project
   ```

   Das Argument, das Sie an den Befehl `cargo new` übergeben, ist der Name des Projekts, das von Cargo erstellt werden soll. Hier lautet der Projektname *first_rust_project*. Es empfiehlt sich, Ihre Rust-Projekte im sogenannten Snake Case-Format zu benennen. Das bedeutet, dass die Wörter klein geschrieben und Leerzeichen durch Unterstriche ersetzt werden.

   Von Cargo wird ein Projekt mit dem von Ihnen angegebenen Namen erstellt. Neue Projekte von Cargo enthalten jeweils den Quellcode für eine sehr einfache App, die die Nachricht *Hello, world!* ausgibt, wie später zu sehen. Zusätzlich zum Projekt *first_rust_project* hat Cargo einen Ordner namens *first_rust_project* erstellt und die Quellcodedateien des Projekts dort abgelegt.

3. Wechseln Sie daher nun mithilfe von `cd` zu diesem Ordner, und starten Sie VS Code im Kontext dieses Ordners.

   ```console
   cd first_rust_project
   code .
   ```

4. Öffnen Sie im Explorer von VS Code die Datei `src` > `main.rs`. Hierbei handelt es sich um die Rust-Quellcodedatei mit dem Einstiegspunkt Ihrer App (eine Funktion mit dem Namen **main**). Das Fenster sieht so aus.

   ```rust
   // main.rs
   fn main() {
     println!("Hello, world!");
   }
   ```

   > [!NOTE]
   > Beim Öffnen der ersten Datei vom Typ `.rs` werden Sie in VS Code darauf hingewiesen, dass einige Rust-Komponenten nicht installiert sind, und gefragt, ob Sie sie installieren möchten. Klicken Sie auf **Ja**. Daraufhin wird von VS Code der Rust-Sprachserver installiert.

   Anhand des Codes in `main.rs` können Sie erkennen, dass es sich bei **main** um eine Funktionsdefinition handelt und dass die Zeichenfolge „Hello, world!“ ausgegeben wird. Ausführlichere Informationen zur Syntax finden Sie auf der Rust-Website unter [Aufbau eines Rust-Programms](https://doc.rust-lang.org/book/ch01-02-hello-world.html#anatomy-of-a-rust-program).

5. Als Nächstes führen wir die App unter dem Debugger aus. Platzieren Sie in Zeile 2 einen Haltepunkt, und klicken Sie auf **Ausführen** > **Debuggen starten** (oder drücken Sie **F5**). Befehle zum **Debuggen** und **Ausführen** stehen auch im Text-Editor zur Verfügung.

   > [!NOTE]
   > Wenn Sie eine App zum ersten Mal unter dem Debugger ausführen, wird ein Dialogfeld mit dem Hinweis angezeigt, dass das Debuggen nicht gestartet werden kann, da keine Startkonfiguration bereitgestellt wurde. Klicken Sie auf **OK**. Daraufhin wird ein zweites Dialogfeld mit dem Hinweis angezeigt, dass „Cargo.toml“ in diesem Arbeitsbereich erkannt wurde, und Sie werden gefragt, ob Sie Startkonfigurationen für die zugehörigen Ziele generieren möchten. Klicken Sie auf **Ja**. Schließen Sie anschließend die Datei „launch.json“, und starten Sie das Debuggen erneut.

6. Wie Sie sehen, unterbricht der Debugger die Verarbeitung in Zeile 2. Drücken Sie **F5**, um fortzufahren. Daraufhin wird die App vollständig ausgeführt. Im Terminalbereich wird die erwartete Ausgabe **Hello, world!** angezeigt.

## <a name="rust-for-windows"></a>RUST für Windows

Sie können Rust nicht nur *unter* Windows verwenden, sondern mit Rust auch Apps *für* Windows schreiben. Über die Kiste *windows* können Sie beliebige alte, aktuelle und zukünftige Windows-APIs aufrufen. Ausführlichere Informationen hierzu sowie Codebeispiele finden Sie im Thema [Rust für Windows und die Kiste „windows“](rust-for-windows.md).

## <a name="related"></a>Verwandte Themen

* [Rust für Windows und Windows-Kiste](rust-for-windows.md)
* [Windows-Subsystem für Linux (WSL)](/windows/wsl/about)
* [Microsoft Build Tools für C++](https://visualstudio.microsoft.com/visual-cpp-build-tools/)
* [Microsoft Visual Studio](https://visualstudio.microsoft.com/downloads/)
* [Visual Studio Code für Windows](https://code.visualstudio.com)
* [Erweiterung „rust-analyzer“](https://marketplace.visualstudio.com/items?itemName=matklad.rust-analyzer)
* [Erweiterung „CodeLLDB“](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb)
* [C/C++-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
