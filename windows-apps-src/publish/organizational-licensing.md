---
description: Im Abschnitt „Unternehmenslizenzierung“ einer App-Übermittlung kannst du angeben, ob und wie deine App für Volumenkäufe über den Microsoft Store für Unternehmen und den Microsoft Store für Bildungseinrichtungen angeboten werden kann.
title: Organisatorische Lizenzierungsoptionen
ms.assetid: 1EB139B0-67E7-4F66-AAEF-491B1E52E96F
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Store für Unternehmen, Store für Bildungseinrichtungen, Unternehmenslizenzierung, Volumenlizenzierung, Unternehmen, Volumenkauf, Großauftrag
localizationpriority: high
ms.openlocfilehash: dbaf44409e387d15225e701ef5c64fc2cb198ece
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034623"
---
# <a name="organizational-licensing-options"></a>Organisatorische Lizenzierungsoptionen


Im Abschnitt **Unternehmenslizenzierung** auf der Seite [Preise und Verfügbarkeit](set-app-pricing-and-availability.md#organizational-licensing) einer App-Übermittlung kannst du angeben, ob und wie deine App für Volumenkäufe über den Microsoft Store für Unternehmen und den Microsoft Store für Bildungseinrichtungen angeboten werden kann.

Mithilfe dieser Einstellung kannst du zulassen, dass deine App für Organisationen (Unternehmen und Bildungseinrichtungen) verfügbar gemacht wird, die mehrere Lizenzen für ihre Benutzer erwerben und bereitstellen. So kannst du deine Reichweite auf Organisationen mit verschiedenen Windows 10-Gerätetypen (einschließlich PCs, Tablets und Smartphones) ausdehnen.

Darüber hinaus musst du die Unternehmenslizenzierung für jegliche [Branchen-Apps](distribute-lob-apps-to-enterprises.md) zulassen, die du direkt für Unternehmen veröffentlichst.

> [!NOTE]
> Die Optionen für deine Apps werden jeweils unabhängig konfiguriert. Sie können Ihre Einstellungen für eine App jederzeit ändern, indem Sie eine neue Übermittlung erstellen, und die Änderungen werden wirksam, nachdem die Übermittlung den [Zertifizierungsprozess](the-app-certification-process.md) abgeschlossen hat.

> [!IMPORTANT]
> Übermittlungen über die [Microsoft Store-Übermittlungs-API](../monetize/create-and-manage-submissions-using-windows-store-services.md) werden nicht für den Microsoft Store für Unternehmen und den Microsoft Store für Bildungseinrichtungen verfügbar gemacht. Wenn du für deine App Volumenkäufe durch Organisationen ermöglichen möchtest, musst du deine Übermittlungen in Partner Center erstellen und übermitteln.


## <a name="allowing-your-app-to-be-offered-to-organizations"></a>Anbieten Ihrer App an Organisationen

Standardmäßig ist das Kontrollkästchen **Meine App für Organisationen mit Store-verwalteter (online) Volumenlizenzierung und Verteilung verfügbar machen** aktiviert. Das bedeutet, Sie möchten Ihre App für die Aufnahme in App-Katalogen verfügbar machen, die Organisationen für den Erwerb von Volumenlizenzen zur Verfügung gestellt werden. Die Lizenzen sollen dabei über das Onlinelizenzierungssystem des Store verwaltet werden.

> [!NOTE]
> Dies garantiert nicht, dass deine App für alle Organisationen verfügbar gemacht wird.

Wenn Sie Ihre App Organisationen nicht für den Erwerb von Volumenlizenzen anbieten möchten, deaktivieren Sie das Kontrollkästchen. Beachten Sie, dass diese Änderung erst erfolgt, nachdem die App den Zertifizierungsprozess abgeschlossen hat. Wenn Unternehmen bereits zuvor Lizenzen für Ihre App erworben haben, bleiben die Lizenzen weiterhin gültig, und Personen, die die App bereits besitzen, können sie weiterhin verwenden.

> [!TIP]
> Wenn du Branchen-Apps exklusiv für eine konkrete Organisation veröffentlichen möchtest, kannst du eine Unternehmenszuordnung einrichten und der Organisation das direkte Hinzufügen der Apps zu ihrem privaten Store ermöglichen. Weitere Informationen finden Sie unter [Verteilen von Branchen-Apps an Unternehmen](distribute-lob-apps-to-enterprises.md).


## <a name="allowing-disconnected-offline-licensing"></a>Zulassen der getrennten (Offline-) Lizenzierung

Viele Unternehmen benötigen Apps, die offline lizenziert werden können. Einige Unternehmen müssen beispielsweise Apps auf Geräten bereitstellen, die nur selten oder nie mit dem Internet verbunden sind. Wenn Sie Ihre App für diese Kunden verfügbar machen möchten, aktivieren Sie das Kontrollkästchen **Organisationsverwaltete (offline) Lizenzierung und Verteilung von Organisationen zulassen**.

Standardmäßig ist das Kontrollkästchen **deaktiviert**. Du musst das Kontrollkästchen aktivieren, damit wir deine App für geprüfte Unternehmen verfügbar machen können, die sie unter Verwendung der (von der Organisation verwalteten) Offlinelizenzierung installieren. Organisationen müssen eine zusätzliche Überprüfung durchlaufen, um kostenpflichtige Apps bei ihren Kunden auf diese Weise zu installieren.

Über die Offlinelizenzierung können Unternehmen Ihre App auf Volumenbasis erwerben, und anschließend auf den Geräten installieren, ohne auf das Lizenzierungssystem des Store zugreifen zu müssen. Die Organisation kann Ihr App-Paket zusammen mit einer Lizenz herunterladen, über die sie auf Geräten installiert werden kann (über ihre eigenen Verwaltungstools oder durch das Vorabladen von Apps auf Betriebssystem-Images), ohne eine Benachrichtigung an den Store senden zu müssen, wenn eine bestimmte Lizenz verwendet wurde. Durch Aktivieren dieses Szenario wird die Flexibilität bei der Bereitstellung drastisch erhöht und damit möglicherweise auch die Attraktivität Ihrer App bei diesen Kunden erheblich gesteigert.

> [!IMPORTANT]
> Die Offlinelizenzierung wird für XAP-Pakete nicht unterstützt.

 
## <a name="paid-app-support"></a>Unterstützung für kostenpflichtige Apps

Aktuell können Entwicklerkonten in bestimmten Märkten Volumenkäufe kostenpflichtiger Apps über den Microsoft Store für Unternehmen anbieten. 

> [!NOTE]
> In einigen Märkten kann sich der Preis einer App im Microsoft Store für Unternehmen oder im Microsoft Store für Bildungseinrichtungen von dem Preis unterscheiden, der Einzelhandelskunden im Microsoft Store für das gleiche Preisniveau angezeigt wird. Die Auszahlung der Erlöse aus Unternehmenskäufen funktioniert genau wie die Auszahlung von Erlösen aus Verbraucherkäufen Ihrer App. Weitere Informationen finden Sie unter [Bezahlung](getting-paid-apps.md) und [Vereinbarung für App-Entwickler](/legal/windows/agreements/app-developer-agreement). Eine Liste der Märkte, in denen Microsoft Store für Unternehmen und Microsoft Store für Bildungseinrichtungen verfügbar sind, findest du unter [Microsoft Store für Unternehmen und Microsoft Store für Bildungseinrichtungen – Übersicht](/windows/manage/windows-store-for-business-overview#supported-markets).

Sollte dein Land oder deine Region unten nicht aufgeführt sein, werden deine kostenpflichtigen Apps aktuell nicht im Microsoft Store für Unternehmen bzw. im Microsoft Store für Bildungseinrichtungen angeboten. In diesem Fall werden die Optionen für die Unternehmenslizenzierung, die du für deine kostenpflichtigen Apps festlegst, ggf. zu einem späteren Zeitpunkt angewendet, wenn die Unterstützung für Einreichungen aus weiteren Entwicklerkontomärkten hinzugefügt wird.

Aktuell können Entwickler in den folgenden Ländern und Regionen kostenpflichtige Apps über den Microsoft Store für Unternehmen und den Microsoft Store für Bildungseinrichtungen an Unternehmenskunden verteilen:

- Österreich
- Belgien
- Bulgarien
- Kanada
- Kroatien
- Zypern
- Tschechien
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
- Großbritannien
- USA
