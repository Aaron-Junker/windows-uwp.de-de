---
description: Dieser Artikel bietet eine Übersicht über Visual Studio-Projekt- und -Elementvorlagen für Windows-Apps.
title: Visual Studio-Projekt- und -Elementvorlagen für Windows-Apps
ms.date: 11/17/2020
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, XAML Islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.openlocfilehash: 28da8ee088c5871e8267e1fcadbb039322f677c7
ms.sourcegitcommit: cc871be2508f52509b6a947fe879aeec360d0fd2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2021
ms.locfileid: "106270187"
---
# <a name="visual-studio-project-and-item-templates-for-windows-apps"></a>Visual Studio-Projekt- und -Elementvorlagen für Windows-Apps

Visual Studio 2019 bietet zahlreiche Projekt- und Elementvorlagen, die Sie beim Erstellen von Apps für Windows 10-Geräte mithilfe von C\# oder C++ unterstützen. In diesem Thema werden die Vorlagen beschrieben, und es wird Ihnen bei der Auswahl einer für Ihr Szenario passenden Vorlage geholfen.

* Projektvorlagen umfassen Projektdateien, Codedateien und andere Ressourcen, die konfiguriert sind, um eine App oder eine Komponente zu erstellen, die von einer App geladen und verwendet werden kann.
* Elementvorlagen sind Projektdateien, die häufig verwendeten Code und XAML-Code enthalten, der einem Projekt hinzugefügt werden kann, um die Entwicklungszeit zu verkürzen. Beispielsweise können Sie eine Elementvorlage verwenden, um Ihrer App ein neues Fenster, eine neue Seite oder ein neues Steuerelement hinzuzufügen.

## <a name="winui-templates"></a>WinUI-Vorlagen

Die [Windows-UI-Bibliothek (WinUI)](../winui/index.md) ist die Plattform für die moderne native Benutzeroberfläche (UI) für Windows-Apps auf allen Desktop- (.NET und natives Win32) und UWP-App-Plattformen. [WinUI 3](../winui/winui3/index.md) ist die neueste Hauptversion von WinUI und transformiert WinUI in ein vollständiges UX-Framework für Windows-Desktop-Apps um.

WinUI 3 ist als Teil von [Project Reunion](../project-reunion/index.md) verfügbar. Es enthält ein VSIX-Paket für Visual Studio 2019, das Projekt- und Elementvorlagen bereitstellt, die Ihnen den Einstieg in das Erstellen von Apps mit einer WinUI-basierten Schnittstelle erleichtern.

Informationen zum Installieren des VSIX-Pakets von Project Reunion sowie der WinUI-Projektvorlagen finden Sie unter [Einrichten der Entwicklungsumgebung](../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment). Weitere Informationen zu den verfügbaren WinUI-Projekt- und -Elementvorlagen finden Sie unter [Erstellen von WinUI 3-Projekten](../winui/winui3/winui-project-templates-in-visual-studio.md).

## <a name="uwp-templates"></a>UWP-Vorlagen

Visual Studio bietet eine Vielzahl verschiedener Projektvorlagen zum Erstellen von UWP-Apps mit C# oder C++. Um diese Projektvorlagen zu verwenden, müssen Sie die Workload **Universelle Windows-Plattformentwicklung** einschließen, wenn Sie Visual Studio installieren. Für die C++-Projektvorlagen müssen Sie außerdem die optionale Komponente **C++ (v142) UWP-Tools (Universelle Windows-Plattform)** für die Workload **Universelle Windows-Plattformentwicklung** einschließen.

### <a name="project-templates-for-c-and-uwp"></a>Projektvorlagen für C# und UWP

Um auf die UWP-Projektvorlagen in C#zuzugreifen, wenn Sie ein neues Projekt in Visual Studio erstellen, filtern Sie die Sprache auf **C#** , die Plattform auf **Windows** und den Projekttyp auf **UWP**.

