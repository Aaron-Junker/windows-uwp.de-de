---
title: Konfigurieren Sie die Xbox-Entwicklung
description: Erfahren Sie, wie Sie die Xbox-Entwicklung zur Unterstützung von Xbox Live-Entwicklung zu konfigurieren.
ms.assetid: f8fd1caa-b1e9-4882-a01f-8f17820dfb55
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 479be2401e0c54801645ad1c0d91b11b7ffb6869
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649015"
---
# <a name="configure-your-xbox-development-console"></a>Konfigurieren Sie die Xbox-Entwicklung

Konfigurieren Sie Ihre entwicklungskonsole:
- Ihre IDs erhalten
- Legen Sie Ihre Sandbox für Ihre Development-kits
- Melden Sie sich mit einem entwicklungskonto

## <a name="get-your-ids"></a>Ihre IDs erhalten
Um Sandboxes und Xbox Live-Dienste aktivieren, müssen Sie mehrere IDs, die zum Konfigurieren des Development Kits und den Titel zu erhalten. Diese können mit dem gleichen Prozess erfolgen.

Führen Sie [Xbox Live-Dienstkonfiguration](../xbox-live-service-configuration.md) Ihre IDs abrufen

## <a name="set-your-sandbox-on-your-development-kits"></a>Legen Sie Ihre Sandbox für Ihre Development-kits
Nicht werden Ihr Development Kit zu starten, ohne Ihre Sandkasten-ID Zu diesem Zweck können Sie die "Xbox ein Manager", die von der xdk-Version auf Ihrem PC installiert ist, oder Sie können ein Befehlsfenster xdk-Version öffnen und verwenden Sie den Konfigurationsbefehl (xbconfig.exe) wie folgt:

Überprüfen Sie Ihre aktuellen Sandbox. Geben Sie an der Eingabeaufforderung Xbconfig-ID ein.

Ändern Sie die Sandkasten-Id, ist dies nicht Ihren Erwartungen. Geben Sie Xbconfig Sandboxid =<your sandbox id> an der Eingabeaufforderung.

Starten Sie Ihre Konsole mit der ein Neustart (xbreboot.exe) an der Eingabeaufforderung neu.

Stellen Sie sicher, dass Ihre Sandbox ordnungsgemäß zurückgesetzt wurde. Geben Sie an der Eingabeaufforderung Xbconfig-ID ein.

## <a name="sign-in-with-a-development-account"></a>Melden Sie sich mit einem entwicklungskonto

Entwicklung von Konten verwendet, um die Anmeldung bei [Xbox-Entwickler-Portal (XDP)](https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts) oder [Partner Center](https://partner.microsoft.com/dashboard)
