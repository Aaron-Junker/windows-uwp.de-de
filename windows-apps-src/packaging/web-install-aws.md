---
title: Hosten von UWP-App-Paketen auf AWS für die Webinstallation
description: Tutorial zum Einrichten von AWS-Webserver zum Überprüfen des app-Installation über App-Installer-App
ms.date: 05/30/2018
ms.topic: article
keywords: Windows 10, Windows 10, UWP, app-Installer AppInstaller, per sideload übertragen, im Zusammenhang festgelegt ist, optionale Pakete, AWS
ms.localizationpriority: medium
ms.openlocfilehash: 53fe01a1c1a825377e886e042b4eef3868cbf5eb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628055"
---
# <a name="hosting-uwp-app-packages-on-aws-for-web-install"></a>Hosten von UWP-App-Paketen auf AWS für die Webinstallation

Mit der App-Installer-App können Entwickler und IT-Spezialisten Windows 10-Apps verteilen, indem sie diese in ihrem eigenen Content Delivery Network (CDN) hosten. Das ist nützlich für Unternehmen, die ihre Apps nicht im Microsoft Store veröffentlichen möchten oder müssen, aber weiterhin die Windows 10-Verpackungs- und -Bereitstellungsplattform nutzen möchten.

Dieses Thema beschreibt die Schritte zum Konfigurieren einer Website Amazon Web Services (AWS) zum Hosten von Paketen UWP-app, und wie Sie die App-Installer-app verwenden, um die app-Pakete zu installieren.

## <a name="setup"></a>Setup

Für eine erfolgreiche Durchführung dieses Lernprogramms benötigen Sie Folgendes:
 
1. AWS-Abonnement 
2. Webseite
3. UWP-App-Paket – das App-Paket, die Sie verteilen möchten

