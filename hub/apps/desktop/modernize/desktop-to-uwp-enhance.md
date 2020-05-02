---
Description: Optimieren Sie Ihre Desktop-Anwendung für Windows 10-Benutzer mithilfe von UWP-APIs (Universal Windows Platform).
title: Verwenden von UWP-APIs in Desktopanwendungen
ms.date: 08/20/2019
ms.topic: article
keywords: Windows 10, UWP
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 78d9760c5ef21b29d09babaace0f4379b6a51209
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "75302604"
---
# <a name="call-uwp-apis-in-desktop-apps"></a>Aufrufen von UWP-APIs in Desktop-Apps

Sie können UWP-APIs (Universelle Windows-Plattform) verwenden, um Ihren Desktop-Apps moderne Funktionen für Windows 10-Benutzer hinzuzufügen.

Richten Sie zunächst Ihr Projekt mit den erforderlichen Verweisen ein. Rufen Sie anschließend UWP APIs aus Ihrem Code auf, um Ihrer Desktop-App Windows 10-Funktionen hinzuzufügen. Sie können separate Builds für Windows 10-Benutzer erstellen oder die gleichen Binärdateien für alle Benutzer verteilen – unabhängig davon, welche Version von Windows sie ausführen.

Einige UWP-APIs werden nur in Desktop-Apps unterstützt, die über [Paket-Identität](modernize-packaged-apps.md) verfügen. Weitere Informationen finden Sie unter [Verfügbare UWP-APIs](desktop-to-uwp-supported-api.md).

## <a name="set-up-your-project"></a>Einrichten Ihres Projekts

Um UWP-APIs zu verwenden, müssen Sie einige Änderungen am Projekt vornehmen.

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Ändern eines .NET-Projekts für die Verwendung von Windows Runtime-APIs

Für .NET-Projekte bestehen zwei Optionen:

* Wenn Ihre App auf Windows 10, Version 1803 oder höher, ausgerichtet ist, können Sie ein NuGet-Paket installieren, das alle erforderlichen Verweise bereitstellt.
* Alternativ können Sie die Verweise manuell hinzufügen.

#### <a name="to-use-the-nuget-option"></a>Verwenden der NuGet-Option

1. Vergewissern Sie sich, dass [Paketverweise](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) aktiviert sind:

    1. Klicken Sie in Visual Studio auf **Extras > NuGet-Paket-Manager > Paket-Manager-Einstellungen**.
    2. Achten Sie darauf, dass die Einstellung **Standardformat für Paketverwaltung** auf **PackageReference** festgelegt ist.

2. Klicken Sie bei in Visual Studio geöffnetem Projekt mit der rechten Maustaste im **Projektmappen-Explorer** auf das Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.

3. Wählen Sie im Fenster **NuGet-Paket-Manager** die Registerkarte **Durchsuchen** aus, und suchen Sie nach `Microsoft.Windows.SDK.Contracts`.

4. Nachdem Sie das `Microsoft.Windows.SDK.Contracts`-Paket gefunden haben, wählen Sie im rechten Bereich des **NuGet-Paket-Manager**-Fensters die **Version** des Pakets aus, das Sie installieren möchten, ausgehend von der Windows 10-Version, die Sie als Zielplattform festlegen möchten:

    * **10.0.18362.xxxx**: Wählen Sie dies für Windows 10, Version 1903.
    * **10.0.17763.xxxx**: Wählen Sie dies für Windows 10, Version 1809.
    * **10.0.17134.xxxx**: Wählen Sie dies für Windows 10, Version 1803.

5. Klicken Sie auf **Installieren**.

#### <a name="to-add-the-required-references-manually"></a>Manuelles Hinzufügen der erforderlichen Verweise

1. Öffnen Sie das **Verweis-Manager**-Dialogfeld, wählen Sie die **Durchsuchen**-Schaltfläche, und wählen Sie dann **Alle Dateien** aus.

    ![Dialogfeld „Verweis hinzufügen“](images/desktop-to-uwp/browse-references.png)

2. Fügen Sie einen Verweis auf diese Dateien hinzu.

    |File|Speicherort|
    |--|--|
    |System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.WindowsRuntime.UI.Xaml|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.InteropServices.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |windows.winmd|C:\Programme (x86)\Windows Kits\10\UnionMetadata\\<*SDK-Version*>\Facade|
    |Windows.Foundation.UniversalApiContract.winmd|C:\Programme (x86)\Windows Kits\10\References\\<*Sdk-Version*>\Windows.Foundation.UniversalApiContract\<*Version*>|
    |Windows.Foundation.FoundationContract.winmd|C:\Programme (x86)\Windows Kits\10\References\\<*Sdk-Version*>\Windows.Foundation.FoundationContract\<*Version*>|

