---
Description: Verbessern Sie Ihre Desktop Anwendung für Windows 10-Benutzer mithilfe von universelle Windows-Plattform-APIs (UWP).
title: Verwenden von UWP-APIs in Desktop-Apps
ms.date: 08/20/2019
ms.topic: article
keywords: Windows 10, UWP
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: bcdeafc3f30f5b385c6feeddee78cf31635177a0
ms.sourcegitcommit: d7eccdb27c22bccac65bd014e62b6572a6b44602
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73142539"
---
# <a name="call-uwp-apis-in-desktop-apps"></a>UWP-APIs in Desktop-Apps aufzurufen

Sie können universelle Windows-Plattform-APIs (UWP) verwenden, um Ihren Desktop-Apps moderne Erfahrungen hinzuzufügen, die für Windows 10-Benutzer sorgen.

Richten Sie zunächst das Projekt mit den erforderlichen verweisen ein. Anschließend können Sie UWP-APIs aus Ihrem Code abrufen, um ihrer Desktop-App Windows 10-Erfahrungen hinzuzufügen. Sie können für Windows 10-Benutzer separat erstellen oder die gleichen Binärdateien unabhängig von der Windows-Version, die Sie ausführen, an alle Benutzer verteilen.

Einige UWP-APIs werden nur in Desktop-Apps unterstützt, die über eine [Paket Identität](modernize-packaged-apps.md)verfügen. Weitere Informationen finden Sie unter [Verfügbare UWP-APIs](desktop-to-uwp-supported-api.md).

## <a name="set-up-your-project"></a>Einrichten des Projekts

Sie müssen einige Änderungen am Projekt vornehmen, um UWP-APIs zu verwenden.

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Ändern eines .NET-Projekts für die Verwendung von Windows-Runtime-APIs

Es gibt zwei Optionen für .net-Projekte:

* Wenn Ihre APP auf Windows 10, Version 1803 oder höher, ausgerichtet ist, können Sie ein nuget-Paket installieren, das alle erforderlichen Verweise bereitstellt.
* Alternativ können Sie die Verweise manuell hinzufügen.

#### <a name="to-use-the-nuget-option"></a>So verwenden Sie die nuget-Option

1. Stellen Sie sicher, dass die [Paket Verweise](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) aktiviert sind:

    1. Klicken Sie in Visual Studio auf Extras **-> nuget-Paket-Manager-> Paket-Manager-Einstellungen**.
    2. Stellen Sie sicher, dass **packagereferenzierung** für das **standardmäßige Paket Verwaltungs Format**ausgewählt ist.

2. Wenn das Projekt in Visual Studio geöffnet ist, klicken Sie in **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **nuget-Pakete verwalten**.

3. Vergewissern Sie sich, dass im Fenster **nuget-Paket-Manager** die Option **Vorabversion einschließen** ausgewählt ist. Wählen Sie dann die Registerkarte **Durchsuchen** aus, und suchen Sie nach `Microsoft.Windows.SDK.Contracts`.

4. Nachdem Sie das `Microsoft.Windows.SDK.Contracts` Paket gefunden haben, wählen Sie im rechten Bereich des Fensters **nuget-Paket-Manager** die **Version** des zu installierenden Pakets basierend auf der Version von Windows 10 aus, die Sie als Zielversion auswählen möchten:

    * **10.0.18362. xxxx-Preview**: Wählen Sie diese Option für Windows 10, Version 1903.
    * **10.0.17763. xxxx-Preview**: Wählen Sie diese Option für Windows 10, Version 1809.
    * **10.0.17134. xxxx-Preview**: Wählen Sie diese Option für Windows 10, Version 1803.

5. Klicken Sie auf **Installieren**.

#### <a name="to-add-the-required-references-manually"></a>So fügen Sie die erforderlichen Verweise manuell hinzu

1. Öffnen Sie das **Verweis-Manager**-Dialogfeld, wählen Sie die **Durchsuchen**-Schaltfläche, und wählen Sie dann **Alle Dateien** aus.

    ![Dialogfeld „Verweis hinzufügen“](images/desktop-to-uwp/browse-references.png)

2. Fügen Sie einen Verweis auf diese Dateien hinzu.

    |Datei|Pfad|
    |--|--|
    |System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.WindowsRuntime.UI.Xaml|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.InteropServices.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |Windows. winmd|C:\Programme (x86) \Windows kits\10\unionmetadata\\<*SDK-Version*> \facade|
    |Windows.Foundation.UniversalApiContract.winmd|C:\Programme (x86) \Windows kits\10\references\\<*SDK-Version*> \Windows.Foundation.UniversalApiContract\<*Version*>|
    |Windows.Foundation.FoundationContract.winmd|C:\Programme (x86) \Windows kits\10\references\\<*SDK-Version*> \Windows.Foundation.FoundationContract\<*Version*>|

