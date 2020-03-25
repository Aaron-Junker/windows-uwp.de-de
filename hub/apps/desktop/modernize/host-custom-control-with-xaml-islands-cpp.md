---
description: In diesem Artikel wird veranschaulicht, wie ein benutzerdefiniertes UWP C++ -Steuerelement in einer Win32-App mithilfe der XAML-Hosting-API gehostet wird.
title: Hosten eines benutzerdefinierten UWP- C++ Steuer Elements in einer Win32-App mithilfe der XAML-Hosting-API
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10, UWP, C++, Win32, XAML-Inseln, benutzerdefinierte Steuerelemente, Benutzer Steuerelemente, Host Steuerelemente
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 2f34c9c56cf9db5dfcfd702b97f2d34273b86e6a
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226314"
---
# <a name="host-a-custom-uwp-control-in-a-c-win32-app"></a>Hosten eines benutzerdefinierten UWP- C++ Steuer Elements in einer Win32-App

In diesem Artikel wird die Verwendung der [UWP-XAML-Hosting-API](using-the-xaml-hosting-api.md) zum Hosten eines benutzerdefinierten UWP-XAML-Steuer Elements in einer neuen C++ Win32-App veranschaulicht. Wenn Sie über ein vorhandenes C++ Win32-App-Projekt verfügen, können Sie diese Schritte und Codebeispiele für Ihr Projekt anpassen.

Um ein benutzerdefiniertes UWP-XAML-Steuerelement zu hosten, erstellen Sie die folgenden Projekte und Komponenten im Rahmen dieser exemplarischen Vorgehensweise:

* **Windows-Desktop Anwendungsprojekt**. Dieses Projekt implementiert eine native C++ Win32-Desktop-App. Fügen Sie diesem Projekt Code hinzu, der die UWP-XAML-Hosting-API zum Hosten eines benutzerdefinierten UWP-XAML-Steuer Elements verwendet.

* **UWP-APP-C++Projekt (/WinRT)** . Dieses Projekt implementiert ein benutzerdefiniertes UWP-XAML-Steuerelement. Außerdem implementiert es einen stammmetadatenanbieter zum Laden von Metadaten für benutzerdefinierte UWP-XAML-Typen im Projekt.

## <a name="requirements"></a>Voraussetzungen

* Visual Studio 2019 Version 16.4.3 oder höher.
* Windows 10, Version 1903 SDK (Version 10.0.18362) oder höher.
* [/WinRT Visual Studio-Erweiterung (VSIX) mit Visual Studio installiert. C++](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) C++/WinRT ist eine vollständig standardisierte, moderne C++17 Sprachprojektion für Windows-Runtime-(WinRT)-APIs, die als headerdateibasierte Bibliothek implementiert ist und Ihnen einen erstklassigen Zugriff auf die moderne Windows-API bietet. Weitere Informationen finden [ C++](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/)Sie unter/WinRT.

## <a name="create-a-desktop-application-project"></a>Erstellen eines Desktop Anwendungs Projekts

1. Erstellen Sie in Visual Studio ein neues **Windows-Desktop Anwendungs** Projekt mit dem Namen **MyDesktopWin32App**. Diese Projektvorlage ist unter den **C++** Projekt filtern, **Windows**und **Desktop** verfügbar.

2. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektmappenknoten, klicken Sie auf Projekt Mappe **neu**zuweisen, wählen Sie die Version **10.0.18362.0** oder höher aus, und klicken Sie dann auf **OK**.

3. Installieren Sie das nuget-Paket [Microsoft. Windows. cppwinrt](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) , um die Unterstützung für [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis) im Projekt zu aktivieren:

    1. Klicken Sie in **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **MyDesktopWin32App** , und wählen Sie **nuget-Pakete verwalten**.
    2. Wählen Sie die Registerkarte **Durchsuchen** aus, suchen Sie nach dem Paket [Microsoft. Windows. cppwinrt](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) , und installieren Sie die neueste Version dieses Pakets.

4. Installieren Sie im Fenster **nuget-Pakete verwalten** die folgenden zusätzlichen nuget-Pakete:

    * [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) (Version v 6.0.0 oder höher). Dieses Paket enthält mehrere Build-und Lauf Zeit Objekte, die es ermöglichen, dass XAML-Inseln in der App funktionieren.
    * [Microsoft. Toolkit. Win32. UI. xamlapplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) (Version v 6.0.0 oder höher).
    * [Microsoft. vcrtforwarders. 140](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140).

