---
description: Neuigkeiten und Änderungen an C++ / WinRT.
title: Neuigkeiten in C++ / WinRT
ms.date: 01/29/2019
ms.topic: article
keywords: Windows 10, Uwp, Standard, c++, Cpp, Winrt, Projektion, News, was die neue
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: cb624a93a010dfe9784cf8c26beed12c6cf2f77d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639955"
---
# <a name="whats-new-in-cwinrt"></a>Neuigkeiten in C++ / WinRT

Die folgende Tabelle enthält Neuigkeiten und Änderungen an [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) in die aktuelle allgemein verfügbare Version des Windows SDK, d.h. 10.0.17763.0 (Windows 10, Version 1809). Diese Änderungen können auch in späteren SDK Insider Preview-Versionen vorhanden sein.

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>Neuigkeiten und Änderungen in Windows SDK, Version 10.0.17763.0 (Windows 10, Version 1809)

| Neue oder geänderte Funktion | Weitere Informationen |
| - | - |
| **Wichtige Änderung**. Für das Kompilieren, C++ / WinRT nicht den Header aus dem Windows SDK abhängig. | Finden Sie unter [isoliert von Windows SDK-Headerdateien](#isolation-from-windows-sdk-header-files)weiter unten. |
| Das Visual Studio-Projektformat System hat sich geändert. | Finden Sie unter [wie neu ausrichten, Ihrem C + c++ / WinRT-Projekt auf eine neuere Version des Windows SDK](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)weiter unten. |
| Es sind neue Funktionen und Basisklassen, können Sie ein Auflistungsobjekt an eine Windows-Runtime-Funktion übergeben oder eine eigene Auflistung – Eigenschaften und Auflistungstypen implementieren. | Finden Sie unter [Sammlungen mit C++ / WinRT](collections.md). |
| Können Sie die [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) Markuperweiterung mit Ihr C++ / WinRT-Runtime-Klassen. | Weitere Informationen und Codebeispiele, finden Sie unter [Übersicht über die Datenbindung](/windows/uwp/data-binding/data-binding-quickstart). |
| Unterstützung für den Abbruch einer Coroutine können Sie einen abbruchsrückruf zu registrieren. | Weitere Informationen und Codebeispiele, finden Sie unter [Abbrechen, eine asynchrone Operation und Abbruch-Rückrufe](concurrency.md#canceling-an-asychronous-operation-and-cancellation-callbacks). |
| Wenn Sie einen Delegaten verweist auf eine Memberfunktion zu erstellen, können Sie einrichten, einen starken oder einen schwachen Verweis auf das aktuelle Objekt (anstelle von einem unformatierten *dies* Zeiger) an dem Punkt, in dem der Handler registriert ist. | Weitere Informationen und Codebeispiele, finden Sie unter den **bei Verwendung eine Memberfunktion als Delegat** Unterabschnitt im Abschnitt [problemlos den Zugriff auf die *dies* Zeiger durch einen Delegaten zur Verarbeitung von Ereignissen](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate). |
| Fehler wurden behoben, die durch verbesserte Visual Studio-Konformität mit dem C++-standard aufgedeckt wurden. Die LLVM und Clang-toolkette ist zudem besser genutzt, um das Überprüfen von C++ / WinRT Übereinstimmung mit Standards. | Nicht mehr auftritt und das Problem, die in beschriebenen [Warum nicht Mein neue Projekt kompiliert? Ich verwende Visual Studio 2017 (Version 15.8.0 oder höher), und von SDK Version 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

Andere Änderungen.

- **Wichtige Änderung**. [**WinRT::get_abi(WinRT::hstring const&)** ](/uwp/cpp-ref-for-winrt/get-abi) gibt nun `void*` anstelle von `HSTRING`. Sie können `static_cast<HSTRING>(get_abi(my_hstring));` um ein HSTRING zu erhalten.
- **Wichtige Änderung**. [**WinRT::put_abi(WinRT::hstring&)** ](/uwp/cpp-ref-for-winrt/put-abi) gibt nun `void**` anstelle von `HSTRING*`. Sie können `reinterpret_cast<HSTRING*>(put_abi(my_hstring));` um ein HSTRING * zu erhalten.
- **Wichtige Änderung**. HRESULT ist jetzt als projiziert **winrt::hresult**. Wenn Sie ein HRESULT (um die typüberprüfung oder zur Unterstützung von typmerkmalen) benötigen, können Sie `static_cast` eine **winrt::hresult**. Andernfalls **winrt::hresult** in HRESULT konvertiert werden, solange Sie einschließen `unknwn.h` bevor Sie C++ einfügen c++ / WinRT-Header.
- **Wichtige Änderung**. GUID ist jetzt als projiziert **winrt::guid**. Für APIs, die Sie implementieren, müssen Sie verwenden **winrt::guid** für GUID-Parameter. Andernfalls **winrt::hresult** auf GUID konvertiert werden, solange Sie einschließen `unknwn.h` bevor Sie C++ einfügen c++ / WinRT-Header.
- **Wichtige Änderung**. Die [ **winrt::handle_type Konstruktor** ](/uwp/cpp-ref-for-winrt/handle-type#handletypehandletype-constructor) hat dazu explizite (es ist jetzt leicht mit falschen Programmieraufwand) verstärkt wurden. Wenn Sie eine unformatierte Handlewert zuweisen möchten, rufen Sie die [ **handle_type::attach Funktion** ](/uwp/cpp-ref-for-winrt/handle-type#handletypeattach-function) stattdessen.
- **Wichtige Änderung**. Die Signaturen der **WINRT_CanUnloadNow** und **WINRT_GetActivationFactory** geändert haben. Sie darf nicht alle diese Funktionen deklarieren. Stattdessen enthalten `winrt/base.h` (der automatisch enthalten ist, wenn Sie C++ einschließen / WinRT-Windows-Namespace-Header-Dateien), die die Deklarationen der diese Funktionen umfassen.
- Für die [ **winrt::clock Struktur**](/uwp/cpp-ref-for-winrt/clock), **From_FILETIME/To_FILETIME** sind veraltet, zugunsten des **From_file_time/To_file_time**.
- APIs, die erwarten, dass **IBuffer** Parameter wurden vereinfacht. Obwohl die meisten APIs, Sammlungen oder Arrays bevorzugen, genügend APIs abhängig **IBuffer** , dass es einfacher, solche APIs von C++ verwendet werden musste. Dieses Update bietet direkten Zugriff auf die zugrunde liegenden Daten eine **IBuffer** Implementierung, über die gleichen Daten verwendete Benennungskonvention die C++-Standardbibliothek-Container. Dies verhindert auch die Kollision mit Metadatennamen, die konventionell mit einem Großbuchstaben beginnen.
- Verbesserte codegenerierung: verschiedene Verbesserungen für die Größe des Codes zu reduzieren inlining verbessern und Optimieren Sie die Factory im Cache.
- Entfernt die unnötige Rekursion. Wenn die Befehlszeile verweist, in einen Ordner, anstatt zu einem bestimmten `.winmd`, `cppwinrt.exe` Tool nicht mehr sucht rekursiv nach `.winmd` Dateien. Die `cppwinrt.exe` Tool jetzt auch verarbeitet Duplikate mehr auf intelligente Weise, wodurch Benutzerfehler stabiler und zu schlecht formatiert `.winmd` Dateien.
- Mit verstärkter Sicherheit intelligente Zeiger. Früher Verschieben – der Fehler beim Widerrufen der zugeordneten Ereignis Rücknahmeschlüssel kein neuer Wert zugewiesen. Dies hat ein Problem entdecken, intelligente zeigerklassen selbstzuweisung zuverlässig gehandhabt waren nicht; als Stamm der [ **winrt::com_ptr Struktur Vorlage**](/uwp/cpp-ref-for-winrt/com-ptr). **WinRT::com_ptr** behoben wurde, und das Ereignis der zugeordneten Rücknahmeschlüssel behoben, behandeln die move-Semantik richtig, damit sie bei der Zuweisung aufzuheben.

> [!IMPORTANT]
> Wichtige Änderungen vorgenommen wurden, um die [C++ / WinRT Visual Studio-Erweiterung (VSIX)](https://aka.ms/cppwinrt/vsix), sowohl in Version 1.0.181002.2, und später in Version 1.0.190128.4. Details zu dieser Änderungen und deren Auswirkung auf Ihren vorhandenen Projekten [Visual Studio-Unterstützung für C++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) und [frühere Versionen der Erweiterung VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

## <a name="isolation-from-windows-sdk-header-files"></a>Isolation von Windows SDK-Headerdateien

Dies ist möglicherweise eine wichtige Änderung für Ihren Code.

Für das Kompilieren, C++ / WinRT hängt nicht mehr Headerdateien aus dem Windows SDK. Headerdateien der C-Laufzeitbibliothek (CRT) und die C++ Standardvorlagenbibliothek (STL), nicht auch alle Windows-SDK-Header enthalten. Und verbessert die Einhaltung von Standards, verhindert die unbeabsichtigte Abhängigkeiten, erheblich reduziert die Anzahl von Makros, die Sie zum Schutz vor.

Diese Unabhängigkeit bedeutet dass c++ / WinRT ist jetzt besser portierbar und Standards entsprechen, und sie die Möglichkeit, dass die Datei eine-Cross-Compiler und Cross-Platform-Bibliothek gefördert. Es bedeutet auch, dass der C / c++ / WinRT-Header nicht negativ auf die betroffenen Makros sind.

Wenn zuvor für C++ bleibt / WinRT, um alle Windows-Header in Ihrem Projekt einzuschließen, müssen Sie jetzt selbst enthalten. Es ist, in jedem Fall immer empfiehlt es sich um explizit die Header, von denen Sie abhängen, und lassen sie nicht in eine andere Bibliothek, um diese für Sie implizit enthalten.

Derzeit sind die einzigen Ausnahmen von Windows SDK-Header-Datei Isolation für systeminterne Funktionen und numerische Werte. Es gibt keine bekannten Probleme mit diesen letzten verbleibenden Abhängigkeiten.

In Ihrem Projekt können Sie Interoperabilität mit dem Windows SDK-Header erneut aktivieren, wenn Sie möchten. Sie können z. B. eine COM-Schnittstelle implementieren möchten (als Stamm [ **IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)). Für dieses Beispiel umfassen `unknwn.h` bevor Sie C++ einfügen c++ / WinRT-Header. Dies bewirkt, dass dies der Fall ist C++ / WinRT-Basisbibliothek zahlreichen Hooks zur Unterstützung von klassischen COM-Schnittstellen zu aktivieren. Ein Codebeispiel finden Sie unter [Autor COM-Komponenten mit C++ / WinRT](author-coclasses.md). Auf ähnliche Weise enthalten Sie explizit alle anderen Windows SDK-Header, die deklarieren Typen und/oder Funktionen, die Sie aufrufen möchten.

## <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>Wie Sie Ihre C + neu zuweisen c++ / WinRT-Projekt auf eine neuere Version des Windows SDK

Die Methode für die neuzuweisung des Projekts, die wahrscheinlich zu den wenigsten Compiler- und Linkeroptionen Problem ist, ist auch der aufwändigste. Diese Methode umfasst das Erstellen eines neuen Projekts (für die Windows SDK-Version Ihrer Wahl), und klicken Sie dann Kopieren von Dateien über dem neuen Projekt auf Ihrem alten. Fallen in Abschnitten von Ihrem alten `.vcxproj` und `.vcxproj.filters` Dateien, die Sie soeben können über zu kopieren, speichern Sie das Hinzufügen von Dateien in Visual Studio.

Es gibt jedoch zwei weitere Methoden, die Ihr Projekt in Visual Studio neu zuweisen.

- Wechseln Sie zu den Projekteigenschaften **allgemeine** \> **Windows SDK-Version**, und wählen Sie **alle Konfigurationen** und **alle Plattformen**. Legen Sie **Windows SDK-Version** auf die Version, die Sie abzielen möchten.
- In **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektknoten, klicken Sie auf **Projekte neu zuweisen**, wählen Sie die Versionen, die Sie verwenden möchten, als Ziel aus, und klicken Sie dann auf **OK**.

Wenn Sie nach der Verwendung einer dieser beiden Methoden Compiler oder Linker-Fehler auftreten, können Sie versuchen, die Projektmappe bereinigen (**erstellen** > **Projektmappe bereinigen** bzw. löschen Sie manuell alle temporären Ordner und Dateien) vor dem Versuch, erneut zu erstellen.

Wenn der C++-Compiler erzeugt "*Fehler C2039: "IUnknown": ist kein Mitglied der "\`globalen Namespace''*", fügen Sie dann `#include <unknwn.h>` am Anfang Ihrer `pch.h` Datei (bevor Sie C++ einfügen c++ / WinRT-Header).

Sie müssen möglicherweise auch hinzufügen `#include <hstring.h>` danach.

Wenn der C++-Linker führt "*Fehler LNK2019: nicht aufgelöstes externes Symbol _WINRT_CanUnloadNow@0 verwiesen wird, in der Funktion _VSDesignerCanUnloadNow@0* ", und klicken Sie dann, die durch das Hinzufügen zu beheben `#define _VSDESIGNER_DONT_LOAD_AS_DLL` auf Ihre `pch.h` Datei.