3. Legen Sie im Dialogfeld **Eigenschaften** die **lokale Kopie** jeder *winmd*-Datei auf **Falsch** fest.

    ![„Lokal kopieren“-Feld](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-win32-project-to-use-windows-runtime-apis"></a>Ändern eines C++ Win32-Projekts für die Verwendung von Windows-Runtime-APIs

Verwenden Sie [ C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/) , um Windows-Runtime-APIs zu verwenden. C++/WinRT ist eine vollständig standardisierte, moderne C++17-Programmiersprache für Windows-Runtime-APIs (WinRT), die als headerdateibasierte Bibliothek implementiert ist und Ihnen einen erstklassigen Zugriff auf die moderne Windows-API bietet.

So konfigurieren Sie Ihr Projekt C++für/WinRT:

* Bei neuen Projekten können Sie die [ C++/WinRT Visual Studio-Erweiterung (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) installieren und eine der C++/WinRT-Projektvorlagen verwenden, die in dieser Erweiterung enthalten sind.
* Bei vorhandenen Projekten können Sie das nuget-Paket [Microsoft. Windows. cppwinrt](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) im Projekt installieren.

Weitere Informationen zu diesen Optionen finden Sie in [diesem Artikel](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="add-windows-10-experiences"></a>Windows 10-Funktionen hinzufügen

Jetzt können Sie moderner Funktionen für Benutzer der Anwendung unter Windows 10 Hinzufügen. Verwenden Sie diesen Designfluss.

:white_check_mark: **Entscheiden Sie zunächst, welche Funktionen Sie hinzufügen möchten**

Es gibt viele zur Auswahl. Beispielsweise können Sie Ihren Bestell Fluss mithilfe von [monetarisierungsapis](/windows/uwp/monetize)vereinfachen oder eine [direkte Aufmerksamkeit auf Ihre Anwendung](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts) haben, wenn Sie etwas interessantes freigeben möchten, z. b. ein neues Bild, das von einem anderen Benutzer gepostet wurde.

![Popup](images/desktop-to-uwp/toast.png)

Auch dann, wenn Benutzer Ihre Nachricht ignorieren oder schließen, können sie diese im Info-Center anzeigen und dann auf die Nachricht klicken, um Ihre App zu öffnen. Dies steigert die Einbindung Ihrer Anwendung und hat den zusätzlichen Vorteil, dass Ihre Anwendung tief in das Betriebssystem integriert werden muss. Wir zeigen Ihnen den Code für diese Darstellung etwas weiter unten in diesem Artikel.

Weitere Ideen finden Sie in der [UWP-Dokumentation](/windows/uwp/get-started/) .

:white_check_mark: **Entscheiden Sie, ob Sie die App verbessern oder erweitern möchten**

Häufig werden wir die Begriffe " *verbessern* " und " *erweitern*" verwenden, um genau zu erläutern, was die einzelnen Begriffe bedeuten.

Wir verwenden den Begriff " *verbessern* ", um Windows-Runtime APIs zu beschreiben, die Sie direkt von Ihrer Desktop-App aus anrufen können (unabhängig davon, ob Sie die Anwendung in einem msix-Paket verpacken möchten). Wenn Sie eine Windows 10-Erfahrung ausgewählt haben, identifizieren Sie die APIs, die Sie zum Erstellen benötigen, und prüfen Sie dann, ob diese API in [dieser Liste](desktop-to-uwp-supported-api.md)angezeigt wird. Dies ist eine Liste von APIs, die Sie direkt von Ihrer Desktop-App aus abrufen können. Wenn Ihre API nicht in dieser Liste angezeigt wird, kann die Funktionalität der zugeordneten API nur in einem UWP-Prozess ausgeführt werden. Häufig enthalten diese APIs APIs, die UWP-XAML wie z. b. ein UWP-Karten Steuerelement oder eine Windows Hello-Sicherheits Aufforderung darstellen.

> [!NOTE]
> Obwohl APIs zum Rendering von UWP-XAML in der Regel nicht direkt von Ihrem Desktop aufgerufen werden können, können Sie möglicherweise alternative Ansätze verwenden. Wenn Sie UWP-XAML-Steuerelemente oder andere benutzerdefinierte visuelle Funktionen hosten möchten, können Sie [XAML-Inseln](xaml-islands.md) (beginnend mit Windows 10, Version 1903) und die [visuelle Ebene](visual-layer-in-desktop-apps.md) (beginnend mit Windows 10, Version 1803) verwenden. Diese Features können in App-Paketen oder nicht verpackten Desktop-Apps verwendet werden.

Wenn Sie die Desktop-app in einem msix-Paket verpacken möchten, können Sie die Anwendung durch Hinzufügen eines UWP-Projekts zur Projekt Mappe *erweitern* . Das Desktop Projekt ist immer noch der Einstiegspunkt der Anwendung, aber das UWP-Projekt bietet Ihnen Zugriff auf alle APIs, die nicht in [dieser Liste](desktop-to-uwp-supported-api.md)angezeigt werden. Die Desktop-App kann mithilfe eines App Service mit dem UWP-Prozess kommunizieren, und wir haben viele Anleitungen dazu, wie Sie diese Vorgehensweise einrichten. Weitere Informationen zum Hinzufügen einer benutzerdefinierten Funktion, die ein UWP-Projekt erfordert, finden Sie unter [erweitern mit UWP-Komponenten](desktop-to-uwp-extend.md).

:white_check_mark: **Referenz-API-Verträge**

Wenn Sie die API direkt von Ihrer Desktop-App aus anrufen können, öffnen Sie einen Browser, und suchen Sie nach dem Referenz Thema für diese API.
Unterhalb der Zusammenfassung der API finden Sie eine Tabelle, die die API-Vertrag für diese API beschreibt. Dies ist ein Beispiel.

![API-Vertrag-Tabelle](images/desktop-to-uwp/contract-table.png)

Wenn Sie eine .NET-basierte Desktop-App haben, fügen Sie einen Verweis auf den API-Vertrag hinzu und legen Sie die **Lokale Kopie** Eigenschaft dieser Datei auf **Falsch** fest. Wenn Sie ein C++-basiertes Projekt haben, fügen Sie **Zusätzliche Include-Verzeichnisse** einen Pfad zu dem Ordner hinzu, der diesen Vertrag enthält.

:white_check_mark: **Aufrufen der APIs, um die Funktion hinzuzufügen**

Dies ist der Code, den Sie verwenden würden, um dem Benachrichtigungsfenster anzuzeigen, dass wir weiter oben behandelt haben. Diese APIs werden in dieser [Liste](desktop-to-uwp-supported-api.md) angezeigt, sodass Sie den Code Ihrer Desktop-App hinzufügen und sofort ausführen können.

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

```C++
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

Weitere Informationen zu Benachrichtigungen finden Sie unter [Adaptive und interaktive Popupbenachrichtigungen](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts).

## <a name="support-windows-xp-windows-vista-and-windows-78-install-bases"></a>Unterstützung der Windows XP-, Windows Vista- und Windows 7/8-Installationsbasis

Sie können Ihre Anwendung für Windows 10 modernisieren, ohne einen neuen Branch erstellen und separate Codebasen verwalten zu müssen.

Wenn Sie separate Binärdateien für Windows 10-Benutzer erstellen möchten, verwenden Sie die bedingten Kompilierung. Wenn Sie einen Satz von Binärdateien für alle Windows-Benutzer erstellen möchten, verwenden Sie Laufzeitprüfungen.

Schauen wir uns die Optionen an.

### <a name="conditional-compilation"></a>Bedingte Kompilierung

Sie können eine Codebasis beibehalten und einen Satz Binärdateien nur für Windows 10-Benutzer kompilieren.

Fügen Sie Ihrem Projekt zuerst eine neue Buildkonfiguration hinzu.

![Buildkonfiguration](images/desktop-to-uwp/build-config.png)

Erstellen Sie für diese Buildkonfiguration eine Konstante, die den Code identifiziert, der Windows-Runtime-APIs aufruft.  

Für .NET-basierte Projekte heißt die Konstante **Bedingte Kompilierungskonstante**.

![Preprozessor](images/desktop-to-uwp/compilation-constants.png)

Für C++ basierte Projekte heißt die Konstante **Preprozessordefinition**.

![Preprozessor](images/desktop-to-uwp/pre-processor.png)

Fügen Sie die Konstanten vor jedem UWP-Codeblock hinzu.

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

### <a name="runtime-checks"></a>Laufzeitprüfungen

Sie können einen Satz von Binärdateien für alle Windows-Benutzer kompilieren, unabhängig davon, welche Version von Windows sie ausführen. Die Anwendung ruft Windows-Runtime APIs nur auf, wenn der Benutzer die Anwendung als Paket Anwendung unter Windows 10 ausführt.

Die einfachste Möglichkeit zum Hinzufügen von Laufzeitüberprüfungen zum Code besteht darin, dieses nuget-Paket zu installieren: [desktopbridge-](https://www.nuget.org/packages/DesktopBridge.Helpers/) Hilfsprogramme und dann die ``IsRunningAsUWP()``-Methode, um den gesamten Code zu deaktivieren, der Windows-Runtime-APIs aufruft. Weitere Detail finden Sie in diesem Blogbeitrag: [Desktop-Brücke - Identifizieren des Anwendungskontexts](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/).

## <a name="related-samples"></a>Verwandte Beispiele

* [Hallo Welt Beispiel](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [Sekundäre Kachel](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Store-API-Beispiel](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [WinForms-Anwendung, die eine UWP-Updatetask implementiert](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [Desktop-App-Bridge zu UWP-Beispielen](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="support-and-feedback"></a>Support und Feedback

**Antworten auf Ihre Fragen**

Haben Sie Fragen? Fragen Sie uns auf Stack Overflow. Unser Team überwacht diese [Tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Fragen Sie uns [hier](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Feedback geben oder Funktions Vorschläge machen**

Weitere Informationen finden Sie unter [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
