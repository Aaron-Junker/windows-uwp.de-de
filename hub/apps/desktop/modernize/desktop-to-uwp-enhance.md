---
Description: Optimieren Sie die desktop-Anwendung für Windows 10-Benutzer mithilfe von universellen Windows-Plattform (UWP) APIs.
title: Verwenden von UWP-APIs in desktop-apps
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, UWP
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 4846a29e914ffed15e4c3dea938cc51cefd566e0
ms.sourcegitcommit: b9e2cd5232ad98f4ef367881b92000a3ae610844
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67131945"
---
# <a name="call-uwp-apis-in-desktop-apps"></a>Rufen Sie die UWP-APIs in desktop-apps

Universelle Windows-Plattform (UWP)-APIs können Sie moderne Funktionen Ihrer desktop-apps hinzufügen, die für Windows 10-Benutzern aufleuchten.

Legen Sie zuerst das Projekt mit der erforderlichen Verweise. Rufen Sie Sie dann die UWP-APIs aus dem Code hinzufügen, dass Windows 10 zu Ihrer desktop-app-Funktionen. Separat für Windows 10-Benutzer erstellen kann oder verteilen die gleichen Binärdateien für alle Benutzer unabhängig davon, welche, die Version von Windows, die sie ausgeführt werden.

Einige UWP-APIs werden nur in desktop-apps, die in gepackt werden unterstützt eine [MSIX Paket](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root). Weitere Informationen finden Sie unter [verfügbaren UWP-APIs](desktop-to-uwp-supported-api.md).

## <a name="set-up-your-project"></a>Einrichten des Projekts

Sie müssen einige Änderungen am Projekt vornehmen, um UWP-APIs zu verwenden.

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Ändern Sie ein Projekt .NET zur Verwendung von Windows-Runtime-APIs

1. Öffnen Sie das **Verweis-Manager**-Dialogfeld, wählen Sie die **Durchsuchen**-Schaltfläche, und wählen Sie dann **Alle Dateien** aus.

    ![Dialogfeld „Verweis hinzufügen“](images/desktop-to-uwp/browse-references.png)

2. Fügen Sie einen Verweis auf diese Dateien.

  |Datei|Speicherort|
  |--|--|
  |System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
  |System.Runtime.WindowsRuntime.UI.Xaml|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
  |System.Runtime.InteropServices.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
  |windows.winmd|C:\Programme\Microsoft Dateien (x86) \Windows Kits\10\UnionMetadata\\<*Sdk-Version*> \Facade|
  |Windows.Foundation.UniversalApiContract.winmd|C:\Programme\Microsoft Dateien (x86) \Windows Kits\10\References\\<*Sdk-Version*> \Windows.Foundation.UniversalApiContract\<*Version*>|
  |Windows.Foundation.FoundationContract.winmd|C:\Programme\Microsoft Dateien (x86) \Windows Kits\10\References\\<*Sdk-Version*> \Windows.Foundation.FoundationContract\<*Version*>|

