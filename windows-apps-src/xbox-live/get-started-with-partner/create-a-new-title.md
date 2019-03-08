---
title: Erstellen Sie einen neuen Titel
description: Erfahren Sie, wie Sie mithilfe von Partner Center einen neuen Titel für Xbox Live zu erstellen.
ms.assetid: b8bd69e6-887a-4b1f-a42d-8affdbec0234
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 1aa2447a2044bec9b2013b30c05e45342b763fc3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656565"
---
# <a name="create-a-new-title-for-xbox-live"></a>Erstellen Sie einen neuen Titel für die Xbox Live

## <a name="introduction"></a>Einführung

Bevor Sie Code schreiben, müssen Sie einen neuen Titel in Ihrem Konfiguration einrichten.  Weitere Informationen finden Sie Informationen zur Dienstkonfiguration in [Xbox Live-Dienstkonfiguration](../xbox-live-service-configuration.md)

Dieser Artikel führt Sie durch diesen Vorgang mit den folgenden Annahmen

1. Entwickeln Sie einen Titel für die universelle Windows-Plattform (UWP).  Führen Sie die UWP-Titel auf Xbox One, Windows 10-desktop-PCs und mobile
2. Konfigurieren Sie Ihre Titel in [Partner Center](https://partner.microsoft.com/dashboard).
3. Verwenden Sie entweder Visual Studio mit Unity oder eine benutzerdefinierte Spiele-Engine.
4. Entwicklungscomputer wird Windows 10 ausgeführt werden.

Vorausgesetzt, dass die oben auf "true" festgelegt sind, werden im weiteren Verlauf dieses Artikels erhalten einen Titel, der in Partner Center, eine neu erstellte Projekt, und Xbox Live-Anmeldung Code geschrieben und getestet wird konfiguriert, um durchlaufen.

> [!NOTE]
> Wenn Sie Teil der Xbox Live Creators-Programm sind, wird die oben aufgeführten Annahmen gelten für Sie und sollten Sie zusammen mit den in diesem Artikel befolgen.

## <a name="partner-center-setup"></a>Setup für den Partner Center

Sie benötigen einen Titel ein Xbox Live aktiviert in erstellt [Partner Center](https://partner.microsoft.com/dashboard) als Voraussetzung für das Xbox Live-Funktionalität arbeiten.

### <a name="create-a-microsoft-account"></a>Microsoft-Konto erstellen
Wenn Sie nicht über ein Microsoft-Account (auch bekannt als ein MSA) verfügen, müssen Sie zunächst nacheinander erstellen [ https://go.microsoft.com/fwlink/p/?LinkID=254486 ](https://go.microsoft.com/fwlink/p/?LinkID=254486).  Wenn Sie über ein Office 365-Konto verfügen, Outlook.com, oder haben Sie ein Konto auf der Xbox Live - haben Sie wahrscheinlich bereits eine MSA.

### <a name="register-as-an-app-developer"></a>Registrieren Sie sich als ein App-Entwickler
Sie müssen als ein App-Entwickler zu registrieren, bevor Sie einen neuen Titel im Partner Center erstellen dürfen.

Wechseln Sie zu registrieren, https://developer.microsoft.com/en-us/store/register und folgen Sie den Registrierungsvorgang ab.

### <a name="create-a-new-uwp-title"></a>Erstellen Sie einen neuen UWP-Titel
Als Nächstes benötigen Sie einen UWP-Titel im Partner Center definiert.  Sie dies durch die erste laufende an dem Dashboard

![](../images/getting_started/first_xbltitle_dashboard.png)

<p>
</p>
<br>
<p>
</p>

Anschließend erstellen Sie einen neuen Titel ein.  Sie müssen einen Namen zu reservieren.

![](../images/getting_started/first_xbltitle_newapp.png)

Klicken Sie dann gelangen Sie zu der *Übersicht über die App* für Ihre app.  Der primäre Seite, in dem Sie konfigurieren Xbox Live demnach, befindet sich in der Dienste -> Xbox Live-Menü unten.

![](../images/getting_started/first_xbltitle_leftnav.png)

<div id="createxblaccount"></div>

## <a name="create-an-xbox-live-account"></a>Erstellen Sie ein Xbox Live-Konto
Sie benötigen ein Xbox Live-Konto bei Xbox Live anmelden.  Wenn Sie bereits eine, die Sie zur verfügen Anmeldung verwenden, auf Ihre Xbox One-Konsole oder in der Xbox-App für Windows 10, können Sie, dass eine verwenden.

Andernfalls sollten Sie die Xbox-App auf Ihrem PC und melden Sie sich mit Ihrem Microsoft Account öffnen.  Es wird dann für die Verwendung mit Xbox Live aktiviert.

Sie finden die Xbox-App durch das aufrufen, die in der *Menü "Start"* und in "Xbox" eingeben, wie unten dargestellt.

![](../images/getting_started/first_xbltitle_xboxapp.png)

## <a name="next-steps"></a>Nächste Schritte
Nun, da Sie einen neuen Titel erstellt haben, können Sie jetzt einen Xbox Live aktiviert Titel in Ihre Spiele-Engine, die Visual Studio oder die Build-Umgebung Ihrer Wahl einrichten.

Finden Sie unter [Schritt für Schritt-Anleitung zum Integrieren von Xbox Live](partners-step-by-step-guide.md)
