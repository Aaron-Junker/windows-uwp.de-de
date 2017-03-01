---
author: jnHs
Description: "Über den Windows Store für Unternehmen können Sie branchenspezifische Apps (Line-of-Business-Apps, LOB-Apps) direkt für Unternehmen veröffentlichen, damit diese Volumenlizenzen erwerben können, ohne die Apps im Store allgemein zur Verfügung zu stellen."
title: Verteilen von branchenspezifischen Apps an Unternehmen
ms.assetid: 2050126E-CE49-4DE3-AC2B-A572AC895158
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 94d12656a122f623240463886297c2378753924b
ms.lasthandoff: 02/07/2017

---

# <a name="distribute-lob-apps-to-enterprises"></a>Verteilen von branchenspezifischen Apps an Unternehmen


Über den Windows Store für Unternehmen können Sie branchenspezifische Apps (Line-of-Business-Apps, LOB-Apps) direkt für Unternehmen veröffentlichen, damit diese Volumenlizenzen erwerben können, ohne die Apps im Store allgemein zur Verfügung zu stellen.

> **Wichtig**  Zurzeit können nur kostenlose Apps über den Windows Store exklusiv an Unternehmen verteilt werden. Wenn Sie eine kostenpflichtige App als LOB übermitteln, steht sie dem Unternehmen derzeit nicht zur Verfügung. 

## <a name="setting-up-the-enterprise-association"></a>Einrichten der Unternehmenszuordnung


Der erste Schritt beim exklusiven Veröffentlichen von branchenspezifischen Apps für ein Unternehmen besteht darin, eine Zuordnung zwischen Ihrem Konto und dem privaten Store des Unternehmens einzurichten.

