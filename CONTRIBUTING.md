---
ms.openlocfilehash: a91c080805bca5d536aad3755ca7edf052d1fe0e
ms.sourcegitcommit: b8087f8b6cf8367f8adb7d6db4581d9aa47b4861
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2019
ms.locfileid: "67414060"
---
# <a name="contributing-to-uwp-conceptual-documentation"></a>Beitragen zur UWP-Konzeptdokumentation

Vielen Dank für dein Interesse an der Dokumentation zur universellen Windows-Plattform (UWP)! Wir freuen uns über dein Feedback sowie über deine Änderungen und Ergänzungen für unsere Dokumentation.

## <a name="writing-content"></a>Erstellen von Inhalten

Unsere Dokumentation ist in Markdown (einer einfachen Textstilsyntax) geschrieben. Auf [GitHub](https://guides.github.com/features/mastering-markdown/) kannst du dich bei Bedarf mit den Grundlagen von Markdown vertraut machen. Solltest du unsicher sein, kannst du jederzeit die Formatierung von anderen Seiten in unserer Dokumentation kopieren.

## <a name="public-contributions"></a>Öffentliche Beiträge

Wenn du **kein** Microsoft-Mitarbeiter bist, kannst du für deine Beiträge das [öffentliche Inhaltsrepository](https://github.com/MicrosoftDocs/windows-uwp) verwenden. Öffentliche Beiträge können zur Änderung oder Präzisierung bereits vorhandener Seiten verwendet werden.

### <a name="editing-a-file"></a>Bearbeiten einer Datei

Wenn du dich bereits im öffentlichen Inhaltsrepository befindest, navigiere zunächst zu der Datei, die du ändern möchtest. Wähle dort über dem angezeigten Inhalt das Stiftsymbol aus, um mit der Bearbeitung zu beginnen.

Falls du gerade eine Seite von „docs.microsoft.com“ betrachtest, kannst du alternativ auch rechts oben auf der Seite auf die Schaltfläche **Bearbeiten** klicken. Dadurch gelangst du zur entsprechenden Quelldatei im Repository.

Wenn du mit der Bearbeitung beginnst, forkt GitHub das offizielle Repository automatisch in dein persönliches GitHub-Konto, in dem du dann die gewünschten Änderungen vornehmen kannst. Anschließend musst du einen Pull Request an den Branch **docs** übermitteln.

### <a name="pull-requests"></a>Pull-Anforderungen

Nach Übermittlung deines Pull Requests wird anhand einer Prüfliste überprüft, ob die Qualität des Inhalts unseren grundlegenden Standards entspricht. Ist die Überprüfung erfolgreich, wird der Request zur weiteren Überprüfung einem Mitglied des UWP-Dokumentationsteams zugewiesen. Andernfalls erfährst du, was geändert werden muss.

Die zugewiesenen Prüfer können deinen Pull Request genehmigen, ablehnen oder mit dir zusammenarbeiten, um weitere Änderungen vorzunehmen.

## <a name="internal-contributions"></a>Interne Beiträge

Microsoft-Mitarbeiter können für ihre Beiträge das [private Inhaltsrepository](https://github.com/microsoftdocs/windows-uwp-pr) verwenden. Informationen zur Verwendung dieses Repositorys findest du im [Erstellungshandbuch für Windows](https://review.docs.microsoft.com/windows-authoring-guide/uwp/?branch=master). Dokumentationen für zukünftige Features dürfen ausschließlich über das private Repository beigesteuert werden.

### <a name="editing-a-file"></a>Bearbeiten einer Datei

Kleinere Änderungen am privaten Repository können genau wie beim öffentlichen Repository über den Browser vorgenommen werden, ohne einen lokalen Klon erstellen zu müssen. Vergewissere dich **unbedingt**, dass du dich im korrekten Branch befindest. Weitere Informationen zur Erstellung eines eigenen Branchs findest du im [Erstellungshandbuch für Windows](https://review.docs.microsoft.com/windows-authoring-guide/uwp/conceptual/branches?branch=master).

### <a name="making-substantial-changes"></a>Vornehmen erheblicher Änderungen

Wenn du umfangreichere Änderungen an einem bereits vorhandenen Artikel vornehmen, Bilder hinzufügen/ändern oder einen neuen Artikel beisteuern möchtest, musst du einen lokalen Klon unseres privaten Inhaltsrepositorys erstellen. Weitere Informationen findest du im [Erstellungshandbuch für Windows](https://review.docs.microsoft.com/windows-authoring-guide/uwp/conceptual/).

### <a name="pull-requests"></a>Pull-Anforderungen

Achte beim Erstellen eines Pull Requests im internen Repository darauf, dass du deinen persönlichen Branch mit dem Branch zusammenführst, auf dessen Grundlage er erstellt wurde.

Nach Übermittlung des Pull Requests wird anhand einer [Pull-Request-Zusammenführung](https://review.docs.microsoft.com/help/contribute/prmerger-overview?branch=master) überprüft, ob er unseren grundlegenden Standards entspricht. Ist die Überprüfung erfolgreich, können Sie den Kommentar `#sign-off` hinzufügen, um den Pull Request an ein Mitglied des UWP-Dokumentationsteams zu übergeben. Andernfalls erfahren Sie, was geändert werden muss, bevor Sie ihn abzeichnen können.

Die zugewiesenen Prüfer können deinen Pull Request genehmigen, ablehnen oder mit dir zusammenarbeiten, um weitere Änderungen vorzunehmen. Prüfer führen den Pull Request erst zusammen, wenn er von dir selbst genehmigt wurde.

## <a name="using-issues-to-provide-feedback-on-uwp-conceptual-documentation"></a>Verwenden von Problemen zum Abgeben von Feedback zur UWP-Konzeptdokumentation

Wenn du Feedback zur Dokumentation abgeben möchtest, anstatt sie selbst zu bearbeiten, kannst du [ein Problem im öffentlichen Repository erstellen](https://github.com/MicrosoftDocs/windows-uwp/issues). Wähle die Registerkarte **Issues** (Probleme) und anschließend die Schaltfläche **New issue** (Neues Problem) aus. Vergiss nicht, den Titel des Themas und die URL für die Seite anzugeben. Dein Problem wird Mitgliedern des UWP-Dokumentationsteams zur Überprüfung zugewiesen.

* Für interne Probleme steht das [WDG-Inhaltsanforderungstool](https://aka.ms/pubrequest) zur Verfügung.
