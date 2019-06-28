---
description: Dieses Tutorial veranschaulicht das Hinzufügen von UWP XAML-Benutzeroberflächen, MSIX-Pakete erstellen und andere moderne Komponenten in Ihrer WPF-Anwendung integrieren.
title: Fügen Sie ein UWP-InkCanvas-Steuerelement, das mithilfe von XAML-Inseln
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, Uwp, Windows Forms, Wpf, XAML-Inseln
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 2f8cf18bce7bec880a2cb0bef298c0b565e20208
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420088"
---
# <a name="part-2-add-a-uwp-inkcanvas-control-using-xaml-islands"></a>Teil 2: Fügen Sie ein UWP-InkCanvas-Steuerelement, das mithilfe von XAML-Inseln

Dies ist der zweite Teil eines Tutorials, die zeigt, wie Sie eine Beispiel WPF-desktop-app mit dem Namen Contoso-Ausgaben zu modernisieren. Eine Übersicht über das Tutorial, Voraussetzungen und Anweisungen zum Herunterladen der Beispiel-app, finden Sie unter [Lernprogramm: Modernisieren von WPF-app](modernize-wpf-tutorial.md). In diesem Artikel wird vorausgesetzt, Sie haben bereits abgeschlossen [Teil 1](modernize-wpf-tutorial-1.md).

Das Contoso-Entwicklungsteam möchte, im fiktiven Szenario in diesem Tutorial die Contoso-Ausgaben-app-Unterstützung für digitale Signaturen hinzugefügt. Die UWP **InkCanvas** Steuerelement ist eine hervorragende Möglichkeit für dieses Szenario, da Freihandeingaben und KI Funktionen wie die Funktion zur Erkennung von Text und Formen unterstützt. Zu diesem Zweck verwenden Sie die [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) umschlossen von UWP-Steuerelement in das Windows-Community-Toolkit verfügbar. Dieses Steuerelement dient als Wrapper für die Schnittstelle und die Funktionalität der UWP **InkCanvas** Steuerelement für die Verwendung in einer WPF-Anwendung. Weitere Informationen zu den umschlossenen UWP-Steuerelementen, finden Sie unter [Host UWP XAML-Steuerelemente in desktop-apps (XAML-Inseln)](xaml-islands.md).

## <a name="configure-the-project-to-use-xaml-islands"></a>Konfigurieren des Projekts zur Verwendung von XAML-Inseln

Bevor Sie hinzufügen können eine **InkCanvas** Steuerelement an die Contoso-Ausgaben-app, müssen Sie zuerst das Projekt zur Unterstützung von UWP-XAML-Inseln konfigurieren.