> **Wichtig**  Dieser Zuordnungsprozess muss vom Unternehmen initiiert werden und muss an die E-Mail-Adresse in den **Kontaktinformationen** Ihres Kontos gesendet werden. Weitere Informationen finden Sie unter [Arbeiten mit LOB-Apps](http://go.microsoft.com/fwlink/p/?LinkId=698846).

Wenn ein Unternehmen Sie zum Veröffentlichen von Apps für die exklusive Nutzung in diesem Unternehmen einlädt, erhalten Sie eine E-Mail mit einem Link, über den Sie die Zuordnung bestätigen können. Sie können diese Zuordnungen auch unter **Unternehmenszusammenschlüsse** in Ihren **Kontoeinstellungen** bestätigen.

Um die Zuordnung zu bestätigen, klicken Sie auf **Akzeptieren**. Über Ihr Konto können dann Apps zur exklusiven Nutzung durch dieses Unternehmen veröffentlicht werden.

## <a name="submitting-an-lob-app"></a>Übermitteln einer branchenspezifischen App


Wenn Sie eine App für die exklusive Nutzung durch ein Unternehmen veröffentlichen möchten, ist das Verfahren vergleichbar mit dem Übermitteln von anderen Apps. Die App durchläuft den gleichen Zertifizierungsprozess und muss allen [Windows Store-Richtlinien](https://msdn.microsoft.com/library/windows/apps/dn764944) entsprechen. Dieser Prozess weicht nur bei einigen wenigen Schritten ab.

### <a name="distribution-and-visibility"></a>Verteilung und Sichtbarkeit

Nachdem Sie eine Unternehmenszuordnung eingerichtet haben, wird jedes Mal, wenn Sie eine App übermitteln, im Abschnitt **Verteilung und Sichtbarkeit** auf der Seite **Preise und Verfügbarkeit** der App ein Dropdownfeld angezeigt. Dies ist standardmäßig auf **Einzelhandel – Distribution** festgelegt. Damit die App exklusiv für ein Unternehmen bereitgestellt wird, müssen Sie **Branche – Distribution** auswählen.

Wenn **Branche – Distribution** aktiviert ist, werden die sonst angezeigten Optionen unter **Verteilung und Sichtbarkeit** durch eine Liste der Unternehmen ersetzt, für die Sie exklusive Apps veröffentlichen dürfen.

Wählen Sie die Unternehmen aus, die die App verwenden dürfen. Niemand sonst kann darauf zugreifen.

> **Hinweis** Sie müssen mindestens ein Unternehmen auswählen, um eine App als branchenspezifische App zu veröffentlichen.

### <a name="organizational-licensing"></a>Organisationslizenzierung

Standardmäßig ist das Kontrollkästchen **App für Organisationen mit vom Store verwalteter (Online-)Volumenlizenzierung verfügbar machen** aktiviert, wenn Sie eine App übermitteln. Bei der Veröffentlichung von branchenspezifischen Apps muss dieses Kontrollkästchen aktiviert sein, damit das Unternehmen Volumenlizenzen für die App erwerben kann. Hierdurch wird die App nicht für andere außerhalb der Unternehmen verfügbar gemacht, die Sie im Abschnitt **Verteilung und Sichtbarkeit** ausgewählt haben.

Wenn Sie die App für das Unternehmen über Offline-Lizenzierung verfügbar machen möchten, können Sie zusätzlich das Kontrollkästchen **Getrennte Lizenzierung (offline) für Käufe von Organisationen zulassen** aktivieren.

Weitere Informationen finden Sie unter [Optionen für die Organisationslizenzierung](organizational-licensing.md).

### <a name="age-ratings"></a>Altersfreigaben
Beim Übermittlungsvorgang von branchenspezifischen Apps ist der Schritt für die [Altersfreigabe](age-ratings.md) mit dem für Einzelhandels-Apps identisch. Sie verfügen jedoch über eine zusätzliche Option, mit der Sie die Store-Altersfreigabe Ihrer App manuell angeben können, anstatt den Fragebogen auszufüllen oder eine vorhandene Freigabe-ID der IARC zu importieren. Diese manuelle Freigabe kann nur mit einer LOB-Verteilung verwendet werden. Wenn Sie also die Einstellung für **Verteilung und Sichtbarkeit** der App zu **Einzelhandel – Distribution** ändern möchten, müssen Sie den Altersfreigabe-Fragebogen ausfüllen, bevor Sie die Übermittlung veröffentlichen können.

### <a name="enterprise-deployment-of-lob-apps"></a>Unternehmensbereitstellung von branchenspezifischen Apps

Wenn Sie auf **An Store übermitteln** klicken, durchläuft die App den Zertifizierungsprozess. Danach muss ein Administrator des Unternehmens die App dem privaten Store des Unternehmens im Portal des Windows Store für Unternehmen hinzufügen. Das Unternehmen kann die App dann für seine Benutzer bereitstellen.

> **Hinweis** Um Ihre branchenspezifische App zu erhalten, muss sich die Organisation in einem [unterstützten Markt](https://technet.microsoft.com/itpro/windows/whats-new/windows-store-for-business-overview#supported-markets) befinden. Beim Übermitteln der App darf dieser Markt nicht ausgeschlossen worden sein. 

Weitere Informationen finden Sie unter [Arbeiten mit Branchen-Apps](http://go.microsoft.com/fwlink/p/?LinkId=698846) und [Verteilen von Apps über den privaten Store](http://go.microsoft.com/fwlink/p/?LinkId=698847).

### <a name="updating-lob-apps"></a>Aktualisieren von branchenspezifischen Apps

Wenn Sie Updates für eine App veröffentlichen möchten, die bereits als branchenspezifische App veröffentlicht wurde, erstellen Sie einfach eine neue Übermittlung. Sie können neue Pakete hochladen oder andere Änderungen vornehmen. Klicken Sie dann auf **An Store übermitteln**, um die aktualisierte Version verfügbar zu machen. Achten Sie darauf, die Auswahl der Unternehmen unter **Verteilung und Sichtbarkeit** nicht zu ändern (es sei denn, Sie möchten sie bewusst ändern und z. B. ein zusätzliches Unternehmen auswählen, das die App erwerben kann, oder eines der zuvor eingerichteten Unternehmen löschen).

Wenn Sie eine App, die Sie zuvor als branchenspezifische App veröffentlicht haben, nicht mehr anbieten möchten und keine neuen Käufe möglich sein sollen, müssen Sie eine neue Übermittlung erstellen. Zunächst müssen Sie die Auswahl unter **Verteilung und Sichtbarkeit** von **Branche – Distribution** in **Einzelhandel – Distribution** ändern. Wählen Sie dann unter **Verteilung und Sichtbarkeit** die Option **Diese App ausblenden und den Verkauf stoppen** aus. Nachdem die Übermittlung den Zertifizierungsprozess durchlaufen hat, kann die App nicht mehr neu erworben werden. Benutzer, die sie bereits erworben haben, können sie jedoch weiter verwenden.

> **Hinweis** Beim Ändern einer App zu **Einzelhandel – Distribution** müssen Sie (sofern noch nicht erfolgt) den [Altersfreigabe-Fragebogen](age-ratings.md) ausfüllen (selbst wenn die App nicht für neue Verkäufe verfügbar ist).

### <a name="distributing-lob-apps-through-sideloading"></a>Verteilen von branchenspezifischen Apps durch Querladen

Wenn Apps über den Store für Unternehmen verfügbar gemacht werden, wird sichergestellt, dass die App vom Store signiert wurde und den Standardrichtlinien des Stores entspricht.

In einigen Fällen möchten Unternehmen möglicherweise aus verschiedenen Gründen nicht, dass ihre branchenspezifischen Apps über das Windows Dev Center übermittelt werden (z. B. aus Compliance-Gründen oder bei Apps, für die weitere Funktionen benötigt werden). In diesem Fall können die Unternehmen Apps durch Querladen direkt auf Computern bereitstellen und müssen nicht den Windows Store für Unternehmen verwenden.

Weitere Informationen finden Sie unter [Querladen von Branchen-Apps in Windows 10](http://go.microsoft.com/fwlink/p/?LinkId=623433).

 

 





