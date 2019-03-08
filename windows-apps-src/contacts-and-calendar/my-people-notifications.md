---
title: Meine Kontakte – Benachrichtigungen
description: Erläutert das Erstellen und Verwenden von Benachrichtigungen für Meine Kontakte, eine neue Art Popups.
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e4657c9dfb651c1e7f6bb4e2cafdc49d0a11237b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630565"
---
# <a name="my-people-notifications"></a>Meine Kontakte – Benachrichtigungen

„Meine Kontakte”-Benachrichtigungen bieten Benutzern eine neue Möglichkeit, sich mit den Menschen, die ihnen wichtig sind, durch schnelle, ausdrucksstarke Gesten zu verbinden. Dieser Artikel zeigt, wie Sie Meine Kontakte-Benachrichtigungen in Ihrer Anwendung entwerfen und implementieren. Vollständige Implementierungen finden Sie unter [Beispiel für „Meine Kontakte”.](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/MyPeopleNotifications)

![Herz-Emoji-Benachrichtigung](images/heart-emoji-notification-small.gif)

## <a name="requirements"></a>Anforderungen

+ Windows 10 und Microsoft Visual Studio 2017 Ausführliche Informationen zur Installation finden Sie unter [Visual Studio einrichten](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up).
+ Sie sollten über Grundkenntnisse in C# oder einer ähnlichen objektorientierten Programmiersprache verfügen. Die ersten Schritte mit C# finden Sie unter [Erstellen der App „Hello, world“ (C++)](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal).

## <a name="how-it-works"></a>Funktionsweise

Als Alternative zur generischen Popupbenachrichtigungen können Sie jetzt Benachrichtigungen über das Feature „Meine Kontakte” senden, um eine persönlichere Erfahrung für Benutzer bereitzustellen. Dies ist eine neue Art von Popup, das von einem Kontakt gesendet wird, der mit der Funktion „Meine Kontakte” auf der Taskleiste des Benutzers angeheftet ist. Wenn die Benachrichtigung empfangen wird, wird das Bild des Kontakts auf der Taskleiste animiert und ein Sound wird abgespielt, was signalisiert, dass eine Benachrichtigung gestartet wird. Die Animation oder das in der Nutzlast angezeigte Bild werden für 5 Sekunden angezeigt (oder, wenn die Nutzlast eine Animation ist, die kürzer als 5 Sekunden dauert, wird diese wiederholt, bis die 5 Sekunden vorbei sind).

## <a name="supported-image-types"></a>Unterstützte Abbildtypen

+ GIF
+ Statisches Bild (JPEG, PNG)
+ Spritesheet (nur vertikal)

> [!NOTE]
> Ein Spritesheet ist eine Animation, die von einem statischen Bild (JPEG- oder PNG) abgeleitet wird. Einzelne Frames werden vertikal angeordnet, so dass sich das erste Bild im Vordergrund befindet (Sie können eine andere Startframe in der Nutzlast der Popupbenachrichtigung angeben). Jedes Frame muss die gleiche Höhe haben, damit das Programm diese wiederholt, um eine animierte Sequenz zu erstellen (z. B. wie ein Flipbook mit seinen Seiten, die vertikal angeordnet sind). Ein Beispiel für ein Spritesheet wird unten gezeigt.

![Spritesheet in Regenbogenfarben](images/shoulder-tap-rainbow-spritesheet.png)

## <a name="notification-parameters"></a>Benachrichtigungsparameter
Benachrichtigungen von „Meine Kontakte” verwenden die [Popupbenachrichtigung](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)-Framework, aber erfordern einen zusätzlichen Bindungsknoten in der Nutzlast des Popups. Die zweite Bindung muss folgende Parameter enthalten:

```xml
experienceType=”shoulderTap”
```

Dies bedeutet, dass das Popup als eine Benachrichtigung für „Meine Kontakte” behandelt werden soll.

