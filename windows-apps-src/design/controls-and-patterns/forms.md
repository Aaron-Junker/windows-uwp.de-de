---
Description: Layoutrichtlinien für Formulare in UWP-Apps.
title: Formulare
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: Windows 10, UWP, Fluent
ms.openlocfilehash: b6533864748b4245b16ec7bcea9d2a831ff1c88a
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "76520445"
---
# <a name="forms"></a>Formulare
Ein Formular ist eine Gruppe von Steuerelementen, die Daten von Benutzern sammeln und übermitteln. Formulare werden in der Regel für Seiten mit Einstellungen und Umfragen, zum Erstellen von Konten und für vieles mehr verwendet. 

In diesem Artikel werden Entwurfsrichtlinien für das Erstellen von XAML-Layouts für Formulare erörtert.

![Beispiel für Formulare](images/PivotHeader.png)

## <a name="when-should-you-use-a-form"></a>Wann sollten Sie ein Formular verwenden?
Ein Formular ist eine spezielle Seite zum Erfassen von Dateneingaben, die eindeutig zueinander gehören. Sie sollten ein Formular verwenden, wenn Sie explizit Daten von einem Benutzer sammeln möchten. Sie können für Benutzer auch ein Formular für folgende Zwecke erstellen:
- Anmelden bei einem Konto
- Registrieren für ein Konto
- Ändern von App-Einstellungen, z. B. Datenschutz- oder Anzeigeoptionen
- Teilnehmen an einer Umfrage
- Kaufen eines Artikels
- Feedback

## <a name="types-of-forms"></a>Arten von Formularen

Wenn es um das Übermitteln und Anzeigen von Benutzereingaben geht, gibt es zwei Arten von Formularen:

### <a name="1-instantly-updating"></a>1. Mit sofortiger Aktualisierung
![Einstellungsseite](images/control-examples/toggle-switch-news.png)

Verwenden Sie ein sich sofort aktualisierendes Formular, wenn Benutzer unmittelbar sehen sollen, welches Ergebnis das Ändern der Werte im Formular hat. Auf Einstellungsseiten werden die aktuell ausgewählten Optionen angezeigt, und sämtliche Änderungen an den Einstellungen werden sofort angewendet. Zum Bestätigen der Änderungen in der App müssen Sie jedem Eingabesteuerelement [einen Ereignishandler hinzufügen](controls-and-events-intro.md). Wenn ein Benutzer ein Eingabesteuerelement ändert, kann Ihre App entsprechend reagieren.

### <a name="2-submitting-with-button"></a>2. Übermitteln per Schaltfläche
Die andere Art von Formular ermöglicht es Benutzern zu entscheiden, ob sie Daten übermitteln, indem Sie auf eine Schaltfläche klicken.

![Seite zum Hinzufügen eines neuen Ereignisses im Kalender](images/calendar-form.png)

Diese Art von Formular bietet Benutzern eine größere Flexibilität hinsichtlich ihrer Antworten. Im Allgemeinen enthalten derartige Formulare Freiform-Eingabefelder, sodass eine größere Bandbreite von Antworten möglich ist. Um bei der Übermittlung gültige Benutzereingaben und ordnungsgemäß formatierte Daten sicherzustellen, sollten Sie folgende Empfehlungen befolgen:

