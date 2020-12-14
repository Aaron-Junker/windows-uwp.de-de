---
description: Eine Infoleiste (InfoBar-Steuerelement) ist eine Inlinebenachrichtigung für wichtige App-weite Meldungen.
title: InfoBar
template: detail.hbs
ms.date: 11/30/2020
ms.topic: article
keywords: Windows 10, WinUI, UWP
pm-contact: gabilka
design-contact: kimsea
dev-contact: ranjeshj
ms.custom: 20H2
ms.localizationpriority: medium
ms.openlocfilehash: 422d2cb0874abe2fbe767a75d718cd1f0637ccee
ms.sourcegitcommit: b99fe39126fbb457c3690312641f57d22ba7c8b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2020
ms.locfileid: "96604963"
---
# <a name="infobar"></a>InfoBar
Das Steuerelement „InfoBar“ wird verwendet, um Benutzern App-weite Statusmeldungen anzuzeigen, die deutlich sichtbar, aber nicht aufdringlich sind. Es gibt integrierte Schweregrade, um die Art der angezeigten Meldung auf einfache Weise anzugeben, sowie die Option, eine eigene Schaltfläche für Handlungsaufforderungen oder Links einzubinden. Da das InfoBar-Steuerelement eine Inlinebenachrichtigung neben anderen Inhalten der Benutzeroberfläche darstellt, besteht die Möglichkeit, festzulegen, dass das Steuerelement immer sichtbar ist oder vom Benutzer verworfen werden kann. 

**Abrufen der Windows-UI-Bibliothek**

:::row:::
   :::column:::
      ![WinUI-Logo](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      Das Steuerelement **InfoBar** erfordert die Windows-UI-Bibliothek. Diese Bibliothek ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für Windows-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](/uwp/toolkits/winui/) (Windows-UI-Bibliothek).
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **APIs der Bibliothek „Windows UI“** [InfoBar-Klasse](/uwp/api/microsoft.ui.xaml.controls.infobar)

> [!TIP]
> In diesem Dokument stellt der Alias **muxc** in XAML die APIs der Windows-UI-Bibliothek dar, die wir in unser Projekt aufgenommen haben. Wir haben dem [Page](/uwp/api/windows.ui.xaml.controls.page)-Element Folgendes hinzugefügt: `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>Im CodeBehind stellt ebenfalls der Alias **muxc** in C# die APIs der Windows-UI-Bibliothek dar, die wir in unser Projekt aufgenommen haben. Wir haben am Anfang der Datei die folgende **using**-Anweisung hinzugefügt: `using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?
Verwenden Sie ein InfoBar-Steuerelement, wenn ein Benutzer über einen geänderten Anwendungsstatus informiert werden, diesen bestätigen oder eine Aktion ausführen soll. Standardmäßig bleibt die Benachrichtigung im Inhaltsbereich, bis sie vom Benutzer geschlossen wird, unterbricht aber nicht unbedingt den Benutzerfluss.

Ein InfoBar-Steuerelement nimmt Platz im Layout in Anspruch und verhält sich wie alle anderen untergeordneten Elemente. Er verdeckt keine anderen Inhalte und ist auch nicht frei schwebend über diesen angeordnet.

Verwenden Sie ein InfoBar-Steuerelement nicht zum Bestätigen oder direkten Reagieren auf eine Benutzeraktion, die den Zustand der App nicht ändert, und nicht für zeitkritische oder unwichtige Meldungen.

### <a name="remarks"></a>Bemerkungen
Verwenden Sie ein InfoBar-Steuerelement, das vom Benutzer geschlossen wird oder geschlossen wird, wenn der Status aufgelöst wird, für Szenarien, die **direkt** die App-Wahrnehmung oder -Erfahrung beeinflussen.