5. Erstellen Sie die Lösung, und vergewissern Sie sich, dass Sie erfolgreich erstellt wurde

## <a name="create-a-uwp-app-project"></a>Erstellen eines UWP-App-Projekts

Fügen Sie als nächstes der Projekt Mappe ein UWP-App-Projekt **C++(/WinRT)** hinzu, und nehmen Sie einige Konfigurationsänderungen an diesem Projekt vor. Später in dieser exemplarischen Vorgehensweise fügen Sie diesem Projekt Code hinzu, um ein benutzerdefiniertes UWP-XAML-Steuerelement zu implementieren und eine Instanz der [Microsoft. Toolkit. Win32. UI. xamlhost. xamlapplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) -Klasse zu definieren, die vom Windows Community Toolkit bereitgestellt wird. Ihre APP verwendet diese Klasse als Stamm-Metadatenanbieter zum Laden von Metadaten für benutzerdefinierte UWP-XAML-Typen.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Knoten Projekt Mappe, und wählen Sie -> **Neues Projekt** **Hinzufügen** .

2. Fügen Sie der Projekt Mappe ein **leeres App-ProjektC++(/WinRT)** hinzu. Benennen Sie das Projekt **myuwpapp** , und stellen Sie sicher, dass die Zielversion und die Mindestversion auf **Windows 10, Version 1903** oder höher, festgelegt sind.

3. Installieren Sie das nuget-Paket [Microsoft. Toolkit. Win32. UI. xamlapplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) im **myuwpapp** -Projekt:

    1. Klicken Sie mit der rechten Maustaste auf das Projekt **myuwpapp** , und wählen Sie **nuget-Pakete verwalten**.
    2. Wählen Sie die Registerkarte **Durchsuchen** aus, suchen Sie nach dem Paket [Microsoft. Toolkit. Win32. UI. xamlapplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) , und installieren Sie v 6.0.0 oder eine spätere Version dieses Pakets.

4. Klicken Sie mit der rechten Maustaste auf den Knoten **myuwpapp** , und wählen Sie **Eigenschaften**. Legen Sie auf der Seite **Allgemeine Eigenschaften** ->  **C++/WinRT** die folgenden Eigenschaften fest, und klicken Sie **dann auf über**nehmen.

    * Legen Sie **Ausführlichkeit** auf **Normal**fest.
    * Legen **Sie** die Einstellung auf **Nein**fest.

    Wenn Sie den Vorgang abgeschlossen haben, sollte die Seite Eigenschaften wie folgt aussehen.

    ![C++/WinRT-Projekteigenschaften](images/xaml-islands/xaml-island-cpp-1.png)

5. Legen Sie im Eigenschaften Fenster auf der Seite **Konfigurations Eigenschaften** -> **Allgemein** den **Konfigurationstyp** auf **dynamische Bibliothek (dll**-Datei) fest, und klicken Sie dann auf **OK** , um das Eigenschaften Fenster zu schließen.

    ![Allgemeine Projekteigenschaften](images/xaml-islands/xaml-island-cpp-2.png)

