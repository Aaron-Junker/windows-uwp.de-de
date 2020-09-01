---
title: Meine Kontakte – Benachrichtigungen
description: Erläutert das Erstellen und Verwenden von Benachrichtigungen für meine Personen, eine neue Art von Toast.
ms.date: 10/25/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 3e00e3de9445a8b7c63ebaead70173c29b637b54
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166324"
---
# <a name="my-people-notifications"></a>Meine Kontakte – Benachrichtigungen

Meine Personen Benachrichtigungen bieten eine neue Möglichkeit für Benutzer, sich mit den Personen zu verbinden, die Sie interessieren, und zwar durch schnelle, ausdrucksstarke Gesten. In diesem Artikel wird das Entwerfen und Implementieren von Benachrichtigungen für meine Personen in Ihrer Anwendung erläutert. Informationen zu den kompletten Implementierungen finden Sie im [Beispiel meine Personen Benachrichtigungen.](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/MyPeopleNotifications)

![Benachrichtigung über Herz-Emoji](images/heart-emoji-notification-small.gif)

## <a name="requirements"></a>Anforderungen

+ Windows 10 und Microsoft Visual Studio 2019. Weitere Informationen zur Installation finden Sie unter [Einrichten von Visual Studio](../get-started/get-set-up.md).
+ Grundkenntnisse in C# oder einer ähnlichen objektorientierten Programmiersprache. Informationen zu den ersten Schritten mit c# finden Sie unter [Erstellen einer "Hello, World"-App](../get-started/create-a-hello-world-app-xaml-universal.md).

## <a name="how-it-works"></a>Funktionsweise

Als Alternative zu generischen Popup Benachrichtigungen können Sie jetzt Benachrichtigungen über das Feature "meine Personen" senden, um Benutzern eine persönlichere Benutzer Funktionalität bereitzustellen. Dabei handelt es sich um eine neue Art von Toast, die von einem Kontakt gesendet wird, der auf der Taskleiste des Benutzers mit der Funktion "meine Personen" fixiert ist. Beim Empfang der Benachrichtigung wird das Kontakt Bild des Absenders in der Taskleiste animiert, und es wird ein Sound abgespielt, der signalisiert, dass die Benachrichtigung gestartet wird. Die in der Nutzlast angegebene Animation oder das Bild wird 5 Sekunden lang angezeigt (oder wenn es sich bei der Nutzlast um eine Animation handelt, die weniger als 5 Sekunden lang ist, wird die Schleife bis 5 Sekunden überschritten).

## <a name="supported-image-types"></a>Unterstützte Bildtypen

+ GIF
+ Statisches Bild (JPEG, PNG)
+ SpriteSheet (nur vertikal)

> [!NOTE]
> Ein SpriteSheet ist eine Animation, die von einem statischen Bild (JPEG oder PNG) abgeleitet ist. Einzelne Frames sind vertikal angeordnet, sodass sich der erste Frame im oberen Bereich befindet (obwohl Sie einen anderen Startframe in der Toast Nutzlast angeben können). Jeder Frame muss dieselbe Höhe aufweisen, die das Programm durchläuft, um eine animierte Sequenz zu erstellen (z. b. ein im Daumenkino, bei dem seine Seiten vertikal angeordnet sind). Ein Beispiel für ein SpriteSheet ist unten dargestellt.

![Regenbogen SpriteSheet](images/shoulder-tap-rainbow-spritesheet.png)

## <a name="notification-parameters"></a>Benachrichtigungs Parameter
Meine Personen Benachrichtigungen verwenden das Popup [Benachrichtigungs](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md) Framework, erfordern jedoch einen zusätzlichen Bindungs Knoten in der Toast Nutzlast. Diese zweite Bindung muss den folgenden Parameter enthalten:

```xml
experienceType="shoulderTap"
```

Dies gibt an, dass der Toast als meine Personen Benachrichtigung behandelt werden soll.

Der Image-Knoten innerhalb der Bindung muss die folgenden Parameter enthalten:

+ **src**
    + Der URI des Assets. Dabei kann es sich entweder um den HTTP/HTTPS-Web-URI, einen msappx-URI oder um einen Pfad zu einer lokalen Datei handeln.