Hier einige Beispiele:
- Internetverbindung wurde getrennt.
- Beim Speichern eines Dokuments tritt ein Fehler auf, wobei der Speichervorgang automatisch ausgelöst wurde und nicht auf eine bestimmte Benutzeraktion zurückzuführen ist.
- Beim Versuch, eine Aufnahme anzufertigen, ist kein Mikrofon angeschlossen.
- Es kann keine Verbindung mit Ihrem Smartphone hergestellt werden.
- Das Abonnement für die Anwendung ist abgelaufen.


Verwenden Sie ein InfoBar-Steuerelement, das vom Benutzer geschlossen wird, für Szenarien, die **indirekt** die App-Wahrnehmung oder -Erfahrung beeinflussen.

Hier einige Beispiele:
- Die Aufzeichnung eines Anrufs wurde gestartet.
- Ein Update wurde installiert, mit einem Link zu „Anmerkungen zu dieser Version“.
- Die Nutzungsbedingungen wurden aktualisiert und erfordern eine Bestätigung.
- Eine App-weite Sicherung wurde erfolgreich, asynchron abgeschlossen
- Das Abonnement für die Anwendung läuft in Kürze ab.

### <a name="when-should-a-different-control-be-used"></a>Wann sollte ein anderes Steuerelement verwendet werden?

Es gibt einige Szenarien, in denen die Verwendung eines [ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)-, [Flyout](/uwp/api/Windows.UI.Xaml.Controls.Flyout)- oder [TeachingTip](/uwp/api/Microsoft.UI.Xaml.Controls.TeachingTip)-Steuerelements sinnvoller ist.

- Für Szenarien, in denen keine dauerhafte Benachrichtigung benötigt wird, z. B. die Anzeige von Informationen im Kontext eines bestimmten Benutzeroberflächenelements, ist ein [Flyout](/uwp/design/controls-and-patterns/dialogs-and-flyouts/flyouts)-Steuerelement die bessere Option. 
- Für Szenarien, in denen die Anwendung eine Benutzeraktion bestätigt und Informationen anzeigt, die der Benutzer lesen **_muss_* _, verwenden Sie ein [ContentDialog](/uwp/design/controls-and-patterns/dialogs-and-flyouts/dialogs)-Steuerelement.
  - Wenn eine Statusänderung der App so schwerwiegend ist, dass sie alle weiteren Möglichkeiten des Benutzers zur Interaktion mit der App blockieren muss, verwenden Sie zusätzlich ein ContentDialog-Steuerelement.
- Für Szenarien, in denen eine Benachrichtigung vorübergehend zu Lehrzwecken angezeigt wird, ist ein [TeachingTip](/uwp/design/controls-and-patterns/dialogs-and-flyouts/teaching-tip)-Steuerelement eine bessere Option.


Weitere Informationen zur Auswahl des richtigen Benachrichtigungssteuerelements finden Sie im Artikel [Dialogfelder und Flyouts](/uwp/design/controls-and-patterns/dialogs-and-flyouts/).


## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Falls die App <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> installiert ist, klicke <a href="xamlcontrolsgallery:/item/InfoBar">hier</a>, um die App zu öffnen und das InfoBar-Steuerelement in Aktion zu sehen.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

### <a name="create-an-infobar"></a>Erstellen eines InfoBar-Steuerelements
Der nachfolgende XAML-Code beschreibt ein InfoBar-Inlinesteuerelement mit der Standardformatierung für eine Informationsbenachrichtigung. Eine Infoleiste kann wie jedes andere Element platziert werden und folgt dem Basislayoutverhalten.
In einem vertikalen StackPanel wird das InfoBar-Steuerelement beispielsweise horizontal erweitert, um die verfügbare Breite auszufüllen. 


Standardmäßig ist das InfoBar-Steuerelement nicht sichtbar. Legen Sie die Eigenschaft „IsOpen“ im XAML- oder im CodeBehind-Code auf „true“ fest, um die Infoleiste anzuzeigen.


```xaml
<muxc:InfoBar x:Name="UpdateAvailableNotification"
    Title="Update available."
    Message="Restart the application to apply the latest update.">
</muxc:InfoBar>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    if(IsUpdateAvailable())
    {
        UpdateAvailableNotification.IsOpen = true;
    }
}
```