6. Fügen Sie dem Projekt **myuwpapp** eine ausführbare Platzhalter Datei hinzu. Diese ausführbare Datei für den Platzhalter ist erforderlich, damit Visual Studio die erforderlichen Projektdateien generiert und das Projekt ordnungsgemäß erstellt.

    1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Projekt Knoten **myuwpapp** , und wählen Sie -> **Neues Element** **Hinzufügen** aus.
    2. Klicken Sie im Dialogfeld **Neues Element hinzufügen** auf der linken Seite auf **Hilfsprogramm** , und wählen Sie dann **Textdatei (. txt)** aus. Geben Sie den Namen **Platzhalter. exe** ein, und klicken Sie auf **Hinzufügen**
      ![Hinzufügen einer Textdatei](images/xaml-islands/xaml-island-cpp-3.png)
    3. Wählen Sie in **Projektmappen-Explorer**die Datei **Platzhalter. exe** aus. Stellen Sie im **Eigenschaften** Fenster sicher, dass die **Content** -Eigenschaft auf **true**festgelegt ist.
    4. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die Datei **Package. appxmanifest** im Projekt **myuwpapp** , wählen Sie **Öffnen mit**aus, und wählen Sie dann **XML (Text)-Editor**aus, und klicken Sie auf **OK**.
    5. Suchen Sie das **&lt;Application&gt;** -Element, und ändern Sie das Attribut der **ausführbaren Datei** in den Wert `placeholder.exe`. Wenn Sie den Vorgang abgeschlossen haben, sollte das **&lt;Anwendungs&gt;** -Element in etwa wie folgt aussehen.

        ```xml
        <Application Id="App" Executable="placeholder.exe" EntryPoint="MyUWPApp.App">
          <uap:VisualElements DisplayName="MyUWPApp" Description="Project for a single page C++/WinRT Universal Windows Platform (UWP) app with no predefined layout"
            Square150x150Logo="Assets\Square150x150Logo.png" Square44x44Logo="Assets\Square44x44Logo.png" BackgroundColor="transparent">
            <uap:DefaultTile Wide310x150Logo="Assets\Wide310x150Logo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
        ```

    6. Speichern und schließen Sie die Datei " **Package. appxmanifest** ".

7. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Knoten **myuwpapp** , und wählen Sie **Projekt entladen**aus.
8. Klicken Sie mit der rechten Maustaste auf den Knoten **myuwpapp** , und wählen Sie **myuwpapp. vcxproj bearbeiten**aus.
9. Suchen Sie das `<Import Project="$(VCTargetsPath)\Microsoft.Cpp.Default.props" />`-Element, und ersetzen Sie es durch den folgenden XML-Code. Mit diesem XML werden direkt vor dem-Element mehrere neue Eigenschaften hinzugefügt.

    ```xml
    <PropertyGroup Label="Globals">
        <WindowsAppContainer>true</WindowsAppContainer>
        <AppxGeneratePriEnabled>true</AppxGeneratePriEnabled>
        <ProjectPriIndexName>App</ProjectPriIndexName>
        <AppxPackage>true</AppxPackage>
    </PropertyGroup>
    <Import Project="$(VCTargetsPath)\Microsoft.Cpp.Default.props" />
    ```

10. Speichern und schließen Sie die Projektdatei.
11. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Knoten **myuwpapp** , und wählen Sie **Projekt erneut laden**aus.

## <a name="configure-the-solution"></a>Konfigurieren der Lösung