Die Image-Knoten in der Bindung sollten die folgenden Parameter enthalten:

+ **src**
    + Die URI des Elements. Dies kann sowohl HTTP/HTTPS web URI, eine msappx URI oder ein Pfad zu einer lokalen Datei sein.
+ **spritesheet-src**
    + Die URI des Elements. Dies kann sowohl HTTP/HTTPS web URI, eine msappx URI oder ein Pfad zu einer lokalen Datei sein. Ist nur für Spritesheet-Animationen erforderlich.
+ **spritesheet-height**
    + Die Framehöhe (in Pixeln). Ist nur für Spritesheet-Animationen erforderlich.
+ **spritesheet-fps**
    + Frames pro Sekunde (FPS). Ist nur für Spritesheet-Animationen erforderlich. Es werden nur Werte von 1‑120 unterstützt.
+ **spritesheet-startingFrame**
    + Die Framenummer, um die Animation zu starten. Wird nur für Spritesheet-Animationen verwendet und hat den Standardwert 0, wenn keine Angabe erfolgt.
+ **alt**
    + Text-Zeichenfolge, die als Bildschirmsprachausgabe verwendet wird.

> [!NOTE]
> Bei einer animierten Benachrichtigung sollten Sie dennoch ein statisches Bild im Parameter „src” angeben. Es wird als Fallback verwendet, wenn die Animation nicht angezeigt wird.

Darüber hinaus muss der Knoten der obersten Ebene Popups **hint-people**-Parameter enthalten, um den Absender-Kontakt anzugeben. Dieser Parameter kann folgende Werte haben:

+ **E-Mail-Adresse** 
    + z. B. mailto:johndoe@mydomain.com
+ **Telefonnummer** 
    + z. B. Tel:888-888-8888
+ **Remote-ID** 
    + z. B. remoteid:1234

