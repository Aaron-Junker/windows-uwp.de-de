---
title: Authentifizierung und Benutzeridentität
description: Apps für die universelle Windows-Plattform (UWP) bieten mehrere Optionen für die Benutzerauthentifizierung – vom einfachen einmaligen Anmelden (Single Sign-On, SSO) mit einem Webauthentifizierungsbroker bis hin zur äußerst sicheren zweistufigen Authentifizierung.
ms.assetid: 53E36DDC-200A-4850-AAF0-07ECA3662BB9
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Sicherheit
ms.localizationpriority: medium
ms.openlocfilehash: b350aa58d7fed0bb78965dcd4742d759b8e9d988
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282450"
---
# <a name="authentication-and-user-identity"></a>Authentifizierung und Benutzeridentität



UWP (Universelle Windows-Plattform)-Apps bieten mehrere Optionen für die Benutzerauthentifizierung, vom einfachen einmaligen Anmelden (Single Sign-On, SSO) mit einem [Webauthentifizierungsbroker](web-authentication-broker.md) bis hin zur äußerst sicheren zweistufigen Authentifizierung.

Normale App-Verbindungen mit Identitätsanbieterdiensten von Drittanbietern, z. B. Facebook, Twitter, Flickr usw., verwenden den [Webauthentifizierungsbroker](web-authentication-broker.md). Verwenden Sie das [Schließfach für Anmeldeinformationen](credential-locker.md), um die Benutzerfreundlichkeit beim Speichern und Roaming der Anmeldeinformationen des Benutzers zu verbessern.

Unternehmen, die Windows 10 verwenden, sollten dringend die Verwendung von [Microsoft Passport und Windows Hello](microsoft-passport.md)in Erwägung gezogen, das eine äußerst sichere zweistufige Authentifizierung ermöglicht. Wenn Microsoft Passport nicht verwendet werden kann, bieten [Smartcards](smart-cards.md) und der [biometrische Fingerabdruck](fingerprint-biometrics.md) eine zusätzliche Sicherheitsstufe.

<table>
<tr><th>Thema</th><th>Beschreibung</th></tr>
<tr><td><a href="credential-locker.md">Schließfach für Anmeldeinformationen</a></td><td>In diesem Artikel wird beschrieben, wie Apps mit dem Schließfach für Anmeldeinformationen Benutzeranmeldeinformationen sicher speichern und abrufen können und per Roaming mit dem Microsoft-Konto des Benutzers zwischen Geräten übertragen können.</td></tr>

