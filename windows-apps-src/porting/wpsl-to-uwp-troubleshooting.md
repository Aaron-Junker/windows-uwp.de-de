---
description: Wir empfehlen dringend, dieses Handbuch für das Portieren vollständig zu lesen. Wir wissen aber auch, dass Sie möglichst schnell die Phase erreichen möchten, in der Ihr Projekt erstellt und ausgeführt wird.
title: Behandeln von Problemen beim Portieren von Windows Phone Silverlight zu UWP
ms.assetid: d9a9a2a7-9401-4990-a992-4b13887f2661
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a2c1353882ade36b5c1b82d0b75967d010ae0dc7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174824"
---
#  <a name="troubleshooting-porting-windowsphone-silverlight-to-uwp"></a>Behandeln von Problemen beim Portieren von Windows Phone Silverlight zu UWP


Im vorherigen Thema ging es um das [Portieren des Projekts](wpsl-to-uwp-porting-to-a-uwp-project.md).

Wir empfehlen dringend, dieses Handbuch für das Portieren vollständig zu lesen. Wir wissen aber auch, dass Sie möglichst schnell die Phase erreichen möchten, in der Ihr Projekt erstellt und ausgeführt wird. Zu diesem Zweck können Sie einen temporären Fortschritt erzielen, indem Sie allen nicht unbedingt erforderlichen Code auskommentieren und anschließend zurückkehren, um die Schulden später zu bezahlen. Die Tabelle mit Symptomen und Möglichkeiten zur Problembehandlung in diesem Thema kann in dieser Phase hilfreich für Sie sein, ersetzt jedoch nicht das Lesen der nächsten Themen. Sie können die Tabelle jederzeit zu Rate ziehen, während Sie die weiteren Themen lesen.

## <a name="tracking-down-issues"></a>Ermitteln von Problemen

XAML-Analyseausnahmen sind u U. schwierig zu diagnostizieren, insbesondere wenn keine sinnvollen Fehlermeldungen innerhalb der Ausnahme vorhanden sind. Stellen Sie sicher, dass der Debugger für die Erfassung von Ausnahmen (erste Chance) konfiguriert ist (um die Analyseausnahme möglichst früh zu erfassen). Möglicherweise können Sie die Ausnahmevariable im Debugger überprüfen, um zu ermitteln, ob das HRESULT oder die Meldung hilfreiche Informationen enthält. Überprüfen Sie auch das Visual Studio-Ausgabefenster auf Fehlermeldungen des XAML-Parsers.

Wenn Ihre App beendet wird und Sie nur wissen, dass während der XAML-Markupanalyse ein Ausnahmefehler ausgelöst wurde, ist die Ursache möglicherweise ein Verweis auf eine fehlende Ressource (d. h. eine Ressource, deren Schlüssel für Windows Phone Silverlight-Apps, jedoch nicht für Windows 10-Apps vorhanden ist, z. B. einige System-**TextBlock**-Stilschlüssel). Oder es kann eine Ausnahme sein, die innerhalb eines **UserControl**-Steuer Elements, eines benutzerdefinierten Steuer Elements oder eines benutzerdefinierten Layoutpanels ausgelöst wird.

Als letzte Möglichkeit kann eine Binärdatei aufgeteilt werden. Entfernen Sie etwa die Hälfte des Markups von einer Seite, und führen Sie die App erneut aus. Sie werden dann feststellen, ob der Fehler in der Hälfte liegt, die Sie entfernt haben (die Sie in jedem Fall wiederherstellen sollten) oder in der Hälfte, die Sie *nicht* entfernt haben. Wiederholen Sie den Vorgang durch Teilen der Hälfte mit den Fehler solange, Sie das Problem eingegrenzt haben.

## <a name="targetplatformversion"></a>TargetPlatformVersion

In diesem Abschnitt wird erläutert, was zu tun ist, wenn beim Öffnen eines Windows 10-Projekts in Visual Studio folgende Meldung angezeigt wird: „Visual Studio-Update erforderlich. Mindestens ein Projekt erfordert ein Plattform-SDK (&lt;version&gt;), das entweder nicht installiert oder in einem zukünftigen Update von Visual Studio enthalten ist.“

