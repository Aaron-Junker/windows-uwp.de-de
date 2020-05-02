---
Description: Sound vervollständigt die Benutzerumgebung einer Anwendung und trägt zur Vermittlung des Windows-Feelings auf allen Plattformen bei.
label: Sound
title: Sound
template: detail.hbs
ms.assetid: 9fa77494-2525-4491-8f26-dc733b6a18f6
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: mattben
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 8ed1344b5ee49244a6c1afcbb873b54fcc28624f
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "75684883"
---
# <a name="sound"></a>Sound

![Herobild](images/header-sound.svg)

Es gibt viele Möglichkeiten, Ihre App mit Sound zu verbessern. Sie können Sound zur Ergänzung von Benutzeroberflächenelementen einsetzen, um Benutzer akustisch auf Ereignisse aufmerksam zu machen. Sound ist beispielsweise für Menschen mit Sehbehinderungen ein hilfreiches Benutzeroberflächenelement. Mit Sound können Sie den Benutzer in das Geschehen einbeziehen, beispielsweise, wenn Sie ein Puzzlespiel mit einer beruhigenden Hintergrundmelodie und ein Horror-/Survivalspiel mit bedrohlichen Soundeffekten unterlegen.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/Sound">die App zu öffnen und Sound in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="sound-global-api"></a>Globale Sound-API

UWP bietet ein einfach zugängliches Soundsystem, bei dem Sie einfach „einen Schalter umlegen“ können und ein beeindruckendes Audioerlebnis für Ihre gesamte App erhalten.

Der [**ElementSoundPlayer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.elementsoundplayer) ist ein integriertes Soundsystem innerhalb von XAML, und wenn es aktiviert ist, spielen alle Standardsteuerelemente automatisch Sounds ab.
```C#
ElementSoundPlayer.State = ElementSoundPlayerState.On;
```
Der **ElementSoundPlayer** verfügt über drei Zustände: **Ein** **Aus** und **Auto**.

In der Einstellung **Aus** wird unabhängig davon, wo Ihre App ausgeführt wird, niemals Sound wiedergegeben. In der Einstellung **Ein** werden Sounds für Ihre App auf jeder Plattform wiedergegeben.

Das Aktivieren des ElementSoundPlayer ermöglicht zudem automatisch räumliches Audio (3D-Sound). Um den 3D-Sound (während weiterhin der Sound eingeschaltet ist) zu deaktivieren, deaktivieren Sie den **SpatialAudioMode** des ElementSoundPlayer: 

```C#
ElementSoundPlayer.SpatialAudioMode = ElementSpatialAudioMode.Off
```

Die **SpatialAudioMode**-Eigenschaft kann folgende Werte annehmen: 
- **Auto**: Räumliches Audio wird eingeschaltet, wenn Sound eingeschaltet ist. 
- **Aus**: Räumliches Audio ist immer ausgeschaltet, selbst wenn Sound eingeschaltet ist.
- **Ein**: Räumliches Audio wird immer wiedergegeben.

