---
title: Unterstützung von natürlicheren Sprachbefehlen in Cortana-Cortana UWP-Entwurf und-Entwicklung
description: Erweitern Sie Cortana mit flexibleren Befehlen in natürlicher Sprache, bei denen der Benutzer den App-Namen an beliebiger Stelle im Befehl verwenden kann.
author: kbridge
label: Conceptual
ms.assetid: c2959c1b-c2f2-4a8d-8f3e-79585f69afcf
ms.date: 01/28/2021
ms.topic: article
keywords: Cortana
ms.openlocfilehash: 94f3d6323649737f978128e442c95449bce94075
ms.sourcegitcommit: d7efd35c1749f695aebbc0db99d8b62b70fb72da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2021
ms.locfileid: "99057792"
---
# <a name="support-natural-language-voice-commands-in-cortana"></a>Unterstützen von Sprachbefehlen in natürlicher Sprache in Cortana

>[!WARNING]
> Diese Funktion wird ab dem Windows 10-Update vom Mai 2020 (Version 2004, Codename "20h1") nicht mehr unterstützt.

Erweitern Sie **Cortana** mit flexibleren Befehlen in natürlicher Sprache, bei denen der Benutzer den App-Namen an beliebiger Stelle im Befehl verwenden kann.

> [!NOTE]
> **Wichtige APIs**
>
> - [**Windows.ApplicationModel.VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD elements and attributes v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

Zur Verwendung von Sprachbefehlen, um Cortana mit Funktionen aus Ihrer App zu erweitern, muss der Benutzer sowohl die App als auch den auszuführenden Befehl bzw. die auszuführende Funktion angeben. Hierzu wird zu Beginn oder zu Ende des Sprachbefehls üblicherweise zunächst der App-Name genannt. Also beispielsweise: "Adventure Works, neue Reise nach Las Vegas hinzufügen".

Manchmal klingt der Befehl aber dadurch vielleicht merkwürdig, unnatürlich oder ergibt womöglich überhaupt keinen Sinn, wenn zuerst der Anwendungsname und dann der Befehl angegeben wird. Häufig ist es praktischer und natürlicher, den App-Namen an einer anderen Stelle innerhalb des Befehls zu nennen, was die Interaktion für den Benutzer deutlich intuitiver und angenehmer macht. Das vorherige Beispiel "Adventure Works, neue Reise nach Las Vegas hinzufügen" ließe sich beispielsweise wie folgt umformulieren: "Neue Adventure Works-Reise nach Las Vegas hinzufügen" oder "Neue Reise nach Las Vegas hinzufügen mit Adventure Works".

Der App-Name kann bei Sprachbefehlen wie folgt verwendet werden:

- Als Präfix (vor dem Befehl)
- Als Infix (innerhalb des Befehls)
- Als Suffix (nach dem Befehl)

> [!TIP]
> **Voraussetzungen**
>
> Dieses Thema baut auf [dem Aktivieren einer Hintergrund-app in Cortana mithilfe von Sprachbefehlen](cortana-launch-a-background-app-with-voice-commands.md)auf. Wir zeigen hier weitere Features anhand einer Reiseplanungs- und Verwaltungs-App mit dem Namen **Adventure Works**.
>
> Wenn Sie noch keine Erfahrung mit der Entwicklung von UWP-Apps (Universelle Windows-App) haben, sollten Sie sich in den folgenden Themen zunächst mit den hier besprochenen Technologien vertraut machen.
>
> - [Erste App erstellen](/windows/uwp/get-started/your-first-app)
> - Informationen zu Ereignissen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](/windows/uwp/xaml-platform/events-and-routed-events-overview).
>
> **Richtlinien zur Benutzer Leistung**
>
> Unter [Cortana-Entwurfs Richtlinien](cortana-design-guidelines.md)  finden Sie Informationen dazu, wie Sie Ihre APP mit **Cortana** und [sprach Interaktionen](speech-interactions.md) integrieren, um hilfreiche Tipps zum Entwerfen einer nützlichen und ansprechenden sprachfähigen APP zu erhalten.

## <a name="specify-an-appname-element-in-the-vcd"></a>Angeben eines **AppName**-Elements in der VCD-Datei

Mithilfe des **AppName**-Elements wird in einem Sprachbefehl ein benutzerfreundlicher Name für eine App angegeben.

```XML
<AppName>Adventure Works</AppName>
```

## <a name="specify-where-the-app-name-can-be-spoken-in-the-voice-command"></a>Angeben der Position, an welcher der App-Name innerhalb des Sprachbefehls gesagt werden kann