![UWP-Projektvorlagen in C#](images/uwp-projects-csharp.png)

Sie können diese Projektvorlagen verwenden, um UWP-Apps in C# zu erstellen.

| Vorlage | Beschreibung |
|----------|-------------|
| Leere App (Universelles Windows) | Erstellt eine UWP-App. Das generierte Projekt enthält eine einfache Seite, die von der [Windows.UI.Xaml.Controls.Page](/uwp/api/windows.ui.xaml.controls.page)-Klasse abgeleitet ist, die Sie als Ausgangspunkt verwenden können, um Ihre Benutzeroberfläche zu entwickeln. |
| Komponententest-App (Universelles Windows) | Erstellt ein Komponententestprojekt in C# für eine UWP-App. Weitere Informationen finden Sie unter [Komponententests mit C#-Code](/visualstudio/test/unit-testing-visual-csharp-code-in-a-store-app). |

Sie können diese Projektvorlagen verwenden, um Teile einer UWP-App in C# zu erstellen.

| Vorlage | Beschreibung |
|----------|-------------|
| Klassenbibliothek (Universelles Windows) | Erstellt eine verwaltete Klassenbibliothek (DLL) in C#, die von anderen UWP-Apps verwendet werden kann, die in verwaltetem Code geschrieben sind. |
| Windows-Runtime-Komponente (Universelles Windows) | Erstellt eine [Windows-Runtime-Komponente](/windows/uwp/winrt-components/) in C#, die von jeder UWP-App verwendet werden kann, unabhängig von der Programmiersprache, in der die App geschrieben ist. |
| Optionales Codepaket (Universelles Windows) | Erstellt ein optionales Paket mit ausführbarem C#-Code, das von einer App geladen werden kann. Weitere Informationen finden Sie unter [Optionale Pakete mit ausführbarem Code](/windows/msix/package/optional-packages-with-executable-code).  |

### <a name="project-templates-for-c-and-uwp"></a>Projektvorlagen für C++ und UWP

Es gibt zwei verschiedene Technologien, die Sie zum Erstellen von UWP-Apps in C++ verwenden können:

* Die empfohlene Technologie ist [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/). Dies ist eine C++-Sprachprojektion, die vollständig in Headerdateien implementiert und darauf ausgelegt ist, Ihnen erstklassigen Zugriff auf die moderne WinRT-API bereitzustellen.
* Alternativ können Sie den älteren Satz von [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)-Erweiterungen verwenden. C++/CX wird weiterhin unterstützt, aber wir empfehlen, stattdessen C++/WinRT zu verwenden.

Um auf die UWP-Projektvorlagen in C++ zuzugreifen, wenn Sie ein neues Projekt in Visual Studio erstellen, filtern Sie die Sprache auf **C++** , die Plattform auf **Windows** und den Projekttyp auf **UWP**. 

> [!NOTE]
> Standardmäßig bietet die Workload **Universelle Windows-Plattformentwicklung** in Visual Studio nur Zugriff auf die C++/WinRT-Projektvorlagen. Für den Zugriff auf die C++/WinRT-Projektvorlagen müssen Sie das [C++/WinRT VSIX](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)-Paket installieren.

![UWP-Projektvorlagen in C++](images/uwp-projects-cpp.png)

Sie können diese Projektvorlagen verwenden, um UWP-Apps in C++ zu erstellen.

| Vorlage | Beschreibung |
|----------|-------------|
| Blank App (C++/WinRT) | Erstellt eine C++/WinRT UWP-App mit einer XAML-Benutzeroberfläche. Das generierte Projekt enthält eine einfache Seite, die von der [Windows.UI.Xaml.Controls.Page](/uwp/api/windows.ui.xaml.controls.page)-Klasse abgeleitet ist, die Sie als Ausgangspunkt verwenden können, um Ihre Benutzeroberfläche zu entwickeln. |
| Core App (C++/WinRT) | Erstellt eine C++/WinRT UWP-App, die [CoreApplication](/uwp/api/Windows.ApplicationModel.Core.CoreApplication) verwendet, um die Integration in eine Vielzahl von UI-Frameworks anstelle einer XAML-Benutzeroberfläche vorzunehmen. Eine exemplarische Vorgehensweise, die veranschaulicht, wie diese Projektvorlage zum Erstellen eines einfachen Spiels verwendet wird, das DirectX verwendet, finden Sie unter [Erstellen eines einfachen UWP-Spiels mit DirectX](/windows/uwp/gaming/tutorial--create-your-first-uwp-directx-game). |
| Leere App (Universelles Windows: C++/CX) | Erstellt eine C++/WinRT UWP-App mit einer XAML-Benutzeroberfläche. Das generierte Projekt enthält eine einfache Seite, die von der [Windows.UI.Xaml.Controls.Page](/uwp/api/windows.ui.xaml.controls.page)-Klasse in der WinUI-Bibliothek abgeleitet ist, die Sie als Ausgangspunkt verwenden können, um Ihre Benutzeroberfläche zu entwickeln. |
| DirectX 11- und XAML-App (Universelles Windows: C++/CX) | Erstellt eine UWP-App, die DirectX 11 sowie ein **SwapChainPanel**-Element verwendet, sodass Sie XAML-UI-Steuerelemente verwenden können. Weitere Informationen finden Sie unter [DirectX-Spielprojektvorlagen](/windows/uwp/gaming/user-interface). |
| DirectX 11-App (Universelles Windows: C++/CX) | Erstellt eine UWP-App, die DirectX 11 verwendet. Weitere Informationen finden Sie unter [DirectX-Spielprojektvorlagen](/windows/uwp/gaming/user-interface). |
| DirectX 12-App (Universelles Windows: C++/CX) | Erstellt eine UWP-App, die DirectX 12 verwendet. Weitere Informationen finden Sie unter [DirectX-Spielprojektvorlagen](/windows/uwp/gaming/user-interface). |
| Komponententest-App (Universelles Windows: C++/CX) | Erstellt ein Komponententestprojekt in C++/CX für eine UWP-App. Weitere Informationen finden Sie unter [Testen einer C++ UWP-DLL](/visualstudio/test/unit-testing-a-visual-cpp-dll-for-store-apps). |

Sie können diese Projektvorlagen verwenden, um Teile einer UWP-App in C++ zu erstellen.

| Vorlage | Beschreibung |
|----------|-------------|
| Komponente für Windows-Runtime (C++/WinRT) | Erstellt eine [Windows-Runtime-Komponente](/windows/uwp/winrt-components/) in C++/WinRT, die von jeder UWP-App verwendet werden kann, unabhängig von der Programmiersprache, in der die App geschrieben ist. |
| Windows-Runtime-Komponente (Universelles Windows) | Erstellt eine [Windows-Runtime-Komponente](/windows/uwp/winrt-components/) in C++/CX, die von jeder UWP-App verwendet werden kann, unabhängig von der Programmiersprache, in der die App geschrieben ist. |
| DLL (Universelles Windows) | Ein Projekt zum Erstellen einer DLL (Dynamic Link Library) in C++/CX, die in einer UWP-App verwendet werden kann. Weitere Informationen finden Sie unter [DLLs (C++/CX)](/cpp/cppcx/dlls-c-cx). |
| Statische Bibliothek (Universelles Windows) | Ein Projekt zum Erstellen einer statischen Bibliothek (LIB) in C++/CX, die in einer UWP-App verwendet werden kann. Weitere Informationen finden Sie unter [Statische Bibliotheken (C++/CX)](/cpp/cppcx/static-libraries-c-cx). |

## <a name="cwin32-templates"></a>C++/Win32-Vorlagen

Visual Studio bietet eine Vielzahl verschiedener Projektvorlagen zum Erstellen von Windows-Desktop-Apps mit nativem C++ sowie direktem Zugriff auf die Win32-API. Um diese Projektvorlagen zu verwenden, müssen Sie die Workload **Desktopentwicklung mit C++** einschließen, wenn Sie Visual Studio installieren. Diese Workload umfasst Projektvorlagen zum Erstellen von Desktop-Apps, Konsolen-Apps und Bibliotheken.

### <a name="project-templates-for-desktop-apps"></a>Projektvorlagen für Desktop-Apps

Um auf die C++-Projektvorlagen für klassische Desktop-Apps zuzugreifen, wenn Sie ein neues Projekt in Visual Studio erstellen, filtern Sie die Sprache auf **C++** , die Plattform auf **Windows** und den Projekttyp auf **Desktop**.

![Projektvorlagen für native C++-Apps](images/desktop-app-projects-cpp.png)

| Vorlage | Beschreibung |
|----------|----------|
| Windows-Desktopanwendung | Erstellt eine klassische Windows-Desktop-App mit C++. Weitere Informationen finden Sie unter [Exemplarische Vorgehensweise: Erstellen einer traditionellen Windows-Desktopanwendung](/cpp/windows/walkthrough-creating-windows-desktop-applications-cpp). |
| Windows-Desktop-Assistent | Stellt einen schrittweisen Assistenten bereit, mit dem Sie einen der folgenden Projekttypen erstellen können: eine klassische Windows-Desktop-App, eine Konsolen-App, eine DLL (Dynamic Link Library) oder eine statische Bibliothek. Weitere Informationen finden Sie unter [Windows-Desktop-Assistent](/cpp/windows/windows-desktop-wizard) und [Exemplarische Vorgehensweise: Erstellen einer traditionellen Windows-Desktopanwendung](/cpp/windows/walkthrough-creating-windows-desktop-applications-cpp).         |
| Paketerstellungsprojekt für Windows-Anwendungen | Erstellt ein Projekt, das Sie zum Erstellen einer Desktop-App in einem [MSIX-Paket](/windows/msix/overview) verwenden können. Dies bietet eine moderne Bereitstellungserfahrung, die Möglichkeit zur Integration in Windows 10-Features mittels Paketerweiterungen und vieles mehr. Weitere Informationen finden Sie unter [Paketerstellungsprojekt für Windows-Anwendungen](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net).  |

### <a name="project-templates-for-console-apps"></a>Projektvorlagen für Konsolen-Apps

Um auf die C++-Projektvorlagen für Konsolen-Apps zuzugreifen, filtern Sie die Sprache auf **C++** , die Plattform auf **Windows** und den Projekttyp auf **Konsole**.

![Projektvorlagen für native C++-Konsolen-Apps](images/desktop-console-projects-cpp.png)

| Vorlage | Beschreibung |
|----------|----------|
| Windows-Konsolenanwendung (C++/WinRT) | Erstellt eine [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/)-Konsolen-App ohne eine Benutzeroberfläche. Weitere Informationen finden Sie im [C++/WinRT-Schnellstart](/windows/uwp/cpp-and-winrt-apis/get-started#a-cwinrt-quick-start). Diese Projektvorlage erfordert das [C++/WinRT VSIX](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)-Paket.  |
| Konsolen-App | Erstellt eine Konsolen-App ohne Benutzeroberfläche. Weitere Informationen finden Sie unter [Exemplarische Vorgehensweise: Erstellen eines C++-Standardprogramms](/cpp/windows/walkthrough-creating-a-standard-cpp-program-cpp). |
| Leeres Projekt | Ein leeres Projekt zum Erstellen einer Anwendung, Bibliothek oder DLL. Sie müssen jeglichen erforderlichen Code und alle Ressourcen hinzufügen. |

### <a name="project-templates-for-libraries"></a>Projektvorlagen für Bibliotheken

Um auf die C++-Projektvorlagen für Bibliotheken zuzugreifen, filtern Sie die Sprache auf **C++** , die Plattform auf **Windows** und den Projekttyp auf **Bibliothek**.

![Projektvorlagen für native C++-Bibliotheken](images/desktop-library-projects-cpp.png)

| Vorlage | Beschreibung |
|----------|----------|
| DLL (Dynamic Link Library) | Ein Projekt zum Erstellen einer DLL (Dynamic Link Library). Weitere Informationen finden Sie unter [Exemplarische Vorgehensweise: Erstellen und Verwenden einer DLL (Dynamic Link Library)](/cpp/build/walkthrough-creating-and-using-a-dynamic-link-library-cpp). |
| Statische Bibliothek | Ein Projekt zum Erstellen einer statischen Bibliothek (LIB). Weitere Informationen finden Sie unter [Exemplarische Vorgehensweise: Erstellen und Verwenden einer statischen Bibliothek](/cpp/build/walkthrough-creating-and-using-a-static-library-cpp). |

### <a name="item-templates-for-native-c-and-win32"></a>Elementvorlagen für natives C++ und Win32

Die C++-Projektvorlagen enthalten viele Elementvorlagen, mit denen Sie Aufgaben wie das Hinzufügen neuer Dateien und Ressourcen zu Ihrem Projekt ausführen können. Eine umfassende Liste finden Sie unter [Verwenden von Visual C++ – Hinzufügen neuer Elementvorlagen](/cpp/build/reference/using-visual-cpp-add-new-item-templates).

## <a name="net-templates"></a>.NET-Vorlagen

Visual Studio bietet eine Vielzahl verschiedener Projektvorlagen zum Erstellen von Windows-Desktop-Apps, die .NET und C# verwenden. Um diese Projektvorlagen zu verwenden, müssen Sie die Workload **.NET-Desktopentwicklung** einschließen, wenn Sie Visual Studio installieren.

Um auf die .NET-Projektvorlagen in C# zuzugreifen, wenn Sie ein neues Projekt in Visual Studio erstellen, filtern Sie die Sprache auf **C#** , die Plattform auf **Windows** und den Projekttyp auf **Desktop**.

![.NET-Projektvorlagen in C#](images/dotnet-projects-csharp.png)

Sie können diese Projektvorlagen verwenden, um Apps mithilfe von C# und .NET zu erstellen.

| Vorlage | Beschreibung |
|----------|----------|
| WPF-App (.NET Core) | Erstellt eine [WPF](/dotnet/framework/wpf/)-App, die [.NET Core](/dotnet/core/) als Ziel hat. Eine exemplarische Vorgehensweise zu dieser Projektvorlage finden Sie unter [Erstellen einer WPF-Anwendung](/visualstudio/get-started/csharp/tutorial-wpf). |
| WPF-App (.NET Framework) | Erstellt eine [WPF](/dotnet/framework/wpf/)-App, die [.NET Framework](/dotnet/framework/) als Ziel hat. Eine exemplarische Vorgehensweise zu dieser Projektvorlage finden Sie unter [Tutorial: Erstellen Ihrer ersten WPF-Anwendung](/dotnet/framework/wpf/getting-started/walkthrough-my-first-wpf-desktop-application). |
| Windows Forms-App (.NET Core) | Erstellt eine [Windows Forms](/dotnet/framework/winforms/)-App, die [.NET Core](/dotnet/core/) als Ziel hat.  |
| Windows Forms-App (.NET Framework) | Erstellt eine [Windows Forms](/dotnet/framework/winforms/)-App, die [.NET Framework](/dotnet/framework/) als Ziel hat. Eine exemplarische Vorgehensweise zu dieser Projektvorlage finden Sie unter [Erstellen einer Windows Forms-App in Visual Studio mit C#](/visualstudio/ide/create-csharp-winform-visual-studio). |
| Paketerstellungsprojekt für Windows-Anwendungen | Erstellt ein Projekt, das Sie zum Erstellen einer WPF- oder Windows Forms-App in einem [MSIX-Paket](/windows/msix/overview) verwenden können. Dies bietet eine moderne Bereitstellungserfahrung, die Möglichkeit zur Integration in Windows 10-Features mittels Paketerweiterungen und vieles mehr. Weitere Informationen finden Sie unter [Paketerstellungsprojekt für Windows-Anwendungen](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net). |