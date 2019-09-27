---
Description: Wenn Sie eine neue Desktop-App erstellen möchten, treffen Sie zuerst die Verwendung der Win32-und com-API oder von .net.
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: Auswählen Ihrer App-Plattform
ms.topic: article
ms.date: 03/18/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: ff32e20c42f613bd9ac3dba9eada2cced0baa64c
ms.sourcegitcommit: f34deba1d4460d85ed08fe9648999fe03ff6a3dd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2019
ms.locfileid: "71316989"
---
# <a name="choose-your-app-platform"></a>Auswählen Ihrer App-Plattform

Wenn Sie eine neue Desktop Anwendung für Windows-PCs erstellen möchten, müssen Sie zuerst entscheiden, welche Anwendungsplattform verwendet werden soll. Windows stellt vier Haupt Anwendungsplattformen mit unterschiedlichen Stärken bereit:

* [Universelle Windows-Plattform (UWP)](#uwp)
* [WPF (.NET)](#wpf)
* [Windows Forms (.net)](#windows-forms)
* [Win32](#win32)

Mit all diesen Anwendungsplattformen können Sie Desktop-Apps wie Word, Excel und Photoshop erstellen, die auf dem klassischen Windows-Desktop ausgeführt werden, und die spezifischen Features dieser Umgebung in vollem Umfang nutzen. Einige dieser Plattformen weisen jedoch einige Merkmale auf und eignen sich besser für bestimmte Anwendungs Typen:

* **UWP, WPF und Windows Forms**. Diese Plattformen bieten verwaltete Laufzeitumgebungen (die Windows-Runtime für UWP und .net für Windows Forms und WPF) mit vielen Vorteilen, insbesondere in den Bereichen der Entwickler Produktivität, der ausgereiften und anpassbaren Benutzeroberfläche und der Anwendungssicherheit. Da diese Frameworks visuelle Designer und Benutzeroberflächen Markup für die schnelle Erstellung der Benutzeroberfläche unterstützen, sind Sie für Branchen Anwendungen besonders gut geeignet.

* **Win32-API**. Die Win32-API (auch als Windows-API bezeichnet) ist die ursprüngliche Plattform für NativeC++ C/Windows-Anwendungen, die direkten Zugriff auf Windows und Hardware benötigen. Sie bietet eine erstklassige Entwicklungsumgebung ohne Abhängigkeit von einer verwalteten Laufzeitumgebung wie .net und WinRT. Dadurch ist die Win32-API die Plattform der Wahl für Anwendungen, die das höchste Maß an Leistung und den direkten Zugriff auf die System Hardware benötigen.

In diesem Artikel werden diese Plattformen ausführlicher beschrieben, und Sie können die beste Lösung für Ihre Anwendung ermitteln. 

> [!NOTE]
> Unabhängig davon, welche App-Plattform Sie auswählen, können Sie viele Features des universelle Windows-Plattform (UWP) verwenden, um in Ihrer APP unter Windows 10 eine moderne Benutzerfunktion bereitzustellen. Wenn Ihre Desktop-App beispielsweise mithilfe von WPF, Windows Forms oder der Win32-API erstellt wurde, können Sie weiterhin viele Features verwenden, die zuerst mit UWP eingeführt wurden, wie z. b. die msix-Paket Bereitstellung und UWP-XAML-Steuerelemente. Weitere Informationen finden Sie unter [modernisieren ihrer Desktop-Apps](modernize/index.md).

## <a name="uwp"></a>UWP

UWP ist die führende Plattform für Windows 10-Anwendungen und-Spiele. Es handelt sich um eine hochgradig anpassbare Plattform, die das XAML-Markup verwendet, um UX (Darstellung) von Code (Geschäftslogik) zu trennen. UWP eignet sich für Desktop Anwendungen, für die eine ausgereifte Benutzeroberfläche, Formatvorlagen Anpassung und grafikintensive Szenarien erforderlich sind. UWP verfügt auch über eine integrierte Unterstützung für das [fließende Entwurfs System](/windows/uwp/design/fluent-design-system/) für die Standard-UX-Erfahrung und ermöglicht den Zugriff auf die [Windows-Runtime (WinRT)-APIs](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis). Durch die Einführung von "Deep" unterstützt UWP automatisch gängige Eingabemethoden wie z. b. frei Hand Eingaben, toucheingaben, Gamepad, Tastatur und Maus.

Sie können nicht nur UWP zum Erstellen von Desktop Anwendungen für Windows-PCs verwenden, sondern auch UWP ist die einzige unterstützte Plattform für Xbox-, hololens-und Surface Hub-Anwendungen. UWP ist unsere neueste, führende Anwendungsplattform.

Weitere Informationen zu UWP finden Sie unter [Einstieg in Windows 10-apps](/windows/uwp/get-started/).

## <a name="wpf"></a>WPF

WPF ist die etablierte Plattform für verwaltete Windows-Anwendungen mit Zugriff auf die vollständige .NET Framework und verwendet außerdem XAML-Markup, um die UX vom Code zu trennen. Diese Plattform ist für Desktop Anwendungen konzipiert, die eine ausgereifte Benutzeroberfläche, Stil Anpassung und grafikintensive Szenarios erfordern. Die WPF-Entwicklungsfähigkeiten ähneln den UWP-Entwicklungsfähigkeiten, sodass die Migration von WPF zu UWP-apps einfacher ist als die Migration von Windows Forms.

Weitere Informationen zu WPF finden Sie unter [Getting Started (WPF)](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/).

## <a name="windows-forms"></a>Windows Forms

Windows Forms ist die ursprüngliche Plattform für verwaltete Windows-Anwendungen mit einem vereinfachten Benutzeroberflächen Modell und Zugriff auf die vollständige .NET Framework. Es ist hervorragend, Entwickler zu ermöglichen, schnell mit dem Entwickeln von Anwendungen zu beginnen, auch für Entwickler, die noch keine neue Plattform haben Dabei handelt es sich um eine Formular basierte, schnelle Anwendungs Entwicklungsplattform mit einer großen integrierten Sammlung von visuellen und nicht visuellen Drag & Drop-Steuerelementen. Windows Forms verwendet nicht XAML. Wenn Sie also später die Anwendung auf UWP erweitern möchten, müssen Sie die Benutzeroberfläche vollständig neu schreiben.

Weitere Informationen zu Windows Forms finden Sie unter [Getting Started with Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/getting-started-with-windows-forms).

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>Platt Form Vergleich: UWP, WPF und Windows Forms

In der folgenden Tabelle werden die verschiedenen Merkmale von Windows Forms, WPF und UWP ausführlich verglichen.

| Funktion oder Szenario  |    UWP     |      WPF     |   Windows Forms  |
|--------|--------|--------|--------|
| **Unterstützte Versionen**      |  Windows 10   |  Windows 7 und höher |  Windows 7 und höher  |
| **Sprachen**      |   C\#, C++/WinRT, C++/CX, VB, JavaScript   |  C\#, C++/CLI (verwaltete Erweiterungen für C++), F\#, VB |  C\#, C++/CLI (verwaltete Erweiterungen für C++), F\#, VB   |
| **UI-Laufzeit** |    Native (C++/WinRT und C++/CX) und verwaltet (.net Native)  |  Verwaltet (.NET Framework und .net Core 3) |   Verwaltet (.NET Framework und .net Core 3)   |
| **Open Source** | [Ja (nur Windows-UI-Bibliothek)](https://github.com/Microsoft/microsoft-ui-xaml)  |  [Ja (nur .net Core)](https://github.com/dotnet/wpf) | [Ja (nur .net Core)](https://github.com/dotnet/winforms)  |
| **Unterstützt XAML** |   Ja   |  Ja  |   Nein   |
| **Stärken**  |  <ul><li>XAML-Markup für die Benutzeroberfläche</li><li>Umfassende und anpassbare UX</li><li>Die vorhandenen Codebasen sind .NET Standard kompatibel.</li><li>Hohe dpi-Unterstützung</li><li>Unterstützung für mehrere Eingabetypen auf Windows-Geräten (einschließlich Toucheingabe, Stift, Gamepad, Maus und Tastatur)</li><li>Unterstützung für Xbox, hololens, IOT oder Surface Hub</li><li>Unterstützung für NativeC++</li><li>Optimierte Akku Lebensdauer</li><li>Unterstützung moderner Barrierefreiheit (z. b. Bildschirm Sprachausgaben)</li><li>Rich-Text-Datenfunktionen (z. b. integrierte Rechtschreibprüfung)</li><li>Unterstützung für Inking</li><li>Sichere Ausführung über Anwendungs Container (z. b. nicht vertrauenswürdiger Inhalt ist Sandbox)</li></ul>  |  <ul><li>XAML-Markup für die Benutzeroberfläche</li><li>Umfassende und anpassbare UX</li><li>Große Sammlung von Steuerelementen von Microsoft und Partnern</li><li>Dichte Benutzeroberfläche</li><li>Unterstützung für Windows 7</li><li>Platt Form Unterstützung für die Eingabevalidierung</li></ul> | <ul><li>Schnelle Anwendungsentwicklung</li><li>WYSIWYG-Editor zum Entwickeln der Benutzeroberfläche</li><li>Große Sammlung von Steuerelementen von Microsoft und Partnern</li><li>Dichte Benutzeroberfläche</li><li>Unterstützung für Windows 7</li><li>Tastatur-und Maus Eingaben</li></ul>          |
| **Szenarios mit eingeschränkter Unterstützung** |  <ul><li>Dichte Benutzeroberfläche (Erstellen einer dichten Benutzeroberfläche erfordert benutzerdefinierte Stile)<sup>1</sup></li><li>Unterstützung mehrerer Fenster<sup>1</sup></li><li>Platt Form Unterstützung für die Eingabevalidierung<sup>1</sup></li><li>Windows 7 wird nicht unterstützt.</li><li>Einige UWP-APIs erfordern bestimmte Mindestversionen von Windows 10.</li><li>Vollständige Platt Form Unterstützung und Shellintegration (z. b. bietet UWP derzeit keine Unterstützung für die Integration von System leisten oder Vollzugriff auf alle Geräte)</li><li>Direkter Zugriff auf alle Dateien auf dem Datenträger</li><li>ADO.NET</li><li>Vorhandene Code Basisklassen Bibliotheken, die Non-.NET-Standard-oder nicht-Windows-zertifizierungskit-kompatible APIs verwenden</li><li>Unterstützung für das lokale Netzwerk Loopback (d. h., wenn Ihre APP mit localhost kommunizieren muss, ohne auf dem Zielgerät eine Loopback Ausnahme zu erstellen)</li><li>Intensive Datei-e/a</li></ul>     |  <ul><li>Hohe dpi-Unterstützung<sup>2</sup></li><li>Fingereingabe<sup>2</sup></li></ul>  |  <ul><li>Hohe dpi-Unterstützung<sup>2</sup></li><li>Fingereingabe<sup>2</sup></li><li>Anpassbare Benutzeroberfläche</li><li>Umfassende Grafiken und Benutzeroberflächen (z. b. berühren und Animationen)</li><li>Umfassende Abstraktion von Sichten und Datenmodellen</li></ul>    |   |

<sup>1</sup> wir haben in einer zukünftigen Version von Windows 10 Features öffentlich angekündigt, die diesem Szenario gerecht werden.

<sup>2</sup> obwohl die Plattform erstklassige API-Unterstützung für dieses Szenario hat, können Entwickler dieses Szenario mit Problem Umgehungen unterstützen.

## <a name="win32"></a>Win32

Durch die Verwendung der Win32 C++ -API mit können Sie die höchste Leistung und Effizienz erzielen, indem Sie die Zielplattform mit nicht verwaltetem Code besser steuern, als dies in einer verwalteten Laufzeitumgebung wie z. b. WinRT und .net möglich ist. Das Ausführen einer solchen Steuerungsebene für die Ausführung Ihrer Anwendung erfordert jedoch eine größere Sorgfalt und Aufmerksamkeit, um das richtige zu erreichen und die Entwicklungsproduktivität für die Laufzeitleistung zu entwickeln.

Hier finden Sie einige Highlights, die die Win32-API C++ und bietet, damit Sie Hochleistungsanwendungen erstellen können.

-   Optimierungen auf Hardware Ebene, einschließlich enger Kontrolle über die Ressourcen Zuordnung, Objekt Lebensdauer, Datenlayout, Ausrichtung, Byte Verpackung und mehr.
-   Zugriff auf leistungsorientierte Anweisungs Sätze wie SSE und AVX durch intrinsische Funktionen.
-   Effiziente, typsichere generische Programmierung mithilfe von Vorlagen.
-   Effiziente und sichere Container und Algorithmen.
-   DirectX, insbesondere Direct3D und DirectCompute (Beachten Sie, dass UWP auch DirectX-Interop bietet).
-   C++LAGER.

Weitere Informationen finden Sie unter [Einstieg in Windows-Desktop-Apps, die die Win32-API](/windows/desktop/desktop-programming) und [Desktop-App-Technologien](/windows/desktop/desktop-app-technologies)verwenden.

### <a name="win32-and-c-for-traditional-desktop-applications"></a>Win32 und C++ für herkömmliche Desktop Anwendungen

Wenn Sie eine Desktop Anwendung in C++schreiben, können Sie Win32 oder MFC für die Benutzeroberfläche oder einen Host von Anwendungs Frameworks von Drittanbietern auswählen, die auch nicht-Windows-Plattformen unterstützen.

-   **Win32** Dies ist die Handle-basierte API der Windows-Plattform (C-Language), einschließlich der Benutzeroberflächen Funktionen, wie z. b. Windowing, Drawing und UI-Steuerelemente. Da es sich um eine API auf niedriger Ebene auf der Basis von Handles handelt, ist es eine seltene Wahl, um moderne, Benutzeroberflächen intensive apps zu erstellen. Sie stellt jedoch die grundlegenden APIs bereit, die für die Interaktion mit der Windows-Plattform erforderlich sind, und ist eine geeignete Wahl für apps, die einfache Anforderungen an die Benutzeroberfläche haben oder die Windows-Benutzeroberfläche so weit wie möglich verlassen möchten, z. b. Spiele.
-   **MFC (Microsoft Foundation Class-Bibliothek):** Dies ist das ehrwürdige Anwendungs Framework und die UI-Bibliothek, das seit 1992 Windows-Entwicklern bereitgestellt hat. Es ist ein schlanker C++ Wrapper über die Handle-basierte Win32-API der C-Sprache und stellt objektorientierte Schnittstellen für viele der vordefinierten Fenster, allgemeinen Steuerelemente und anderen Windows-Objekten bereit. Obwohl viele moderne Benutzeroberflächen-Frameworks im .NET-Ökosystem MFC in der Benutzeroberfläche überschreiten, ist es immer noch das C++ Native Benutzeroberflächen Framework, das für viele Entwickler geeignet ist, die Anwendungen für den Windows-Desktop
-   **Anwendungs Frameworks von Drittanbietern:** Da C++ auf einer Vielzahl von Plattformen ausgeführt werden kann und nicht an Windows oder die .NET-Runtime gebunden ist, haben Drittanbieter neue Anwendungs-und Benutzer C++ Oberflächen-Frameworks für entwickelt, um die Entwicklung von plattformübergreifenden Anwendungen mit umfangreichen Benutzeroberflächen zu vereinfachen. Einige dieser Frameworks bieten ein eigenes Erscheinungsbild &, während andere, z. b. wxWidgets oder Qt, den nativen Steuerungs Satz der Plattform verwenden oder emulieren. Mithilfe dieser Bibliotheken ist es möglich, fast den gesamten Quellcode einer Anwendung für die Versionen der Anwendung freizugeben, die unter Windows oder anderen Plattformen wie z. b. OSX oder Linux ausgeführt werden.

## <a name="other-app-platforms"></a>Andere APP-Plattformen

### <a name="progressive-web-apps-pwas"></a>Progressive Web-Apps (PWAs)

PWAs ermöglicht Entwicklern das Verpacken Ihres Website Codes, sodass er wie eine Anwendung auf Windows 10-PCs installiert und ausgeführt werden kann. Weitere Informationen finden Sie unter [Progressive Web-Apps](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started).

### <a name="xamarin"></a>Xamarin

Verwenden Sie xamarin zum Erstellen plattformübergreifender Anwendungen für Windows 10, die auch unter IOS und Android ausgeführt werden können. Weitere Informationen finden Sie unter [xamarin](https://docs.microsoft.com/xamarin/xamarin-forms/get-started/index).