Weitere Informationen zu räumlichem Audio und dessen Behandlung durch XAML finden Sie unter [AudioGraph – räumliches Audio](/windows/uwp/audio-video-camera/audio-graphs#spatial-audio).

### <a name="sound-for-tv-and-xbox"></a>Sound für TV und Xbox

Sound ist ein wesentlicher Bestandteil der 10-Fuß-Schnittstelle. Standardmäßig verwendet der **ElementSoundPlayer** den Zustand **Auto**, was bedeutet, dass Sound nur dann wiedergegeben wird, wenn Ihre App auf Xbox ausgeführt wird.
Weitere Informationen zur Funktionsweise von Sound für TV und Xbox finden Sie im Artikel [Entwerfen für Xbox und Fernsehgeräte](https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv?redirectedfrom=MSDN).

## <a name="sound-volume-override"></a>Lautstärkeüberschreibung

Mit dem **Volume**-Steuerelement kann die Lautstärke aller Sounds innerhalb der App verringert werden. Allerdings können Sounds innerhalb der App nicht *lauter als die Systemlautstärke* werden.

Rufen Sie zum Festlegen der Lautstärke der App Folgendes auf:
```C#
ElementSoundPlayer.Volume = 0.5;
```
Wobei die maximale Lautstärke (relativ zur Systemlautstärke) 1,0 und die minimale Lautstärke 0,0 ist (im Wesentlichen stumm).

## <a name="control-level-state"></a>Zustand des Steuerungsgrads

Wenn der Standardsound eines Steuerelements nicht erwünscht ist, kann er deaktiviert werden. Dies erfolgt über die **ElementSoundMode**-Option für das Steuerelement.

Die **ElementSoundMode**-Option verfügt über zwei Zustände: **Aus** und **Standard**. Wenn nichts festgelegt wird, hat sie den Zustand **Standard**. In der Einstellung **Aus** wird jeder Sound, der durch das Steuerelement wiedergegeben werden soll, stumm geschaltet, *außer der Fokus*.

```XAML
<Button Name="ButtonName" Content="More Info" ElementSoundMode="Off"/>
```

```C#
ButtonName.ElementSoundState = ElementSoundMode.Off;
```

## <a name="is-this-the-right-sound"></a>Ist das der richtige Sound?

Beim Erstellen eines benutzerdefinierten Steuerelements oder Ändern des Sounds eines vorhandenen Steuerelements ist es wichtig zu verstehen, wie alle Sounds, die das System bietet, verwendet werden.

Jeder Sound bezieht sich auf eine bestimmte grundlegende Benutzerinteraktion. Obwohl Sounds angepasst werden können, um bei einer Interaktion wiedergegeben zu werden, werden in diesem Abschnitt die Szenarien erläutert, in denen mithilfe von Sounds die Einheitlichkeit aller UWP-Apps gewährleistet werden sollte.

### <a name="invoking-an-element"></a>Aufrufen eines Elements

Der häufigste durch ein Steuerelement ausgelöste Sound in unserem System ist der **Invoke**-Sound. Dieser Sound wird wiedergegeben, wenn ein Benutzer mittels Tippen/Klicken/Eingabe/Leertaste oder Drücken der Taste „A“ auf einem Gamepad ein Steuerelement aufruft.

In der Regel wird dieser Sound nur wiedergegeben, wenn ein Benutzer explizit ein einfaches Steuerelement oder ein Steuerelementteil über ein [Eingabegerät](../input/index.md) aufruft.


Um diesen Sound über ein Steuerelementereignis wiederzugeben, rufen Sie einfach die Play-Methode von **ElementSoundPlayer** auf und übergeben sie an **ElementSound.Invoke**:
```C#
ElementSoundPlayer.Play(ElementSoundKind.Invoke);
```

### <a name="showing--hiding-content"></a>Anzeigen und Ausblenden von Inhalten

In XAML gibt es viele Flyouts, Dialogfelder und leicht ausblendbare Benutzeroberflächen, und jede Aktion, die eine dieser Überlagerungen auslöst, sollte einen **Show**- oder **Hide**-Sound aufrufen.

Wenn ein Überlagerungsinhaltsfenster eingeblendet wird, sollte der **Show**-Sound aufgerufen werden:

```C#
ElementSoundPlayer.Play(ElementSoundKind.Show);
```
Wenn dagegen ein Überlagerungsinhaltsfenster geschlossen wird (oder einfach ausgeblendet wird), sollte der **Hide**-Sound aufgerufen werden:

```C#
ElementSoundPlayer.Play(ElementSoundKind.Hide);
```
### <a name="navigation-within-a-page"></a>Navigation innerhalb einer Seite

Beim Navigieren zwischen Bereichen oder Ansichten innerhalb einer App-Seite (siehe [Registerkarten und Pivots](../controls-and-patterns/pivot.md)) gibt es in der Regel eine bidirektionale Bewegung. Das bedeutet, dass Sie zur nächsten Ansicht bzw. zum nächsten Bereich (oder zur/zum vorherigen) wechseln können, ohne die aktuelle App-Seite zu verlassen, auf der Sie sich befinden.

Das Audioerlebnis für dieses Navigationskonzept wird durch den **MovePrevious**- und **MoveNext**-Sound umgesetzt.

Beim Wechsel zu einer Ansicht bzw. zu einem Bereich, die/der als das *nächste Element* in einer Liste gilt, rufen Sie Folgendes auf:

```C#
ElementSoundPlayer.Play(ElementSoundKind.MoveNext);
```
Und beim Wechsel zu einer vorherigen Ansicht bzw. zu einem vorherigen Bereich in einer Liste, die/der als das *vorherige Element* gilt, rufen Sie Folgendes auf:

```C#
ElementSoundPlayer.Play(ElementSoundKind.MovePrevious);
```
### <a name="back-navigation"></a>Rückwärtsnavigation

Beim Navigieren von der aktuellen Seite zur vorherigen Seite innerhalb einer App sollte der **GoBack**-Sound aufgerufen werden:

```C#
ElementSoundPlayer.Play(ElementSoundKind.GoBack);
```
### <a name="focusing-on-an-element"></a>Fokussieren auf ein Element

Der **Focus**-Sound ist der einzige implizite Sound in unserem System. Das heißt, dass ein Benutzer nicht mit irgendetwas direkt interagiert, jedoch trotzdem einen Sound hört.

Das Fokussieren geschieht, wenn ein Benutzer durch eine App navigiert – dies kann mit dem Gamepad, der Tastatur, der Fernbedienung oder mit Kinect geschehen. In der Regel wird der **Focus**-Sound *bei PointerEntered- oder Mauszeigeereignissen nicht wiedergegeben*.

Um ein Steuerelement zur Wiedergabe des **Focus**-Sounds einzurichten, wenn Ihr Steuerelement den Fokus erhält, rufen Sie Folgendes auf:

```C#
ElementSoundPlayer.Play(ElementSoundKind.Focus);
```
### <a name="cycling-focus-sounds"></a>Wechseln zwischen Focus-Sounds

Als zusätzliche Funktion zum Aufrufen von **ElementSound.Focus** wechselt das Soundsystem standardmäßig bei jedem Navigationsaufruf zwischen 4 unterschiedlichen Sounds. Das bedeutet, dass keine zwei genau gleichen Focus-Sounds direkt nacheinander wiedergegeben werden.

Diese Wechselfunktion soll verhindern, dass die Focus-Sounds monoton werden und vom Benutzer nicht mehr wahrgenommen werden. Denn Focus-Sounds werden am häufigsten wiedergegeben und sollten daher möglichst unaufdringlich sein.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

* [Entwerfen für Xbox und Fernsehgeräte](/windows/uwp/design/devices/designing-for-tv)
* [Dokumentation zur ElementSoundPlayer-Klasse](/uwp/api/windows.ui.xaml.elementsoundplayer)
