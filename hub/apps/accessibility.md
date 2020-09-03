---
title: Barrierefreiheit unter Windows 10
description: Diese Seite enthält die Informationen, die Sie zum Entwickeln von barrierefreien Windows-Apps benötigen.
ms.topic: article
ms.date: 09/12/2019
keywords: Barrierefreiheit in Windows 10, Barrierefreiheit, Erstellen barrierefreier Win32-Apps, Erstellen barrierefreier UWP-Apps, Erstellen barrierefreier WPF-Apps, Erstellen barrierefreier WinForms-Apps
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.openlocfilehash: 7cdfa91bec648ab6f3d09d0aeff0d62e2962db86
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169044"
---
# <a name="accessibility-in-windows-10"></a>Barrierefreiheit unter Windows 10

![Heroabbildung Barrierefreiheit](images/hero-accessibility-bar-smaller.png)

## <a name="build-accessibility-into-your-applications-to-empower-people-of-all-abilities"></a>Integrieren Sie Barrierefreiheit in Ihre Anwendungen, um Menschen mit unterschiedlichsten Fähigkeiten zu unterstützen

Produkte und Dienste – einschließlich elektronischer Medien – sind barrierefrei, wenn sie von so vielen Personen wie möglich vollständig und erfolgreich genutzt werden können.

Erstellen Sie barrierefreie und inklusive Windows-Anwendungen mit verbesserter Funktionalität und Benutzerfreundlichkeit für Personen mit Behinderungen (sowohl temporär als auch permanent), unterschiedlichen persönlichen Einstellungen, bestimmten Arbeitsstilen oder situationsbedingten Einschränkungen (z. B. in gemeinsam genutzten Arbeitsbereichen, beim Autofahren, Kochen, Geblendetwerden usw.). Zu den gängigen Lösungen gehören das Bereitstellen von Informationen in alternativen Formaten (z. B. Beschriftungen in einem Video) oder das Aktivieren der Verwendung von Hilfstechnologien (z. B. Bildschirmsprachausgaben).

**Alle sollten gleichermaßen über Treppe oder Lift Zugang zu den gleichen Räumen in einem Gebäude haben.**

Diese Seite informiert darüber, wie die verschiedenen Windows-Entwicklungsframeworks die Barrierefreiheit für Entwickler von Windows-Anwendungen unterstützen, für Hilfstechnologieentwickler, die Tools wie Sprachausgaben und Bildschirmlupen erstellen, sowie Softwaretestentwickler, die automatisierte Skripts zum Testen von Anwendungen erstellen.

## <a name="platform-specific-documentation"></a>Plattformspezifische Dokumentation

