---
title: Webdienste
description: Erfahren Sie, wie Sie einen Webdienst für Ihre app erstellen
ms.assetid: ''
ms.date: 06-04-2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Webdienste
ms.openlocfilehash: 66668336e3575201b305e6ecf09b4f277d2fc7b3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600015"
---
# <a name="set-up-web-services-in-udc"></a>Einrichten von Webdiensten in UDC

> [!WARNING]
> Im folgende Artikel ist für ID@Xbox und verwaltete Partnerkonten Entwickler nur aufgrund der Einschränkungen, die auf Web-Dienstkonfiguration. Web Services-Konfiguration ist nur mit den vertrauenden Seiten Konto Berechtigung auf Serverebene gewährt Entwicklern zur Verfügung. Wenn Sie keine Kontrolle über Ihr Konto an, dass die Berechtigungen auf Serverebene Ihrer Entwicklung Account Manager (DAM) wenden Sie sich an.

Herausgeber können Webdienste erstellen, sollten sie die Methode anpassen, die ihre apps/Titel mit Xbox Live-Diensten interagieren. Webdienste werden auf verlegerebene Konfigurationen und können von jeder Titel in einer Sandbox, die im Besitz vom Verleger Konfigurieren der einmaligen Anmeldung aufgerufen werden.

Gründe für die Webdienste definieren:

1. Einmaliges Anmelden bereitstellen, Xbox Live-Benutzer – In der Reihenfolge für den Webdienst Single-Sign-on für Xbox Live-Benutzer angeben, muss sie als eine vertrauende Seite von Xbox Live konfiguriert werden. Wenn auf diese Weise konfiguriert werden, werden Benutzer, die für Xbox Live authentifiziert werden automatisch mit Ihrem Dienst authentifiziert ohne einen anderen Satz von Anmeldeinformationen erneut eingeben zu müssen.
2. Bedienen von Dienstaufrufen von einem Dienst für Xbox Live Services - vornehmen, wenn Ihr Produkt einer Ihrer Webdienste zum Verwenden einer Xbox Live-Dienst aufrufen, benötigen entweder direkt oder für die einzelnen Benutzer Sie geschäftliche Partner ein Zertifikat.

1. ## <a name="create-a-web-service"></a>Erstellen eines Webdiensts

