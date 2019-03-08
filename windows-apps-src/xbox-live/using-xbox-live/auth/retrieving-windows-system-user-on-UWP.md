---
title: Abrufen von Windows-Systembenutzer in UWP
description: Erfahren Sie, wie Sie den Windows-System-Benutzer in einem Spiel universelle Windows-Plattform (UWP) abrufen.
ms.date: 06/07/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Systembenutzer
ms.localizationpriority: medium
ms.openlocfilehash: c46f7e98c2dea3b23beb2cec80816067d4c4e341
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632755"
---
# <a name="retrieving-the-windows-system-user-in-a-universal-windows-platform-uwp-title"></a>Abrufen von den Windows-System-Benutzer in einen Titel für die universelle Windows-Plattform (UWP)

## <a name="windowssystemuser"></a>Windows.System.User

Ein [Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user) -Objekt stellt einen lokalen Windows-Benutzer dar. Auf der Xbox-Konsole ermöglicht es Ihnen mehrere Windows angemeldet sein, zur gleichen Zeit in einer interaktiven Sitzung, wenn Ihre app eine Anwendung mit mehreren Benutzern ist, und klicken Sie dann ein Objekt Windows.System.User benötigt werden, um eine XboxLiveUser zu erstellen, um live-Dienste zugreifen. Es wurde entweder nur eine Windows-Benutzer auf einem Gerät zulassen, oder eine interaktive Sitzung übergeben eine Windows.System.User zum Erstellen einer XboxLiveUser ist nicht erforderlich, auf andere Windows-Plattformen wie PC oder Phone.

## <a name="ways-to-retrieve-windows-system-user"></a>Möglichkeiten zum Abrufen von Windows-System-Benutzer

* **Mithilfe der statischen Methoden von [Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user) Klasse.**

  [Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user) Klasse bietet eine Reihe von statischen Methode zu erleichtern, Windows.System.User Serverobjekte werden abgerufen. Sie können z. B. FindAllAsync rufen Sie alle aktiven Windows-Benutzer aufrufen.

* **[UserPicker](https://docs.microsoft.com/en-us/uwp/api/windows.system.userpicker)**

  [Windows.System.UserPicker](https://docs.microsoft.com/en-us/uwp/api/windows.system.userpicker) -Klasse enthält Methoden zum Starten von Benutzerauswahl-UI, und wählen einen Windows-System-Benutzer in Szenarien mit mehreren Benutzern.

* **[IGameController](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.igamecontroller)**

  [Windows.Gaming.Input.IGameController](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.igamecontroller) ist die Kernschnittstelle, die für alle Controller-Geräte (Gamepad, Rennspiels Wheel, Steuerknüppel usw.). Sie erhalten möglicherweise die Windows.System.User-Objekt, das der game Controller zugeordnet sind, durch den Aufruf der User-Eigenschaft.  

  Hier sind zwei Gamecontroller von Windows implementiert [ArcadeStick](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.arcadestick), [FlightStick](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.flightstick), [Gamepad](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.gamepad), [RacingWheel](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.racingwheel).

* **[UserDeviceAssociation](https://docs.microsoft.com/en-us/uwp/api/windows.system.userdeviceassociation)**

  Sie können statische Methode FindUserFromDeviceId, den zugehörigen Benutzer mit Geräte-Id aufrufen. Sie können die Geräte-Id in der Regel aus Windows-Eingabeereignisse, abrufen, z. B. [Windows.UI.Xaml.Input.KeyRoutedEventArgs](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs), [Windows.UI.Core.KeyEventArgs](https://docs.microsoft.com/en-us/uwp/api/windows.ui.core.keyeventargs)