:::row:::
   :::column:::
      ![Universelle Windows-Plattform (UWP)](images/platform-uwp.png)

      **Universelle Windows-Plattform (UWP)**

      Entwickeln Sie barrierefreie Apps und Tools auf der modernen Plattform für Windows 10-Anwendungen und -Spiele auf allen Windows-Geräten (einschließlich PCs, Smartphones, Xbox One, HoloLens usw.), und veröffentlichen Sie sie im Microsoft Store.

      [Entwerfen von barrierefreier Software](/windows/uwp/accessibility/designing-inclusive-software)

      [Entwickeln von barrierefreien Windows-Apps](/windows/uwp/accessibility/developing-inclusive-windows-apps)

      [Testen der Barrierefreiheit](/windows/uwp/accessibility/accessibility-testing)

      [Barrierefreiheit im Store](/windows/uwp/accessibility/accessibility-in-the-store)
   :::column-end:::
   :::column:::
      ![Win32-Plattform-Apps](images/platform-win32.png)

      **Win32-Plattform**

      Entwickeln Sie barrierefreie Apps und Tools auf der ursprünglichen Plattform für C/CC++-Windows-Anwendungen.

      [Neues zu Windows-Barrierefreiheit und -Automatisierung](/windows/desktop/accessibility-whatsnew)

      [Entwickeln von barrierefreien Anwendungen für Windows](/windows/desktop/accessibility-appdev)

      [Entwickeln von barrierefreien Benutzeroberflächenframeworks für Windows](/windows/desktop/accessibility-uiframeworkdev)

      [Entwickeln von Hilfstechnologie für Windows](/windows/desktop/accessibility-atdev)

      [Testen für Barrierefreiheit](/windows/desktop/accessibility-testwithuia)

      [Legacy-Barrierefreiheit und -Automatisierungstechnologie – von MSAA bis Benutzeroberflächenautomatisierung](/windows/desktop/accessibility-legacy)

      [Windows-Eingabehilfefunktionen](/windows/desktop/winauto/about-windows-accessibility-features)

      [Richtlinien zum Entwerfen barrierefreier Apps](/windows/desktop/uxguide/inter-accessibility)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      ![WPF-Plattform](images/platform-wpf2-small.png)

      **Windows Presentation Foundation (WPF)**

      Entwickeln Sie barrierefreie Apps und Tools auf der bekannten Plattform für verwaltete Windows-Anwendungen mit einem XAML-UI-Modell und dem .NET Framework.

      [Bewährte Methoden für Eingabehilfen](/dotnet/framework/ui-automation/accessibility-best-practices)

      [Grundlagen der Benutzeroberflächenautomatisierung](/dotnet/framework/ui-automation/index)

      [Benutzeroberflächenautomatisierungs-Anbieter für verwalteten Code](/dotnet/framework/ui-automation/ui-automation-providers-for-managed-code)

      [Benutzeroberflächenautomatisierungs-Clients für verwalteten Code](/dotnet/framework/ui-automation/ui-automation-clients-for-managed-code)

      [Steuerelementmuster für Benutzeroberflächenautomatisierung](/dotnet/framework/ui-automation/ui-automation-control-patterns)

      [Textmuster zur Benutzeroberflächenautomatisierung](/dotnet/framework/ui-automation/ui-automation-text-pattern)

      [Steuerelementtypen der Benutzeroberflächenautomatisierung](/dotnet/framework/ui-automation/ui-automation-control-types)

      [Spezifikation für die Benutzeroberflächenautomatisierung und Zusicherung an die Community](/dotnet/framework/ui-automation/ui-automation-specification-and-community-promise)
   :::column-end:::
   :::column:::
      ![Apps für Windows Forms-Plattform](images/platform-winforms.png)

      **Windows Forms (WinForms)**

      Entwickeln Sie barrierefreie Apps und Tools für verwaltete Windows-Anwendungen mit einem XAML-UI-Modell und dem .NET Framework.

      [Windows Forms-Barrierefreiheit](/dotnet/framework/winforms/advanced/windows-forms-accessibility)

      [Erstellen von behindertengerechten Windows-Anwendungen](/dotnet/framework/winforms/advanced/walkthrough-creating-an-accessible-windows-based-application)

      [Eigenschaften für Windows Forms-Steuerelemente, die Richtlinien für Eingabehilfen unterstützen](/dotnet/framework/winforms/advanced/properties-on-windows-forms-controls-that-support-accessibility-guidelines)

      [Informationen über Eingabehilfen für Steuerelemente in Windows Forms](/dotnet/framework/winforms/controls/providing-accessibility-information-for-controls-on-a-windows-form)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      **Webbarrierefreiheit**

      Entwickeln, erstellen und testen Sie barrierefreie Websites in Microsoft Edge.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Einführung in die Barrierefreiheit im Internet](/microsoft-edge/accessibility)

      [Entwerfen von barrierefreien Websites](/microsoft-edge/accessibility/design)
   :::column-end:::
   :::column:::
      [Erstellen von barrierefreien Websites](/microsoft-edge/accessibility/build)

      [Testen von barrierefreien Websites](/microsoft-edge/accessibility/test)
   :::column-end:::
:::row-end:::

## <a name="samples"></a>Beispiele

Laden Sie vollständige Windows-Beispiele herunter, die verschiedene Barrierefreiheitsfeatures und -funktionen veranschaulichen, und führen Sie sie aus.

