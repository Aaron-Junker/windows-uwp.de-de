---
description: In diesem Tutorial wird veranschaulicht, wie Sie UWP-XAML-Benutzeroberflächen hinzufügen, msix-Pakete erstellen und andere moderne Komponenten in Ihre WPF-App integrieren.
title: Hinzufügen eines UWP-InkCanvas-Steuerelements mithilfe von XAML Islands
ms.topic: article
ms.date: 01/24/2020
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, XAML-Inseln
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 945cc2f1cf225c194e5820990bdbeda584069e4c
ms.sourcegitcommit: 1455e12a50f98823bfa3730c1d90337b1983b711
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76814040"
---
# <a name="part-2-add-a-uwp-inkcanvas-control-using-xaml-islands"></a>Teil 2: Hinzufügen eines UWP InkCanvas-Steuer Elements mithilfe von XAML-Inseln

Dies ist der zweite Teil eines Tutorials, in dem veranschaulicht wird, wie eine WPF-Beispiel-Desktop-App mit dem Namen "ca Eine Übersicht über das Tutorial, Voraussetzungen und Anweisungen zum Herunterladen der Beispiel-App finden Sie unter [Tutorial: modernisieren einer WPF-App](modernize-wpf-tutorial.md). In diesem Artikel wird davon ausgegangen, dass Sie [Teil 1](modernize-wpf-tutorial-1.md)bereits abgeschlossen haben.

Im fiktiven Szenario dieses Lernprogramms möchte das Entwicklungsteam von "Configuration Manager" die Unterstützung digitaler Signaturen der App "kostenpflichtige app" für die kostenpflichtige app hinzufügen. Das UWP **InkCanvas** -Steuerelement ist eine gute Option für dieses Szenario, da es digitale Freihand-und Ki-Funktionen unterstützt, wie z. b. die Fähigkeit, Text und Formen zu erkennen. Zu diesem Zweck verwenden Sie das in der Windows Community Toolkit verfügbare [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) -UWP-Steuerelement. Dieses Steuerelement umschließt die-Schnittstelle und die Funktionalität des UWP **InkCanvas** -Steuer Elements für die Verwendung in einer WPF-App. Weitere Informationen zu umschließenen UWP-Steuerelementen finden Sie unter [Host UWP-XAML-Steuerelemente in Desktop-Apps (XAML-Inseln)](xaml-islands.md).

