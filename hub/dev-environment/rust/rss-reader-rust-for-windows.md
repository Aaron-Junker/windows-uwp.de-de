---
title: Tutorial zu RSS Reader (Rust für Windows mit VS Code)
description: Eine exemplarische Vorgehensweise, in der eine einfache App geschrieben wird, die die Titel von Blogbeiträgen aus einem RSS-Feed herunterlädt.
author: stevewhims
ms.author: stwhi
manager: jken
ms.topic: article
keywords: Rust, Windows 10, Microsoft, Rust lernen, Rust unter Windows für Anfänger, Rust mit VS Code, Rust für Windows
ms.localizationpriority: medium
ms.date: 03/04/2021
ms.openlocfilehash: b55faf6d44395989cb7eec39fb9cdd1ee7128aa7
ms.sourcegitcommit: bcdec8bda3106cd5588464531e582101d52dcc80
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2021
ms.locfileid: "102254553"
---
# <a name="rss-reader-tutorial-rust-for-windows-with-vs-code"></a>Tutorial zu RSS Reader (Rust für Windows mit VS Code)

Im vorherigen Thema wurden [Rust für Windows und die Kiste „windows“](rust-for-windows.md) vorgestellt.

In diesem Thema können Sie Rust für Windows ausprobieren, indem sei eine einfache App schreiben, die Titel von Blogbeiträgen aus einem RSS-Feed (Really Simple Syndication) herunterlädt.

1. Starten Sie eine Eingabeaufforderung (etwa die **x64 Native Tools-Eingabeaufforderung für VS** oder `cmd.exe`), und wechseln Sie mithilfe von `cd` zu einem Ordner, in dem Sie Ihre Rust-Projekte speichern möchten.

2. Erstellen Sie dann über Cargo ein neues Rust-Projekt namens *rss_reader*, und wechseln Sie mithilfe von `cd` in den neu erstellten Ordner des Projekts.

   ```console
   cargo new rss_reader
   cd rss_reader
   ```

3. Erstellen Sie anschließend über Cargo ein neues Unterprojekt namens *bindings*. Wie im folgenden Befehl zu sehen, handelt es sich bei diesem neuen Projekt um eine Bibliothek, die als Medium für die Bindung an die aufzurufenden Windows-APIs fungiert. Zur Buildzeit wird das untergeordnete Bibliotheksprojekt *bindings* zu einer *Kiste* verarbeitet. (So werden in Rust Binärdateien oder Bibliotheken genannt.) Diese Kiste wird dann innerhalb des Projekts *rss_reader* genutzt, wie später gezeigt.

   ```console
   cargo new --lib bindings
   ```

   Da wir *bindings* als geschachtelte Kiste erstellen, kann *bindings* beim Erstellen von *rss_reader* die Ergebnisse aller von uns importierten Bindungen zwischenspeichern.

4. Öffnen Sie als Nächstes das Projekt *rss_reader* in VS Code.

   ```console
   code .
   ```