<tr><td><a href="fingerprint-biometrics.md">Biometrischer Fingerabdruck</a> </td><td>In diesem Artikel wird beschrieben, wie Sie Ihrer App einen biometrischen Fingerabdruck hinzufügen. Sie können die Sicherheit Ihrer App erhöhen, indem Sie die Anforderung einer Authentifizierung per Fingerabdruck integrieren, wenn die Zustimmung des Benutzers für eine bestimmte Aktion erforderlich ist. So können Sie beispielsweise die Authentifizierung per Fingerabdruck vor der Autorisierung eines In-App-Kaufs oder vor dem Zugriff auf eingeschränkte Ressourcen anfordern. Die Verwaltung der Authentifizierung per Fingerabdruck erfolgt mithilfe der <a href="https://docs.microsoft.com/uwp/api/Windows.Security.Credentials.UI.UserConsentVerifier">UserConsentVerifier</a>-Klasse im <a href="https://docs.microsoft.com/uwp/api/Windows.Security.Credentials.UI">Windows.Security.Credentials.UI</a>-Namespace.</td></tr>
<tr><td><a href="microsoft-passport.md">Microsoft Passport und Windows Hello</a></td><td>In diesem Artikel wird die neue Microsoft Passport-Technologie von Windows 10 beschrieben und erörtert, wie Entwickler diese Technologie implementieren, um ihre Apps und Back-End-Dienste zu schützen. Darin werden die spezifischen Funktionen dieser Technologien hervorgehoben, die dabei helfen, aus herkömmlichen Anmeldeinformationen erwachsende Bedrohungen zu mindern. Darüber hinaus bietet sie eine Anleitung darüber, wie diese Technologien als Bestandteil Ihrer Windows 10-Einführung entworfen und bereitgestellt werden. </td></tr>
<tr><td><a href="microsoft-passport-login.md">Erstellen einer Microsoft Passport-Anmelde-App</a></td><td>Teil 1 der umfassenden exemplarischen Vorgehensweise zum Erstellen einer Windows 10-App für die Universelle Windows-Plattform (UWP), die Microsoft Passport als Alternative zu herkömmlichen Authentifizierungssystemen mit Benutzername und Kennwort verwendet.</td></tr>
<tr><td><a href="microsoft-passport-login-auth-service.md">Erstellen eines Microsoft Passport-Anmeldediensts</a></td><td>Teil 2 der umfassenden exemplarischen Vorgehensweise zum Verwenden von Microsoft Passport als Alternative zu herkömmlichen Authentifizierungssystemen mit Benutzername und Kennwort in Windows 10-Apps für die universelle Windows-Plattform (UWP).</td></tr>
<tr><td><a href="smart-cards.md">Smartcards</a></td><td>In diesem Thema wird erläutert, wie Apps Smartcards verwenden können, um Benutzer mit sicheren Netzwerkdiensten zu verbinden, einschließlich Informationen für den Zugriff auf physische Smartcardleser, zum Erstellen virtueller Smartcards, zum Kommunizieren mit Smartcards, zum Authentifizieren von Benutzern, zum Zurücksetzen von Benutzer-PINs und zum Entfernen oder Trennen von Smartcards.</td></tr>
<tr><td><a href="share-certificates.md">Freigeben von Zertifikaten zwischen Apps</a></td><td>UWP-Apps, die über eine Kombination aus Benutzer-ID und Kennwort hinaus eine sichere Authentifizierung benötigen, können Zertifikate für die Authentifizierung verwenden. Die Zertifikatauthentifizierung bietet eine hohe Vertrauenswürdigkeit bei der Benutzerauthentifizierung. Es kann vorkommen, dass eine Gruppe von Diensten einen Benutzer für mehrere Apps authentifizieren möchte. In diesem Artikel wird gezeigt, wie Sie mehrere Apps mit demselben Zertifikat authentifizieren und für einen Benutzer geeigneten Code zum Importieren eines Zertifikats bereitstellen können, das für den Zugriff auf sichere Webdienste bestimmt ist.</td></tr>
<tr><td><a href="companion-device-unlock.md">Entsperren von Windows mit Begleitgeräten (IoT)</a></td><td>Ein Begleitgerät ist ein Gerät, das in Verbindung mit dem Windows 10-Desktopgerät zur Verbesserung der Benutzerauthentifizierung verwendet werden kann. Mit dem Begleitgeräteframework kann ein Begleitgerät umfangreiche Funktionen für Microsoft Passport bereitstellen, auch wenn Windows Hello nicht verfügbar ist (beispielsweise, wenn das Windows 10-Desktopgerät über keine Kamera für die Gesichtsauthentifizierung oder kein Fingerabdrucklesegerät verfügt).</td></tr>
<tr><td><a href="web-account-manager.md">Webkonto-Manager</a></td><td>In diesem Artikel wird beschrieben, wie Sie AccountsSettingsPane anzeigen und Ihre App für die universelle Windows-Plattform (UWP-App) mit externen Identitätsanbietern wie Microsoft oder Facebook verbinden. Dazu verwenden Sie die neuen Web Account Manager-APIs in Windows 10. Sie erfahren, wie Sie die Berechtigung eines Benutzers für die Verwendung seines Microsoft-Kontos anfordern können, ein Zugriffstoken erhalten und damit grundlegende Vorgänge (wie das Abrufen von Profildaten oder das Hochladen von Dateien in das OneDrive-Verzeichnis des Benutzers) durchführen können. </td></tr>
<tr><td><a href="web-authentication-broker.md">Webauthentifizierungsbroker</a></td><td>In diesem Artikel wird erläutert, wie Ihre App eine Verbindung mit einem Onlineidentitätsanbieter herstellen kann, der Authentifizierungsprotokolle wie OpenID oder OAuth verwendet, wie Twitter, Facebook, Flickr, Instagram usw. Die <a href="https://docs.microsoft.com/uwp/api/windows.security.authentication.web.webauthenticationbroker.authenticateasync">AuthenticateAsync</a>-Methode sendet eine Anforderung an den Onlineidentitätsanbieter und erhält als Antwort ein Zugriffstoken, das die für die App zugänglichen Anbieterressourcen beschreibt.</td></tr>
</table>

