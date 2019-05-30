---
description: Sie haben zwei Möglichkeiten, wenn Sie mit dem Portierungsprozess beginnen.
title: Portieren eines Windows-Runtime 8.x-Projekts zu einem UWP-Projekt
ms.assetid: 2dee149f-d81e-45e0-99a4-209a178d415a
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: fac31b773c2326f8a9e0ed50cdf876552fee5689
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372254"
---
# <a name="porting-a-windows-runtime-8x-project-to-a-uwp-project"></a>Portieren eines Windows-Runtime 8.x-Projekts zu einem UWP-Projekt



Sie haben zwei Möglichkeiten, wenn Sie mit dem Portierungsprozess beginnen. Eine Möglichkeit ist das Bearbeiten einer Kopie der vorhandenen Projektdateien, einschließlich des App-Paketmanifests (die Vorgehensweise finden Sie in den Informationen zum Aktualisieren der Projektdateien unter [Migrieren von Apps zur universellen Windows-Plattform (UWP)](https://docs.microsoft.com/visualstudio/misc/migrate-apps-to-the-universal-windows-platform-uwp?view=vs-2015)). Die andere Option besteht darin erstellen ein neues Windows 10-Projekt in Visual Studio, und kopieren Ihre Dateien hinein. Im ersten Abschnitt dieses Themas wird diese zweite Möglichkeit beschrieben, aber im restlichen Thema finden Sie zusätzliche Informationen für beide Vorgehensweisen. Sie können auch auswählen, um das neue Windows 10-Projekt in der gleichen Projektmappe wie Ihre vorhandenen Projekte beibehalten und Freigabe von Quellcodedateien, die mit einem freigegebenen Projekt. Alternativ können Sie das neue Projekt in einer eigenen Projektmappe platzieren und Quellcodedateien mithilfe des Features für verknüpfte Dateien in Visual Studio freigeben.

## <a name="create-the-project-and-copy-files-to-it"></a>Erstellen des Projekts und Kopieren der Dateien

Diese Schritte konzentrieren sich auf die Option zum Erstellen eines neuen Windows 10-Projekts in Visual Studio, und kopieren Ihre Dateien hinein. Einige der Details (etwa, wie viele Projekte Sie erstellen und welche Dateien Sie kopieren) sind abhängig von den Faktoren und Entscheidungen, die unter [Bei einer universellen 8.1-App](w8x-to-uwp-root.md) und in den anschließenden Abschnitten beschrieben sind. Bei den folgenden Schritten wird vom einfachsten Fall ausgegangen.

1.  Starten Sie Microsoft Visual Studio 2015, und Erstellen eines neuen Projekts für die leere Anwendung (Windows universell). Weitere Informationen finden Sie unter [beschleunigen Sie Ihre Windows-Runtime 8.x-app mithilfe von Vorlagen (C#, C++, Visual Basic)](https://docs.microsoft.com/previous-versions/windows/apps/hh768232(v=win.10)). Für das neue Projekt wird ein App-Paket (APPX-Datei) erstellt, das auf allen Gerätefamilien ausgeführt werden kann.
2.  Geben Sie in Ihrem universellen 8.1-App-Projekt alle Quellcodedateien und visuellen Ressourcendateien an, die Sie wiederverwenden möchten. Kopieren Sie mithilfe des Explorers die Datenmodelle, Ansichtsmodelle, visuellen Ressourcen, Ressourcenverzeichnisse sowie die Ordnerstruktur und alles andere, was Sie wiederverwenden möchten, in das neue Projekt. Kopieren oder erstellen Sie bei Bedarf Unterordner auf dem Datenträger.
3.  Kopieren Sie auch Ansichten (z. B. „MainPage.xaml“ und „MainPage.xaml.cs“) in das neue Projekt. Erstellen Sie bei Bedarf auch hier neue Unterordner, und entfernen Sie die vorhandenen Ansichten aus dem Projekt. Erstellen Sie vor dem Überschreiben oder Entfernen einer von Visual Studio generierten Ansicht jedoch eine Kopie, um später darauf zurückgreifen zu können. In der ersten Portierungsphase einer universellen 8.1-App wird in erster Linie dafür gesorgt, dass die App auf einer Gerätefamilie gut aussieht und reibungslos funktioniert. Später können Sie sich dann auf eine einwandfreie Anpassung der Ansichten an alle Formfaktoren konzentrieren und bei Bedarf adaptiven Code für die optimale Nutzung einer bestimmten Gerätefamilie hinzufügen.
4.  Vergewissern Sie sich im **Projektmappen-Explorer**, dass **Alle Dateien anzeigen** aktiviert ist. Wählen Sie die kopierten Dateien aus, klicken Sie mit der rechten Maustaste darauf, und klicken Sie dann auf **Zu Projekt hinzufügen**. Dadurch werden die zugehörigen Ordner automatisch hinzugefügt. Sie können **Alle Dateien anzeigen** jetzt ggf. deaktivieren. Eine alternative Vorgehensweise ist die Verwendung des Befehls **Vorhandenes Element hinzufügen**, wenn Sie alle erforderlichen Unterordner im **Projektmappen-Explorer** von Visual Studio erstellt haben. Stellen Sie sicher, dass für Ihre visuellen Ressourcen **Buildvorgang** auf **Inhalt** und **In Ausgabeverzeichnis kopieren** auf **Nicht kopieren** festgelegt ist.
5.  In dieser Phase begegnen Ihnen wahrscheinlich noch einige Buildfehler. Wenn Sie wissen, was Sie ändern müssen, können Sie mithilfe des Visual Studio-Befehls **Suchen und ersetzen** globale Änderungen in Ihrem Quellcode vornehmen. Gezieltere Änderungen können Sie im imperativen Code-Editor von Visual Studio mit den Befehlen **Auflösen** und **Using-Direktiven organisieren** aus dem Kontextmenü durchführen.

## <a name="maximizing-markup-and-code-reuse"></a>Maximieren der Wiederverwendung von Markup und Code

Sie werden feststellen, dass Sie durch kleinere Umgestaltungsmaßnahmen und/oder zusätzlichen adaptiven Code (siehe Erläuterung weiter unten) das Markup und den Code für alle Gerätefamilien maximieren können. Weitere Details:

-   Dateien, die für alle Gerätefamilien verwendet werden, bedürfen keiner besonderen Beachtung. Diese Dateien werden von der App für alle Gerätefamilien verwendet, mit denen sie ausgeführt wird. Dazu zählen XAML-Markupdateien, imperative Quellcodedateien und Ressourcendateien.
-   Ihre App kann die Gerätefamilie erkennen, mit der sie ausgeführt wird, und eine Ansicht verwenden, die speziell für diese Gerätefamilie gestaltet wurde. Weitere Informationen finden Sie unter [Erkennen der Plattform, auf der Ihre App ausgeführt wird](w8x-to-uwp-input-and-sensors.md).
-   Steht keine Alternative zur Verfügung, ist unter Umständen folgende ähnliche Methode hilfreich: Versehen Sie eine Markupdatei oder eine **ResourceDictionary**-Datei (oder den Ordner, der die Datei enthält) mit einem speziellen Namen, sodass sie zur Laufzeit nur dann automatisch geladen wird, wenn Ihre App mit einer bestimmten Gerätefamilie ausgeführt wird. Diese Methode wird in der Fallstudie [Bookstore1](w8x-to-uwp-case-study-bookstore1.md) veranschaulicht.
-   Sie sollten in der Lage, viele der Anweisungen, die für die bedingte Kompilierung in Ihrer Universal 8.1-app-Quellcode zu entfernen, wenn nur Windows 10 unterstützt werden müssen. Weitere Informationen finden Sie in diesem Thema unter [Bedingte Kompilierung und adaptiver Code](#conditional-compilation-and-adaptive-code).
-   Für Features, die nicht bei allen Gerätefamilien zur Verfügung stehen (wie etwa Drucker, Scanner oder die Kamerataste), können Sie adaptiven Code schreiben. Weitere Informationen finden Sie im dritten Beispiel dieses Themas unter [Bedingte Kompilierung und adaptiver Code](#conditional-compilation-and-adaptive-code).
-   Wenn Sie Windows 8.1, Windows Phone 8.1 und Windows 10 unterstützen möchten, können Sie drei Projekte in der gleichen Projektmappe beibehalten und Freigeben von Code für ein freigegebenes Projekt. Alternativ können Sie Quellcodedateien projektübergreifend freigeben. Klicken Sie dazu in Visual Studio im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, wählen Sie **Vorhandenes Element hinzufügen** und dann die freizugebenden Dateien aus, und klicken Sie auf **Als Link hinzufügen**. Speichern Sie die Quellcodedateien in einem gemeinsamen Ordner im Dateisystem, in dem sie für verknüpfte Projekte sichtbar sind. Und vergessen Sie nicht, die Dateien der Quellcodeverwaltung hinzuzufügen.
-   Informationen zur Wiederverwendung auf binärer Ebene finden Sie unter [Erstellen von Komponenten für Windows-Runtime in C# und Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br230301(v=vs.140)). Es gibt auch Portable Class Libraries, die die Teilmenge von .NET APIs zu unterstützen, die in .NET Framework für Windows 8.1, Windows Phone 8.1 und Windows 10-apps ((.NET Core) und das vollständige .NET Framework verfügbar sind. Portable Klassenbibliothekassemblys sind mit allen diesen Plattformen binärkompatibel. Erstellen Sie mit Visual Studio ein Projekt für eine portable Klassenbibliothek. Weitere Informationen finden Sie unter [Plattformübergreifende Entwicklung mit der portablen Klassenbibliothek](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library).

## <a name="extension-sdks"></a>Erweiterungs-SDKs

Die meisten der Windows-Runtime-APIs, die Ihre universelle 8.1-App bereits aufruft, sind in der Gruppe der APIs implementiert, die als universelle Gerätefamilie bezeichnet wird. Einige sind jedoch in Erweiterungs-SDKs implementiert, und Visual Studio erkennt nur APIs, die von der Zielgerätefamilie Ihrer App oder von einem Erweiterungs-SDK implementiert werden, auf das Sie verwiesen haben.

Im Fall von Kompilierungsfehlern, die auf nicht gefundene Namespaces, Typen oder Member zurückzuführen sind, ist dies wahrscheinlich die Ursache. Öffnen Sie in der API-Referenzdokumentation das entsprechende API-Thema, und navigieren Sie zum Abschnitt mit den Anforderungen. Hier erfahren Sie, von welcher Gerätefamilie die API implementiert wird. Handelt es sich dabei nicht um Ihre Zielgerätefamilie, benötigen Sie einen Verweis auf das Erweiterungs-SDK für diese Gerätefamilie, um die API für Ihr Projekt verfügbar zu machen.

Klicken Sie auf **Projekt** &gt; **Verweis hinzufügen** &gt; **Windows Universal** &gt; **Erweiterungen** und Wählen Sie das entsprechende Erweiterungs-SDK. Wenn die gewünschten APIs also beispielsweise nur in der Familie für mobile Geräte verfügbar sind und in Version 10.0.x.y eingeführt wurden, wählen Sie **Windows Mobile-Erweiterungen für die UWP** aus.

Dadurch wird Ihrer Projektdatei folgender Verweis hinzugefügt:

```XML
<ItemGroup>
    <SDKReference Include="WindowsMobile, Version=10.0.x.y">
        <Name>Windows Mobile Extensions for the UWP</Name>
    </SDKReference>
</ItemGroup>
```

Name und Versionsnummer entsprechen den Ordnern am Installationsort Ihres SDKs. Die oben angegebenen Informationen entsprechen beispielsweise dem folgenden Ordnernamen:

`\Program Files (x86)\Windows Kits\10\Extension SDKs\WindowsMobile\10.0.x.y`

Falls Ihre App nicht auf die Gerätefamilie ausgerichtet ist, die die API implementiert, müssen Sie vor dem Aufrufen der API mithilfe der [**ApiInformation**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Metadata.ApiInformation)-Klasse ermitteln, ob die API vorhanden ist. (Dies wird als adaptiver Code bezeichnet.) Diese Bedingung wird unabhängig davon ausgewertet, wo Ihre App ausgeführt wird. Das Ergebnis ist aber nur dann „True“, wenn sie auf Geräten ausgeführt wird, bei denen die API vorhanden ist und aufgerufen werden kann. Verwenden Sie Erweiterungs-SDKs und adaptiven Code erst, nachdem Sie geprüft haben, ob eine universelle API vorhanden ist. Beispiele finden Sie im nächsten Abschnitt.

Weitere Informationen finden Sie auch unter [App-Paketmanifest](#app-package-manifest).

## <a name="conditional-compilation-and-adaptive-code"></a>Bedingte Kompilierung und adaptiver Code

Wenn Sie für die bedingte Kompilierung nutzen (mit C# Präprozessordirektiven), damit Ihre Codedateien auf Windows 8.1 und Windows Phone 8.1 arbeiten, klicken Sie dann überprüfen Sie nun, für die bedingte Kompilierung angesichts der Konvergenz Arbeit unter Windows 10. Konvergenz bedeutet, dass in Ihrer Windows 10-app einige Bedingungen vollständig entfernt werden können. Bei anderen werden Laufzeitprüfungen verwendet, wie in den folgenden Beispielen veranschaulicht.

**Beachten Sie**    Wenn Sie Windows 8.1, Windows Phone 8.1 und Windows 10 in einer einzelnen Codedatei unterstützen möchten, können Sie dies auch tun. Wenn Sie in Ihrem Windows 10-Projekt auf den Eigenschaftenseiten des Projekts ansehen, sehen Sie, dass das Projekt WINDOWS definiert\_UAP als Symbol für bedingte Kompilierung. Daher können, die in Kombination mit WINDOWS\_-APP und WINDOWS\_PHONE\_APP. Diese Beispiele zeigen, den einfacheren Fall entfernen die bedingte Kompilierung aus einer Universal 8.1-app und den entsprechenden Code für eine Windows 10-app ersetzen.

Das erste Beispiel zeigt das Verwendungsmuster für die **PickSingleFileAsync**-API (gilt nur für Windows 8.1) und die **PickSingleFileAndContinue**-API (gilt nur für die Windows Phone 8.1).

```csharp
#if WINDOWS_APP
    // Use Windows.Storage.Pickers.FileOpenPicker.PickSingleFileAsync
#else
    // Use Windows.Storage.Pickers.FileOpenPicker.PickSingleFileAndContinue
#endif // WINDOWS_APP
```

Windows 10 zusammengeführt werden, auf die [ **PickSingleFileAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) -API, damit Ihr Code folgt vereinfacht:

```csharp
    // Use Windows.Storage.Pickers.FileOpenPicker.PickSingleFileAsync
```

In diesem Beispiel behandeln wir die Hardwaretaste „Zurück“ – allerdings nur für Windows Phone.

```csharp
#if WINDOWS_PHONE_APP
        Windows.Phone.UI.Input.HardwareButtons.BackPressed += this.HardwareButtons_BackPressed;
#endif // WINDOWS_PHONE_APP

...

#if WINDOWS_PHONE_APP
    void HardwareButtons_BackPressed(object sender, Windows.Phone.UI.Input.BackPressedEventArgs e)
    {
        // Handle the event.
    }
#endif // WINDOWS_PHONE_APP
```

In Windows 10 ist das Ereignis der Schaltfläche "zurück" ein universal Konzept. In die Hardware oder Software implementierte „Zurück“-Schaltflächen lösen das [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.backrequested)-Ereignis aus. Daher müssen wir dieses Ereignis behandeln.

```csharp
    Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested +=
        this.ViewModelLocator_BackRequested;

...

private void ViewModelLocator_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
{
    // Handle the event.
}
```

Dieses letzte Beispiel ähnelt dem vorherigen Beispiel. Hier behandeln wir die Hardwaretaste für die Kamera – allerdings wieder nur in dem Code, der zum Windows Phone-App-Paket kompiliert wird.

```csharp
#if WINDOWS_PHONE_APP
    Windows.Phone.UI.Input.HardwareButtons.CameraPressed += this.HardwareButtons_CameraPressed;
#endif // WINDOWS_PHONE_APP

...

#if WINDOWS_PHONE_APP
void HardwareButtons_CameraPressed(object sender, Windows.Phone.UI.Input.CameraEventArgs e)
{
    // Handle the event.
}
#endif // WINDOWS_PHONE_APP
```

In Windows 10 ist die Hardwaretaste "Kamera" ein Konzept, die speziell für mobile Geräte. Da auf allen Geräten ein einzelnes App-Paket ausgeführt wird, ändern wir unsere Kompilierzeitbedingung mithilfe von so genanntem adaptivem Code in eine Laufzeitbedingung. Hierzu fragen wir zur Laufzeit mithilfe der [**ApiInformation**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Metadata.ApiInformation)-Klasse ab, ob die [**HardwareButtons**](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons)-Klasse vorhanden ist. **HardwareButtons** ist im mobilen Erweiterungs-SDK definiert. Daher müssen wir unserem Projekt einen Verweis auf dieses SDK hinzufügen, um den Code kompilieren zu können. Beachten Sie jedoch, dass der Handler nur auf einem Gerät ausgeführt wird, das die im mobilen Erweiterungs-SDK definierten Typen implementiert und somit der Familie mobiler Geräte angehört. Dieser Code entspricht also grundsätzlich dem universellen 8.1-Code, da sichergestellt wird, dass nur Features verwendet werden, die tatsächlich vorhanden sind (auch wenn dies auf andere Weise bewerkstelligt wird).

```csharp
    // Note: Cache the value instead of querying it more than once.
    bool isHardwareButtonsAPIPresent = Windows.Foundation.Metadata.ApiInformation.IsTypePresent
        ("Windows.Phone.UI.Input.HardwareButtons");

    if (isHardwareButtonsAPIPresent)
    {
        Windows.Phone.UI.Input.HardwareButtons.CameraPressed +=
            this.HardwareButtons_CameraPressed;
    }

    ...

private void HardwareButtons_CameraPressed(object sender, Windows.Phone.UI.Input.CameraEventArgs e)
{
    // Handle the event.
}
```

Weitere Informationen finden Sie unter [Erkennen der Plattform, auf der Ihre App ausgeführt wird](w8x-to-uwp-input-and-sensors.md).

## <a name="app-package-manifest"></a>App-Paketmanifest

Die [Änderungen in Windows 10-](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/what-s-changed-in-windows-10) Thema listet Änderungen an der Paketmanifest-Schemareferenz für Windows 10, einschließlich der Elemente, die hinzugefügt, entfernt, geändert und wurden. Referenzinformationen zu allen Elementen, Attributen und Typen im Schema finden Sie unter [Elementhierarchie](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/root-elements). Wenn Sie eine Windows Phone Store-App portieren, oder wenn Ihre App ein Update für eine App aus dem Windows Phone Store ist, stellen Sie sicher, dass das Element **pm: phoneidentity** dem Inhalt des Anwendungsmanifests Ihrer vorherigen App entspricht (verwenden Sie dieselben GUIDs, die der App vom Store zugewiesen wurden). Dadurch wird sichergestellt, dass Benutzer Ihrer App, die eine Aktualisierung auf Windows 10 durchführen, Ihre neue App als Update und nicht als Duplikat erhalten. Weitere Einzelheiten finden Sie im Referenzthema [**pm:PhoneIdentity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-pm-phoneidentity).

Mit den Einstellungen in Ihrem Projekt (einschließlich der Verweise auf Erweiterungs-SDKs) wird der API-Oberflächenbereich bestimmt, der von Ihrer App aufgerufen werden kann. Aber anhand Ihres App-Paketmanifests wird die eigentliche Gruppe der Geräte bestimmt, auf denen Kunden Ihre App aus dem Store installieren können. Weitere Informationen finden Sie unter „Beispiele“ in [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily).

Sie können das App-Paketmanifest bearbeiten und verschiedene Deklarationen, Funktionen und andere Einstellungen festlegen, die für einige Features erforderlich sind. Sie können das Manifest mit dem App-Paketmanifest-Editor von Visual Studio bearbeiten. Falls der **Projektmappen-Explorer** nicht angezeigt wird, wählen Sie ihn im Menü **Ansicht** aus. Doppelklicken Sie auf **Package.appxmanifest**. Dadurch wird das Fenster des Manifest-Editors geöffnet. Klicken Sie auf die gewünschte Registerkarte, nehmen Sie Ihre Änderungen vor, und speichern Sie sie.

Das nächste Thema ist [Problembehandlung](w8x-to-uwp-troubleshooting.md).

## <a name="related-topics"></a>Verwandte Themen

* [Entwickeln von apps für die universelle Windows-Plattform](https://docs.microsoft.com/visualstudio/cross-platform/develop-apps-for-the-universal-windows-platform-uwp?view=vs-2015)
* [Beschleunigen Sie Ihre Windows-Runtime 8.x-app mithilfe von Vorlagen (C#, C++, Visual Basic)](https://docs.microsoft.com/previous-versions/windows/apps/hh768232(v=win.10))
* [Erstellen Windows-Runtime-Komponenten](https://docs.microsoft.com/previous-versions/windows/apps/hh441572(v=vs.140))
* [Plattformübergreifende Entwicklung mit der portablen Klassenbibliothek](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)