5. Widmen Sie sich zunächst der Bibliothek *bindings*.

   Öffnen Sie im Explorer von VS Code die Datei `bindings` > `Cargo.toml`.

   ![In VS Code geöffnete Datei „Cargo.toml“ nach Erstellung des Projekts](../../images/rust-rss-reader-1.png)

   Bei einer Datei vom Typ `Cargo.toml` handelt es sich um eine Textdatei zur Beschreibung eines Rust-Projekts (einschließlich aller zugehörigen Abhängigkeiten).

   Der Abschnitt `dependencies` ist momentan noch leer. Bearbeiten Sie diesen Abschnitt nun, und fügen Sie auch einen Abschnitt vom Typ `[build-dependencies]` hinzu. Das sollte dann wie folgt aussehen:

   ```toml
   # bindings\Cargo.toml
   ...

   [dependencies]
   windows="0.3.1"

   [build-dependencies]
   windows="0.3.1"
   ```

   Wir haben soeben eine Abhängigkeit für die Kiste *windows* erstellt, und zwar sowohl für die Bibliothek *bindings* als auch für das zugehörige Buildskript. Dadurch kann Windows-Unterstützung von Cargo als Paket heruntergeladen, erstellt und zwischengespeichert werden. Legen Sie die Versionsnummer auf die aktuelle Version fest. Diese finden Sie auf der [Webseite für die Windows-Kiste](https://crates.io/crates/windows).

6. Nun können wir das eigentliche Buildskript hinzufügen. An dieser Stelle generieren wir die später erforderlichen Bindungen. Klicken Sie in VS Code mit der rechten Maustaste auf den Ordner *bindings*, und klicken Sie anschließend auf **Neue Datei**. Geben Sie den Namen *build.rs* ein, und drücken Sie die **EINGABETASTE**. Bearbeiten Sie `build.rs` wie folgt:

   ```rust
   // bindings\build.rs
   fn main() { 
       windows::build!(windows::web::syndication::SyndicationClient);
   }
   ```

   Durch das Makro `windows::build!` werden alle Abhängigkeiten in Form von Dateien vom Typ `.winmd` aufgelöst und Bindungen für ausgewählte Typen direkt auf der Grundlage von Metadaten generiert. Mit `windows::web::syndication::*` anstelle von `windows::web::syndication::SyndicationClient` hätten wir auch einen *vollständigen Namespace* anfordern können. Hier sollen jedoch nur Bindungen für den Typ [**SyndicationClient**](/uwp/api/windows.web.syndication.syndicationclient) generiert werden. Auf diese Weise können Sie genau so viel importieren, wie Sie benötigen, ohne auf die Generierung und Kompilierung von Code für nicht benötigte Dinge zu warten.
   
   Außerdem sucht das Makro *build* automatisch nach allen Abhängigkeiten für die explizit aufgeführten Typen. Hier enthält **SyndicationClient** eine Methode, für die ein Parameter vom Typ [**Uri**](/uwp/api/windows.foundation.uri) erwartet wird. Das Makro *build* enthält daher die Definition für **Uri**, damit diese Methode aufgerufen werden kann. Andere Typen sind Teil der Kiste **windows**. **windows::Result** wird beispielsweise durch die Kiste *windows* definiert und ist somit immer verfügbar. Sie werden feststellen, dass die meisten Elemente, die Sie aus dem Namespace [**Windows.Foundation**](/uwp/api/windows.foundation) benötigen, automatisch enthalten sind.

7. Öffnen Sie die Quellcodedatei `bindings` > `src` > `lib.rs`. Ersetzen Sie den Standardcode in `lib.rs` durch Folgendes, um die im vorherigen Schritt generierten Bindungen einzuschließen:

   ```rust
   // bindings\src\lib.rs
   ::windows::include_bindings!();
   ```

   Das Makro `windows::include_bindings!` enthält den Quellcode, der im vorherigen Schritt durch das Buildskript generiert wurde. Wenn Sie nun Zugriff auf zusätzliche APIs benötigen, können Sie sie einfach im Buildskript (`build.rs`) auflisten.

8. Als Nächstes implementieren wir das Hauptprojekt *rss_reader*. Öffnen Sie zunächst die Datei `Cargo.toml` im Stammverzeichnis des Projekts, und fügen Sie die folgende Abhängigkeit für die innere Kiste *bindings* hinzu.

   ```toml
   # Cargo.toml
   ...

   [dependencies] 
   bindings = { path = "bindings" }
   ```

9. Öffnen Sie abschließend die Quellcodedatei `src` > `main.rs` des Projekts *rss_reader*. Darin befindet sich der einfache Code, durch den die Nachricht *Hello, world!* ausgegeben wird. Fügen Sie diesen Code am Anfang von `main.rs` hinzu.

   ```rust
   // src\main.rs
   use bindings::{ 
       windows::foundation::Uri,
       windows::web::syndication::SyndicationClient,
       windows::Result,
   };

   fn main() {
       println!("Hello, world!");
   }
   ```

   Durch die Deklaration `use` wird der Pfad auf die von uns verwendeten Typen verkürzt. Hier sehen Sie auch den zuvor erwähnten Typ **Uri**. **windows::Result** ist für die Fehlerweitergabe sowie für eine präzise Fehlerbehandlung hilfreich.

10. Fügen Sie den folgenden Code in die Funktion **main** ein, um einen neuen [**URI**](/uwp/api/windows.foundation.uri) zu erstellen.

   ```rust
   // src\main.rs
   ...

   fn main() -> Result<()> {
       let uri = Uri::create_uri("https://blogs.windows.com/feed")?;

       Ok(())
   }
   ```

   Beachten Sie, dass **windows::Result** als Rückgabetyp der Funktion **main** verwendet wird. Dies dient der Vereinfachung, da häufig Fehler von Betriebssystem-APIs behandelt werden müssen.

   Haben Sie den Fragezeichen-Operator am Ende der Codezeile zum Erstellen eines **URI** bemerkt? Er wird verwendet, um weniger eingeben zu müssen und die Fehlerweitergabe und Kurzschlusslogik von Rust zu verwenden. Das bedeutet, dass für dieses einfache Beispiel keine umfangreiche manuelle Fehlerbehandlung erforderlich ist. Weitere Informationen zu diesem Feature von Rust finden Sie unter [Einfachere Fehlerbehandlung durch den ?-Operator](https://doc.rust-lang.org/edition-guide/rust-2018/error-handling-and-panics/the-question-mark-operator-for-easier-error-handling.html).

11. Zum Herunterladen des RSS-Feeds erstellen wir ein neues Objekt vom Typ **SyndicationClient**.

   ```rust
   // src\main.rs
   ...

   fn main() -> Result<()> {
       let uri = Uri::create_uri("https://blogs.windows.com/feed")?;
       let client = SyndicationClient::new()?;

       Ok(())
   }
   ```

   Die Funktion **new** ist das Rust-Äquivalent des Standardkonstruktors.

12. Nun können wir das Objekt **SyndicationClient** verwenden, um den Feed abzurufen.

   ```rust
   // src\main.rs
   ...

   fn main() -> Result<()> {
       let uri = Uri::create_uri("https://blogs.windows.com/feed")?;
       let client = SyndicationClient::new()?;
       let feed = client.retrieve_feed_async(uri)?.get()?;

       Ok(())
   }
   ```

Da es sich bei [**RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) um eine asynchrone API handelt, können wir die blockierende Funktion **Get** verwenden (wie oben gezeigt). Alternativ können wir auch den Operator `await` in einer Funktion vom Typ `async` verwenden, um ähnlich wie in C# oder C++ kooperativ auf die Ergebnisse zu warten.

13. Nun können wir einfach die resultierenden Elemente durchlaufen und die Ausgabe auf die Titel beschränken.

   ```rust
   // src\main.rs
   ...

   fn main() -> Result<()> {
       let uri = Uri::create_uri("https://blogs.windows.com/feed")?;
       let client = SyndicationClient::new()?;
       let feed = client.retrieve_feed_async(uri)?.get()?;

       for item in feed.items()? {
           println!("{}", item.title()?.text()?);
       }

       Ok(())
   }
   ```

14. Überprüfen Sie als Nächstes, ob das Erstellen und Ausführen funktioniert. Klicken Sie hierzu auf **Ausführen** > **Ohne Debuggen ausführen** (oder drücken Sie **STRG+F5**). Befehle zum **Debuggen** und **Ausführen** stehen auch im Text-Editor zur Verfügung. Alternativ können Sie an der Eingabeaufforderung den Befehl `cargo run` ausführen (nachdem Sie zuvor mithilfe von `cd` zum Ordner `rss_reader` gewechselt sind), um die Erstellung und die anschließende Ausführung zu initiieren.

   ![In den Text-Editor eingebettete Befehle zum Debuggen und Ausführen](../../images/rust-rss-reader-2.png)

   Weiter unten im Terminalbereich sehen Sie, dass Cargo die Kiste **windows** erfolgreich herunterlädt und kompiliert, die Ergebnisse zwischenspeichert und so nachfolgende Buildvorgänge beschleunigt **.** Anschließend wird das Beispiel erstellt und ausgeführt, und es wird eine Liste mit Blogbeitragstiteln angezeigt.

   ![Liste mit Blogbeitragstiteln](../../images/rust-rss-reader-3.png)

So einfach ist das Programmieren mit Rust für Windows. Im Hintergrund wird jedoch viel Arbeit in die Erstellung der Tools investiert, damit Rust zur Kompilierzeit Dateien vom Typ `.winmd` auf der Grundlage von [ECMA-335](https://www.ecma-international.org/publications-and-standards/standards/ecma-335/) (Common Language Infrastructure, CLI) analysieren sowie zur Laufzeit ordnungsgemäß die COM-basierte binäre Anwendungsschnittstelle (Application Binary Interface, ABI) unter Berücksichtigung von Sicherheits- und Effizienzaspekten nutzen kann.

## <a name="related"></a>Verwandte Themen

* [Rust für Windows und Windows-Kiste](rust-for-windows.md)
* [ECMA-335](https://www.ecma-international.org/publications-and-standards/standards/ecma-335/)
* [Einfachere Fehlerbehandlung durch den ?-Operator](https://doc.rust-lang.org/edition-guide/rust-2018/error-handling-and-panics/the-question-mark-operator-for-easier-error-handling.html)
