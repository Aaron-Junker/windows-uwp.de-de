---
Description: Erfahren Sie mehr über die Vorteile der Globalisierung und Lokalisierung Ihrer APP und genau, was diese Begriffe bedeuten.
Search.SourceType: Video
title: Globalisierung und Lokalisierung
ms.assetid: c0791eec-5bb8-4a13-8977-61d7d98e35ce
label: Intro
template: detail.hbs
ms.date: 12/07/2018
ms.topic: article
keywords: Windows 10, UWP, Globalisierung, Lokalisier barkeit, Lokalisierung
ms.localizationpriority: medium
ms.openlocfilehash: d60f0e825cefec0ba6ad5bcdd6a705f0992019b4
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2020
ms.locfileid: "82967915"
---
# <a name="globalization-and-localization"></a>Globalisierung und Lokalisierung

Windows wird auf der ganzen Welt von Anwendern unterschiedlicher Sprachen, Kulturen und Herkunft verwendet. Ihre Benutzer sprechen eine Vielzahl verschiedener Sprachen in vielen verschiedenen Ländern und Regionen. Einige Benutzer sprechen mehrere Sprachen. Daher wird Ihre APP in Konfigurationen ausgeführt, die viele Permutationen von Systemeinstellungen für Sprache, Region und Kultur umfassen. Sie können den potenziellen Markt für Ihre APP erhöhen, indem Sie Sie so entwerfen, dass Sie mithilfe von *Globalisierung* und *Lokalisierung*problemlos angepasst werden kann.

Dieses Video enthält eine kurze Einführung in die Vorbereitung Ihrer APP für die Welt: [Einführung in die Globalisierung und Lokalisierung](https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-globalization-and-localization).

Bei der **Globalisierung** handelt es sich um den Prozess, bei dem Ihre APP so entworfen und entwickelt wird, dass Sie auf verschiedenen globalen Märkten (auf Systemen mit unterschiedlichen sprach-und Kultur Konfigurationen) ordnungsgemäß funktioniert, ohne dass kulturspezifische Änderungen oder Anpassungen erforderlich sind.

- Berücksichtigen Sie beim Bearbeiten von Zeichen folgen die Kultur. ändern Sie beispielsweise die Groß-/Kleinschreibung von Zeichen folgen vor dem Vergleich
- Verwenden Sie Kalender, die für die aktuelle Kultur geeignet sind.
- Verwenden Sie Globalisierungs-APIs, um Daten anzuzeigen, die entsprechend für das Land oder die Region formatiert sind, z. b. Zahlen, Datumsangaben, Uhrzeiten und Währungen.
- Berücksichtigen Sie, dass unterschiedliche Kulturen verschiedene Regeln zum Sortieren (Sortieren) von Text und anderen Daten aufweisen.

Ihr Code muss in allen Kulturen, die von ihrer App unterstützt werden, gleichermaßen gut funktionieren. Im Idealfall funktioniert der Code im Kontext *jeder beliebigen* Sprache, Region oder Kultur gleichermaßen gut. Die effizienteste Methode zum Globalisieren der Funktionen Ihrer APP ist die Verwendung des Konzepts der Kulturen/Gebiets Schemas. Eine Kultur bzw. ein Gebiets Schema besteht aus einer Reihe von Regeln und einem Satz von Daten, die spezifisch für eine bestimmte Sprache und einen geografischen Bereich sind. Diese Regeln und Daten enthalten Informationen zu folgenden Informationen.

- Zeichenklassifizierung
- System wird geschrieben
- Formatieren von Datum und Uhrzeit
- Numerische, Währungs-, Gewichtungs-und Measure-Konventionen
- Sortierregeln

