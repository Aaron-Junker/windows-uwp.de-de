---
Description: Optimieren Sie die desktop-Anwendung für Windows 10-Benutzer mithilfe von universellen Windows-Plattform (UWP) APIs.
Search.Product: eADQiWindows 10XVcnh
title: Verbessern Ihrer Desktopanwendung für Windows 10
ms.date: 10/15/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0e06de7d33604981e0055d9d57f774d280753db2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628035"
---
# <a name="enhance-your-desktop-application-for-windows-10"></a>Verbessern Ihrer Desktopanwendung für Windows 10

Sie können Windows-Runtime-APIs verwenden, moderne Funktionen hinzufügen, die für Windows 10-Benutzern aufleuchten.

Richten Sie zuerst Ihr Projekt ein. Dann fügen Sie Windows 10-Funktionen hinzu. Sie können separate Builds für Windows 10-Benutzer erstellen oder die gleichen Binärdateien für alle Benutzer verteilen – unabhängig davon, welche Version von Windows sie ausführen.

## <a name="first-set-up-your-project"></a>Einrichten Ihres Projekts

Sie müssen einige Änderungen am Projekt vornehmen, um UWP-APIs zu verwenden.

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Ändern Sie ein Projekt .NET zur Verwendung von Windows-Runtime-APIs

Öffnen Sie das **Verweis-Manager**-Dialogfeld, wählen Sie die **Durchsuchen**-Schaltfläche, und wählen Sie dann **Alle Dateien** aus.

![Dialogfeld „Verweis hinzufügen“](images/desktop-to-uwp/browse-references.png)

Fügen Sie dann einen Verweis auf diese Dateien hinzu.

|Datei|Pfad|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|System.Runtime.WindowsRuntime.UI.Xaml|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|System.Runtime.InteropServices.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Programme\Microsoft Dateien (x86) \Windows Kits\10\References\<*Sdk-Version*> \Windows.Foundation.UniversalApiContract\<*Version*>|
|Windows.Foundation.FoundationContract.winmd|C:\Programme\Microsoft Dateien (x86) \Windows Kits\10\References\<*Sdk-Version*> \Windows.Foundation.FoundationContract\<*Version*>|

Legen Sie im Dialogfeld **Eigenschaften** die **lokale Kopie** jeder *winmd*-Datei auf **Falsch** fest.

