---
description: Dies ist ein Übersichtsthema mit umfassenden Informationen für Entwickler zum Zusammenhang zwischen der Windows Information Protection (WIP) und Dateien, Puffern, der Zwischenablage, dem Netzwerk, Hintergrundaufgaben und dem Schutz von Daten bei Sperre.
MS-HAID: dev\_enterprise.edp\_hub
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Windows Information Protection (WIP)
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Windows Information Protection, Unternehmensdaten, Unternehmens Datenschutz, EDP, aktivierte apps
ms.assetid: 08f0cfad-f15d-46f7-ae7c-824a8b1c44ea
ms.localizationpriority: medium
ms.openlocfilehash: cb3006541a5e14936035bc7b9d3b6493df031823
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032013"
---
# <a name="windows-information-protection-wip"></a>Windows Information Protection (WIP)

__Hinweis__ Die Windows Information Protection (WIP)-Richtlinie kann auf Windows 10, Version 1607 angewendet werden.

WIP schützt Daten, die zu einem Unternehmen gehören, indem Richtlinien durchgesetzt werden, die von dem Unternehmen definiert sind. Wenn Ihre App diese Richtlinien enthält, unterliegen alle Daten von der App den Richtlinien. Dieses Thema unterstützt Sie beim Erstellen von Apps, die diese Richtlinien besser durchsetzen, ohne Auswirkung auf persönliche Daten des Benutzers.
<iframe src="https://channel9.msdn.com/Blogs/Windows-Development-for-the-Enterprise/Securing-Enterprise-Data-with-Windows-Information-Protection/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="first-what-is-wip"></a>Als Erstes: Was ist WIP?

WIP ist ein Satz von Features auf Desktops, Laptops, Tablets und Smartphones, welche das Mobile Device Management (MDM)-System und das System zur mobilen Anwendungsverwaltung (Mobile Application Management; MAM) des Unternehmens unterstützen.

WIP kann mit MDM dem Unternehmen mehr Kontrolle darüber geben, wie Daten des Unternehmens auf Geräten, die das Unternehmen verwaltet, behandelt werden. In einigen Fällen bringen Benutzer Geräte mit zur Arbeit und registrieren sie nicht im MDM der Organisation.  In diesen Fällen können Organisationen MAM verwenden, um eine bessere Kontrolle über die Behandlung ihrer Daten in bestimmten Branchen-Apps zu erhalten, die Benutzer auf dem Gerät installieren.

Mit MDM oder MAM können Administratoren angeben, welche Apps auf Dateien zugreifen dürfen, die dem Unternehmen gehören, und ob Benutzer Daten aus diesen Dateien kopieren und dann in persönliche Dokumente einsetzen können.

Dies funktioniert folgendermaßen: Benutzer registrieren ihre Geräte im System für Verwaltung mobiler Geräte (Mobile Device Management, MDM). Ein Administrator im Verwaltungsunternehmen verwendet Microsoft Intune oder System Center Configuration Manager (SCCM) zur Definition einer Richtlinie und anschließender Bereitstellung auf den registrierten Geräten.

Wenn Benutzer ihre Geräte nicht registrieren müssen, verwenden Administratoren ihr MAM-System, um eine Richtlinie zu definieren und bereitzustellen, die für spezifische Apps gilt. Wenn Benutzer diese Apps installieren, erhalten sie die zugehörige Richtlinie.

Diese Richtlinie identifiziert die Apps, die Zugriff auf Unternehmensdaten haben dürfen (auch *Liste der zugelassenen Apps* der Richtlinie genannt). Diese Apps können auf geschützte Unternehmensdateien, virtuelle Private Netzwerke (VPN) und Unternehmensdaten in der Zwischenablage oder über einen Freigabe-Vertrag zugreifen. Die Richtlinie definiert auch die Regeln, die für die Daten gelten. Beispielsweise, ob Daten von unternehmenseigenen Dateien kopiert und dann in nicht unternehmenseigene Dateien eingesetzt werden können.

Wenn Benutzer die Registrierung ihres Geräts im MDM-System der Organisation aufheben oder Apps deinstallieren, die vom MAM-System der Organisation erkannt werden, können Administratoren Unternehmensdaten remote vom Gerät entfernen.

![WIP-Lebenszyklus](images/wip-lifecycle.png)

