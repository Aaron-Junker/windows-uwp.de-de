---
author: jnHs
Description: You can publish line-of-business (LOB) apps directly to enterprises for volume acquisition via the Microsoft Store for Business or Microsoft Store for Education, without making the apps broadly available in the Store.
title: Verteilen von branchenspezifischen Apps an Unternehmen
ms.assetid: 2050126E-CE49-4DE3-AC2B-A572AC895158
ms.author: wdg-dev-content
ms.date: 03/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, LOB, Branche, Unternehmens-Apps, Store für Unternehmen, Store für Bildungseinrichtungen, Enterprise
ms.localizationpriority: medium
ms.openlocfilehash: 9149533a12263e105356a1683257c4d9172eefb5
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2018
ms.locfileid: "2882107"
---
# <a name="distribute-lob-apps-to-enterprises"></a>Verteilen von branchenspezifischen Apps an Unternehmen


Über den Microsoft Store für Unternehmen oder den Microsoft Store für Bildungseinrichtungen können Sie branchenspezifische Apps (Line-of-Business-Apps, LOB-Apps) direkt für Unternehmen veröffentlichen, damit diese Volumenlizenzen erwerben können, ohne die Apps im Store allgemein zur Verfügung zu stellen.

> [!NOTE]
> Zurzeit können nur kostenlose Apps exklusiv über den Microsoft Store für Unternehmen oder den Microsoft Store für Bildungseinrichtungen verteilt werden. Wenn Sie eine kostenpflichtige App als LOB übermitteln, steht sie dem Unternehmen nicht zur Verfügung. 

> [!IMPORTANT]
> Sie können die [Microsoft Store-Übermittlungs-API](../monetize/create-and-manage-submissions-using-windows-store-services.md) nicht verwenden, um branchenspezifische Apps direkt an Unternehmen zu veröffentlichen. Alle Übermittlungen für branchenspezifische Apps müssen über das Windows Dev Center-Dashboard erfolgen.


## <a name="set-up-the-enterprise-association"></a>Einrichten der Unternehmenszuordnung

Der erste Schritt beim exklusiven Veröffentlichen von branchenspezifischen Apps für ein Unternehmen besteht darin, eine Zuordnung zwischen Ihrem Konto und dem privaten Store des Unternehmens einzurichten.

