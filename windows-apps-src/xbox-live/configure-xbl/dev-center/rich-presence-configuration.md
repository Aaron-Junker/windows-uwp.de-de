---
title: Umfassender vorhanden Konfiguration im Partner Center
description: Informationen Sie zum Konfigurieren von Rich-Anwesenheit Zeichenfolgen im Partner Center
ms.date: 02/27/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, umfassende Anwesenheitsfunktionen Zeichenfolgen, Partner Center
ms.openlocfilehash: 5c2add99c6fc6ad1c7f085eb35dc4fdba9e0688d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57624125"
---
# <a name="configure-rich-presence-in-partner-center"></a>Konfigurieren von Rich-Präsenz im Partner Center

Vorhandensein von Rich-Zeichenfolgen Anzeigen eines Benutzers in-Game-Aktivität. Angezeigt des Spielers Gamertag, in der **Freunde & Kreuz** Liste auch, wie ihre Xbox Live-Benutzerprofil. Konfigurierte umfassender vorhanden-Zeichenfolgen werden an den Namen des Spiels wiedergegeben wird angefügt. Wenn Sie ein Spiel namens BubblePop erstellen und konfigurieren die Anwesenheit von Rich-Zeichenfolge "Popping Blasen mit Freunden", wird Ihre konfigurierten Zeichenfolge "BubblePop - Popping Blasen mit Freunden" als Status erstellt. Im folgenden sehen Sie, wie eine umfangreiche Anwesenheit-Zeichenfolge im Kontext angezeigt wird.

Im folgenden Screenshot Xbox Live-Benutzer **letzten Roar** und **Lucha Uno** mit umfassender vorhanden Zeichenfolgen spielen.

![Beispiel für die Liste von Freunden](../../images/rich_presence/RichPresence_FriendsList_Screen.jpg)

Im folgenden Screenshot sehen Sie **Lucha Unos** vollständige umfassender vorhanden-Zeichenfolge im Profil.

![Profil-Beispiel](../../images/rich_presence/RichPresence_Config_ProfileScreen.jpg)

> [!IMPORTANT]
> Vorhandensein von Rich-Zeichenfolgen sind nicht verfügbar, Xbox Live Creators-Programm Titel und aus diesem Grund sind nicht konfigurierbar, für diesen Titel. Der Inhalt in diesem Artikel ist für ID@Xbox und verwaltete Partnerkonten Titel.

## <a name="requirements"></a>Anforderungen

Vor dem Konfigurieren von Rich-Anwesenheit Zeichenfolgen müssen Sie und Ihre Titel der folgenden Kriterien erfüllen:

- Sie müssen ein Windows Development-Konto verfügen.
- Ihr entwicklungskonto muss registriert werden, der ID@Xbox Programm oder als ein Entwicklerkonto für den verwalteten Partner.
- Titel des muss im Partner Center registriert werden und werden von Xbox Live aktiviert.

Bevor Sie umfangreiche Anwesenheit Zeichenfolgen verwenden können, müssen Sie sie im Partner Center konfigurieren.

## <a name="rich-presence-configuration-page"></a>Seite "Konfiguration" der Rich-Präsenz

