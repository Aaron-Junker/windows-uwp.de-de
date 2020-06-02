---
Description: Es wird beschrieben, wie Sie in das Erstellen von Desktop-Apps für Windows-PCs einsteigen, z. B. das Auswählen der richtigen App-Plattform für neue Apps und das Modernisieren von vorhandenen Apps für Windows 10.
title: Erstellen von Desktop-Apps für Windows-PCs
ms.topic: article
ms.date: 11/04/2019
keywords: Windows Win32, Desktopentwicklung
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: d052ad0f670bccd9b32d2e3643520dd6129ed22a
ms.sourcegitcommit: cc645386b996f6e59f1ee27583dcd4310f8fb2a6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84262741"
---
# <a name="build-desktop-apps-for-windows-pcs"></a>Erstellen von Desktop-Apps für Windows-PCs

Dieser Artikel enthält Informationen, die Sie benötigen, um mit dem Entwickeln von Desktop-Apps für Windows zu beginnen oder vorhandene Desktop-Apps zu aktualisieren, um die neuesten Funktionen von Windows 10 zu nutzen.

## <a name="platforms-for-desktop-apps"></a>Plattformen für Desktop-Apps

Es gibt vier Hauptplattformen zum Entwickeln von Desktop-Apps für Windows-PCs. Jede Plattform bietet ein App-Modell, das den Lebenszyklus der App definiert, einen vollständigen Satz von UI-Steuerelementen und Zugriff auf einen umfassenden Satz verwalteter oder nativer APIs für die Verwendung von Windows-Features.

In der folgenden Tabelle werden die Plattformen vorgestellt. Einen ausführlichen Vergleich dieser Plattformen sowie zusätzliche Ressourcen für jede Plattform finden Sie unter [Auswählen Ihrer App-Plattform](choose-your-platform.md).

<br/>

<table>
<colgroup>
<col width="20%" />
<col width="60%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th>Plattform</th>
<th>Beschreibung</th>
<th>Tools und Ressourcen</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="https://docs.microsoft.com/windows/uwp/">Universelle Windows-Plattform (UWP)</a></td>
<td><p>Die führende Plattform für Windows 10-Apps und Spiele. Sie können UWP-Apps erstellen, die ausschließlich UWP-Steuerelemente und -APIs verwenden, oder Sie verwenden UWP-Steuerelemente und -APIs in Desktop-Apps, die mit einer der anderen Plattformen erstellt wurden.</p></td>
<td><a href="/windows/uwp/get-started/">Erste Schritte</a><br/><a href="/uwp/">API-Referenz</a><br/><a href="https://github.com/Microsoft/Windows-universal-samples">Beispiele</a></td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/windows/win32/">Win32</a></td>
<td><p>Die Plattform der Wahl für native C/C++-Windows-Apps, die direkten Zugriff auf Windows und Hardware erfordern.</p></td>
<td><a href="/windows/win32/desktop-programming/">Erste Schritte</a><br/><a href="/windows/win32/apiindex/windows-api-list/">API-Referenz</a><br/><a href="https://github.com/Microsoft/Windows-classic-samples">Beispiele</a></td>
</tr>
<tr class="odd">
<td><a href="https://docs.microsoft.com/dotnet/framework/wpf/">WPF</a></td>
<td><p>Die festgelegte. NET-basierte Plattform für grafisch aufbereitet verwaltete Windows-Apps mit einem XAML-Benutzeroberflächenmodell. Diese Apps können als Ziel <a href="https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0">.NET Core 3</a> oder das vollständige .NET Framework haben.</p></td>
<td><a href="/dotnet/framework/wpf/getting-started/">Erste Schritte</a><br/><a href="https://docs.microsoft.com/dotnet/api/index">API-Referenz (.NET)</a><br/><a href="https://github.com/Microsoft/WPF-Samples">Beispiele</a></td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/dotnet/framework/winforms/">Windows Forms</a></td>
<td><p>Eine .NET-basierte Plattform, die für verwaltete branchenspezifische Apps mit einem vereinfachten Benutzeroberflächenmodell entwickelt wurde. Diese Apps können als Ziel <a href="https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0">.NET Core 3</a> oder das vollständige .NET Framework haben.</p></td>
<td><a href="/dotnet/framework/winforms/getting-started-with-windows-forms">Erste Schritte</a><br/><a href="https://docs.microsoft.com/dotnet/api/index">API-Referenz (.NET)</a></td>
</tr>
</tbody>
</table>

