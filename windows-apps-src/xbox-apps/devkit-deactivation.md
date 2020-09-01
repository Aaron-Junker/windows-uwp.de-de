---
title: Deaktivierung des Xbox One-Entwicklermodus
description: Wenn Sie entscheiden, dass Sie die-Konsole für die Entwicklung nicht mehr verwenden möchten, verwenden Sie diese Schritte, um den Entwicklermodus zu deaktivieren.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 244124dd-d80a-4a72-91db-1c9c2fbc7c3c
ms.localizationpriority: medium
ms.openlocfilehash: 6e1629435151ca7f66ea5d2e4ef5c90e7336d4f8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174714"
---
# <a name="xbox-one-developer-mode-deactivation"></a>Deaktivierung des Xbox One-Entwicklermodus

Wenn Sie die Konsole nicht mehr für die Entwicklung verwenden möchten, führen Sie die folgenden Schritte aus, um den Entwicklermodus zu deaktivieren.

## <a name="switch-to-retail-mode"></a>Wechseln zum Einzelhandelsmodus

Setzen Sie zunächst die Xbox One-Konsole in den Einzelhandelsmodus zurück.

1. Öffnen Sie **Dev Home**.

2. Wählen Sie Entwicklungs **Modus verlassen**aus.  Die Konsole wird im Einzelhandelsmodus neu gestartet.  

   ![Beenden des Entwicklermodus](images/devkit-deactivation-1.png)

Deaktivieren Sie jetzt die Konsole mithilfe einer der folgenden Methoden.

## <a name="deactivate-your-console-using-the-dev-mode-activation-app"></a>Deaktivieren der Konsole mit der DevMode-Aktivierungsapp

Die bevorzugte Methode zum Deaktivieren des Entwicklermodus auf Ihrer Konsole ist die Verwendung der APP **dev Mode Activation** . 

1. Navigieren Sie zu **Games & apps**  >  **apps**.
  
   ![Aktivierungsschritt 3](images/devkit-deactivation-5.png)    
   
2.  Öffnen Sie die DevMode-Aktivierungs-App.

3.  Wählen Sie **Deaktivieren**aus.
  
    ![Deaktivieren der Konsole](images/deactivation-app.png)

Weitere Informationen zur Entwickler **Modus-Aktivierungs** -App finden Sie unter [Xbox One Developer Mode Activation](devkit-activation.md) . 

## <a name="reset-your-console"></a>Zurücksetzen der Konsole

Sie können den Entwicklermodus auch deaktivieren, indem Sie die Konsole zurücksetzen.  

> [!NOTE]
> Wenn Sie die Konsole zurücksetzen, gehen alle lokal gespeicherten Spieldaten verloren.

Führen Sie zum Zurücksetzen der Konsole die folgenden Schritte aus:

1.  Wechseln Sie zu **My games & apps**.

2.  Wählen Sie **Apps** und dann **Einstellungen**.

3.  Wechseln Sie im linken Bereich zu **System** , und wählen Sie dann im rechten Bereich **Konsolen Informationen** aus.   
   
    ![Console info and updates](images/devkit-deactivation-2.png)  
    
4.  Wählen Sie **Konsole zurücksetzen**aus.
    
    ![Reset console](images/devkit-deactivation-3.png)
    
5.  Wählen Sie als nächstes alle **Zurücksetzen**aus. Mit dieser Option wird die Konsole in den ursprünglichen Einzelhandelszustand zurückgesetzt.  Alle Apps, Spiele und lokal gespeicherten Daten werden gelöscht. Beachten Sie, dass die Konsole nicht aus dem Entwicklerprogramm gelöscht wird, wenn Sie die andere Option, **Reset and keep my games & apps**, auswählen.  
   
    ![Reset and remove everything](images/devkit-deactivation-4.png)

## <a name="deactivate-your-console-using-partner-center"></a>Deaktivieren der Konsole mithilfe von Partner Center

Wenn Sie aus irgendeinem Grund auf Ihre Konsole nicht zugreifen können, können Sie auch den Entwicklermodus in der Konsole mithilfe von Partner Center deaktivieren.

1. Navigieren Sie in Partner Center zur Seite [Xbox One-Konsolen verwalten](https://partner.microsoft.com/xboxconfig/devices) . Sie werden unter Umständen aufgefordert, sich anzumelden.

2. Suchen Sie in der Liste der Konsolen die Konsole, die Sie deaktivieren möchten, anhand der Seriennummer, Konsolen-ID oder Geräte-ID.  

3. Klicken Sie auf **Deaktivieren**.  
  
![Deaktivieren mit Dev Center](images/devkit-deactivation-6.png)

Wenn Sie Ihre Xbox One-Konsole nicht bereits in den Einzelhandels Modus zurückgeschaltet haben, können Sie dies jetzt tun, wie in [Wechsel zum Einzelhandels Modus](#switch-to-retail-mode)beschrieben.

## <a name="see-also"></a>Weitere Informationen
- [Aktivierung des Xbox One-Entwicklermodus](devkit-activation.md)
- [UWP auf Xbox One](index.md)
