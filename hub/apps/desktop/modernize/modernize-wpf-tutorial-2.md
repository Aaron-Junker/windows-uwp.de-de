---
description: In diesem Tutorial wird veranschaulicht, wie du UWP-XAML-Benutzeroberflächen hinzufügst, MSIX-Pakete erstellst und weitere moderne Komponenten in deine WPF-App integrierst.
title: Hinzufügen eines UWP-InkCanvas-Steuerelements mithilfe von XAML Islands
ms.topic: article
ms.date: 01/24/2020
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, XAML Islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 0b5250f1e01aece4f73d83dc7327f193a58f53cf
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161494"
---
# <a name="part-2-add-a-uwp-inkcanvas-control-using-xaml-islands"></a>Teil 2: Hinzufügen eines UWP-InkCanvas-Steuerelements mithilfe von XAML Islands

Dies ist der zweite Teil eines Tutorials, in dem das Modernisieren der WPF-Beispiel-Desktop-App Contoso Expenses veranschaulicht wird. Eine Übersicht über das Tutorial, die Voraussetzungen und Anweisungen zum Herunterladen der Beispiel-App findest du unter [Tutorial: Modernisieren einer WPF-App](modernize-wpf-tutorial.md). In diesem Artikel wird davon ausgegangen, dass du [Teil 1](modernize-wpf-tutorial-1.md) bereits abgeschlossen hast.

Im fiktiven Szenario dieses Tutorials möchte das Contoso-Entwicklungsteam der App Contoso Expenses Unterstützung für digitale Signaturen hinzufügen. Das UWP-Steuerelement **InkCanvas** ist eine großartige Option für dieses Szenario, da es Freihandschrift und KI-gestützte Funktionen wie die Fähigkeit zur Erkennung von Text und Formen unterstützt. Dazu verwendest du das mit [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) umschlossene UWP-Steuerelement, das im Windows Community Toolkit verfügbar ist. Dieses Steuerelement umschließt die Schnittstelle und Funktionalität des UWP-Steuerelements **InkCanvas** zur Verwendung in einer WPF-App. Weitere Details zu umschlossenen UWP-Steuerelementen findest du unter [Hosten von UWP-XAML-Steuerelementen in Desktop-Apps (XAML Islands)](xaml-islands.md).

## <a name="configure-the-project-to-use-xaml-islands"></a>Konfigurieren des Projekts für die Verwendung von XAML Islands

Bevor du der App Contoso Expenses ein **InkCanvas**-Steuerelement hinzufügen kannst, musst du das Projekt zunächst für die Unterstützung von UWP XAML Islands konfigurieren.

