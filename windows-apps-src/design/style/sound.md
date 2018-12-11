---
Description: Sound helps complete an application's user experience, and gives them that extra audio edge they need to match the feel of Windows across all platforms.
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
ms.openlocfilehash: 74d1d5b04b13795a075e7111ed898243ed59e7b7
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "8877078"
---
# <a name="sound"></a>Sound

![Favoritenbild](images/header-sound.svg)

Es gibt viele Möglichkeiten, Ihre App mit Sound zu verbessern. Sie können Sound zur Ergänzung von Benutzeroberflächenelementen einsetzen, um Benutzer akustisch auf Ereignisse aufmerksam zu machen. Sound ist beispielsweise für Menschen mit Sehbehinderungen ein hilfreiches Benutzeroberflächenelement. Mit Sound können Sie den Benutzer in das Geschehen einbeziehen, beispielsweise, wenn Sie ein Puzzlespiel mit einer beruhigenden Hintergrundmelodie und ein Horror-/Survivalspiel mit bedrohlichen Soundeffekten unterlegen.

## <a name="sound-global-api"></a>Globale Sound-API

UWP bietet ein einfach zugängliches Soundsystem, bei dem Sie einfach „einen Schalter umlegen“ können und ein beeindruckendes Audioerlebnis für Ihre gesamte App erhalten.

Der [**ElementSoundPlayer**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.elementsoundplayer) ist ein integriertes Soundsystem innerhalb von XAML, und wenn es aktiviert ist, spielen alle Standardsteuerelemente automatisch Sounds ab.
```C#
ElementSoundPlayer.State = ElementSoundPlayerState.On;
```
Der **ElementSoundPlayer** verfügt über drei Zustände: **Ein**, **Aus** und **Auto**.

In der Einstellung **Aus** wird unabhängig davon, wo Ihre App ausgeführt wird, niemals Sound wiedergegeben. In der Einstellung **Ein** werden für Sounds für Ihre App auf jeder Plattform wiedergegeben.

Das Aktivieren des ElementSoundPlayer ermöglicht automatisch räumliches Audio (3D-Sound). Um den 3D-Sound (während weiterhin der Sound eingeschaltet ist) zu deaktivieren, deaktivieren Sie die **SpatialAudioMode** vom ElementSoundPlayer: 

```C#
ElementSoundPlayer.SpatialAudioMode = ElementSpatialAudioMode.Off
```

Die **SpatialAudioMode**-Eigenschaft kann diese Werte erhalten: 
- **Automatische**: räumliches Audio wird eingeschaltet, wenn der Ton eingeschaltet ist. 
- **Deaktiviert**: räumliches Audio ist immer ausgeschaltet, selbst wenn Sound eingeschaltet ist.
- **Ein**: räumliches Audio wird immer wiedergegeben.