>[!NOTE]
> Eine Liste der unterstützten Gebiets Schema Namen in der Windows-Betriebssystemversion finden Sie in der Spalte Sprachtag der Tabelle in [Anhang a: Produktverhalten](https://docs.microsoft.com/openspecs/windows_protocols/ms-lcid/a9eac961-e77d-41a6-90a5-ce1a8b0cdb9c) in der [Referenz zur Windows-Sprach Code Kennung (LCID)](https://docs.microsoft.com/openspecs/windows_protocols/ms-lcid/70feba9f-294e-491e-b6eb-56532684c37f).

**Lokalisier barkeit** ist der Prozess, bei dem eine globalisierte App für die Lokalisierung vorbereitet und/oder überprüft wird, ob die APP für die Lokalisierung bereit ist. Eine ordnungsgemäße lokalisierbare App bedeutet, dass der spätere Lokalisierungsprozess keine funktionalen Fehler in der APP aufdecken wird. Die wichtigste Eigenschaft einer lokalisierbaren App besteht darin, dass der ausführbare Code ordnungsgemäß von den lokalisierbaren Ressourcen der APP getrennt wurde.

- Zeichen folgen, die in verschiedene Sprachen übersetzt werden, können stark variieren. Entwerfen Sie also Ihre Benutzeroberfläche, um unterschiedliche Textlängen und Schriftgrößen für Bezeichnungen und Texteingabe-Steuerelemente zu verarbeiten.
- Versuchen Sie, Text und/oder Kultur abhängige Materialien in Bildern zu vermeiden.
- Sie sollten keine Zeichen folgen und Kultur abhängigen Images im Code und Markup Ihrer APP hart codieren. Speichern Sie Sie stattdessen als Zeichen folgen-und Bild Ressourcen, damit Sie unabhängig von den erstellten Binärdateien Ihrer APP an verschiedene lokale Märkte angepasst werden können.
- Pseudo Lokalisierung Ihrer APP, um Lokalisier barkeits Probleme offenzulegen.

**Lokalisierung** ist der Prozess, bei dem die lokalisierbaren Ressourcen ihrer App angepasst oder übersetzt werden, um die sprach-, Kultur-und politischen Anforderungen der spezifischen lokalen Märkte zu erfüllen, die die APP unterstützen soll. Wenn Sie die Phase der Lokalisierung Ihrer APP erreichen, müssen Sie bei diesem Prozess keine Logik ändern, wenn Ihre APP lokalisiert werden kann.

- Übersetzen Sie die Zeichen folgen Ressourcen und andere Assets der APP für den neuen Markt.
- Ändern Sie alle Kultur abhängigen Images nach Bedarf.
- Dateien können auch je nach Region des Benutzers variieren, getrennt von der Sprache. Beispielsweise kann eine Karte abhängig von der Position des Benutzers verschiedene Rahmen aufweisen, die Bezeichnungen sollten jedoch der bevorzugten Sprache des Benutzers entsprechen.

Die meisten Lokalisierungsteams verwenden spezielle Tools, um den Prozess zu unterstützen. Beispielsweise durch wieder verwenden von Übersetzungen von wiederkehrendem Text.

| Artikel | BESCHREIBUNG |
|---------|-------------|
| [Richtlinien für Globalisierung](guidelines-and-checklist-for-globalizing-your-app.md) | Entwerfen und entwickeln Sie Ihre APP so, dass Sie auf Systemen mit unterschiedlichen sprach-und Kultur Konfigurationen ordnungsgemäß funktioniert. |
| [Grundlegendes zu Benutzerprofil Sprachen und App-Manifest-Sprachen](manage-language-and-region.md) | In diesem Thema werden die Begriffe "Benutzerprofil-Sprachliste", "App-Manifest-Sprachliste" und "App-Lauf Zeit Sprachliste" definiert. Diese Begriffe werden in diesem Thema und in anderen Themen dieses featurebereichs verwendet, daher ist es wichtig, zu wissen, was Sie bedeuten. |
| [Globalisieren von Datums-, Uhrzeit- und Zahlenformaten](use-global-ready-formats.md) | Entwerfen Sie Ihre APP so, dass Sie Global bereit ist, indem Sie Datumsangaben, Uhrzeiten, Zahlen, Telefonnummern und Währungen entsprechend formatieren. Anschließend können Sie Ihre APP später für weitere Kulturen, Regionen und Sprachen auf dem globalen Markt anpassen. |
| [Verwenden von Vorlagen und Mustern zum Formatieren von Datums- und Uhrzeitwerten](use-patterns-to-format-dates-and-times.md) | Verwenden Sie Klassen im [**Windows. Globalization. datetimeformatierung**](/uwp/api/windows.globalization.datetimeformatting?branch=live) -Namespace mit benutzerdefinierten Vorlagen und Mustern, um Datumsangaben und Uhrzeiten in genau dem gewünschten Format anzuzeigen. |
| [Anpassen von Layout und Schriftarten und Unterstützen von „Von rechts nach links“](adjust-layout-and-fonts--and-support-rtl.md) | Entwerfen Sie Ihre APP, um die Layouts und Schriftarten mehrerer Sprachen zu unterstützen, einschließlich der Fluss Richtung von rechts nach links. |
| [NumeralSystem-Werte](glob-numeralsystem-values.md) | In diesem Thema sind die Werte aufgeführt, die für die **numeralsystem** -Eigenschaft verschiedener Klassen im [**Windows. Globalization**](/uwp/api/windows.globalization?branch=live) -Namespace verfügbar sind. |
| [App lokalisierbar machen](prepare-your-app-for-localization.md) | Eine lokalisierte APP ist eine, die auf anderen Märkten, Sprachen oder Regionen lokalisiert werden kann, ohne funktionale Mängel in der APP zu decken. Die wichtigste Eigenschaft einer lokalisierbaren App besteht darin, dass der ausführbare Code ordnungsgemäß von den lokalisierbaren Ressourcen getrennt wurde. |
| [Internationale Schriftarten](loc-international-fonts.md) | Dieses Thema enthält eine Liste der verfügbaren Schriftarten für Windows-apps, die in anderen Sprachen als US-Englisch lokalisiert werden. |
| [Entwerfen einer App für bidirektionalen Text](design-for-bidi-text.md) | Entwerfen Sie Ihre APP so, dass bidirektionale Textunterstützung (Bidi) bereitgestellt wird, sodass Sie Skripts von links nach rechts und von rechts nach links geschriebenen Schreibsystemen kombinieren können. |
| [Verwenden des Multilingual App Toolkit 4.0](use-mat.md) | Das mehrsprachige App Toolkit (Mat) 4,0 ist in Microsoft Visual Studio 2017 und höher integriert, um Windows-apps Übersetzungsunterstützung, Übersetzungsdatei Verwaltung und Editor-Tools bereitzustellen. |
| [Mehrsprachige App-Toolkit 4,0 FAQ & Problembehandlung](mat-faq-troubleshooting.md) | Dieses Thema enthält Antworten auf häufig gestellte Fragen und Probleme im Zusammenhang mit dem mehrsprachigen App Toolkit (Mat) 4,0. |
| [UTF-8-Codepage verwenden](use-utf8-code-page.md) | UTF-8 ist die universelle Codepage für die Internationalisierung. |
| [Vorbereiten Ihrer Anwendung auf den Wechsel der japanischen Ära](japanese-era-change.md) | Hier erhalten Sie Informationen zum im Mai 2019 anstehenden Wechsel der japanischen Ära, und wie Sie Ihre Anwendung darauf entsprechend vorbereiten können. |
