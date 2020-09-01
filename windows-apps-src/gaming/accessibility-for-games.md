---
title: Erstellen barrierefreier Spiele
description: Erfahren Sie, wie Sie barrierefreie Spiele erstellen. Verwenden Sie das inklusive Spielentwurfsprinzip, um ein barrierefreies Spiel zu entwickeln.
ms.assetid: f5ba1e60-0d7c-11e6-91ec-0002a5d5c51b
ms.date: 11/09/2017
ms.topic: article
keywords: Windows 10, UWP, Barrierefreiheit, Spiele
ms.localizationpriority: medium
ms.openlocfilehash: f90f976f696d5c49e7f772627bbb7d0e3e3d0908
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159434"
---
#  <a name="making-games-accessible"></a>Erstellen barrierefreier Spiele

Eingabehilfen können jede Person und jedes Unternehmen auf der Welt dabei unterstützen, mehr zu erreichen. Dies gilt auch für die Verbesserung der Barrierefreiheit von Spielen. Dieser Artikel ist für Spieleentwickler, Spiele-Designer und Producer geschrieben. Er enthält eine Übersicht über Anleitungen für die Entwicklung barrierefreier Spiele verschiedener Organisationen, die unten im Referenzabschnitt aufgelistet werden, und führt Sie in das inklusive Spielentwurfsprinzip ein, sodass Sie die Barrierefreiheit Ihrer Spiele verbessern können.

## <a name="gaming-for-everyone"></a>Gaming for Everyone

Microsoft ist der Meinung, dass Spiele für alle Benutzer Spaß machen sollten. Wir haben uns dazu verpflichtet, mehr zu tun, um Gaming zu einer inklusiven Umgebung zu machen, in der sich alle Wir sind davon überzeugt, dass das, was wir für unsere Lüfter entwickeln, und die Art und Weise, wie wir – innerhalb und außerhalb der Wände von Microsoft erscheinen – eine Reflektion von der Person ist. Wir haben das Programm so entworfen, dass es die wichtigsten Werte in Form einer Organisation widerspiegelt und dass das Programm zu positiven Änderungen führen kann – nicht nur in unserem Arbeitsplatz, sondern in den Produkten, die wir für die von uns entwickelten Spieler erstellen. " ([Blog Beitrag](https://blogs.microsoft.com/blog/2016/06/13/gaming-for-everyone/) von Phil Spencer)