1. Klicke in Visual Studio 2019 im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **ContosoExpenses.Core**, und wähle **NuGet-Pakete verwalten** aus.

    ![Menü „NuGet-Pakete verwalten“ in Visual Studio](images/wpf-modernize-tutorial//ManageNuGetPackages.png)

2. Klicke im Fenster **NuGet-Paket-Manager** auf **Durchsuchen**. Suche nach dem Paket `Microsoft.Toolkit.Wpf.UI.Controls`, und installiere mindestens Version 6.0.0.

    > [!NOTE]
    > Dieses Paket enthält die gesamte erforderliche Infrastruktur zum Hosten der UWP XAML Islands in einer WPF-App, einschließlich des umschlossenen UWP-Steuerelements **InkCanvas**. Ein ähnliches Paket mit dem Namen `Microsoft.Toolkit.Forms.UI.Controls` ist für Windows Forms-Apps verfügbar.

3. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **ContosoExpenses.Core**, und wähle **Hinzufügen > Neues Element** aus.

4. Wähle **Anwendungsmanifestdatei** aus. Nenne die Datei **app.manifest**, und klicke auf **Hinzufügen**. Weitere Informationen Anwendungsmanifesten findest du in [diesem Artikel](/windows/desktop/SbsCs/application-manifests).

5. Hebe in der Manifestdatei die Auskommentierung des folgenden `<supportedOS>`-Elements für Windows 10 auf.

    ```xml
    <!-- Windows 10 -->
    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    ```

6. Suche in der Manifestdatei das folgende kommentierte `<application>`-Element.

    ```xml
    <!--
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
        <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
      </windowsSettings>
    </application>
    -->
    ```

7. Lösche diesen Abschnitt, und ersetze ihn durch den folgenden XML-Code. Dadurch wird die App so konfiguriert, dass sie DPI-fähig ist und verschiedene von Windows 10 unterstützte Skalierungsfaktoren besser handhaben kann.

    ```xml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/PM</dpiAware>
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
      </windowsSettings>
    </application>
    ```

8. Speichere und schließe die Datei `app.manifest`.

9. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **ContosoExpenses.Core**, und wähle **Eigenschaften** aus.

10. Stelle sicher, dass im Abschnitt **Ressourcen** der Registerkarte **Anwendung** die Dropdownliste **Manifest** auf **app.manifest** festgelegt ist.

    ![.NET Core-App-Manifest](images/wpf-modernize-tutorial/NetCoreAppManifest.png)

11. Speichere die Änderungen an den Projekteigenschaften.

## <a name="add-an-inkcanvas-control-to-the-app"></a>Hinzufügen des Steuerelements InkCanvas zur App

Nachdem du dein Projekt zur Verwendung von UWP XAML Islands konfiguriert hast, kannst du jetzt der App das umschlossene UWP-Steuerelemente [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) hinzufügen.

1. Erweitere im **Projektmappen-Explorer** den Ordner **Ansichten** des Projekts **ContosoExpenses.Core**, und doppelklicke auf die Datei **ExpenseDetail.xaml**.

2. Füge dem **Window**-Element im oberen Bereich der XAML-Datei das folgende Attribut hinzu. So wird auf den XAML-Namespace für das umschlossene UWP-Steuerelement [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) verwiesen.

    ```xml
    xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    Nach dem Hinzufügen dieses Attributs sollte das **Window**-Element in etwa so aussehen.

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

4. Suche in der Datei **ExpenseDetail.xaml** das schließende `</Grid>`-Tag, das dem Kommentar `<!-- Chart -->` unmittelbar vorausgeht. Füge den folgenden XML-Ausschnitt direkt vor dem schließenden `</Grid>`-Tag ein. Hiermit wird ein **InkCanvas**-Steuerelement (mit dem Schlüsselwort **toolkit** als Präfix, das zuvor als Namespace definiert wurde) und ein einfaches **TextBlock**-Element hinzugefügt, das als Header des Steuerelements fungiert.

    ```xml
    <TextBlock Text="Signature:" FontSize="16" FontWeight="Bold" Grid.Row="5" />

    <toolkit:InkCanvas x:Name="Signature" Grid.Row="6" />
    ```

5. Speichere die Datei **ExpenseDetail.xaml**.

6. Drücke F5, um die App im Debugger auszuführen.

7. Wähle einen Mitarbeiter in der Liste und dann eine der verfügbaren Ausgaben aus. Beachte, dass die Ausgabendetailseite Platz für das Steuerelement **InkCanvas** enthält.

    ![InkCanvas: nur Stift](images/wpf-modernize-tutorial/InkCanvasPenOnly.png)

    Wenn du über ein Gerät mit Unterstützung eines digitalen Stifts verfügst – wie beispielsweise Surface – und du dieses Lab auf einem physischen Computer ausführst, versuche, den Stift zu nutzen. Die Freihandschrift wird auf dem Bildschirm angezeigt. Wenn du jedoch über kein solches Gerät verfügst und versuchst, mit deiner Maus zu zeichnen, geschieht nichts. Dies liegt daran, dass das Steuerelement **InkCanvas** standardmäßig nur für die Eingabe mit einem digitalen Stift aktiviert ist. Dieses Verhalten kann jedoch geändert werden.

8. Schließe die App, und doppelklicke im Projektmappen-Explorer im Ordner **Ansichten** des Projekts **ContosoExpenses.Core** auf die **Datei ExpenseDetail.xaml**.

9. Füge am Anfang der Klasse die folgende Namespacedeklaration hinzu:

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

10. Suche den `ExpenseDetail()`-Konstruktor.

11. Füge nach der `InitializeComponent()`-Methode die folgende Codezeile ein, und speichere die Codedatei.

    ```csharp
    Signature.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    Du kannst das Standardverhalten für die Freihandeingabe mit dem **InkPresenter**-Objekt anpassen. Dieser Code verwendet die **InputDeviceTypes**-Eigenschaft, um sowohl die Maus- als auch die Stifteingabe zu aktivieren.

12. Drücke erneut F5, um die App neu zu kompilieren und im Debugger auszuführen. Wähle einen Mitarbeiter in der Liste und dann eine der verfügbaren Ausgaben aus.

13. Versuche jetzt, etwas im Unterschriftsbereich mit der Maus zu zeichnen. Dieses Mal wird die Freihandschrift auf dem Bildschirm angezeigt.

    ![Signatur](images/wpf-modernize-tutorial/Signature.png)

## <a name="next-steps"></a>Nächste Schritte

An dieser Stelle im Tutorial hast du der Contoso Expenses-App erfolgreich das UWP-Steuerelement **InkCanvas** hinzugefügt. Du bis nun bereit für [Teil 3: Hinzufügen des UWP-Steuerelements CalendarView mithilfe von XAML Islands](modernize-wpf-tutorial-3.md).