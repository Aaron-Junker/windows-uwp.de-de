---
Description: Im folgenden Artikel werden alle Eigenschaften und Elemente in der XML-Nutzlast des Popupinhalts beschrieben.
title: XML-Schema des Popupinhalts
ms.assetid: AF49EFAC-447E-44C3-93C3-CCBEDCF07D22
label: Toast content XML schema
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6b9535cd8c2dd82b0c209919080df9a88bb80ccc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612775"
---
# <a name="toast-content-xml-schema"></a>XML-Schema des Popupinhalts

 

Im Folgenden werden alle Eigenschaften und Elemente in der XML-Nutzlast des Popupinhalts beschrieben.

In den folgenden XML-Schemas bedeutet das Suffix „?“, dass ein Attribut optional ist.

## <a name="ltvisualgt-and-ltaudiogt"></a>&lt;Visual&gt; und &lt;Audio&gt;

```
<toast launch? duration? activationType? scenario? >
  <visual lang? baseUri? addImageQuery? >
    <binding template? lang? baseUri? addImageQuery? >
      <text lang? hint-maxLines? >content</text>
      <image src placement? alt? addImageQuery? hint-crop? />
      <group>
        <subgroup hint-weight? hint-textStacking? >
          <text />
          <image />
        </subgroup>
      </group>
    </binding>
  </visual>
  <audio src? loop? silent? />
</toast>
```

**Attribute in &lt;Toast&gt;**

launch?

-   launch? = string
-   Dies ist ein optionales Attribut.
-   Eine Zeichenfolge wird an die Anwendung übergeben, wenn sie durch das Popup aktiviert wird.
-   Abhängig vom Wert für „activationType“ kann dieser Wert von der App im Vordergrund, innerhalb der Hintergrundaufgabe oder von einer anderen App empfangen werden, die per Protokollstart über die ursprüngliche App gestartet wird.
-   Das Format und der Inhalt dieser Zeichenfolge werden von der App für eigene Zwecke definiert.
-   Wenn der Benutzer zum Starten der zugeordneten App auf das Popup tippt oder klickt, stellt die Startzeichenfolge der App den Kontext bereit, um dem Benutzer eine Ansicht passend zum Popupinhalt zu ermöglichen, anstatt sie in der Standardgröße starten.
-   Ist die Aktivierung erfolgt, weil der Benutzer auf eine Aktion statt auf den Popuptext geklickt hat, erhält der Entwickler die vordefinierten „Argumente“ in diesem &lt;action&gt;-Tag zurück, statt des vordefinierten „launch“ im &lt;toast&gt;-Tag.

duration?

-   duration? = "short|long"
-   Dies ist ein optionales Attribut. Der Standardwert lautet „kurz“.
-   Es dient nur für bestimmte Szenarien und appCompat. Im Alarmszenario wird es nicht mehr benötigt.
-   Die Verwendung dieser Eigenschaft wird nicht empfohlen.

activationType?

-   activationType? = "foreground | background | protocol | system"
-   Dies ist ein optionales Attribut.
-   Der Standardwert lautet „foreground“

scenario?

-   scenario? = "default | alarm | reminder | incomingCall"
-   Dies ist ein optionales Attribut, der Standardwert lautet „default“.
-   Sie benötigen es nur, wenn es sich bei Ihrem Szenario um einen Alarm, eine Erinnerung oder einen eingehenden Anruf handelt.
-   Verwenden Sie es nicht, um die Benachrichtigung dauerhaft auf dem Bildschirm anzuzeigen.

**Attribute in &lt;visual&gt;**

lang?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230847) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

baseUri?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230847) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

addImageQuery?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230847) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

**Attribute in &lt;Bindung&gt;**

template?

-   \[Wichtige\] Vorlage? = "ToastGeneric"
-   Wenn Sie eines der neuen Features für adaptive und interaktive Benachrichtigungen verwenden, stellen Sie sicher, dass Sie mit der Vorlage „ToastGeneric“ statt mit der Legacyvorlage beginnen.
-   Möglicherweise können Sie die Legacyvorlagen mit den neuen Aktionen verwenden, aber da dies nicht der gewünschte Anwendungsfall ist, können wir keinen Erfolg garantieren.

lang?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230847) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

baseUri?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230847) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

addImageQuery?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230847) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

**Attribute in &lt;Text&gt;**

