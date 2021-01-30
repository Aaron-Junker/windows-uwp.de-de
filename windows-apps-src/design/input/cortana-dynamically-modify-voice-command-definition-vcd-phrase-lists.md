---
title: Dynamisches Ändern von Cortana VCD-Ausdrucks Listen-Cortana UWP-Entwurf und-Entwicklung
description: Erfahren Sie, wie Sie mithilfe des Spracherkennungsergebnisses zur Laufzeit auf die Liste der unterstützten Begriffe (PhraseList-Elemente) in einer VCD-Datei (Voice Command Definition) zugreifen und diese aktualisieren
ms.assetid: b497145b-c7a0-454a-8329-6bc1228953bb
ms.date: 01/28/2021
ms.topic: article
keywords: Cortana
ms.openlocfilehash: 9f9e0aeb1cf23eb64df3104cf1f90e2b30d083ba
ms.sourcegitcommit: d7efd35c1749f695aebbc0db99d8b62b70fb72da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2021
ms.locfileid: "99057809"
---
# <a name="dynamically-modify-cortana-vcd-phrase-lists"></a>Dynamisches Ändern von Cortana VCD-Ausdrucks Listen

>[!WARNING]
> Diese Funktion wird ab dem Windows 10-Update vom Mai 2020 (Version 2004, Codename "20h1") nicht mehr unterstützt.

Erfahren Sie, wie Sie mithilfe des Spracherkennungsergebnisses zur Laufzeit auf die Liste der unterstützten Begriffe (**PhraseList**-Elemente) in einer VCD-Datei (Voice Command Definition) zugreifen und diese aktualisieren

> [!NOTE]
> Ein Sprachbefehl ist eine einzelne Äußerung mit einer bestimmten Absicht, die in einer sprach Befehls Definitionsdatei (VCD) definiert ist, die an eine installierte App über **Cortana** gerichtet ist.
>
> Eine VCD-Datei definiert einen oder mehrere Sprachbefehle, die jeweils einem bestimmten Zweck dienen.
>
> Sprach Befehlsdefinitionen können von der Komplexität abweichen. Sie können alles von einer einzelnen, eingeschränkten Äußerung zu einer Sammlung flexibler, natürlicher Spracheingaben unterstützen, die alle dieselbe Absicht bezeichnen.

Das dynamische Ändern einer Begriffsliste zur Laufzeit ist nützlich, wenn der Sprachbefehl sich speziell auf eine Aufgabe bezieht, die benutzerdefinierte oder transiente App-Daten umfasst.

> [!NOTE]
> **Wichtige APIs**
>
> - [**Windows.ApplicationModel.VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD elements and attributes v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

Angenommen, Sie verfügen über eine Reise-app, in der Benutzer Ziele eingeben können, und Sie möchten, dass Benutzer die app starten können, indem Sie den Namen der APP, gefolgt von "Trip to &lt; Destination anzeigen", sagen &gt; . Im **ListenFor**-Element selbst geben Sie Folgendes an: `<ListenFor> Show trip to {destination}  </ListenFor>`, wobei „Ziel“ der Wert des Attributs **Label** für die **PhraseList** ist.

Durch das Aktualisieren der Begriffsliste zur Laufzeit muss kein eigenes **ListenFor**-Element für jedes mögliche Ziel mehr erstellt werden. Stattdessen können Sie **PhraseList** dynamisch mit Zielen auffüllen, die vom Benutzer beim Eingeben der Reiseroute angegeben werden.

Weitere Informationen zu **PhraseList** und anderen VCD-Elementen finden Sie in unter [**VCD elements and attributes v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2).

> [!TIP]
> **Voraussetzungen**
>
> Wenn Sie noch keine Erfahrung mit der Entwicklung von UWP-Apps (Universelle Windows-App) haben, sollten Sie sich in den folgenden Themen zunächst mit den hier besprochenen Technologien vertraut machen.
>
> - [Erste App erstellen](/windows/uwp/get-started/your-first-app)
> - Informationen zu Ereignissen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](/windows/uwp/xaml-platform/events-and-routed-events-overview).
>
> **Richtlinien zur Benutzer Leistung**
>
> Unter [Cortana-Entwurfs Richtlinien](cortana-design-guidelines.md)  finden Sie Informationen dazu, wie Sie Ihre APP mit **Cortana** und [sprach Interaktionen](speech-interactions.md) integrieren, um hilfreiche Tipps zum Entwerfen einer nützlichen und ansprechenden sprachfähigen APP zu erhalten.

