---
description: C++/WinRT ist eine vollkommen standardmäßige, moderne C++17-Programmiersprache für Windows-Runtime-APIs (WinRT), die als headerdateibasierte Bibliothek implementiert ist.
title: C++/WinRT
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projektion
ms.localizationpriority: medium
ms.openlocfilehash: 61a54edc236f94bec44420471a176a2014fcdb0d
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79208796"
---
# <a name="cwinrt"></a>C++/WinRT

[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) ist eine vollständig standardisierte, moderne C++17-Programmiersprache für Windows-Runtime-APIs (WinRT), die als headerdateibasierte Bibliothek implementiert ist und Ihnen einen erstklassigen Zugriff auf die moderne Windows-API bietet. Mit C++/WinRT können Sie Windows-Runtime-APIs mit jedem standardkonformen C++17-Compiler erstellen und verwenden. Das in Version 10.0.17134.0 (Windows 10, Version 1803) eingeführte Windows SDK enthält C++/WinRT.

C++/WinRT ist für Entwickler gedacht, die schönen und schnellen Code für Windows schreiben möchten. Die Begründung hierfür finden Sie unten.

## <a name="the-case-for-cwinrt"></a>Vorteile von C++/WinRT
&nbsp;
> [!VIDEO https://www.youtube.com/embed/TLSul1XxppA]

Die Programmiersprache C++ wird sowohl im Enterprise- *als auch* im Independent Software Vendor (ISV)-Segment für Anwendungen eingesetzt, bei denen ein hohes Maß an Korrektheit, Qualität und Leistung wichtig ist. Dies sind zum Beispiel: Systemprogrammierung, integrierte und mobile Systeme mit Ressourcenbeschränkung, Spiele und Grafiken, Gerätetreiber und industrielle, wissenschaftliche und medizinische Anwendungen.

Aus Sicht der Sprache ging es bei C++ immer darum, Abstraktionen zu erstellen und zu nutzen, die sowohl typenreich als auch einfach sind. Aber die Sprache hat sich seit den grundlegenden Zeigern und Schleifen und der mühsamen Speicherzuweisung und -freigabe von C++ aus dem Jahr 1998 radikal verändert. Modernes C++ (ab C++11) ist ideenreicher, einfacher, lesbarer und viel weniger fehleranfällig.

Für die Erstellung und Nutzung von Windows-Runtime-APIs mit C++ gibt es C++/WinRT. Dies ist der von Microsoft empfohlene Ersatz für die [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live)-Programmiersprache und die [C++-Vorlagenbibliothek für Windows-Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live).

Mit C++/WinRT verwenden Sie Standard-C++ Datentypen, -Algorithmen und -Schlüsselwörter. Die Projektion hat ihre eigenen benutzerdefinierten Datentypen, aber in den meisten Fällen müssen Sie sie nicht kennen, da es entsprechende Konvertierungen in und aus Standardtypen gibt. Auf diese Weise können Sie weiterhin die gewohnten Standardsprachfunktionen von C++ und den bereits vorhandenen Quellcode verwenden. C++/WinRT macht es extrem einfach, Windows-Runtime-APIs in jeder C++ Anwendung aufzurufen – von Win32 bis UWP.

C++/WinRT arbeitet besser und erzeugt kleinere Binärdateien als jede andere Sprachoption für die Windows-Runtime. Sie übertrifft sogar handgeschriebenen Code, der die ABI-Schnittstellen direkt nutzt. Dies liegt daran, dass die Abstraktionen moderne C++-Idiome verwenden, die vom Visual C++ Compiler optimiert werden. Hierzu gehören magische Statics, leere Basisklassen, **strlen**-Elision, sowie viele neuere Optimierungen in der neuesten Version von Visual C++, die speziell auf die Verbesserung der Performance von C++/WinRT abzielen.

### <a name="topics-about-cwinrt"></a>Themen zu C++/WinRT

| Thema | Beschreibung |
| - | - |
| [Einführung in C++/WinRT](intro-to-using-cpp-with-winrt.md) | Enthält eine Einführung in C++/WinRT – eine Standard C++ Sprachprojektion für Windows-Runtime-APIs. |
| [Erste Schritte mit C++/WinRT](get-started.md) | Damit Sie C++/WinRT schneller verwenden können, werden in diesem Thema einige einfache Codebeispiele vorgestellt. |
| [Neuigkeiten in C++/WinRT](news.md) | Neuigkeiten und Änderungen von C++/WinRT. |
| [Häufig gestellte Fragen](faq.md) | Antworten auf Fragen zur Erstellung und Nutzung von Windows-Runtime-APIs mit C++/WinRT. |
| [Problembehandlung](troubleshooting.md) | Die Tabelle mit den Symptomen und Problembehandlungen in diesem Thema kann für Sie hilfreich sein. Hierbei spielt es keine Rolle, ob Sie neuen Code schreiben oder eine bestehende App portieren. |
| [C++/WinRT-Beispielanwendung eines Foto-Editors](photo-editor-sample.md) | Foto-Editor ist eine UWP-Beispielanwendung, die das Entwickeln von Apps mit der C++/WinRT-Programmiersprache veranschaulicht. Mit der Beispielanwendung können Sie Fotos aus der Bibliothek **Bilder** abrufen und dann das ausgewählte Bild mit verschiedenen Fotoeffekten bearbeiten. | 
| [String-Verarbeitung](strings.md) | Mit C++/WinRT können Sie Windows-Runtime-APIs mit C++-String-Standardtypen aufrufen, oder Sie können den Typ [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) verwenden. |
| [C++-Standarddatentypen und C++/WinRT](std-cpp-data-types.md) | Mit C++/WinRT können Sie Windows-Runtime-APIs über C++-Standarddatentypen aufrufen. |
| [Boxing und Unboxing von Einzelwerten für IInspectable](boxing.md) | Ein Einzelwert muss in ein Referenzklassenobjekt gepackt werden, bevor er an eine Funktion übergeben wird, die **IInspectable** erwartet. Dieser Wrapping-Prozess wird als *Boxing* des Werts bezeichnet. |
| [Verwenden von APIs mit C++/WinRT](consume-apis.md) | In diesem Thema wird gezeigt, wie vom Benutzer selbst, von Windows oder von Drittanbietern implementierte C++/WinRT-APIs verwendet werden. |
| [Erstellen von APIs mit C++/WinRT](author-apis.md) | Dieses Thema zeigt, wie C++/WinRT-APIs mithilfe der **winrt::implements**-Basisstruktur direkt oder indirekt erstellt werden. |
| [Fehlerbehandlung bei C++/WinRT](error-handling.md) | In diesem Thema werden Strategien zur Behandlung von Fehlern bei der Programmierung mit C++/WinRT erörtert. |
| [Verarbeiten von Ereignissen über Delegaten](handle-events.md) | Dieses Thema zeigt, wie Event-Handling-Delegaten mit C++/WinRT registriert und widerrufen werden. |
| [Erstellen von Ereignissen](author-events.md) | Dieses Thema zeigt, wie eine Komponente für Windows-Runtime erstellt wird, die eine Laufzeitklasse zum Auslösen von Ereignissen enthält. Es zeigt außerdem eine App, die die Komponente nutzt und die Ereignisse verarbeitet. |
| [Sammlungen mit C++/WinRT](collections.md) | C++/WinRT verfügt über Funktionen und Basisklassen, mit denen Sie beim Implementieren bzw. Übergeben von Sammlungen viel Zeit sparen können. |
| [Parallelität und asynchrone Vorgänge](concurrency.md) | Dieses Thema zeigt, wie Sie asynchrone Windows-Runtime-Objekte mit C++/WinRT erstellen und nutzen können. |
| [Erweiterte Parallelität und Asynchronie](concurrency-2.md) | Weitere erweiterte Szenarien mit Parallelität und Asynchronie in C++/WinRT. |
| [XAML-Steuerelemente; Binden an eine C++/WinRT-Eigenschaft](binding-property.md) | Eine Eigenschaft, die effektiv an ein XAML-Steuerelement gebunden werden kann, wird als *Observable*-Eigenschaft bezeichnet. Dieses Thema zeigt, wie man eine Observable-Eigenschaft implementiert und nutzt und ein XAML-Steuerelement daran bindet. |
| [XAML-Elementsteuerelemente; Binden an eine C++/WinRT-Sammlung](binding-collection.md) | Eine Sammlung, die effektiv an ein XAML-Elementsteuerelement gebunden werden kann, wird als *Observable*-Sammlung bezeichnet. Dieses Thema zeigt, wie man eine Observable-Sammlung implementiert und nutzt und wie man ein XAML-Elementsteuerelement daran bindet. |
| [Benutzerdefinierte (vorlagenbasierte) XAML-Steuerelemente mit C++/WinRT](xaml-cust-ctrl.md) | In diesem Thema werden die Schritte zum Erstellen eines einfachen benutzerdefinierten Steuerelements mit C++/WinRT beschrieben. Sie können diese Informationen nutzen, um Ihre eigenen Benutzeroberflächen-Steuerelemente mit vielen Funktionen und Anpassungsmöglichkeiten zu erstellen. |
| [Übergabe von Parametern in die ABI-Grenze](pass-parms-to-abi.md) | C++/WinRT vereinfacht die Übergabe von Parametern in die ABI-Grenze, indem automatische Konvertierungen für gängige Fälle bereitgestellt werden. |
| [Verwenden von COM-Komponenten mit C++/WinRT](consume-com.md) | In diesem Thema wird anhand eines vollständigen Direct2D-Codebeispiels verdeutlicht, wie Sie C++/WinRT für die Nutzung von COM-Klassen und -Schnittstellen verwenden. |
| [Erstellen von COM-Komponenten mit C++/WinRT](author-coclasses.md) | C++/WinRT dient Ihnen als Hilfe beim Erstellen von klassischen COM-Komponenten, wie dies auch für Windows-Runtime-Klassen der Fall ist. |
| [Umstellen von C++/CX auf C++/WinRT](move-to-winrt-from-cx.md) | In diesem Thema wird gezeigt, wie Sie C++/CX-Code zum entsprechenden Äquivalent in C++/WinRT portieren. |
| [Interoperabilität zwischen C++/WinRT und C++/CX](interop-winrt-cx.md) | In diesem Thema werden zwei Hilfsfunktionen gezeigt, die verwendet werden können, um zwischen [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live)- und C++/WinRT-Objekten zu konvertieren. |
| [Umstellen von WRL auf C++/WinRT](move-to-winrt-from-wrl.md) | In diesem Thema wird gezeigt, wie Sie Code der [C++-Vorlagenbibliothek für Windows-Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) zum entsprechenden Äquivalent in C++/WinRT portieren. |
| [Umstellen von C# auf C++/WinRT](move-to-winrt-from-csharp.md) | In diesem Thema wird gezeigt, wie Sie C#-Code zum äquivalenten Code in C++/WinRT portieren. |
| [Interoperabilität zwischen C++/WinRT und der ABI](interop-winrt-abi.md) | Dieses Thema zeigt, wie man zwischen Application Binary Interface (ABI) und C++/WinRT-Objekten konvertiert. |
| [Starke und schwache Verweise in C++/WinRT](weak-references.md) | Windows-Runtime ist ein System mit Verweiszählung. Es ist wichtig, dass Sie mit der Bedeutung und dem Unterschied zwischen starken und schwachen Verweisen vertraut sind. |
| [Agile Objekte](agile-objects.md) | Ein agiles Objekt ist ein Objekt, auf das von jedem Thread aus zugegriffen werden kann. Ihre C++/WinRT-Typen sind standardmäßig agil, aber Sie können diese Option auch deaktivieren. |
| [Diagnostizieren direkter Zuordnungen](diag-direct-alloc.md) | Dieses Thema erläutert eingehend ein C++/WinRT 2.0-Feature, das Ihnen beim Diagnostizieren eines Fehlers hilft, der entsteht, weil ein Objekt des Implementierungstyps im Stapel statt wie vorgesehen durch Verwendung der [**winrt::make**](/uwp/cpp-ref-for-winrt/make)-Hilfsprogrammfamilie erstellt wurde. |
| [Erweiterungspunkte für Ihre Implementierungstypen](details-about-destructors.md) | Mit diesen Erweiterungspunkten in C++/WinRT 2.0 können Sie die Zerstörung ihrer Implementierungstypen hinausschieben, um eine sichere Abfrage während der Zerstörung zu ermöglichen, einen Hook für den Eintrag zur Verfügung zu haben und Ihre projektierten Methoden zu beenden. |
| [Eine einfache C++/WinRT-Windows-UI-Beispielbibliothek](simple-winui-example.md) | Dieses Thema führt Sie durch den Prozess zum Hinzufügen von einfacher Unterstützung für WinUI innerhalb eines C++/WinRT-Projekts. |

### <a name="topics-about-the-c-language"></a>Themen zur C++-Sprache

| Thema | Beschreibung |
| - | - |
| [Wertekategorien und zugehörige Verweise](cpp-value-categories.md) | In diesem Thema werden die verschiedenen Kategorien von Werten beschrieben, die in C++ vorhanden sind. Sie haben sicherlich schon von „lvalues“ und „rvalues“ gehört, aber es gibt auch andere Arten. |

## <a name="important-apis"></a>Wichtige APIs
* [winrt-Namespace](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Zugehörige Themen
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++-Vorlagenbibliothek für Windows-Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)