Vorhandensein von Rich-Zeichenfolgen werden als Teil des Xbox Live-Dienst für Ihre Titel im konfiguriert [Partner Center](https://partner.microsoft.com/dashboard).

Navigieren Sie auf der Konfigurationsseite von umfassender vorhanden mit den folgenden Anweisungen:

1. Wechseln Sie zu [Partner Center](https://partner.microsoft.com/dashboard) auf developer.microsoft.com.
2. Melden Sie sich mit Ihrem registrierten Windows-Entwicklerkonto, wenn Anmeldung angefordert wird.
3. Wählen Sie das Xbox Live aktiviert Titel oder die App aus dem **Übersicht** Seite. Wählen Sie einen Titel Creators-Programm nicht, wie es für Rich-Präsenz-Konfiguration nicht aktiviert wird.
4. Klicken Sie auf die **Services** Dropdownliste aus, und wählen Sie die Xbox Live.
5. Führen Sie einen Bildlauf nach unten, um die **umfassender vorhanden** verknüpfen, und klicken Sie darauf.

Die Anwesenheit von Rich-Seite zeigt eine kurze Beschreibung des Diensts eine Schaltfläche, um eine neue Zeichenfolge mit umfassender vorhanden und eine durchsuchbare Liste mit den zuvor konfigurierten Zeichenfolgen zu erstellen. Auf dieser Seite können Sie konfigurieren die neue Zeichenfolgen als auch bearbeiten und überprüfen Sie Ihre konfigurierten Zeichenfolgen.

![Rich-Anwesenheit Beispielzeichenfolge Config Page](../../images/rich_presence/RichPresence_ConfigPage_New.JPG)

> [!NOTE]
> Zeichenfolgen wie "Playing Net Runner - Multiplayer-Deathmatch Mond-Basis mit 10 Verlauf." wird nicht für Entwickler, die seit dem Update Data Platform 2017 verfügbar sein. Datenplattform 2013 *Variablen* sind in Data Platform 2017 nicht verfügbar. Die Variable ist in diesem Fall, dass die Anzahl der "10" beendet. Die entsprechende Zeichenfolge nach dem das Data-Plattform-2017-Update "Playing Net Runner - Multiplayer-Deathmatch Mond als Grundlage." wäre "Während der Wiedergabe Net Runner – In den Menüs" ist immer noch eine gültige Zeichenfolge für die umfassende vorhanden.

## <a name="create-a-new-rich-presence-string"></a>Erstellen Sie eine neue Zeichenfolge mit umfassender vorhanden

Um eine umfassende Anwesenheitsfunktionen-Zeichenfolge zu erstellen, klicken Sie auf die Schaltfläche **neuen Rich Presence-Zeichenfolge**. Angezeigt mit Benutzeroberfläche, geben Sie die **Anwesenheit Details** enthalten die **Eindeutigeid der umfassender vorhanden** als auch die **Anzeigezeichenfolge** für Ihre neue Zeichenfolge mit umfassender vorhanden.

![neue umfassende Anwesenheitsfunktionen Zeichenfolge UI](../../images/rich_presence/RichPresence_Config_NewString.JPG)

**Eindeutige ID der Rich-Anwesenheit** – die eindeutige ID für die Rich-Anwesenheit ist eine Zeichenfolge, die zum Identifizieren der Zeichenfolge umfassender vorhanden. Diese Zeichenfolge verwendet werden, um den Status der Spieler für Ihr Spiel festzulegen und bezieht sich auf die bestimmte Zeichenfolge, die Sie anzeigen möchten. Die ID kann maximal 50 Zeichen sein.

**Anzeigezeichenfolge** -Zeichenfolge die Anzeige ist die Zeichenfolge, die anzuzeigende wird angefügt, um den Status einige Spieler wiedergeben Ihres Spiels. Dies ist, in dem Sie eingeben, werden in der Zeichenfolge umfassender vorhanden, angezeigt werden sollen Interesse an Ihr Spiel zu generieren. Anzeige kann maximal 100 Zeichen sein, jedoch werden Instanzen, in dem nur die ersten 40 Zeichen angezeigt wird.

Um Ihre neue umfassende Anwesenheitsfunktionen Zeichenfolge zu erstellen, geben Sie sowohl Felder und drücken Sie die **speichern** Schaltfläche.
Nachdem Sie auf Speichern, die Sie wieder auf der Konfigurationsseite von umfassender vorhanden ausgeführt werden klicken, in dem Sie Ihre neue umfassender vorhanden-Zeichenfolge, die hinzugefügt werden, um die Liste der konfigurierten Zeichenfolgen sehen.

## <a name="review-edit-and-delete-strings"></a>Überprüfen Sie, bearbeiten und Löschen von Zeichenfolgen

Hier sehen Sie eine umfassende Anwesenheitsfunktionen-Konfigurationsseite mit ein paar konfigurierten Zeichenfolgen.
![Umfangreiche Anwesenheit Seite so konfiguriert, Beispiel](../../images/rich_presence/RichPresence_ConfigPage_Configured.JPG)

Navigieren Sie zum zuvor erstellte Zeichenfolgen zu überprüfen, einfach die Liste auf der Konfigurationsseite von umfassender vorhanden. Es können Sie sowohl die Eindeutigeid der umfassender vorhanden, und die Anzeigezeichenfolge zusammen angezeigt. Dies ist nützlich sein, wenn Sie die eindeutige Id des Rich-Präsenz im Titel des Code zu verwenden, zum Angeben von umfassender vorhanden müssen.

So bearbeiten Sie eine umfassende Präsenz Zeichenfolge klicken Sie einfach auf die **Eindeutigeid der Rich-Anwesenheit** Link, um die Zeichenfolge, die Sie bearbeiten möchten. Sie gelangen auf die gleiche UI verwendet, um eine neue umfassende Anwesenheitsfunktionen-Zeichenfolge mit der aktuellen Zeichenfolge-Einstellungen für die Bearbeitung ausgefüllt erstellt werden. Nach dem vornehmen von Änderungen klicken Sie auf die **speichern** Schaltfläche, um die konfigurierten Zeichenfolge mit Ihren Änderungen zu aktualisieren.

So löschen Sie einen konfigurierten umfassender vorhanden Zeichenfolge auf die **löschen** Link auf der Konfigurationsseite von umfassender vorhanden in derselben Zeile wie die Präsenz von Rich-Zeichenfolge, die Sie löschen möchten. Sie werden aufgefordert zu bestätigen.

## <a name="further-reading"></a>Weiterführende Themen

Um ausführliche konzeptionelle Informationen zu zeichenfolgenfeatures umfassender vorhanden und ihre Implementierung zu erhalten, lesen die [umfassender vorhanden Dokumentation](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/social-platform/rich-presence-strings/rich-presence-strings-overview).