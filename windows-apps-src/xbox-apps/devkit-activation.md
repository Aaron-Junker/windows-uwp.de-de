---
title: Aktivierung des Xbox One-Entwicklermodus
description: Dieser Artikel beschreibt das Aktivieren des Entwicklermodus, sodass Sie zwischen Einzelhandelsmodus und Entwicklermodus wechseln können.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: ade80769-17ae-46e9-9c2f-bf08ae5a51ee
ms.localizationpriority: medium
ms.openlocfilehash: d93fef1b06fa52616b7e5eddf7b3ac0530856dc2
ms.sourcegitcommit: a15bc17aa0640722d761d0d33f878cb2a822e8ed
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2020
ms.locfileid: "96577102"
---
# <a name="xbox-one-developer-mode-activation"></a>Aktivierung des Xbox One-Entwicklermodus

## <a name="how-developer-mode-works"></a>Funktionsweise des Entwicklermodus
Dieser Artikel gilt nur für Xbox One-und Xbox-Serie X | S-Konsolen, die über Einzelhandelskanäle erworben wurden. Informationen zu Development Kit-HW, die über ein verwaltetes Entwicklungsprogramm erworben wurden, finden Sie im Hinweis am Ende des Artikels.

Xbox Retail-Konsolen können über zwei Modi, den Einzelhandels Modus (1) und den Entwicklermodus (2) verfügen. Im Einzelhandels Modus befindet sich die Konsole in Ihrem normalen Zustand: Sie können Spiele spielen und apps ausführen, die über den Xbox Store erworben wurden. Im Entwicklermodus können Sie Software für die-Konsole entwickeln und testen, Sie können jedoch keine Einzelhandels Spiele spielen oder Verkaufs-apps ausführen.

Der Entwicklermodus kann in jeder Einzelhandel-Xbox-Konsole über die app "Retail-zu-dev-Kit-Konvertierung" im Xbox Store aktiviert werden. Nachdem der Entwicklermodus auf der Einzelhandels Konsole aktiviert wurde, können Sie zwischen Retail (2a) und Developer Modes (2B) hin-und herwechseln.

> [!NOTE]
> Führen Sie diese Konvertierungs-APP nicht auf einer Xbox-Entwicklungs Hardware aus, die über ein von Xbox verwaltetes Programm (z. b.) erworben wurde, ID@Xbox oder führen Sie bei der Entwicklung Ihres Spiels Fehler und Verzögerungen ein Wenn Sie ein verwalteter Partner sind, können Sie weitere Informationen zum Aktivieren der Entwicklungs Hardware erhalten. Gehe zu https://developer.microsoft.com/en-us/games/xbox/docs/gdk/provisioning-role.

<br></br>

![Xbox One-Modi](images/dev-mode-flow.png)

## <a name="activate-developer-mode-on-your-retail-xbox-console"></a>Aktivieren des Entwicklermodus auf der Xbox-Konsole des Einzelhandels

1.  Starten Sie Ihre Xbox Console.

2.  Suchen und installieren Sie die Entwickler **Modus-Aktivierungs** -App aus dem Xbox One-Speicher.

    ![Installieren der DevMode-Aktivierungs-App](images/devkit-activation-1.png)

3.  Starten Sie die APP auf der Store-Seite.

    ![DevMode-Aktivierungs-App](images/devkit-activation-2.png)

4.  Notieren Sie sich den in der DevMode-Aktivierungs-App angezeigten Code.

    ![Aktivierungsschritt 5](images/activation-step-5.png)  
    
5.  [Registrieren Sie ein App-Entwicklerkonto im Partner Center](https://developer.microsoft.com/store/register).  Dies ist auch der erste Schritt zum Veröffentlichen Ihres Spiels.

6.  Melden Sie sich mit Ihrem gültigen, aktuellen Partner Center-App-Entwicklerkonto bei [Partner Center](https://partner.microsoft.com/dashboard) an.  Wenn im linken Navigationsbereich nicht mehrere Optionen angezeigt werden oder die Option **neue APP erstellen** im **Übersichts** Abschnitt nicht angezeigt wird, funktionieren die folgenden Schritte und Aktivierungs Links _nicht_. Stellen Sie sicher, dass Sie Ihr App-Entwicklerkonto im vorherigen Schritt vollständig registriert haben.

7.  Wechseln Sie zu [Partner.Microsoft.com/xboxconfig/Devices](https://partner.microsoft.com/xboxconfig/devices).

8.  Geben Sie den in der DevMode Aktivierungs-App angezeigten Aktivierungscode ein. Ihrem Konto ist eine begrenzte Anzahl von Aktivierungen zugewiesen. Nachdem der Entwicklermodus aktiviert wurde, zeigt Partner Center an, dass Sie eine der Aktivierungen verwendet haben, die Ihrem Konto zugeordnet sind.

    ![Aktivierungsschritt 8](images/activation-step-8-rs2.png)    
    
9.  Klicken Sie auf **Agree and activate**. Dadurch wird die Seite neu geladen, und Ihr Gerät wird in der Tabelle aufgeführt. Die Nutzungsbedingungen für das Aktivierungsprogramm für den Xbox One-Entwicklermodus finden Sie unter [Programm zur Aktivierung des Xbox One-Entwicklermodus](/legal/windows/agreements/xbox-one-developer-mode-activation).

10. Nachdem Sie den Aktivierungscode eingegeben haben, wird auf der Konsole ein Statusbildschirm für den Aktivierungsvorgang angezeigt.  
    
11. Öffnen Sie nach Abschluss der Aktivierung die DevMode-Aktivierungs-App, und klicken Sie auf **Switch und restart**, um zum Entwicklermodus zu wechseln. Beachten Sie, dass dies länger als gewöhnlich dauert.

    ![Aktivierungsschritt 12](images/activation-step-12.png)   

## <a name="switch-between-retail-and-developer-mode"></a>Wechseln zwischen Einzelhandels- und Entwicklermodus
Nachdem der Entwicklermodus auf der Konsole aktiviert wurde, können Sie mithilfe von **Dev Home** zwischen Einzelhandelsmodus und Entwicklermodus wechseln. Weitere Informationen zum Starten und Verwenden von dev Home finden Sie unter [Einführung in Xbox One-Tools](introduction-to-xbox-tools.md).

* Öffnen Sie **dev Home**, um in den Einzelhandels Modus zu wechseln. Wählen Sie unter " **schnelle Aktionen**" die Option Entwicklungs **Modus verlassen** aus. Dadurch wird die Konsole im Einzelhandelsmodus neu gestartet.    

  ![Aktivierungsschritt 13](images/activation-step-13-rs4.png)  
  
* Um zum Entwicklermodus zu wechseln, verwenden Sie die DevMode-Aktivierungsapp. Öffnen Sie die APP, und wählen Sie **Switch und Restart** aus. Dadurch wird die Konsole im Entwicklermodus neu gestartet.  

  ![Aktivierungsschritt 14](images/activation-step-12.png)  

## <a name="see-also"></a>Siehe auch
- [Deaktivierung des Xbox One-Entwicklermodus](devkit-deactivation.md)
- [UWP auf Xbox One](index.md)