lang?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230847) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

**Attribute in &lt;Image&gt;**

src

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230844) finden Sie ausführliche Informationen zu diesem erforderlichen Attribut.

placement?

-   placement? = "inline" | "appLogoOverride"
-   Dieses Attribut ist optional.
-   Es bestimmt, wo dieses Bild angezeigt wird.
-   „inline“ bedeutet innerhalb des Popuptextes, unter dem Text; „appLogoOverride“ steht für das Ersetzen des Anwendungssymbols (das in der oberen linken Ecke des Popups angezeigt wird).
-   Für jeden Platzierungswert ist maximal ein Bild möglich.

alt?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230844) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

addImageQuery?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230844) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

hint-crop?

-   hint-crop? = "none" | "circle"
-   Dieses Attribut ist optional.
-   „none“ ist der Standardwert, sodass kein Zuschneiden möglich ist.
-   „circle“ schneidet das Bild in Kreisform. Verwenden Sie diese Option für Profilbilder eines Kontakts, Bilder einer Person usw.

**Attribute in &lt;audio&gt;**

src?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230842) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

loop?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230842) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

silent?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230842) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

## <a name="schemas-ltactiongt"></a>Schemata: &lt;Aktion&gt;


In den folgenden XML-Schemas bedeutet das Suffix „?“, dass ein Attribut optional ist.

```
<toast>
  <visual>
  </visual>
  <audio />
  <actions>
    <input id type title? placeHolderContent? defaultInput? >
      <selection id content />
    </input>
    <action content arguments activationType? imageUri? hint-inputId />
  </actions>
</toast>
```

**Attribute in &lt;Eingabe&gt;**

id

-   id = string
-   Dieses Attribut ist erforderlich.
-   Das id-Attribut ist erforderlich und wird von Entwicklern verwendet, um Eingaben des Benutzers abzurufen, sobald die App (im Vordergrund oder im Hintergrund) aktiviert ist.

type

-   type = "text | selection"
-   Dieses Attribut ist erforderlich.
-   Damit wird eine Texteingabe oder Eingabe aus einer Liste von vordefinierten Optionen angegeben.
-   Auf Mobilgeräten und Desktops wird damit angegeben, ob eine Eingabe per Textfeld oder per Listenfeld erfolgen soll.

title?

-   title? = string
-   Das Title-Attribut ist optional. Entwickler können damit einen Titel für die Eingabe festlegen, damit Shells gerendert werden können.
-   Auf Mobilgeräten und Desktops wird dieser Titel über der Eingabe angezeigt.

placeHolderContent?

-   placeHolderContent? = string
-   Das Attribut „placeHolderContent“ ist optional und der ausgegraute Hinweistext für den Texteingabetyp. Dieses Attribut wird ignoriert, wenn der Eingabetyp nicht „text“ lautet.

defaultInput?

-   defaultInput? = string
-   Das Attribut „defaultInput“ ist optional und wird verwendet, um einen Standardeingabewert bereitzustellen.
-   Beim Eingabetyp „text“ wird dieser als Zeichenfolgeneingabe behandelt.
-   Lautet der Eingabetyp „selection“, wird angenommen, dass dieser die ID einer der verfügbaren Auswahl in diesen Eingabeelementen ist.

**Attribute in &lt;Auswahl&gt;**

id

-   Dieses Attribut ist erforderlich. Hiermit wird die Auswahl des Benutzers ermittelt. Die ID wird an Ihre App zurückgegeben.

content

-   Dieses Attribut ist erforderlich. Es enthält die Zeichenfolge, die für dieses Auswahlelement angezeigt werden soll.

**Attribute in &lt;Aktion&gt;**

content

-   content = string
-   Das Inhaltsattribut ist erforderlich. Es enthält die Textzeichenfolge, die auf der Schaltfläche angezeigt wird.

arguments

-   arguments = string
-   Die Argument-Attribut ist erforderlich. Es beschreibt die von der App definierten Daten, die die App später abrufen kann, nachdem sie vom Benutzer durch diese Aktion aktiviert wird.

activationType?

-   activationType? = "foreground | background | protocol | system"
-   Das Attribut „activationType“ ist optional und der Standardwert lautet „foreground“.
-   Es beschreibt die Art der Aktivierung, die diese Aktion auslöst: im Vordergrund, im Hintergrund, Starten einer anderen App über Protokollstart oder Aufrufen einer Systemaktion.

