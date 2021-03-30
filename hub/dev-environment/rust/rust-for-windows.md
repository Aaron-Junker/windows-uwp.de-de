---
title: Rust für Windows und die Kiste *windows*
description: Hier erfahren Sie, wie Sie die Kiste *windows* verwenden und Windows-APIs aufrufen.
author: stevewhims
ms.author: stwhi
manager: jken
ms.topic: article
keywords: Rust, Windows 10, Microsoft, Rust lernen, Rust unter Windows für Anfänger, Rust mit VS Code, Rust für Windows
ms.localizationpriority: medium
ms.date: 03/04/2021
ms.openlocfilehash: aa3288d01207b7f51fe0f63ea996c4ea5cbdde48
ms.sourcegitcommit: 85b9a5fc16f4486bc23b4ec8f4fae5ab6211a066
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102194159"
---
# <a name="rust-for-windows-and-the-windows-crate"></a>Rust für Windows und die Kiste *windows*

&nbsp;
> [!VIDEO https://www.youtube.com/embed/LajquCjHXK4]

## <a name="introducing-rust-for-windows"></a>Einführung in Rust für Windows

Im [Überblick über die Entwicklung unter Windows mit Rust](overview.md) wurde eine einfache App gezeigt, die die Nachricht *Hello, world!* ausgibt. Sie können Rust aber nicht nur *unter* Windows verwenden, sondern mit Rust auch Apps *für* Windows schreiben.

Rust für Windows befindet sich aktuell in der Vorschauphase und ist die neueste Sprachprojektion für Windows. Mit Rust für Windows können Sie eine beliebige alte, aktuelle oder zukünftige Windows-API direkt und nahtlos über die [Kiste *windows*](https://crates.io/crates/windows) aufrufen. (Als *Kiste* wird in Rust eine Binärdatei oder Bibliothek bzw. der Quellcode bezeichnet, der zu einer Binärdatei oder Bibliothek verarbeitet wird.)

Ganz gleich, ob es sich um zeitlose Funktionen wie [CreateEventW](/windows/win32/api/synchapi/nf-synchapi-createeventw) und [WaitForSingleObject](/windows/win32/api/synchapi/nf-synchapi-waitforsingleobject), um leistungsstarke Grafikengines wie [Direct3D](/windows/win32/direct3d12/directx-12-programming-guide), um herkömmliche Fensterfunktionen wie [CreateWindowExW](/windows/win32/api/winuser/nf-winuser-createwindowexw) und [DispatchMessageW](/windows/win32/api/winuser/nf-winuser-dispatchmessagew) oder um neuere Benutzeroberflächenframeworks wie [Composition](/uwp/api/windows.ui.composition) und [XAML](/uwp/api/windows.ui.xaml) handelt: die [Kiste *windows*](https://crates.io/crates/windows) hat, was Sie brauchen.

Das Projekt [win32metadata](https://github.com/microsoft/win32metadata) dient dazu, Metadaten für Win32-APIs bereitzustellen. Diese Metadaten beschreiben die API-Oberfläche – stark typisierte API-Signaturen, -Parameter und -Typen. Dadurch kann die gesamte Windows-API automatisiert und vollständig für die Nutzung durch Rust (sowie durch Sprachen wie C# und C++) projiziert werden. Weitere Informationen finden Sie unter [Verbessern der Zugänglichkeit zu Win32-APIs für mehr Sprachen](https://blogs.windows.com/windowsdeveloper/2021/01/21/making-win32-apis-more-accessible-to-more-languages/).

Als Rust-Entwickler verwenden Sie Cargo (Paketverwaltungstool von Rust) zusammen mit `https://crates.io` (Kistenregistrierung der Rust-Community), um die Abhängigkeiten in Ihren Projekten zu verwalten. Die gute Nachricht ist, dass Sie in Ihren Rust-Apps auf die [Kiste *windows*](https://crates.io/crates/windows) verweisen und dann sofort Windows-APIs aufrufen können. Die [Rust-Dokumentation für die Kiste *windows*](https://docs.rs/windows/0.3.1/windows/) finden Sie hier: `https://docs.rs`.

Ähnlich wie bei [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/) ist [Rust für Windows](https://github.com/microsoft/windows-rs) eine auf GitHub entwickelte Open-Source-Sprachprojektion. Verwenden Sie das [Repository von Rust für Windows](https://github.com/microsoft/windows-rs), wenn Sie Fragen zu Rust für Windows haben oder Probleme melden möchten.

Das Repository von Rust für Windows enthält auch [einige einfache Beispiele](https://github.com/microsoft/windows-rs/tree/master/examples), die Sie sich ansehen können. Und es gibt eine ausgezeichnete Beispiel-App in Form von [Minesweeper](https://github.com/robmikh/minesweeper-rs) von Robert Mikhayelyan.

## <a name="contribute-to-rust-for-windows"></a>Mitwirken an Rust für Windows

Beiträge zu [Rust für Windows](https://github.com/microsoft/windows-rs) sind jederzeit willkommen!

* Identifizieren und Beheben von Bugs im [Quellcode](https://github.com/microsoft/windows-rs/tree/master/src)

## <a name="rust-documentation-for-the-windows-api"></a>Rust-Dokumentation für die Windows-API

Rust für Windows profitiert von der optimierten Toolkette, die Rust-Entwicklern zur Verfügung steht. Sollte jedoch der Gedanke, Zugriff auf die gesamte Windows-API zu haben, für Sie etwas abschreckend sein, gibt es auch eine [Rust-Dokumentation für die Windows-API](https://microsoft.github.io/windows-docs-rs/doc/bindings/windows/).

Darin ist im Wesentlichen dokumentiert, wie die Windows-APIs und -Typen in idiomatisches Rust projiziert werden. In dieser Ressource können Sie die APIs durchstöbern oder nach den benötigten APIs suchen und sich darüber informieren, wie sie aufgerufen werden.

## <a name="writing-an-app-with-rust-for-windows"></a>Schreiben einer App mit Rust für Windows

Das nächste Thema ist das [Tutorial zu RSS Reader](rss-reader-rust-for-windows.md). Dort erfahren Sie Schritt für Schritt, wie Sie eine einfache App mit Rust für Windows schreiben.

## <a name="related"></a>Verwandte Themen

* [Überblick über die Entwicklung unter Windows mit Rust](overview.md)
* [Tutorial zu RSS Reader](rss-reader-rust-for-windows.md)
* [Kiste *windows*](https://crates.io/crates/windows)
* [Dokumentation zur Kiste *windows*](https://docs.rs/windows/0.3.1/windows/)
* [Win32-Metadaten](https://github.com/microsoft/win32metadata)
* [Verbessern der Zugänglichkeit zu Win32-APIs für mehr Sprachen](https://blogs.windows.com/windowsdeveloper/2021/01/21/making-win32-apis-more-accessible-to-more-languages/).
* [Rust-Dokumentation für die Windows-API](https://microsoft.github.io/windows-docs-rs/doc/bindings/windows/)
* [RUST für Windows](https://github.com/microsoft/windows-rs)
* [Minesweeper-Beispiel-App](https://github.com/robmikh/minesweeper-rs)