In diesem Abschnitt aktualisieren Sie die Projekt Mappe, die beide Projekte enthält, um Projekt Abhängigkeiten und Buildeigenschaften zu konfigurieren, die für die ordnungsgemäße Erstellung der Projekte erforderlich sind.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektmappenknoten, und fügen Sie eine neue XML-Datei mit dem Namen **Solution.**
2. Fügen Sie den folgenden XML-Code in die Datei " **Solution.** die"

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
      <PropertyGroup>
        <IntDir>$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(MSBuildProjectName)\</IntDir>
        <OutDir>$(SolutionDir)\bin\$(Platform)\$(Configuration)\$(MSBuildProjectName)\</OutDir>
        <GeneratedFilesDir>$(IntDir)Generated Files\</GeneratedFilesDir>
      </PropertyGroup>
    </Project>
    ```

3. Klicken Sie im Menü **Ansicht** auf **Eigenschaften-Manager** (abhängig von der Konfiguration kann dies in der **Ansicht** -> **anderen Fenstern**angezeigt werden).
4. Klicken Sie im **Eigenschaften-Manager** Fenster mit der rechten Maustaste auf **MyDesktopWin32App** , und wählen Sie **vorhandenes Eigenschaften Blatt hinzufügen**aus. Navigieren Sie zu **der soeben** hinzugefügten Projektmappendatei, und klicken Sie auf **Öffnen**.
5. Wiederholen Sie den vorherigen Schritt, um die Datei " **Solution.** -Eigenschaften" dem Projekt **myuwpapp** im **Eigenschaften-Manager** Fenster hinzuzufügen.
6. Schließen Sie das Fenster **Eigenschaften-Manager** .
7. Vergewissern Sie sich, dass die Eigenschaften Blatt Änderungen ordnungsgemäß gespeichert wurden. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt **MyDesktopWin32App** , und wählen Sie **Eigenschaften**aus. Klicken Sie auf **Konfigurations Eigenschaften** -> **genneral**, und vergewissern Sie sich, dass das **Ausgabeverzeichnis** und die **zwischen Verzeichnis** Eigenschaften die Werte aufweisen, die Sie der Projektmappendatei "Projekt Mappe **.** Sie können auch das gleiche für das Projekt **myuwpapp** bestätigen.
    ![Projekteigenschaften](images/xaml-islands/xaml-island-cpp-4.png)

8. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Knoten Projekt Mappe, und wählen Sie **Projekt Abhängigkeiten**. Stellen Sie sicher, dass in der Dropdown Liste **Projekte** die Option **MyDesktopWin32App** ausgewählt ist, und wählen Sie in der Liste **abhängig von** die Option **myuwpapp** aus.
    ![Projekt Abhängigkeiten](images/xaml-islands/xaml-island-cpp-5.png)

9. Klicken Sie auf **OK**.

## <a name="add-code-to-the-uwp-app-project"></a>Hinzufügen von Code zum UWP-App-Projekt

Nun können Sie dem Projekt **myuwpapp** Code hinzufügen, um die folgenden Aufgaben auszuführen:

* Implementieren eines benutzerdefinierten UWP-XAML-Steuer Elements. Später in dieser exemplarischen Vorgehensweise fügen Sie Code hinzu, der dieses Steuerelement im **MyDesktopWin32App** -Projekt hostet.
* Definieren Sie einen Typ, der von der [Microsoft. Toolkit. Win32. UI. xamlhost. xamlapplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) -Klasse im Windows Community Toolkit abgeleitet ist. Diese Klasse fungiert als Stamm-Metadatenanbieter zum Laden von Metadaten für benutzerdefinierte UWP-XAML-Typen.

### <a name="define-a-custom-uwp-xaml-control"></a>Definieren eines benutzerdefinierten UWP-XAML-Steuer Elements

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf **myuwpapp** , und wählen Sie -> **Neues Element** **Hinzufügen** aus. Wählen Sie im linken Bereich den Knoten **visuell C++**  aus, wählen Sie **leeres BenutzerC++Steuerelement (/WinRT)** , benennen Sie es **MyUserControl**, und klicken Sie auf **Hinzufügen**.
2. Ersetzen Sie im XAML-Editor den Inhalt der Datei **MyUserControl. XAML** durch den folgenden XAML-Code, und speichern Sie die Datei.

    ```xml
    <UserControl
        x:Class="MyUWPApp.MyUserControl"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyUWPApp"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <StackPanel HorizontalAlignment="Center" Spacing="10" 
                    Padding="20" VerticalAlignment="Center">
            <TextBlock HorizontalAlignment="Center" TextWrapping="Wrap" 
                           Text="Hello from XAML Islands" FontSize="30" />
            <TextBlock HorizontalAlignment="Center" Margin="15" TextWrapping="Wrap"
                           Text="😍❤💋🌹🎉😎�🐱‍👤" FontSize="16" />
            <Button HorizontalAlignment="Center" 
                    x:Name="Button" Click="ClickHandler">Click Me</Button>
        </StackPanel>
    </UserControl>
    ```

### <a name="define-a-xamlapplication-class"></a>Definieren einer xamlapplication-Klasse

Überarbeiten Sie als nächstes die Standard- **App** -Klasse im Projekt **myuwpapp** so, dass Sie von der [Microsoft. Toolkit. Win32. UI. xamlhost. xamlapplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) -Klasse abgeleitet wird, die vom Windows Community Toolkit bereitgestellt wird. Später in dieser exemplarischen Vorgehensweise aktualisieren Sie das Desktop Projekt, um eine Instanz dieser Klasse als Stamm-Metadatenanbieter zum Laden von Metadaten für benutzerdefinierte UWP-XAML-Typen zu erstellen.

  > [!NOTE]
  > Jede Projekt Mappe, die XAML-Inseln verwendet, kann nur ein Projekt enthalten, das ein `XamlApplication` Objekt definiert. Alle benutzerdefinierten UWP-XAML-Steuerelemente in Ihrer APP haben dasselbe `XamlApplication` Objekt. 

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die Datei " **MainPage. XAML** " im Projekt **myuwpapp** . Klicken Sie auf **Entfernen** und dann auf **Löschen** , um diese Datei aus dem Projekt zu löschen.
2. Erweitern Sie im Projekt **myuwpapp** die Datei **app. XAML** .
3. Ersetzen Sie den Inhalt der Dateien **app. XAML**, **app. cpp**, **app. h**und **app. idl** durch den folgenden Code.

    * **App. XAML**:

        ```xml
        <Toolkit:XamlApplication
            x:Class="MyUWPApp.App"
            xmlns:Toolkit="using:Microsoft.Toolkit.Win32.UI.XamlHost"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="using:MyUWPApp">
        </Toolkit:XamlApplication>
        ```

    * **App. idl**:

        ```IDL
        namespace MyUWPApp
        {
             [default_interface]
             runtimeclass App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
             {
                App();
             }
        }
        ```

    * **App. h**:

        ```cpp
        #pragma once
        #include "App.g.h"
        #include "App.base.h"
        namespace winrt::MyUWPApp::implementation
        {
            class App : public AppT2<App>
            {
            public:
                App();
                ~App();
            };
        }
        namespace winrt::MyUWPApp::factory_implementation
        {
            class App : public AppT<App, implementation::App>
            {
            };
        }
        ```

    * **App. cpp**:

        ```cpp
        #include "pch.h"
        #include "App.h"
        using namespace winrt;
        using namespace Windows::UI::Xaml;
        namespace winrt::MyUWPApp::implementation
        {
            App::App()
            {
                Initialize();
                AddRef();
                m_inner.as<::IUnknown>()->Release();
            }
            App::~App()
            {
                Close();
            }
        }
        ```

4. Fügen Sie dem Projekt **myuwpapp** eine neue Header Datei mit dem Namen **app. base. h**hinzu.
5. Fügen Sie den folgenden Code in die Datei " **app. base. h** " ein, speichern Sie die Datei, und schließen Sie Sie.

    ```cpp
    #pragma once
    namespace winrt::MyUWPApp::implementation
    {
        template <typename D, typename... I>
        struct App_baseWithProvider : public App_base<D, ::winrt::Windows::UI::Xaml::Markup::IXamlMetadataProvider>
        {
            using IXamlType = ::winrt::Windows::UI::Xaml::Markup::IXamlType;
            IXamlType GetXamlType(::winrt::Windows::UI::Xaml::Interop::TypeName const& type)
            {
                return AppProvider()->GetXamlType(type);
            }
            IXamlType GetXamlType(::winrt::hstring const& fullName)
            {
                return AppProvider()->GetXamlType(fullName);
            }
            ::winrt::com_array<::winrt::Windows::UI::Xaml::Markup::XmlnsDefinition> GetXmlnsDefinitions()
            {
                return AppProvider()->GetXmlnsDefinitions();
            }
        private:
            bool _contentLoaded{ false };
            std::shared_ptr<XamlMetaDataProvider> _appProvider;
            std::shared_ptr<XamlMetaDataProvider> AppProvider()
            {
                if (!_appProvider)
                {
                    _appProvider = std::make_shared<XamlMetaDataProvider>();
                }
                return _appProvider;
            }
        };
        template <typename D, typename... I>
        using AppT2 = App_baseWithProvider<D, I...>;
    }
    ```

6. Erstellen Sie die Lösung, und vergewissern Sie sich, dass Sie erfolgreich erstellt wurde

## <a name="configure-the-desktop-project-to-consume-custom-control-types"></a>Konfigurieren des Desktop Projekts für die Nutzung benutzerdefinierter Steuerelement Typen

Bevor die **MyDesktopWin32App** -App ein benutzerdefiniertes UWP-XAML-Steuerelement in einer XAML-Insel hosten kann, muss Sie so konfiguriert werden, dass Sie benutzerdefinierte Steuerelement Typen aus dem Projekt **myuwpapp** verwendet Hierfür gibt es zwei Möglichkeiten, und Sie können eine der beiden Optionen auswählen, wenn Sie diese exemplarische Vorgehensweise durcharbeiten.

### <a name="option-1-package-the-app-using-msix"></a>Option 1: Verpacken der APP mit msix

Sie können die app in einem [msix-Paket](https://docs.microsoft.com/windows/msix) für die Bereitstellung verpacken. Msix ist die moderne App-Paket Technologie für Windows und basiert auf einer Kombination aus MSI-, AppX-, App-V-und ClickOnce-Installationstechnologien.

1. Fügen Sie der Projekt Mappe ein neues [Windows-Anwendungspaket](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) hinzu. Wenn Sie das Projekt erstellen, benennen Sie es **MyDesktopWin32Project** , und wählen Sie **Windows 10, Version 1903 (10,0; Build 18362)** für die **Zielversion** und die **Mindestversion**.

2. Klicken Sie im Paket Projekt mit der rechten Maustaste auf den Knoten **Anwendungen** , und wählen Sie **Verweis hinzufügen**aus. Aktivieren Sie in der Liste der Projekte das Kontrollkästchen neben dem **MyDesktopWin32App** -Projekt, und klicken Sie auf **OK**.
    ![Verweis Projekt](images/xaml-islands/xaml-island-cpp-6.png)

> [!NOTE]
> Wenn Sie die Anwendung nicht in einem [msix-Paket](https://docs.microsoft.com/windows/msix) für die Bereitstellung packen, muss auf Computern, auf denen Ihre APP ausgeführt wird, [Visual C++ Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) installiert sein.

### <a name="option-2-create-an-application-manifest"></a>Option 2: Erstellen eines Anwendungs Manifests

Sie können Ihrer APP ein [Anwendungs Manifest](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) hinzufügen.

1. Klicken Sie mit der rechten Maustaste auf das Projekt **MyDesktopWin32App** , und wählen Sie -> **Neues Element** **Hinzufügen** 
2. Klicken Sie im Dialogfeld **Neues Element hinzufügen** im linken Bereich auf **Web** , und wählen Sie dann **XML-Datei (. Xml)** aus. 
3. Nennen Sie die neue Datei **app. Manifest** , und klicken Sie auf **Hinzufügen**.
4. Ersetzen Sie den Inhalt der neuen Datei durch den folgenden XML-Code. Dieses XML registriert benutzerdefinierte Steuerelement Typen im **myuwpapp** -Projekt.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <assembly
     xmlns="urn:schemas-microsoft-com:asm.v1"
     xmlns:asmv3="urn:schemas-microsoft-com:asm.v3"
     manifestVersion="1.0">
      <asmv3:file name="MyUWPApp.dll">
        <activatableClass
            name="MyUWPApp.App"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
        <activatableClass
            name="MyUWPApp.XamlMetadataProvider"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
        <activatableClass
            name="MyUWPApp.MyUserControl"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
      </asmv3:file>
    </assembly>
    ```

## <a name="configure-additional-desktop-project-properties"></a>Zusätzliche Desktop Projekteigenschaften konfigurieren

Aktualisieren Sie als nächstes das **MyDesktopWin32App** -Projekt, um ein Makro für zusätzliche include-Verzeichnisse zu definieren und zusätzliche Eigenschaften zu konfigurieren.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt **MyDesktopWin32App** , und wählen Sie **Projekt entladen**aus.

2. Klicken Sie mit der rechten Maustaste auf **MyDesktopWin32App (entladen)** , und wählen Sie **MyDesktopWin32App. vcxproj bearbeiten**aus.

3. Fügen Sie den folgenden XML-Code direkt vor dem schließenden `</Project>` Tag am Ende der Datei hinzu. Speichern und schließen Sie dann die Datei.

    ```xml
      <!-- Configure these for your UWP project -->
      <PropertyGroup>
        <AppProjectName>MyUWPApp</AppProjectName>
      </PropertyGroup>
      <PropertyGroup>
        <AppIncludeDirectories>$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(AppProjectName)\;$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(AppProjectName)\Generated Files\;</AppIncludeDirectories>
      </PropertyGroup>
      <ItemGroup>
        <ProjectReference Include="..\$(AppProjectName)\$(AppProjectName).vcxproj" />
      </ItemGroup>
      <!-- End Section-->
    ```

4. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf **MyDesktopWin32App (entladen)** , und wählen Sie **Projekt erneut laden**aus.

5. Klicken Sie mit der rechten Maustaste auf **MyDesktopWin32App**, wählen Sie **Eigenschaften**aus, und klicken Sie im linken Bereich auf den Knoten **C/C++**  . Vergewissern Sie sich, dass das **zusätzliche include-Verzeichnisse** -Makro aus der Projektdatei Änderung definiert wurde, die Sie im vorherigen Schritt vorgenommen haben.
    ![C/C++ Projekteinstellungen](images/xaml-islands/xaml-island-cpp-7.png)

6. Erweitern Sie im Dialogfeld **Eigenschaften Seiten** die Option **Manifest-Tool** -> **Eingabe und Ausgabe**. Legen Sie die Eigenschaft **dpi** -Wert auf **pro Monitor mit hoher dpi**-Wert fest. Wenn Sie diese Eigenschaft nicht festlegen, kann es in bestimmten Szenarien mit hoher dpi-Wert zu einem Manifest-Konfigurationsfehler kommen.
    ![C/C++ Projekteinstellungen](images/xaml-islands/xaml-island-cpp-8.png)

## <a name="host-the-custom-uwp-xaml-control-in-the-desktop-project"></a>Hosten des benutzerdefinierten UWP-XAML-Steuer Elements im Desktop Projekt

Schließlich können Sie dem **MyDesktopWin32App** -Projekt Code hinzufügen, um das benutzerdefinierte UWP-XAML-Steuerelement zu hosten, das Sie zuvor im **myuwpapp** -Projekt definiert haben.

1. Öffnen Sie im **MyDesktopWin32App** -Projekt die Datei " **Framework. h** ", und kommentieren Sie die folgende Codezeile aus. Speichern Sie die Datei, wenn Sie fertig sind.

    ```cpp
    #define WIN32_LEAN_AND_MEAN
    ```

2. Öffnen Sie die Datei " **MyDesktopWin32App. h** ", und ersetzen Sie den Inhalt dieser Datei durch den folgenden C++Code, um auf die erforderlichen/WinRT-Header Dateien zu verweisen. Speichern Sie die Datei, wenn Sie fertig sind.

    ```cpp
    #pragma once

    #include "resource.h"
    #include <winrt/Windows.Foundation.Collections.h>
    #include <winrt/Windows.system.h>
    #include <winrt/windows.ui.xaml.hosting.h>
    #include <windows.ui.xaml.hosting.desktopwindowxamlsource.h>
    #include <winrt/windows.ui.xaml.controls.h>
    #include <winrt/Windows.ui.xaml.media.h>
    #include <winrt/Windows.UI.Core.h>
    #include <winrt/MyUWPApp.h>

    using namespace winrt;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Xaml::Hosting;
    using namespace Windows::Foundation::Numerics;
    using namespace Windows::UI::Xaml::Controls;
    ```

3. Öffnen Sie die Datei " **MyDesktopWin32App. cpp** ", und fügen Sie den folgenden Code zum Abschnitt "`Global Variables:`" hinzu.

    ```cpp
    winrt::MyUWPApp::App hostApp{ nullptr };
    winrt::Windows::UI::Xaml::Hosting::DesktopWindowXamlSource _desktopWindowXamlSource{ nullptr };
    winrt::MyUWPApp::MyUserControl _myUserControl{ nullptr };
    ```

4. Fügen Sie in derselben Datei dem Abschnitt `Forward declarations of functions included in this code module:` den folgenden Code hinzu.

    ```cpp
    void AdjustLayout(HWND);
    ```

5. Fügen Sie in derselben Datei direkt nach dem `TODO: Place code here.` Kommentar in der `wWinMain`-Funktion den folgenden Code hinzu.

    ```cpp
    // TODO: Place code here.
    winrt::init_apartment(winrt::apartment_type::single_threaded);
    hostApp = winrt::MyUWPApp::App{};
    _desktopWindowXamlSource = winrt::Windows::UI::Xaml::Hosting::DesktopWindowXamlSource{};
    ```

6. Ersetzen Sie in derselben Datei die Standard `InitInstance`-Funktion durch den folgenden Code.

    ```cpp
    BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
    {
        hInst = hInstance; // Store instance handle in our global variable

        HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
            CW_USEDEFAULT, 0, CW_USEDEFAULT, 0, nullptr, nullptr, hInstance, nullptr);

        if (!hWnd)
        {
            return FALSE;
        }

        // Begin XAML Islands walkthrough code.
        if (_desktopWindowXamlSource != nullptr)
        {
            auto interop = _desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
            check_hresult(interop->AttachToWindow(hWnd));
            HWND hWndXamlIsland = nullptr;
            interop->get_WindowHandle(&hWndXamlIsland);
            RECT windowRect;
            ::GetWindowRect(hWnd, &windowRect);
            ::SetWindowPos(hWndXamlIsland, NULL, 0, 0, windowRect.right - windowRect.left, windowRect.bottom - windowRect.top, SWP_SHOWWINDOW);
            _myUserControl = winrt::MyUWPApp::MyUserControl();
            _desktopWindowXamlSource.Content(_myUserControl);
        }
        // End XAML Islands walkthrough code.

        ShowWindow(hWnd, nCmdShow);
        UpdateWindow(hWnd);

        return TRUE;
    }
    ```

7. Fügen Sie in der gleichen Datei die folgende neue Funktion am Ende der Datei hinzu.

    ```cpp
    void AdjustLayout(HWND hWnd)
    {
        if (_desktopWindowXamlSource != nullptr)
        {
            auto interop = _desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
            HWND xamlHostHwnd = NULL;
            check_hresult(interop->get_WindowHandle(&xamlHostHwnd));
            RECT windowRect;
            ::GetWindowRect(hWnd, &windowRect);
            ::SetWindowPos(xamlHostHwnd, NULL, 0, 0, windowRect.right - windowRect.left, windowRect.bottom - windowRect.top, SWP_SHOWWINDOW);
        }
    }
    ```

8. Suchen Sie in derselben Datei die `WndProc`-Funktion. Ersetzen Sie den Standard `WM_DESTROY` Handler in der Switch-Anweisung durch den folgenden Code.

    ```cpp
    case WM_DESTROY:
        PostQuitMessage(0);
        if (_desktopWindowXamlSource != nullptr)
        {
            _desktopWindowXamlSource.Close();
            _desktopWindowXamlSource = nullptr;
        }
        break;
    case WM_SIZE:
        AdjustLayout(hWnd);
        break;
    ```

9. Speichern Sie die Datei.
10. Erstellen Sie die Lösung, und vergewissern Sie sich, dass Sie erfolgreich erstellt wurde

## <a name="test-the-app"></a>Testen der APP

Führen Sie die Lösung aus, und vergewissern Sie sich, dass **MyDesktopWin32App** mit dem folgenden Fenster geöffnet wird

![MyDesktopWin32App-App](images/xaml-islands/xaml-island-cpp-9.png)

## <a name="next-steps"></a>Nächste Schritte

Viele Desktop Anwendungen, die XAML-Inseln hosten, müssen zusätzliche Szenarios verarbeiten, um eine reibungslose Benutzer Leistung zu gewährleisten. Beispielsweise müssen Desktop Anwendungen möglicherweise Tastatureingaben in XAML-Inseln verarbeiten, die Navigation zwischen XAML-Inseln und anderen Benutzeroberflächen Elementen und Layoutänderungen steuern.

Weitere Informationen zur Behandlung dieser Szenarien und Zeiger auf Verwandte Codebeispiele finden Sie unter [Erweiterte Szenarien für XAML-Inseln C++ in Win32-apps](advanced-scenarios-xaml-islands-cpp.md).

## <a name="related-topics"></a>Verwandte Themen

* [Hosten von UWP-XAML-Steuerelementen in Desktop-Apps (XAML-Inseln)](xaml-islands.md)
* [Verwenden der UWP-XAML-Hosting- C++ API in einer Win32-App](using-the-xaml-hosting-api.md)
* [Hosten eines UWP-Standard Steuer C++ Elements in einer Win32-App](host-standard-control-with-xaml-islands-cpp.md)
* [Erweiterte Szenarien für XAML-Inseln C++ in Win32-apps](advanced-scenarios-xaml-islands-cpp.md)
* [Codebeispiele für XAML-Inseln](https://github.com/microsoft/Xaml-Islands-Samples)
