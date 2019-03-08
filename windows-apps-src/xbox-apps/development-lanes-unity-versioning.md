---
title: Unity – Versionskontrolle für Ihr UWP-Projekt
description: Versionsverwaltung für Ihr Unity-UWP.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 064eaf42fe7d664be273cd7e2222fa5d90be1a11
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608865"
---
# <a name="unity-version-control-your-uwp-project"></a>Unity: Versionskontrolle für Ihr UWP-Projekt

Sie haben Ihr Unity-Spiel für Xbox noch immer nicht in die universelle Windows-Plattform (UWP) portiert?  Lesen Sie zunächst [Portieren von Unity-Spielen für Xbox auf die UWP](development-lanes-unity.md).

Es gibt verschiedene Gründe dafür, Teile Ihres generierten UWP-Verzeichnisses der Versionskontrolle hinzuzufügen. Hierzu zählt unter anderem das Hinzufügen von Abhängigkeiten (z. B. Xbox Live SDK).  Dieses Szenario wird in diesem Lernprogramm als Beispiel herangezogen und hilft Ihnen hoffentlich dabei, die individuellen Anforderungen Ihres Projekts zu erfüllen.

***Haftungsausschluss: Wir verwenden Git als unsere Version Control-Lösung.  Wenn sich Ihre unterscheidet, sollte die Konzepte weiterhin übersetzt werden.***

Zur Erinnerung: Das Verzeichnis für unser Spiel ***ScrapyardPhoenix*** sieht aktuell wie folgt aus:

![Build-Zielordner](images/build-destination.png)

Und unser UWP-Verzeichnis sieht wie folgt aus:

![UWP-VS-Lösung](images/uwp-vs-solution.png)

In diesem Verzeichnis interessiert uns nur der Ordner ***ScrapyardPhoenix*** (Name Ihres Spiels).  Alles andere ist für unsere Versionskontrolle nicht relevant.

***Mit eine gitignore-Datei wird ist nicht vertraut?  Finden Sie unter [Gitignore](https://git-scm.com/docs/gitignore).***

    ##################################################################
    # The original .gitignore file can be found at
    # https://github.com/github/gitignore/blob/master/Unity.gitignore
    ##################################################################

    # standard ignores for a Unity Project
    ...

    # ignore the whole UWP directory
    /UWP/**

    # except we want to keep... (this line will be modified and removed further down)
    !/UWP/ScrapyardPhoenix/

Wir möchten einige unterschiedliche Dateien und Ordner aus dem Ordner **UWP/ScrapyardPhoenix** auswählen und unserer Versionskontrolle hinzufügen.  Sehen wir uns das Ganze erst einmal im Detail an:

![UWP-Buildverzeichnis](images/uwp-build-directory.png)  

## <a name="folders"></a>Ordner  

`Assets` | ***Umfassen*** | Enthält Microsoft Store-images  
`Data`   | ***Ignorieren Sie*** | In denen kompiliert Unity für Ihr Projekts (im Hintergrund, Shadern, Skripts, Prefabs usw.).  
`Dependencies` | ***Umfassen*** | Dieser Ordner ist, die ich erstellt habe alle UWP-Abhängigkeiten (z. B. XboxLiveSDK.dll) beibehalten möchten  
`Properties` | ***Umfassen*** | Enthält erweiterte Einstellungen, die vom Entwickler geändert werden können  
`Unprocessed` | ***Ignorieren Sie*** | Enthält Unity `.dll` und `.pdb` Dateien  

## <a name="files"></a>Dateien  

`App.cs` | ***Umfassen*** | Der Einstiegspunkt für Ihre UWP-Anwendung. Dies kann geändert und mit anderen Quelldateien erweitert  
`Package.appxmanifest` | ***Umfassen*** | App-Paket-manifest-Source-Datei für Ihre AppX  
`project.json` | ***Umfassen*** | Beschreibt die NuGet-Pakete Ihrem `*.csproj` hängt  
`ScrapyardPhoenix.csproj` | ***Umfassen*** | Beschreibt Ihre UWP-Build-Ziel an. Wenn Sie zusätzliche Abhängigkeiten hinzufügen, die sich in Ihre UWP-Projekt dieses `*.csproj` -Datei enthält diese Informationen  
`ScrapyardPhoenix.csproj.user` | ***Ignorieren Sie*** | Diese Datei enthält lokale Benutzerinformationen

## <a name="resulting-gitignore"></a>Resultierende GITIGNORE-Datei

    ##################################################################
    # The original .gitignore file can be found at
    # https://github.com/github/gitignore/blob/master/Unity.gitignore
    ##################################################################

    # standard ignores for a Unity Project
    ...

    # ignore the whole UWP directory
    /UWP/**

    # except we want to keep...
    !/UWP/ScrapyardPhoenix/Assets/*
    !/UWP/ScrapyardPhoenix/Dependencies/*
    !/UWP/ScrapyardPhoenix/Properties/*
    !/UWP/ScrapyardPhoenix/App.cs
    !/UWP/ScrapyardPhoenix/Package.appxmanifest
    !/UWP/ScrapyardPhoenix/project.json
    !/UWP/ScrapyardPhoenix/ScrapyardPhoenix.csproj

Geschafft: Ihre Teammitglieder sind nun mit dem von Ihnen generierten UWP-Projekt synchronisiert. Nun können Sie Ihrem UWP-Projekt problemlos zusätzliche Ressourcen, Quellen und Abhängigkeiten hinzufügen.

Einige weitere Versionsverwaltungsbeispiele für den UWP-Ordner finden Sie [hier](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview).

## <a name="adding-dependencies-to-your-uwp-app"></a>Hinzufügen von Abhängigkeiten zu Ihrer UWP-App

Fügen Sie Abhängigkeiten zu DLL- und WINMD-Dateien hinzu, indem Sie diese im Unterordner **Unity Assets** des Ordners **Plug-Ins** ablegen und auswählen. Legen Sie anschließend im Paketprüfungstool die Plattform-Zieleinstellungen fest.

![UWP-Lösung](images/uwp-solution.PNG)

***ScrapyardPhoenix (Universal Windows)*** ist das Projekt, dem Sie einen Verweis (etwa auf das Xbox Live SDK) hinzufügen würden.

## <a name="see-also"></a>Siehe auch
- [Vorhandene Spiele anbieten für Xbox](development-lanes-landing.md)
- [UWP auf Xbox One](index.md)
