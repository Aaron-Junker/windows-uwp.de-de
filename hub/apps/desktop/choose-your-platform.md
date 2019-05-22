---
Description: Wenn Sie eine neue desktop-app erstellen möchten, ist die erste Entscheidung, die Sie treffen, ob der Win32 und COM-API oder .NET verwendet.
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: Wählen Sie Ihre app-Plattform
ms.topic: article
ms.date: 03/18/2019
ms.openlocfilehash: 960dda5e4cb7e8edc1edf7ce2e81da8306555f1c
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984490"
---
# <a name="choose-your-app-platform"></a>Wählen Sie Ihre app-Plattform

Wenn Sie eine neue Desktopanwendung für Windows-PCs erstellen möchten, ist die erste Entscheidung, die Sie treffen die Anwendungsplattform zu verwenden. Windows bietet vier hauptanwendung Plattformen, die jeweils unterschiedliche Vorteile:

* [Universelle Windows-Plattform (UWP)](#uwp)
* [WPF (.NET)](#wpf)
* [Windows Forms (.NET)](#windows-forms)
* [Win32](#win32)

Alle diese Plattformen können Sie die desktop-apps wie Word, Excel und Photoshop zu erstellen, die in der klassischen Windows Desktop- und Take vollem Umfang dieser Umgebung bestimmte Funktionen ausführen. Allerdings einige dieser Plattformen eine "traits" und eignen sich besser für bestimmte Arten von Anwendungen:

* **UWP, WPF und Windows Forms**. Diese Plattformen bieten verwaltete Laufzeitumgebungen (die Windows-Runtime für UWP und .NET für Windows Forms und WPF), zahlreiche Vorteile, insbesondere in den Bereichen der Produktivität von Entwicklern, moderne und anpassbare Benutzeroberfläche und anwendungssicherheit. Da diese Frameworks visuellen Designern und UI-Markup für das schnelle Erstellen einer Benutzeroberfläche unterstützen, sind sie besonders gut geeignet für Line-of-Business-Anwendungen.

* **Win32 API**. Die Win32-API (auch als die Windows-API bezeichnet) ist die ursprüngliche Plattform für systemeigene C /C++ Windows-Anwendungen, die direkten Zugriff auf Windows und Hardware erfordern. Es bietet eine erstklassige entwicklungserfahrung ohne abhängig von der eine verwaltete Laufzeitumgebung wie .NET und WinRT. Dadurch wird die Win32-API der Plattform der Wahl für Anwendungen, die höchste Leistung und direkten Zugriff auf die System-Hardware zu benötigen.

Dieser Artikel beschreibt diese Plattformen im Detail und können Sie die beste Option für Ihre Anwendung zu ermitteln.

## <a name="uwp"></a>UWP

UWP ist die führenden Plattform für Windows 10-Anwendungen und Spiele. Es ist eine hochgradig anpassbare Plattform, die XAML-Markup verwendet wird, um die Benutzererfahrung (Presentation) von Code (Geschäftslogik) zu trennen. UWP eignet sich für desktop-Anwendungen, die, die eine moderne Benutzeroberfläche, formatanpassung und grafikintensive Szenarien erfordern. UWP bietet auch integrierte Unterstützung für die [Fluent-Entwurfssystem](/windows/uwp/design/fluent-design-system/) für den Standard-UX-Erfahrung und ermöglicht den Zugriff auf die [Windows-Runtime (WinRT) APIs](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis). Durch Übernahme von Fluent, unterstützt die UWP automatisch allgemeine Eingabemethoden, z. B. Touch, Freihandeingaben, Gamepad, Tastatur und Maus.

Nicht nur können Sie zum Erstellen von desktopanwendungen für Windows-PCs UWP verwenden, sondern UWP ist auch die einzige unterstützte Plattform für Xbox, HoloLens und Surface Hub-Anwendungen. UWP ist unsere neuesten und fortschrittliche Anwendungsplattform.

Weitere Informationen zu UWP finden Sie unter [erste Schritte mit Windows 10-apps](/windows/uwp/get-started/).

## <a name="wpf"></a>WPF

WPF ist die Plattform für verwaltete Windows-Anwendungen mit Zugriff auf das vollständige .NET Framework und XAML-Markup außerdem verwendet, um die UX von Code zu trennen. Diese Plattform dient für desktop-Anwendungen, die eine moderne Benutzeroberfläche, formatanpassung und grafikintensive Szenarien erfordern. WPF-entwicklungsfähigkeiten ähneln UWP-entwicklungsfähigkeiten, daher Migration von WPF-zu UWP-apps einfacher als die Migration von Windows Forms.

Weitere Informationen zu WPF finden Sie unter [erste Schritte (WPF)](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/).

## <a name="windows-forms"></a>Windows Forms

Windows Forms ist die ursprüngliche Plattform für verwaltete Windows-Anwendungen mit einem einfachen Benutzeroberflächenmodell und den Zugriff auf das vollständige .NET Framework. Er hervorragend ermöglicht es Entwicklern einen schnellen Einstieg Erstellen von Anwendungen, auch für Entwickler, die für die Plattform noch. Dies ist eine Entwicklungsplattform für formularbasierte, eine schnelle Anwendung mit einer großen integrierte Sammlung von visual und nicht visuelle Drag & Drop-Steuerelemente. Windows Forms verwendet keine XAML, damit Sie später entscheiden, erweitern Sie Ihre Anwendung für die UWP ist ein vollständig neu schreiben Ihre Benutzeroberfläche verbunden.

Weitere Informationen zu Windows Forms, finden Sie unter [erste Schritte mit Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/getting-started-with-windows-forms).

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>Plattformvergleich: UWP, WPF und Windows Forms

Die folgende Tabelle vergleicht verschiedene Merkmale der Windows Forms, WPF und UWP im Detail.

| Funktion oder Szenario  |    UWP     |      WPF     |   Windows Forms  |
|--------|--------|--------|--------|
| **Unterstützte Versionen**      |  Windows 10   |  Windows 7 und höher |  Windows 7 und höher  |
| **Sprachen**      |   C\#, C++/WinRT, C++/CX, VB, JavaScript   |  C\#, C++/CLI (verwalteter Erweiterungen für C++), F\#, VB |  C\#, C++/CLI (verwalteter Erweiterungen für C++), F\#, VB   |
| **UI-Laufzeit** |    Native (C++"/ WinRT" und C++/CX) und verwaltet (.NET Native)  |  (Verwaltet (.NET Framework)<br/><br/>Unterstützung für .NET Core-3 wird bald verfügbar sein.  |   (Verwaltet (.NET Framework)<br/><br/>Unterstützung für .NET Core-3 wird bald verfügbar sein.    |
| **Open-source** | [Ja (nur Windows-Benutzeroberflächenbibliothek)](https://github.com/Microsoft/microsoft-ui-xaml)  |  [Ja (nur .NET Core)](https://github.com/dotnet/wpf) | [Ja (nur .NET Core)](https://github.com/dotnet/winforms)  |
| **Unterstützt XAML** |   Ja   |  Ja  |   Nein   |
| **Stärken**  |  <ul><li>XAML-Markup für die Benutzeroberfläche</li><li>Umfassende und anpassbare Benutzeroberfläche</li><li>Ihre vorhandenen Codebasen sind .NET Standard kompatibel</li><li>Unterstützung hoher DPI-Wert</li><li>Unterstützung für verschiedene Eingabetypen für Windows-Geräte (einschließlich Touch, Stift, Gamepad, Maus und Tastatur)</li><li>Unterstützung für Xbox, HoloLens, IoT oder Surface Hub</li><li>Unterstützung für nativeC++</li><li>Optimierte Akkuverbrauch gering zu halten</li><li>Moderne Accessibility-Unterstützung (z.B. die Sprachausgabe)</li><li>Rich-Text-Data-Funktionen (z. B. integrierte Rechtschreibprüfung)</li><li>Freihand-Unterstützung</li><li>Sichere Ausführung über Anwendungscontainer (z. B. von nicht vertrauenswürdigen Inhalt ist Sandbox)</li></ul>  |  <ul><li>XAML-Markup für die Benutzeroberfläche</li><li>Umfassende und anpassbare Benutzeroberfläche</li><li>Umfangreiche Sammlung von Steuerelementen von Microsoft und Partnern</li><li>Dichten UI</li><li>Unterstützung für Windows 7</li><li>Plattform unterstützen, für die eingabeüberprüfung</li></ul> | <ul><li>schnelle Anwendungsentwicklung</li><li>WYSIWYG-Editor zum Erstellen von Benutzeroberflächen</li><li>Umfangreiche Sammlung von Steuerelementen von Microsoft und Partnern</li><li>Dichten UI</li><li>Unterstützung für Windows 7</li><li>Tastatur- und Mauseingaben</li></ul>          |
| **Szenarien, die nur unterstützt begrenzt** |  <ul><li>Dichten UI (Erstellen einer dichten Benutzeroberflächenautomatisierungs erfordert benutzerdefinierte Stile)<sup>1</sup></li><li>Unterstützung für mehrere Fenster<sup>1</sup></li><li>Unterstützung für die eingabeüberprüfung<sup>1</sup></li><li>Windows 7 wird nicht unterstützt.</li><li>Einige UWP-APIs erfordern bestimmte Mindestversionen von Windows 10</li><li>Vollständige Unterstützung und Shell-Integration Platform (z. B. UWP derzeit unterstützt die Integration der Taskleiste oder Vollzugriff auf alle Geräte)</li><li>Direkter Zugriff auf alle Dateien auf dem Datenträger</li><li>ADO.NET</li><li>Vorhandene Codebasis Klassenbibliotheken, die nicht .NET Standard verwenden oder nicht - Windows-Zertifizierungskit für Apps konform-APIs</li><li>Lokales netzwerkloopback unterstützt (d. h., wenn Ihre app muss mit "localhost" zu kommunizieren, ohne dass eine Loopback-Ausnahme auf dem Zielgerät erstellt)</li><li>E/a-intensive Datei</li></ul>     |  <ul><li>Unterstützung hoher DPI-Wert<sup>2</sup></li><li>Fingereingabe<sup>2</sup></li></ul>  |  <ul><li>Unterstützung hoher DPI-Wert<sup>2</sup></li><li>Fingereingabe<sup>2</sup></li><li>Anpassbare Benutzeroberfläche</li><li>Sehr hohe Grafik- und Benutzer auftritt (z. B. Touch und Animationen)</li><li>Umfassende Abstraktion von Ansichten und Modellen</li></ul>    |   |

<sup>1</sup> öffentlich haben wir angekündigt, Funktionen, die dieses Szenario in einer zukünftigen Version von Windows 10 gerecht werden.

<sup>2</sup> , obwohl die Plattform auf erstklassige-API-Unterstützung für dieses Szenario fehlt, können Entwickler unterstützen dieses Szenario mit problemumgehungen.

## <a name="win32"></a>Win32

Mithilfe der Win32-API mit C++ macht es möglich, die ein Höchstmaß an Leistung und Effizienz zu erzielen, indem Sie mehr Kontrolle über die Zielplattform mit nicht verwaltetem Code als für eine verwaltete Laufzeitumgebung wie WinRT und .NET möglich ist. Jedoch gewissenhaft solchen Grad an Kontrolle über die Ausführung Ihrer Anwendung erfordert mehr Sorgfalt und Aufmerksamkeit verwirklichen, und Klassen-Entwicklungsproduktivität für Leistung zur Laufzeit.

Hier sind einige Highlights in welche die Win32-API und C++ bietet Ihnen die Erstellung von Anwendungen mit hoher Leistung zu aktivieren.

-   Hardware-Ebene-Optimierungen, einschließlich einer engen Kontrolle über die Ressourcenzuordnung "," Lebensdauer von Objekten "," Datenlayout "," Ausrichtung "," Byte, packen, und vieles mehr.
-   Legt fest, den Zugriff auf leistungsorientierter-Anweisung wie die SSE- und AVX über systeminterne Funktionen.
-   Effiziente, typsichere generische Programmierung mithilfe von Vorlagen.
-   Effiziente und sichere Container und Algorithmen.
-   DirectX, insbesondere auf Direct3D und DirectCompute (Beachten Sie, dass die UWP bietet auch DirectX-Interop).
-   C++ AMP.

Weitere Informationen finden Sie unter [erste Schritte mit Windows-desktop-apps, die die Win32-API verwenden](/windows/desktop/desktop-programming) und [Desktop-app-Technologien](/windows/desktop/desktop-app-technologies).

### <a name="win32-and-c-for-traditional-desktop-applications"></a>Win32 und C++ für herkömmliche desktop-Anwendungen

Wenn Sie eine desktop-Anwendung schreiben, C++, Sie können Win32 oder MFC auswählen, für die Benutzeroberfläche oder eine Vielzahl von Frameworks für Anwendungen von Drittanbietern, die auch nicht-Windows-Plattformen unterstützen.

-   **Win32:** Dies ist der Handle-basierte, C-Sprachen-API der Windows-Plattform, einschließlich aber nicht beschränkt auf die Funktionen der Benutzeroberfläche wie Windowing, Zeichnungen und UI-Steuerelemente. Da es sich um eine Low-Level "," C-Sprachen-API, die basierend auf Handles ist, ist es eine seltene Wahl zum Erstellen moderner, UI-intensiven apps. Allerdings bietet die folgenden grundlegenden APIs zum interagieren mit der Windows-Plattform und ist eine passende Wahl für apps, die einfache Benutzeroberflächenanforderungen aufweisen, oder nur die Windows-Benutzeroberfläche aus dem Weg so weit wie möglich bleiben, z. B. Spiele verwenden sollen.
-   **MFC (Microsoft Foundation Class Library):** Dies ist der altehrwürdige Anwendungsframework und UI-Bibliothek, die Windows-Entwickler seit 1992 bereitgestellt wurde. Es ist eine schlanke C++ Wrapper für die Win32-Handle-basierte, C-Sprachen API sowie objektorientierte Schnittstellen für viele der vordefinierten Windows-Standardsteuerelemente und andere Windows-Objekte. Obwohl viele moderne Benutzeroberflächen-Frameworks im .NET-Ökosystem MFC in der Einfachheit halber (warningthreshold) überschreiten, ist es immer noch das native UI-Framework Ihrer Wahl für viele C++ Entwickler für den Windows-Desktop-Anwendungen erstellen.
-   **Anwendungen von Drittanbietern Frameworks:** Da C++ können auf eine Vielzahl von Plattformen ausgeführt werden und ist nicht gebunden, Windows oder die .NET Runtime, Drittanbietern entwickelt haben, neue Anwendungen und Benutzeroberflächen-Frameworks für C++ auf die Entwicklung von plattformübergreifenden Anwendungen mit umfassender Benutzeroberflächen zu vereinfachen. Einige dieser Frameworks bieten ihren eigenen Aussehen und dem Verhalten, während andere, z. B. WxWidgets oder Qt verwenden oder den nativen Steuerelements-Satz von der Plattform zu emulieren. Verwenden diese Bibliotheken, ist es möglich, fast alle des Quellcodes einer Anwendung, zwischen verschiedenen Versionen der Anwendung gemeinsam nutzen, die unter Windows oder anderen Plattformen, wie z. B. OSX oder Linux ausgeführt.

## <a name="other-app-platforms"></a>Andere app-Plattformen

### <a name="progressive-web-apps-pwas"></a>Progressive Web-Apps (PWAs)

Von PWAs können Entwickler Packen ihrer Website zu codieren, damit es installiert und wie eine Anwendung auf Windows 10-PCs ausgeführt werden kann. Weitere Informationen finden Sie unter [Progressive Web-Apps](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started).

### <a name="xamarin"></a>Xamarin

Verwenden Sie Xamarin, zum Erstellen plattformübergreifender Anwendungen für Windows 10, die auch unter iOS ausführen können und Android. Weitere Informationen finden Sie unter [Xamarin](https://docs.microsoft.com/xamarin/xamarin-forms/get-started/index).