1. Wechseln Sie zu der [Partner Center-Dashboard](https://partner.microsoft.com/dashboard/windows/overview)  
2. Klicken Sie auf das strukturiert Zahnradsymbol auf der oberen rechten Ecke der Seite, um Zugriff auf die **Einstellungen** Dropdownliste.
3. Wählen Sie in der Dropdown-Liste **Entwicklereinstellungen**.
4. Erweitern Sie auf der linken Navigationsleiste die Option **Xbox Live** , und wählen Sie **Webdienste**.

![Web Services gif ](../../images/dev-center/web-services/web-services.gif)

5. Klicken Sie auf der Webdienstseite **neuen Webdienst**.
6. Geben Sie den Namen des Webdienstes aus, und wählen Sie den Zugriff nach Bedarf.  
    * Telemetriezugriff kann Ihr Dienst für alle Ihre Spiele spielen Telemetriedaten abrufen.
    * App-Channel-Zugriff bietet den Media-Anbieter, der Besitzer des Diensts der Berechtigung zum Veröffentlichen Sie programmgesteuert auf app-Kanäle für die Nutzung in der Konsole über die Verflechtung OneGuide.
7. Klicken Sie auf **Speichern**.

An diesem Punkt haben Sie den Dienst definiert und Xbox Live ist sich bewusst, von der Existenz des Diensts. Abhängig von den Gründen für das Erstellen des Webdiensts müssen Sie zum Konfigurieren der vertrauenden Seite Parties(Single Sing-On) oder Business-Partner Certificates(Service-to-service calls).  

## <a name="configure-relying-party"></a>Vertrauende Seite konfiguriert

Ein Webdienst muss konfiguriert werden, wie eine vertrauende Seite von Xbox live, um die Single-Sign-On für Xbox Live-Benutzer zu ermöglichen – zu Xbox Live authentifizierten Benutzern automatisch an den Webdienst authentifiziert werden, werden ohne zur erneuten Eingabe ein anderen Satz von Anmeldeinformationen. Um dies zu ermöglichen, muss die Vertrauensstellung zwischen Xbox-Diensten und den Webdienst hergestellt werden. Ein Satz von Ansprüchen (z. B. Gamertag, Gerätetyp, Titel-ID) werden als Teil der vertrauenden Seite Partei-Konfigurationen verwendet, um diese Vertrauensstellung zu erzwingen. Dies sind die Informationen, die zwischen den Xbox Live und den Webdienst, damit der Benutzer automatisch authentifizieren.

### <a name="create-a-relying-party"></a>Erstellen einer vertrauenden Seite

1. Wechseln Sie zu dem Partner Center-Dashboard  
2. Klicken Sie auf das strukturiert Zahnradsymbol auf der oberen rechten Ecke der Seite, um Zugriff auf die **Einstellungen** Dropdownliste.
3. Wählen Sie in der Dropdown-Liste **Entwicklereinstellungen**.
4. Erweitern Sie auf der linken Navigationsleiste die Option **Xbox Live** , und wählen Sie **vertrauenden Seiten**.
5. Klicken Sie auf der vertrauenden Seite auf **neue Relying Party**.
6. Geben Sie einen URI für die vertrauende Seite in folgendem Format: *"example.com"*.
7. Wählen Sie die Verschlüsselung verwendet werden, um sicherzustellen, dass der Dienst einer vertrauenden Seite Sicherheit.
8. Wenn Sie die symmetrische Verschlüsselung mit freigegebenen Schlüsseln, die im vorherigen Schritt ausgewählt haben, klicken Sie auf **neue Schlüssel generieren** um einen neuen gemeinsam verwendeten Schlüssel zu erhalten. Befolgen Sie die Anweisungen auf dem Bildschirm, um den Schlüssel sicher speichern.
9. Geben Sie die **Token Lebensdauer** in Stunden.
10. Klicken Sie unter **Ansprüche**, die Dropdownliste bietet eine Liste von Ansprüchen, die Ihrem Dienst einer vertrauenden Seite für die Authentifizierung verwenden können. Wählen Sie alle Ansprüche, die Sie verwenden möchten. Die ausgewählte Ansprüche werden unterhalb der Dropdownliste angezeigt. Einige standardansprüche werden in diesem Bereich wird standardmäßig aufgefüllt.
11. Klicken Sie auf **speichern** Wenn Sie fertig sind.  

## <a name="configure-a-business-partner-certificate"></a>Konfigurieren Sie ein Zertifikat des Hostpartners Business

Wenn Ihr Produkt einer Ihrer Webdienste verwendet, um Aufrufe an eine Xbox Live-Dienst, entweder direkt oder für die einzelnen Benutzer ausführen, benötigen Sie ein Business-Partner-Zertifikat.

### <a name="generate-a-business-partner-certificate"></a>Generieren Sie ein Zertifikat des Hostpartners Business

Fahren Sie nach erfolgreicher Erstellung eines Webdiensts mit den folgenden Schritten fort.  

1. Finden Sie auf der Webdienstseite den Webdienst, dem Sie ein Business Partnerzertifikat zuordnen möchten.
2. Wählen Sie die **Zertifikat generieren** Link für den ausgewählten Webdienst.
3. Klicken Sie auf **Anzeigeoptionen** neben ein neues Zertifikat generiert. Dadurch werden Befehle angezeigt, die von PowerShell mit Administratorrechten ausgeführt werden soll.
4. Alle Befehle nacheinander ausgeführt, nach dem anderen erfolgreich eine Base64 erzielen-codierte Blob. Dies ist der öffentliche Schlüssel. Kopieren Sie den öffentlichen Schlüssel aus PowerShell, und fügen Sie ihn in den Platzhalter für das CSP-Blob.
5. Klicken Sie auf **herunterladen** und befolgen Sie die Anweisungen auf der Seite für die Bindung der Zertifikate.
    1. Verwenden Sie den gleichen Computer, die, den Sie zum Generieren des öffentlichen Schlüssels verwendet.
    2. Führen Sie diesen Befehl in PowerShell: *mmc.exe*
    3. Wählen Sie die Datei, und wählen Sie Snap-In hinzufügen/entfernen.
    4. Wählen Sie Zertifikate, und wählen Sie hinzufügen. Stellen Sie sicher, wählen Sie das Computerkonto für das Zertifikat-Snap-in und klicken Sie auf "Fertig stellen", und klicken Sie auf OK.
    5. Öffnen Sie den Personal\Certificate-Speicher.
    6. Klicken Sie mit der rechten Maustaste auf Zertifikate, und wählen Sie alle Tasks, und wählen Sie importieren.
    7. Wählen Sie das Zertifikat, das Sie aus dem UDC heruntergeladen.
    8. Der rechten Maustaste auf das Zertifikat in der Benutzeroberfläche, nachdem er importiert wurde, und wählen Sie alle Tasks, und wählen Sie exportieren.
    9. Führen Sie den Assistenten zum Exportieren, und achten Sie darauf, dass Sie auswählen, um den privaten Schlüssel mit dem Zertifikat exportieren.
    10. Beenden Sie den Export-Assistenten.