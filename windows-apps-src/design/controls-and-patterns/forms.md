---
Description: Layoutrichtlinien für die Formulare in UWP-apps.
title: Formulare
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: Windows 10, UWP, fluent
ms.openlocfilehash: 8a57f13e168a248569bca1beeceed7b4f6c89f69
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658155"
---
# <a name="forms"></a>Formulare
Ein Formular ist eine Gruppe von Steuerelementen, die erfassen und Senden von Daten von Benutzern. Formulare werden in der Regel verwendet, für die Seite mit Einstellungen, Umfragen, Erstellen von Konten und vieles mehr. 

Dieser Artikel beschreibt Entwurfsrichtlinien für das Erstellen von XAML-Layouts für Formulare.

![Beispiel für form](images/PivotHeader.png)

## <a name="when-should-you-use-a-form"></a>Wann sollten Sie ein Formular verwenden?
Ein Formular ist eine dedizierte Seite zum Sammeln von von Dateneingaben, die eindeutig miteinander verknüpft sind. Sie sollten ein Formular verwenden, wenn Sie explizit Daten von einem Benutzer sammeln möchten. Sie können ein Formular für einen Benutzer zu erstellen:
- Melden Sie sich bei einem Konto
- Melden Sie sich für ein Konto
- Ändern Sie die Appeinstellungen, z. B. Datenschutz oder Anzeigeoptionen
- Machen Sie eine Umfrage
- Erwerben Sie ein Element
- Feedback bereitstellen

## <a name="types-of-forms"></a>Arten von Formularen

Wenn übermittelt und Darstellung von Benutzereingaben, gibt es zwei Arten von Formularen:

### <a name="1-instantly-updating"></a>1. Sofort aktualisieren.
![Seite "Einstellungen"](images/control-examples/toggle-switch-news.png)

Verwenden Sie ein sofort aktualisieren Formular aus, wenn Benutzer sofort die Ergebnisse Ändern der Werte im Formular angezeigt werden sollen. Z. B. in die Seite mit Einstellungen, die aktuellen Einstellungen werden angezeigt, und alle an der Auswahl der vorgenommenen Änderungen werden sofort angewendet. Um die Änderungen in Ihrer app zu bestätigen, dass [Hinzufügen eines ereignishandlers](controls-and-events-intro.md) auf jedem Eingabesteuerelement. Wenn ein Benutzer ein Eingabesteuerelements ändert, kann Ihre app angemessen reagieren.

### <a name="2-submitting-with-button"></a>2. Übermitteln mit der Schaltfläche
Die andere Art von Formular kann den Benutzer beim Senden von Daten mit einem Klick auf eine Schaltfläche auswählen.

![Kalender hinzufügen, neue Seite "Ereignisse"](images/calendar-form.png)

Diese Art von Formular bietet die Benutzerflexibilität reagiert. In der Regel dieses Typs des Formulars weitere formfreien Eingabefelder enthält und daher empfängt eine größere Vielfalt von Antworten. Um gültige Benutzereingaben und ordnungsgemäß formatierte Daten nach der Übermittlung zu gewährleisten, sollten Sie folgende Aspekte:

- Machen Sie es nicht möglich, ungültige Informationen über das richtige Steuerelement senden (d. h., verwenden Sie eine CalendarDatePicker statt ein Textfeld für Datumsangaben). Weitere Informationen finden Sie bei der Auswahl der entsprechenden Benutzereingabe-Steuerelemente im Formular in der Benutzereingabe-Steuerelemente in einem späteren Abschnitt aktivieren.
- Wenn TextBox-Steuerelemente verwenden, geben Sie Benutzern einen Hinweis, der das gewünschte Eingabeformat mit der [PlaceholderText](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox.PlaceholderText) Eigenschaft.
- Geben Sie die Benutzer mit dem entsprechenden Bildschirmtastatur, indem Sie angeben, die erwartete Eingabe eines Steuerelements mit der [InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.inputscope) Eigenschaft.
- Markieren Sie erforderliche Eingabe mit einem Sternchen * für die Bezeichnung.
- Deaktivieren Sie die Schaltfläche "Senden" aus, bis alle erforderlichen Informationen ausgefüllt wird.
- Wenn ungültige Daten nach der Übermittlung vorhanden sind, markieren Sie die Steuerelemente mit der ungültigen Eingabe mit hervorgehobenen Feldern oder Rahmen und muss der Benutzer das Formular erneut übermitteln.
- Stellen Sie sicher, dass eine entsprechende Fehlermeldung für den Benutzer anzuzeigen, auf andere Fehler, z. B. Fehler bei der Netzwerkverbindung. 


## <a name="layout"></a>Layout