![Beispiel eines InfoBar-Steuerelements im Standardzustand mit einer Schließen-Schaltfläche und einer Meldung](images/infobar-default-title-message.png)

### <a name="using-pre-defined-severity-levels"></a>Verwenden vordefinierter Schweregrade
Der Typ der Infoleiste kann über die Eigenschaft „Severity“ festgelegt werden, um automatisch eine konsistente Statusfarbe, ein Symbol und Einstellungen für Hilfstechnologien in Abhängigkeit von der Wichtigkeit der Benachrichtigung festzulegen. Wenn kein Schweregrad festgelegt ist, wird die standardmäßige Informationsformatierung angewendet.


```xaml
<muxc:InfoBar x:Name="SubscriptionExpiringNotification"
    Severity="Warning"
    Title="Your subscription is expiring in 3 days."
    Message="Renew your subscription to keep all functionality" />
```
![Beispiel eines InfoBar-Steuerelements im Zustand „Warnung“ mit einer Schließen-Schaltfläche und einer Meldung](images/infobar-warning-title-message.png)

### <a name="programmatic-close-in-infobar"></a>Programmgesteuertes Schließen der Infoleiste
Ein InfoBar-Steuerelement kann vom Benutzer über die Schaltfläche „Schließen“ oder programmseitig geschlossen werden. Wenn die Benachrichtigung so lange angezeigt werden soll, bis der Status aufgelöst ist, und Sie nicht zulassen möchten, dass der Benutzer die Infoleiste schließt, können Sie die Eigenschaft „IsClosable“ auf „false“ festlegen.


Der Standardwert dieser Eigenschaft ist „true“, was bedeutet, dass eine Schließen-Schaltfläche in Form eines „X“ vorhanden ist.


```xaml
<muxc:InfoBar x:Name="NoInternetNotification"
    Severity="Error"
    Title="No Internet"
    Message="Reconnect to continue working."
    IsClosable="False" />
```
![Beispiel eines InfoBar-Steuerelements im Zustand „Fehler“ ohne Schließen-Schaltfläche](images/infobar-error-no-close.png)

### <a name="customization-background-color-and-icon"></a>Anpassung: Hintergrundfarbe und Symbol
Über die vordefinierten Schweregrade hinaus können die Eigenschaften „Background“ und „IconSource“ so festgelegt werden, dass das Symbol und die Hintergrundfarbe angepasst werden. Das InfoBar-Steuerelement behält die Einstellungen für die Hilfstechnologien des definierten Schweregrads bzw. wenn keine definiert wurden, die Standardeinstellungen bei.


Eine benutzerdefinierte Hintergrundfarbe kann über die Standardeigenschaft „Background“ festgelegt werden und überschreibt die durch „Severity“ festgelegte Farbe.
Berücksichtigen Sie beim Festlegen eigener Farben die Lesbarkeit und Barrierefreiheit von Inhalten.


Ein benutzerdefiniertes Symbol kann über die IconSource-Eigenschaft festgelegt werden. Standardmäßig ist ein Symbol sichtbar (vorausgesetzt, das Steuerelement ist nicht reduziert).
Sie können dieses Symbol entfernen, indem Sie die Eigenschaft „IsIconVisible“ auf „false“ festlegen. Bei benutzerdefinierten Symbolen beträgt die empfohlene Symbolgröße 20 px.


```xaml
<muxc:InfoBar x:Name="CallRecordingNotification"
    Title="Recording started"
    Message="Your call has begun recording."
    Background="{StaticResource LavenderBackgroundBrush}">
    <muxc:InfoBar.IconSource>
        <muxc:SymbolIconSource Symbol="Phone" />
    </muxc:InfoBar.IconSource>
</muxc:InfoBar>
```

![Beispiel eines InfoBar-Steuerelements im Standardzustand mit einer benutzerdefinierten Hintergrundfarbe, einem benutzerdefinierten Symbol und einer Schließen-Schaltfläche](images/infobar-custom-icon-color.png)