> **Weitere Informationen zu WIP** <br>
* [Einführung in Windows Information Protection](https://techcommunity.microsoft.com/t5/Windows-IT-Pro-Blog/bg-p/Windows10Blog)
* [Schützen von Unternehmensdaten mit Windows Information Protection (WIP)](/windows/whats-new/edp-whats-new-overview)

Wenn Ihre App auf der Liste der zugelassenen Apps steht, unterliegen alle von der App erstellten Daten den Einschränkungen der Richtlinien. Das bedeutet: Wenn Administratoren den Zugriff des Benutzers auf Unternehmensdaten widerrufen, geht dem Benutzer der Zugriff auf alle Daten verloren, die Ihre App erstellt hat.

Dies ist in Ordnung, wenn Ihre App nur für Unternehmen entwickelt wurde. Wenn Ihre App Daten erstellt, die die Benutzer als persönlich erachten, sollten Sie Ihre App *optimieren* , intelligent zwischen Unternehmens- und persönlichen Daten zu unterscheiden. Wir bezeichnen diese Art App *unternehmensoptimiert* , da sie problemlos eine Unternehmensrichtlinie durchsetzen und gleichzeitig die Integrität der persönlichen Daten des Benutzers beibehalten kann.

## <a name="create-an-enterprise-enlightened-app"></a>Erstellen Sie eine unternehmensoptimierte App

Verwenden Sie WIP-APIs, um Ihre App zu optimieren und dann als unternehmensoptimiert zu deklarieren.

Optimieren Sie Ihre App, wenn diese für Unternehmens- und persönliche Zwecke verwendet wird.

Optimierung der App, wenn Sie das Durchsetzen der Richtlinienelemente problemlos behandeln möchten.

Beispiel: Wenn die Richtlinie Benutzern erlaubt, Unternehmensdaten in einem privaten Dokument einzusetzen, können Sie verhindern, dass Benutzer auf ein Zustimmungsdialogfeld reagieren müssen, bevor sie die Daten einsetzen können. Auf ähnliche Weise können Sie benutzerdefinierte Dialogfelder zu Informationszwecken als Antwort auf diese Art von Ereignissen darstellen.

Wenn Sie bereit sind, die App zu optimieren, sehen Sie sich eines dieser Handbücher an:

**Für universelle Windows-Plattform-Apps (UWP), die Sie mithilfe von C erstellen #**

[WIP-Entwicklerhandbuch (Windows Information Protection)](wip-dev-guide.md).

**Für Desktop-Apps, die Sie mit C++ erstellen**

[Entwicklerhandbuch zu Windows Information Protection (WIP) (C++)](/previous-versions/windows/desktop/EDP/wip-developer-guide).


## <a name="create-non-enlightened-enterprise-app"></a>Erstellen einer nicht für Unternehmen optimierten App

Wenn Sie eine Branchen-App erstellen, die nicht zur privaten Verwendung vorgesehen ist, müssen Sie diese nicht optimieren.

### <a name="windows-desktop-apps"></a>Windows-Desktop-Apps
Sie müssen eine Windows-Desktop-App nicht optimieren, sollten aber Ihre App testen, um sicherzustellen, dass sie ordnungsgemäß unter der Richtlinie funktioniert. Starten Sie beispielsweise Ihre App, verwenden Sie sie, und heben Sie dann die Registrierung des Geräts im MDM auf. Stellen Sie dann sicher, dass die App gestartet werden kann. Wenn für den Betrieb der App wichtige Dateien verschlüsselt werden, kann die App möglicherweise nicht gestartet werden. Überprüfen Sie außerdem die Dateien, mit denen Ihre App interagiert, um sicherzustellen, dass Ihre App nicht versehentlich private Dateien des Benutzers verschlüsselt. Dies kann Metadatendateien, Bilder und andere Dinge umfassen.

Nach dem Test Ihrer App fügen Sie der Ressourcendatei oder Ihrem Projekt dieses Kennzeichen hinzu, und kompilieren Sie die App dann neu.

```cpp
MICROSOFTEDPAUTOPROTECTIONALLOWEDAPPINFO EDPAUTOPROTECTIONALLOWEDAPPINFOID
BEGIN
    0x0001
END
```
Die MDM-Richtlinien erfordern zwar kein Kennzeichen, die MAM-Richtlinien allerdings schon.

### <a name="uwp-apps"></a>UWP-Apps

Wenn Ihre App in einer MAM-Richtlinie enthalten sein soll, müssen Sie sie optimieren. Für Richtlinien, die für Geräte unter MDM bereitgestellt wurden, ist dies nicht erforderlich. Aber wenn Sie Ihre App an Unternehmenskunden verteilen, wird es schwierig, wenn nicht unmöglich, den Typ des verwendeten Richtlinienverwaltungssystems zu ermitteln. Optimieren Sie Ihre App, um zu gewährleisten, dass Ihre App in beiden Richtlinienverwaltungssystemen (MDM und MAM) funktioniert.






 
