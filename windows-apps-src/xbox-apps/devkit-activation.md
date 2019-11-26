---
title: Aktivierung des Entwicklermodus auf Xbox One
description: Dieser Artikel beschreibt das Aktivieren des Entwicklermodus, sodass Sie zwischen Retailmodus und Entwicklermodus wechseln können.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: ade80769-17ae-46e9-9c2f-bf08ae5a51ee
ms.localizationpriority: medium
ms.openlocfilehash: 95b65e63c081734a560a852a5d064ef76c423ef6
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258722"
---
# <a name="xbox-one-developer-mode-activation"></a>Aktivierung des Entwicklermodus auf Xbox One

## <a name="how-developer-mode-works"></a>Funktionsweise des Entwicklermodus
Die Xbox One verfügt über zwei Modi: *Einzelhandelsmodus* (**1**) und *Entwicklermodus* (**2**). Im Einzelhandelsmodus ist die Konsole in dem Zustand, in dem sie von jedem Kunden oder Benutzer einer Xbox One-Konsole verwendet wird: Sie können als Benutzer Spiele spielen und Apps ausführen. Im Entwicklermodus können Sie Software für die Konsole entwickeln, jedoch keine Spiele spielen und Apps ausführen.

Der Entwicklermodus kann auf jeder Xbox One-Konsole aktiviert werden. Nach dem Aktivieren des Entwicklermodus können Sie zwischen dem Einzelhandelsmodus (**2a**) und dem Entwicklermodus (**2b**) wechseln.

![Xbox One-Modi](images/dev-mode-flow.png)

## <a name="activate-developer-mode-on-your-retail-xbox-one-console"></a>Aktivieren des Entwicklermodus auf der Xbox One-Konsole

1.  Starten Sie die Xbox One-Konsole.

2.  Suchen Sie die **DevMode-Aktivierungs**-App im Xbox One-Store, und installieren Sie sie.

    ![Installieren der DevMode-Aktivierungs-App](images/devkit-activation-1.png)

3.  Starten Sie die App über die Store-Seite.

    ![DevMode-Aktivierungs-App](images/devkit-activation-2.png)

4.  Notieren Sie sich den in der DevMode-Aktivierungs-App angezeigten Code.

    ![Aktivierungsschritt 5](images/activation-step-5.png)  
    
5.  [Registrieren Sie ein App-Entwicklerkonto im Partner Center](https://developer.microsoft.com/store/register).  Dies ist auch der erste Schritt zum Veröffentlichen Ihres Spiels.

6.  Melden Sie sich mit Ihrem gültigen, aktuellen Partner Center-App-Entwicklerkonto bei [Partner Center](https://partner.microsoft.com/dashboard) an.  Wenn im linken Navigationsbereich nicht mehrere Optionen angezeigt werden oder die Option **neue APP erstellen** im **Übersichts** Abschnitt nicht angezeigt wird, funktionieren die folgenden Schritte und Aktivierungs Links _nicht_. Stellen Sie sicher, dass Sie Ihr App-Entwicklerkonto im vorherigen Schritt vollständig registriert haben.

7.  Wechseln Sie zu [Partner.Microsoft.com/xboxconfig/Devices](https://partner.microsoft.com/xboxconfig/devices).

8.  Geben Sie den in der DevMode Aktivierungs-App angezeigten Aktivierungscode ein. Ihrem Konto ist eine begrenzte Anzahl von Aktivierungen zugewiesen. Nachdem der Entwicklermodus aktiviert wurde, zeigt Partner Center an, dass Sie eine der Aktivierungen verwendet haben, die Ihrem Konto zugeordnet sind.

    ![Aktivierungsschritt 8](images/activation-step-8-rs2.png)    
    
9.  Klicken Sie auf **Agree and activate**. Dadurch wird die Seite neu geladen, und Ihr Gerät wird in der Tabelle aufgeführt. Die Nutzungsbedingungen für das Aktivierungsprogramm für den Xbox One-Entwicklermodus finden Sie unter [Programm zur Aktivierung des Xbox One-Entwicklermodus](https://docs.microsoft.com/legal/windows/agreements/xbox-one-developer-mode-activation).

10. Nachdem Sie den Aktivierungscode eingegeben haben, wird auf der Konsole ein Statusbildschirm für den Aktivierungsvorgang angezeigt.  
    
11. Öffnen Sie nach Abschluss der Aktivierung die DevMode-Aktivierungs-App, und klicken Sie auf **Switch und restart**, um zum Entwicklermodus zu wechseln. Beachten Sie, dass dies länger als gewöhnlich dauert.

    ![Aktivierungsschritt 12](images/activation-step-12.png)   

## <a name="switch-between-retail-and-developer-mode"></a>Wechseln zwischen Einzelhandels- und Entwicklermodus
Nachdem der Entwicklermodus auf der Konsole aktiviert wurde, können Sie mithilfe von **Dev Home** zwischen Einzelhandelsmodus und Entwicklermodus wechseln. Weitere Informationen zum Starten und Verwenden von Dev Home, finden Sie unter [Einführung in Xbox One-Tools](introduction-to-xbox-tools.md).

* Um in den Einzelhandelsmodus wechseln, öffnen Sie **Dev Home**. Wählen Sie unter **Schnelle Aktionen** die Option **Entwicklermodus verlassen**. Dadurch wird die Konsole im Einzelhandelsmodus neu gestartet.    

  ![Aktivierungsschritt 13](images/activation-step-13-rs4.png)  
  
* Um zum Entwicklermodus zu wechseln, verwenden Sie die DevMode-Aktivierungsapp. Öffnen Sie die App, und wählen Sie **Wechseln und neu starten**. Dadurch wird die Konsole im Entwicklermodus neu gestartet.  

  ![Aktivierungsschritt 14](images/activation-step-12.png)  

## <a name="see-also"></a>Weitere Informationen:
- [Xbox One-Entwicklermodus-Deaktivierung](devkit-deactivation.md)
- [UWP auf Xbox One](index.md)