### <a name="add-an-action-button"></a>Hinzufügen einer Aktionsschaltfläche

Sie können eine zusätzliche Aktionsschaltfläche hinzufügen, indem Sie eine eigene Schaltfläche definieren, die [ButtonBase](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase) erbt, und diese in der Eigenschaft „ActionButton“ festlegen. Benutzerdefinierte Formatierungen werden auf Aktionsschaltflächen des Typs [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) und [HyperlinkButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton) angewendet, um Konsistenz und Barrierefreiheit zu gewährleisten. Neben der ActionButton-Eigenschaft können zusätzliche Aktionsschaltflächen über benutzerdefinierte Inhalte hinzugefügt werden und werden dann unterhalb der Meldung angezeigt.


```xaml
<muxc:InfoBar x:Name="NoInternetNotification"
    Severity="Error"
    Title="No Internet"
    Message="Reconnect to continue working.">
    <muxc:InfoBar.ActionButton>
        <Button Content="Network Settings" Click="InternetInfoBarButton_Click" />
    </muxc:InfoBar.ActionButton>
</muxc:InfoBar>
```

![Beispiel eines InfoBar-Steuerelements im Zustand „Fehler“ mit einer einzeiligen Meldung und einer Aktionsschaltfläche](images/infobar-error-action-button.png)

```xaml
<muxc:InfoBar x:Name="TermsAndConditionsUpdatedNotification"
    Title="Terms and Conditions Updated"
    Message="Dismissal of this message implies agreement to the updated Terms and Conditions. Please view the changes on our website.">
    <muxc:InfoBar.ActionButton>
        <HyperlinkButton Content="Terms and Conditions Sep 2020" 
            NavigateUri="https://www.example.com"
            Click="UpdateInfoBarHyperlinkButton_Click" />
    <muxc:InfoBar.ActionButton />
</muxc:InfoBar>
```
![Beispiel eines InfoBar-Steuerelements mit einer Meldung, die mehrere Zeilen und einen Link enthält](images/infobar-default-hyperlink.png)

### <a name="content-wrapping"></a>Umbrechen von Inhalten
Wenn der Inhalt des InfoBar-Steuerelements (mit Ausnahme benutzerdefinierter Inhalte) nicht in eine einzelne horizontale Zeile passt, wird er vertikal angeordnet. Die Steuerelemente „Title“, „Message“ und „ActionButton“ (falls vorhanden) werden jeweils in separaten Zeilen angezeigt.


```xaml
<muxc:InfoBar x:Name="BackupCompleteNotification"
    Severity="Success"
    Title="Backup complete: Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."  
    Message="All documents were uploaded successfully. Ultrices sagittis orci a scelerisque. Aliquet risus feugiat in ante metus dictum at tempor commodo. Auctor augue mauris augue neque gravida.">
    <muxc:InfoBar.ActionButton>
        <Button Content="Action"
            Click="BackupInfoBarButton_Click" />
    </muxc:InfoBar.ActionButton>
</muxc:InfoBar>
```

![Beispiel eines InfoBar-Steuerelements im Zustand „Erfolg“ mit mehrzeiligem Titel und einer Meldung](images/infobar-success-content-wrapping.png)


### <a name="custom-content"></a>Benutzerdefinierter Inhalt
XAML-Inhalt kann mithilfe der Content-Eigenschaft zu einem InfoBar-Steuerelement hinzugefügt werden. Er wird in einer eigenen Zeile unter dem restlichen Inhalt des Steuerelements angezeigt. Das InfoBar-Steuerelement wird so erweitert, dass es sich dem definierten Inhalt anpasst.


```xaml
<muxc:InfoBar x:Name="BackupInProgressNotification"
    Title="Backup in progress"  
    Message="Your documents are being saved to the cloud"
    IsClosable="False">
    <muxc:InfoBar.Content>
        <ProgressBar IsIndeterminate="True" Margin="0,0,0,6"/>
    </muxc:InfoBar.Content>
</muxc:InfoBar>
```

