---
description: Foto-Editor ist eine UWP-Beispielanwendung, die das Entwickeln von Apps mit der C++/WinRT-Programmiersprache veranschaulicht. Mit der Beispielanwendung kannst du Fotos aus der Bibliothek „Bilder“ abrufen und dann das ausgewählte Bild mit verschiedenen Fotoeffekten bearbeiten.
title: C++/WinRT-Beispielanwendung eines Foto-Editors
ms.date: 04/23/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projektion, Beispiel, Anwendung, Beispielanwendung, Foto, Editor, Foto-Editor
ms.localizationpriority: medium
ms.openlocfilehash: 2938bc8c7d054241e57db9ed26c7600dc189f6c5
ms.sourcegitcommit: 539b428bcf3d72c6bda211893df51f2a27ac5206
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "102629358"
---
# <a name="photo-editor-cwinrt-sample-application"></a>C++/WinRT-Beispielanwendung eines Foto-Editors

> [!NOTE]
> Das Beispiel wurde für Windows 10, Version 1903 (10.0; Build 18362), sowie für Visual Studio 2019 konzipiert und getestet. Falls gewünscht, kannst du die Projekteigenschaften anpassen, um das Projekt für Windows 10, Version 1809 (10.0; Build 17763) zu verwenden, und/oder das Beispiel in Visual Studio 2017 öffnen.

Informationen zum Klonen oder Herunterladen der Beispielanwendung findest du unter [C++/WinRT-Beispielanwendung eines Foto-Editors](/samples/microsoft/windows-appsample-photo-editor/photo-editor-cwinrt-sample-application/) im Katalog mit Codebeispielen.

Der Foto-Editor ist eine UWP-Beispielanwendung (Universelle Windows-Plattform), die die Entwicklung mit der [C++/WinRT](intro-to-using-cpp-with-winrt.md)-Sprachprojektion veranschaulicht. Mit der Beispielanwendung können Sie Fotos aus der Bibliothek **Bilder** abrufen und dann das ausgewählte Bild mit verschiedenen Fotoeffekten bearbeiten. Der Quellcode enthält eine Reihe allgemeiner Methoden wie [Datenbindung](binding-property.md) und [asynchrone Aktionen und Vorgänge](concurrency.md), die unter Verwendung der C++/WinRT-Projektion durchgeführt werden. Hier findest du einige der spezifischen Features aus dem Beispiel.

- Verwendung der standardmäßigen C++17-Syntax und -Bibliotheken mit WinRT-APIs (Windows-Runtime)
- Verwendung von Coroutinen (einschließlich „co_await“, „co_return“, [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) und [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation-1))
- Erstellung und Verwendung benutzerdefinierter projizierter Windows-Runtime-Klassentypen (Laufzeitklassen) und Implementierungstypen. Weitere Informationen zu diesen Begriffen findest du unter [Nutzen von APIs mit C++/WinRT](consume-apis.md) sowie unter [Erstellen von APIs mit C++/WinRT](author-apis.md).
- [Ereignisbehandlung](handle-events.md) – einschließlich der Verwendung von Ereignistoken mit automatischem Widerruf
- Verwendung des externen Win2D-NuGet-Pakets sowie von [Windows::UI::Composition](/uwp/api/windows.ui.composition) für Bildeffekte
- XAML-Datenbindung – einschließlich der [{x:Bind}-Markuperweiterung](../xaml-platform/x-bind-markup-extension.md)
- XAML-Formatierung und Anpassung der Benutzeroberfläche – einschließlich [verbundener Animationen](../design/motion/connected-animation.md)

Siehe auch [Wo finde ich C++/WinRT-Beispiel-Apps?](./faq.yml#where-can-i-find-c---winrt-sample-apps-).