> [!NOTE]
> Falls Ihre App die [ContactStore APIs](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contactstore) verwendet und auf dem Smartphone gespeicherte Kontakte mithilfe der [StoredContact.RemoteId](https://docs.microsoft.com/en-us/uwp/api/Windows.Phone.PersonalInformation.StoredContact.RemoteId)-Eigenschaft mit remote gespeicherten Kontakten verknüpft, muss der Wert für die RemoteId-Eigenschaft unbedingt stabil und eindeutig sein. Die Remote-ID muss also durchweg ein einzelnes Benutzerkonto identifizieren und ein eindeutiges Tag enthalten, um zu verhindern, dass sich Konflikte mit den Remote-IDs anderer Kontakte auf dem PC ergeben. Hierzu zählen auch Kontakte von anderen Apps.
> Falls die Stabilität und Eindeutigkeit der von Ihrer App verwendeten Remote-IDs nicht gewährleistet ist, können Sie allen Ihren [RemoteIdHelper](https://msdn.microsoft.com/en-us/library/windows/apps/jj207024(v=vs.105).aspx#BKMK_UsingtheRemoteIdHelperclass) mithilfe der später in diesem Thema beschriebenen -Klasse ein eindeutiges Tag hinzufügen, bevor Sie die Remote-IDs dem System hinzufügen. Alternativ können Sie auch ganz auf die Verwendung der RemoteId-Eigenschaft verzichten und stattdessen eine benutzerdefinierte erweiterte Eigenschaft erstellen, um die Remote-IDs für Ihre Kontakte zu speichern.

Neben der zweiten Bindung und der Nutzlast müssen Sie eine weitere Nutzlast in die erste Bindung für das Fallback-Popup aufnehmen. Die Benachrichtigung verwendet diese, wenn sie gezwungen ist, zu einem regulären Popup zurückzukehren (wird am [Ende dieses Artikels](/windows/uwp/contacts-and-calendar/my-people-notifications#falling-back-to-toast) näher erläutert).

## <a name="creating-the-notification"></a>Erstellen der Benachrichtigung
Sie können eine Benachrichtigungsvorlage für Meine Kontakte genau so erstellen wie eine [Popupbenachrichtigung](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md).

Hier ist ein Beispiel für das Erstellen einer Benachrichtigung für „Meine Kontakte” mithilfe eines statischen Bilds:

```xml
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastGeneric">
            <text hint-style="body">Toast fallback</text>
            <text>Add your fallback toast content here</text>
        </binding>
        <binding template="ToastGeneric" experienceType="shoulderTap">
            <image src="https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/images/shoulder-tap-static-payload.png"/>
        </binding>
    </visual>
</toast>
```

Wenn Sie die Benachrichtigung starten, sollte sie wie folgt aussehen:

![Benachrichtigung als statisches Bild](images/static-image-notification-small.gif)

Hier ist ein Beispiel, wie man eine „Meine Kontakte”-Benachrichtigung mit einer statischen Bild-Nutzlast erstellt: Dieses Spritesheet hat eine Framehöhe von 80 Pixel, die mit 25 Frames pro Sekunde animiert werden. Wir legen Sie den ersten Frame auf 15 fest und geben ihm ein statisches Fallback-Bild mit dem Parameter "Src". Das Fallback-Bild wird verwendet, wenn die Spritesheet-Animation nicht angezeigt wird.

```xml
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastGeneric">
            <text hint-style="body">Toast fallback</text>
            <text>Add your fallback toast content here</text>
        </binding>
        <binding template="ToastGeneric" experienceType="shoulderTap">
            <image src="https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/images/shoulder-tap-pizza-static.png"
                spritesheet-src="https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/images/shoulder-tap-pizza-spritesheet.png"
                spritesheet-height='80' spritesheet-fps='25' spritesheet-startingFrame='15'/>
        </binding>
    </visual>
</toast>
```

Wenn Sie die Benachrichtigung starten, sollte sie wie folgt aussehen:

![Benachrichtigung als Spritesheet](images/pizza-notification-small.gif)

## <a name="starting-the-notification"></a>Starten der Benachrichtigung
Um eine Benachrichtigung für „Meine Kontakte” zu starten, müssen wir die Popup-Vorlage in ein [XmlDocument](https://msdn.microsoft.com/en-us/library/windows/apps/windows.data.xml.dom.xmldocument.aspx)-Objekt konvertieren. Falls Sie das Popup in einer XML-Datei (hier „content.xml“ genannt) definiert haben, verwenden Sie diesen Code zum Starten verwenden:

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

## <a name="falling-back-to-toast"></a>Zurückgreifen auf Popupbenachrichtigungen
Es gibt einige Fälle, wo eine Benachrichtigung über „Meine Kontakte” stattdessen als reguläre Popupbenachrichtigung angezeigt wird. Eine Benachrichtigung für „Meine Kontakte” greift unter folgenden Umständen auf eine Popupbenachrichtigung zurück:

+ Die Benachrichtigung wird nicht angezeigt
+ Benachrichtigungen für „Meine Kontakte” sind nicht vom Empfänger aktiviert
+ Der Kontakt des Absenders ist nicht an die Taskleiste des Empfängers angeheftet

Wenn eine Benachrichtigung für „Meine Kontakte” erneut auf eine Popupbenachrichtigung zurückfällt, wird die zweite spezifische Bindung für „Meine Kontakte” ignoriert, und nur die erste Bindung wird verwendet, um das Popup anzuzeigen. Deshalb ist es wichtig, eine Fallback-Nutzlast in der ersten Popupbindung bereitzustellen.

## <a name="see-also"></a>Siehe auch
+ [Meine Benutzer Notifications-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/MyPeopleNotifications)
+ [Hinzufügen von Meine Benutzer zu unterstützen.](my-people-support.md)
+ [Adaptive Popupbenachrichtigungen](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)
+ [ToastNotification-Klasse](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification)