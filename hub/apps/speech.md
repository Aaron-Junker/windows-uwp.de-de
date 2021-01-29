---
title: Sprache, Stimme und Konversation in Windows 10
description: Auf dieser Seite finden Sie Informationen zu den ersten Schritten bei der Entwicklung von sprachfähigen Windows-apps.
ms.topic: article
ms.date: 09/12/2019
keywords: Sprache in Windows 10, Speech, Voice, Conversation, Win32 Speech apps, UWP Speech apps, WPF Speech apps, WinForms Speech apps
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.openlocfilehash: d157d33316925db6112ab892e13b2c849cfcaa5d
ms.sourcegitcommit: d51c3dd64d58c7fa9513ba20e736905f12df2a9a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98988702"
---
# <a name="speech-voice-and-conversation-in-windows-10"></a>Sprache, Stimme und Konversation in Windows 10

![Sprach herabbild](images/hero-speech-composite-small.png)

Die Sprache kann eine effektive, natürliche und angenehme Methode für die Interaktion von Benutzern mit Ihren Windows-Anwendungen sein, die auf der Maus, Tastatur, Toucheingabe, dem Controller oder Gesten basiert, um traditionelle Interaktionen zu ergänzen oder sogar zu ersetzen.

Sprachbasierte Funktionen, wie z. b. Spracherkennung, Diktat, Sprachsynthese (auch bekannt als Text-zu-Sprache oder TTS) und einschließende sprach-Assistenten (z. b. Cortana oder Alexa) können barrierefreie und inklusivbenutzer Umgebungen bereitstellen, die es Benutzern ermöglichen, Ihre Anwendungen zu verwenden, wenn andere Eingabegeräte möglicherweise nicht ausreichen.

Diese Seite enthält Informationen dazu, wie die verschiedenen Windows-Entwicklungs Frameworks Spracherkennung, Sprachsynthese und Unterstützung von Konversation für Entwickler bereitstellen, die Windows-Anwendungen erstellen.

## <a name="platform-specific-documentation"></a>Plattformspezifische Dokumentation

