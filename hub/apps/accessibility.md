---
title: Barrierefreiheit in Windows 10
description: Diese Seite enthält Informationen, die Ihnen den Einstieg in die Entwicklung zugänglicher Windows-apps erleichtern.
ms.topic: article
ms.date: 09/12/2019
keywords: Barrierefreiheit in Windows 10, Barrierefreiheit, Barrierefreiheit, barrierefreie Win32-apps, Aufbau barer UWP-apps, Aufbau zugänglicher WPF-apps, Aufbau von zugänglichen WinForms-apps
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.openlocfilehash: 76bbd3f0e04bbb2f729ad0950bae190b2fffb6ac
ms.sourcegitcommit: 6e7665b457ec4585db19b70acfa2554791ad6e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2019
ms.locfileid: "70987195"
---
# <a name="accessibility-in-windows-10"></a>Barrierefreiheit in Windows 10

![Hero-Accessibility-Bar-Smaller. png](images/hero-accessibility-bar-smaller.png)

## <a name="build-accessibility-into-your-applications-to-empower-people-of-all-abilities"></a>Erstellen Sie Barrierefreiheit in Ihre Anwendungen, um die Benutzer aller Fähigkeiten zu unterstützen

Produkte und Dienste – einschließlich elektronischer Medien – können aufgerufen werden, wenn Sie so viele Personen wie möglich vollständige und erfolgreiche Umgebungen bereitstellen.

Erstellen Sie barrierefreie und inklusivwindows-Anwendungen mit verbesserter Funktionalität und Benutzerfreundlichkeit für Personen mit Behinderungen (sowohl temporär als auch permanent), persönliche Einstellungen, bestimmte Arbeitsstile oder situations Einschränkungen (z. b. freigegebene Arbeitsbereiche, "Driving", "Cooking", "glare" usw.). Zu den gängigen Lösungen gehören das Bereitstellen von Informationen in alternativen Formaten (z. b. Beschriftungen in einem Video) oder das Aktivieren der Verwendung von Hilfstechnologien (z. b. Bildschirm Sprachausgaben).

**Jeder sollte auf die gleichen Räume in einem Gebäude zugreifen können, egal, ob er die Treppe oder den Lift verwenden muss.**

Diese Seite enthält Informationen dazu, wie die verschiedenen Windows-Entwicklungs Frameworks Barrierefreiheits Unterstützung für Entwickler bereitstellen, die Windows-Anwendungen erstellen, hilfstechnologieentwickler, die Tools wie Sprachausgabe und Bildschirmlupe sowie Software erstellen. Test Entwickler, die automatisierte Skripts zum Testen von Anwendungen erstellen.

## <a name="platform-specific-documentation"></a>Plattformspezifische Dokumentation