> [!NOTE]
> Alle diese Anwendungsplattformen bietet ein komplettes Benutzeroberflächen-Framework und eine Reihe von Benutzeroberflächen-Steuerelementen, mit denen Desktopanwendungen wie Word, Excel und Photoshop erstellen werden können, die auf dem klassischen Windows-Desktop laufen und die spezifischen Features dieser Umgebung voll ausnutzen können. Unter Windows 10 unterstützen alle diese Plattformen auch die Windows-UI-Bibliothek (WinUI) zum Erstellen ihrer Benutzeroberflächen. Weitere Informationen zu WinUI für Desktop-Apps finden Sie in [diesem Abschnitt](choose-your-platform.md#windows-ui-library).

## <a name="update-existing-desktop-apps-for-windows-10"></a>Aktualisieren vorhandener Desktop-Apps für Windows 10

Wenn Sie über eine vorhandene WPF-, Windows Forms- oder native Win32-Desktop-App verfügen, bieten Windows 10 und die Universelle Windows-Plattform (UWP) viele Features, mit denen Sie in Ihren Apps eine moderne Benutzerumgebung bereitstellen können. Die meisten dieser Features sind als modulare Komponenten verfügbar, die Sie je nach Bedarf in Ihre Apps übernehmen können, ohne dass Sie Ihre App für eine andere Plattform umschreiben müssen.

Hier sind einige der verfügbaren Features zum erweitern vorhandener Desktop-Apps aufgeführt:

* Verwenden Sie [MSIX](/windows/msix/), um Ihre Desktop-Apps zu verpacken und bereitzustellen. MSIX ist ein modernes Windows-App-Paketformat, bei dem eine universelle Verpackungsoberfläche für alle Windows-Apps bereitgestellt wird. MSIX vereint die besten Aspekte der MSI-, APPX-, App-V- und ClickOnce-Installationstechnologie und ist auf Sicherheit, Schutz und Zuverlässigkeit ausgelegt.
* Integrieren Sie Ihre Desktop-App mit [Paketerweiterungen](/windows/apps/desktop/modernize/desktop-to-uwp-extensions) in Windows 10-Umgebungen. Zeigen Sie z.B. auf Startkacheln für die App, machen Sie Ihre App zu einem Freigabeziel, oder senden Sie Popupbenachrichtigungen von Ihrer App.
* Verwenden Sie [XAML-Inseln](/windows/apps/desktop/modernize/xaml-islands), um UWP-XAML-Steuerelemente in Ihrer Desktop-App zu hosten. Viele der neuesten Windows 10-Benutzeroberflächenfeatures sind nur für UWP-XAML-Steuerelemente verfügbar.

Weitere Informationen finden Sie in folgenden Artikeln.

<br/>

| Artikel | Beschreibung |
|---------|-------------|
| [Modernisieren von Desktop-Apps](/windows/apps/desktop/modernize) | Beschreibt die neuesten Windows 10- und UWP-Entwicklungsfeatures, die Sie in beliebigen Desktop-Apps wie WPF-, Windows Forms- und C++-Win32-Apps verwenden können. |
| [Tutorial: Modernisieren einer WPF-App](/windows/apps/desktop/modernize/modernize-wpf-tutorial) | Befolgen Sie die Schritt-für-Schritt-Anweisungen, um eine vorhandene branchenspezifische WPF-Beispiel-App zu modernisieren, indem Sie der App UWP-Freihand- und Kalendersteuerelemente hinzufügen und sie in einem MSIX-Paket verpacken.  |

## <a name="create-new-desktop-apps"></a>Erstellen neuer Desktop-Apps

Wenn Sie eine neue Desktop-App für Windows erstellen, finden Sie hier einige Ressourcen, die Ihnen beim Einstieg helfen.

<br/>

| Artikel | Beschreibung |
|---------|-------------|
| [Auswählen Ihrer App-Plattform](choose-your-platform.md) | Bietet einen ausführlichen Vergleich der wichtigsten Desktop-App-Plattformen und unterstützt Sie bei der Auswahl der richtigen Plattform für Ihre Bedürfnisse. Außerdem finden Sie in diesem Artikel nützliche Links zur Dokumentation für jede Plattform. |
| [Modernisieren von Desktop-Apps](/windows/apps/desktop/modernize) | Beschreibt die neuesten Windows 10- und UWP-Entwicklungsfeatures, die Sie in beliebigen Desktop-Apps wie WPF-, Windows Forms- und C++-Win32-Apps verwenden können. |
| [Features und Technologien](/windows/apps/features-and-technologies) | Bietet eine Übersicht über die Windows-Features, auf die über jede der Hauptplattformen für Desktop-Apps zugegriffen werden kann, sowie Links zur zugehörigen Dokumentation. |

## <a name="related-documentation-and-technologies"></a>Verwandte Dokumentation und Technologien

| Ressource | Beschreibung |
|---------|-------------|
| [.NET Core 3.0](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0) | Erfahren Sie mehr über die neuesten Features von .NET Core 3.0, einschließlich Erweiterungen für WPF- und Windows Forms-Apps. |
| [Desktophandbuch für WPF und .NET Core 3.0](https://docs.microsoft.com/dotnet/desktop-wpf/overview/index) | Entwickeln Sie WPF-Apps mit .NET Core 3.0 als Ziel, anstelle des vollständigen .NET Frameworks.  |
| [Azure](https://docs.microsoft.com/azure/) | Erweitern Sie die Reichweite Ihrer Apps mit Azure Cloud Services. |
| [Visual Studio](https://docs.microsoft.com/visualstudio/) | Erfahren Sie, wie Sie Visual Studio zum Entwickeln von Apps und Diensten verwenden. |
| [MSIX](https://docs.microsoft.com/windows/msix/) | Sie können Windows-Apps in einem modernen und universellen Paketformat verpacken und bereitstellen. |
| [Windows KI](https://docs.microsoft.com/windows/ai/) | Verwenden Sie Windows-KI, um intelligente Lösungen für komplexe Probleme in ihren Apps zu erstellen. |
| [Windows-Container](https://docs.microsoft.com/virtualization/windowscontainers/) | Verpacken Sie Ihre Anwendungen mit ihren Abhängigkeiten in schnellen, vollständig isolierten Windows-Umgebungen. |
| [Progressive Web-Apps](https://docs.microsoft.com/microsoft-edge/progressive-web-apps) | Konvertieren Sie Ihre Web-Apps in Progressive Web-Apps, die als UWP-Apps unter Windows 10 verteilt und ausgeführt werden können. |
| [Xamarin](https://docs.microsoft.com/xamarin/) | Erstellen Sie plattformübergreifende Apps für Windows, Android, iOS und macOS mithilfe von .NET-Code und plattformspezifischen Benutzeroberflächen. |
| [Dokumentarchiv für Windows 8.x und früher](https://docs.microsoft.com/previous-versions/windows/) | Hier finden Sie die archivierte Dokumentation zur Entwicklung von Apps für Windows 8.x und frühere Versionen. |
