---
Description: Wenn Sie eine neue Desktop-App erstellen möchten, müssen Sie zuerst entscheiden, ob Sie die Win32- und COM-API oder .NET verwenden möchten.
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: Auswählen Ihrer App-Plattform
ms.topic: article
ms.date: 11/04/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
keywords: Windows Win32, Desktopentwicklung
ms.openlocfilehash: d0d87f8e4b6524471ff5e2ada9012a22641b06d7
ms.sourcegitcommit: ddf0137929945eddf01041a81aa4d26038e70f46
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2019
ms.locfileid: "74392093"
---
# <a name="choose-your-app-platform"></a>Auswählen Ihrer App-Plattform

Wenn Sie eine neue Desktop Anwendung für Windows-PCs erstellen möchten, müssen Sie zuerst entscheiden, welche Anwendungsplattform Sie verwenden möchten. Windows stellt vier Hauptanwendungsplattformen mit jeweils unterschiedlichen Stärken bereit:

* [Universelle Windows-Plattform (UWP)](#uwp)
* [WPF (.NET)](#wpf)
* [Windows Forms (.NET)](#windows-forms)
* [Win32](#win32)

Mit all diesen Anwendungsplattformen können Sie Desktop-Apps wie Word, Excel und Photoshop erstellen, die auf dem klassischen Windows-Desktop ausgeführt werden, und die spezifischen Features dieser Umgebung in vollem Umfang nutzen. Einigen dieser Plattformen sind jedoch spezifische Merkmale gemein, und sie eignen sich besser für bestimmte Arten von Anwendungen:

* **UWP, WPF und Windows Forms**. Diese Plattformen bieten verwaltete Laufzeitumgebungen (die Windows-Runtime für UWP und .NET für Windows Forms und WPF) mit vielen Vorteilen, insbesondere in den Bereichen der Entwicklerproduktivität, der ausgereiften und anpassbaren Benutzeroberfläche und der Anwendungssicherheit. Da diese Frameworks visuelle Designer und Benutzeroberflächen-Markup für die schnelle Erstellung von Benutzeroberflächen unterstützen, sind sie für branchenspezifische Anwendungen besonders gut geeignet.

* **Win32 API**. Die Win32-API (auch als Windows-API bezeichnet) ist die ursprüngliche Plattform für native C/C++-Windows-Anwendungen, die direkten Zugriff auf Windows und Hardware erfordern. Sie bietet eine erstklassige Entwicklungsumgebung ohne Abhängigkeit von einer verwalteten Laufzeitumgebung wie .NET und WinRT. Dadurch ist die Win32-API die Plattform der Wahl für Anwendungen, die das höchste Maß an Leistung und den direkten Zugriff auf die Systemhardware benötigen.

In diesem Artikel werden diese Plattformen ausführlicher beschrieben, damit Sie die beste Lösung für Ihre Anwendung ermitteln können. 

> [!NOTE]
> Unabhängig davon, welche App-Plattform Sie auswählen, können Sie viele Features der universellen Windows-Plattform (UWP) verwenden, um in Ihrer App unter Windows 10 eine moderne Benutzerumgebung bereitzustellen. Selbst wenn Ihre Desktop-App mithilfe von WPF, Windows Forms oder der Win32-API erstellt wurde, können Sie beispielsweise weiterhin viele Features verwenden, die zuerst mit UWP eingeführt wurden, wie z.B. die MSIX-Paketbereitstellung und UWP-XAML-Steuerelemente. Weitere Informationen finden Sie unter [Modernisieren Ihrer Desktop-Apps](modernize/index.md).

## <a name="uwp"></a>UWP

UWP ist die führende Plattform für Windows 10-Anwendungen und Spiele. Es handelt sich um eine hochgradig anpassbare Plattform, die XAML-Markup verwendet, um die Benutzeroberfläche (Darstellung) vom Code (Geschäftslogik) zu trennen. UWP eignet sich für Desktopanwendungen, für die eine ausgereifte Benutzeroberfläche, die Anpassung von Formatvorlagen sowie grafikintensive Szenarien erforderlich sind. UWP verfügt zudem über eine integrierte Unterstützung für das [Fluent Design-System](/windows/uwp/design/fluent-design-system/) für die Standard-UX-Erfahrung und ermöglicht den Zugriff auf die [Windows-Runtime-APIs (WinRT)](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis). Durch die Einführung von Fluent unterstützt UWP automatisch gängige Eingabemethoden wie z.B. Freihandeingaben, Toucheingaben, Gamepad, Tastatur und Maus.

Nicht nur können Sie UWP zum Erstellen von Desktopanwendungen für Windows-PCs verwenden, sondern UWP ist auch die einzige unterstützte Plattform für Xbox-, HoloLens- und Surface Hub-Anwendungen. UWP ist unsere neueste, führende Anwendungsplattform.

Weitere Informationen zu UWP finden Sie in den folgenden Artikeln:

* [Erste Schritte](/windows/uwp/get-started/)
* [Design und UI](/windows/uwp/design/)
* [Technologien und Features](/windows/uwp/develop/)
* [API-Referenz](/uwp/)
* [Beispiele](https://github.com/Microsoft/Windows-universal-samples)

## <a name="wpf"></a>WPF

WPF ist die etablierte Plattform für verwaltete Windows-Anwendungen mit Zugriff auf .NET Core oder das gesamte .NET Framework und verwendet ebenfalls XAML-Markup, um die Benutzeroberfläche vom Code zu trennen. Diese Plattform wurde für Desktopanwendungen entwickelt, für die eine ausgereifte Benutzeroberfläche, die Anpassung von Formatvorlagen sowie grafikintensive Szenarien erforderlich sind. Die WPF-Entwicklungsfunktionen ähneln den UWP-Entwicklungsfunktionen, sodass die Migration von WPF- zu UWP-Apps einfacher ist als die Migration von Windows Forms.

Weitere Informationen zu WPF finden Sie in den folgenden Artikeln:

* [Erste Schritte (WPF)](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/).
* [Erstellen Ihrer ersten App (.NET Core)](/visualstudio/get-started/csharp/tutorial-wpf/)
* [Erstellen Ihrer ersten App (.NET Framework)](/dotnet/framework/wpf/getting-started/walkthrough-my-first-wpf-desktop-application/)
* [Migrieren von WPF-Apps zu .NET Core](/dotnet/desktop-wpf/migration/convert-project-from-net-framework/)
* [API-Referenz (.NET)](https://docs.microsoft.com/dotnet/api/index)
* [Beispiele](https://github.com/Microsoft/WPF-Samples)

## <a name="windows-forms"></a>Windows Forms

Windows Forms ist die ursprüngliche Plattform für verwaltete Windows-Anwendungen mit einem einfachen Benutzeroberflächenmodell und Zugriffsmöglichkeit auf .NET Core oder das gesamte .NET Framework. Es ist hervorragend geeignet, Entwickler einen schnellen Einstieg in die Entwicklung von Anwendungen zu ermöglichen, auch für Entwickler, die noch keine Erfahrungen mit der Plattform haben. Es handelt sich um eine formularbasierte, schnelle Anwendungsentwicklungsplattform mit einer großen integrierten Sammlung von visuellen und nicht visuellen Drag & Drop-Steuerelementen. Windows Forms verwendet kein XAML. Wenn Sie also später beschließen, die Anwendung auf UWP zu erweitern, müssen Sie die Benutzeroberfläche vollständig neu schreiben.

Weitere Informationen zu Windows Forms finden Sie in den folgenden Artikeln:

* [Erste Schritte mit Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/getting-started-with-windows-forms)
* [Erstellen Ihrer ersten Windows Forms-App](/dotnet/framework/winforms/creating-a-new-windows-form)
* [Tutorial: Erstellen eines Bildanzeigeprogramms](/visualstudio/ide/tutorial-1-create-a-picture-viewer?view=vs-2019)
* [API-Referenz (.NET)](https://docs.microsoft.com/dotnet/api/index)
* [Erweitern von Windows Forms-Apps](/dotnet/framework/winforms/advanced/)

## <a name="win32"></a>Win32

Durch Verwendung der Win32-API mit C++ können Sie höchste Leistung und Effizienz erzielen, indem Sie über die Zielplattform mit nicht verwaltetem Code mehr Kontrolle erhalten, als dies in einer verwalteten Laufzeitumgebung wie WinRT und .NET möglich ist. Einen solchen Grad an Kontrolle über die Ausführung Ihrer Anwendung auszuüben, setzt jedoch größere Sorgfalt und Aufmerksamkeit voraus, um Fehler zu vermeiden, und tauscht Entwicklungsproduktivität gegen Laufzeitleistung ein.

Hier finden Sie einige Highlights der Win32-API mit C++, die Ihnen die Erstellung von Hochleistungsanwendungen ermöglichen.

* Optimierungen auf Hardwareebene, einschließlich strenger Kontrolle der Ressourcenzuordnung, Objektlebensdauer, Datenlayout, Ausrichtung, Bytepakete und mehr.
* Zugriff auf leistungsorientierte Anweisungssätze wie SSE und AVX durch intrinsische Funktionen.
* Effiziente, typsichere generische Programmierung mithilfe von Vorlagen.
* Effiziente und sichere Container und Algorithmen.
* DirectX, insbesondere Direct3D und DirectCompute (Hinweis: UWP bietet ebenfalls DirectX-Interoperabilität).

Weitere Informationen finden Sie in den folgenden Artikeln:

* [Erste Schritte](/windows/win32/desktop-programming/)
* [Erstellen Ihrer ersten Win32- und C++-App](/windows/win32/learnwin32/learn-to-program-for-windows/)
* [Technologien und Features](/windows/win32/desktop-app-technologies)
* [API-Referenz](/windows/win32/apiindex/windows-api-list/)
* [Beispiele](https://github.com/Microsoft/Windows-classic-samples)

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>Plattformvergleich: UWP, WPF und Windows Forms

In der folgenden Tabelle werden die verschiedenen Merkmale von Windows Forms, WPF und UWP detailliert verglichen.

| Funktion oder Szenario  |    UWP     |      WPF     |   Windows Forms  |
|--------|--------|--------|--------|
| **Unterstützte Versionen**      |  Windows 10   |  Windows 7 und höher |  Windows 7 und höher  |
| **Sprachen**      |   C\#, C++/WinRT, C++/CX, VB, JavaScript   |  C\#, C++/CLI (verwaltete Erweiterungen für C++), F\#, VB |  C\#, C++/CLI (verwaltete Erweiterungen für C++), F\#, VB   |
| **Benutzeroberflächenlaufzeit** |    Nativ (C++/WinRT und C++/CX) und verwaltet (.NET Native)  |  Verwaltet (.NET Framework und .NET Core 3) |   Verwaltet (.NET Framework und .NET Core 3)   |
| **Open Source** | [Ja (nur Windows-Benutzeroberflächenbibliothek)](https://github.com/Microsoft/microsoft-ui-xaml)  |  [Ja (nur .NET Core)](https://github.com/dotnet/wpf) | [Ja (nur .NET Core)](https://github.com/dotnet/winforms)  |
| **Unterstützt XAML** |   Ja   |  Ja  |   Nein   |
| **Stärken**  |  <ul><li>XAML-Markupdateien für Benutzeroberfläche</li><li>Umfassende und anpassbare Benutzeroberfläche</li><li>Ihre vorhandenen Codebasen sind kompatibel mit .NET Standard.</li><li>Unterstützung hoher DPI-Werte</li><li>Unterstützung für mehrere Eingabetypen auf Windows-Geräten (einschließlich Toucheingabe, Stift, Gamepad, Maus und Tastatur)</li><li>Unterstützung für Xbox, HoloLens, das Internet der Dinge (IoT) oder Surface Hub</li><li>Optionale dichte (kompakte) Benutzeroberfläche</li><li>Unterstützung für natives C++</li><li>Optimierte Akkulebensdauer</li><li>Unterstützung moderner Barrierefreiheit (z.B. Bildschirmsprachausgaben)</li><li>Umfassende Textdatenfunktionen (z.B. integrierte Rechtschreibprüfung)</li><li>Unterstützung für Freihandeingaben</li><li>Sichere Ausführung über Anwendungscontainer (nicht vertrauenswürdiger Inhalt z.B. über Sandbox)</li></ul>  |  <ul><li>XAML-Markupdateien für Benutzeroberfläche</li><li>Umfassende und anpassbare Benutzeroberfläche</li><li>Große Sammlung von Steuerelementen von Microsoft und Partnern</li><li>Dichte Benutzeroberfläche</li><li>Unterstützung für Windows 7</li><li>Plattformunterstützung für die Eingabevalidierung</li></ul> | <ul><li>Schnelle Anwendungsentwicklung</li><li>WYSIWYG-Editor zum Entwickeln der Benutzeroberfläche</li><li>Große Sammlung von Steuerelementen von Microsoft und Partnern</li><li>Dichte Benutzeroberfläche</li><li>Unterstützung für Windows 7</li><li>Tastatur- und Mauseingaben</li></ul>          |
| **Szenarien mit eingeschränkter Unterstützung** |  <ul><li>Unterstützung mehrerer Fenster<sup>1</sup></li><li>Plattformunterstützung für die Eingabevalidierung<sup>1</sup></li><li>Windows 7 wird nicht unterstützt</li><li>Einige UWP-APIs erfordern bestimmte Mindestversionen von Windows 10</li><li>Vollständige Plattformunterstützung und Shellintegration (UWP bietet z.B. derzeit keine Unterstützung für die Integration von Systemleisten oder Vollzugriff auf alle Geräte)</li><li>Direkter Zugriff auf alle Dateien auf dem Datenträger</li><li>ADO.NET</li><li>Vorhandene Codebasis-Klassenbibliotheken, die APIs verwenden, die nicht dem .NET-Standard entsprechen oder nicht mit dem Zertifizierungskit für Windows-Apps kompatibel sind</li><li>Unterstützung für das lokale Netzwerkloopback (d.h., wenn Ihre App mit localhost kommunizieren muss, ohne auf dem Zielgerät eine Loopbackausnahme zu erstellen)</li><li>Intensive Datei-E/A</li></ul>     |  <ul><li>Unterstützung hoher DPI-Werte<sup>2</sup></li><li>Toucheingabe<sup>2</sup></li></ul>  |  <ul><li>Unterstützung hoher DPI-Werte<sup>2</sup></li><li>Toucheingabe<sup>2</sup></li><li>Anpassbare Benutzeroberfläche</li><li>Umfassende Grafiken und Benutzeroberflächen (z.B. Toucheingabe und Animationen)</li><li>Umfassende Abstraktion von Sichten und Datenmodellen</li></ul>    |   |

<sup>1</sup> Wir haben Features in einer zukünftigen Version von Windows 10 öffentlich angekündigt, die diesem Szenario gerecht werden.

<sup>2</sup> Obwohl die Plattform keine erstklassige API-Unterstützung für dieses Szenario bietet, können Entwickler dieses Szenario mit Problemumgehungen unterstützen.

## <a name="other-app-platforms"></a>Andere App-Plattformen

### <a name="progressive-web-apps-pwas"></a>Progressive Web Apps (PWAs)

PWAs ermöglichen es Entwicklern, ihren Website-Code so zu verpacken, dass er wie eine Anwendung auf Windows 10-PCs installiert und ausgeführt werden kann. Weitere Informationen finden Sie unter [Progressive Web Apps](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started).

### <a name="xamarin"></a>Xamarin

Xamarin dient zum Erstellen von plattformübergreifenden Anwendungen für Windows 10, die auch unter iOS und Android verwendet werden können. Weitere Informationen finden Sie unter [Xamarin](https://docs.microsoft.com/xamarin/xamarin-forms/get-started/index).