Das **ListenFor**-Element verfügt über ein **RequireAppName**-Attribut, das angibt, wo der App-Name innerhalb des Sprachbefehls verwendet werden kann. Dieses Attribut unterstützt vier Werte:

1. **BeforePhrase**

   Standard.

   Gibt an, dass der Benutzer den Namen Ihrer App vor dem Befehl sagen muss.

   Hier hört Cortana auf "Adventure Works, wann ist meine Reise nach Las Vegas".

   ```xml
   <ListenFor RequireAppName="BeforePhrase"> show [my] trip to {destination} </ListenFor>
   ```

2. **AfterPhrase**

    Gibt an, dass der Benutzer den Namen Ihrer App nach dem Befehl sagen muss.

    Das System stellt eine lokalisierte Begriffsliste mit präpositionalen Konjunktionen bereit. Diese enthält Ausdrücke wie "mithilfe von", "mit" und "in".

    Hier hört Cortana auf Befehle wie "Zeige meine nächste Reise nach Las Vegas in Adventure Works" oder "Zeige meine nächste Reise nach Las Vegas mit Adventure Works".

    ```xml
    <ListenFor RequireAppName="AfterPhrase">show [my] next trip to {destination} </ListenFor>
    ```

3. **BeforeOrAfterPhrase**

    Gibt an, dass der Benutzer den Namen Ihrer App vor oder nach dem Befehl sagen muss.

    Für die Suffix-Variante stellt das System eine lokalisierte Begriffsliste mit präpositionalen Konjunktionen bereit. Diese enthält Ausdrücke wie "mithilfe von", "mit" und "in".

    Hier hört Cortana auf Befehle wie "Adventure Works, zeige meine nächste Reise nach Las Vegas" oder "Zeige meine nächste Reise nach Las Vegas in Adventure Works".

    ``` xml
    <ListenFor RequireAppName="BeforeOrAfterPhrase">show [my] next trip to {destination}</ListenFor>
    ```

4. **ExplicitlySpecified**

    Gibt an, dass der Benutzer den Namen Ihrer App genau an der vorgegebenen Stelle des Befehls sagen muss. Der Benutzer muss den Namen der App nicht vor oder nach dem Befehl sagen.

    Auf den App-Namen muss explizit mit dem **{builtin:AppName}**-Tag verwiesen werden.

    Hier hört Cortana auf Befehle wie "Adventure Works, zeige meine nächste Reise nach Las Vegas" oder "Zeige meine nächste Adventure Works-Reise nach Las Vegas".

    ```xml
    <ListenFor RequireAppName="ExplicitlySpecified">show [my] next {builtin:AppName} trip to {destination} </ListenFor>
    ```

## <a name="special-cases"></a>Spezialfälle

Wenn Sie ein **ListenFor**-Element deklarieren, bei dem **RequireAppName** entweder auf "AfterPhrase" oder auf "ExplicitlySpecified" festgelegt ist, müssen bestimmte Anforderungen erfüllt werden:

1. **{builtin:AppName}** muss genau einmal vorkommen, wenn **RequireAppName** auf „ExplicitlySpecified“ festgelegt ist.

    Bei diesem Wert kann das System nicht ableiten, an welcher Position der App-Name innerhalb des Sprachbefehls verwendet werden kann. Die Position muss explizit angegeben werden.

2. Ein Sprachbefehl darf nicht mit einem **PhraseTopic**-Element beginnen. Dieses Element wird in der Regel für die Spracherkennung mit umfangreichem Vokabular verwendet. Es muss mindestens ein Wort vorangestellt werden.

    Dies macht es unwahrscheinlicher, dass **Cortana** Ihre App startet, wenn der Name Ihrer App (oder ein Teil des Namens) an beliebiger Stelle in einem Sprachbefehl auftaucht.

    Hier sehen Sie eine ungültige Deklaration, bei der die Gefahr besteht, dass **Cortana** die App **Adventure Works** startet, wenn der Benutzer etwas sagt wie "Show me reviews for Kinect adventure works (Rezensionen für Kinect Adventure-Inhalte anzeigen)".
  
    ```xml
    <ListenFor RequireAppName="ExplicitlySpecified">{searchPhrase} {builtin:AppName}</ListenFor>
    ```