- Schließen Sie mit dem richtigen Steuerelement aus, dass ungültige Informationen übermittelt werden (verwenden Sie z. B. ein CalendarDatePicker-Steuerelement anstelle eines TextBox-Steuerelements für Datumsangaben). Weitere Informationen zum Auswählen der richtigen Eingabesteuerelemente für das Formular finden Sie weiter unten im Abschnitt „Eingabesteuerelemente“.
- Geben Sie bei Verwendung von TextBox-Steuerelementen Benutzern mit der [PlaceholderText](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox.PlaceholderText)-Eigenschaft einen Hinweis, welches Eingabeformat gewünscht wird.
- Stellen Sie für Benutzer die geeignete Bildschirmtastatur bereit, indem Sie die erwartete Eingabe eines Steuerelements mit der [InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.inputscope)-Eigenschaft angeben.
- Heben Sie erforderliche Eingaben in der Bezeichnung mit einem Sternchen (*) hervor.
- Deaktivieren Sie die Schaltfläche zum Übermitteln, bis alle erforderlichen Informationen eingetragen wurden.
- Wenn bei der Übermittlung ungültige Daten festgestellt werden, kennzeichnen Sie die Steuerelemente mit ungültigen Eingaben mit hervorgehobenen Feldern oder Rahmen, und fordern Sie den Benutzer auf, das Formular erneut zu übermitteln.
- Stellen Sie sicher, dass bei sonstigen Fehlern (z. B. einer unterbrochenen Netzwerkverbindung) eine entsprechende Fehlermeldung für den Benutzer ausgegeben wird. 


## <a name="layout"></a>Layout

Befolgen Sie die nachstehenden Empfehlungen für das Entwerfen von Formularlayouts, um die Benutzerfreundlichkeit zu steigern und sicherzustellen, dass Benutzer die richtigen Informationen eingeben können. 