:::row:::
   :::column:::
      [Browser für Codebeispiele](/samples/browse/)

      Der neue Beispielebrowser ersetzt den MSDN-Codekatalog.
   :::column-end:::
   :::column:::
      [MSDN Code Gallery (GitHub-Archiv)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample)

      Laden Sie Beispiele für Windows, Windows Phone, Microsoft Azure, Office, SharePoint, Silverlight und andere Produkte herunter.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Klassische Windows-Beispiele auf GitHub](https://github.com/microsoft/Windows-classic-samples/search?q=accessibility&unscoped_q=accessibility)

      Diese Beispiele veranschaulichen die Funktionalität und das Programmiermodell für Windows und Windows Server. 
   :::column-end:::
   :::column:::
      [UWP-Beispiele (Universelle Windows-Plattform) auf GitHub](https://github.com/microsoft/Windows-universal-samples/search?q=accessibility&unscoped_q=accessibility)

      Diese Beispiele veranschaulichen die API-Verwendungsmuster für die Universelle Windows-Plattform (UWP) im Windows Software Development Kit (SDK) für Windows 10.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [XAML-Steuerelementekatalog](https://github.com/microsoft/Xaml-Controls-Gallery)

      Diese App zeigt die verschiedenen XAML-Steuerelemente, die im Fluent Design-System unterstützt werden.
   :::column-end:::
:::row-end:::

## <a name="videos"></a>Videos

Verschiedene Videos, in denen erläutert wird, wie Sie barrierefreie Windows-Anwendungen für allgemeine Barrierefreiheitsprobleme erstellen, und wie Microsoft diese behandelt.

:::row:::
   :::column:::
      **Vorgehensweise: Einstieg in die Barrierefreiheit in Windows-Apps**
   :::column-end:::
   :::column:::
      **Eine Entwickler-Minute: Entwickeln von Apps für Barrierefreiheit**
   :::column-end:::
   :::column:::
      **Einführung zu Behinderung und Barrierefreiheit**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2017/P4072/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/Kl4CT4DaypM]
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      **Vom Hackathon zum Produkt – Augensteuerung für Windows 10**
   :::column-end:::
   :::column:::
      **Barrierefreiheit unter Windows 10**
   :::column-end:::
   :::column:::
      **Einführung in die Erstellung barrierefreier UWP-Apps**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/AShNPfmAkvY]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2016/P541/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2016/P497/player]
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      **Entwerfen von Windows-Eingabehilfefunktionen**
   :::column-end:::
   :::column:::
      **Windows 10-Barrierefreiheitsfeatures befähigen alle**
   :::column-end:::
   :::column:::
      **Verbessern der Mauszeigersichtbarkeit**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/Y_NJbE7wxlk]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/BseTf-4q9GA]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/4UzaF7_T3bw]
   :::column-end:::
:::row-end:::

## <a name="other-resources"></a>Weitere Ressourcen

:::row:::
   :::column span="3":::
      **Blogs und Neuigkeiten**

      Das Neueste aus der Welt der Microsoft-Barrierefreiheit.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Nachrichten](https://news.microsoft.com/presskits/accessibility/)
   :::column-end:::
   :::column:::
      [Barrierefreiheitsblogs](https://blogs.microsoft.com/accessibility/).
   :::column-end:::
   :::column:::
      [Blogs zur Automatisierung der Windows-Benutzeroberfläche](/archive/blogs/winuiautomation/)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="3":::
      **Community und Support**

      Ein Ort, an dem Windows-Entwickler und -Benutzer zusammen arbeiten und lernen können.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Windows-Community: Barrierefreiheit](https://community.windows.com/search?q=accessibility)
   :::column-end:::
   :::column:::
      [Entwicklungsforum zu Windows-Barrierefreiheit und -Automatisierung](https://social.msdn.microsoft.com/Forums/windows/home?forum=windowsaccessibilityandautomation)
   :::column-end:::
   :::column:::
      [Answer Desk zu Behinderungen](https://www.microsoft.com/Accessibility/disability-answer-desk)
   :::column-end:::
:::row-end:::