1. Visual Studio-2019, mit der Maustaste auf die **ContosoExpenses.Core** Projekt **Projektmappen-Explorer** , und wählen Sie **NuGet-Pakete verwalten**.

    ![Verwalten von NuGet-Pakete-Menü in Visual Studio](images/wpf-modernize-tutorial//ManageNuGetPackages.png)

2. In der **NuGet Package Manager** Fenster, klicken Sie auf **Durchsuchen**. Wählen Sie die **Vorabversion einbeziehen** aus, suchen Sie nach der `Microsoft.Toolkit.Wpf.UI.Controls` Packen und Installieren der aktuellen Preview-Version des Pakets in den Ergebnissen angezeigt.

    > [!NOTE]
    > Dieses Paket enthält die notwendige Infrastruktur zum Hosten von UWP-XAML-Inseln in einer WPF-Anwendung, einschließlich der **InkCanvas** UWP-Steuerelement umschlossen. Eine ähnliche Paket mit dem Namen `Microsoft.Toolkit.Forms.UI.Controls` ist für Windows Forms-apps verfügbar.

3. Mit der rechten Maustaste **ContosoExpenses.Core** Projekt **Projektmappen-Explorer** , und wählen Sie **hinzufügen -> Neues Element**.

4. Wählen Sie **Anwendungsmanifestdatei**, nennen Sie sie **"App.manifest"** , und klicken Sie auf **hinzufügen**.

5. Suchen Sie in der Manifestdatei geöffnet, die **Kompatibilität** aus, und identifizieren Sie den folgenden kommentierten Eintrag.

    ```xml
    <!-- Windows 10 -->
    <!--<supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />-->
    ```

6. Fügen Sie unter diesem Eintrag das folgende Element hinzu.

    ```xml
    <maxversiontested Id="10.0.18362.0"/>
    ```

7. Heben Sie die auskommentierung der **SupportedOS** Eintrag für Windows 10. Dieser Abschnitt sollte jetzt wie folgt aussehen.

    ```xml
    <!-- Windows 10 -->
    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    <maxversiontested Id="10.0.18362.0"/>
    ```

    > [!NOTE]
    > Dieser Eintrag gibt an, dass die app Windows 10, Version 1903 erfordert (build 18362) oder höher. Dies ist die erste Version von Windows 10, die XAML-Inseln unterstützt. Ohne diesen Eintrag im Anwendungsmanifest löst die app zur Laufzeit eine Ausnahme aus.

8. Suchen Sie in der Manifestdatei, die folgenden kommentierten **Anwendung** Abschnitt.

    ```xml
    <!--
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
        <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
      </windowsSettings>
    </application>
    -->
    ```

9. Löschen Sie diesen Abschnitt, und Ersetzen Sie ihn durch folgendes XML. Dadurch wird die app, um die DPI-Wert beachten und besser Handle andere Skalierungsfaktoren von Windows 10 unterstützt werden konfiguriert.

    ```xml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/PM</dpiAware>
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
      </windowsSettings>
    </application>
    ```

10. Speichern und schließen Sie die `app.manifest` Datei.

12. In **Projektmappen-Explorer**, mit der rechten Maustaste die **ContosoExpenses.Core** Projekt, und wählen **Eigenschaften**.

13. In der **Ressourcen** Teil der **Anwendung** Registerkarte, stellen Sie sicher, dass die **Manifest** Dropdownliste nastaven NA hodnotu **"App.manifest"** .

    ![.NET Core-app-manifest](images/wpf-modernize-tutorial/NetCoreAppManifest.png)

16. Speichern Sie die Änderungen an den Projekteigenschaften.

## <a name="add-an-inkcanvas-control-to-the-app"></a>Fügen Sie der App ein InkCanvas-Steuerelement

Nun, dass Sie das Projekt zur Verwendung von UWP-XAML-Inseln konfiguriert haben, Sie sind jetzt bereit zum Hinzufügen einer [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) UWP-Steuerelement für die app umschlossen.

1. In **Projektmappen-Explorer**, erweitern Sie die **Ansichten** Ordner die **ContosoExpenses.Core** Projekt, und doppelklicken Sie auf die **ExpenseDetail.xaml**Datei.

2. In der **Fenster** Element am oberen Rand der XAML-Datei, fügen Sie das folgende Attribut hinzu. Dies verweist auf der XAML-Namespace für die [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) UWP-Steuerelement umschlossen.

    ```xml
    xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    Nach dem Hinzufügen dieses Attributs, das **Fenster** -Element sollte nun wie folgt aussehen.

    ```xml
    <Window x:Class="ContosoExpenses.Views.ExpenseDetail"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            xmlns:converters="clr-namespace:ContosoExpenses.Converters"
            DataContext="{Binding Source={StaticResource ViewModelLocator}, Path=ExpensesDetailViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Expense Detail" Height="500" Width="800"
            Background="{StaticResource HorizontalBackground}">
    ```

4. In der **ExpenseDetail.xaml** Datei, suchen Sie das schließende `</Grid>` Tag, das unmittelbar vor der `<!-- Chart -->` Kommentar. Fügen Sie den folgenden XAML direkt vor dem abschließenden `</Grid>` Tag. Dieses XAML Fügt ein **InkCanvas** Steuerelement (vorangestellt der **Toolkit** Schlüsselwort, die Sie zuvor als Namespace definiert) und ein einfaches **TextBlock** , die als ein Header für das Steuerelement fungiert.

    ```xml
    <TextBlock Text="Signature:" FontSize="16" FontWeight="Bold" Grid.Row="5" />

    <toolkit:InkCanvas x:Name="Signature" Grid.Row="6" />
    ```

5. Speichern Sie die **ExpenseDetail.xaml** Datei.

6. Drücken Sie F5, um die app im Debugger auszuführen.

7. Wählen Sie einen Mitarbeiter aus der Liste aus, und wählen Sie dann eine der verfügbaren Ausgaben. Beachten Sie, dass die Expense-Detailseite Speicherplatz für enthält die **InkCanvas** Steuerelement.

    ![Canvas Kugelschreiber nur](images/wpf-modernize-tutorial/InkCanvasPenOnly.png)

    Wenn Sie ein Gerät, das mit einen digitalen Stift, wie eine Oberfläche unterstützt und Sie diese Übungseinheit auf einem physischen Computer ausführen, fahren Sie aus, und versuchen Sie, ihn zu verwenden. Sie sehen, dass die digitale Freihandeingabe, die auf dem Bildschirm angezeigt. Aber wenn Sie nicht, ein Pen-fähiges Gerät haben, und Sie versuchen, sich mit der Maus, geschieht nichts. Dies geschieht, da die **InkCanvas** Steuerelement ist nur für digitale Stifte standardmäßig aktiviert. Allerdings können wir dieses Verhalten zu ändern.

8. Schließen Sie die app, und doppelklicken Sie auf der **ExpenseDetail.xaml.cs** -Datei unter dem **Ansichten** Ordner der **ContosoExpenses.Core** Projekt.

9. Fügen Sie am oberen Rand der Klasse die folgende Namespacedeklaration hinzu:

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

10. Suchen Sie die `ExpenseDetail()` Konstruktor.

11. Fügen Sie die folgende Codezeile unmittelbar nach der `InitializeComponent()` Methode, und speichern Sie die Codedatei.

    ```csharp
    Signature.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    Sie können die **InkPresenter** Objekt, das Anpassen der Freihand-Erfahrung. Dieser Code verwendet die **InputDeviceTypes** Eigenschaft, um die Maus und Stifteingabe aktiviert.

12. Drücken Sie erneut F5, um erneut, und führen die app im Debugger aus. Wählen Sie einen Mitarbeiter aus der Liste aus, und wählen Sie dann eine der verfügbaren Ausgaben.

13. Versuchen Sie es jetzt, um etwas in der Signatur Speicherplatz mit der Maus zu zeichnen. Dieses Mal sehen Sie die Freihandeingaben auf dem Bildschirm angezeigt.

    ![Signatur](images/wpf-modernize-tutorial/Signature.png)

## <a name="next-steps"></a>Nächste Schritte

An diesem Punkt in diesem Tutorial, Sie haben erfolgreich hinzugefügt eine UWP **InkCanvas** Steuerelement für die Contoso-Ausgaben-app. Sie sind nun bereit zur [Teil 3: Hinzufügen ein UWP-CalendarView-Steuerelements, das mithilfe von XAML-Inseln](modernize-wpf-tutorial-3.md).