:::row:::
   :::column:::
      ![Universelle Windows-Plattform (UWP)](images/platform-uwp.png)

      **Universelle Windows-Plattform (UWP)**

      Entwickeln Sie barrierefreie apps und Tools auf der modernen Plattform für Windows 10-Anwendungen und-Spiele auf allen Windows-Geräten (einschließlich PCs, Smartphones, Xbox One, hololens usw.), und veröffentlichen Sie Sie auf dem Microsoft Store.

      [Entwerfen von barrierefreier Software](https://docs.microsoft.com/windows/uwp/accessibility/designing-inclusive-software)

      [Entwickeln von barrierefreien Windows-Apps](https://docs.microsoft.com/windows/uwp/accessibility/developing-inclusive-windows-apps)

      [Testen der Barrierefreiheit](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-testing)

      [Barrierefreiheit im Store](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-in-the-store)
   :::column-end:::
   :::column:::
      ![Win32-Plattform-apps](images/platform-win32.png)

      **Win32-Plattform**

      Entwickeln Sie barrierefreie apps und Tools auf der ursprünglichen Plattform für CC++ /Windows-Anwendungen.

      [Neues bei Windows-Barrierefreiheit und-Automatisierung](https://docs.microsoft.com/windows/desktop/accessibility-whatsnew)

      [Entwickeln von zugänglichen Anwendungen für Windows](https://docs.microsoft.com/windows/desktop/accessibility-appdev)

      [Entwickeln barrierefreier Benutzeroberflächen-Frameworks für Windows](https://docs.microsoft.com/windows/desktop/accessibility-uiframeworkdev)

      [Entwickeln von Hilfstechnologien für Windows](https://docs.microsoft.com/windows/desktop/accessibility-atdev)

      [Testen auf Barrierefreiheit](https://docs.microsoft.com/windows/desktop/accessibility-testwithuia)

      [Legacy-Barrierefreiheit und Automatisierungs Technologie-MSAA für die Benutzeroberflächen Automatisierung](https://docs.microsoft.com/windows/desktop/accessibility-legacy)

      [Windows-Barrierefreiheits Funktionen](https://docs.microsoft.com/windows/desktop/winauto/about-windows-accessibility-features)

      [Richtlinien zum Entwerfen barrierefreier Apps](https://docs.microsoft.com/windows/desktop/uxguide/inter-accessibility)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      ![WPF-Plattform](images/platform-wpf2-small.png)

      **Windows Presentation Foundation (WPF)**

      Entwickeln Sie barrierefreie apps und Tools auf der eingerichteten Plattform für verwaltete Windows-Anwendungen mit einem XAML-Benutzeroberflächen Modell und den .NET Framework.

      [Bewährte Methoden für die Barrierefreiheit](https://docs.microsoft.com/dotnet/framework/ui-automation/accessibility-best-practices)

      [Grundlagen der Benutzeroberflächenautomatisierung](https://docs.microsoft.com/dotnet/framework/ui-automation/index)

      [Benutzeroberflächenautomatisierungs-Anbieter für verwalteten Code](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-providers-for-managed-code)

      [Benutzeroberflächenautomatisierungs-Clients für verwalteten Code](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-clients-for-managed-code)

      [Steuerelement Muster für Benutzeroberflächen Automatisierung](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-control-patterns)

      [Benutzeroberflächenautomatisierungs-Text Muster](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-text-pattern)

      [Steuerelement Typen der Benutzeroberflächen Automatisierung](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-control-types)

      [Benutzeroberflächenautomatisierungs-Spezifikation und Community Promise](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-specification-and-community-promise)
   :::column-end:::
   :::column:::
      ![Apps für Windows Forms Plattform](images/platform-winforms.png)

      **Windows Forms (WinForms)**

      Entwickeln Sie barrierefreie apps und Tools für verwaltete Windows-Anwendungen mit einem XAML-Benutzeroberflächen Modell und dem .NET Framework.

      [Windows Forms Barrierefreiheit](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)

      [Erstellen einer zugänglichen Windows-Anwendung](https://docs.microsoft.com/dotnet/framework/winforms/advanced/walkthrough-creating-an-accessible-windows-based-application)

      [Eigenschaften für Windows Forms Steuerelemente, die Barrierefreiheits Richtlinien unterstützen](https://docs.microsoft.com/dotnet/framework/winforms/advanced/properties-on-windows-forms-controls-that-support-accessibility-guidelines)

      [Bereitstellen von Barrierefreiheits Informationen für Steuerelemente in einem Windows Form](https://docs.microsoft.com/dotnet/framework/winforms/controls/providing-accessibility-information-for-controls-on-a-windows-form)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      **Web-Barrierefreiheit**

      Entwerfen, erstellen und testen Sie barrierefreie Websites in Microsoft Edge.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Einführung in Web-Barrierefreiheit](https://docs.microsoft.com/microsoft-edge/accessibility)

      [Entwerfen von barrierefreien Websites](https://docs.microsoft.com/microsoft-edge/accessibility/design)
   :::column-end:::
   :::column:::
      [Erstellen von barrierefreien Websites](https://docs.microsoft.com/microsoft-edge/accessibility/build)

      [Testen von zugänglichen Websites](https://docs.microsoft.com/microsoft-edge/accessibility/test)
   :::column-end:::
:::row-end:::

## <a name="samples"></a>Proben

Laden Sie vollständige Windows-Beispiele herunter, die verschiedene Barrierefreiheits Features und-Funktionen veranschaulichen.

:::row:::
   :::column:::
      [Code Beispiel Browser](https://docs.microsoft.com/en-us/samples/browse/)

      Der neue Beispiele-Browser ersetzt die MSDN Code Gallery.
   :::column-end:::
   :::column:::
      [MSDN Code Gallery (veraltet)](https://code.msdn.microsoft.com/site/search?query=accessibility&f%5B0%5D.Value=accessibility&f%5B0%5D.Type=SearchText&ac=2)

      Laden Sie Beispiele für Windows, Windows Phone, Microsoft Azure, Office, SharePoint, Silverlight und andere Produkte herunter.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Klassische Windows-Beispiele auf GitHub](https://github.com/microsoft/Windows-classic-samples/search?q=accessibility&unscoped_q=accessibility)

      Diese Beispiele veranschaulichen die Funktionalität und das Programmiermodell für Windows und Windows Server. 
   :::column-end:::
   :::column:::
      [Universelle Windows-Plattform (UWP)-Beispiele auf GitHub](https://github.com/microsoft/Windows-universal-samples/search?q=accessibility&unscoped_q=accessibility)

      Diese Beispiele veranschaulichen die API-Verwendungs Muster für den universelle Windows-Plattform (UWP) im Windows Software Development Kit (SDK) für Windows 10.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [XAML-Steuerelementekatalog](https://github.com/microsoft/Xaml-Controls-Gallery)

      Diese APP zeigt die verschiedenen XAML-Steuerelemente, die im System eigenen Design System unterstützt werden.
   :::column-end:::
:::row-end:::

## <a name="videos"></a>Videos

Verschiedene Videos, in denen erläutert wird, wie Sie barrierefreie Windows-Anwendungen für allgemeine Barrierefreiheits Probleme erstellen und wie Microsoft diese adressiert.

:::row:::
   :::column:::
      **Einstieg in die Barrierefreiheit in Windows-apps**
   :::column-end:::
   :::column:::
      **Eine dev-Minute: Entwickeln von Apps für Barrierefreiheit**
   :::column-end:::
   :::column:::
      **Einführung in die Barrierefreiheit und Barrierefreiheit**
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
      **Von Hack zu Produkt, Eye Control für Windows 10**
   :::column-end:::
   :::column:::
      **Barrierefreiheit unter Windows 10**
   :::column-end:::
   :::column:::
      **Einführung in das Entwickeln von zugänglichen UWP-apps**
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
      **Entwerfen von Windows-Barrierefreiheits Features**
   :::column-end:::
   :::column:::
      **Windows 10-Barrierefreiheits Features ermöglichen allen Benutzern**
   :::column-end:::
   :::column:::
      **Vereinfachen der Mauszeiger**
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

      Die neuesten aus der Welt der Microsoft-Barrierefreiheit.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [In den Nachrichten](https://news.microsoft.com/presskits/accessibility/)
   :::column-end:::
   :::column:::
      [Barrierefreiheits Blogs](https://blogs.microsoft.com/accessibility/)
   :::column-end:::
   :::column:::
      [Blogs zur Windows-Benutzeroberflächen Automatisierung](https://blogs.msdn.microsoft.com/winuiautomation/)
   :::column-end:::
:::row-end:::

:::row:::
   :::column span="3":::
      **Community und Support**

      Ein Ort, an dem Windows-Entwickler und-Benutzer zusammenarbeiten und kennenlernen.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Windows-Community: Barrierefreiheit](https://community.windows.com/search?q=accessibility)
   :::column-end:::
   :::column:::
      [Entwicklungs Forum für Windows-Barrierefreiheit und-Automatisierung](https://social.msdn.microsoft.com/Forums/windows/home?forum=windowsaccessibilityandautomation)
   :::column-end:::
   :::column:::
      [Fähigkeits-Antwort Desk](https://www.microsoft.com/Accessibility/disability-answer-desk)
   :::column-end:::
:::row-end:::