+ **SpriteSheet-src**
    + Der URI des Assets. Dabei kann es sich entweder um den HTTP/HTTPS-Web-URI, einen msappx-URI oder um einen Pfad zu einer lokalen Datei handeln. Nur für SpriteSheet-Animationen erforderlich.
+ **SpriteSheet-Höhe**
    + Die Rahmenhöhe (in Pixel). Nur für SpriteSheet-Animationen erforderlich.
+ **SpriteSheet-fps**
    + Frames pro Sekunde (fps). Nur für SpriteSheet-Animationen erforderlich. Es werden nur die Werte 1-120 unterstützt.
+ **SpriteSheet-startingframe**
    + Die Frame Nummer, zu der die Animation beginnen soll. Wird nur für SpriteSheet-Animationen verwendet und ist standardmäßig auf 0 gesetzt, wenn nicht angegeben.
+ **alt**
    + Text Zeichenfolge für die Sprachausgabe.

> [!NOTE]
> Beim Erstellen einer animierten Benachrichtigung sollten Sie weiterhin ein statisches Bild im "src"-Parameter angeben. Sie wird als Fallback verwendet, wenn die Animation nicht angezeigt werden kann.

Außerdem muss der Popup-Knoten der obersten Ebene den **Hint-People-** Parameter enthalten, um den Sende Kontakt anzugeben. Dieser Parameter kann einen der folgenden Werte aufweisen:

+ **E-Mail-Adresse** 
    + Beispiel: ` mailto:johndoe@mydomain.com `
+ **Telefonnummer** 
    + Beispiel: Tel: 888-888-8888
+ **Remote-ID** 
    + Beispiel: RemoteID: 1234