3. Neben dem Namen Ihrer App und Verweisen auf **PhraseTopic**-Elemente muss die **ListenFor**-Zeichenfolge mindestens zwei Wörter enthalten.

    Ähnlich wie im zweiten Fall müssen Sie sicherstellen, dass Ihre Befehle über genügend phonetischen Inhalt verfügen, um ein unabsichtliches Starten Ihrer App zu vermeiden.

    Dadurch können Sie Ihre Anwendung optimal einrichten und dafür sorgen, dass sie nicht ungewollt gestartet wird, wenn ein Benutzer beispielsweise „Find Kinect Adventure works“ (Kinect Adventure-Inhalte suchen) sagt.

    Hier sehen Sie ungültige Deklarationen, bei denen die Gefahr besteht, dass **Cortana** die App **Adventure Works** startet, wenn der Benutzer etwas sagt wie "Hey Adventure Works" oder "Find Kinect adventure works (Kinect Adventure-Inhalte suchen)".

    ```xml
    <ListenFor RequireAppName="ExplicitlySpecified">Hey {builtin:AppName}</ListenFor>
    <ListenFor RequireAppName="ExplicitlySpecified">Find {searchPhrase} {builtin:AppName}</ListenFor>
    ```

## <a name="remarks"></a>Bemerkungen

Durch die Unterstützung zusätzlicher Sprachbefehlsvariationen für **Cortana** erhöht sich auch die allgemeine Benutzerfreundlichkeit Ihrer App.

Vermeiden Sie "Hey \[ App Name \] " als **appname**. Benutzer sind viel wahrscheinlicher, dass Sie "Hey Cortana" aufrufen, um Cortana über die Sprach Aktivierung aufzurufen, und dass "Hey \[ App Name \] " in der Äußerung nicht ganz natürlich klingt. Beispiel: "Hey Cortana, zeige meine nächste Reise nach Las Vegas in Hey Adventure Works".

Erweitern Sie Ihre vorhandenen Sprachbefehle ggf. um Infix-/Suffix-Varianten. Wie Sie gesehen haben, können Sie Ihren vorhandenen **ListenFor**-Elementen ohne großen Aufwand ein zusätzliches Attribut hinzufügen und so Suffix-Varianten unterstützen. "Hey Cortana, zeige meine nächste Reise nach Las Vegas in Adventure Works" ist einfach deutlich natürlicher als "Hey Cortana, Adventure Works, zeige meine nächste Reise nach Las Vegas".

Verwenden Sie in Situationen, in denen sich ein Konflikt mit vorhandenen Funktionen von **Cortana** ergibt (also beispielsweise bei Anrufen, Nachrichten oder Ähnlichem), den Namen Ihrer App ggf. als Präfix. Beispiel: "Adventure Works, Message \[ Travel Agent \] about Las Vegas Trip".

## <a name="complete-example"></a>Vollständiges Beispiel

Hier sehen Sie eine VCD-Datei, die verschiedene Möglichkeiten für natürlichere Sprachbefehle veranschaulicht:

> [!NOTE]
> Es ist zulässig, dass mehrere **listenfor** -Elemente vorhanden sind, die jeweils einen anderen Wert für das Attribut "Requirements **appname** " aufweisen.

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="commandSet_en-us">
    <AppName>Adventure Works</AppName>
    <Example> When is my trip to Las Vegas? </Example>
    <Command Name="whenIsTripToDestination">
      <Example> When is my trip to Las Vegas?</Example>
      <ListenFor RequireAppName="BeforePhrase">
        when is my] trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands like 
           “Show my next trip to Las Vegas on Adventure Works”; “Show my next 
           trip to Las Vegas using Adventure Works” -->
      <ListenFor RequireAppName="AfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands when 
           the user specifies your app name either before or after the command. 
           “Adventure Works, show my next trip to Las Vegas”; 
           “Show my next trip to Last Vegas on Adventure works” -->
      <ListenFor RequireAppName="BeforeOrAfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands 
           when the user specifies your app name inline. 
           “Show my next Adventure Works trip to Las Vegas” -->
      <ListenFor RequireAppName="ExplicitlySpecified">
        show [my] next {builtin:AppName} trip to {destination} </ListenFor>

      <Feedback> Looking for trip to {destination} </Feedback>
      <Navigate />
    </Command>
    <PhraseList Label="destination">
      <Item> Las Vegas </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>
  </CommandSet>
  <!-- Other CommandSets for other languages -->
</VoiceCommands>
```

## <a name="related-articles"></a>Verwandte Artikel

- [Cortana-Interaktionen in Windows-apps](cortana-interactions.md)
- [Cortana-Entwurfs Richtlinien](cortana-design-guidelines.md)
- [VCD elements and attributes v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana-Sprachbefehlbeispiel](https://go.microsoft.com/fwlink/p/?LinkID=619899)