imageUri?

-   imageUri? = string
-   ImageUri ist optional und wird verwendet, um ein Bildsymbol für diese Aktion bereitzustellen, welches auf der Schaltfläche im Textkontext angezeigt wird.

hint-inputId

-   hint-inputId = string
-   Das hint-inpudId-Attribut ist erforderlich. Es wird speziell für das schnelle Beantworten einer Nachricht verwendet.
-   Der Wert muss der ID entsprechen, die dem Eingabeelement zugeordnet werden soll.
-   Auf Mobilgeräten und Desktops wird die Schaltfläche rechts neben dem Eingabefeld platziert.

## <a name="attributes-for-system-handled-actions"></a>Attribute für systemgesteuerte Aktionen


Das System kann Aktionen für die Funktionen zum erneuen Erinnern und zum Schließen von Benachrichtigungen auslösen, wenn Sie nicht wünschen, dass Ihre App das erneute Erinnern/Neuplanen von Benachrichtigungen im Hintergrund ausführt. Systemgesteuerte Aktionen können kombiniert (oder einzeln festgelegt) werden. Wir raten jedoch davon ab, eine erneute Erinnerung ohne Möglichkeit zum Schließen zu implementieren.

Kombinationsfeld für System-Befehle: SnoozeAndDismiss

```
<toast>
  <visual>
  </visual>
  <actions hint-systemCommands="SnoozeAndDismiss" />
</toast>
```

Einzelne systemgesteuerte Aktionen

```
<toast>
  <visual>
  </visual>
  <actions>
  <input id="snoozeTime" type="selection" defaultInput="10">
    <selection id="5" content="5 minutes" />
    <selection id="10" content="10 minutes" />
    <selection id="20" content="20 minutes" />
    <selection id="30" content="30 minutes" />
    <selection id="60" content="1 hour" />
  </input>
  <action activationType="system" arguments="snooze" hint-inputId="snoozeTime" content=""/>
  <action activationType="system" arguments="dismiss" content=""/>
  </actions>
</toast>
```

Gehen Sie wie folgt vor, um individuelle Aktionen zum erneuten Erinnern und Schließen zu erstellen:

-   Legen Sie Folgendes fest: activationType = "system"
-   Legen Sie Argumente fest = "snooze" | "dismiss"
-   Legen Sie Inhalt fest:
    -   Wenn Sie wünschen, dass lokalisierte Zeichenfolgen für „snooze“ und „dismiss“ in den Aktionen angezeigt werden, legen Sie den Inhalt als leere Zeichenfolge fest: &lt;action content = ""/&gt;
    -   Wenn Sie eine benutzerdefinierte Zeichenfolge wünschen, geben Sie seinen Wert an: &lt;action content="Erinnere mich später" /&gt;
-   Legen Sie Eingaben fest:
    -   Wenn Sie nicht möchten, dass der Benutzer ein Intervall für das erneute Erinnern auswählen kann, sondern das erneute Erinnern an die Benachrichtigung nur einmal in einem vom System definierten (in allen Betriebssystemen einheitlichen) Zeitintervall erfolgt, legen Sie keinen Wert für &lt;input&gt; fest.
    -   Wenn Sie mögliche Intervalle für das erneute Erinnern bereitstellen möchten:
        -   Gegen Sie „hint-inputId“ in der Aktion für das erneute Erinnerung an.
        -   Stimmen Sie die ID der Eingabe auf den Wert für „hint-inputId“ der Aktion für das erneute Erinnern ab: : &lt;input id="snoozeTime"&gt;&lt;/input&gt;&lt;action hint-inputId="snoozeTime"/&gt;
        -   Legen Sie für die Auswahl-ID eine positive ganze Zahl (nonNegativeInteger) fest, die dem Intervall für das erneute Erinnern in Minuten entspricht: &lt;selection id="240" /&gt; bedeutet, dass die erneute Erinnerung in vier Stunden erfolgt.
        -   Stellen Sie sicher, dass der Wert für „defaultInput“ in &lt;input&gt; einer der IDs der untergeordneten &lt;selection&gt; -Elemente entspricht.
        -   Sie können bis zu (jedoch nicht mehr als) 5 &lt;selection&gt;-Werte bereitstellen