---
description: Optimieren Sie Ihre Desktop-Anwendung für Windows 10-Benutzer mithilfe von Windows-Runtime-APIs.
title: Windows-Runtime-APIs in Desktop-Apps aufrufen
ms.date: 01/28/2021
ms.topic: article
keywords: Windows 10, UWP
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 2dc56597dccf00a15ffc672f60ca2e1f0936f14f
ms.sourcegitcommit: 6f15cc14e0c4c13999c862664fa7a70de8730b74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98981869"
---
# <a name="call-windows-runtime-apis-in-desktop-apps"></a>Windows-Runtime-APIs in Desktop-Apps aufrufen

Sie können UWP-APIs (Universelle Windows-Plattform) verwenden, um Ihren Desktop-Apps moderne Funktionen für Windows 10-Benutzer hinzuzufügen.

Richten Sie zunächst Ihr Projekt mit den erforderlichen Verweisen ein. Rufen Sie anschließend Windows-Runtime APIs aus Ihrem Code auf, um Ihrer Desktop-App Windows 10-Funktionen hinzuzufügen. Sie können separate Builds für Windows 10-Benutzer erstellen oder die gleichen Binärdateien für alle Benutzer verteilen – unabhängig davon, welche Version von Windows sie ausführen.

Einige Windows-Runtime-APIs werden nur in Desktop-Apps unterstützt, die über [Paket-Identität](modernize-packaged-apps.md) verfügen. Weitere Informationen finden Sie unter [Verfügbare Windows-Runtime-APIs](desktop-to-uwp-supported-api.md).

## <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Ändern eines .NET-Projekts für die Verwendung von Windows Runtime-APIs

Für .NET-Projekte bestehen mehrere Optionen:

* Ab .NET 5 Preview 8 können Sie einen Zielframeworkmoniker (TFM) zu Ihrer Projektdatei hinzufügen, um auf WinRT-APIs zuzugreifen. Diese Option wird in Projekten unterstützt, die auf Windows 10, Version 1809 oder höher, ausgerichtet sind.
* Für frühere .NET-Versionen können Sie das NuGet-Paket `Microsoft.Windows.SDK.Contracts` installieren, um alle notwendigen Referenzen zu Ihrem Projekt hinzuzufügen. Diese Option wird in Projekten unterstützt, die auf Windows 10, Version 1803 oder höher, ausgerichtet sind.
* Wenn Ihr Projekt auf mehrere Ziele ausgerichtet ist – .NET 5 Preview 8 (oder höher) und frühere Versionen von .NET – können Sie die Projektdatei so konfigurieren, dass beide Optionen verwendet werden (Multi-Targeting).

### <a name="net-5-use-the-target-framework-moniker-option"></a>.NET 5: Verwenden der Zielframeworkmoniker-Option