-   Ermitteln Sie zunächst die Versionsnummer des SDK für Windows 10, die Sie installiert haben. Navigieren Sie zu **C: \\ Programme (x86) \\ Windows Kits \\ 10 \\ include \\ &lt; versionfoldername &gt; ** , und notieren Sie sich den * &lt; versionfoldername &gt; *, der in Quad-Notation "Major. Minor. Build. Revision" angegeben wird.
-   Öffnen Sie Ihre Projektdatei zur Bearbeitung, und suchen Sie das `TargetPlatformVersion`-Element und das `TargetPlatformMinVersion`-Element. Bearbeiten Sie diese, sodass Sie * &lt; versionfoldername &gt; * durch die Versionsnummer der Quad-Notation ersetzt, die Sie auf dem Datenträger gefunden haben:

```xml
   <TargetPlatformVersion><versionfoldername></TargetPlatformVersion>
   <TargetPlatformMinVersion><versionfoldername></TargetPlatformMinVersion>
```

## <a name="troubleshooting-symptoms-and-remedies"></a>Symptome und Möglichkeiten zur Problembehandlung

Die Lösungsinformationen in der Tabelle sollten ausreichen, um Ihr Problem selbst zu behandeln. Weitere Details zu den einzelnen Problemen finden Sie in den späteren Themen.