### <a name="labels"></a>Bezeichnungen
[Beschriftungen](labels.md) sollten linksbündig ausgerichtet und über dem Steuerelement platziert sein. Viele Steuerelemente verfügen über eine integrierte Header-Eigenschaft zum Anzeigen der Beschriftung. Für Steuerelemente ohne Header-Eigenschaft oder zum Beschriften von Steuerelementgruppen können Sie stattdessen ein [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Element verwenden.

Beschriften Sie beim [Entwerfen für Barrierefreiheit](../accessibility/accessibility.md) alle einzelnen Steuerelemente und Steuerelementgruppen, damit diese sowohl für Benutzer als auch für die Sprachausgabe eindeutig sind. 

Verwenden Sie für Schriftschnitte die standardmäßige [UWP-Typhierarchie](../style/typography.md). Verwenden Sie `TitleTextBlockStyle` für Seitentitel, `SubtitleTextBlockStyle` für Gruppenüberschriften und `BodyTextBlockStyle` für Bezeichnungen von Steuerelementen.

<div class="mx-responsive-img">
<table>
<tr><th>Empfohlen</th><th>Nicht empfohlen</th></tr>
<tr>
<td><img src="../controls-and-patterns/images/forms-shortform1col.png" alt="form with top labels"></td>
<td><img src="../controls-and-patterns/images/forms-leftlabel-donot1.png" alt="form with left labels don't"></td>
</tr>
</table>
</div>

### <a name="spacing"></a>Abstand
Informationen zur visuellen Abgrenzung einer Gruppe von Steuerelementen finden Sie unter [Ausrichtung, Ränder und Abstand](../layout/alignment-margin-padding.md). Einzelne Eingabesteuerelemente weisen eine Höhe von 80 Pixel auf, zwischen ihnen sollte ein Abstand von 24 Pixeln liegen. Der Abstand zwischen Gruppen von Eingabesteuerelementen sollte 48 Pixel betragen.

![Formulargruppen](images/forms-groups.png)

### <a name="columns"></a>Spalten
Durch das Erstellen von Spalten können Sie unnötigen Leerraum in Formularen verringern, insbesondere für größere Bildschirmgrößen. Wenn Sie jedoch ein Formular mit mehreren Spalten erstellen möchten, sollte die Anzahl der Spalten von der Anzahl der Eingabesteuerelemente auf der Seite und der Bildschirmgröße des App-Fensters abhängen. Anstatt den Bildschirm mit einer Vielzahl von Eingabesteuerelementen zu überfrachten, erwägen Sie, mehrere Seiten für das Formular zu erstellen.  

<div class="mx-responsive-img">
<table>
<tr><th>Empfohlen</th><th>Nicht empfohlen</th></tr>
<tr>
<td><img src="../controls-and-patterns/images/forms-2cols.png" alt="form with 2 columns"></td>
<td><img src="../controls-and-patterns/images/forms-2cols-bad.png" alt="form with 2 bad columns"></td>
</tr>
<tr><td><img src="../controls-and-patterns/images/forms-3cols.png" alt="form with 3 columns"></td></tr>
</table>

</div>

### <a name="responsive-layout"></a>Dynamisches Layout
Die Größe von Formularen sollte sich bei Änderungen der Bildschirm- oder Fenstergröße ändern, damit Benutzer Eingabefelder nicht übersehen. Weitere Informationen finden Sie unter [Reaktionsfähige Designtechniken](../layout/responsive-design.md). Es kann beispielsweise von Vorteil sein, bestimmte Bereiche des Formulars immer sichtbar zu halten, ungeachtet von der jeweiligen Bildschirmgröße.

![Fokus auf Formularen](images/forms-focus2.png)

### <a name="tab-stops"></a>Tabstopps
Benutzer können über die Eingabe von [Tabstopps](../input/keyboard-interactions.md#tab-stops) auf der Tastatur zwischen Steuerelementen wechseln. Standardmäßig spiegelt die Aktivierreihenfolge der Steuerelemente die Reihenfolge wider, in der sie in XAML erstellt wurden. Um das Standardverhalten zu überschreiben, ändern Sie die **IsTabStop**-Eigenschaft oder die **TabIndex**-Eigenschaft des Steuerelements. 

![Fokus durch TAB-TASTE auf Steuerelement im Formular](images/forms-focus1.png)

## <a name="input-controls"></a>Eingabesteuerelemente
Eingabesteuerelemente sind Elemente der Benutzeroberfläche, über die Benutzer Informationen in Formulare eingeben können. Nachfolgend sind einige gängige Steuerelemente aufgeführt, die Formularen hinzugefügt werden können, sowie Angaben zu ihrer Verwendung.

### <a name="text-input"></a>Texteingabe
Control | Verwendung | Beispiel
 - | - | -
[TextBox](text-box.md) | Erfassen einer oder mehrerer Textzeilen | Namen, formfreie Antworten oder Feedback
[PasswordBox](password-box.md) | Erfassen privater Daten durch Verbergen der Zeichen | Kennwörter, Sozialversicherungsnummern (SSN), PINs, Kreditkarteninformationen 
[AutoSuggestBox](auto-suggest-box.md) | Anzeigen einer Liste mit Vorschlägen für Benutzer aus einer entsprechenden Menge von Daten während der Eingabe | Datenbanksuche, mail to:-Zeile, vorherige Abfragen
[RichEditBox](rich-edit-box.md) | Bearbeiten von Textdateien mit formatiertem Text, Links und Bildern | Hochladen von Dateien, Vorschau und Bearbeiten in der App

### <a name="selection"></a>Auswahl
Control | Verwendung | Beispiel
- | - | - 
| [CheckBox](checkbox.md) | Aktivieren oder Deaktivieren eines oder mehrerer Aktionselemente | Akzeptieren von Geschäftsbedingungen, Hinzufügen von optionalen Elementen, Auswählen aller zutreffenden Optionen
[RadioButton](radio-button.md) | Auswählen einer Option aus mindestens zwei Auswahlmöglichkeiten | Auswählen von Typ, Versandart usw.
[ToggleSwitch](toggles.md) | Auswählen einer von zwei einander ausschließenden Optionen | Ein/Aus

> **Hinweis**: Wenn fünf oder mehr Auswahlelemente vorhanden sind, verwenden Sie ein Listensteuerelement.

### <a name="lists"></a>Listen
Control | Verwendung | Beispiel
- | - | -
[ComboBox](combo-box.md) | Starten im kompakten Modus und Erweitern, um die Liste der auswählbaren Elemente anzuzeigen | Auswählen aus einer langen Liste von Elementen, z. B. Status oder Länder/Regionen
[ListView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#list-views) | Kategorisieren von Elementen und Zuweisen von Gruppenüberschriften, Verschieben von Elementen per Drag & Drop, Überprüfen von Inhalten und Neuanordnen von Elementen | Bilden einer Rangfolge für Optionen
[GridView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#grid-views) | Anordnen und Durchsuchen von bildbasierten Sammlungen | Auswählen eines Fotos, einer Farbe oder Anzeigedesigns

### <a name="numeric-input"></a>Numerische Eingabe
Control | Verwendung | Beispiel
- | - | -
[Schieberegler](slider.md) | Auswählen einer Zahl aus einem Bereich von fortlaufenden numerischen Werten | Prozentsätze, Lautstärke, Wiedergabegeschwindigkeit
[Rating](rating.md) | Bewerten mit Sternen | Kundenfeedback

### <a name="date-and-time"></a>Datum und Uhrzeit

Control | Verwendung 
- | - 
[CalendarView](calendar-view.md) | Auswählen eines einzelnen Datums oder eines Datumsbereichs aus einem immer sichtbaren Kalender 
[CalendarDatePicker](calendar-date-picker.md) | Auswählen eines einzelnen Datums aus einem kontextbezogenen Kalender 
[DatePicker](date-picker.md) | Auswählen eines einzelnen bekannten Datums, wenn kontextbezogene Informationen nicht wichtig sind
[TimePicker](time-picker.md) | Auswählen eines einzelnen Uhrzeitwerts

### <a name="additional-controls"></a>Zusätzliche Steuerelemente 
Eine komplette Liste der UWP-Steuerelemente finden Sie unter [Index der Steuerelemente nach Funktion](controls-by-function.md).

Komplexere und benutzerdefinierte Steuerelemente der Benutzeroberfläche finden Sie in UWP-Ressourcen, die von Unternehmen wie [Telerik](https://www.telerik.com/), [SyncFusion](https://www.syncfusion.com/uwp-ui-controls), [DevExpress](https://www.devexpress.com/Products/NET/Controls/Win10Apps/), [Infragistics](https://www.infragistics.com/products/universal-windows-platform), [ComponentOne](https://www.componentone.com/Studio/Platform/UWP) und [ActiPro](https://www.actiprosoftware.com/products/controls/universal) bereitgestellt werden.

## <a name="one-column-form-example"></a>Beispiel für Formular mit einer Spalte
In diesem Beispiel werden eine Acrylic-[Master/Detail](master-details.md)-[Listenansicht](lists.md) und ein [NavigationView](navigationview.md)-Steuerelement verwendet.
![Screenshot eines weiteren Formularbeispiels](images/FormExample2.png)
```xaml
<StackPanel>
    <TextBlock Text="New Customer" Style="{StaticResource TitleTextBlockStyle}"/>
    <Button Height="100" Width="100" Background="LightGray" Content="Add photo" Margin="0,24" Click="AddPhotoButton_Click"/>
    <TextBox x:Name="Name" Header= "Name" Margin="0,24,0,0" MaxLength="32" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
    <TextBox x:Name="PhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="15" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
    <TextBox x:Name="Email" Header="Email" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <TextBox x:Name="Address" Header="Address" PlaceholderText="Address" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <TextBox x:Name="Address2" Margin="0,24,0,0" PlaceholderText="Address 2" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <RelativePanel>
        <TextBox x:Name="City" PlaceholderText="City" Margin="0,24,0,0" MaxLength="50" Width="200" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
        <ComboBox x:Name="State" PlaceholderText="State" Margin="24,24,0,0"  Width="100" RelativePanel.RightOf="City">
             <x:String>WA</x:String>
        </ComboBox>
    </RelativePanel>
    <TextBox x:Name="ZipCode" PlaceholderText="Zip Code" Margin="0,24,0,0" MaxLength="6" Width="100" HorizontalAlignment="Left" InputScope="Number" />
    <StackPanel Orientation="Horizontal">
        <Button Content="Save" Margin="0,24" Click="SaveButton_Click"/>
        <Button Content="Cancel" Margin="24" Click="CancelButton_Click"/>
    </StackPanel>  
</StackPanel>
```

## <a name="two-column-form-example"></a>Beispiel für Formular mit zwei Spalten
In diesem Beispiel werden zusätzlich zu Eingabesteuerelementen das [Pivot](pivot.md)-Steuerelement, der [Acrylic](../style/acrylic.md)-Hintergrund und [CommandBar](app-bars.md) verwendet.
![Screenshot eines Formularbeispiels](images/FormExample.png)
```xaml
<Grid>
    <Pivot Background="{ThemeResource SystemControlAccentAcrylicWindowAccentMediumHighBrush}" >
        <Pivot.TitleTemplate>
            <DataTemplate>
                <Grid>
                    <TextBlock Text="Company Name" Style="{ThemeResource HeaderTextBlockStyle}"/>
                </Grid>
            </DataTemplate>
        </Pivot.TitleTemplate>
        <PivotItem Header="Orders" Margin="0"/>    
        <PivotItem Header="Customers" Margin="0">
            <!--Form Example-->
            <Grid Background="White">
                <RelativePanel>
                    <StackPanel x:Name="Customer" Margin="20">
                        <TextBox x:Name="CustomerName" Header= "Customer Name" Margin="0,24,0,0" MaxLength="320" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
                        <TextBox x:Name="CustomerPhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
                        <TextBox x:Name="Address" Header="Address" PlaceholderText="Address" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="AlphanumericFullWidth" />
                        <TextBox x:Name="Address2" Margin="0,24,0,0" PlaceholderText="Address 2" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="AlphanumericFullWidth" />
                        <RelativePanel>
                            <TextBox x:Name="City" PlaceholderText="City" Margin="0,24,0,0" MaxLength="50" Width="200" HorizontalAlignment="Left" InputScope="Text" />
                            <ComboBox x:Name="State" PlaceholderText="State" Margin="24,24,0,0"  Width="100" RelativePanel.RightOf="City">
                                <x:String>WA</x:String>
                            </ComboBox>
                        </RelativePanel>
                        <TextBox x:Name="ZipCode" PlaceholderText="Zip Code" Margin="0,24,0,0" MaxLength="6" Width="100" HorizontalAlignment="Left" InputScope="Number" />
                    </StackPanel>
                    <StackPanel x:Name="Associate" Margin="20" RelativePanel.RightOf="Customer">
                        <TextBox x:Name="AssociateName" Header= "Associate" Margin="0,24,0,0" MaxLength="320" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
                        <TextBox x:Name="AssociatePhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
                        <DatePicker x:Name="TargetInstallDate" Header="Target install Date" HorizontalAlignment="Left" Margin="0,24,0,0"></DatePicker>
                        <TimePicker x:Name="InstallTime" Header="Install Time" HorizontalAlignment="Left" Margin="0,24,0,0"></TimePicker>
                    </StackPanel>
                </RelativePanel>
            </Grid>
        </PivotItem>
        <PivotItem Header="Invoices"/>
        <PivotItem Header="Stock"/>
        <Pivot.RightHeader>
            <CommandBar OverflowButtonVisibility="Collapsed" Background="Transparent">
                <AppBarButton Icon="Add"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Edit" />
                <AppBarButton Icon="Delete"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Save"/>
            </CommandBar>
        </Pivot.RightHeader>
    </Pivot>
</Grid>
```

## <a name="customer-orders-database-sample"></a>Beispieldatenbank für Kundenaufträge
![Screenshot von Datenbank für Kundenaufträge](images/customerorderform.png) Eine Beschreibung, wie Sie Formulareingaben mit einer **Azure**-Datenbank verbinden, sowie ein vollständig implementiertes Formular finden Sie im Beispiel zur App [Datenbank für Kundenaufträge](https://github.com/Microsoft/Windows-appsample-customers-orders-database).

## <a name="related-topics"></a>Zugehörige Themen
- [Eingabesteuerelemente](controls-and-events-intro.md)
- [Typografie](../style/typography.md)