> [!NOTE]
> Wenn Ihre APP die [contactstore-APIs](/uwp/api/windows.applicationmodel.contacts.contactstore) verwendet und die [storedcontact. RemoteID-](/uwp/api/Windows.Phone.PersonalInformation.StoredContact.RemoteId) Eigenschaft verwendet, um Kontakte zu verknüpfen, die auf dem PC mit den Remote gespeicherten Kontakten gespeichert sind, ist es von entscheidender Bedeutung, dass der Wert für die RemoteID-Eigenschaft stabil und eindeutig ist. Dies bedeutet, dass die Remote-ID ein einzelnes Benutzerkonto einheitlich identifizieren muss und ein eindeutiges Tag enthalten sollte, um sicherzustellen, dass es nicht mit den Remote-IDs anderer Kontakte auf dem PC in Konflikt steht, einschließlich der Kontakte, die sich im Besitz anderer apps befinden.
> Wenn die Remote-IDs, die von Ihrer APP verwendet werden, nicht sicher und eindeutig sind, können Sie die [remoteidhelper-Klasse](/previous-versions/windows/apps/jj207024(v=vs.105)#BKMK_UsingtheRemoteIdHelperclass) verwenden, um allen Remote-IDs ein eindeutiges Tag hinzuzufügen, bevor Sie Sie dem System hinzufügen. Wahlweise können Sie auch auswählen, dass Sie nicht die RemoteID-Eigenschaft verwenden, sondern stattdessen eine benutzerdefinierte erweiterte Eigenschaft erstellen, in der Remote-IDs für Ihre Kontakte gespeichert werden.

Zusätzlich zur zweiten Bindung und Nutzlast müssen Sie eine andere Nutzlast in die erste Bindung für den Fall Back Toast einschließen. Diese Benachrichtigung wird verwendet, wenn die Wiederherstellung auf einen regulären Popup erzwungen wird (am [Ende dieses Artikels](#falling-back-to-toast)ausführlicher erläutert).

## <a name="creating-the-notification"></a>Erstellen der Benachrichtigung
Sie können eine Benachrichtigungs Vorlage für meine Personen wie eine Popup [Benachrichtigung](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)erstellen.

Im folgenden finden Sie ein Beispiel für das Erstellen einer Benachrichtigung für meine Personen mit einer statischen Bild Nutzlast:

```xml
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastGeneric">
            <text hint-style="body">Toast fallback</text>
            <text>Add your fallback toast content here</text>
        </binding>
        <binding template="ToastGeneric" experienceType="shoulderTap">
            <image src="https://docs.microsoft.com/windows/uwp/contacts-and-calendar/images/shoulder-tap-static-payload.png"/>
        </binding>
    </visual>
</toast>
```

Wenn Sie die Benachrichtigung starten, sollte Sie wie folgt aussehen:

![Benachrichtigung über statisches Bild](images/static-image-notification-small.gif)

Im folgenden finden Sie ein Beispiel für das Erstellen einer Benachrichtigung mit einer animierten SpriteSheet-Nutzlast. Dieses SpriteSheet hat eine Rahmenhöhe von 80 Pixel, die wir mit 25 Frames pro Sekunde animieren werden. Wir legen den Startframe auf 15 fest und geben ihm ein statisches Fall Back-Bild im "src"-Parameter. Das Fall Back-Image wird verwendet, wenn die SpriteSheet-Animation nicht angezeigt werden kann.

```xml
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastGeneric">
            <text hint-style="body">Toast fallback</text>
            <text>Add your fallback toast content here</text>
        </binding>
        <binding template="ToastGeneric" experienceType="shoulderTap">
            <image src="https://docs.microsoft.com/windows/uwp/contacts-and-calendar/images/shoulder-tap-pizza-static.png"
                spritesheet-src="https://docs.microsoft.com/windows/uwp/contacts-and-calendar/images/shoulder-tap-pizza-spritesheet.png"
                spritesheet-height='80' spritesheet-fps='25' spritesheet-startingFrame='15'/>
        </binding>
    </visual>
</toast>
```

Wenn Sie die Benachrichtigung starten, sollte Sie wie folgt aussehen:

![SpriteSheet-Benachrichtigung](images/pizza-notification-small.gif)

## <a name="starting-the-notification"></a>Die Benachrichtigung wird gestartet.
Um eine Benachrichtigung für meine Personen zu starten, müssen wir die Popup Vorlage in ein [XmlDocument](/uwp/api/windows.data.xml.dom.xmldocument) -Objekt konvertieren. Wenn Sie den Popup in einer XML-Datei (hier mit dem Namen "content.xml") definiert haben, können Sie diesen Code verwenden, um ihn zu starten:

```CSharp
string xmlText = File.ReadAllText("content.xml");
XmlDocument xmlContent = new XmlDocument();
xmlContent.LoadXml(xmlText);
```

Sie können diesen Code anschließend verwenden, um das Popupfenster zu erstellen und zu senden.

```CSharp
ToastNotification notification = new ToastNotification(xmlContent);
ToastNotificationManager.CreateToastNotifier().Show(notification);
```

## <a name="falling-back-to-toast"></a>Fallback zum Toast
In einigen Fällen wird die Benachrichtigung "meine Personen" stattdessen als reguläre Popup Benachrichtigung angezeigt. Eine Benachrichtigung für meine Personen wird unter den folgenden Bedingungen auf Toast zurückgreifen:

+ Die Benachrichtigung kann nicht angezeigt werden.
+ Meine Personen Benachrichtigungen werden nicht vom Empfänger aktiviert.
+ Der Kontakt des Absenders ist nicht an die Taskleiste des Empfängers angeheftet.

Wenn eine Benachrichtigung "meine Personen" auf Toast zurückgreift, wird die zweite meine Personen spezifische Bindung ignoriert, und nur die erste Bindung wird verwendet, um den Toast anzuzeigen. Aus diesem Grund ist es wichtig, eine Fall Back Nutzlast in der ersten Popup Bindung bereitzustellen.

## <a name="see-also"></a>Weitere Informationen
+ [Beispiel für meine Personen Benachrichtigungen](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/MyPeopleNotifications)
+ [Unterstützung für meine Personen hinzufügen](my-people-support.md)
+ [Adaptive Popup Benachrichtigungen](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)
+ [Klasse "-Benachrichtigungs Klasse"](/uwp/api/windows.ui.notifications.toastnotification)