> [!IMPORTANT]
> Dieser Zuordnungsprozess muss vom Unternehmen initiiert werden, und Sie müssen die E-Mail-Adresse mit dem Microsoft-Konto verwenden, das mit das Entwicklerkonto erstellt wurde. Weitere Informationen finden Sie unter [Arbeiten mit LOB-Apps](http://go.microsoft.com/fwlink/p/?LinkId=698846).

Wenn ein Unternehmen Sie zum Veröffentlichen von Apps für die exklusive Nutzung in diesem Unternehmen einlädt, erhalten Sie eine E-Mail mit einem Link, über den Sie die Zuordnung bestätigen können. Sie können diese Zuordnungen auch überprüfen, indem Sie zum Abschnitt **Unternehmenszusammenschlüsse** Ihrer **Kontoeinstellungen** navigieren (sofern Sie mit dem Microsoft-Konto angemeldet sind, das verwendet wurde, um das Entwicklerkonto zu eröffnen).

Um die Zuordnung zu bestätigen, klicken Sie auf **Akzeptieren**. Über Ihr Konto können dann Apps zur exklusiven Nutzung durch dieses Unternehmen veröffentlicht werden.


## <a name="submit-lob-apps"></a>Branchenspezifische Apps übermitteln

Wenn Sie eine App für die exklusive Nutzung durch ein Unternehmen veröffentlichen möchten, ist das Verfahren vergleichbar mit dem Übermitteln von anderen Apps. Die App durchläuft den gleichen [Zertifizierungsprozess](the-app-certification-process.md) und muss allen [Microsoft Store-Richtlinien](https://docs.microsoft.com/legal/windows/agreements/store-policies). entsprechen. Dieser Prozess weicht nur bei einigen wenigen Schritten ab.


### <a name="visibility"></a>Sichtbarkeit

Nachdem Sie eine Unternehmenszuordnung eingerichtet haben, wird jedes Mal, wenn Sie eine App übermitteln, im Abschnitt **Sichtbarkeit** auf der Seite **Preise und Verfügbarkeit** der App ein Dropdownfeld angezeigt. Dies ist standardmäßig auf **Einzelhandel – Distribution** festgelegt. Damit die App exklusiv für ein Unternehmen bereitgestellt wird, müssen Sie **Branche – Distribution** auswählen.

Wenn **Branche – Distribution** aktiviert ist, werden die sonst angezeigten Optionen unter **Sichtbarkeit** durch eine Liste der Unternehmen ersetzt, für die Sie exklusive Apps veröffentlichen dürfen. Niemand außerhalb der(s) ausgewählten Unternehmen(s) kann die App anzeigen oder herunterladen.

Sie müssen mindestens ein Unternehmen auswählen, um eine App als branchenspezifische App zu veröffentlichen.

<span id="organizational" />

### <a name="organizational-licensing"></a>Organisationslizenzierung

Standardmäßig ist das Kontrollkästchen **App für Organisationen mit vom Store verwalteter (Online-)Volumenlizenzierung verfügbar machen** aktiviert, wenn Sie eine App übermitteln. Bei der Veröffentlichung von branchenspezifischen Apps muss dieses Kontrollkästchen aktiviert sein, damit das Unternehmen Volumenlizenzen für die App erwerben kann. Hierdurch wird die App nicht für andere außerhalb der Unternehmen verfügbar gemacht, die Sie im Abschnitt **Verteilung und Sichtbarkeit** ausgewählt haben.

Wenn Sie die App für das Unternehmen über Offline-Lizenzierung verfügbar machen möchten, können Sie zusätzlich das Kontrollkästchen **Getrennte Lizenzierung (offline) für Käufe von Organisationen zulassen** aktivieren.

Weitere Informationen finden Sie unter [Optionen für die Organisationslizenzierung](organizational-licensing.md).


### <a name="age-ratings"></a>Altersfreigaben

Beim Übermittlungsvorgang von branchenspezifischen Apps ist der Schritt für die [Altersfreigabe](age-ratings.md) mit dem für Einzelhandels-Apps identisch. Sie verfügen jedoch über eine zusätzliche Option, mit der Sie die Store-Altersfreigabe Ihrer App manuell angeben können, anstatt den Fragebogen auszufüllen oder eine vorhandene Freigabe-ID der IARC zu importieren. Diese manuelle Freigabe kann nur mit einer LOB-Verteilung verwendet werden. Wenn Sie also die Einstellung für **Sichtbarkeit** der App zu **Einzelhandel – Distribution** ändern möchten, müssen Sie den Altersfreigabe-Fragebogen ausfüllen, bevor Sie die Übermittlung veröffentlichen können.


## <a name="enterprise-deployment-of-lob-apps"></a>Unternehmensbereitstellung von branchenspezifischen Apps

Wenn Sie auf **An Store übermitteln** klicken, durchläuft die App den Zertifizierungsprozess. Danach muss ein Administrator des Unternehmens die App dem privaten Store des Unternehmens im Portal des Microsoft Store für Unternehmen oder des Microsoft Store für Bildungswesen hinzufügen. Das Unternehmen kann die App dann für seine Benutzer bereitstellen.

> [!NOTE]
> Um Ihre branchenspezifische App zu erhalten, muss sich die Organisation in einem [unterstützten Markt](https://technet.microsoft.com/itpro/windows/whats-new/windows-store-for-business-overview#supported-markets) befinden. Beim Übermitteln der App darf dieser Markt [nicht ausgeschlossen worden sein](define-pricing-and-market-selection.md). 

Weitere Informationen finden Sie unter [Arbeiten mit Branchen-Apps](http://go.microsoft.com/fwlink/p/?LinkId=698846) und [Verteilen von Apps über den privaten Store](http://go.microsoft.com/fwlink/p/?LinkId=698847).


## <a name="update-lob-apps"></a>Aktualisieren branchenspezifischer Apps

Wenn Sie Updates für eine App veröffentlichen möchten, die bereits als branchenspezifische App veröffentlicht wurde, erstellen Sie einfach eine neue Übermittlung. Sie können neue Pakete hochladen oder andere Änderungen vornehmen. Klicken Sie dann auf **An Store übermitteln**, um die aktualisierte Version verfügbar zu machen. Achten Sie darauf, die Auswahl der Unternehmen unter **Sichtbarkeit** nicht zu ändern (es sei denn, Sie möchten bewusst Änderungen vornehmen und z.B. ein zusätzliches Unternehmen auswählen, das die App erwerben kann, oder eines der zuvor eingerichteten Unternehmen löschen).

Wenn Sie eine App, die Sie zuvor als branchenspezifische App veröffentlicht haben, nicht mehr anbieten möchten und keine neuen Käufe möglich sein sollen, müssen Sie eine neue Übermittlung erstellen. Zunächst müssen Sie die Auswahl unter **Sichtbarkeit** von **Branche – Distribution** in **Einzelhandel – Distribution** ändern. Wählen Sie mit der Option **Beenden des Erwerbs** im Abschnitt [Erkennbarkeit](choose-visibility-options.md#discoverability) **Make this product available but not discoverable in the Store** aus.

Nachdem die Übermittlung den Zertifizierungsprozess durchlaufen hat, kann die App nicht mehr neu erworben werden. Benutzer, die sie bereits erworben haben, können sie jedoch weiter verwenden.

> [!NOTE]
> Beim Ändern einer App zu **Einzelhandel – Distribution** müssen Sie (sofern noch nicht erfolgt) den [Altersfreigabe-Fragebogen](age-ratings.md) ausfüllen (selbst wenn die App nicht für neue Verkäufe verfügbar ist).


## <a name="distribute-lob-apps-through-sideloading"></a>Verteilen von branchenspezifischen Apps durch Querladen

Wenn Apps über den Microsoft Store für Unternehmen oder den Microsoft Store für Bildungswesen für ein Unternehmen verfügbar gemacht werden, wird sichergestellt, dass die App vom Store signiert wurde und den Standardrichtlinien des Stores entspricht.

In einigen Fällen möchten Unternehmen möglicherweise nicht, dass ihre branchenspezifischen Apps über das Windows Dev Center übermittelt werden (z.B. aus Compliance-Gründen oder für Apps, für die weitere Funktionen benötigt werden). In diesem Fall kann das Unternehmen Apps durch Querladen direkt auf Computern bereitstellen und müssen nicht den Microsoft Store für Unternehmen oder den Microsoft Store für Bildungseinrichtungen verwenden.

Weitere Informationen finden Sie unter [Querladen von Branchen-Apps in Windows10](http://go.microsoft.com/fwlink/p/?LinkId=623433).

 

 