## <a name="identify-the-command-and-update-the-phrase-list"></a>Identifizieren des Befehls und Aktualisieren der Begriffsliste

Diese VCD-Beispieldatei definiert den **Befehl** „showTripToDestination“ und eine **PhraseList**, die drei Optionen für Ziele in der Reise-App **Adventure Works** definiert. Wenn der Benutzer Ziele in der App speichert oder löscht, aktualisiert die App die Optionen in der **PhraseList**.

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works, </AppName>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> show trip to London  </Example>
      <ListenFor> show trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate/>
    </Command>

    <PhraseList Label="destination">
      <Item> London </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>

  </CommandSet>

<!-- Other CommandSets for other languages -->

</VoiceCommands>
```

Wenn Sie ein **PhraseList**-Element in der VCD-Datei aktualisieren möchten, rufen Sie das **CommandSet**-Element ab, das die Begriffsliste enthält. Verwenden Sie das Attribut **Name** für dieses **CommandSet**-Element (**Name** muss in der VCD-Datei eindeutig sein) als Schlüssel für den Zugriff auf die Eigenschaft [**VoiceCommandManager.InstalledCommandSets**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandManager), und rufen Sie die [**VoiceCommandSet**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandSet)-Referenz ab.

Nachdem Sie den Befehlssatz identifiziert haben, rufen Sie eine Referenz zu der Begriffsliste ab, die Sie ändern möchten, und rufen Sie die [**SetPhraseListAsync**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandSet)-Methode mit dem **Label**-Attribut des **PhraseList**-Elements und einem Array von Zeichenfolgen als neuen Inhalt der Begriffsliste auf.

> [!NOTE]
> Wenn Sie eine Ausdrucks Liste ändern, wird die gesamte Ausdrucks Liste ersetzt. Wenn Sie neue Elemente in eine Begriffsliste einfügen möchten, müssen Sie im Aufruf von [**SetPhraseListAsync**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandSet) sowohl die vorhandenen Elemente als auch die neuen Elemente angeben.

In diesem Beispiel wird die im vorherigen Beispiel gezeigte **PhraseList** mit dem zusätzlichen Reiseziel „Phoenix“ aktualisiert.

```CSharp
Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinition.VoiceCommandSet commandSetEnUs;

if (Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
      InstalledCommandSets.TryGetValue(
        "AdventureWorksCommandSet_en-us", out commandSetEnUs))
{
  await commandSetEnUs.SetPhraseListAsync(
    "destination", new string[] {“London”, “Dallas”, “New York”, “Phoenix”});
}
```

## <a name="remarks"></a>Bemerkungen

Eine **PhraseList** eignet sich zur Einschränkung der Erkennung für einen relativ kleinen Satz oder Wörter. Wenn der Wörtersatz zu groß ist (z. B. mehrere hundert Wörter umfasst) oder nicht eingeschränkt werden soll, verwenden Sie das **PhraseTopic**-Element und ein **Subject**-Element zum Einschränken der Relevanz der Spracherkennungsergebnisse, um die Skalierbarkeit zu verbessern.

In unserem Beispiel haben wir ein **phrasetopic** mit dem **Szenario** "Search", das durch den **Betreff** "City State" weiter verfeinert wird \\ .

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works, </AppName>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> show trip to London  </Example>
      <ListenFor> show trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate/>
    </Command>

    <PhraseList Label="destination">
      <Item> London </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>

    <PhraseTopic Label="destination" Scenario="Search">
      <Subject>City/State</Subject>
    </PhraseTopic>

  </CommandSet>
```

## <a name="related-articles"></a>Verwandte Artikel

- [Cortana-Interaktionen in Windows-apps](cortana-interactions.md)
- [VCD elements and attributes v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Aktivieren einer Vordergrund-App mit Sprachbefehlen über Cortana](cortana-launch-a-foreground-app-with-voice-commands.md)
- [Aktivieren einer Hintergrund-app in Cortana mithilfe von Sprachbefehlen](cortana-launch-a-background-app-with-voice-commands.md)
- [Cortana-Entwurfs Richtlinien](cortana-design-guidelines.md)
- [Cortana-Sprachbefehlbeispiel](https://go.microsoft.com/fwlink/p/?LinkID=619899)