> [!NOTE]
> In diesem Tutorial hostet die WPF-App nur UWP-Steuerelemente von erst Anbietern aus der Windows SDK. In diesem Tutorial werden daher die Schritte zum Definieren einer Instanz der [Microsoft. Toolkit. Win32. UI. xamlhost. xamlapplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) -Klasse, wie [hier](host-standard-control-with-xaml-islands.md#required-components)beschrieben, ausgelassen.

## <a name="configure-the-project-to-use-xaml-islands"></a>Konfigurieren des Projekts für die Verwendung von XAML-Inseln

Vor dem Hinzufügen eines **InkCanvas** -Steuer Elements zur APP für die kostenpflichtige app müssen Sie zuerst das Projekt für die Unterstützung von UWP-XAML-Inseln konfigurieren.

1. Klicken Sie in Visual Studio 2019 mit der rechten Maustaste auf das Projekt **contosoaufwands. Core** in **Projektmappen-Explorer** , und wählen Sie **nuget-Pakete verwalten**aus.

    ![Menü "nuget-Pakete verwalten" in Visual Studio](images/wpf-modernize-tutorial//ManageNuGetPackages.png)

2. Klicken Sie im Fenster **nuget-Paket-Manager** auf **Durchsuchen**. Suchen Sie nach dem `Microsoft.Toolkit.Wpf.UI.Controls` Paket, und installieren Sie die Version 6.0.0 oder eine höhere Version.

    > [!NOTE]
    > Dieses Paket enthält die gesamte erforderliche Infrastruktur zum Hosting von UWP-XAML-Inseln in einer WPF-App, einschließlich des mit **InkCanvas** umschließenden UWP-Steuer Elements. Ein ähnliches Paket mit dem Namen `Microsoft.Toolkit.Forms.UI.Controls` ist für Windows Forms-apps verfügbar.

3. Klicken Sie in **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **contosoaufwendungen. Core** , und wählen Sie **Add-> Neues Element**aus.

4. Wählen Sie **Anwendungs Manifest-Datei**aus, nennen Sie Sie **app. Manifest**, und klicken Sie auf **Hinzufügen**. Weitere Informationen zu Anwendungs Manifesten finden Sie in [diesem Artikel](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests).

5. Heben Sie die Auskommentierung des folgenden `<supportedOS>` Elements für Windows 10 in der Manifest-Datei auf.

    ```xml
    <!-- Windows 10 -->
    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    ```

6. Suchen Sie in der Manifest-Datei nach folgendem kommentierten `<application>` Element.

    ```xml
    <!--
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
        <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
      </windowsSettings>
    </application>
    -->
    ```

7. Löschen Sie diesen Abschnitt, und ersetzen Sie ihn durch den folgenden XML-Code. Dadurch wird die APP so konfiguriert, dass Sie dpi-fähig ist, und die verschiedenen von Windows 10 unterstützten Skalierungsfaktoren besser verarbeiten.

    ```xml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/PM</dpiAware>
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
      </windowsSettings>
    </application>
    ```

8. Speichern und schließen Sie die Datei `app.manifest`.

9. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt **contosoaufwendungen. Core** , und wählen Sie **Eigenschaften**aus.

10. Vergewissern Sie sich, dass im Abschnitt **Ressourcen** der Registerkarte **Anwendung** die Dropdown Liste **Manifest** auf **app. Manifest**festgelegt ist.

    ![.Net Core-App-Manifest](images/wpf-modernize-tutorial/NetCoreAppManifest.png)

11. Speichern Sie die Änderungen an den Projekteigenschaften.

## <a name="add-an-inkcanvas-control-to-the-app"></a>Fügen Sie der APP ein InkCanvas-Steuerelement hinzu.

Nachdem Sie Ihr Projekt für die Verwendung von UWP-XAML-Inseln konfiguriert haben, können Sie nun ein mit [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) umschließtes UWP-Steuerelement zur APP hinzufügen.

1. Erweitern Sie in **Projektmappen-Explorer**den Ordner **views** des Projekts **contosoaufwendungen. Core** , und doppelklicken Sie auf die Datei **expensdetail. XAML** .

2. Fügen Sie im **Fenster** Element in der Nähe des oberen Rands der XAML-Datei das folgende Attribut hinzu. Dies verweist auf den XAML-Namespace für das mit [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) umschließende UWP-Steuerelement.

    ```xml
    xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    Nach dem Hinzufügen dieses Attributs sollte das **Fenster** Element nun wie folgt aussehen.

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

4. Suchen Sie in der Datei " **expensdetail. XAML** " das schließende `</Grid>`-Tag, das dem `<!-- Chart -->` Kommentar unmittelbar vorangestellt ist. Fügen Sie den folgenden XAML-Code direkt vor dem schließenden `</Grid>`-Tag hinzu. Dieser XAML-Code fügt ein **InkCanvas** -Steuerelement hinzu (vorangestellt das **Toolkit** -Schlüsselwort, das Sie zuvor als Namespace definiert haben) und einen einfachen **TextBlock** , der als Header für das Steuerelement fungiert.

    ```xml
    <TextBlock Text="Signature:" FontSize="16" FontWeight="Bold" Grid.Row="5" />

    <toolkit:InkCanvas x:Name="Signature" Grid.Row="6" />
    ```

5. Speichern Sie die Datei " **expenldetail. XAML** ".

6. Drücken Sie F5, um die APP im Debugger auszuführen.

7. Wählen Sie einen Mitarbeiter aus der Liste aus, und wählen Sie dann eine der verfügbaren Ausgaben aus. Beachten Sie, dass die Seite mit den Ausgaben Details Platz für das **InkCanvas** -Steuerelement enthält.

    ![Nur Ink-Canvas-Stift](images/wpf-modernize-tutorial/InkCanvasPenOnly.png)

    Wenn Sie ein Gerät haben, das einen digitalen Stift wie eine Oberfläche unterstützt, und Sie dieses Lab auf einem physischen Computer ausführen, können Sie versuchen, es zu verwenden. Der digitale Ink wird auf dem Bildschirm angezeigt. Wenn Sie jedoch nicht über ein Stift fähiges Gerät verfügen und versuchen, sich mit der Maus anzumelden, geschieht nichts. Dies geschieht, da das **InkCanvas** -Steuerelement standardmäßig nur für digitale Stifte aktiviert ist. Dieses Verhalten kann jedoch geändert werden.

8. Schließen Sie die APP, und doppelklicken Sie auf die Datei **ExpenseDetail.XAML.cs** im Ordner **views** des Projekts **contosoaufwendungen. Core** .

9. Fügen Sie am Anfang der-Klasse die folgende Namespace Deklaration hinzu:

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

10. Suchen Sie den `ExpenseDetail()`-Konstruktor.

11. Fügen Sie nach der `InitializeComponent()`-Methode die folgende Codezeile hinzu, und speichern Sie die Codedatei.

    ```csharp
    Signature.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    Sie können das **InkPresenter** -Objekt verwenden, um die standardmäßige Einbindung zu ändern. In diesem Code wird die **inputdevicetypes** -Eigenschaft verwendet, um die Maus und die Stift Eingabe zu aktivieren.

12. Drücken Sie erneut F5, um die APP im Debugger neu zu erstellen und auszuführen. Wählen Sie einen Mitarbeiter aus der Liste aus, und wählen Sie dann eine der verfügbaren Ausgaben aus.

13. Versuchen Sie jetzt, etwas im Signatur Bereich mit der Maus zu zeichnen. Dieses Mal wird die frei Hand Eingabe auf dem Bildschirm angezeigt.

    ![Signatur](images/wpf-modernize-tutorial/Signature.png)

## <a name="next-steps"></a>Nächste Schritte

An dieser Stelle im Tutorial haben Sie ein UWP-Steuerelement " **InkCanvas** " zu der App "appto-Ausgaben" hinzugefügt. Sie sind jetzt bereit für [Teil 3: Hinzufügen eines UWP CalendarView-Steuer Elements mithilfe von XAML-Inseln](modernize-wpf-tutorial-3.md).
