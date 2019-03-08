---
description: Foto-Editor ist eine UWP-Beispielanwendung, die das Entwickeln von Apps mit der C++/WinRT-Programmiersprache veranschaulicht. Mit der Beispielanwendung können Sie Fotos aus der Bilder-Bibliothek abrufen und dann das ausgewählte Bild mit verschiedenen Fotoeffekten bearbeiten.
title: C++/WinRT-Beispielanwendung eines Foto-Editors
ms.date: 06/08/2018
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projektion, Beispiel, Anwendung, Foto, Editor
ms.localizationpriority: medium
ms.openlocfilehash: 8c6f668ef3d92f968e75659b0ba1937abadb079c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606705"
---
# <a name="photo-editor-cwinrt-sample-application"></a>C++/WinRT-Beispielanwendung eines Foto-Editors
Sie können die Beispielanwendung aus dem GitHub-Repository [C++/WinRT-Beispielanwendung eines Foto-Editors](https://github.com/Microsoft/Windows-appsample-photo-editor) klonen oder herunterladen.

Der Foto-Editor ist eine UWP-Beispielanwendung (Universelle Windows-Plattform), die das Entwickeln von Apps mit der [C++/WinRT](intro-to-using-cpp-with-winrt.md)-Sprachprojektion veranschaulicht. Mit der Beispielanwendung können Sie Fotos aus der **Bilder**-Bibliothek abrufen und dann das ausgewählte Bild mit verschiedenen Fotoeffekten bearbeiten. Im Quellcode des Beispiels sehen Sie eine Reihe allgemeiner Methoden&mdash;wie [Datenbindung](binding-property.md) und [asynchrone Aktionen und Vorgänge](concurrency.md)&mdash;, die mithilfe der C++/ WinRT-Projektion durchgeführt werden. Hier sind einige der spezifischen Features, die im Beispiel gezeigt werden.
    
- Verwendung der standardmäßigen C++17-Syntax und -Bibliotheken mit Windows-Runtime (WinRT)-APIs
- Verwendung von Coroutinen, einschließlich der Verwendung von co_await, co_return, [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) und [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_)
- Erstellung und Verwendung von benutzerdefinierten projizierten Windows-Runtime-Klassentypen (Runtime-Klasse) und Implementierungstypen. Weitere Informationen zu diesen Begriffen finden Sie unter [Nutzen von APIs mit C++/WinRT](consume-apis.md) und [Erstellen von APIs mit C++/WinRT](author-apis.md).
- [Ereignisbehandlung](handle-events.md), einschließlich der Verwendung von automatisch widerrufenden Ereignis-Token
- Verwendung des externen Win2D-NuGet-Pakets und von [Windows::UI::Composition](/uwp/api/windows.ui.composition) für Bildeffekte
- XAML-Datenbindung, einschließlich der [{x:Bind}-Markuperweiterung](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)
- XAML-Formatierung und Anpassung der Benutzeroberfläche, einschließlich [verbundener Animationen](../design/motion/connected-animation.md)