3. Legen Sie im Fenster **Eigenschaften** das Feld **Lokale Kopie** für jede *WINMD*-Datei auf **Falsch** fest.

    ![Feld „Lokale Kopie“](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-win32-project-to-use-windows-runtime-apis"></a>Ändern eines C++-Win32-Projekts für die Verwendung von Windows Runtime-APIs

Verwenden Sie [C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/), um Windows Runtime-APIs zu nutzen. C++/WinRT ist eine vollständig standardisierte, moderne C++17-Programmiersprache für Windows-Runtime-APIs (WinRT), die als headerdateibasierte Bibliothek implementiert ist und Ihnen einen erstklassigen Zugriff auf die moderne Windows-API bietet.

Konfigurieren Ihres Projekts für C++/WinRT:

* Für neue Projekte können Sie die [C++/WinRT-Visual Studio-Erweiterung (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) installieren und eine der in dieser Erweiterung enthaltenen C++/WinRT-Projektvorlagen verwenden.
* Für vorhandene Projekte können Sie das [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)-NuGet-Paket im Projekt installieren.

Weitere Informationen zu diesen Optionen finden Sie in [diesem Artikel](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="add-windows-10-experiences"></a>Hinzufügen von Windows 10-Funktionen

Jetzt sind Sie bereit, moderne Funktionen hinzuzufügen, die sichtbar werden, wenn Benutzer Ihre Anwendung unter Windows 10 ausführen. Verwenden Sie diesen Entwurfsablauf.

:white_check_mark: **Entscheiden Sie zunächst, welche Funktionen Sie hinzufügen möchten**

Es stehen viele zur Auswahl. Beispielsweise können Sie mithilfe von [Monetisierungs-APIs](/windows/uwp/monetize) den Kaufablauf vereinfachen oder mehr [Aufmerksamkeit für Ihre Anwendung generieren](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts), wenn Sie etwas Interessantes zu teilen haben, etwa ein neues Bild, das ein anderer Benutzer gepostet hat.

![Toast](images/desktop-to-uwp/toast.png)

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

Sie können Ihre Anwendung für Windows 10 modernisieren, ohne einen neuen Branch zu erstellen und eine separate Codebasis verwalten zu müssen.

Wenn Sie separate Binärdateien für Windows 10-Benutzer erstellen möchten, verwenden Sie bedingte Kompilierung. Wenn Sie einen Satz von Binärdateien für alle Windows-Benutzer erstellen möchten, verwenden Sie Laufzeitprüfungen.

Sehen wir uns beide Optionen kurz an.

### <a name="conditional-compilation"></a>Bedingte Kompilierung

Sie können eine Codebasis beibehalten und einen Satz Binärdateien nur für Windows 10-Benutzer kompilieren.

Fügen Sie Ihrem Projekt zunächst eine neue Buildkonfiguration hinzu.

![Buildkonfiguration](images/desktop-to-uwp/build-config.png)

Erstellen Sie für diese Buildkonfiguration eine Konstante, um den Code zu identifizieren, der Windows Runtime-APIs aufruft.  

Bei .NET-basierten Projekten heißt die Konstante **Bedingte Kompilierungskonstante**.

![Präprozessor](images/desktop-to-uwp/compilation-constants.png)

Für C++ basierte Projekte wird die Konstante als **Präprozessordefinition** bezeichnet.

![Präprozessor](images/desktop-to-uwp/pre-processor.png)

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

Die einfachste Möglichkeit zum Hinzufügen von Laufzeitüberprüfungen zu Ihrem Code besteht darin, dieses NuGet-Paket zu installieren: [Desktop Bridge-Hilfsprogramme](https://www.nuget.org/packages/DesktopBridge.Helpers/) und anschließend die ``IsRunningAsUWP()``-Methode zu verwenden, um den gesamten Code zu deaktivieren, der Windows-Runtime-APIs aufruft. Weitere Informationen finden Sie in diesem Blogbeitrag: [Desktop Bridge: Identifizieren des Anwendungskontexts](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/).

## <a name="related-samples"></a>Verwandte Beispiele

* [„Hello World“-Beispiel](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [Sekundäre Kachel](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Store-API-Beispiel](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [WinForms-App, die einen UWP-UpdateTask implementiert](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [Beispiele für Desktop-App-Bridge zu UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="find-answers-to-your-questions"></a>Antworten auf Ihre Fragen

Haben Sie Fragen? Frage uns auf Stack Overflow. Unser Team überwacht diese [Tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Du kannst uns auch [hier](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D) fragen.
