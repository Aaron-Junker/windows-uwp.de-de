---
Description: Mit adaptiven und interaktiven Popupbenachrichtigungen können Sie flexible Popupbenachrichtigungen mit mehr Inhalt, optionalen Inlinebildern und optionaler Benutzerinteraktion erstellen.
title: Popupinhalt
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
label: Toast content
template: detail.hbs
ms.date: 11/20/2017
ms.topic: article
keywords: Windows 10, UWP, Popup Benachrichtigungen, interaktive Toasts, Adaptive Toasts, Popup Inhalt, Toast Nutzlast
ms.localizationpriority: medium
ms.openlocfilehash: 97dd16d712dca3de69a98c608b7c8947ebbddfea
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173364"
---
# <a name="toast-content"></a>Popupinhalt

Mithilfe von adaptiven und interaktiven Popup Benachrichtigungen können Sie flexible Benachrichtigungen mit Text, Bildern und Schaltflächen/Eingaben erstellen.

> **Wichtige APIs:** [NuGet-Paket für UWP-Community-Toolkit-Benachrichtigungen](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

> [!NOTE]
> Die Legacy Vorlagen aus Windows 8.1 und Windows Phone 8,1 finden Sie im Katalog mit der Legacy-Popup [Vorlage](/previous-versions/windows/apps/hh761494(v=win.10)).


## <a name="getting-started"></a>Erste Schritte

**Installieren Sie die Benachrichtigungsbibliothek.** Wenn Sie C# anstelle von XML verwenden möchten, um Benachrichtigungen zu generieren, installieren Sie das NuGet-Paket mit dem Namen [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/). (Suchen Sie nach „Benachrichtigungen UWP“.) Die in diesem Artikel bereitgestellten C#-Beispiele verwenden Version 1.0.0 des NuGet-Pakets.

**Installieren Sie den Notifications Visualizer.** Diese kostenlose Windows-App hilft Ihnen beim Entwerfen interaktiver Popup Benachrichtigungen, indem Sie während der Bearbeitung eine sofortige visuelle Vorschau Ihres Popups bereitstellen, ähnlich wie der XAML-Editor/die Entwurfs Ansicht von Visual Studio. Weitere Informationen finden Sie unter [Benachrichtigungs](notifications-visualizer.md) Schnellansicht, oder [Laden Sie die Benachrichtigungs Schnellansicht aus dem Store herunter](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1).


## <a name="sending-a-toast-notification"></a>Senden einer Popup Benachrichtigung

Informationen zum Senden von Benachrichtigungen finden Sie unter [Send local Toast](send-local-toast.md). In dieser Dokumentation wird nur das Erstellen des Popup Inhalts behandelt.


## <a name="toast-notification-structure"></a>Struktur der Popupbenachrichtigung

Popup Benachrichtigungen sind eine Kombination aus einigen Daten Eigenschaften wie Tag/Gruppe (mit denen die Benachrichtigung identifiziert werden können) und dem Popup *Inhalt*.

Die Kernkomponenten von Popup Inhalten sind...
* **Launch**: definiert, welche Argumente an die APP zurückgegeben werden, wenn der Benutzer auf den Popup klickt, sodass Sie tief in den richtigen Inhalt, den der Popup anzeigt, verweisen können. Weitere Informationen finden Sie unter [Send local Toast](send-local-toast.md).
* **Visual**: der visuelle Teil des Popups, einschließlich der generischen Bindung, die Text und Bilder enthält.
* **Aktionen**: der interaktive Teil des Toast, einschließlich Eingaben und Aktionen.
* **Audiodatei**: steuert die Audiowiedergabe, wenn dem Benutzer der Popup angezeigt wird.

Der Popup Inhalt ist in unformatiertem XML definiert, aber Sie können unsere [nuget-Bibliothek](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) verwenden, um ein c#-Objektmodell (oder ein C++-Objekt) zum Erstellen des Popup Inhalts zu erhalten. In diesem Artikel wird alles dokumentiert, was innerhalb des Popup Inhalts liegt.

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

Hier sehen Sie eine visuelle Darstellung des Popup Inhalts:

![Struktur der Popupbenachrichtigung](images/adaptivetoasts-structure.jpg)


## <a name="visual"></a>Grafisches Element

Jeder Toast muss ein visuelles Element angeben, in dem Sie eine generische Popup Bindung bereitstellen müssen, die Text, Bilder und mehr enthalten kann. Diese Elemente werden auf verschiedenen Windows-Geräten, einschließlich Desktop, Smartphones, Tablets und Xbox, gerendert.

Informationen zu allen Attributen, die im visuellen Abschnitt und seinen untergeordneten Elementen unterstützt werden, finden Sie in [der Schema Dokumentation](toast-schema.md#toastvisual).

Die Identität ihrer App auf der Popup Benachrichtigung wird über das App-Symbol übermittelt. Wenn Sie jedoch das App-Logo außer Kraft setzen, wird Ihr App-Name unter ihren Textzeilen angezeigt.

| App-Identität für den normalen Toast | App-Identität mit applogooverride |
| -- | -- |
| <img src="images/adaptivetoasts-withoutapplogooverride.jpg" alt="notification without appLogoOverride" width="364"/> | <img alt="notification with appLogoOverride" src="images/adaptivetoasts-withapplogooverride.jpg" width="364"/> |


## <a name="text-elements"></a>Text Elemente

Jeder Toast muss mindestens ein Textelement enthalten und kann zwei zusätzliche Textelemente enthalten, die alle den Typ " [**adaptivetext**](toast-schema.md#adaptivetext)" aufweisen.

<img alt="Toast with title and description" src="images/toast-title-and-description.jpg" width="364"/>

Seit dem Windows 10 Anniversary Update können Sie steuern, wie viele Textzeilen angezeigt werden, indem Sie die **hintmaxlines** -Eigenschaft für den Text verwenden. Der Standardwert (und der Höchstwert) beträgt bis zu zwei Textzeilen für den Titel und bis zu 4 Zeilen (kombiniert) für die beiden zusätzlichen Beschreibungs Elemente (der zweite und dritte **adaptivetext**).

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


## <a name="app-logo-override"></a>App-Logo Überschreibung

Standardmäßig wird Ihr App-Logo im Toast angezeigt. Allerdings können Sie dieses Logo mit Ihrem eigenen "Image Image [**capplogo**](toast-schema.md#toastgenericapplogo) "-Image überschreiben. Wenn es sich beispielsweise um eine Benachrichtigung von einer Person handelt, empfiehlt es sich, das App-Logo mit einem Bild dieser Person zu überschreiben.

<img alt="Toast with app logo override" src="images/toast-applogooverride.jpg" width="364"/>

Sie können die **hintcrop** -Eigenschaft verwenden, um das Abschneiden des Bilds zu ändern. " **Circle** " führt beispielsweise zu einem Zirkel gebundenen Bild. Andernfalls ist das Bild quadratisch. Bild Dimensionen sind 48 x 48 Pixel bei einer Skalierung von 100%.

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


## <a name="hero-image"></a>Herabbild

**Neu in Anniversary Update**: die Toasts können ein Herausgeber Bild anzeigen. Hierbei handelt es sich um ein geenderetes [**toastgenericheroimage**](toast-schema.md#toastgenericheroimage) , das hervorgehoben im Popup Banner und im Aktions Center angezeigt wird. Bild Dimensionen sind 364x 180 Pixel bei der Skalierung von 100%.

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


## <a name="inline-image"></a>Inline Bild

Sie können ein Inline Bild mit voller Breite bereitstellen, das angezeigt wird, wenn Sie den Toast erweitern.

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


## <a name="image-size-restrictions"></a>Einschränkungen bei der Image Größe

Die Images, die Sie in ihrer Popup Benachrichtigung verwenden, können aus stammen...

 - http://
 - ms-appx:///
 - ms-appdata:///

Für http-und HTTPS-Remoteweb Images gibt es Beschränkungen hinsichtlich der Dateigröße jedes einzelnen Bilds. Im Fall Creators Update (16299) haben wir den Grenzwert bei normalen Verbindungen um 3 MB und bei getakteten Verbindungen um 1 MB verlängert. Zuvor waren Bilder immer auf 200 KB beschränkt.

| Normale Verbindung | Getaktete Verbindung | Vor Fall Creators Update |
| - | - | - |
| 3 MB | 1 MB | 200 KB |

Wenn ein Bild die Dateigröße überschreitet oder nicht heruntergeladen werden kann oder ein Timeout auftritt, wird das Image gelöscht, und der Rest der Benachrichtigung wird angezeigt.


## <a name="attribution-text"></a>Zuweisungs Text

**Neu in Anniversary Update**: Wenn Sie auf die Quelle ihres Inhalts verweisen müssen, können Sie den Zuweisungs Text verwenden. Dieser Text wird immer am Ende der Benachrichtigung zusammen mit der Identität ihrer APP oder dem Zeitstempel der Benachrichtigung angezeigt.

In älteren Versionen von Windows, die keinen Zuweisungs Text unterstützen, wird der Text einfach als anderes Textelement angezeigt (vorausgesetzt, Sie haben nicht bereits die maximal drei Textelemente).

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

**Neues in Creators Update**: Sie können nun den vom System bereitgestellten Zeitstempel mit Ihrem eigenen Zeitstempel überschreiben, der genau angibt, wann die Nachricht/Informationen/der Inhalt generiert wurde. Dieser Zeitstempel ist innerhalb des Aktions Centers sichtbar.

<img alt="Toast with custom timestamp" src="images/toast-customtimestamp.jpg" width="396"/>

Weitere Informationen zum Verwenden eines benutzerdefinierten Zeitstempels finden Sie unter [benutzerdefinierte Zeitstempel in den-Umfassungen](custom-timestamps-on-toasts.md).

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

**Neues in Creators Update**: Sie können eine Statusanzeige für ihre Popup Benachrichtigung bereitstellen, um den Benutzer über den Fortschritt von Vorgängen, z. b. Downloads, zu informieren.

<img alt="Toast with progress bar" src="images/toast-progressbar.png" width="364"/>

Weitere Informationen zum Verwenden einer Statusanzeige finden Sie unter Popup- [Status](toast-progress-bar.md)Anzeige.


## <a name="headers"></a>Header

**Neues in Creators Update**: Sie können Benachrichtigungen im Aktions Center unter Header gruppieren. Beispielsweise können Sie Nachrichten aus einem Gruppenchat unter einem Header gruppieren oder Benachrichtigungen eines allgemeinen Designs unter einem Header gruppieren.

<img alt="Toasts with header" src="images/toast-headers-action-center.png" width="396"/>

Weitere Informationen zum Verwenden von Headern finden Sie unter Popup [Header](toast-headers.md).


## <a name="adaptive-content"></a>Adaptive Inhalte

**Neu in Anniversary Update**: Zusätzlich zu den oben angegebenen Inhalten können auch zusätzliche Adaptive Inhalte angezeigt werden, die sichtbar sind, wenn der Toast erweitert wird.

Dieser zusätzliche Inhalt wird mithilfe von adaptiver angegeben. Weitere Informationen hierzu finden Sie in der [Dokumentation zu adaptiven Kacheln](create-adaptive-tiles.md).

Beachten Sie, dass alle adaptiven Inhalte in einer [**adaptivegroup**](./toast-schema.md#adaptivegroup)enthalten sein müssen. Andernfalls wird Sie nicht mit Adaptive gerendert.


### <a name="columns-and-text-elements"></a>Spalten und Textelemente

Im folgenden finden Sie ein Beispiel, in dem Spalten und einige erweiterte Adaptive Textelemente verwendet werden. Da sich die Textelemente innerhalb einer **adaptivegroup**befinden, unterstützen Sie alle umfangreichen adaptiven Formatierungs Eigenschaften.

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

Schaltflächen machen Ihren Toast interaktiv, sodass der Benutzer schnelle Aktionen für ihre Popup Benachrichtigung durchführen kann, ohne den aktuellen Workflow zu unterbrechen. Beispielsweise können Benutzer direkt von einem Toast aus auf eine Nachricht antworten oder eine e-Mail löschen, ohne die e-Mail-APP zu öffnen. Schaltflächen werden im erweiterten Teil ihrer Benachrichtigung angezeigt.

Weitere Informationen zum End-to-End-Implementieren von Schaltflächen finden Sie unter [Send local Toast](send-local-toast.md).

Schaltflächen können die folgenden unterschiedlichen Aktionen ausführen...

-   Aktivieren der APP im Vordergrund mit einem Argument, das verwendet werden kann, um zu einer bestimmten Seite/einem bestimmten Kontext zu navigieren.
-   Aktivieren der Hintergrundaufgabe der APP für eine schnelle Antwort oder ein ähnliches Szenario.
-   Aktivieren einer anderen App per Protokollstart.
-   Ausführen einer System Aktion, z. b. das Zurückstellen oder verwerfen der Benachrichtigung.

> [!NOTE]
> Sie können nur bis zu 5 Schaltflächen (einschließlich Kontextmenü Elemente, die später besprochen werden) enthalten.

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

Sie können den Schaltflächen Symbole hinzufügen. Diese Symbole sind weiß transparente 16x16-Pixelbilder bei der Skalierung von 100% und sollten keine Auffüll Zeichen im Bild selbst enthalten. Wenn Sie Symbole für eine Popup Benachrichtigung bereitstellen möchten, müssen Sie Symbole für alle Schaltflächen in der Benachrichtigung angeben, da der Stil der Schaltflächen in Symbol Schaltflächen transformiert wird.

> [!NOTE]
> Achten Sie bei der Barrierefreiheit darauf, dass Sie eine weiße Kontrast Version des Symbols (ein schwarzes Symbol für weiße Hintergründe) einschließen, damit das Symbol angezeigt wird, wenn der Benutzer hoher Kontrast den weißen Modus wechselt. Weitere Informationen finden Sie auf der [Seite Toast Barrierefreiheits](tile-toast-language-scale-contrast.md).

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


### <a name="buttons-with-pending-update-activation"></a>Schaltflächen mit ausstehender Update Aktivierung

**Neu bei Fall Creators Update**: auf den Hintergrund Aktivierungs Schaltflächen können Sie ein nach dem Aktivierungs Verhalten von " **tabdingupdate** " verwenden, um mehrstufige Interaktionen in ihren Popup Benachrichtigungen zu erstellen. Wenn der Benutzer auf die Schaltfläche klickt, wird die Hintergrundaufgabe aktiviert, und der Toast wird in den Zustand "ausstehende Aktualisierung" versetzt, wo er auf dem Bildschirm bleibt, bis die Hintergrundaufgabe den Toast durch einen neuen Toast ersetzt.

Informationen dazu, wie Sie diese Implementierung implementieren, finden Sie unter Popup [Pending Update](toast-pending-update.md).

![Toast mit ausstehenden Updates](images/toast-pendingupdate.gif)


### <a name="context-menu-actions"></a>Aktionen im Kontextmenü

**Neu in Anniversary Update**: Sie können dem vorhandenen Kontextmenü zusätzliche Kontextmenü Aktionen hinzufügen, die angezeigt werden, wenn der Benutzer im Aktions Center mit der rechten Maustaste auf den Toast klickt. Beachten Sie, dass dieses Menü nur angezeigt wird, wenn Sie mit der rechten Maustaste im Aktions Center Wird nicht angezeigt, wenn Sie mit der rechten Maustaste auf ein Popup-Popup Banner klicken.

> [!NOTE]
> Auf älteren Geräten werden diese zusätzlichen Kontextmenü Aktionen einfach als normale Schaltflächen auf dem Toast angezeigt.

Die zusätzlichen Kontextmenü Aktionen, die Sie hinzufügen (z. b. "Änderungs Speicherort"), werden oberhalb der beiden Standardsystem Einträge angezeigt.

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
> Zusätzliche Kontextmenü Elemente tragen zum Gesamtlimit von 5 Schaltflächen in einem Toast bei.

Die Aktivierung zusätzlicher Kontextmenü Elemente wird mit Popup Schaltflächen identisch behandelt.


## <a name="inputs"></a>Eingaben

Eingaben werden innerhalb des Aktionsbereichs des Popup Bereichs des Popups angegeben. Dies bedeutet, dass Sie nur sichtbar sind, wenn der Toast erweitert wird.


### <a name="quick-reply-text-box"></a>Textfeld für die schnelle Antwort

Wenn Sie ein Textfeld für die schnelle Antwort (z. b. in einer Messaging-APP) aktivieren möchten, fügen Sie eine Texteingabe und eine Schaltfläche hinzu, und verweisen Sie auf die ID des Texteingabe Felds, sodass die Schaltfläche neben dem Eingabefeld angezeigt wird. Das Symbol für die Schaltfläche muss ein 32 x 32 Pixel-Bild ohne Auffüll Zeichen, weiße Pixel auf transparent und 100% Skalierung sein.

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


### <a name="inputs-with-buttons-bar"></a>Eingaben mit Schaltflächen Leiste

Sie können auch über eine (oder viele) Eingabe mit normalen Schaltflächen verfügen, die unter den Eingaben angezeigt werden.

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


### <a name="selection-input"></a>Auswahl Eingabe

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


### <a name="snoozedismiss"></a>Zurückstellen/verwerfen

Mit einem Auswahlmenü und zwei Schaltflächen können wir eine Erinnerungs Benachrichtigung erstellen, die das System zum Zurücksetzen und Verwerfen von Aktionen verwendet. Stellen Sie sicher, dass das Szenario als Erinnerung festgelegt wird, damit die Benachrichtigung sich wie eine Erinnerung verhält.

<img alt="reminder notification" src="images/adaptivetoasts-xmlsample07.jpg" width="364"/>

Wir verknüpfen die Schaltfläche "Zurücksetzen" mit der Auswahlmenü Eingabe mithilfe der **selectionboxid** -Eigenschaft auf der Popup Schaltfläche.

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

So verwenden Sie die System Aktion zum Zurücksetzen und Verwerfen von Aktionen:

-   Geben Sie ein " **onastbuttonsnooze** " oder " **onastbuttonverwerfen** " an
-   Geben Sie optional eine benutzerdefinierte Inhalts Zeichenfolge an:
    -   Wenn Sie keine Zeichenfolge angeben, werden automatisch lokalisierte Zeichen folgen für "zurückstellen" und "verwerfen" verwendet.
-   Geben Sie optional **selectionboxid**an:
    -   Wenn Sie nicht möchten, dass der Benutzer ein Intervall für das erneute Erinnern auswählen kann, sondern das erneute Erinnern an die Benachrichtigung nur einmal in einem vom System definierten (in allen Betriebssystemen einheitlichen) Zeitintervall erfolgt, legen Sie keinen Wert für &lt;input&gt; fest.
    -   Wenn Sie mögliche Intervalle für das erneute Erinnern bereitstellen möchten:
        -   Angeben von **selectionboxid** in der Aktion zum Zurücksetzen
        -   Vergleichen Sie die ID der Eingabe mit der **selectionboxid** der Aktion zum Zurücksetzen.
        -   Legen Sie den Wert von " **alastselectionboxitem**" als nonnegativeinzelteger fest, der das Intervall für die Wartezeit in Minuten darstellt.



## <a name="audio"></a>Audio

Benutzerdefiniertes Audiogerät wurde von mobilen Geräten immer unterstützt und wird in Desktop Version 1511 (Build 10586) oder höher unterstützt. Auf benutzerdefiniertes Audiodaten kann über die folgenden Pfade verwiesen werden:

-   ms-appx:///
-   ms-appdata:///

Alternativ können Sie aus der [Liste der MS-winsoundebug](/uwp/schemas/tiles/toastschema/element-audio#attributes-and-elements)-Ereignisse auswählen, die immer auf beiden Plattformen unterstützt werden.

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

Informationen zu Audiobenachrichtigungen in Popup Benachrichtigungen finden Sie auf der [Seite Audioschema](/uwp/schemas/tiles/toastschema/element-audio) . Informationen zum Senden eines Popups mithilfe von benutzerdefiniertem Audioinhalt finden Sie unter [benutzerdefinierte Audioinformationen in Popups](custom-audio-on-toasts.md).


## <a name="alarms-reminders-and-incoming-calls"></a>Alarme, Erinnerungen und eingehende Anrufe

Um Alarme, Erinnerungen und eingehende Benachrichtigungen zu erstellen, verwenden Sie einfach eine normale Popup Benachrichtigung mit einem ihm zugewiesenen szenariowert. Das Szenario ist eine Reihe von Verhaltensweisen, mit denen eine konsistente und einheitliche Benutzerfunktion erstellt wird.

> [!IMPORTANT]
> Bei der Verwendung der Erinnerung oder des Alarms müssen Sie mindestens eine Schaltfläche in der Popup Benachrichtigung angeben. Andernfalls wird der Toast als normaler Toast behandelt.

* **Erinnerung**: die Benachrichtigung bleibt auf dem Bildschirm, bis der Benutzer Sie schließt oder Maßnahmen ergreift. Unter Windows Mobile wird der Toast auch vorab erweitert angezeigt. Ein Erinnerungs Sound wird wiedergegeben.
* **Alarm**: Zusätzlich zu den Erinnerungs Verhaltensweisen werden Alarme zusätzlich zu einem standardmäßigen Alarm Soundschleifen.
* **IncomingCall:** eingehende Benachrichtigungen werden auf Windows Mobile-Geräten als Vollbild angezeigt. Andernfalls haben Sie das gleiche Verhalten wie Alarme, außer Sie verwenden Rington-Audiodaten und ihre Schaltflächen werden anders formatiert.

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


## <a name="localization-and-accessibility"></a>Lokalisierung und Barrierefreiheit

Mit ihren Kacheln und-aufbildern können Zeichen folgen und Bilder geladen werden, die für die Anzeige Sprache, den Anzeige Skalierungsfaktor, den hohen Kontrast und andere Lauf Zeit Kontexte Weitere Informationen finden Sie [unter Unterstützung für Kachel-und Popup Benachrichtigungen für Sprache, Skalierung und hohen Kontrast](tile-toast-language-scale-contrast.md).


## <a name="handling-activation"></a>Aktivierungs Aktivierung
Informationen zum Behandeln von Popup Aktivierungen (der Benutzer, der auf den Toast oder die Schaltflächen des Popups klickt) finden Sie unter [Send local Toast](send-local-toast.md).
 
## <a name="related-topics"></a>Zugehörige Themen

* [Senden eines lokalen Popups und behandeln der Aktivierung](send-local-toast.md)
* [Benachrichtigungs Bibliothek auf GitHub (Teil des UWP Community Toolkit)](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
* [Unterstützung für Kachel-und Popup Benachrichtigungen für Sprache, Skalierung und hohen Kontrast](tile-toast-language-scale-contrast.md)