Um die benutzererfahrung zu vereinfachen, und stellen Sie sicher, dass Benutzer die richtige Eingabe eingeben können, sollten Sie die folgenden Empfehlungen für das Entwerfen von Layouts für Formulare aus. 

### <a name="labels"></a>Beschriftungen
[Bezeichnungen](labels.md) ausgerichtet und über das Steuerelement platziert werden soll. Viele Steuerelemente verfügen über eine integrierte Header-Eigenschaft der Bezeichnung angezeigt werden. Für Steuerelemente ohne Header-Eigenschaft oder zum Beschriften von Steuerelementgruppen können Sie stattdessen ein [TextBlock](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Element verwenden.

Um [Entwurf für den Zugriff auf](../accessibility/accessibility.md), bezeichnen alle Person und eine Gruppe von Steuerelementen für Übersichtlichkeit für beide Menschen und Sprachausgabe. 

Verwenden Sie die Standardeinstellung für Schriftschnitte, [UWP Typ Ramp](../style/typography.md). Verwendung `TitleTextBlockStyle` für Seitentitel, `SubtitleTextBlockStyle` für Gruppenüberschriften, und `BodyTextBlockStyle` für Bezeichnungen.

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
Um eine Gruppe von Steuerelementen visuell voneinander zu trennen, verwenden Sie [Alignment, Margin und Padding](../layout/alignment-margin-padding.md). Einzelne Eingabesteuerelementen sollte 80px Höhe werden und durch Leerzeichen getrennten 24px auseinander liegen. Gruppen von Benutzereingabe-Steuerelemente sollten Abstand 48px auseinander liegen.

![Forms-Gruppen](images/forms-groups.png)

### <a name="columns"></a>Spalten
Erstellen von Spalten kann unnötigen Leerraum in Formularen, vor allem bei größeren Bildschirmgrößen reduziert werden. Wenn Sie ein Formular mit mehreren Spalten erstellen möchten, sollte die Anzahl der Spalten hängt jedoch die Anzahl der Benutzereingabe-Steuerelemente auf der Seite und die Größe des Bildschirms der app-Fensters ab. Statt den Bildschirm mit zahlreichen Eingabesteuerelementen zu überlasten, erwägen Sie, mehrere Seiten für das Formular zu erstellen.  

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

### <a name="responsive-layout"></a>Dynamisches layout
Forms sollte als die größenänderungen Bildschirm- oder Größe ändern, damit der Benutzer alle Felder für die Eingabe nicht detailliert erläutern. Weitere Informationen finden Sie unter [reaktionsfähige Designtechniken](../layout/responsive-design.md). Beispielsweise empfiehlt es sich zu bestimmten Regionen des Formulars immer in der Ansicht jeder Bildschirmgröße.

![Forms-Fokus](images/forms-focus2.png)

### <a name="tab-stops"></a>Tabstopps
Benutzer können mithilfe die Tastatur Steuerelemente mit navigieren [Tabstopps](../input/keyboard-interactions.md#tab-stops). Standardmäßig gibt die Aktivierreihenfolge von Steuerelementen für die Reihenfolge, in der sie in XAML erstellt werden. Um das Standardverhalten zu überschreiben, ändern Sie die **IsTabStop** oder **TabIndex** Eigenschaften des Steuerelements. 

![TAB-Fokus auf das Steuerelement im Formular](images/forms-focus1.png)

## <a name="input-controls"></a>Benutzereingabe-Steuerelemente
Benutzereingabe-Steuerelemente sind die Elemente der Benutzeroberfläche, die Benutzer zur Eingabe von Informationen in Formulare zu ermöglichen. Einige allgemeine Steuerelemente, die Formen hinzugefügt werden können, sowie Informationen zu deren Verwendung weiter unten.

### <a name="text-input"></a>Texteingabe
Steuerelement | Verwendung | Beispiel
 - | - | -
[TextBox](text-box.md) | Erfassen Sie eine oder mehrere Textzeilen | Namen, formfreien Antworten oder feedback
[PasswordBox](password-box.md) | Sammeln Sie private Daten verdecken von Zeichen | Kennwörter, Sozialversicherungsnummern (SSN), PINs, Kreditkarteninformationen 
[AutoSuggestBox](auto-suggest-box.md) | Zeigen Sie Benutzer eine Liste mit Vorschlägen in einen entsprechenden Satz von Daten an, während sie eingeben | Datenbank suchen, um e-Mail-: Zeile, die vorherige Abfragen
[RichEditBox](rich-edit-box.md) | Bearbeiten von Textdateien mit formatierten Text, links und Bilder | Datei hochladen, Vorschau und in der app bearbeiten

### <a name="selection"></a>Auswahl
Steuerelement | Verwendung | Beispiel
- | - | - 
| [CheckBox](checkbox.md) | Aktivieren Sie oder deaktivieren Sie eine oder mehrere Aufgaben | Geschäftsbedingungen stimmen Sie zu, fügen Sie optionale Elemente hinzu, wählen Sie alle zutreffenden
[RadioButton](radio-button.md) | Wählen Sie eine Option aus, aus zwei oder mehr Optionen | Wählen Sie Typ, Versand, Methode usw. an.
[ToggleSwitch](toggles.md) | Wählen Sie eine der zwei sich gegenseitig ausschließende Optionen | Aktivieren/Deaktivieren

> **Hinweis**: Wenn fünf oder mehr Auswahlelemente vorhanden sind, verwenden Sie ein Listensteuerelement.

### <a name="lists"></a>Listen
Steuerelement | Verwendung | Beispiel
- | - | -
[ComboBox](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists.md#drop-down-lists) | Starten Sie in compact Zustand, und erweitern Sie, um die Liste der auswählbaren Elemente anzeigen | Wählen Sie aus einer langen Liste von Elementen, z. B. Status oder Länder
[ListView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#list-views) | Einträge und Kopfzeilen von Gruppen zuweisen, ziehen Sie Elemente ablegen, kommentieren Sie die Inhalte und Elemente neu anordnen | Rank-Optionen
[GridView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#grid-views) | Anordnen und Image-basierte Sammlungen durchsuchen | Wählen Sie ein Foto, Farbe, zeigen Design an

### <a name="numeric-input"></a>Numerische Eingaben
Steuerelement | Verwendung | Beispiel
- | - | -
[Schieberegler](slider.md) | Wählen Sie eine Zahl aus einem Bereich von fortlaufenden numerischen Werten | Prozentsätze "," Volume "," wiedergabegeschwindigkeit
[Bewertung](rating.md) | Bewertung mit Sternen | Kundenfeedback

### <a name="date-and-time"></a>Datum und Uhrzeit

Steuerelement | Verwendung 
- | - 
[CalendarView](calendar-view.md) | Wählen Sie ein einzelnes Datum oder einen Bereich von Datumswerten aus einem Kalender stets sichtbar sind 
[CalendarDatePicker](calendar-date-picker.md) | Wählen Sie ein einzelnes Datum aus einem kontextbezogene Kalender 
["DatePicker"](date-picker.md) | Wählen Sie ein einzelnes lokalisierte Datum Wenn kontextbezogene Informationen nicht wichtig ist
[TimePicker](time-picker.md) | Wählen Sie einen einzigen Wert

### <a name="additional-controls"></a>Zusätzliche Steuerelemente 
Eine vollständige Liste der UWP-Steuerelementen, finden Sie unter [Index der Steuerelemente nach Funktion](controls-by-function.md).

Komplexer und benutzerdefinierte Benutzeroberflächen-Steuerelemente, finden Sie in UWP-Ressourcen, die von anderen Unternehmen verfügbar wie z. B. [Telerik](https://www.telerik.com/), [SyncFusion](https://www.syncfusion.com/products/uwp), [DevExpress](https://www.devexpress.com/Products/NET/Controls/Win10Apps/), [ Infragistics](https://www.infragistics.com/products/universal-windows-platform), [ComponentOne](https://www.componentone.com/Studio/Platform/UWP), und [ActiPro](https://www.actiprosoftware.com/products/controls/universal).

## <a name="one-column-form-example"></a>Beispiel für eine Spalte form
Dieses Beispiel verwendet ein Blog [Master/Detail-](master-details.md) [Listenansicht](lists.md) und [NavigationView](navigationview.md) Steuerelement.
![Screenshot von einer anderen Form-Beispiel](images/FormExample2.png)
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

## <a name="two-column-form-example"></a>Beispiel für die Form von zwei Spalten
Dieses Beispiel verwendet die [Pivot](pivot.md) Steuerelement [Blog](../style/acrylic.md) Hintergrund und [CommandBar](app-bars.md) neben der Benutzereingabe-Steuerelemente.
![Screenshot des Form-Beispiel](images/FormExample.png)
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

## <a name="customer-orders-database-sample"></a>Customer-Orders-Datenbank-Beispiel
![Kunde bestellt Datenbank Screenshot](images/customerorderform.png) erfahren, wie Formulareingabe für die Verbindung ein **Azure** Datenbank und ein vollständig implementiertes Formular, finden Sie unter der [Kunden Auftragsdatenbank](https://github.com/Microsoft/Windows-appsample-customers-orders-database) Beispiel-app.

## <a name="related-topics"></a>Verwandte Themen
- [Benutzereingabe-Steuerelemente](controls-and-events-intro.md)
- [Typography](../style/typography.md)