![Beispiel eines InfoBar-Steuerelements im Standardzustand mit einer unbestimmten Statusanzeige](images/infobar-default-custom-content.gif)

### <a name="lightweight-styling"></a>Einfache Formatierung

Sie können das Standardformat und das ControlTemplate-Steuerelement ändern, um dem Steuerelement ein einzigartiges Aussehen zu verleihen. Weitere Informationen finden Sie im Abschnitt [Einfache Formatierung](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-styles#lightweight-styling) im Artikel über die [Formatierung von Steuerelementen](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-styles).

Zum Beispiel bewirkt das Folgende, dass die Schriftgröße der Titelleiste der InfoBar-Steuerelemente einer Seite 22 pt beträgt:

```xaml
<Page.Resources>
    <x:Double x:Key="InfoBarTitleFontSize">22</x:Double>
</Page.Resources>
```
### <a name="canceling-close"></a>Abbrechen des Schließen-Ereignisses
Das Schließen-Ereignis kann verwendet werden, um das Schließen eines InfoBar-Steuerelements abzubrechen und/oder zu verzögern. Dadurch können Sie das InfoBar-Steuerelement offen lassen oder etwas Zeit für die Ausführung einer benutzerdefinierten Aktion einräumen. Wenn das Schließen eines InfoBar-Steuerelements abgebrochen wird, wird „IsOpen“ wieder auf „true“ festgelegt. Programmgesteuertes Schließen kann auch abgebrochen werden.


```xaml
<muxc:InfoBar x:Name="UpdateAvailable"
    Title="Update Available"
    Message="Please close this tip to apply required security updates to this application"
    Closing="InfoBar_Closing">
</muxc:InfoBar>
```

```csharp
public void InfoBar_Closing(InfoBar sender, InfoBarClosingEventArgs args)
{
    if (args.Reason == InfoBarCloseReason.CloseButton) {
        if (!ApplyUpdates()) {
            // could not apply updates - do not close
            args.Cancel = true;
        }
    }   
}
```

## <a name="recommendations"></a>Empfehlungen

### <a name="enter-and-exit-usability"></a>Benutzerfreundlichkeit von Eingabe und Beenden
#### <a name="flashing-content"></a>Blinkende Inhalte
Das InfoBar-Steuerelement sollte nicht schnell ein- und ausgeblendet werden, um ein Blinken auf dem Bildschirm zu vermeiden. Vermeiden Sie blinkende visuelle Objekte, um Menschen mit Lichtempfindlichkeit nicht zu beeinträchtigen und um die Benutzerfreundlichkeit Ihrer Anwendung zu verbessern.


Für InfoBar-Steuerelemente, die über eine App-Statusbedingung automatisch angezeigt werden und die Anzeige verlassen, sollten Sie Ihrer Anwendung eine Logik hinzufügen, um zu verhindern, dass Inhalte schnell oder mehrmals hintereinander ein- und ausgeblendet werden. Dieses Steuerelement sollte jedoch generell für langlebige Statusmeldungen verwendet werden.


#### <a name="updating-the-infobar"></a>Aktualisieren des InfoBar-Steuerelements
Sobald das Steuerelement geöffnet ist, wird durch Änderungen an den verschiedenen Eigenschaften, wie z. B. durch Aktualisieren der Meldung oder des Schweregrads, kein Benachrichtigungsereignis ausgelöst. Wenn Sie Benutzer, die Sprachausgaben verwenden, über den aktualisierten Inhalt des InfoBar-Steuerelements informieren möchten, empfehlen wir Ihnen, das Steuerelement zu schließen und erneut zu öffnen, um das Ereignis auszulösen.


#### <a name="inline-messages-offsetting-content"></a>Inlinemeldungen, die zum Versetzen von Inhalt führen
Bei InfoBar-Steuerelementen, die inline mit anderen Benutzeroberflächeninhalten angezeigt werden, sollten Sie bedenken, wie der Rest der Seite auf das Hinzufügen des Elements reagieren wird.


InfoBar-Steuerelemente mit einer großen Höhe könnten das Layout der übrigen Elemente auf der Seite erheblich verändern. Wenn das InfoBar-Steuerelement schnell, insbesondere nacheinander, angezeigt und ausgeblendet wird, kann dieser wechselnde visuelle Zustand den Benutzer verwirren.


### <a name="color-and-icon"></a>Farbe und Symbol
Wenn Sie die Farbe und das Symbol außerhalb der vordefinierten Schweregrade anpassen, berücksichtigen Sie die Erwartungen der Benutzer bezüglich der Konnotationen aus dem Satz der Standardsymbole und -farben.


Zusätzlich wurden die vordefinierten Farben für die Schweregrade bereits auf Designänderungen, den Modus mit hohem Kontrast, Farbverwechslungen, Barrierefreiheit und den Kontrast mit Vordergrundfarben abgestimmt. Wir empfehlen, wann immer möglich, diese Farben zu verwenden, und benutzerdefinierte Logik in Ihre Anwendung zu integrieren, um eine Anpassung an die verschiedenen Farbzustände und Barrierefreiheitsfeatures zu gewährleisten.


Schauen Sie sich die Anleitung zur Benutzerumgebung (UX) für [Standardsymbole](/windows/win32/uxguide/vis-std-icons) und [Farben](/windows/win32/uxguide/vis-color) an, um sicherzustellen, dass Ihre Botschaft Benutzern klar und verständlich vermittelt wird.


#### <a name="severity"></a>Schweregrad
 Legen Sie die Severity-Eigenschaft nicht für eine Benachrichtigung fest, die den im Titel, in der Nachricht oder in benutzerdefinierten Inhalten kommunizierten Informationen nicht entspricht.
 

 Um die nachfolgenden Schweregrade zu verwenden, sollten die zugehörigen Informationen sich auf entsprechende Inhalte beziehen.
 - Fehler: Einen Fehler oder ein Problem, der/das aufgetreten ist.
 - Warnung: Eine Bedingung, die in der Zukunft ein Problem verursachen kann.
 - Erfolg: Eine Aufgabe mit langer Ausführungszeit und/oder ein Hintergrundtask ist abgeschlossen.
 - Standardwert: Allgemeine Informationen, die die Aufmerksamkeit des Benutzers erfordern.

Symbole und Farben dürfen nicht die einzigen Komponenten der Benutzeroberfläche sein, die die Bedeutung ihrer Benachrichtigung veranschaulichen. Der Text im Titel und/oder in der Meldung der Benachrichtigung sollte zur Anzeige von Informationen enthalten sein.


### <a name="message"></a>Meldung 

Der Text in der Benachrichtigung verfügt über keine konstante Länge in allen Sprachen. Bei den Eigenschaften „Title“ und „Message“ kann sich dies darauf auswirken, ob Ihre Benachrichtigung auf eine zweite Zeile erweitert wird. Wir empfehlen, die Positionierung auf Basis der Nachrichtenlänge oder anderer Benutzeroberflächenelemente einer bestimmten Sprache zu vermeiden.

Die Benachrichtigung folgt dem Standardspiegelungsverhalten beim Lokalisieren in/aus Sprachen, die von rechts nach links (RTL) oder von links nach rechts (LTR) verlaufen. Das Symbol wird nur gespiegelt, wenn eine Richtung vorliegt.

Weitere Informationen zur Textlokalisierung in Ihrer Benachrichtigung finden Sie in der Anleitung zum [Anpassen von Layout und Schriftarten und Unterstützen von „Von rechts nach links“](/uwp/design/globalizing/adjust-layout-and-fonts--and-support-rtl).

## <a name="related-articles"></a>Verwandte Artikel

_ [Dialogfelder und Flyouts](./dialogs-and-flyouts/index.md)