3. Legen Sie im Dialogfeld **Eigenschaften** die **lokale Kopie** jeder *winmd*-Datei auf **Falsch** fest.

    ![„Lokal kopieren“-Feld](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-project-to-use-windows-runtime-apis"></a>Ändern Sie ein C++-Projekt zur Verwendung von Windows-Runtime-APIs

Verwendung [C++ / WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/) Windows-Runtime-APIs nutzen. C++/WinRT ist eine vollständig standardisierte, moderne C++17 Sprachprojektion für Windows-Runtime-(WinRT)-APIs, die als headerdateibasierte Bibliothek implementiert ist und Ihnen einen erstklassigen Zugriff auf die moderne Windows-API bietet.

Zum Konfigurieren des Projekts für C++ / WinRT, finden Sie unter [ändern Sie ein Windows-Desktop-Anwendungsprojekt zum Hinzufügen von C++ / WinRT-Unterstützung](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

## <a name="add-windows-10-experiences"></a>Windows 10-Funktionen hinzufügen

Jetzt können Sie moderner Funktionen für Benutzer der Anwendung unter Windows 10 Hinzufügen. Verwenden Sie diesen Designfluss.

:white_check_mark: **Entscheiden Sie zuerst, welche Erfahrungen, die Sie hinzufügen möchten**

Es gibt viele zur Auswahl. Sie können z. B. den Purchase Order Flow vereinfachen, indem Sie mithilfe von [monetarisierung APIs](/windows/uwp/monetize), oder [direkte Aufmerksamkeit für Ihre Anwendung](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts) bei etwas Interessantes freigeben, z. B. ein neues Bild, das ein anderer Benutzer wurde bereitgestellt.

![Toast](images/desktop-to-uwp/toast.png)

Auch dann, wenn Benutzer Ihre Nachricht ignorieren oder schließen, können sie diese im Info-Center anzeigen und dann auf die Nachricht klicken, um Ihre App zu öffnen. Dies erhöht die Kundeninteraktion mit Ihrer Anwendung und hat den zusätzlichen Vorteil, von dem Sie die Anwendung vollständig in das Betriebssystem integriert werden. Wir zeigen den Code für diese Umgebung etwas weiter unten in diesem Artikel Ihnen.

Besuchen Sie die [Dokumentation zur UWP](/windows/uwp/get-started/) Weitere Ideen.

:white_check_mark: **Entscheiden Sie, ob verbessern oder erweitern**

Hören Sie oft uns die Nutzungsbedingungen *verbessern* und *erweitern*, also wir einen Moment Zeit, um jedes dieser Mean Begriffe zu erläutern.

Verwenden wir den Begriff *verbessern* um Windows-Runtime-APIs zu beschreiben, die Sie direkt aus Ihrer desktop-app (unabhängig davon, ob Sie ausgewählt haben Packen Ihrer Anwendung in einem Paket MSIX) aufrufen können. Wenn Sie eine Windows 10-Umgebung ausgewählt haben, identifizieren Sie die APIs, die Sie verwenden möchten, erstellen Sie ihn, und prüfen, ob diese API wird, in angezeigt [dieser Liste](desktop-to-uwp-supported-api.md). Dies ist eine Liste der APIs, die Sie direkt aus Ihrer desktop-app aufrufen können. Wenn Ihre API nicht in dieser Liste angezeigt wird, kann die Funktionalität der zugeordneten API nur in einem UWP-Prozess ausgeführt werden. Oft dazu gehören APIs, die z. B. ein UWP-Map-Steuerelement oder ein Windows Hello-Sicherheitshinweis UWP XAML zu rendern.

> [!NOTE]
> Obgleich APIs, die in der Regel Rendern von UWP XAML nicht direkt von Ihrem Desktop aufgerufen werden kann, verwenden Sie alternative Ansätze möglicherweise. Wenn Sie die UWP XAML-Steuerelemente oder andere benutzerdefinierte visuelle Erlebnisse hosten möchten, können Sie [XAML-Inseln](xaml-islands.md) (ab Windows 10, Version 1903) und die [visueller Ebene](visual-layer-in-desktop-apps.md) (ab Windows 10, Version 1803). Diese Funktionen können in App-Pakete oder entpackt desktop-apps verwendet werden.

Wenn Sie zum Packen Ihrer desktop-app in einem Paket MSIX ausgewählt haben, eine andere Möglichkeit ist *erweitern* der Anwendung durch Hinzufügen eines UWP-Projekts Ihrer Projektmappe. Das Desktopprojekt ist immer noch den Einstiegspunkt der Anwendung, aber das UWP-Projekt bietet Ihnen Zugriff auf alle APIs, die nicht in erscheinen [dieser Liste](desktop-to-uwp-supported-api.md). Die desktop-app mit der UWP-Prozess kommunizieren kann, mithilfe einer app Service, und wir haben Sie viele Anleitungen Anleitungen zum einrichten. Wenn Sie eine Umgebung hinzufügen möchten, die ein UWP-Projekt erfordert, finden Sie unter [erweitern mit UWP-Komponente](desktop-to-uwp-extend.md).

:white_check_mark: **Referenz zur API-Verträgen**

Wenn Sie die API direkt aus Ihrer desktop-app aufrufen können, öffnen Sie einen Browser, und die Suche nach dem Referenzthema für die API.
Unterhalb der Zusammenfassung der API finden Sie eine Tabelle, die die API-Vertrag für diese API beschreibt. Dies ist ein Beispiel.

![API-Vertrag-Tabelle](images/desktop-to-uwp/contract-table.png)

Wenn Sie eine .NET-basierte Desktop-App haben, fügen Sie einen Verweis auf den API-Vertrag hinzu und legen Sie die **Lokale Kopie** Eigenschaft dieser Datei auf **Falsch** fest. Wenn Sie ein C++-basiertes Projekt haben, fügen Sie **Zusätzliche Include-Verzeichnisse** einen Pfad zu dem Ordner hinzu, der diesen Vertrag enthält.

:white_check_mark: **Aufrufen der APIs aus, um Ihre Umgebung hinzufügen**

Dies ist der Code, den Sie verwenden würden, um dem Benachrichtigungsfenster anzuzeigen, dass wir weiter oben behandelt haben. Diese APIs werden in diesem [Liste](desktop-to-uwp-supported-api.md) fügen Sie diesen Code zu Ihrer desktop-app und führen Sie es jetzt.

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