| Symptom | Problembehandlung |
|---------|--------|
| Der XAML-Parser oder -Compiler meldet den Fehler „_Der Name „&lt;typename&gt;“ ist im Namespace „[…]“ nicht vorhanden_“. | Wenn &lt;typename&gt; ein benutzerdefinierter Typ ist, ändern Sie in Ihren Namespace-Präfixdeklarationen im XAML-Markup „clr-namespace“ in „using“, und entfernen Sie alle Assemblytoken. Für Plattformtypen bedeutet dies, dass der Typ nicht für die Universelle Windows-Plattform (UWP) gilt. Suchen Sie also nach der Entsprechung, und aktualisieren Sie Ihr Markup damit. Die Beispiele `phone:PhoneApplicationPage` und `shell:SystemTray.IsVisible` können sofort auftreten. | 
| Der XAML-Parser oder -Compiler meldet den Fehler „_Der Member „&lt;membername&gt;“ wurde nicht erkannt, oder es kann nicht auf den Member zugegriffen werden_“ oder „_Die „&lt;propertyname&gt;“-Eigenschaft wurde in Typ „[…]“ nicht gefunden_“. | Diese Fehler werden angezeigt, nachdem Sie einige Typnamen portiert haben, z. B. das **Page**-Stammelement. Der Member oder die Eigenschaft gilt nicht für die UWP. Suchen Sie also nach der Entsprechung, und aktualisieren Sie Ihr Markup damit. Die Beispiele `SupportedOrientations` und `Orientation` können sofort auftreten. |
| Der XAML-Parser oder -Compiler meldet den Fehler „_Die anfügbare […]-Eigenschaft wurde nicht gefunden […]_“ oder „_Unbekannter verknüpfbarer Member [...]_“. | Dies wird wahrscheinlich durch den Typ und nicht durch die angefügte Eigenschaft verursacht; in diesem Fall tritt bereits ein Fehler für diesen Typ auf, der verschwindet, sobald sie ihn behoben haben. Die Beispiele `phone:PhoneApplicationPage.Resources` und `phone:PhoneApplicationPage.DataContext` können sofort auftreten. | 
|Der XAML-Parser bzw. -Compiler oder eine Laufzeitausnahme meldet den Fehler „_Die Ressource „&lt;resourcekey&gt;“ konnte nicht aufgelöst werden_“. | Der Ressourcenschlüssel gilt nicht für UWP (Universelle Windows-Plattform)-Apps. Suchen Sie nach dem richtigen äquivalenten Ressourcenobjekt, und aktualisieren Sie Ihr Markup. System-**TextBlock**-Stilschlüssel wie `PhoneTextNormalStyle` können sofort auftreten. |
| Der C#-Compiler gibt folgenden Fehler aus: „_Der Typ- oder Namespacename „&lt;name&gt;“ konnte nicht gefunden werden […]_“ oder „_Der Typ- oder Namespacename „&lt;name&gt;“ ist im Namespace […] nicht vorhanden_“ oder „_Der Typ- oder Namespacename „&lt;name&gt;“ ist im aktuellen Kontext nicht vorhanden_“. | Dies bedeutet wahrscheinlich, dass der Compiler den richtigen UWP-Namespace für einen Typ noch nicht kennt. Sie können den Visual Studio-Befehl **Auflösen** verwenden, um dieses Problem zu beheben. <br/>Wenn die API nicht in der Gruppe der APIs enthalten ist, die als Familie der universellen Geräte bezeichnet wird (die API also in einem Erweiterungs-SDK implementiert ist), verwenden Sie die [Erweiterungs-SDKs](wpsl-to-uwp-porting-to-a-uwp-project.md).<br/>Es kann andere Fälle geben, in denen das Portieren weniger einfach ist. Die Beispiele `DesignerProperties` und `BitmapImage` können sofort auftreten. | 
|Wenn die APP auf dem Gerät ausgeführt wird, wird Sie beendet. Wenn Sie Sie von Visual Studio aus starten, wird die Fehlermeldung "Windows-Runtime 8. x-APP kann nicht aktiviert werden [...]" angezeigt. Fehler bei der Aktivierungsanforderung: „Windows konnte nicht mit der Zielanwendung kommunizieren.“ Dies weist in der Regel darauf hin, dass der Prozess der Zielanwendung abgebrochen wurde. […]”. | Das Problem ist möglicherweise der imperative Code, der während der Initialisierung auf Ihren eigenen Seiten oder in gebundenen Eigenschaften (oder anderen Typen) ausgeführt wird. Es könnte auch beim Analysieren der XAML-Datei passieren, die angezeigt wird, wenn die App beendet wird (beim Starten in Visual Studio ist dies die Startseite). Suchen Sie nach ungültigen Ressourcen Schlüsseln, und/oder probieren Sie einige der Anleitungen im Abschnitt [Nachverfolgen von Problemen](#tracking-down-issues) in diesem Thema aus.|
| _Xamlcompiler-Fehler WMC0055: der Textwert ' &lt; your Stream Geometry &gt; ' kann der ' Clip '-Eigenschaft des Typs ' rechglegeometry ' nicht zugewiesen werden._ | In der UWP der Typ der UWP-App mit [Microsoft DirectX](/windows/desktop/directx) XAML und C++. |
| _XamlCompiler-Fehler WMC0001: Unbekannter Typ „RadialGradientBrush“ im XML-Namespace [...]_ | Die UWP verfügt nicht über den **RadialGradientBrush**-Typ. Entfernen Sie das **RadialGradientBrush**-Element aus dem Markup, und verwenden Sie einen anderen Typ von UWP-App mit [Microsoft DirectX](/windows/desktop/directx), XAML und C++. |
| _Xamlcompiler-Fehler WMC0011: Unbekannter Member "OpacityMask" für das Element " &lt; UIElement Type &gt; "._ | Die UWP-App mit [Microsoft DirectX](/windows/desktop/directx), XAML und C++. |
| _Eine erste Chance-Ausnahme vom Typ ' System. Runtime. InteropServices. COMException ' ist in SYSTEM.NI.DLL aufgetreten. Zusätzliche Informationen: die Anwendung rief eine Schnittstelle auf, die für einen anderen Thread gemarshallt wurde. (Ausnahme von HRESULT: 0x8001010E (RPC_E_WRONG_THREAD))._ | Die Schritte, die Sie gerade ausführen, müssen im UI-Thread ausgeführt werden. Rufen Sie den [**CoreWindow.GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread)) auf. |
| Eine Animation wird ausgeführt, hat jedoch keine Auswirkungen auf ihre Zieleigenschaft. | Gestalten Sie die Animation unabhängig, oder legen Sie `EnableDependentAnimation="True"` dafür fest. Siehe [Animation](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Beim Öffnen eines Windows 10-Projekts in Visual Studio wird folgende Meldung angezeigt: „Visual Studio-Update erforderlich. Mindestens ein Projekt erfordert ein Plattform-SDK (&lt;version&gt;), das entweder nicht installiert oder in einem zukünftigen Update von Visual Studio enthalten ist.“ | Weitere Informationen finden Sie im Abschnitt [TargetPlatformVersion](#targetplatformversion) in diesem Thema. |
| Wenn in einer XAML.CS-Datei „InitializeComponent“ aufgerufen wird, wird eine „System.InvalidCastException“ ausgelöst. | Dies kann passieren, wenn mehrere XAML-Dateien (mindestens eine davon MRT-qualifiziert) dieselbe XAML.CS-Datei verwenden und Elemente x:Name-Attribute aufweisen, die zwischen den beiden XAML-Dateien inkonsistent sind. Versuchen Sie, den gleichen Elementen in XAML-Dateien denselben Namen hinzuzufügen, oder lassen Sie Namen ganz weg. | 

Das nächste Thema ist [Portieren von XAML und UI](wpsl-to-uwp-porting-xaml-and-ui.md).