Wir möchten eine interessante, vielfältige und inklusivumgebung erstellen, in der jeder Benutzer spielen kann. "Um eine dauerhafte Auswirkung zu haben, ist eine Kultur Schicht erforderlich, die nicht über Nacht stattfindet. Unser Team ist jedoch verpflichtet, jeden Tag besser zu werden, um sich gegenseitig zu informieren, um den Entscheidungsprozess anzuhalten und sich über die beeindruckende Vielfalt von Anforderungen, Fähigkeiten und Interessen zwischen den gamten auf der ganzen Welt Gedanken zu machen. " ([Blog Beitrag](https://blogs.microsoft.com/blog/2016/06/13/gaming-for-everyone/) von Phil Spencer)

Wir hoffen, dass Sie uns auf dieser Journey beitreten, um das [Spiel für jeden](https://news.microsoft.com/gamingforeveryone/) Benutzer zu gestalten. 

##  <a name="why-make-games-accessible"></a>Warum sollten Sie barrierefreie Spiele entwickeln?

### <a name="increased-gamer-base"></a>Größere Zahl von Spielern

Grundsätzlich betrachtet, ist es nicht schwierig, die Entwicklung barrierefreier Spiele geschäftlich zu begründen:

Sie multiplizieren die Zahl der Benutzer, die Ihr Spiel spielen können, mit der Qualität Ihres Spiels und erhalten so die Verkaufszahlen für Ihr Spiel.

Wenn Sie ein beeindruckendes Spiel entwickelt haben, das so kompliziert oder komplex ist, dass es nur von sehr wenigen Menschen gespielt werden kann, begrenzen Sie Ihre Verkaufszahlen. Und wenn Sie ein Spiel entwickeln, das von Menschen mit physischen, sensorischen oder kognitiven Behinderungen nicht gespielt werden kann, entgehen Ihnen ebenfalls mögliche Umsätze. Wenn z. b. [19% der Menschen in der USA eine gewisse Behinderung aufweisen](https://www.census.gov/newsroom/releases/archives/miscellaneous/cb12-134.html), [haben geschätzte 14% der Erwachsenen in den USA Schwierigkeiten beim Lesen](https://nces.ed.gov/naal/estimates/overview.aspx), und [geschätzte 10% der Männer weisen eine Form von Farb sichtvermögen](https://www.aao.org/eye-health/diseases/color-blindness-risk)auf. Dies kann sich möglicherweise sehr stark auf den Umsatz ihres Titels auswirken. 

Weitere geschäftliche Begründungen finden Sie unter [Erstellen barrierefreier Videospiele](/windows/desktop/DxTechArts/accessibility-best-practices).

### <a name="better-games"></a>Bessere Spiele

Wenn Sie die Barrierefreiheit eines Spiels verbessern, kann dies dazu führen, dass das Spiel insgesamt besser wird. 

Ein Beispiel hierfür ist die Verwendung von Untertiteln in Spielen. In der Vergangenheit unterstützten Spiele nur selten Untertitel bzw. Untertitelungen für die Dialoge in Spielen. Heute wird erwartet, dass Spiele Untertitel bzw. Untertitelungen enthalten. Diese Änderung wurde nicht von Spielern mit Behinderungen verursacht. Stattdessen wurde sie von der Lokalisierung gesteuert, wurde aber mit einer Vielzahl von Spielern beliebt, die einfach mit Untertiteln spielen wollten, weil die Spiele Darstellung besser war. Spieler aktivieren Untertitel bzw. Untertitelungen, wenn sie mit zu viel Hintergrundgeräuschen spielen; wenn sie Schwierigkeiten haben, Stimmen zu hören, wenn gleichzeitig mehrere Audioeffekte oder Umgebungsgeräusche abgespielt werden; oder wenn sie einfach nur die Lautstärke niedrig halten möchten, um andere nicht zu stören. Untertitel und Untertitelungen haben Spielern nicht nur geholfen, eine bessere Spielerfahrung zu erzielen, sondern ermöglichen auch Menschen mit beeinträchtigter Hörfähigkeit, Spiele zu spielen.

Die Neuzuordnung von Controllern ist ein weiteres Feature, das aus ähnlichen Gründen allmählich zum Standard in der Spielebranche wird. Dies wird am häufigsten als Vorteil für alle Spieler angeboten. Einige Spieler können Ihre Spiele Umgebung anpassen, und einige unterscheiden sich einfach von der Bedeutung der Designer. Die meisten Leute sind nicht davon abhängig, dass die Möglichkeit zum Neuzuordnen von Schaltflächen auf einem Eingabegerät tatsächlich auch ein Barrierefreiheits Feature ist, das dazu gedacht war, ein Spiel für Personen mit unterschiedlichen Arten von motorischen Behinderungen zu spielen, die physisch nicht in der Lage sind oder es schwierig sind, bestimmte Bereiche des Controllers zu betreiben.

Letzten Endes führt das Nachdenken darüber, wie Sie die Barrierefreiheit Ihres Spiels verbessern können, häufig zu einem besseren Spiel, da Sie auf diese Weise eine Umgebung mit höherer Benutzerfreundlichkeit und besserer Anpassbarkeit für Ihre Spieler entwickeln.

### <a name="social-space-and-quality-of-life"></a>Sozialer Raum und Qualität der Lebensdauer

Video Spiele sind eine der besten Grotten von Unterhaltungs-und Spielzeiten. Für einige ist Gaming jedoch nicht nur eine Form der Unterhaltung, sondern auch eine Möglichkeit, einem Bett im Krankenhaus, chronischen Schmerzen oder lähmenden sozialen Ängsten zu entkommen. Spieler werden in eine Welt transportiert, in denen sie zu den Hauptpersonen im Videospiel werden. Durch das Gaming können Sie einen sozialen Raum für sich selbst erstellen und in diesem agieren, der sie von ihren alltäglichen Problemen aufgrund ihrer Behinderung ablenkt und ihnen eine Möglichkeit bietet, mit Menschen zu kommunizieren, mit denen sie andernfalls möglicherweise nicht interagieren könnten. 

Gaming ist auch eine Kultur. Wenn Sie in der Lage sind, die gleichen Informationen wie alle Ihre Freunde zu verwenden, ist es etwas, das für die Lebensdauer von Menschen sehr wertvoll sein kann.

##  <a name="is-the-game-you-are-making-today-accessible"></a>Ist das Spiel, das Sie heute entwickeln, barrierefrei?

Wenn Sie zum ersten Mal darüber nachdenken, Ihr Spiel barrierefrei zu machen, finden Sie im Folgenden einige Fragen, die Sie sich stellen sollten:

* Können Sie das Spiel mit nur einer Hand spielen? 
* Kann ein durchschnittlicher Benutzer das Spiel auswählen und sofort spielen?
* Können Sie das Spiel auf einem kleinen Bildschirm oder auf einem Fernseher aus einer gewissen Entfernung effektiv spielen?
* Unterstützen Sie mehr als eine Art von Eingabegerät, das während des gesamten Spiels verwendet werden kann?
* Können Sie das Spiel ohne Audio spielen?
* Können Sie das Spiel mit einem auf Schwarzweiß eingestellten Monitor spielen?
* Wenn Sie das letzte gespeicherte Spiel nach einem Monat laden, können Sie leicht ermitteln, wo Sie sich im Spiel befinden, und wissen, was Sie tun müssen, um den Vorgang fortzusetzen?

Wenn Sie die meisten Fragen mit „nein“ beantwortet haben oder die Antwort auf die meisten Fragen nicht kennen, ist es an der Zeit, die Barrierefreiheit Ihres Spiels zu verbessern.

## <a name="defining-disability"></a>Definition von Behinderungen

Behinderungen werden als „fehlende Übereinstimmung zwischen den Bedürfnissen einer Person und dem Service, dem Produkt oder der Umgebung, die angeboten werden“ definiert. ([Video zum Inklusivprinzip](https://www.microsoft.com/design/inclusive/), Microsoft.com.) Dies bedeutet, dass jeder Benutzer eine Behinderung erfahren kann und dies ein kurzfristiger oder situationsbedingter Zustand sein kann. Stellen Sie sich die Herausforderungen vor, denen sich Spieler in einer derartigen Lage gegenübersehen, wenn sie Ihr Spiel spielen, und überlegen Sie, wie Sie Ihr Spiel für diese Personen verbessern können. Im Folgenden finden Sie einige Behinderungen, an die Sie denken sollten:

### <a name="vision"></a>Bildanalyse

*   Medizinische langfristige Bedingungen wie grüner Star, Katarakte, Farbblindheit, Kurzsichtigkeit und diabetesbedingte Retinopathie
*   Kurzfristige, situationsbedingte Bedingungen wie ein kleiner Monitor oder Bildschirmgröße, ein Bildschirm mit niedriger Auflösung oder Bildschirm Glanz aufgrund von hellen Lichtquellen wie der Sonne auf einem Monitor oder mobilen Bildschirm
        
### <a name="hearing"></a>Hörvermögen

* Medizinische langfristige Bedingungen wie vollständige Taubheit oder teilweiser Hörverlust aufgrund von Krankheiten oder genetischen Ursachen
* Kurzfristige, situationsbedingte Bedingungen wie übermäßige Hintergrundgeräusche, Audioqualität mit niedriger Qualität oder eingeschränktes Volume, um andere Probleme zu vermeiden
        
### <a name="motor"></a>Motorische Fähigkeiten

* Medizinische langfristige Bedingungen wie Parkinson's Krankheit, amyotrophe Lateralsklerose (ALS), Arthritis und Muskeldystrophie
* Kurzfristige situationsbedingte Zustände wie eine verletzte Hand, Halten eines Getränks oder Tragen eines Kindes auf einem Arm
  
### <a name="cognitive"></a>Kognitiv

* Medizinische langfristige Bedingungen wie Dyslexie, Epilepsie, Aufmerksamkeitsdefizitstörung (ADHD), Demenz und Amnesie
* Kurzfristige, situationsbedingte Bedingungen wie der Alkohol Verbrauch, der Mangel an Standbymodus oder temporäre Ablenkungen, wie z. b. Sirenen von einem Notfall Fahrzeug, das das Haus steuert

### <a name="speech"></a>Spracheingabe

* Medizinische langfristige Bedingen wie beschädigte Stimmbänder, Dysarthrie und Apraxie
* Kurzfristige situationsbedingte Zustände wie zahnärztliche Behandlungen oder Essen und Trinken


## <a name="how-to-make-games-more-accessible"></a>Wie können Sie die Barrierefreiheit von Spielen verbessern?

### <a name="design-shift-inclusive-game-design-approach"></a>Änderung des Designansatzes: inklusives Spieledesign

Das inklusive Design konzentriert sich auf das Erstellen von Produkten und Diensten, die für ein breiteres Spektrum von Verbrauchern besser zugänglich sind, darunter auch Menschen mit Behinderungen.

Um erfolgreich zu sein, müssen sich die Spieleentwickler von heute über das Erstellen von spielen Gedanken machen, die Sie genießen. Spiele-Designer müssen wissen, wie sich Ihre Entwurfsentscheidungen auf die allgemeine Barrierefreiheit des Spiels auswirken. die Playability des Spiels für die gesamte Zielgruppe, einschließlich derjenigen mit Behinderungen.

Es muss daher ein Paradigmenwechsel vom herkömmlichen Spieledesign hin zum inklusiven Konzept für das Design von Spielen stattfinden. Das inklusive Spieledesign geht über das einfache Spieledesign hinaus, das der Zielgruppe Unterhaltung bietet. Es bedeutet, zusätzliche oder geänderte Personas zu entwickeln, um eine breitere Gruppe von Spielern anzusprechen. Sie müssen sich sehr bewusst sein, dass Sie Barrieren in Ihrem Spiel entwerfen und sicherstellen, dass Sie keine unnötigen Barrieren hinzufügen, die sich von der vorgesehenen Benutzer Arbeit unterscheidet.

Durch die Identifizierung von Lücken können Sie das ursprüngliche Entwurfskonzept optimieren, das ursprüngliche Entwurfskonzept durchlaufen und es besser gestalten, sodass mehr Personen Ihre Vision erleben können. Wenn Sie sich Zeit nehmen und den inklusiven Designansatz für Ihr Spiel berücksichtigen, wird Ihr Spiel letzten Endes besser zugänglich. Kein Spiel kann für jeden Benutzer funktionieren, die Definition des Spiels erfordert, dass es eine gewisse Herausforderung gibt, aber durch die Berücksichtigung der Barrierefreiheit können Sie sicherstellen, dass niemand unnötig ausgeschlossen wird.

### <a name="empower-gamers-give-gamers-options"></a>Optionen für Spieler

Fast jede Barrierefreiheits Lösung ist auf eine von zwei Prinzipien zurückgegriffen. Der erste gibt ihren Gamern die Optionen zum Anpassen des Spielerlebnisses. Wenn Sie bereits eine großer Fanbasis haben, gibt es möglicherweise eine erhebliche Zahl von Spielern, die nicht möchten, dass die Umgebung auch nur minimal geändert wird. Das ist in Ordnung. Ermöglichen Sie Ihren Spielern, diese Features zu aktivieren und zu deaktivieren, und ermöglichen Sie die individuelle Konfiguration von Features. Sie müssen den Benutzern die Möglichkeit geben, das Spiel so zu gestalten, dass es Ihren Anforderungen und Vorlieben am besten entspricht.

### <a name="reinforce-communicate-information-in-more-than-one-way"></a>Verstärkung: Informationen auf mehr als eine Weise kommunizieren

Das zweite Prinzip ist das Konzept des universellen Entwurfs, bei dem es sich um einen einzelnen Ansatz handelt, der nicht nur mehr Player bietet, sondern auch die Funktion für alle verbessert. Beispielsweise ein Bild und Text, ein Symbol und eine Farbe. Eine Karte, die auf einem Bereich unterschiedlicher farbiger Marker basiert, ist nicht nur für farbenblind Spieler unmöglich, sondern auch für alle anderen Personen, die sich merken müssen, was alles entspricht. Durch das Hinzufügen von Symbolen wird es für alle Benutzer besser.

### <a name="innovate-be-creative"></a>Innovationen: Seien Sie kreativ

Es gibt viele kreative Möglichkeiten, um die Barrierefreiheit Ihres Spiels zu verbessern. Werden Sie kreativ, und lernen Sie von anderen barrierefreien Spielen auf dem Markt. Wenn Sie bereits ein Spiel entwickelt haben, sollten Sie versuchen, aktuelle Features Ihres Spiels zu identifizieren, die verbessert werden können, ohne das ursprüngliche Design der zentralen Spielmechanismen und der Spielerfahrung zu verändern. Wie bereits erwähnt, geht es bei der Barrierefreiheit von Spielen darum, Spielern Optionen für die Anpassung ihrer Spielerfahrung bereitzustellen. Dies kann durch die Verstärkung oder die Kommunikation von Informationen auf mehrere Weise erfolgen. 

Wenn Sie die Barrierefreiheit in Erwägung ziehen, können Sie den Entwurf aus einem neuen Winkel und möglicherweise Ideen, die Sie sich nicht anders vorstellen würden. Diese Vorgehensweise hat sich nicht nur auf interessante Konzepte, sondern auch auf Produkte entwickelt, die eine weit verbreitete Einführung oder einen kommerziellen Erfolg von Massenmärkten haben. Beispiele hierfür sind Predictive Text, Voice Recognition, Drosselung, der Lautsprecher, der Typewriter und die optische Zeichenerkennung (OCR). Ideen für diese Produkte stammen von denjenigen, die sich Gedanken über Lösungen für Barrierefreiheit machten.

### <a name="adopt-quality-means-accessible-features"></a>Übernehmen: Qualität bedeutet barrierefreie Features

Barrierefreiheit ist ein Maß der Qualität. Es muss eine Funktions Anforderung und keine gute Arbeitsaufgabe sein. Beispielsweise wird "Anpassen von Minikarte für die Farb Blindheit" nicht als Arbeits Element mit niedriger Priorität angesehen, das Sie erhalten, wenn Sie zusätzliche Zeit haben. Wenn dieses Arbeits Element nicht abgeschlossen ist, bedeutet dies lediglich, dass das gesamte Minikarte-Feature unvollständig ist und nicht versendet werden kann.

### <a name="evangelize-make-accessibility-a-priority-in-your-game-studio"></a>Werben Sie: Machen Sie die Barrierefreiheit in Ihrem Spielestudio zu einer Priorität

Für die Spieleentwicklung gibt es stets einen engen Terminplan. Wenn Sie die Barrierefreiheit zur Priorität erklären, wird es einfacher. Eine Möglichkeit besteht darin, das Spiel von Anfang an im Gedanken an Barrierefreiheit zu entwerfen. Wenn Sie die Barrierefreiheit in Erwägung gezogen haben, desto einfacher und günstiger wird Sie. 

Teilen Sie Ihr Wissen über Barrierefreiheit mit Ihrem Team, teilen Sie die geschäftlichen Begründungen, und bereinigen Sie die allgemeinen Missverständnisse – das ist nicht für viele Personen von Vorteil, es wird Ihr Mechaniker verwässert, und die Implementierung ist schwierig und teuer.

### <a name="review-constantly-evaluate-your-game"></a>Überprüfen Sie: Bewerten Sie Ihr Spiel laufend

Sie können einen Überprüfungsprozess während der Entwicklung einführen, um sicherzustellen, dass die Barrierefreiheit bei jedem Schritt berücksichtigt wird. Erstellen Sie eine Checkliste wie die unten gezeigte, um Ihrem Team zu helfen, laufend zu überprüfen, ob die entwickelten Features barrierefrei sind.

| Checkliste                                         | Barrierefreiheitsfunktionen                                                                                                         |
|---------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| Kinoeffekte im Spiel                                | Hat Untertitel und Untertitelungen, ist auf Fotosensibilität getestet                                                                           |
| Grafiken im Allgemeinen (2D- und 3D-Grafiken)              | Farben und Optionen für farbenblind Farben, die nicht vollständig von der Farbe für die Identifikation abhängen, aber auch Formen und Muster verwenden|
| Startbildschirm, Einstellungsmenü und andere Menüs       | Möglichkeit, Optionen vorzulesen, Möglichkeit, Einstellungen zu merken, alternative Eingabemethode für Befehlssteuerung, anpassbarer UI-Schriftgrad  |
| Spielplan                                          | Umfassend anpassbarere Schwierigkeitsgrade, Untertitel und Untertitelungen, gutes visuelles und Audiofeedback für die Spieler                           |
| HUD-Anzeige                                       | Anpassbare Bildschirmposition, anpassbare Schriftart Größe, farbenblind (Option)                                                  |        
| Steuerelementeingabe                                     | Möglichkeit, Steuerelemente dem Eingabegerät zuzuordnen, benutzerdefinierte Controller-Unterstützung, Zulässigkeit vereinfachter Eingaben im Spiel                               |        

### <a name="playtest-and-iterate-get-gamers-feedback"></a>Spieletests und Überarbeitungen: Bitten Sie Spieler um Feedback

Laden Sie Spieletester mit Behinderungen, für die bei der Spieleentwicklung berücksichtigt wurden, zu den Spieletests ein. Denken Sie daran, die Fragen zur Barrierefreiheit in die Beta Test-Frage stellen einzubeziehen. Lokale Behindertengruppen sind eine hervorragend Quelle von Teilnehmern. Achten Sie darauf, wie sie spielen, und bitten Sie sie um Feedback. Stellen Sie fest, welche Änderungen vorgenommen werden müssen, um das Spiel zu verbessern.

Verwenden Sie soziale Medien und das Forum Ihres Spiels, um Eingaben zu überwachen, welche Barrierefreiheits Features am meisten von Bedeutung sind und wie Sie implementiert werden sollten. 

### <a name="shout-it-out-let-the-world-know-your-game-is-accessible"></a>Erzählen Sie es jedem: Lassen Sie die Welt wissen, dass Ihr Spiel barrierefrei ist

Verbraucher möchten wissen, ob Ihr Spiel von Spielern mit Behinderungen gespielt werden kann. Stellen Sie sicher, dass die Barrierefreiheit des Spiels auf der Game-Website eindeutig ist, drücken Sie Releases und Verpacken, um sicherzustellen, dass die Kunden wissen, was beim Kauf Ihres Spiels zu erwarten ist Denken Sie daran, auch die Website und alle Vertriebskanäle für das Spiel barrierefrei zu gestalten. An wichtigsten ist jedoch, dass Sie die Community von Spielern mit Behinderungen über Ihr Spiel informieren.

## <a name="game-accessibility-features"></a>Barrierefreiheitsfeatures von Spielen

In diesem Abschnitt werden einige Features beschrieben, mit denen Sie die Barrierefreiheit Ihres Spiels verbessern können. Diese Features werden von Richtlinien abgeleitet, die von der Website zur [Spiel Barrierefreiheit Richtlinien](http://gameaccessibilityguidelines.com/) stammen. Diese Ressource stellt die Ergebnisse einer Zusammenarbeits Gruppe von Ateliers, Spezialisten und Akademikern dar.

### <a name="color-blind-friendly-graphics-and-user-interface"></a>Farb Blind freundliche Grafik und Benutzeroberfläche

Die Netzhaut des Auges besitzt zwei Arten von lichtempfindlichen Zellen: die Zapfen, um zu erkennen, wo Licht ist, und die Stäbchen, um bei schlechten Lichtbedingungen sehen zu können. 

Es gibt drei Arten von Zapfen (für Rot, Grün und Blau), um uns zu ermöglichen, Farben korrekt zu erkennen. Die Farb Blindheit tritt auf, wenn einer oder mehrere der drei Typen "lightcones" nicht erwartungsgemäß funktionieren. Der Grad der Farbenblindheit kann von fast normaler Farbwahrnehmung mit eingeschränkter Empfindlichkeit bis zum roten, grünen oder blauen Lichtreichen, bis eine vollständige Möglichkeit besteht, überhaupt keine Farbe wahrzunehmen. 

Da es weniger üblich ist, die Empfindlichkeit für das blaue Licht zu verringern, ist die Auswahl der Farben beim Entwerfen der farbenblind auf Personen ausgerichtet, die rot oder grün farbenblind sind:
 
  + Farbkombinationen verwenden, die von Personen mit roter oder grüner Farbenblindheit unterschieden werden können:
  
    * Ähnliche Farben: alle Abstufungen von Rot und Grün sowie von Braun und Orange
    * Farben, die sich abheben: Blau und Gelb
    
  + Verlassen Sie sich nicht ausschließlich auf Farben, um Spielobjekte zu kommunizieren oder zu unterscheiden. Verwenden Sie auch Formen und Muster.
  + Wenn Sie eigenständige Farben verwenden müssen, kombinieren Sie Voreinstellungen mit einer kostenlosen Auswahl von Farben, damit Sie von den Playern, die Sie benötigen, vollständig anpassbar sind und keine zusätzlichen Aufgaben für Spieler erstellen können, die Sie nicht benötigen.
  + Verwenden Sie einen Farb Blind Simulator, um Ihre Entwürfe zu testen, damit Sie Ihre Entwürfe mit farbenblind Augen anzeigen können. Dies kann Ihnen dabei helfen, häufige Kontrast Probleme zu vermeiden. [Color Oracle](https://www.colororacle.org) ist ein freier, Farben Linker Simulator, der die drei gängigsten Arten von Farb Sichtbarkeit simulieren kann – Deuteranopia, Protanopia und tritanopia.
  
### <a name="closed-captioning-and-subtitles"></a>Untertitelungen und Untertitel

Ziel des Designs von Untertitelungen und Untertiteln für Ihr Spiel ist die optionale Bereitstellung lesbarer Untertitel, damit Ihr Spiel auch ohne Audio gespielt werden kann. Es sollte möglich sein, Spielkomponenten wie Spieldialoge, Spielaudio und Soundeffekte als Text auf dem Bildschirm anzuzeigen.

Im Folgenden werden einige einfache Anleitungen aufgelistet, die Sie beim Design von Untertitelungen und Untertiteln berücksichtigen sollten:

*   Wählen Sie eine einfache, lesbare Schriftart.
*   Wählen Sie einen ausreichend großen Schriftgrad, oder stellen Sie eine Option für die Anpassung des Schriftgrads bereit, um größere Flexibilität zu bieten. (Der ideale Schriftgrad ist von der Bildschirmgröße, dem Abstand vom Bildschirm usw. abhängig.)
*   Schaffen Sie einen hohen Kontrast zwischen Hintergrund und Schriftfarbe. Verwenden Sie für den Text starke Gliederung und Schatten. Verwenden Sie für die Untertitel eine dunkle Hintergrund Überlagerung, und stellen Sie sicher, dass Sie Optionen für die Aktivierung aktivieren oder deaktivieren. (Weitere Informationen hierzu finden Sie unter [Informationen zum Kontrastverhältnis](../design/accessibility/accessible-text-requirements.md).)
* Zeigen Sie kurze Sätze auf dem Bildschirm an, maximal 38 Zeichen pro Zeile und maximal 2-3 Zeilen gleichzeitig. (Denken Sie daran, nicht zu viel über das Spiel zu verraten, indem Sie den Text anzeigen, bevor das Ereignis eintritt.)
*   Stellen Sie klar, aus welcher Quelle der Audioeffekt stammt oder wer gerade spricht. (Beispiel: „Daniel: Hallo!“.)
*   Bieten Sie die Möglichkeit, Untertitelungen und Untertitel ein- und auszuschalten. (Zusätzliches Feature: Bieten Sie die Möglichkeit an, anhand der Bedeutung auszuwählen, wie viele Audioinformationen angezeigt werden.)

### <a name="game-chat-transcription"></a>Aufzeichnung von Spiel Chats

Wenn Ihr Titel es gamten ermöglicht, mithilfe von Voice zu kommunizieren und Textnachrichten an einander zu senden, sollten die Funktionen "Text-zu-Sprache" und "Sprache-zu-Text" als Option verfügbar sein.

Personen, die nicht an Ihr Gaming-Gerät angehängte Mikrofone haben, können immer noch eine sprach Konversation mit jemandem haben, der spricht. Sie können Text in das Chatfenster eingeben und diese Nachrichten in Voice konvertieren lassen. Außerdem kann jemand, der nicht sehr gut hören kann, die Textnachrichten von der Person lesen, mit der er eine Stimme hat.

Für Entwickler im ID@Xbox -und Managed Partners-Programm stehen die Funktionen "Text-zu-Sprache" und "Sprache-zu-Text" im Rahmen der [Game Chat 2-Barrierefreiheits Funktionen](/gaming/xbox-live/multiplayer/chat/using-game-chat-2.md#accessibility) im Xbox Live-Dienst zur Verfügung. Weitere Informationen finden Sie in der [Übersicht über Game Chat 2](/gaming/xbox-live/multiplayer/chat/game-chat-2-overview.md).

### <a name="sound-feedback"></a>Audiofeedback

Audio- oder Soundeffekte bieten dem Spieler Feedback, zusätzlich zum visuellen Feedback. Ein gutes Audiodesign des Spiels kann die Barrierefreiheit für Spieler mit Sehschwächen verbessern. Im Folgenden werden einige Anleitungen aufgeführt, die Sie berücksichtigen sollten:

*   Verwenden Sie 3D-Audiohinweise, um zusätzliche räumliche Informationen bereitzustellen.
* Trennen Sie Musik-, Sprach- und Audio- bzw. Soundeffekte.
*   Gestalten Sie die Sprachhinweise so, dass sie den Spielern nützliche Informationen bieten. (Beispiel: „Die Feinde nähern sich“ im Gegensatz zu „Die Feinde nähern sich von der Hintertür aus“.)
*   Stellen Sie sicher, dass die Sprachhinweise mit einer akzeptablen Geschwindigkeit gesprochen werden, und ermöglichen Sie die Steuerung der Geschwindigkeit, um eine bessere Barrierefreiheit zu erzielen.

### <a name="fully-mappable-controls"></a>Vollständig zuordbare Steuerelemente

Es gibt Unternehmen und Organisationen wie z. B. [Special Effect](https://www.specialeffect.org.uk/), die benutzerdefinierte Steuergeräte für Spiele entwickeln, die mit verschiedenen Gamingsystemen wie Windows und Xbox One verwendet werden können. Diese Anpassung ermöglicht Benutzern mit unterschiedlichen Arten von Behinderungen, Spiele zu spielen, die sie andernfalls möglicherweise nicht spielen könnten. Weitere Informationen zu Personen, die nun mithilfe angepasster Steuergeräte eigenständig Spiele spielen können, finden Sie unter [Unterstützte Personen](https://www.specialeffect.org.uk/who-we-helped).

Als Spieleentwickler können Sie die Barrierefreiheit Ihres Spiels verbessern, indem Sie die vollständige Zuordbarkeit von Steuerelemente ermöglichen, damit Spieler ihre benutzerdefinierten Steuergeräte anschließen und die Tasten entsprechend ihren Bedürfnissen zuordnen können.

Vollständig mappbare Steuerelemente profitieren auch von Personen, die Standard Controller verwenden. Ihre Spieler können ein Layout entwerfen, das Ihren individuellen individuellen Anforderungen entspricht.

Sowohl die Xbox One-Standardsteuergeräte als auch die Xbox Elite-Steuergeräte ermöglichen die Anpassung der Steuergeräte, um ein präzises Gaming zu bieten. Um die Neuzuordnung vollständig zu überprüfen, __empfiehlt es sich, dass Entwickler die Neuzuordnung direkt im Spiel einschließen__. Weitere Informationen hierzu finden Sie unter [Xbox One](https://support.xbox.com/xbox-one/accessories/customize-standard-controller-with-accessories-app) und [Xbox Elite](https://support.xbox.com/xbox-one/accessories/use-accessories-app-configure-elite-controller).

### <a name="wider-selection-of-difficulty-levels"></a>Breitere Auswahl an Schwierigkeitsgraden

Videospiele bieten Unterhaltung. Die Herausforderung für Spieleentwickler besteht darin, den Schwierigkeitsgrad so anzupassen, dass den Spielern der richtige Grad an Herausforderungen geboten wird. Nicht alle Spieler besitzen die gleiche Geschicklichkeit und die gleichen Fähigkeiten, sodass die Entwicklung einer breiteren Auswahl an Schwierigkeitsoptionen die Chance verbessert, Spielern den richtigen Grad an Herausforderungen zu bieten. Gleichzeitig verbessert diese breitere Auswahl auch die Barrierefreiheit Ihres Videospiels, da so eine potenziell größere Zahl von Menschen mit Behinderungen Ihr Spiel spielen kann. Denken Sie daran, dass Spieler in einem Spiel Herausforderungen überwinden und dafür belohnt werden möchten. Sie möchten kein Spiel spielen, in dem sie nicht gewinnen können.

Die Anpassung der Schwierigkeitsgrade Ihres Spiels ist ein heikler Vorgang. Wenn Ihr Spiel zu einfach ist, sind die Spieler möglicherweise gelangweilt. Wenn es zu schwierig ist, geben die Spieler möglicherweise auf und spielen das Spiel nicht mehr. Der Ausgleich zwischen diesen beiden Extremen ist Kunst und Wissenschaft zugleich. Es gibt viele Möglichkeiten, ein Spiellevel mit dem richtigen Grad an Herausforderungen zu entwickeln. Einige Spiele bieten vereinfachte Eingaben wie Ein-Klick-Optionen, eine Rückspulungs- und Wiederholungsoption, um Fehler im Spiel leichter verzeihbar zu machen, oder bieten weniger und schwächere Feinde an, um es einfacher machen, im Spiel weiterzukommen, wenn mehrere Versuche gescheitert sind.

### <a name="photosensitivity-epilepsy-testing"></a>Tests auf Lichtempfindlichkeitsepilepsie

Bei der photosensitive-Epilepsie (PSE) handelt es sich um eine Bedingung, bei der die Anfälle durch visuelle Reize ausgelöst werden, einschließlich der Oberfläche für blinkende Beleuchtung oder bestimmte visuelle Formen und Muster Diese Erkrankung tritt bei ungefähr drei Prozent der Menschen auf und ist besonders bei Kindern und Jugendlichen verbreitet. Im Hinblick auf die Zahlen betrachten wir ungefähr [1 in 4000 Personen, die älter sind als 5-24](https://www.epilepsy.com/learn/professionals/about-epilepsy-seizures/reflex-seizures-and-related-epileptic-syndromes-0).

Es gibt viele Faktoren, die beim Spielen von Videospielen eine lichtempfindliche Reaktion auslösen können, darunter die Spieldauer, die Häufigkeit des aufblitzenden Lichts, die Intensität des Lichts, der Kontrast zwischen Hintergrund und Licht, der Abstand zwischen Bildschirm und Spieler sowie die Wellenlänge des Lichts.

Viele Leute erkennen, dass Sie durch eine Einnahme von Epilepsie verfügen. Spieler können Ihre ersten Anfälle durch Videogames erreichen, und dies kann zu einer physischen Verletzung führen. Als Entwickler finden Sie hier einige Tipps zum Entwerfen eines Spiels, um das Risiko von Angriffen zu verringern, die durch eine photosensitive Epilepsie verursacht werden.

Vermeiden Sie Folgendes:
* Blinken von Lichtern mit einer Häufigkeit von 5 bis 30 blinkten pro Sekunde (Hertz), da blinkende Beleuchtung in diesem Bereich am wahrscheinlichsten eine Beschlagnahme auslöst.
* Eine beliebige Sequenz von blinkenden Bildern, die länger als 5 Sekunden dauern
* Mehr als drei blinkte in einer Sekunde, die 25% + des Bildschirms abdecken
* Verschieben von wiederholten Mustern oder einheitlichen Text, abdecken von 25% + Bildschirm
* Statische wiederholte Muster oder einheitlicher Text, die 40% + des Bildschirms abdecken
* Eine sofortige hohe Änderung an Helligkeit/Kontrast (einschließlich schneller Schnitte) oder auf/von der Farbe Rot
* Mehr als fünf nicht gleichmäßig entfernte hohe Kontraste – Zeilen oder Spalten (z. b. Raster und Check Boards), die aus kleineren regulären Elementen wie z. b. Polkadots bestehen können
* Mehr als fünf Textzeilen, die nur als Großbuchstaben formatiert sind, bei denen es sich nicht um einen großen Abstand zwischen Buchstaben handelt, und Zeilen Abstände derselben Höhe wie die Zeilen selbst.

Verwenden Sie ein automatisiertes System, um das Spiel auf Reize zu überprüfen, die einen Anfall von Lichtempfindlichkeitsepilepsie auslösen können. (Beispiel: [der Harding-Test und der](https://www.hardingtest.com/index.php?page=test) [Harding Flash and Pattern Analyzer (BPA) G2](https://www.hardingfpa.com/harding-fpa-for-games/) wurden von Cambridge Research System Ltd und Professor Graham Harding entwickelt.) 

Fügen Sie **blinkende ein/aus** als Einstellungs Option ein, und legen Sie das **blinken** standardmäßig auf **Off** fest. Dabei schützen Sie Spieler, die noch nicht wissen, dass Sie anfällig für Beschlagnahmungen sind.

Planen Sie Pausen zwischen Spiellevels ein, damit Spieler nicht ohne Unterbrechung spielen.

## <a name="other-accessibility-resources"></a>Weitere Ressourcen für barrierefreie Spiele

Im Folgenden finden Sie einige externe Websites, die zusätzliche Informationen in Bezug auf barrierefreie Spiele bereitstellen.

### <a name="game-accessibility-guidelines"></a>Anleitungen für die Barrierefreiheit von Spielen
* [Richtlinien zur Spiel Barrierefreiheit](http://gameaccessibilityguidelines.com/) (als Referenz in diesem Thema verwendet)
* [Ablegamner Foundation Guidelines](https://accessible.games/accessible-player-experiences/) (als Referenz in diesem Thema verwendet)
* [Universell zugängliche Spiele (Universally Accessible (UA)-Spiele)](https://www.ics.forth.gr/hci/ua-games/index_main.php?l=e&c=555)

### <a name="custom-input-controllers"></a>Benutzerdefinierte Eingabesteuergeräte
* [Special Effect](https://www.specialeffect.org.uk/)
* [Warfighter Engaged](https://www.warfighterengaged.org/)

### <a name="other-references-used"></a>Andere verwendete Verweise
* [Color Blind Awareness, a Community Interest Company](https://www.colourblindawareness.org/colour-blindness/types-of-colour-blindness/)
* [Vorgehensweise bei Untertiteln &mdash; ein Blog Artikel zu Gamasutra von Ian Hamilton](https://www.gamasutra.com/blogs/IanHamilton/20150715/248571/How_to_do_subtitles_well__basics_and_good_practices.php)
* [Innovation for All Programme](https://www.inclusivedesign.no/practical-tools/definitions-article56-127.html)
* [Epilepsie Foundation](https://www.epilepsy.com/)

### <a name="related-links"></a>Verwandte Links
* [Inklusives Design](https://www.microsoft.com/design/inclusive/)
* [Microsoft Accessibility Developer Hub](https://developer.microsoft.com/windows/accessible-apps)
* [Entwickeln von barrierefreien UWP-Apps](../design/accessibility/accessibility.md)
* [E-Book: Engineering Software For Accessibility (Entwickeln von barrierefreier Software)](https://www.microsoft.com/download/details.aspx?id=19262)