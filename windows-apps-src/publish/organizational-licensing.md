---
Description: Sie können angeben, ob und wie Ihre App für Volume-Käufe über den Windows Store für Unternehmen und Microsoft Store für Bildungseinrichtungen angeboten wird, indem Sie die Einstellungen auf der Seite für Verfügbarkeit und Preise einer App-Übermittlung im Abschnitt Unternehmenslizenzierung festlegen.
title: Organisatorische Lizenzierungsoptionen
ms.assetid: 1EB139B0-67E7-4F66-AAEF-491B1E52E96F
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Store für Unternehmen, Store für Bildungseinrichtungen, Organisationseinheit, Volumenlizenzierung, Enterprise, Education Store, Business Store, Volumeneinkäufe, Massen
localizationpriority: high
ms.openlocfilehash: b7f0ecc1eb7faec39eaad925ccd6c9d501d39749
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/24/2019
ms.locfileid: "63788397"
---
# <a name="organizational-licensing-options"></a>Organisatorische Lizenzierungsoptionen


Sie können angeben, ob und wie Ihre App für Volume-Käufe über den Windows Store für Unternehmen und den Windows Store für Bildungseinrichtungen angeboten wird, indem Sie die Einstellungen auf der Seite [Verfügbarkeit und Preise](set-app-pricing-and-availability.md#organizational-licensing) einer App-Übermittlung im Abschnitt **Unternehmenslizenzierung** festlegen.

Durch diese Einstellungen können Sie damit Ihre app verfügbar gemacht werden kann ablehnen, Organisationen (Business- und Bildungseinrichtungen) aus, für die erwerben und Bereitstellen mehrere Lizenzen für ihre Benutzer eine Chance, Ihre Reichweite zu Organisationen auf Windows 10 zu erhöhen. Gerät und Typen, einschließlich PCs, Tablets und Smartphones.

Sie müssen zudem Organisationslizenzierung für jede [Branchen-Apps](distribute-lob-apps-to-enterprises.md) zulassen, die Sie direkt für Unternehmen veröffentlichen.

> [!NOTE]
> Die Auswahlmöglichkeiten für Ihre Apps werden unabhängig voneinander konfiguriert. Sie können Ihre Einstellungen für eine App jederzeit ändern, indem Sie eine neue Übermittlung erstellen, und die Änderungen werden wirksam, nachdem die Übermittlung den [Zertifizierungsprozess](the-app-certification-process.md) abgeschlossen hat.

> [!IMPORTANT]
> Übermittlungen, die die [Microsoft Store-Übermittlungs-API](../monetize/create-and-manage-submissions-using-windows-store-services.md) verwenden, werden nicht an Microsoft Store für Unternehmen und Microsoft Store für Bildungseinrichtungen zur Verfügung gestellt. Um Ihre app, die Organisationen für Volumenkäufe verfügbar zu machen, müssen Sie erstellen und übermitteln Ihre einsendungen im Partner Center.


## <a name="allowing-your-app-to-be-offered-to-organizations"></a>Anbieten Ihrer App an Organisationen

Standardmäßig ist das Kontrollkästchen **Meine App für Organisationen mit Store-verwalteter (online) Volumenlizenzierung und Verteilung verfügbar machen** aktiviert. Das bedeutet, Sie möchten Ihre App für die Aufnahme in App-Katalogen verfügbar machen, die Organisationen für den Erwerb von Volumenlizenzen zur Verfügung gestellt werden. Die Lizenzen sollen dabei über das Onlinelizenzierungssystem des Store verwaltet werden.

> [!NOTE]
> Dies garantiert nicht, dass Ihre App für alle Organisationen verfügbar gemacht wird.

Wenn Sie Ihre App Organisationen nicht für den Erwerb von Volumenlizenzen anbieten möchten, deaktivieren Sie das Kontrollkästchen. Beachten Sie, dass diese Änderung erst erfolgt, nachdem die App den Zertifizierungsprozess abgeschlossen hat. Wenn Unternehmen bereits zuvor Lizenzen für Ihre App erworben haben, bleiben die Lizenzen weiterhin gültig, und Personen, die die App bereits besitzen, können sie weiterhin verwenden.

> [!TIP]
> Wenn Sie Branchen-Apps exklusiv für eine konkrete Organisation veröffentlichen möchten, können Sie eine Unternehmenszuordnung einrichten und der Organisation das direkte Hinzufügen der Apps zum privaten Speicher gestatten. Weitere Informationen finden Sie unter [Verteilen von Branchen-Apps an Unternehmen](distribute-lob-apps-to-enterprises.md).


## <a name="allowing-disconnected-offline-licensing"></a>Zulassen der getrennten (Offline-) Lizenzierung

Viele Unternehmen benötigen Apps, die offline lizenziert werden können. Einige Unternehmen müssen beispielsweise Apps auf Geräten bereitstellen, die nur selten oder nie mit dem Internet verbunden sind. Wenn Sie Ihre App für diese Kunden verfügbar machen möchten, aktivieren Sie das Kontrollkästchen **Organisationsverwaltete (offline) Lizenzierung und Verteilung von Organisationen zulassen**.

Standardmäßig ist das Kontrollkästchen **deaktiviert**. Aktivieren Sie das Kontrollkästchen, damit wir Ihre App für geprüfte Unternehmen verfügbar machen können, die sie mithilfe der organisationsverwalteten (offline) Lizenzierung installieren. Organisationen müssen eine zusätzliche Überprüfung durchlaufen, um kostenpflichtige Apps bei ihren Kunden auf diese Weise zu installieren.

Über die Offlinelizenzierung können Unternehmen Ihre App auf Volumenbasis erwerben, und anschließend auf den Geräten installieren, ohne auf das Lizenzierungssystem des Store zugreifen zu müssen. Die Organisation kann Ihr App-Paket zusammen mit einer Lizenz herunterladen, über die sie auf Geräten installiert werden kann (über ihre eigenen Verwaltungstools oder durch das Vorabladen von Apps auf Betriebssystem-Images), ohne eine Benachrichtigung an den Store senden zu müssen, wenn eine bestimmte Lizenz verwendet wurde. Durch Aktivieren dieses Szenario wird die Flexibilität bei der Bereitstellung drastisch erhöht und damit möglicherweise auch die Attraktivität Ihrer App bei diesen Kunden erheblich gesteigert.

> [!IMPORTANT]
> Die Offlinelizenzierung wird für XAP-Pakete nicht unterstützt.

 
## <a name="paid-app-support"></a>Unterstützung für kostenpflichtige Apps

Zurzeit können Entwicklerkonten in bestimmten Märkten Volumenkäufe kostenpflichtiger Apps im Microsoft Store für Unternehmen anbieten. 

> [!NOTE]
> In einigen Märkten kann sich der Preis einer App im Microsoft Store für Unternehmen oder im Microsoft Store für Bildungseinrichtungen von dem Preis unterscheiden, der Einzelhandelskunden im Microsoft Store für das gleiche Preisniveau angezeigt wird. Die Auszahlung der Erlöse aus Unternehmenskäufen funktioniert genau wie die Auszahlung von Erlösen aus Verbraucherkäufen Ihrer App. Weitere Informationen finden Sie unter [Bezahlung](getting-paid-apps.md) und [Vereinbarung für App-Entwickler](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement). Eine Liste der Märkte, in denen Microsoft Store für Unternehmen und Microsoft Store für Bildungseinrichtungen verfügbar sind, finden Sie unter [Microsoft Store für Unternehmen und Microsoft Store für Bildungseinrichtungen – Übersicht](https://technet.microsoft.com/itpro/windows/manage/windows-store-for-business-overview#supported-markets).

Wenn Ihr Land oder Ihre Region unten nicht aufgeführt ist, werden Ihre kostenpflichtigen Apps zurzeit nicht im Microsoft Store für Unternehmen und Microsoft Store für Bildungseinrichtungen angeboten werden. Wenn dies der Fall ist, werden die Lizenzierungsoptionen für Unternehmen, die Sie für Ihre kostenpflichtigen Apps einrichten, möglicherweise zu einem späteren Zeitpunkt angewendet, da wir später Unterstützung für die Übermittlung aus zusätzlichen Entwicklerkontenmärkten hinzufügen.

Zu diesem Zeitpunkt können Entwickler in den folgenden Ländern und Regionen über den Microsoft Store für Unternehmen und den Windows Store für Bildungseinrichtungen kostenpflichtige Apps an Unternehmenskunden verteilen.

- Österreich
- Belgien
- Bulgarien
- Kanada
- Kroatien
- Zypern
- Tschechische Republik
- Dänemark
- Estland
- Finnland
- Frankreich
- Deutschland
- Griechenland
- Ungarn
- Irland
- Isle of Man
- Italien
- Lettland
- Liechtenstein
- Litauen
- Luxemburg
- Malta
- Monaco
- Niederlande
- Norwegen
- Polen
- Portugal
- Rumänien
- Slowakei
- Slowenien
- Spanien
- Schweden
- Schweiz
- Vereinigtes Königreich
- USA
