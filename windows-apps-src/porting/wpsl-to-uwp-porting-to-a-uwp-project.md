---
description: Beginnen Sie den portiervorgang durch Erstellen eines neuen Windows 10-Projekts in Visual Studio, und kopieren Ihre Dateien hinein.
title: Portieren von Windows Phone Silverlight-Projekten auf UWP-Projekte
ms.assetid: d86c99c5-eb13-4e37-b000-6a657543d8f4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: fe07e19a66bb7fc85a69a3bd542894e0b22aba9b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634185"
---
# <a name="porting-windowsphone-silverlight-projects-to-uwp-projects"></a>Portieren von Windows Phone Silverlight-Projekten auf UWP-Projekte


Das vorherige Thema war [Namespace- und Klassenzuordnungen](wpsl-to-uwp-namespace-and-class-mappings.md).

Beginnen Sie den portiervorgang durch Erstellen eines neuen Windows 10-Projekts in Visual Studio, und kopieren Ihre Dateien hinein.

## <a name="create-the-project-and-copy-files-to-it"></a>Erstellen des Projekts und Kopieren der Dateien

1.  Starten Sie Microsoft Visual Studio 2015, und Erstellen eines neuen Projekts für die leere Anwendung (Windows universell). Weitere Informationen finden Sie unter [beschleunigen Sie Ihre Windows-Runtime 8.x-app mithilfe von Vorlagen (C#, C++, Visual Basic)](https://msdn.microsoft.com/library/windows/apps/hh768232). Für das neue Projekt wird ein App-Paket (APPX-Datei) erstellt, das auf allen Gerätefamilien ausgeführt werden kann.
2.  Identifizieren Sie in Ihrer Windows Phone Silverlight-app-Projekt alle Quellcodedateien und visueller-Dateien, die Sie wiederverwenden möchten. Kopieren Sie mithilfe des Explorers die Datenmodelle, Ansichtsmodelle, visuellen Ressourcen, Ressourcenverzeichnisse sowie die Ordnerstruktur und alles andere, was Sie wiederverwenden möchten, in das neue Projekt. Kopieren oder erstellen Sie bei Bedarf Unterordner auf dem Datenträger.
3.  Kopieren Sie auch Ansichten (z. B. „MainPage.xaml“ und „MainPage.xaml.cs“) in den neuen Projektknoten. Erstellen Sie bei Bedarf auch hier neue Unterordner, und entfernen Sie die vorhandenen Ansichten aus dem Projekt. Erstellen Sie vor dem Überschreiben oder Entfernen einer von Visual Studio generierten Ansicht jedoch eine Kopie, um später darauf zurückgreifen zu können. Die erste Phase der Portieren einer Windows Phone Silverlight-app konzentriert sich auf die sich das spätere Aussehen und Funktion auf eine Gerätefamilie abrufen. Später können Sie sich dann auf eine einwandfreie Anpassung der Ansichten an alle Formfaktoren konzentrieren und bei Bedarf adaptiven Code für die optimale Nutzung einer bestimmten Gerätefamilie hinzufügen.
4.  Vergewissern Sie sich im **Projektmappen-Explorer**, dass **Alle Dateien anzeigen** aktiviert ist. Wählen Sie die kopierten Dateien aus, klicken Sie mit der rechten Maustaste darauf, und klicken Sie dann auf **Zu Projekt hinzufügen**. Dadurch werden die zugehörigen Ordner automatisch hinzugefügt. Sie können **Alle Dateien anzeigen** jetzt ggf. deaktivieren. Eine alternative Vorgehensweise ist die Verwendung des Befehls **Vorhandenes Element hinzufügen**, wenn Sie alle erforderlichen Unterordner im **Projektmappen-Explorer** von Visual Studio erstellt haben. Stellen Sie sicher, dass für Ihre visuellen Ressourcen **Buildvorgang** auf **Inhalt** und **In Ausgabeverzeichnis kopieren** auf **Nicht kopieren** festgelegt ist.
5.  In dieser Phase werden wegen der Unterschiede bei den Namespace- und Klassennamen viele Buildfehler generiert. Wenn Sie die von Visual Studio generierten Ansichten öffnen, sehen Sie beispielsweise, dass sie vom Typ [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) sind und nicht **PhoneApplicationPage**. Es gibt viele Unterschiede beim XAML-Markup und imperativen Code, die in den folgenden Themen dieses Handbuchs für das Portieren detailliert beschrieben werden. Wenn Sie die folgenden allgemeinen Schritte befolgen, werden Sie jedoch schnell Fortschritte machen: Ändern Sie in Ihren Namespacepräfixdeklarationen im XAML-Markup „clr-namespace“ in „using“. Nehmen Sie entsprechend den Angaben im Thema [Namespace- und Klassenzuordnungen](wpsl-to-uwp-namespace-and-class-mappings.md) mit dem Visual Studio-Befehl **Suchen und ersetzen** Massenänderungen an Ihrem Quellcode vor (ersetzen Sie z. B. „System.Windows“ durch „Windows.UI.Xaml“). Verwenden Sie im Editor für imperativen Code von Visual Studio die Befehle **Auflösen** und **using-Direktiven organisieren** im Kontextmenü, um gezieltere Änderungen vorzunehmen.

## <a name="extension-sdks"></a>Erweiterungs-SDKs

Die meisten UWP (Universelle Windows-Plattform)-APIs, die von Ihrer portierten App aufgerufen werden, sind in der Gruppe der APIs implementiert, die als Familie der universellen Geräte bezeichnet wird. Einige sind jedoch in Erweiterungs-SDKs implementiert, und Visual Studio erkennt nur APIs, die von der Zielgerätefamilie Ihrer App oder von einem Erweiterungs-SDK implementiert werden, auf das Sie verwiesen haben.

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

Falls Ihre App nicht auf die Gerätefamilie ausgerichtet ist, die die API implementiert, müssen Sie vor dem Aufrufen der API mithilfe der [**ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001)-Klasse ermitteln, ob die API vorhanden ist. (Dies wird als adaptiver Code bezeichnet.) Diese Bedingung wird unabhängig davon ausgewertet, wo Ihre App ausgeführt wird. Das Ergebnis ist aber nur dann „True“, wenn sie auf Geräten ausgeführt wird, bei denen die API vorhanden ist und aufgerufen werden kann. Verwenden Sie Erweiterungs-SDKs und adaptiven Code erst, nachdem Sie geprüft haben, ob eine universelle API vorhanden ist. Beispiele finden Sie im nächsten Abschnitt.

Weitere Informationen finden Sie auch unter [App-Paketmanifest](#the-app-package-manifest).

## <a name="maximizing-markup-and-code-reuse"></a>Maximieren der Wiederverwendung von Markup und Code

Sie werden feststellen, dass Sie durch kleinere Umgestaltungsmaßnahmen und/oder zusätzlichen adaptiven Code (siehe Erläuterung weiter unten) das Markup und den Code für alle Gerätefamilien maximieren können. Weitere Details:

-   Dateien, die für alle Gerätefamilien verwendet werden, bedürfen keiner besonderen Beachtung. Diese Dateien werden von der App für alle Gerätefamilien verwendet, mit denen sie ausgeführt wird. Dazu zählen XAML-Markupdateien, imperative Quellcodedateien und Ressourcendateien.
-   Ihre App kann die Gerätefamilie erkennen, mit der sie ausgeführt wird, und eine Ansicht verwenden, die speziell für diese Gerätefamilie gestaltet wurde. Weitere Informationen finden Sie unter [Erkennen der Plattform, auf der Ihre App ausgeführt wird](wpsl-to-uwp-input-and-sensors.md).
-   Steht keine Alternative zur Verfügung, ist unter Umständen folgende ähnliche Methode hilfreich: Versehen Sie eine Markupdatei oder eine **ResourceDictionary**-Datei (oder den Ordner, der die Datei enthält) mit einem speziellen Namen, sodass sie automatisch zur Laufzeit nur geladen wird, wenn Ihre App mit einer bestimmten Gerätefamilie ausgeführt wird. Diese Methode wird in der Fallstudie [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md) veranschaulicht.
-   Für Features, die nicht bei allen Gerätefamilien zur Verfügung stehen (wie etwa Drucker, Scanner oder die Kamerataste), können Sie adaptiven Code schreiben. Weitere Informationen finden Sie im dritten Beispiel dieses Themas unter [Bedingte Kompilierung und adaptiver Code](#conditional-compilation-and-adaptive-code).
-   Wenn Sie sowohl Windows Phone Silverlight und Windows 10 unterstützen möchten, klicken Sie dann zum Freigeben von Quellcode-Dateien zwischen Projekten möglicherweise. Klicken Sie dazu in Visual Studio im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, wählen Sie **Vorhandenes Element hinzufügen** und dann die freizugebenden Dateien aus, und klicken Sie auf **Als Link hinzufügen**. Speichern Sie die Quellcodedateien in einem gemeinsamen Ordner im Dateisystem, in dem sie für verknüpfte Projekte sichtbar sind, und vergessen Sie nicht, sie der Quellcodeverwaltung hinzuzufügen. Wenn Sie Ihren imperativen Quellcode so unterteilen können, dass der Großteil einer Datei oder die ganze Datei auf beiden Plattformen funktioniert, benötigen Sie nicht zwei Kopien. Sie können plattformspezifische Logik in der Datei bei Bedarf mit bedingten Kompilierungsdirektiven oder Laufzeitbedingungen umschließen. Weitere Informationen finden Sie im nächsten Abschnitt [C#-Präprozessordirektiven](https://msdn.microsoft.com/library/ed8yd1ha.aspx).
-   Für die Wiederverwendung auf Binärebene, statt auf Quellcodeebene stehen Ihnen Portable Class Libraries, die die Teilmenge von .NET APIs, die in Windows Phone Silverlight verfügbar sind, sowie die Teilmenge für Windows 10-apps ((.NET Core) zu unterstützen. Portable Klassenbibliothekassemblys sind mit allen diesen .NET-Plattformen sowie weiteren Plattformen binärkompatibel. Erstellen Sie mit Visual Studio ein Projekt für eine portable Klassenbibliothek. Weitere Informationen finden Sie unter [Plattformübergreifende Entwicklung mit der portablen Klassenbibliothek](https://msdn.microsoft.com/library/gg597391.aspx).

## <a name="conditional-compilation-and-adaptive-code"></a>Bedingte Kompilierung und adaptiver Code

Wenn Sie sowohl Windows Phone Silverlight und Windows 10 in einer einzigen Codedatei zu unterstützen, dann können Sie dies tun möchten. Wenn Sie in Ihrem Windows 10-Projekt auf den Eigenschaftenseiten des Projekts ansehen, sehen Sie, dass das Projekt WINDOWS definiert\_UAP als Symbol für bedingte Kompilierung. Im Allgemeinen können Sie zum Durchführen der bedingten Kompilierung die folgende Logik verwenden.

```csharp
#if WINDOWS_UAP
    // Code that you want to compile into the Windows 10 app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // WINDOWS_UAP
```

Wenn Sie über Code, die Sie zwischen einer Windows Phone Silverlight-app und eine Windows-Runtime 8.x-app mit Teilen verfügen Ihnen, müssen Sie bereits Quellcode mit Logik wie folgt:

```csharp
#if NETFX_CORE
    // Code that you want to compile into the Windows Runtime 8.x app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // NETFX_CORE
```

Wenn dies der Fall ist, und jetzt möchten Sie darüber hinaus unterstützt der Windows 10, können Sie die zu tun.

```csharp
#if WINDOWS_UAP
    // Code that you want to compile into the Windows 10 app.
#else
#if NETFX_CORE
    // Code that you want to compile into the Windows Runtime 8.x app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // NETFX_CORE
#endif // WINDOWS_UAP
```

Unter Umständen haben Sie die bedingte Kompilierung verwendet, um die Behandlung der Hardwaretaste „Zurück“ auf Windows Phone zu begrenzen. In Windows 10 ist das Ereignis der Schaltfläche "zurück" ein universal Konzept. In die Hardware oder Software implementierte „Zurück“-Schaltflächen lösen das [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596)-Ereignis aus. Daher müssen wir dieses Ereignis behandeln.

```csharp
       Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested +=
            this.ViewModelLocator_BackRequested;

...

    private void ViewModelLocator_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        // Handle the event.
    }

```

Unter Umständen haben Sie die bedingte Kompilierung verwendet, um die Behandlung der Hardwaretaste „Kamera“ auf Windows Phone zu begrenzen. In Windows 10 ist die Hardwaretaste "Kamera" ein Konzept, die speziell für mobile Geräte. Da auf allen Geräten ein einzelnes App-Paket ausgeführt wird, ändern wir unsere Kompilierzeitbedingung mithilfe von so genanntem adaptivem Code in eine Laufzeitbedingung. Hierzu fragen wir zur Laufzeit mithilfe der [**ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001)-Klasse ab, ob die [**HardwareButtons**](https://msdn.microsoft.com/library/windows/apps/jj207557)-Klasse vorhanden ist. **HardwareButtons** ist im mobilen Erweiterungs-SDK definiert. Daher müssen wir unserem Projekt einen Verweis auf dieses SDK hinzufügen, um den Code kompilieren zu können. Beachten Sie jedoch, dass der Handler nur auf einem Gerät ausgeführt wird, das die im mobilen Erweiterungs-SDK definierten Typen implementiert und somit der Familie mobiler Geräte angehört. Im folgenden Code wird also sorgfältig darauf geachtet, dass nur die vorhandenen Features verwendet werden, auch wenn dies auf andere Art als bei der bedingten Kompilierung erreicht wird.

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

Weitere Informationen finden Sie unter [Erkennen der Plattform, auf der Ihre App ausgeführt wird](wpsl-to-uwp-input-and-sensors.md).

## <a name="the-app-package-manifest"></a>Das App-Paketmanifest

Mit den Einstellungen in Ihrem Projekt (einschließlich der Verweise auf Erweiterungs-SDKs) wird der API-Oberflächenbereich bestimmt, der von Ihrer App aufgerufen werden kann. Aber anhand Ihres App-Paketmanifests wird die eigentliche Gruppe der Geräte bestimmt, auf denen Kunden Ihre App aus dem Store installieren können. Weitere Informationen finden Sie unter „Beispiele“ in [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903).

Kenntnisse in der Bearbeitung des App-Paketmanifests sind von Vorteil, da es in den folgenden Themen um seine Verwendung für verschiedene Deklarationen, Funktionen und andere Einstellungen geht, die für einige Features erforderlich sind. Sie können das Manifest mit dem App-Paketmanifest-Editor von Visual Studio bearbeiten. Falls der **Projektmappen-Explorer** nicht angezeigt wird, wählen Sie ihn im Menü **Ansicht** aus. Doppelklicken Sie auf **Package.appxmanifest**. Dadurch wird das Fenster des Manifest-Editors geöffnet. Wählen Sie die geeignete Registerkarte aus, nehmen Sie Änderungen vor, und speichern Sie sie. Es empfiehlt sich sicherzustellen, dass das **pm:PhoneIdentity**-Element im portierten App-Manifest dem Element im Manifest der App entspricht, die Sie portieren (ausführliche Informationen finden Sie im Thema [**pm:PhoneIdentity**](https://msdn.microsoft.com/library/windows/apps/dn934763)).

Finden Sie unter [Paketmanifest-Schemareferenz für Windows 10](https://msdn.microsoft.com/library/windows/apps/dn934820).

Das nächste Thema ist [Problembehandlung](wpsl-to-uwp-troubleshooting.md).

