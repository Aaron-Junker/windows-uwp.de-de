---
title: Überblick über die Entwicklung unter Windows mit Rust
description: Eine Übersicht für Anfänger, die unter Windows mit Rust entwickeln möchten.
author: stevewhims
ms.author: stwhi
manager: jken
ms.topic: article
keywords: Rust, Windows 10, Microsoft, Rust lernen, Rust unter Windows für Anfänger, Rust mit VS Code
ms.localizationpriority: medium
ms.date: 03/04/2021
ms.openlocfilehash: 7d3d2cbe392fe54a82af982a23b866a745e993ae
ms.sourcegitcommit: 85b9a5fc16f4486bc23b4ec8f4fae5ab6211a066
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102194155"
---
# <a name="overview-of-developing-on-windows-with-rust"></a>Überblick über die Entwicklung unter Windows mit Rust

Der Einstieg in [Rust](https://www.rust-lang.org/) ist nicht schwer. Wenn Sie noch keine Erfahrung mit Rust unter Windows 10 haben, empfiehlt es sich, diesen Leitfaden Schritt für Schritt durchzugehen. Hier finden Sie Informationen zur Installation, und Sie erfahren, wie Sie Ihre Entwicklungsumgebung einrichten.

> [!TIP]
> Wenn Sie schon von Rust überzeugt sind, Ihre Rust-Umgebung bereits eingerichtet haben und nur noch Informationen zum Aufrufen von Windows-APIs benötigen, können Sie direkt mit dem Thema [Rust für Windows und die Kiste „windows“](rust-for-windows.md) fortfahren.

## <a name="what-is-rust"></a>Was ist Rust?

Rust ist eine Programmiersprache für Systeme und wird beispielsweise zum Programmieren von Betriebssystemen verwendet. Sie kann jedoch auch für Anwendungen verwendet werden, bei denen Leistung und Vertrauenswürdigkeit eine wichtige Rolle spielen. Rust hat eine ähnliche Syntax wie C++ und ist auch in puncto Leistung mit modernem C++ vergleichbar. Darüber hinaus trifft Rust bei vielen erfahrenen Entwicklern den richtigen Ton in Sachen Kompilierungs- und Laufzeitmodell, Typsystem und deterministischer Abschluss.

Darüber hinaus verspricht Rust garantierte Speichersicherheit ganz ohne Garbage Collection.

Warum also haben wir uns bei der neuesten Sprachprojektion für Windows Rust entschieden? Ein Faktor ist, dass Rust laut [jährlicher Entwicklerumfrage](https://insights.stackoverflow.com/survey) von Stack Overflow Jahr für Jahr die mit Abstand beliebteste Programmiersprache ist. Die Sprache hat zwar mitunter eine recht steile Lernkurve, doch wenn man einmal damit vertraut ist, fällt es schwer, sie nicht zu mögen.

Außerdem gehört Microsoft zu den Gründungsmitgliedern der [Rust Foundation](https://foundation.rust-lang.org/). Dabei handelt es sich um eine unabhängige gemeinnützige Organisation mit einem neuen Konzept für den Erhalt und Ausbau eines umfangreichen und beteiligungsorientierten Open-Source-Ökosystems.

## <a name="the-pieces-of-the-rust-development-toolsetecosystem"></a>Die Teile des Rust-Entwicklungstoolsets/-ökosystems

In diesem Abschnitt werden einige Rust-Tools und -Begriffe vorgestellt. Kehren Sie bei Bedarf hierher zurück, um Ihre Kenntnisse aufzufrischen.

* Eine *Kiste* ist eine Kompilierungs- und Verknüpfungseinheit in Rust. Eine Kiste kann als Quellcode vorliegen und von dort aus zu einer Kiste in Form einer binären ausführbaren Datei (kurz: *Binärdatei*) oder einer binären Bibliothek (kurz: *Bibliothek*) verarbeitet werden.
* Ein Rust-Projekt wird als *Paket* bezeichnet. Ein Paket enthält mindestens eine Kiste sowie eine Datei vom Typ `Cargo.toml` mit Informationen zur Kistenerstellung.
* `rustup` ist das Installations- und Aktualisierungsprogramm für die Rust-Toolkette.
* *Cargo* ist der Name des Paketverwaltungstools von Rust.
* `rustc` ist der Compiler für Rust. `rustc` wird in der Regel nicht direkt, sondern indirekt über Cargo aufgerufen.
* *crates.io* (`https://crates.io/`) ist die Kistenregistrierung der Rust-Community.

## <a name="setting-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

Im nächsten Thema erfahren Sie, wie Sie [Ihre Entwicklungsumgebung unter Windows für Rust einrichten](setup.md).

## <a name="related"></a>Verwandte Themen

* [Die Rust-Website](https://www.rust-lang.org/)
* [Rust für Windows und Windows-Kiste](rust-for-windows.md)
* [Jährliche Entwicklerumfrage von Stack Overflow](https://insights.stackoverflow.com/survey)
* [Rust Foundation](https://foundation.rust-lang.org/)
* [crates.io](https://crates.io/)
* [Einrichten Ihrer Entwicklungsumgebung unter Windows für Rust](setup.md)