Diese Option wird nur in Projekten unterstützt, die .NET 5 (oder ein höheres Release) verwenden und auf Windows 10, Version 1809, oder ein späteres Release des Betriebssystems ausgerichtet sind. Weitere Hintergrundinformationen zu diesem Szenario finden Sie in [diesem Blogbeitrag](https://blogs.windows.com/windowsdeveloper/2020/09/03/calling-windows-apis-in-net5/).

1. Klicken Sie bei in Visual Studio geöffnetem Projekt mit der rechten Maustaste im **Projektmappen-Explorer** auf das Projekt, und wählen Sie **Projektdatei bearbeiten** aus. Ihre Projektdatei sollte etwa wie folgt aussehen.

    ```csharp
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">
      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>net5.0</TargetFramework>
        <UseWindowsForms>true</UseWindowsForms>
      </PropertyGroup>
    </Project>
    ```

2. Ersetzen Sie den Wert des **TargetFramework**-Elements durch eine der folgenden Zeichenfolgen:

    * **net5.0-windows10.0.17763.0**: Verwenden Sie diesen Wert, wenn Ihre App auf Windows 10, Version 1809, ausgerichtet ist.
    * **net5.0-windows10.0.18362.0**: Verwenden Sie diesen Wert, wenn Ihre App auf Windows 10, Version 1903, ausgerichtet ist.
    * **net5.0-windows10.0.19041.0**: Verwenden Sie diesen Wert, wenn Ihre App auf Windows 10, Version 2004, ausgerichtet ist.

    Das folgende Element ist zum Beispiel für ein Projekt bestimmt, das auf Windows 10, Version 2004, ausgerichtet ist.

    ```csharp
    <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
    ```

3. Speichern Sie Ihre Änderungen, und schließen Sie die Projektdatei.

### <a name="earlier-versions-of-net-install-the-microsoftwindowssdkcontracts-nuget-package"></a>Frühere .NET-Versionen: Installieren Sie das NuGet-Paket „Microsoft.Windows.SDK.Contracts“.

Verwenden Sie diese Option, wenn Ihre Anwendung .NET Core 3.x, .NET 5 Preview 7 (oder früher) oder .NET Framework verwendet. Diese Option wird in Projekten unterstützt, die auf Windows 10, Version 1803, oder ein späteres Release des Betriebssystems ausgerichtet sind.

1. Vergewissern Sie sich, dass [Paketverweise](/nuget/consume-packages/package-references-in-project-files) aktiviert sind:

    1. Klicken Sie in Visual Studio auf **Extras > NuGet-Paket-Manager > Paket-Manager-Einstellungen**.
    2. Achten Sie darauf, dass die Einstellung **Standardformat für Paketverwaltung** auf **PackageReference** festgelegt ist.

2. Klicken Sie bei in Visual Studio geöffnetem Projekt mit der rechten Maustaste im **Projektmappen-Explorer** auf das Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.

3. Wählen Sie im Fenster **NuGet-Paket-Manager** die Registerkarte **Durchsuchen** aus, und suchen Sie nach `Microsoft.Windows.SDK.Contracts`.

4. Nachdem Sie das `Microsoft.Windows.SDK.Contracts`-Paket gefunden haben, wählen Sie im rechten Bereich des **NuGet-Paket-Manager**-Fensters die **Version** des Pakets aus, das Sie installieren möchten, ausgehend von der Windows 10-Version, die Sie als Zielplattform festlegen möchten:

    * **10.0.19041.xxxx**: Wählen Sie dies für Windows 10, Version 2004.
    * **10.0.18362.xxxx**: Wählen Sie dies für Windows 10, Version 1903.
    * **10.0.17763.xxxx**: Wählen Sie dies für Windows 10, Version 1809.
    * **10.0.17134.xxxx**: Wählen Sie dies für Windows 10, Version 1803.

5. Klicken Sie auf **Installieren**.

### <a name="configure-projects-that-multi-target-different-versions-of-net"></a>Konfigurieren von Projekten, die auf verschiedene Versionen von .NET ausgerichtet sind (Multi-Targeting)

Wenn Ihr Projekt auf mehrere Ziele ausgerichtet ist – .NET 5 Preview 8 (oder höher) sowie frühere Versionen (beispielweise .NET Core 3.x und .NET Framework) – können Sie die Projektdatei so konfigurieren, dass der Zielframeworkmoniker verwendet wird, um die WinRT-API-Referenzen für .NET 5 automatisch per Pull abzurufen und das `Microsoft.Windows.SDK.Contracts`-NuGet-Paket für frühere Versionen zu verwenden.

1. Klicken Sie bei in Visual Studio geöffnetem Projekt mit der rechten Maustaste im **Projektmappen-Explorer** auf das Projekt, und wählen Sie **Projektdatei bearbeiten** aus. Das folgende Beispiel zeigt eine Projektdatei für eine App, die .NET Core 3.1 verwendet.

    ```csharp
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">
      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.1</TargetFramework>
        <UseWindowsForms>true</UseWindowsForms>
      </PropertyGroup>
    </Project>
    ```

2. Ersetzen Sie das **TargetFramework**-Element in der Datei durch ein **TargetFrameworks**-Element (beachten Sie den Plural). Geben Sie in diesem Element die Zielframeworkmoniker für alle Versionen von .NET an, die Sie als Ziel verwenden möchten, getrennt durch Semikolons. 

    * Für .NET 5 Preview 8 oder höher verwenden Sie einen der folgenden Zielframeworkmoniker:
        * **net5.0-windows10.0.17763.0**: Verwenden Sie diesen Wert, wenn Ihre App auf Windows 10, Version 1809, ausgerichtet ist.
        * **net5.0-windows10.0.18362.0**: Verwenden Sie diesen Wert, wenn Ihre App auf Windows 10, Version 1903, ausgerichtet ist.
        * **net5.0-windows10.0.19041.0**: Verwenden Sie diesen Wert, wenn Ihre App auf Windows 10, Version 2004, ausgerichtet ist.
    * Verwenden Sie für .NET Core 3.x **netcoreapp3.0** oder **netcoreapp3.1**.
    * Verwenden Sie für .NET Framework **net46**.

    Das folgende Beispiel veranschaulicht, wie .NET Core 3.1 und .NET 5 Preview 8 (für Windows 10, Version 2004) auf mehrere Ziele ausgerichtet werden können.

    ```csharp
    <TargetFrameworks>netcoreapp3.1;net5.0-windows10.0.19041.0</TargetFrameworks>
    ```

3. Fügen Sie nach dem **PropertyGroup**-Element ein **PackageReference**-Element hinzu, das eine bedingte Anweisung enthält, die das NuGet-Paket `Microsoft.Windows.SDK.Contracts` für alle Versionen von .NET Core 3.x oder .NET Framework installiert, auf die Ihre App ausgerichtet ist. Das **PackageReference**-Element muss ein untergeordnetes Element des **ItemGroup**-Elements sein. Das folgende Beispiel veranschaulicht die Vorgehensweise für .NET Core 3.1.

    ```csharp
    <ItemGroup>
      <PackageReference Condition="'$(TargetFramework)' == 'netcoreapp3.1'"
                        Include="Microsoft.Windows.SDK.Contracts"
                        Version="10.0.19041.0" />
    </ItemGroup>
    ```

    Wenn Sie fertig sind, sollte Ihre Projektdatei etwa wie folgt aussehen.

    ```csharp
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">
      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFrameworks>netcoreapp3.1;net5.0-windows10.0.19041.0</TargetFrameworks>
        <UseWPF>true</UseWPF>
      </PropertyGroup>
      <ItemGroup>
        <PackageReference Condition="'$(TargetFramework)' == 'netcoreapp3.1'"
                         Include="Microsoft.Windows.SDK.Contracts"
                         Version="10.0.19041.0" />
      </ItemGroup>
    </Project>
    ```

4. Speichern Sie Ihre Änderungen, und schließen Sie die Projektdatei.

## <a name="modify-a-c-win32-project-to-use-windows-runtime-apis"></a>Ändern eines C++-Win32-Projekts für die Verwendung von Windows Runtime-APIs

Verwenden Sie [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/), um Windows Runtime-APIs zu nutzen. C++/WinRT ist eine vollständig standardisierte, moderne C++17-Programmiersprache für Windows-Runtime-APIs (WinRT), die als headerdateibasierte Bibliothek implementiert ist und Ihnen einen erstklassigen Zugriff auf die moderne Windows-API bietet.

Konfigurieren Ihres Projekts für C++/WinRT:

* Für neue Projekte können Sie die [C++/WinRT-Visual Studio-Erweiterung (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) installieren und eine der in dieser Erweiterung enthaltenen C++/WinRT-Projektvorlagen verwenden.
* Für vorhandene Projekte können Sie das [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)-NuGet-Paket im Projekt installieren.

Weitere Informationen zu diesen Optionen finden Sie in [diesem Artikel](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="add-windows-10-experiences"></a>Hinzufügen von Windows 10-Funktionen

Jetzt sind Sie bereit, moderne Funktionen hinzuzufügen, die sichtbar werden, wenn Benutzer Ihre Anwendung unter Windows 10 ausführen. Verwenden Sie diesen Entwurfsablauf.

:white_check_mark: **Entscheiden Sie zunächst, welche Funktionen Sie hinzufügen möchten**

Es stehen viele zur Auswahl. Beispielsweise können Sie mithilfe von [Monetisierungs-APIs](/windows/uwp/monetize) den Kaufablauf vereinfachen oder mehr [Aufmerksamkeit für Ihre Anwendung generieren](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts), wenn Sie etwas Interessantes zu teilen haben, etwa ein neues Bild, das ein anderer Benutzer gepostet hat.

![Popupbenachrichtigung](images/desktop-to-uwp/toast.png)

Auch dann, wenn Benutzer Ihre Nachricht ignorieren oder schließen, können sie diese im Info-Center anzeigen und dann auf die Nachricht klicken, um Ihre App zu öffnen. Dies erhöht die Interaktion mit Ihrer Anwendung und hat den zusätzlichen Vorteil, das Sie die Anwendung tief in das Betriebssystem integrieren. Wir zeigen Ihnen den Code für diese Funktion weiter unten in diesem Artikel.

Weitere Ideen finden Sie in der [UWP-Dokumentation](/windows/uwp/get-started/).

:white_check_mark: **Entscheiden Sie, ob Sie die App verbessern oder erweitern möchten**

Wir verwenden häufig die Begriffe *verbessern* und *erweitern*. Daher möchten wir erklären, was diese Begriffe bedeuten.

Wir verwenden den Begriff *verbessern*, um Windows-Runtime-APIs zu beschreiben, die Sie direkt aus Ihrer Desktop-App aufrufen können (unabhängig davon, ob Sie sich entschieden haben, die Anwendung in einem MSIX-Paket zu verpacken). Wenn Sie eine Windows 10-Funktion ausgewählt haben, identifizieren Sie die APIs, die Sie benötigen, um sie zu erstellen. Überprüfen Sie dann, ob die API in [dieser Liste](desktop-to-uwp-supported-api.md) aufgeführt wird. Hierbei handelt es sich um eine Liste der APIs, die Sie direkt aus Ihrer Desktopanwendung aufrufen können. Wenn Ihre API nicht in dieser Liste aufgeführt ist, kann die Funktionalität der zugeordneten API nur in einem UWP-Prozess ausgeführt werden. Oftmals sind dies APIs, die UWP XAML rendern, etwa ein UWP-Kartensteuerelement oder eine Windows Hello-Sicherheitsaufforderung.

> [!NOTE]
> Zwar können APIs, die UWP XAML rendern, normalerweise nicht direkt aus Ihrer Desktopanwendung aufgerufen werden, möglicherweise können Sie aber alternative Vorgehensweisen verwenden. Wenn Sie UWP-XAML-Steuerelemente oder andere benutzerdefinierte visuelle Funktionen hosten möchten, können Sie [XAML Islands](xaml-islands.md) (ab Windows 10, Version 1903) und die [visuelle Ebene](visual-layer-in-desktop-apps.md) (ab Windows 10, Version 1803) verwenden. Diese Features können in verpackten oder unverpackten Desktop-Apps verwendet werden.

Wenn Sie sich dafür entschieden haben, Ihre Desktop-App in einem MSIX-Paket zu verpacken, besteht eine weitere Möglichkeit darin, die Anwendung zu *erweitern*, indem Sie Ihrer Projektmappe ein UWP-Projekt hinzufügen. Das Desktop-Projekt bildet immer noch den Einstiegspunkt der Anwendung, aber das UWP-Projekt ermöglicht den Zugriff auf alle APIs, die nicht in [dieser Liste](desktop-to-uwp-supported-api.md) aufgeführt sind. Die Desktopanwendung kann mithilfe eines App-Diensts mit dem UWP-Prozess kommunizieren. Wir stellen zahlreiche Anleitungen zum Einrichten dieser Kommunikation bereit. Wenn Sie eine Funktion hinzufügen möchten, die ein UWP-Projekt erfordert, finden Sie unter [Erweitern mit UWP-Komponenten](desktop-to-uwp-extend.md) weitere Informationen.

:white_check_mark: **Verweisen auf API-Verträge**

Wenn Sie die API direkt aus Ihrer Desktopanwendung aufrufen können, öffnen Sie einen Browser, und suchen Sie nach dem Verweisthema für diese API.
Unterhalb der Zusammenfassung der API finden Sie eine Tabelle, die den API-Vertrag für diese API beschreibt. Hier sehen Sie ein Beispiel für diese Tabelle:

![API-Vertragstabelle](images/desktop-to-uwp/contract-table.png)

Wenn Sie eine .NET-basierte Desktop-App haben, fügen Sie einen Verweis auf den API-Vertrag hinzu, und legen Sie dann die Eigenschaft **Lokale Kopie** dieser Datei auf **Falsch** fest. Wenn Sie ein C++-basiertes Projekt haben, fügen Sie Ihren **Zusätzlichen Include-Verzeichnissen** einen Pfad zu dem Ordner hinzu, der diesen Vertrag enthält.

:white_check_mark: **Aufrufen der APIs, um die Funktion hinzuzufügen**

Dies ist der Code, den Sie verwenden würden, um das Benachrichtigungsfenster anzuzeigen, das wir weiter oben behandelt haben. Diese APIs werden in dieser [Liste](desktop-to-uwp-supported-api.md) aufgeführt, d. h. Sie können Ihrer Desktopanwendung diesen Code hinzufügen und ihn sofort ausführen.

```csharp
using Windows.Foundation;
using Windows.System;
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;
...

private void ShowToast()
{
    string title = "featured picture of the day";
    string content = "beautiful scenery";
    string image = "https://picsum.photos/360/180?image=104";
    string logo = "https://picsum.photos/64?image=883";

    string xmlString =
    $@"<toast><visual>
       <binding template='ToastGeneric'>
       <text>{title}</text>
       <text>{content}</text>
       <image src='{image}'/>
       <image src='{logo}' placement='appLogoOverride' hint-crop='circle'/>
       </binding>
      </visual></toast>";

    XmlDocument toastXml = new XmlDocument();
    toastXml.LoadXml(xmlString);

    ToastNotification toast = new ToastNotification(toastXml);

    ToastNotificationManager.CreateToastNotifier().Show(toast);
}
```

```cppwinrt
#include <sstream>
#include <winrt/Windows.Data.Xml.Dom.h>
#include <winrt/Windows.UI.Notifications.h>

using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::System;
using namespace winrt::Windows::UI::Notifications;
using namespace winrt::Windows::Data::Xml::Dom;

void UWP::ShowToast()
{
    std::wstring const title = L"featured picture of the day";
    std::wstring const content = L"beautiful scenery";
    std::wstring const image = L"https://picsum.photos/360/180?image=104";
    std::wstring const logo = L"https://picsum.photos/64?image=883";

    std::wostringstream xmlString;
    xmlString << L"<toast><visual><binding template='ToastGeneric'>" <<
        L"<text>" << title << L"</text>" <<
        L"<text>" << content << L"</text>" <<
        L"<image src='" << image << L"'/>" <<
        L"<image src='" << logo << L"'" <<
        L" placement='appLogoOverride' hint-crop='circle'/>" <<
        L"</binding></visual></toast>";

    XmlDocument toastXml;

    toastXml.LoadXml(xmlString.str().c_str());

    ToastNotificationManager::CreateToastNotifier().Show(ToastNotification(toastXml));
}
```

```cppcx
using namespace Windows::Foundation;
using namespace Windows::System;
using namespace Windows::UI::Notifications;
using namespace Windows::Data::Xml::Dom;

void UWP::ShowToast()
{
    Platform::String ^title = "featured picture of the day";
    Platform::String ^content = "beautiful scenery";
    Platform::String ^image = "https://picsum.photos/360/180?image=104";
    Platform::String ^logo = "https://picsum.photos/64?image=883";

    Platform::String ^xmlString =
        L"<toast><visual><binding template='ToastGeneric'>" +
        L"<text>" + title + "</text>" +
        L"<text>"+ content + "</text>" +
        L"<image src='" + image + "'/>" +
        L"<image src='" + logo + "'" +
        L" placement='appLogoOverride' hint-crop='circle'/>" +
        L"</binding></visual></toast>";

    XmlDocument ^toastXml = ref new XmlDocument();

    toastXml->LoadXml(xmlString);

    ToastNotificationManager::CreateToastNotifier()->Show(ref new ToastNotification(toastXml));
}
```

Weitere Informationen zu Benachrichtigungen finden Sie unter [Adaptive und interaktive Popupbenachrichtigungen](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts).

## <a name="support-windows-xp-windows-vista-and-windows-78-install-bases"></a>Unterstützung der Windows XP-, Windows Vista- und Windows 7/8-Installationsbasis

Sie können Ihre Anwendung für Windows 10 modernisieren, ohne einen neuen Branch zu erstellen und eine separate Codebasis verwalten zu müssen.

Wenn Sie separate Binärdateien für Windows 10-Benutzer erstellen möchten, verwenden Sie bedingte Kompilierung. Wenn Sie einen Satz von Binärdateien für alle Windows-Benutzer erstellen möchten, verwenden Sie Laufzeitprüfungen.

Sehen wir uns beide Optionen kurz an.

### <a name="conditional-compilation"></a>Bedingte Kompilierung

Sie können eine Codebasis beibehalten und einen Satz Binärdateien nur für Windows 10-Benutzer kompilieren.

Fügen Sie Ihrem Projekt zunächst eine neue Buildkonfiguration hinzu.

![Buildkonfiguration](images/desktop-to-uwp/build-config.png)

Erstellen Sie für diese Buildkonfiguration eine Konstante, um den Code zu identifizieren, der Windows Runtime-APIs aufruft.  

Bei .NET-basierten Projekten heißt die Konstante **Bedingte Kompilierungskonstante**.

![Konstante „Bedingte Kompilierung“](images/desktop-to-uwp/compilation-constants.png)

Für C++ basierte Projekte wird die Konstante als **Präprozessordefinition** bezeichnet.

![Konstante „Präprozessordefinition“](images/desktop-to-uwp/pre-processor.png)

Fügen Sie diese Konstante vor jedem UWP-Codeblock hinzu.

```csharp
[System.Diagnostics.Conditional("_UWP")]
private void ShowToast()
{
 ...
}
```

```C++
#if _UWP
void UWP::ShowToast()
{
 ...
}
#endif
```

Der Compiler erstellt den Code nur dann, wenn die Konstante in der aktiven Buildkonfiguration definiert ist.

### <a name="runtime-checks"></a>Laufzeitüberprüfungen

Sie können einen Satz von Binärdateien für alle Windows-Benutzer kompilieren, unabhängig davon, welche Version von Windows sie ausführen. Ihre Anwendung ruft Windows-Runtime-APIs nur dann auf, wenn der Benutzer Ihre Anwendung als verpackte Anwendung unter Windows 10 ausführt.

Die einfachste Möglichkeit zum Hinzufügen von Laufzeitüberprüfungen zu Ihrem Code besteht darin, dieses NuGet-Paket zu installieren: [Desktop Bridge-Hilfsprogramme](https://www.nuget.org/packages/DesktopBridge.Helpers/) und anschließend die ``IsRunningAsUWP()``-Methode zu verwenden, um den gesamten Code zu deaktivieren, der Windows-Runtime-APIs aufruft. Weitere Informationen finden Sie in diesem Blogbeitrag: [Desktop Bridge: Identifizieren des Anwendungskontexts](/archive/blogs/appconsult/desktop-bridge-identify-the-applications-context).

## <a name="related-samples"></a>Verwandte Beispiele

* [„Hello World“-Beispiel](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [Sekundäre Kachel](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Store-API-Beispiel](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [WinForms-App, die einen UWP-UpdateTask implementiert](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [Beispiele für Desktop-App-Bridge zu UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="find-answers-to-your-questions"></a>Antworten auf Ihre Fragen

Haben Sie Fragen? Frage uns auf Stack Overflow. Unser Team überwacht diese [Tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Du kannst uns auch [hier](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D) fragen.
