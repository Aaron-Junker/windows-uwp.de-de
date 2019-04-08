---
Description: Mit adaptiven und interaktiven Popupbenachrichtigungen können Sie flexible Popupbenachrichtigungen mit mehr Inhalt, optionalen Inlinebildern und optionaler Benutzerinteraktion erstellen.
title: Popupinhalt
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
label: Toast content
template: detail.hbs
ms.date: 11/20/2017
ms.topic: article
keywords: Windows 10, UWP, Popupbenachrichtigungen, interaktive Popups, adaptive Popups, Popup-Inhalt, Nutzlast des Popups
ms.localizationpriority: medium
ms.openlocfilehash: a75e39dfcddbef5bb5c37c2a253a46a7b9cc9577
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596825"
---
# <a name="toast-content"></a>Popupinhalt

Mit adaptiven und interaktiven Popupbenachrichtigungen können Sie flexible Benachrichtigungen mit Text, Bildern und Schaltflächen/Eingaben erstellen.

> **Wichtige APIs:** [NuGet-Paket für UWP-Community-Toolkit-Benachrichtigungen](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

> [!NOTE]
> Die älteren Vorlagen von Windows 8.1 und Windows Phone 8.1 finden Sie unter den [ältere popupvorlagenkatalog](https://msdn.microsoft.com/library/windows/apps/hh761494).


## <a name="getting-started"></a>Erste Schritte

**Installieren Sie Benachrichtigungen-Bibliothek.** Wenn Sie C# anstelle von XML verwenden möchten, um Benachrichtigungen zu generieren, installieren Sie das NuGet-Paket mit dem Namen [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/). (Suchen Sie nach „Benachrichtigungen UWP“.) Die in diesem Artikel bereitgestellten C#-Beispiele verwenden Version 1.0.0 des NuGet-Pakets.

**Installieren Sie Benachrichtigungen Schnellansicht.** Diese kostenlose UWP-App hilft Ihnen, interaktive Popupbenachrichtigungen zu entwerfen, indem sie während der Bearbeitung des Popups sofort eine Vorschau des Popups bereitstellen, ähnlich dem XAML-Editor/der Entwurfsansicht von Visual Studio. Weitere Informationen finden Sie unter [Notifications Visualizer](notifications-visualizer.md) oder [Notifications Visualizer aus dem Store herunterladen](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1).


## <a name="sending-a-toast-notification"></a>Senden einer Popupbenachrichtigung

Weitere Informationen zum Senden einer Benachrichtigung finden Sie unter [Senden einer lokalen Popupbenachrichtigung](send-local-toast.md). In dieser Dokumentation wird nur die Erstellung des Popupinhalts behandelt.


## <a name="toast-notification-structure"></a>Struktur der Popupbenachrichtigung

Popupbenachrichtigungen sind eine Kombination aus einigen Dateneigenschaften wie Tag/Group (mit denen Sie die Benachrichtigung identifizieren können) und dem *Popupinhalt*.

Die Kernkomponenten des Popupinhalts sind...
* **Starten Sie**: Hiermit wird definiert, welche Argumente werden zurück an Ihre app übergeben werden, klickt der Benutzer Ihre Toast, da Sie deep-Link in den richtigen Inhalt, den der Toast angezeigt. Weitere Informationen hierzu finden Sie unter [Senden einer lokalen Popupbenachrichtigung](send-local-toast.md).
* **Visual**: Der visuelle Teil den Toast, enthält die generische Bindung dar, einschließlich Text und Bilder.
* **Aktionen**: Der interaktive Teil den Toast, einschließlich der Eingaben und Aktionen.
* **Audio**: Steuerelemente werden das Audio abgespielt, wenn es sich bei der Toast, die dem Benutzer angezeigt wird.

Der Popupinhalt ist in XML-Rohdaten definiert, aber Sie können unsere [NuGet-Bibliothek](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) verwenden, um ein C# (oder C++)-Objektmodell für die Erstellung des Popupinhalts zu erhalten. In diesem Artikel werden alle Elemente des Popupinhalts dokumentiert.

```csharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric() { ... }
    },
 
    Actions = new ToastActionsCustom() { ... },
 
    Audio = new ToastAudio() { ... }
};
```

```xml
<toast launch="app-defined-string">

  <visual>
    <binding template="ToastGeneric">
      ...
    </binding>
  </visual>

  <actions>
    ...
  </actions>

  <audio src="ms-winsoundevent:Notification.Reminder"/>

</toast>
```

Hier sehen Sie eine visuelle Darstellung des Inhalts des Popups:

![Struktur der Popupbenachrichtigung](images/adaptivetoasts-structure.jpg)


## <a name="visual"></a>Visuelles Element

Jedes Popup muss ein visuelles Element angeben, in dem Sie eine generische Popupbindung angeben müssen, die Text, Bilder und vieles mehr enthalten kann. Diese Elemente werden auf verschiedenen Windows-Geräten wie Desktops, Smartphones, Tablets und Xbox wiedergegeben.

Alle Attribute, die im Abschnitt „Visuelle Elemente“ unterstützt werden, sowie die untergeordneten Elemente [finden Sie in der Schemadokumentation](toast-schema.md#toastvisual).

Die Identität Ihrer App in der Popupbenachrichtigung wird über Ihr App-Symbol angegeben. Wenn Sie jedoch die App-Logo-Überschreibung verwenden, wird der Name Ihrer App unterhalb der Textzeilen angezeigt.

| App-Identität für normale Popups | App-Identität mit appLogoOverride |
| -- | -- |
| <img src="images/adaptivetoasts-withoutapplogooverride.jpg" alt="notification without appLogoOverride" width="364"/> | <img alt="notification with appLogoOverride" src="images/adaptivetoasts-withapplogooverride.jpg" width="364"/> |


## <a name="text-elements"></a>Textelemente

Jedes Popup muss mindestens ein Textelement enthalten und kann zwei zusätzliche Textelemente enthalten, wobei alle den Typ [**AdaptiveText**](toast-schema.md#adaptivetext) aufweisen müssen.

<img alt="Toast with title and description" src="images/toast-title-and-description.jpg" width="364"/>

Seit dem Windows 10 Anniversary Update können Sie mithilfe der **HintMaxLines**-Eigenschaft für den Text steuern, wie viele Zeilen Text angezeigt werden. Der Standardwert (maximal) ist maximal 2 Zeilen Text für den Titel und maximal 4 Zeilen (kombiniert) für die zwei zusätzlichen Beschreibungselemente (der zweite und dritte **AdaptiveText**).

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        new AdaptiveText()
        {
            Text = "Adaptive Tiles Meeting",
            HintMaxLines = 1
        },

        new AdaptiveText()
        {
            Text = "Conf Room 2001 / Building 135"
        },

        new AdaptiveText()
        {
            Text = "10:00 AM - 10:30 AM"
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    <text hint-maxLines="1">Adaptive Tiles Meeting</text>
    <text>Conf Room 2001 / Building 135</text>
    <text>10:00 AM - 10:30 AM</text>
</binding>
```


## <a name="app-logo-override"></a>Überschreibung des App-Logos

Standardmäßig zeigt das Popup Ihr App-Logo an. Allerdings können Sie dieses Logo mit Ihrem eigenen [**ToastGenericAppLogo**](toast-schema.md#toastgenericapplogo)-Bild überschreiben. Wenn es sich beispielsweise um eine Benachrichtigung von einer Person handelt, empfehlen wir, das App-Logo mit einem Bild der betreffenden Person zu überschreiben.

<img alt="Toast with app logo override" src="images/toast-applogooverride.jpg" width="364"/>

Sie können die **HintCrop**-Eigenschaft verwenden, um den Zuschnitt des Bilds zu ändern. So ergibt **Kreis** z. B. ein kreisförmig zugeschnittenes Bild. Andernfalls ist das Bild quadratisch. Bildabmessungen sind 48 x 48 Pixel bei einer Skalierung von 100 %.

```csharp
new ToastBindingGeneric()
{
    ...

    AppLogoOverride = new ToastGenericAppLogo()
    {
        Source = "https://picsum.photos/48?image=883",
        HintCrop = ToastGenericAppLogoCrop.Circle
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="appLogoOverride" hint-crop="circle" src="https://picsum.photos/48?image=883"/>
</binding>
```


## <a name="hero-image"></a>Favoritenbild

**Im Anniversary Update**: Popups können anzeigen, eine Hero-Bild, d.h. eine empfohlene [ **ToastGenericHeroImage** ](toast-schema.md#toastgenericheroimage) angezeigte an gut sichtbarer Stelle innerhalb der popupbanner und während Info-Center. Bildabmessungen sind 364 x 180 Pixel bei einer Skalierung von 100 %.

<img alt="Toast with hero image" src="images/toast-heroimage.jpg" width="364"/>

```csharp
new ToastBindingGeneric()
{
    ...

    HeroImage = new ToastGenericHeroImage()
    {
        Source = "https://picsum.photos/364/180?image=1043"
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="hero" src="https://picsum.photos/364/180?image=1043"/>
</binding>
```


## <a name="inline-image"></a>Inline-Bild

Sie können ein Inline-Bild in voller Breite bereitstellen, das angezeigt wird, wenn das Popup erweitert wird.

<img alt="Toast with additional image" src="images/toast-additionalimage.jpg" width="364"/>

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        ...

        new AdaptiveImage()
        {
            Source = "https://picsum.photos/360/202?image=1043"
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image src="https://picsum.photos/360/202?image=1043" />
</binding>
```


## <a name="image-size-restrictions"></a>Größenbeschränkung

Die Bilder, die Sie in Ihre Popupbenachrichtigung verwenden, können von folgenden Quellen kommen...

 - http://
 - ms-appx:///
 - ms-appdata:///

Für http und https/Remotewebbilder gibt es Dateigrößenbeschränkungen für jedes einzelne Bild. Im Fall Creators Update (16299) erhöhten wir die Beschränkung auf 3 MB für normale Verbindungen und 1 MB für getaktete Verbindungen. Davor waren Bilder immer auf 200 KB begrenzt.

| Normale Verbindung | Getaktete Verbindung | Vor dem Fall Creators Update |
| - | - | - |
| 3 MB | 1 MB | 200 KB |

Wenn ein Bild die Dateigröße überschreitet oder nicht herunterladbar ist oder ein Timeout hervorruft, wird das Bild verworfen, und der Rest der Benachrichtigung wird angezeigt.


## <a name="attribution-text"></a>Zuschreibungstext

**Im Anniversary Update**: Wenn Sie die Quelle Ihrer Inhalte verweisen müssen, können Sie Attribution Text. Dieser Text wird zusammen mit der Identität Ihrer App oder dem Zeitstempel der Benachrichtigung immer am unteren Rand der Benachrichtigung angezeigt.

Für ältere Versionen von Windows, die Zuschreibungstext nicht unterstützen, wird der Text einfach als weiteres Textelement angezeigt (sofern Sie nicht bereits die maximalen drei Textelemente verwenden).

<img alt="Toast with attribution text" src="images/toast-attributiontext.jpg" width="364"/>

```csharp
new ToastBindingGeneric()
{
    ...

    Attribution = new ToastGenericAttributionText()
    {
        Text = "Via SMS"
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <text placement="attribution">Via SMS</text>
</binding>
```


## <a name="custom-timestamp"></a>Benutzerdefinierter Zeitstempel

**Creators Update neues**: Sie können jetzt den Zeitstempel vom System bereitgestellten mit Ihren eigenen Zeitstempel überschreiben, die genau darstellt, wenn der Inhalt der Nachricht/Informationen/generiert wurde. Dieser Zeitstempel wird im Info-Center angezeigt.

<img alt="Toast with custom timestamp" src="images/toast-customtimestamp.jpg" width="396"/>

Weitere Informationen zum Verwenden eines benutzerdefinierten Zeitstempels [benutzerdefinierten Zeitstempel für Popups](custom-timestamps-on-toasts.md).

```csharp
ToastContent toastContent = new ToastContent()
{
    DisplayTimestamp = new DateTime(2017, 04, 15, 19, 45, 00, DateTimeKind.Utc),
    ...
};
```

```xml
<toast displayTimestamp="2017-04-15T19:45:00Z">
  ...
</toast>
```


## <a name="progress-bar"></a>Statusleiste

**Creators Update neues**: Sie können eine Statusanzeige in Ihrer Toast-Benachrichtigung der Benutzer über den Status der Vorgänge, z. B. Downloads und vieles mehr zu bereitstellen.

<img alt="Toast with progress bar" src="images/toast-progressbar.png" width="364"/>

Weitere Informationen zur Verwendung einer Statusanzeige finden Sie unter [Popup-Statusanzeige](toast-progress-bar.md).


## <a name="headers"></a>Header

**Creators Update neues**: Sie können Benachrichtigungen unter Header im Info-Centers gruppieren. Beispielsweise können Sie einen Gruppenchat unter einem Header zusammenfassen, oder einer Gruppenbenachrichtigungen unter einem Header zusammenfassen oder mehr.

<img alt="Toasts with header" src="images/toast-headers-action-center.png" width="396"/>

Weitere Informationen zum Verwenden von Headern finden Sie unter [Toast headers](toast-headers.md).


## <a name="adaptive-content"></a>Adaptiver Inhalt

**Im Anniversary Update**: Zusätzlich zu den oben angegebenen Inhalt können Sie auch zusätzlichen adaptiven Inhalte anzeigen, der angezeigt wird, wenn der Toast erweitert wird.

Dieser zusätzliche Inhalt wird mit Adaptive angegeben. Mehr zu diesem Thema erfahren Sie in der [Dokumentation zu adaptiven Kacheln](create-adaptive-tiles.md).

Beachten Sie, dass jeglicher adaptiver Inhalt in einer [**AdaptiveGroup**](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/toast-schema#adaptivegroup) enthalten sein müssen. Andernfalls wird der nicht mit Adaptive gerendert.


### <a name="columns-and-text-elements"></a>Spalten und Textelemente

Hier ist ein Beispiel, in dem Spalten und einige erweiterte adaptive Textelemente verwendet werden. Da die Textelemente in einer **AdaptiveGroup** enthalten sind, unterstützen sie alle erweiterten adaptiven Stileigenschaften.

<img alt="Toast with additional text" src="images/toast-additionaltext.jpg" width="364"/>

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        ...

        new AdaptiveGroup()
        {
            Children =
            {
                new AdaptiveSubgroup()
                {
                    Children =
                    {
                        new AdaptiveText()
                        {
                            Text = "52 attendees",
                            HintStyle = AdaptiveTextStyle.Base
                        },
                        new AdaptiveText()
                        {
                            Text = "23 minute drive",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle
                        }
                    }
                },
                new AdaptiveSubgroup()
                {
                    Children =
                    {
                        new AdaptiveText()
                        {
                            Text = "1 Microsoft Way",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle,
                            HintAlign = AdaptiveTextAlign.Right
                        },
                        new AdaptiveText()
                        {
                            Text = "Bellevue, WA 98008",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle,
                            HintAlign = AdaptiveTextAlign.Right
                        }
                    }
                }
            }
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <group>
        <subgroup>
            <text hint-style="base">52 attendees</text>
            <text hint-style="captionSubtle">23 minute drive</text>
        </subgroup>
        <subgroup>
            <text hint-style="captionSubtle" hint-align="right">1 Microsoft Way</text>
            <text hint-style="captionSubtle" hint-align="right">Bellevue, WA 98008</text>
        </subgroup>
    </group>
</binding>
```


## <a name="buttons"></a>Schaltflächen

Schaltflächen machen Ihr Popup interaktiv. Sie erlauben dem Benutzer, in Ihrer Popupbenachrichtigung schnelle Aktionen auszuführen, ohne den aktuellen Workflow zu unterbrechen. Benutzer können z. B. eine Nachricht direkt in einem Popup beantworten oder eine E-Mail löschen, ohne die E-Mail-App überhaupt zu öffnen. Schaltflächen werden im erweiterten Teil der Benachrichtigung angezeigt.

Weitere Informationen zur Implementierung von Schaltflächen End-to-End finden Sie unter [Lokale Popups senden](send-local-toast.md).

Schaltflächen können die folgenden verschiedenen Aktionen ausführen...

-   Aktivieren der App im Vordergrund mit einem Argument, das zum Navigieren zu einer bestimmten Seite bzw. einem bestimmten Kontext verwendet werden kann
-   Aktivieren der Hintergrundaufgabe der App für eine schnelle Antwort oder ein ähnliches Szenario
-   Aktivieren einer anderen App per Protokollstart.
-   Durchführen einer Systemaktion, z. B. erneute Erinnerung oder Schließen der Benachrichtigung

> [!NOTE]
> Sie können nur bis zu 5 Schaltflächen haben (einschließlich Elementen des Kontextmenüs, die später erläutert werden).

<img alt="notification with actions, example 1" src="images/adaptivetoasts-xmlsample02.jpg" width="364"/>

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Buttons =
        {
            new ToastButton("See more details", "action=viewdetails&contentId=351")
            {
                ActivationType = ToastActivationType.Foreground
            },

            new ToastButton("Remind me later", "action=remindlater&contentId=351")
            {
                ActivationType = ToastActivationType.Background
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <action
            content="See more details"
            arguments="action=viewdetails&amp;contentId=351"
            activationType="foreground"/>

        <action
            content="Remind me later"
            arguments="action=remindlater&amp;contentId=351"
            activationType="background"/>

    </actions>

</toast>
```


### <a name="buttons-with-icons"></a>Schaltflächen mit Symbolen

Sie können Ihren Schaltflächen Symbole hinzufügen. Diese Symbole sind weiße, transparente und 16 x 16 Pixel große Bilder mit einer Skalierung von 100 %. Sie sollten keinen Abstand enthalten in dem Bild selbst enthalten. Wenn Sie Symbole auf eine Popupbenachrichtigung bereitstellen, müssen Sie die Symbole für alle Schaltflächen in der Benachrichtigung bereitstellen, da es den Stil der Schaltflächen in der Symbolschaltflächen umwandelt.

> [!NOTE]
> Fügen Sie für mehr Barrierefreiheit eine Version mit Kontrast (weiß) für das Symbol hinzu (ein schwarzes Symbol auf weißem Hintergrund): Wenn der Benutzer den Modus „Hoher Kontrast (Weiß)“ aktiviert, wird das Symbol angezeigt wird. Erfahren Sie mehr auf der [Seite für Popup-Bedienungshilfen](tile-toast-language-scale-contrast.md).

<img src="images\adaptivetoasts-buttonswithicons.png" width="364" alt="Toast that has buttons with icons"/>

```csharp
new ToastButton("Dismiss", "dismiss")
{
    ActivationType = ToastActivationType.Background,
    ImageUri = "Assets/ToastButtonIcons/Dismiss.png"
}
```


```xml
<action
    content="Dismiss"
    imageUri="Assets/ToastButtonIcons/Dismiss.png"
    arguments="dismiss"
    activationType="background"/>
```


### <a name="buttons-with-pending-update-activation"></a>Schaltflächen mit ausstehenden Updates in Aktion

**Neues in Fall Creators Update**: Für Schaltflächen die Hintergrund-Aktivierung, können Sie eine nach Aktivierung Verhalten **"pendingupdate"** zum Erstellen von Aktivitäten, die mit mehreren Schritten in Ihrer Popupbenachrichtigungen. Wenn der Benutzer die Schaltfläche anklickt, wird die Hintergrundaufgabe aktiviert und das Popup wird in den Zustand "ausstehendes Update" versetzt, ein Zustand, in dem es auf dem Bildschirm bleibt, bis die Hintergrundaufgabe das Popup durch eine neue Popupbenachrichtigung ersetzt wird.

Informationen zur Implementierung finden Sie unter [ausstehende Updates für Popups](toast-pending-update.md).

![Popup mit ausstehendem Update](images/toast-pendingupdate.gif)


### <a name="context-menu-actions"></a>Kontextmenüaktionen

**Im Anniversary Update**: Sie können zusätzliche Kontextmenü verfügbaren Aktionen, die die vorhandenen Kontextmenü hinzufügen, die angezeigt wird, klickt der Benutzer rechten Ihrer Toast aus im Info-Center. Beachten Sie, dass dieses Menü nur angezeigt wird, wenn der Benutzer mit der rechten Maustaste auf das Info-Center klickt. Es wird nicht angezeigt, wenn der Benutzer mit der rechten Maustaste auf ein Popup-Banner klickt.

> [!NOTE]
> Bei älteren Geräten werden diese zusätzlichen Kontextmenüaktionen einfach als normale Schaltflächen im Popup angezeigt.

Die zusätzlichen Kontextmenüaktionen, die Sie hinzufügen (wie. "Pfad ändern"), werden über die zwei standardmäßigen Systemeinträge angezeigt.

<img alt="Toast with context menu" src="images/toast-contextmenu.png" width="444"/>

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        ContextMenuItems =
        {
            new ToastContextMenuItem("Change location", "action=changeLocation")
        }
    }
};
```

```xml
<toast>

    ...

    <actions>

        <action
            placement="contextMenu"
            content="Change location"
            arguments="action=changeLocation"/>

    </actions>

</toast>
```

> [!NOTE]
> Zusätzliche Kontextmenüelemente tragen zu dem Gesamtlimit von 5 Schaltflächen für eine Popupbenachrichtigung bei.

Die Aktivierung der zusätzlichen Menüelementkontexte ist identisch mit den Schaltflächen für die Popupbenachrichtigungen.


## <a name="inputs"></a>Eingaben

Eingaben werden innerhalb des Bereichs „Aktionen“ der Popupregion des Popups angegeben. Das bedeutet, dass sie nur angezeigt werden, wenn das Popup erweitert wird.


### <a name="quick-reply-text-box"></a>Textfeld für schnelle Antworten

Um ein Textfeld für schnelle Antworten – etwa für ein Nachrichten-Szenario – zu aktivieren, fügen Sie eine Texteingabe und eine Schaltfläche hinzu, und verweisen Sie auf die ID der Texteingabe, damit die Schaltfläche neben der Eingabe angezeigt wird.

<img alt="notification with text input and actions" src="images/adaptivetoasts-xmlsample05.jpg" width="364"/>

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("tbReply")
            {
                PlaceholderContent = "Type a reply"
            }
        },

        Buttons =
        {
            new ToastButton("Reply", "action=reply&convId=9318")
            {
                ActivationType = ToastActivationType.Background,

                // To place the button next to the text box,
                // reference the text box's Id and provide an image
                TextBoxId = "tbReply",
                ImageUri = "Assets/Reply.png"
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="textBox" type="text" placeHolderContent="Type a reply"/>

        <action
            content="Send"
            arguments="action=reply&amp;convId=9318"
            activationType="background"
            hint-inputId="textBox"
            imageUri="Assets/Reply.png"/>

    </actions>

</toast>
```


### <a name="inputs-with-buttons-bar"></a>Eingaben mit Schaltflächenleiste

Es können auch eine oder mehrere Eingaben mit normalen Schaltflächen unterhalb der Eingaben angezeigt werden.

<img alt="notification with text and input actions" src="images/adaptivetoasts-xmlsample04.jpg" width="364"/>

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("tbReply")
            {
                PlaceholderContent = "Type a reply"
            }
        },

        Buttons =
        {
            new ToastButton("Reply", "action=reply&threadId=9218")
            {
                ActivationType = ToastActivationType.Background
            },

            new ToastButton("Video call", "action=videocall&threadId=9218")
            {
                ActivationType = ToastActivationType.Foreground
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="textBox" type="text" placeHolderContent="Type a reply"/>

        <action
            content="Reply"
            arguments="action=reply&amp;threadId=9218"
            activationType="background"/>

        <action
            content="Video call"
            arguments="action=videocall&amp;threadId=9218"
            activationType="foreground"/>

    </actions>

</toast>
```


### <a name="selection-input"></a>Auswahleingabe

Zusätzlich zu Textfeldern können Sie auch ein Auswahlmenü verwenden.

<img alt="notification with selection input and actions" src="images/adaptivetoasts-xmlsample06.jpg" width="364"/>

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("time")
            {
                DefaultSelectionBoxItemId = "lunch",
                Items =
                {
                    new ToastSelectionBoxItem("breakfast", "Breakfast"),
                    new ToastSelectionBoxItem("lunch", "Lunch"),
                    new ToastSelectionBoxItem("dinner", "Dinner")
                }
            }
        },

        Buttons = { ... }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="time" type="selection" defaultInput="lunch">
            <selection id="breakfast" content="Breakfast" />
            <selection id="lunch" content="Lunch" />
            <selection id="dinner" content="Dinner" />
        </input>

        ...

    </actions>

</toast>
```


### <a name="snoozedismiss"></a>Erneutes Erinnern/Schließen

Mithilfe eines Auswahlmenüs und zwei Schaltflächen können wir eine Erinnerungsbenachrichtigung erstellen, welche die Systemaktionen zum erneuten Erinnern und Schließen verwendet. Legen Sie unbedingt das Szenario für die Benachrichtigung auf „Erinnerung“ fest, damit sie sich wie eine Erinnerung verhält.

<img alt="reminder notification" src="images/adaptivetoasts-xmlsample07.jpg" width="364"/>

Wir verknüpfen die Schaltfläche „Erneut erinnern“ mithilfe der **SelectionBoxId**-Eigenschaft der Popupschaltfläche mit der Auswahlmenüeingabe.

```csharp
ToastContent content = new ToastContent()
{
    Scenario = ToastScenario.Reminder,

    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("snoozeTime")
            {
                DefaultSelectionBoxItemId = "15",
                Items =
                {
                    new ToastSelectionBoxItem("5", "5 minutes"),
                    new ToastSelectionBoxItem("15", "15 minutes"),
                    new ToastSelectionBoxItem("60", "1 hour"),
                    new ToastSelectionBoxItem("240", "4 hours"),
                    new ToastSelectionBoxItem("1440", "1 day")
                }
            }
        },

        Buttons =
        {
            new ToastButtonSnooze()
            {
                SelectionBoxId = "snoozeTime"
            },
 
            new ToastButtonDismiss()
        }
    }
};
```

```xml
<toast scenario="reminder" launch="action=viewEvent&amp;eventId=1983">
   
  ...
 
  <actions>
     
    <input id="snoozeTime" type="selection" defaultInput="15">
      <selection id="1" content="1 minute"/>
      <selection id="15" content="15 minutes"/>
      <selection id="60" content="1 hour"/>
      <selection id="240" content="4 hours"/>
      <selection id="1440" content="1 day"/>
    </input>
 
    <action activationType="system" arguments="snooze" hint-inputId="snoozeTime" content="" />
 
    <action activationType="system" arguments="dismiss" content=""/>
     
  </actions>
   
</toast>
```

Gehen Sie wie folgt vor, um die Systemaktionen zum erneuten Erinnern und Schließen zu verwenden:

-   Angeben eines **ToastButtonSnooze** oder **ToastButtonDismiss**
-   Geben Sie optional eine benutzerdefinierte Inhaltszeichenfolge an:
    -   Wenn Sie keine Zeichenfolge angeben, verwenden wir automatisch lokalisierte Zeichenfolgen für "Erneut erinnern" und "Verwerfen".
-   Geben Sie optional die **SelectionBoxId** an:
    -   Wenn Sie nicht möchten, dass der Benutzer ein Intervall für das erneute Erinnern auswählen kann, sondern das erneute Erinnern an die Benachrichtigung nur einmal in einem vom System definierten (in allen Betriebssystemen einheitlichen) Zeitintervall erfolgt, legen Sie keinen Wert für &lt;input&gt; fest.
    -   Wenn Sie mögliche Intervalle für das erneute Erinnern bereitstellen möchten:
        -   Geben Sie **SelectionBoxId** in der Aktion für das erneute Erinnern an
        -   Stimmen Sie die ID der Eingabe auf den Wert für **SelectionBoxId** der Aktion für das erneute Erinnern ab
        -   Legen Sie für den Wert von **ToastSelectionBoxItem** eine positive ganze Zahl (nonNegativeInteger) fest, die dem Intervall für das erneute Erinnern in Minuten entspricht.



## <a name="audio"></a>Audio

Benutzerdefinierte Audioeffekte wurden schon immer von Mobile unterstützt und wird in der Desktop-Version 1511 (Build 10586) oder einer neueren Version unterstützt. Benutzerdefinierte Audioeffekte können über die folgenden Pfade verwiesen werden:

-   ms-appx:///
-   ms-appdata:///

Sie können alternativ aus der [Liste "ms-winsoundevents"](/uwp/schemas/tiles/toastschema/element-audio#attributes-and-elements) auswählen, welche bisher immer auf beiden Plattformen unterstützt wurden.

```csharp
ToastContent content = new ToastContent()
{
    ...

    Audio = new ToastAudio()
    {
        Src = new Uri("ms-appx:///Assets/NewMessage.mp3")
    }
}
```

```xml
<toast launch="app-defined-string">

    ...

    <audio src="ms-appx:///Assets/NewMessage.mp3"/>

</toast>
```

Erhalten Sie unter der [Seite](/uwp/schemas/tiles/toastschema/element-audio) Informationen zu Töne für Popupbenachrichtigungen. Informationen für das Senden einer Popupbenachrichtigung mit benutzerdefinierten Audioeffekten finden Sie unter [benutzerdefiniertes Audio auf Popups](custom-audio-on-toasts.md).


## <a name="alarms-reminders-and-incoming-calls"></a>Alarme, Erinnerungen und eingehende Anrufe

Um Alarme, Erinnerungen und Benachrichtigungen über eingehende Anrufe zu erstellen, verwenden Sie einfach eine normale Popupbenachrichtigung mit einem zugewiesenen Szenariowert. Das Szenario umfasst einige Verhaltensweisen, um eine konsistente und einheitliche Benutzererfahrung zu schaffen.

> [!IMPORTANT]
> Wenn Sie Alarme oder Erinnerungen verwenden, müssen Sie mindestens eine Schaltfläche auf Ihrer Popupbenachrichtigung angeben. Andernfalls wird das Popup als ein normales Popup behandelt.

* **Erinnerung**: Die Benachrichtigung, auf dem Bildschirm verbleibt, bis der Benutzer es schließt oder Maßnahmen ergreift. Unter Windows Mobile wird das Popup auch vorab vergrößert angezeigt. Ein Erinnerungston wird wiedergegeben.
* **Alarm**: Zusätzlich zu das Verhalten Erinnerung durchlaufen Alarme darüber Audio eine Standard-Alarm Sound.
* **IncomingCall**: Benachrichtigungen über eingehende Anrufe werden auf Windows Mobile-Geräten im Vollbildmodus angezeigt. Andernfalls weisen sie die gleichen Verhaltensweisen wie Alarme auf, außer dass sie einen Klingelton verwenden und die Schaltflächen anders aussehen.

```csharp
ToastContent content = new ToastContent()
{
    Scenario = ToastScenario.Reminder,

    ...
}
```

```xml
<toast scenario="reminder" launch="app-defined-string">

    ...

</toast>
```


## <a name="localization-and-accessibility"></a>Lokalisierung und Bedienungshilfen

Ihre Kacheln und Popups können Zeichenfolgen und Bilder laden, die speziell auf die Sprache, den Skalierungsfaktor für die Anzeige, das Design, den hohen Kontrast und anderen Laufzeitkontexte angepasst wurden. Weitere Informationen finden Sie unter [Unterstützte Kachel- und Popupbenachrichtigungen für Sprache, Skalierungsfaktor und hohen Kontrast](tile-toast-language-scale-contrast.md)


## <a name="handling-activation"></a>Behandeln der Aktivierung
Informationen dazu, wie Sie Popupaktivierungen behandeln (der Benutzer klickt auf Ihr Popup oder auf Schaltflächen im Popup), finden Sie unter [Senden einer lokalen Popupbenachrichtigung](send-local-toast.md).
 
## <a name="related-topics"></a>Verwandte Themen

* [Senden Sie eine lokale Popup- und Handle-Aktivierung](send-local-toast.md)
* [Benachrichtigungsbibliothek auf GitHub (Teil von das UWP Community Toolkit)](https://github.com/Microsoft/UWPCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
* [Kachel- und popupbildgrößen benachrichtigungsunterstützung für Sprache, Skalierung und hoher Kontrast](tile-toast-language-scale-contrast.md)
