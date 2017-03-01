---
author: jnHs
Description: "Beim Einreichen der App haben Sie die Möglichkeit, auf der Seite Hinweise für Zertifizierung zusätzliche Informationen für Zertifizierungstester bereitzustellen. Mit diesen Informationen kann sichergestellt werden, dass die App richtig getestet wird."
title: "Hinweise für Zertifizierung"
ms.assetid: 4A740A5F-F39F-4FE2-9391-EE00DB46B25A
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 92a110879f6ff92b3e1e41964ff6acea6b7cb5ec
ms.lasthandoff: 02/07/2017

---

# <a name="notes-for-certification"></a>Hinweise für Zertifizierung


Beim Einreichen der App haben Sie die Möglichkeit, auf der Seite **Hinweise für Zertifizierung** zusätzliche Informationen für Zertifizierungstester bereitzustellen. Mit diesen Informationen kann sichergestellt werden, dass die App richtig getestet wird.

Achten Sie darauf, folgende Informationen anzugeben (falls sie für Ihre App relevant sind):

-   **Benutzernamen und Kennwörter für Testkonten**

    Falls Ihre App die Anmeldung bei einem Dienst erfordert, stellen Sie den Benutzernamen und das Kennwort für ein Testkonto bereit. Die Zertifizierungstester verwenden dieses Konto beim Prüfen Ihrer App.

-   **Schritte für den Zugriff auf versteckte oder gesperrte Features**

    Falls Ihre App Features enthält, die für Tester nicht offensichtlich sind, beschreiben Sie kurz, wie sie auf diese Features zugreifen können. Apps, die scheinbar unvollständig sind, werden möglicherweise nicht zertifiziert.

-   **Schritte zur Überprüfung der Nutzung von Audiodateien im Hintergrund**

    Wenn die App das Ausführen von Audiodateien im Hintergrund zulässt, benötigen Tester u. U. eine Anleitung für den Zugriff auf dieses Feature, damit sie die ordnungsgemäße Funktionsweise sicherstellen können.

-   **Informationen zu Änderungen in einem App-Update**

    Informieren Sie Tester über Änderungen, die Sie an zuvor veröffentlichten Apps vorgenommen haben. Dies gilt insbesondere, wenn Ihre Pakete unverändert bleiben und Sie nur Änderungen am App-Eintrag vornehmen (beispielsweise weitere Screenshots hinzufügen, die App-Kategorie ändern oder die Beschreibung bearbeiten).

Berücksichtigen Sie beim Zusammenstellen der Informationen Folgendes:

-   **Diese Hinweise kann von realen Benutzern gelesen.** Verwenden Sie daher möglichst freundliche Formulierungen und klare, hilfreiche Anweisungen.
-   **Formulieren Sie knapp und verständlich.** Sollten weitere Erläuterungen erforderlich sein, können Sie einen Link zu einer Seite mit weiteren Informationen angeben. Denken Sie daran, dass Ihre App-Kunden diese Hinweise nicht sehen können. Wenn Sie komplizierte Anweisungen zum Testen Ihrer App benötigen, überlegen Sie, ob die App vereinfacht werden kann, damit Ihre Kunden wissen, wie sie zu verwenden ist.
-   **Dienste und externe Komponenten müssen online und verfügbar sein.** Falls Ihre App im Rahmen ihrer Funktion eine Verbindung mit einem Dienst herstellt, muss der Dienst online und verfügbar sein. Stellen Sie alle erforderlichen Informationen zum entsprechenden Dienst für Tester bereit. Falls Ihre App während des Tests nicht auf einen erforderlichen Dienst zugreifen kann, ist die Zertifizierung u. U. nicht erfolgreich.

 

 





