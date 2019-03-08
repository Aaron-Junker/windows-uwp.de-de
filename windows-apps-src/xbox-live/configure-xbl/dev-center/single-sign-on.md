---
title: Konfigurieren Sie einmaliges Anmelden für im Partner Center
description: Beschreibt, wie Sie im Partner Center, um einen Titel ein, melden Sie einen Benutzer mithilfe ihrer Xbox Live-ID in Ihren Diensten ermöglichen einmaliges Anmelden konfigurieren können
ms.assetid: ''
ms.date: 02/21/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, udc, universal-Entwicklercenter, einmaliges Anmelden
ms.openlocfilehash: 32f06edd407d8c1fa74795d0a230c7d56ba8e838
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632875"
---
# <a name="configure-single-sign-on-in-partner-center"></a>Konfigurieren Sie einmaliges Anmelden für im Partner Center

Können einmaliges Anmelden ein Players unter Verwendung der Titel des, ihrer Xbox Live-Anmeldung mit in Ihren Diensten anzumelden. Dadurch können einen Player, der signiert wird, in Xbox Live, führen Sie eine app oder ein Spiel für den Dienst ohne ein zweites Mal mit einer anderen Anmeldeinformationen für Ihren Dienst anmelden zu müssen.

> [!NOTE]
> Dieses Thema gilt nicht für Titel, in dem Xbox Live Creators-Programm.

Z. B. möglicherweise der Titel einer app, die Ihr Dienst das Streamen von Inhalten (Videos, Musik usw.) auf ihr Gerät ermöglicht, solange sie ein gültiges Konto mit Ihrem Dienst verfügen. Wenn der Benutzer in ihrer Xbox Live-Konto angemeldet ist, sollten sie sich Inhalte streamen können, ohne sich mit Ihrem Dienst jedes Mal anmelden.

Auch wenn Ihre app Kinect-Daten an einen externen Dienst sendet, können Sie die hier konfigurieren.

Wenn Ihr Dienst erfordert, dass der Benutzer ein Konto von Xbox Live getrennt erstellt werden, sollten Sie eine Möglichkeit für den Benutzer auf ihre Xbox Live-Konto mit ihrem Konto für Ihren Dienst als link bereitstellen einmalige Aktion.

Wenn Sie einmaliges Anmelden konfigurieren, können Sie die URLs und ihrer vertrauenden Seite angeben. Jedes Mal, die Ihre app einer der URLs angegeben wird aufrufen, wird Xbox Live ein Xbox Secure Token Service (XSTS)-Token automatisch angefügt. Der Dienst, den Schlüssel empfängt, wird als bezeichnet ein *vertrauende*, und Sie müssen konfigurieren [vertrauende Seiten](https://developer.microsoft.com/en-US/xboxconfig/relyingparties/index) einmaliges Anmelden konfigurieren zu können. Jede parteikonfiguration der vertrauenden Seite gibt an, welche Daten enthalten sind, in das Token XSTS als auch einen eindeutigen Verschlüsselungsschlüssel, den die vertrauende Seite verwenden können, um das XSTS-Token zu decodieren.

Fügen Sie Konfiguration wie folgt hinzu:

1. Nach der Auswahl Ihrer Titels [Partner Center](https://partner.microsoft.com/dashboard), navigieren Sie zu **Services** > **Xbox Live**.

2. Klicken Sie auf den Link, um **Xbox Live-SSO**.

3. Klicken Sie auf die **-URL hinzufügen** klicken, um einen neuen einzelnen anmelden Eintrag zu erstellen. Dadurch wird eine neue Zeile am Ende der Liste der Konfigurationen hinzugefügt.

4. Geben Sie in das Feld URL die URL für Ihren Dienst über einen vollständig qualifizierten Domänennamen. Sie können die Unterdomäne der untersten Ebene ersetzen, mit einem Platzhalterzeichen ("\*"). Dies entspricht einer beliebigen URL, die über die gleichen übergeordneten Domänen verfügt. Z. B. "*. example.com&quot; "bar.example.com"oder"foo.bar.example.com"entspricht.

5. Wählen Sie im Feld der vertrauenden Seite Partei der vertrauenden Seite parteikonfiguration, die angibt, wie das Token XSTS codiert wird.

6. Klicken Sie auf die **speichern** Schaltfläche, um die Änderungen zu speichern.

![Screenshot der Seite für einmaliges Anmelden – Konfiguration](../../images/dev-center/single-signon.png)