![„Lokal kopieren“-Feld](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-project-to-use-windows-runtime-apis"></a>Ändern Sie ein C++-Projekt zur Verwendung von Windows-Runtime-APIs

Verwendung [C++ / WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/) Windows-Runtime-APIs nutzen. C++/WinRT ist eine vollständig standardisierte, moderne C++17 Sprachprojektion für Windows-Runtime-(WinRT)-APIs, die als headerdateibasierte Bibliothek implementiert ist und Ihnen einen erstklassigen Zugriff auf die moderne Windows-API bietet.

Zum Konfigurieren des Projekts für C++ / WinRT, finden Sie unter [ändern Sie ein Windows-Desktop-Anwendungsprojekt zum Hinzufügen von C++ / WinRT-Unterstützung](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

## <a name="add-windows-10-experiences"></a>Windows 10-Funktionen hinzufügen

Jetzt können Sie moderner Funktionen für Benutzer der Anwendung unter Windows 10 Hinzufügen. Verwenden Sie diesen Designfluss.

:white_check_mark: **Entscheiden Sie zuerst, welche Erfahrungen, die Sie hinzufügen möchten**

Es gibt viele zur Auswahl. Beispielsweise können Sie den Purchase Order Flow vereinfachen, mit monetarisierung APIs oder direkte Aufmerksamkeit für Ihre Anwendung, wenn man etwas Interessantes, z. B. ein neues Bild freigeben, die eines anderen Benutzers gepostet hat.

![Toast](images/desktop-to-uwp/toast.png)

Auch dann, wenn Benutzer Ihre Nachricht ignorieren oder schließen, können sie diese im Info-Center anzeigen und dann auf die Nachricht klicken, um Ihre App zu öffnen. Dies erhöht die Kundeninteraktion mit Ihrer Anwendung und hat den zusätzlichen Vorteil, von dem Sie die Anwendung vollständig in das Betriebssystem integriert werden. Wir zeigen Ihnen den Code für diese Funktion weiter unten.

Besuchen Sie unsere [Developer Center](https://developer.microsoft.com/windows) mit weiteren Ideen.

:white_check_mark: **Entscheiden Sie, ob verbessern oder erweitern**

Wir verwenden häufig die Begriffe „verbessern“ und „Erweitern“. Daher möchten wir erklären, was diese Begriffe bedeuten.

Wir verwenden den Begriff "erhöhen" Windows-Runtime-APIs zu beschreiben, die Sie direkt über die desktop-Anwendung aufrufen können. Wenn Sie eine Windows 10-Funktion ausgewählt haben, identifizieren Sie die APIs, die Sie benötigen, um sie zu erstellen. Überprüfen Sie dann, ob die API in dieser [Liste](desktop-to-uwp-supported-api.md) aufgeführt wird. Hierbei handelt es sich um eine Liste der APIs, die Sie direkt aus Ihrer Desktopanwendung aufrufen können. Wenn Ihre API nicht in dieser Liste angezeigt wird, kann die Funktionalität der zugeordneten API nur in einem UWP-Prozess ausgeführt werden. Häufig sind dies APIs, die moderne Benutzeroberflächenelemente wie z. B. ein UWP-Karten-Steuerelement oder ein Windows Hello-Anmeldung nutzen.

Dies bedeutet, wenn Sie diese Funktionen in der Anwendung nutzen möchten, „erweitern“ sie einfach die Anwendung, indem Sie ein UWP-Projekt zur Projektmappe hinzufügen. Das Desktop-Projekt ist immer noch den Einstiegspunkt der Anwendung, aber das UWP-Projekt ermöglicht den Zugriff auf alle APIs, die nicht in dieser [Liste](desktop-to-uwp-supported-api.md) aufgeführt sind. Der Desktopanwendung kann mithilfe eines App-Dienstes mit dem UWP-Prozess kommunizieren. Wir stellen zahlreiche Anleitungen zum Einrichten dieser Kommunikation bereit. Wenn Sie eine Umgebung hinzufügen möchten, die ein UWP-Projekt erfordert finden Sie unter [Erweitern mit UWP](desktop-to-uwp-extend.md) weitere Informationen.

:white_check_mark: **Referenz zur API-Verträgen**

Wenn Sie die API direkt aus Ihrer Desktopanwendung aufrufen können, öffnen Sie einen Browser und suchen Sie nach dem Referenzthema für diese API.
Unterhalb der Zusammenfassung der API finden Sie eine Tabelle, die die API-Vertrag für diese API beschreibt. Dies ist ein Beispiel.

![API-Vertrag-Tabelle](images/desktop-to-uwp/contract-table.png)

Wenn Sie eine .NET-basierte Desktop-App haben, fügen Sie einen Verweis auf den API-Vertrag hinzu und legen Sie die **Lokale Kopie** Eigenschaft dieser Datei auf **Falsch** fest. Wenn Sie ein C++-basiertes Projekt haben, fügen Sie **Zusätzliche Include-Verzeichnisse** einen Pfad zu dem Ordner hinzu, der diesen Vertrag enthält.

:white_check_mark: **Aufrufen der APIs aus, um Ihre Umgebung hinzufügen**

Dies ist der Code, den Sie verwenden würden, um dem Benachrichtigungsfenster anzuzeigen, dass wir weiter oben behandelt haben. Diese APIs werden in dieser [Liste](desktop-to-uwp-supported-api.md) aufgeführt. Sie können diesen Code sofort in Ihrer Desktopanwendung nutzen.

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

Sie können Ihre Anwendung für Windows 10 modernisieren, ohne einen neuen Branch erstellen und Verwalten von unterschiedlichen Codebasen.

Wenn Sie separate Binärdateien für Windows 10-Benutzer erstellen möchten, verwenden Sie die bedingten Kompilierung. Wenn Sie einen Satz von Binärdateien für alle Windows-Benutzer erstellen möchten, verwenden Sie Laufzeitprüfungen.

Schauen wir uns die Optionen an.

### <a name="conditional-compilation"></a>Bedingte Kompilierung

Sie können eine Codebasis beibehalten und einen Satz Binärdateien nur für Windows 10-Benutzer kompilieren.

Fügen Sie Ihrem Projekt zuerst eine neue Buildkonfiguration hinzu.

![Buildkonfiguration](images/desktop-to-uwp/build-config.png)

Erstellen Sie für diese Konfiguration erstellen eine Konstante, die zur Identifizierung von Code, der Windows-Runtime-APIs aufruft.  

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

Sie können einen Satz von Binärdateien für alle Windows-Benutzer kompilieren, unabhängig davon, welche Version von Windows sie ausführen. Die Anwendung ruft Windows-Runtime-APIs nur dann, wenn der Benutzer ausgeführt wird, ist Ihre Anwendung wie eine gepackte Anwendung unter Windows 10.

Die einfachste Möglichkeit zum Überprüfungen zur Laufzeit zu Ihrem Code hinzufügen, wird dieses Nuget-Paket installieren: [Desktop-Brücke Hilfsprogramme](https://www.nuget.org/packages/DesktopBridge.Helpers/) und verwenden Sie dann die ``IsRunningAsUWP()`` Methode, um Gate alle Code, der Windows-Runtime-APIs aufruft. Lesen Sie diesen Blogbeitrag für Weitere Informationen: [Desktop-Brücke – Identifizieren der Anwendungskontext](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/).

## <a name="related-video"></a>Verwandte Videos

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Use-UWP-APIs-in-Your-Code-3d78c6WhD_9506218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="related-samples"></a>Verwandte Beispiele

* [Hello World-Beispiel](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [Sekundäre Kachel](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Store-API-Beispiel](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [WinForms-Anwendung, die eine UWP-UpdateTask implementiert](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [Desktop-app-Verbindung und UWP-Beispielen](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)


## <a name="support-and-feedback"></a>Support und Feedback

**Hier finden Sie Antworten auf Ihre Fragen**

Haben Sie Fragen? Fragen Sie uns auf Stack Overflow. Unser Team überwacht diese [Tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Fragen Sie uns [hier](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Geben Sie Feedback oder Vorschläge für Features**

Weitere Informationen finden Sie unter [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