Weitere Informationen zu räumlichem Audio und wie XAML diesen behandelt finden Sie unter [AudioGraph – räumliches Audio](/windows/uwp/audio-video-camera/audio-graphs#spatial-audio).

### <a name="sound-for-tv-and-xbox"></a>Sound für TV und Xbox

Sound ist ein wesentlicher Bestandteil der 10-Fuß-Schnittstelle. Standardmäßig verwendet der **ElementSoundPlayer** den Zustand **Auto**, was bedeutet, dass Sound nur dann wiedergegeben wird, wenn Ihre App auf Xbox ausgeführt wird.
Weitere Informationen zur Funktionsweise von Sound für TV und Xbox finden Sie im Artikel [Entwerfen für Xbox und Fernsehgeräte](http://go.microsoft.com/fwlink/?LinkId=760736).

## <a name="sound-volume-override"></a>Lautstärkeüberschreibung

Mit dem **Volume**-Steuerelement kann die Lautstärke aller Sounds innerhalb der App verringert werden. Allerdings können Sounds innerhalb der App nicht *lauter als die Systemlautstärke* werden.

Rufen Sie zum Festlegen der Lautstärke der App auf:
```C#
ElementSoundPlayer.Volume = 0.5;
```
Wobei die maximale Lautstärke (relativ zur Systemlautstärke) 1,0 ist und die minimale 0,0 ist (im Wesentlichen stumm).

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

Jeder Sound bezieht sich auf eine bestimmte grundlegende Benutzerinteraktion, und obwohl Sounds angepasst werden können, um bei einer Interaktion wiedergegeben zu werden, soll dieser Abschnitt die Szenarien erläutern, in denen Sounds verwendet werden sollten, um die Einheitlichkeit aller UWP-Apps zu gewährleisten.

### <a name="invoking-an-element"></a>Aufrufen eines Elements

Der in unserem System am häufigsten verwendete durch ein Steuerelement ausgelöste Sound ist der **Invoke**-Sound. Dieser Sound wird wiedergegeben, wenn ein Benutzer mittels Tippen/Klicken/Eingabe/Leertaste oder Drücken der Taste "A" auf einem Gamepad ein Steuerelement aufruft.

In der Regel wird dieser Sound nur wiedergegeben, wenn ein Benutzer explizit ein einfaches Steuerelement oder ein Steuerelementteil über ein [Eingabegerät](../input/index.md) anzielt.

<SelectButtonClick.mp3 Audioclip hier>

Um diesen Sound von einem Steuerelementereignis aus wiederzugeben, rufen Sie einfach die Play-Methode aus dem **ElementSoundPlayer** auf und übergeben sie an **ElementSound.Invoke**:
```C#
ElementSoundPlayer.Play(ElementSoundKind.Invoke);
```

### <a name="showing--hiding-content"></a>Anzeigen und Ausblenden von Inhalten

In XAML gibt es viele Flyouts, Dialogfelder und leicht ausblendbare Benutzeroberflächen, und jede Aktion, die eine dieser Überlagerungen auslöst, sollte einen **Show**- oder **Hide**-Sound aufrufen.

Wenn ein Überlagerungs-Inhaltsfenster eingeblendet wird, sollte der **Show**-Sound aufgerufen werden:

<OverlayIn.mp3 Audioclip hier>

```C#
ElementSoundPlayer.Play(ElementSoundKind.Show);
```
Wenn dagegen ein Überlagerungs-Inhaltsfenster geschlossen wird (oder einfach ausgeblendet wird), sollte der **Hide**-Sound aufgerufen werden:

<OverlayOut.mp3 Audioclip hier>

```C#
ElementSoundPlayer.Play(ElementSoundKind.Hide);
```
### <a name="navigation-within-a-page"></a>Navigation innerhalb einer Seite

Beim Navigieren zwischen Bereichen oder Ansichten innerhalb einer App-Seite (siehe [Hub](../controls-and-patterns/hub.md) oder [Registerkarten und Pivots](../controls-and-patterns/tabs-pivot.md)) gibt es in der Regel eine bidirektionale Bewegung. Das bedeutet, dass Sie zur nächsten Ansicht bzw. zum nächsten Bereich (oder zur/zum vorherigen) wechseln können, ohne die aktuelle App-Seite zu verlassen, auf der Sie sich befinden.

Das Audioerlebnis für dieses Navigationskonzept wird durch die **MovePrevious**- und **MoveNext**-Sounds umgesetzt.

Beim Wechsel zu einer Ansicht bzw. zu einem Bereich, die/der als das *nächste Element* in einer Liste gilt, rufen Sie auf:

<PageTransitionRight.mp3 Audioclip hier>

```C#
ElementSoundPlayer.Play(ElementSoundKind.MoveNext);
```
Und beim Wechsel zu einer vorherigen Ansicht bzw. zu einem vorherigen Bereich in einer Liste, die/der als das *vorherige Element* gilt, rufen Sie auf:

<PageTransitionLeft.mp3 Audioclip hier>

```C#
ElementSoundPlayer.Play(ElementSoundKind.MovePrevious);
```
### <a name="back-navigation"></a>Rückwärtsnavigation

Beim Navigieren von der aktuellen Seite zur vorherigen Seite innerhalb einer App sollte der **GoBack**-Sound aufgerufen werden:

<BackButtonClick.mp3 Audioclip hier>

```C#
ElementSoundPlayer.Play(ElementSoundKind.GoBack);
```
### <a name="focusing-on-an-element"></a>Fokussieren auf ein Element

Der **Focus**-Sound ist der einzige implizite Sound in unserem System. Das heißt, dass ein Benutzer nicht mit irgendetwas direkt interagiert, jedoch trotzdem einen Sound hört.

Das Fokussieren geschieht, wenn ein Benutzer durch eine App navigiert– dies kann mit dem Gamepad, der Tastatur, der Fernbedienung oder mit Kinect geschehen. In der Regel wird der **Focus**-Sound *bei PointerEntered- oder Mauszeigeereignissen nicht wiedergegeben*.

Um ein Steuerelement zur Wiedergabe des **Focus**-Sounds einzurichten, wenn Ihr Steuerelement den Fokus erhält, rufen Sie auf:

<ElementFocus1.mp3 Audioclip hier>

```C#
ElementSoundPlayer.Play(ElementSoundKind.Focus);
```
### <a name="cycling-focus-sounds"></a>Wechseln zwischen Focus-Sounds

Als zusätzliche Funktion zum Aufrufen von **ElementSound.Focus** wechselt das Soundsystem standardmäßig bei jedem Navigationsaufruf zwischen 4 unterschiedlichen Sounds. Das bedeutet, dass keine zwei genau gleichen Focus-Sounds direkt nacheinander wiedergegeben werden.

Diese Wechselfunktion soll verhindern, dass die Focus-Sounds monoton werden und durch den Benutzer nicht mehr wahrgenommen werden. Denn Focus-Sounds werden am häufigsten wiedergegeben und sollten daher möglichst unaufdringlich sein.

## <a name="related-articles"></a>Verwandte Artikel

* [Entwerfen für Xbox und Fernsehgeräte](http://go.microsoft.com/fwlink/?LinkId=760736)
* [Dokumentation zur ElementSoundPlayer-Klasse](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.elementsoundplayer)