Optional: [Startprojekt](https://github.com/AppInstaller/MySampleWebApp) auf GitHub. Dies ist hilfreich, wenn Sie kein App-Paket oder keine Webseite zum Arbeiten verfügbar haben, aber dennoch lernen möchten, wie Sie dieses Feature verwenden.

Gewusst wie: Einrichten einer Webseite und Hosten von Paketen in AWS werden in diesem Lernprogramm besprochen. Dies erfordert ein AWS-Abonnement. Abhängig vom Umfang des Vorgangs können Sie ihre kostenlose Mitgliedschaft zum Durcharbeiten dieses Lernprogramms verwenden. 

## <a name="step-1---aws-membership"></a>Schritt 1: Mitgliedschaft in AWS
Um ein AWS-Mitgliedschaft zu erhalten, besuchen Sie die [AWS-Konto – Seite "Details"](https://aws.amazon.com/free/). Für die Zwecke dieses Lernprogramms können Sie eine kostenlose Mitgliedschaft verwenden.

## <a name="step-2---create-an-amazon-s3-bucket"></a>Schritt 2: Erstellen einer Amazon S3-bucket

Amazon Simple Storage Service (S3) ist ein Angebot für das Sammeln, speichern und Analysieren von Daten AWS. S3-Buckets handelt es sich um eine praktische Möglichkeit zum Host UWP-app-Pakete und Webseiten für die Verteilung. 

Nach der Anmeldung AWS mit Ihren Anmeldeinformationen, unter `Services` finden `S3`. 

Wählen Sie **erstellen Bucket**, und geben Sie einen **bucketname** für Ihre Website. Führen Sie die Dialogfeld-Anweisungen zum Festlegen von Eigenschaften und Berechtigungen. Um sicherzustellen, dass die UWP-app auf Ihrer Website verteilt werden kann, aktivieren **lesen** und **schreiben** Berechtigungen für Ihr Bucket, und wählen **erteilen öffentlichen Lesezugriff auf diesem Bucket** .

![Festlegen von Berechtigungen für Amazon S3-bucket](images/aws-permissions.png) 

Überprüfen Sie die Zusammenfassung, um sicherzustellen, dass die ausgewählten Optionen wiedergegeben werden. Klicken Sie auf **erstellen Bucket** zum Abschluss dieses Schritts. 

## <a name="step-3---upload-uwp-app-package-and-web-pages-to-an-s3-bucket"></a>Schritt 3: Hochladen von UWP-app-Paket und Webseiten in einem S3-Bucket

Eine haben Sie eine Amazon S3-Bucket erstellt, Sie in der Amazon S3-Ansicht angezeigt werden. Hier ist ein Beispiel, wie unsere Demo-Bucket aussieht:

![Amazon S3-Bucket-Ansicht](images/aws-post-create.png)

Wir sind jetzt bereit zum Hochladen der app-Pakete und die Webseiten, die wir in unserer Amazon S3-Bucket hosten möchten. 

Klicken Sie auf den neu erstellten Bucket zum Hochladen von Inhalten. Der Bucket ist zurzeit leer, da nichts noch hochgeladen wurde. Klicken Sie auf die **hochladen** Schaltfläche, und wählen Sie die app-Pakete und die Webseite-Dateien, die Sie hochladen möchten.

> [!NOTE]
> Sie können das App-Paket verwenden, das Teil des bereitgestellten [Startprojekt](https://github.com/AppInstaller/MySampleWebApp)-Repositorys auf GitHub ist, falls Ihnen kein App-Paket zur Verfügung steht. Die Zertifikat (MySampleApp.cer), mit dem das Paket signiert wurde, ist ebenfalls im Beispiel auf GitHub enthalten. Sie müssen das Zertifikat vor der Installation der App auf Ihrem Gerät installieren.

![app-Paket hochladen](images/aws-upload-package.png)

Ähnlich wie die Berechtigungen zum Erstellen einer Amazon S3-Bucket, der Inhalt in den Bucket auch müssen **lesen**, **schreiben**, und **erteilen öffentlichen Lesezugriff auf diese Objekte** Berechtigungen.

Wenn Sie des Hochladens von einer Webseite testen möchten, aber nicht vorhanden, können Sie die Beispiel-html-Seite (default.html) verwenden, aus der [Startprojekt](https://github.com/AppInstaller/MySampleWebApp/blob/master/MySampleWebApp/default.html).

> [!IMPORTANT]
> Bevor Sie die Webseite hochgeladen haben, vergewissern Sie sich, dass die app-Paketverweis auf Ihrer Webseite richtig ist. 

Rufen Sie die Referenz zur app-Paket hochladen Sie das app-Paket zuerst aus, und kopieren Sie die URL der app-Pakets. Bearbeiten Sie die html-Webseite entsprechend den Pfad der richtigen app-Pakets. Finden Sie im Codebeispiel für weitere Details. 

Wählen Sie die hochgeladene app-Paketdatei, um den Verweislink, um das app-Paket zu erhalten, sie sollte etwa wie folgt:

![Pfad für hochgeladene Paket](images/aws-package-path.png)

**Kopie** den Link, um die app-Paket, und fügen Sie den Verweis auf Ihrer Webseite. 

```html
<html>
    <head>
        <meta charset="utf-8" />
        <title> Install My Sample App</title>
    </head>
    <body>
        <a href="ms-appinstaller:?source=https://s3-us-west-2.amazonaws.com/appinstaller-aws-demo/MySampleApp.appxbundle"> Install My Sample App</a>
    </body>
</html>
```
Laden Sie die html-Datei in Ihrer Amazon S3-Bucket hoch. Denken Sie daran, legen Sie die Berechtigungen zum Zulassen **lesen** und **schreiben** Zugriff.

## <a name="step-4---test"></a>Schritt 4: Testen

Sobald die Webseite in Ihrer Amazon S3-Bucket hochgeladen wurde, rufen Sie den Link zu der Webseite, dazu die hochgeladene html-Datei.

Verwenden Sie den Link, um die Webseite zu öffnen. Da wir die Berechtigungen zum Erteilen öffentlichen Zugriffs auf die app-Paket und die Webseite festlegen, wird jeder Benutzer mit dem Link zur Webseite in der Lage, darauf zuzugreifen und Ihre UWP-app-Pakete mithilfe von App-Installer installieren. Beachten Sie, dass App-Installer Teil der Windows 10-Plattform. Als Entwickler müssen Sie nicht zum Hinzufügen von zusätzlichen Code oder Funktionen zu Ihrer app um die Verwendung von App-Installer zu aktivieren. 

## <a name="troubleshooting"></a>Problembehandlung

### <a name="app-installer-fails-to-install"></a>App-Installer nicht installiert 

App-Installation schlägt fehl, wenn das Zertifikat, dem das app-Paket mit signiert ist nicht auf dem Gerät installiert ist. Um dieses Problem zu beheben, müssen Sie das Zertifikat vor der Installation der App installieren. Wenn Sie ein app-Paket zur öffentlichen Verteilung hosten, hat es empfohlen, zum Signieren des app-Pakets mit einem Zertifikat von einer Zertifizierungsstelle. 