:::row:::
   :::column:::
      ![Universelle Windows-Plattform (UWP)](images/platform-uwp.png)

      **Universelle Windows-Plattform (UWP)**

      Erstellen Sie sprach fähige apps auf der modernen Plattform für Windows 10-Anwendungen und-Spiele auf allen Windows-Geräten (einschließlich PCs, Smartphones, Xbox One, hololens usw.), und veröffentlichen Sie Sie auf dem Microsoft Store.

      [Sprachinteraktionen](/windows/uwp/design/input/speech-interactions)

      [Spracherkennung](/windows/uwp/design/input/speech-recognition)

      [Kontinuierliches Diktieren](/windows/uwp/design/input/enable-continuous-dictation)

      [Sprachsynthese](/uwp/api/windows.media.speechsynthesis)

      [Konversations-Agents](/uwp/api/windows.applicationmodel.conversationalagent)

      [Cortana-Sprachbefehle](/cortana/voice-commands/vcd)
   :::column-end:::
   :::column:::
      ![Win32-Plattform-Apps](images/platform-win32.png)

      **Win32-Plattform**

      Entwickeln Sie sprach aktivierte Anwendungen für Windows Desktop und Windows Server mithilfe der hier bereitgestellten Tools, Informationen und Beispiel-Engines und Anwendungen.

      [Microsoft Speech Platform – Software Development Kit (SDK) (Version 11)](https://www.microsoft.com/download/details.aspx?id=27226)
      
      [Microsoft Speech SDK, Version 5.1](https://www.microsoft.com/download/details.aspx?id=10121)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      ![.NET](images/platform-dotnet.png)

      **.NET Framework**

      Entwickeln Sie barrierefreie Apps und Tools auf der bekannten Plattform für verwaltete Windows-Anwendungen mit einem XAML-UI-Modell und dem .NET Framework.

      [System.Speech-Programmierhandbuch für .NET Framework](/previous-versions/office/developer/speech-technologies/hh361625(v=office.14))
   :::column-end:::
   :::column:::
      ![Azure-Sprachdienste](images/platform-azure-speech.png)

      **Azure-Sprachdienste**

      Integrieren Sie die Sprachverarbeitung in apps und Dienste.

      [Spracherkennung](https://azure.microsoft.com/services/cognitive-services/speech-to-text/)

      [Sprachsynthese](https://azure.microsoft.com/services/cognitive-services/text-to-speech/)
      
      [Sprachübersetzung](https://azure.microsoft.com/services/cognitive-services/speech-translation/)

      [Sprechererkennung](https://azure.microsoft.com/en-us/services/cognitive-services/speaker-recognition/)

      [Voice-First Virtual Assistenten](/azure/cognitive-services/speech-service/voice-first-virtual-assistants)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      **Legacy Features**

      Ältere, veraltete und/oder nicht unterstützte Versionen der Sprach-und Konversations Technologie von Microsoft.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Cortana-Funktionskit](/cortana/skills/)

      Als Teil unserer Zielsetzung, die modernen Produktivitäts Erfahrungen zu transformieren, indem Cortana tief in Microsoft 365 eingebettet wird, wird das Cortana Skills Kit für Consumer (die Entwicklerplattform) und alle Fähigkeiten, die auf dieser Plattform aufbauen, zurückgezogen.
   :::column-end:::
   :::column:::

      [Microsoft-Agent](/windows/win32/lwef/microsoft-agent)

      [Microsoft Speech Application Software Development Kit (sasdk) Version 1,0](https://www.microsoft.com/download/details.aspx?id=2200)

      [Microsoft Speech API (SAPI) 5,3](/previous-versions/windows/desktop/ms723627(v=vs.85))

      [Microsoft Speech API (SAPI) 5,4](/previous-versions/windows/desktop/ee125663(v=vs.85))

      [Das Bing-Spracheingabe Erkennungs Steuerelement](/previous-versions/bing/speech/dn434583(v=msdn.10))
   :::column-end:::
:::row-end:::

## <a name="samples"></a>Beispiele

Laden Sie vollständige Windows-Beispiele herunter, die verschiedene Barrierefreiheitsfeatures und -funktionen veranschaulichen, und führen Sie sie aus.

:::row:::
   :::column:::
      [Browser für Codebeispiele](/samples/browse/?term=speech)

      Der neue Beispiel Browser (ersetzt die MSDN Code Gallery).
   :::column-end:::
   :::column:::
      [Klassische Windows-Beispiele auf GitHub](https://github.com/microsoft/Windows-classic-samples/search?q=speech&unscoped_q=speech)

      Diese Beispiele veranschaulichen die Funktionalität und das Programmiermodell für Windows und Windows Server. 
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [UWP-Beispiele (Universelle Windows-Plattform) auf GitHub](https://github.com/microsoft/Windows-universal-samples/search?q=speech&unscoped_q=speech)

      Diese Beispiele veranschaulichen die API-Verwendungsmuster für die Universelle Windows-Plattform (UWP) im Windows Software Development Kit (SDK) für Windows 10.
   :::column-end:::
   :::column:::
      [XAML-Steuerelementekatalog](https://github.com/microsoft/Xaml-Controls-Gallery)

      Diese App zeigt die verschiedenen XAML-Steuerelemente, die im Fluent Design-System unterstützt werden.
   :::column-end:::
:::row-end:::

## <a name="videos"></a>Videos

Verschiedene Videos zum Erstellen von Windows-Anwendungen, die sprach Interaktionen einschließen.

:::row:::
   :::column:::
      **Cortana und Sprachplattform im Detail**
   :::column-end:::
   :::column:::
      **Cortana-Erweiterbarkeit in universellen Windows-apps**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2015/3-716/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2015/2-691/player]
   :::column-end:::
:::row-end:::

## <a name="other-resources"></a>Weitere Ressourcen

:::row:::
   :::column:::
      **Blogs und Neuigkeiten**

      Die neuesten aus der Welt der Microsoft-Sprache.
   :::column-end:::
   :::column:::
      **Community und Support**

      , Wo Windows-Entwickler und-Benutzer zusammenarbeiten und kennenlernen.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Nachrichten](https://news.microsoft.com/?s=speech)

      [Sprachblogs](https://blogs.windows.com/windowsdeveloper/?s=speech)
   :::column-end:::
   :::column:::
      [Windows-Community-Speech](https://community.windows.com/search?q=speech)

      [Entwickler Forum für Windows Speech](https://social.msdn.microsoft.com/Forums/home?filter=alltypes&sort=firstpostdesc&searchTerm=speech)
   :::column-end:::
:::row-end:::