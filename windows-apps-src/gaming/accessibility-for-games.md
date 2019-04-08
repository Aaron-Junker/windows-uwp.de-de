---
title: Erstellen barrierefreier Spiele
description: Erfahren Sie, wie Sie barrierefreie Spiele erstellen. Verwenden Sie das inklusive Spielentwurfsprinzip, um ein barrierefreies Spiel zu entwickeln.
ms.assetid: f5ba1e60-0d7c-11e6-91ec-0002a5d5c51b
ms.date: 11/09/2017
ms.topic: article
keywords: Windows 10, UWP, Bedienungshilfen, Spiele
ms.localizationpriority: medium
ms.openlocfilehash: 0e1d9d25fc63dd2fbb8d258fdaee692ccdfdc911
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592935"
---
#  <a name="making-games-accessible"></a>Erstellen barrierefreier Spiele

Eingabehilfen können jede Person und jedes Unternehmen auf der Welt dabei unterstützen, mehr zu erreichen. Dies gilt auch für die Verbesserung der Barrierefreiheit von Spielen. Dieser Artikel wurde für Spieleentwickler geschrieben, für Spieledesigner und Hersteller. Er enthält eine Übersicht über Anleitungen für die Entwicklung barrierefreier Spiele verschiedener Organisationen, die unten im Referenzabschnitt aufgelistet werden, und führt Sie in das inklusive Spielentwurfsprinzip ein, sodass Sie die Barrierefreiheit Ihrer Spiele verbessern können.

## <a name="gaming-for-everyone"></a>Gaming for Everyone

Wir von Microsoft sind überzeugt, dass Spiele Spaß für alle Benutzer bedeuten. Wir „möchten eine inklusive Umgebung für Spiele erstellen, die jeden anspricht. Wir glauben daran, dass wir für unsere Fans arbeiten und wir in der Art und Weise, in der wir verfügbar sin – innerhalb und außerhalb der Mauern von Microsoft – zeigen, wer wir sind. Wir haben das Programm entwickelt, um die unsere Unternehmenswerte darzustellen und glauben, dass das Programm dazu führet, positive Änderung durchzusetzen – nicht nur an unserem Arbeitsplatz, sondern in den Produkten, die wir für Spieler erstellen.” ([Blogbeitrag](https://blogs.microsoft.com/blog/2016/06/13/gaming-for-everyone) von Phil Spencer)

Wir möchten eine ansprechende, vielseitige und inklusive Umgebung erstellen, in denen jeder spielen kann. "Eine dauerhafte Auswirkung erfordert die Änderung der Kultur, die nicht über Nacht durchgeführt wird. Allerdings verpflichtet sich unser Team, täglich besser zu werden, unserer Entscheidungsfindung durchzudenken und die Vielfalt der Anforderungen, Fähigkeiten und Interessen der Spieler auf der ganzen Welt zu berücksichtigen." ([Blogbeitrag](https://blogs.microsoft.com/blog/2016/06/13/gaming-for-everyone) von Phil Spencer)

Wir hoffen, Sie folgen uns auf dem Weg, [Gaming for Everyone](https://news.microsoft.com/gamingforeveryone) zu verwirklichen. 

##  <a name="why-make-games-accessible"></a>Warum sollten Sie barrierefreie Spiele entwickeln?

### <a name="increased-gamer-base"></a>Größere Zahl von Spielern

Grundsätzlich betrachtet, ist es nicht schwierig, die Entwicklung barrierefreier Spiele geschäftlich zu begründen:

Sie multiplizieren die Zahl der Benutzer, die Ihr Spiel spielen können, mit der Qualität Ihres Spiels und erhalten so die Verkaufszahlen für Ihr Spiel.

Wenn Sie ein beeindruckendes Spiel entwickelt haben, das so kompliziert oder komplex ist, dass es nur von sehr wenigen Menschen gespielt werden kann, begrenzen Sie Ihre Verkaufszahlen. Und wenn Sie ein Spiel entwickeln, das von Menschen mit physischen, sensorischen oder kognitiven Behinderungen nicht gespielt werden kann, entgehen Ihnen ebenfalls mögliche Umsätze. Angesichts der Tatsache, dass [19 % der Bevölkerung in den Vereinigten Staaten über eine Art von Behinderung verfügen](https://www.census.gov/newsroom/releases/archives/miscellaneous/cb12-134.html), [ungefähr 14 % aller Erwachsener in den USA Probleme haben](https://nces.ed.gov/naal/estimates/overview.aspx) und [ungefähr 10 % der Männer über eine Form von Farbenblindheit verfügen](https://www.aao.org/eye-health/diseases/color-blindness-risk), kann dies potenziell einen großen Einfluss auf den Umsatz Ihres Titels haben. 

Weitere geschäftliche Begründungen finden Sie unter [Erstellen barrierefreier Videospiele](https://msdn.microsoft.com/library/windows/desktop/ee415219).

### <a name="better-games"></a>Bessere Spiele

Wenn Sie die Barrierefreiheit eines Spiels verbessern, kann dies dazu führen, dass das Spiel insgesamt besser wird. 

Ein Beispiel hierfür ist die Verwendung von Untertiteln in Spielen. In der Vergangenheit unterstützten Spiele nur selten Untertitel bzw. Untertitelungen für die Dialoge in Spielen. Heute wird erwartet, dass Spiele Untertitel bzw. Untertitelungen enthalten. Diese Änderung wurde nicht von Spielern mit Behinderungen verursacht. Sie geht auf die Lokalisierung zurück und wurde unter den Spielers populär, die es einfach bevorzugten, mit Untertiteln zu spielen, da so ihre Spielerfahrung verbessert wurde. Spieler aktivieren Untertitel bzw. Untertitelungen, wenn sie mit zu viel Hintergrundgeräuschen spielen; wenn sie Schwierigkeiten haben, Stimmen zu hören, wenn gleichzeitig mehrere Audioeffekte oder Umgebungsgeräusche abgespielt werden; oder wenn sie einfach nur die Lautstärke niedrig halten möchten, um andere nicht zu stören. Untertitel und Untertitelungen haben Spielern nicht nur geholfen, eine bessere Spielerfahrung zu erzielen, sondern ermöglichen auch Menschen mit beeinträchtigter Hörfähigkeit, Spiele zu spielen.

Die Neuzuordnung von Controllern ist ein weiteres Feature, das aus ähnlichen Gründen allmählich zum Standard in der Spielebranche wird. Es wird am häufigsten als einen Vorteil für alle Spieler angeboten. Einige Spieler genießen es, ihre Spielumgebungen anzupassen, und einige Spieler haben einfach lieber etwas anders, als das, was die Designer vorgesehen haben. Was die meisten Menschen nicht wissen, ist, dass die Möglichkeit, Tasten auf einem Eingabegerät neu zuzuordnen, auch eine Funktion ist, wodurch ein Spiel auch von Personen mit verschiedenen Behinderungen gespielt werden kann, die physisch nicht in der Lage sind oder nur schwer die Bereiche des Controllers bedienen können.

Letzten Endes führt das Nachdenken darüber, wie Sie die Barrierefreiheit Ihres Spiels verbessern können, häufig zu einem besseren Spiel, da Sie auf diese Weise eine Umgebung mit höherer Benutzerfreundlichkeit und besserer Anpassbarkeit für Ihre Spieler entwickeln.

### <a name="social-space-and-quality-of-life"></a>Soziale Interaktionen und Lebensqualität

Videospiele sind eine der erfolgreichsten Formen der Unterhaltung und Spiele können Stunden von Spaß bieten. Für einige ist Gaming jedoch nicht nur eine Form der Unterhaltung, sondern auch eine Möglichkeit, einem Bett im Krankenhaus, chronischen Schmerzen oder lähmenden sozialen Ängsten zu entkommen. Spieler werden in eine Welt transportiert, in denen sie zu den Hauptpersonen im Videospiel werden. Durch das Gaming können Sie einen sozialen Raum für sich selbst erstellen und in diesem agieren, der sie von ihren alltäglichen Problemen aufgrund ihrer Behinderung ablenkt und ihnen eine Möglichkeit bietet, mit Menschen zu kommunizieren, mit denen sie andernfalls möglicherweise nicht interagieren könnten. 

Gaming ist auch eine Kultur. An einem Spiel teilzunehmen, über das alle Ihre Freunde sprechen ist etwas, das sehr nützlich für die Lebensqualität verschiedener Personen sein kann.

##  <a name="is-the-game-you-are-making-today-accessible"></a>Ist das Spiel, das Sie heute entwickeln, barrierefrei?

Wenn Sie zum ersten Mal darüber nachdenken, Ihr Spiel barrierefrei zu machen, finden Sie im Folgenden einige Fragen, die Sie sich stellen sollten:

* Können Sie das Spiel mit nur einer Hand spielen? 
* Kann ein durchschnittlicher Benutzer das Spiel auswählen und sofort spielen?
* Können Sie das Spiel auf einem kleinen Bildschirm oder auf einem Fernseher aus einer gewissen Entfernung effektiv spielen?
* Unterstützen Sie mehr als eine Art von Eingabegerät, das während des gesamten Spiels verwendet werden kann?
* Können Sie das Spiel ohne Audio spielen?
* Können Sie das Spiel mit einem auf Schwarzweiß eingestellten Monitor spielen?
* Beim Laden Ihres letzten Spielstands nach einem Monat können Sie ganz einfach herausfinden, wo Sie im Spiel stehen und was für Ihren Fortschritt erforderlich ist.

Wenn Sie die meisten Fragen mit „nein“ beantwortet haben oder die Antwort auf die meisten Fragen nicht kennen, ist es an der Zeit, die Barrierefreiheit Ihres Spiels zu verbessern.

## <a name="defining-disability"></a>Definition von Behinderungen

Behinderungen werden als „fehlende Übereinstimmung zwischen den Bedürfnissen einer Person und dem Service, dem Produkt oder der Umgebung, die angeboten werden“ definiert. ([Inklusive Video](https://www.microsoft.com/design/inclusive), "Microsoft.com".) Dies bedeutet, dass jeder Benutzer eine Behinderung auftreten kann, und eine Bedingung kurz- oder erhalten werden. Stellen Sie sich die Herausforderungen vor, denen sich Spieler in einer derartigen Lage gegenübersehen, wenn sie Ihr Spiel spielen, und überlegen Sie, wie Sie Ihr Spiel für diese Personen verbessern können. Im Folgenden finden Sie einige Behinderungen, an die Sie denken sollten:

### <a name="vision"></a>Sehvermögen

*   Medizinische langfristige Bedingungen wie grüner Star, Katarakte, Farbblindheit, Kurzsichtigkeit und diabetesbedingte Retinopathie
*   Kurzfristige situationsbedingte Zustände wie ein kleiner Monitor oder Bildschirm, ein Bildschirm mit niedriger Auflösung oder ein Bildschirm, der aufgrund von hellen Lichtquellen wie der Sonne auf dem Monitor blendet
        
### <a name="hearing"></a>Hörvermögen

* Medizinische langfristige Bedingungen wie vollständige Taubheit oder teilweiser Hörverlust aufgrund von Krankheiten oder genetischen Ursachen
* Kurzfristige situationsbedingte Zustände wie übermäßige Hintergrundgeräusche, niedrige Qualität oder Audioqualität oder eingeschränkte Lautstärke, um andere nicht zu stören
        
### <a name="motor"></a>Motorische Fähigkeiten

* Medizinische langfristige Bedingungen wie Parkinson's Krankheit, amyotrophe Lateralsklerose (ALS), Arthritis und Muskeldystrophie
* Kurzfristige situationsbedingte Zustände wie eine verletzte Hand, Halten eines Getränks oder Tragen eines Kindes auf einem Arm
  
### <a name="cognitive"></a>Kognitive Fähigkeiten

* Medizinische langfristige Bedingungen wie Dyslexie, Epilepsie, Aufmerksamkeitsdefizitstörung (ADHD), Demenz und Amnesie
* Kurzfristige situationsbedingte Zustände wie Alkohol, Schlafmangel oder temporäre Ablenkungen wie Sirenen eines Einsatzfahrzeugs, das am Haus vorbeifährt

### <a name="speech"></a>Spracherkennung

* Medizinische langfristige Bedingen wie beschädigte Stimmbänder, Dysarthrie und Apraxie
* Kurzfristige situationsbedingte Zustände wie zahnärztliche Behandlungen oder Essen und Trinken


## <a name="how-to-make-games-more-accessible"></a>Wie können Sie die Barrierefreiheit von Spielen verbessern?

### <a name="design-shift-inclusive-game-design-approach"></a>Design-Shift: Spiele-Entwurfsansatz inklusive

Das inklusive Design konzentriert sich auf das Erstellen von Produkten und Diensten, die für ein breiteres Spektrum von Verbrauchern besser zugänglich sind, darunter auch Menschen mit Behinderungen.

Um erfolgreich zu sein, müssen die Spieledesigner von heute über die Entwicklung von unterhaltsamen Spielen hinaus denken, was Spaß macht. Spieledesigner müssen die Auswirkungen ihrer Designentscheidungen auf die allgemeine Barrierefreiheit des Spiels berücksichtigen, d. h. die Spielbarkeit des Spiels für ihre potenzielle Zielgruppe insgesamt, einschließlich Menschen mit Behinderungen.

Es muss daher ein Paradigmenwechsel vom herkömmlichen Spieledesign hin zum inklusiven Konzept für das Design von Spielen stattfinden. Das inklusive Spieledesign geht über das einfache Spieledesign hinaus, das der Zielgruppe Unterhaltung bietet. Es bedeutet, zusätzliche oder geänderte Personas zu entwickeln, um eine breitere Gruppe von Spielern anzusprechen. Sie müssen beim Entwerfen von Hindernissen in Ihrem Spiel nachdenken und sicher stellen, dass sie nicht unnötig Hindernisse hinzuzufügen, die den Spaß der vorgesehenen Umgebung mindern.

Indem Sie diese Lücken identifizieren, können Sie das ursprüngliche Designkonzept optimieren, überarbeiten und verbessern, damit mehr Menschen Ihre Vision entdecken. Wenn Sie sich Zeit nehmen und den inklusiven Designansatz für Ihr Spiel berücksichtigen, wird Ihr Spiel letzten Endes besser zugänglich. Kein Spiel gefällt jedem, die Definition eines Spiels umfasst eine gewisse Herausforderung, aber durch die Berücksichtigung der Barrierefreiheit können Sie sicherstellen, dass niemand unnötigerweise ausgeschlossen wird.

### <a name="empower-gamers-give-gamers-options"></a>Ermöglichen Sie es Spielern ermöglichen: Optionen für die es Spielern ermöglichen erteilen

Nahezu jede Eingabehilfen-Lösung hängt von zwei Prinzipien ab. Geben Sie als erstes Spielern Optionen, um ihre Spielumgebung anzupassen. Wenn Sie bereits eine großer Fanbasis haben, gibt es möglicherweise eine erhebliche Zahl von Spielern, die nicht möchten, dass die Umgebung auch nur minimal geändert wird. Das ist in Ordnung. Ermöglichen Sie Ihren Spielern, diese Features zu aktivieren und zu deaktivieren, und ermöglichen Sie die individuelle Konfiguration von Features. Sie müssen Spielern die Möglichkeit geben, ihre eigenen Anforderungen und Einstellungen am geeignetsten auszuprobieren.

### <a name="reinforce-communicate-information-in-more-than-one-way"></a>Verstärken: Informationen in mehr als eine Möglichkeit zu kommunizieren

Das zweite Prinzip umfasst das Konzept des universellen Designs in einem einheitlichen Ansatz, der nicht nur mehr Spieler anzieht sondern auch die Benutzerfreundlichkeit für alle verbessert. Beispielsweise, ein Bild mit Text, ein Symbol mit Farbe. Eine Karte, die auf eine Reihe von verschiedenfarbigen Markern basiert ist nicht nur für farbenblinde Spieler unmöglich, sondern auch frustrierend für alle anderen Benutzer, die sich alles entsprechend merken müssen. Das Hinzufügen von Symbolen ist optimal für alle Benutzer.

### <a name="innovate-be-creative"></a>Innovation: Seien Sie kreativ

Es gibt viele kreative Möglichkeiten, um die Barrierefreiheit Ihres Spiels zu verbessern. Werden Sie kreativ, und lernen Sie von anderen barrierefreien Spielen auf dem Markt. Wenn Sie bereits ein Spiel entwickelt haben, sollten Sie versuchen, aktuelle Features Ihres Spiels zu identifizieren, die verbessert werden können, ohne das ursprüngliche Design der zentralen Spielmechanismen und der Spielerfahrung zu verändern. Wie bereits erwähnt, geht es bei der Barrierefreiheit von Spielen darum, Spielern Optionen für die Anpassung ihrer Spielerfahrung bereitzustellen. Dies kann durch das Unterstreichen oder die Weitergabe von Informationen auf mehre Arten geschehen. 

Sehen Sie die Barrierefreiheit als Ansatz für ein neues Design und möglicherweise Ideen, an die Sie nicht anderweitig gedacht hätten. Dieser Ansatz zum Entwurf ergibt nicht nur interessante Konzepte sondern Produkte für die weitere Verbreitung oder die Massenvermarktung für den wirtschaftlichen Erfolg. Beispiele umfassen die Textvorhersage, Spracherkennung, weniger Beschränkungen, die Lautsprecher, die Schreibmaschine und optische Zeichenerkennung (OCR). Ideen für diese Produkte stammen von Personen, die die Herangehensweise für Lösungen für die Eingabehilfen überdacht haben.

### <a name="adopt-quality-means-accessible-features"></a>Übernehmen: Qualität bedeutet zugänglich features

Barrierefreiheit bedeutet Qualität. Es muss eine Funktionsanforderung sein und nicht nur ein funktionierendes Element. "Anpassbarer Minikarte für Farbenblindheit" gilt beispielsweise nicht als Arbeitsaufgabe mit niedriger Priorität, das Sie einführen, wenn Sie zusätzliche Zeit haben. Wenn dieses Element nicht abgeschlossen ist, bedeutet dies einfach, dass das gesamte Minikarten-Feature nicht vollständig ist und nicht versendet werden kann.

### <a name="evangelize-make-accessibility-a-priority-in-your-game-studio"></a>Übertragen: Stellen Sie Zugriff auf eine Priorität in Ihrer game studio

Für die Spieleentwicklung gibt es stets einen engen Terminplan. Wenn Sie die Barrierefreiheit zur Priorität erklären, wird es einfacher. Eine Möglichkeit besteht darin, das Spiel von Anfang an im Gedanken an Barrierefreiheit zu entwerfen. Je früher Sie die Eingabehilfen berücksichtigen, desto einfacher und kostengünstiger wird die Arbeit. 

Teilen Sie Ihr Wissen über Barrierefreiheit mit Ihrem Team, Ihre geschäftlichen Gründe und zersträuen Sie Missverständnisse – dass viele Benutzer davon nicht profitieren, es die Mechanik hindert und schwierig und teuer zu implementieren ist.

### <a name="review-constantly-evaluate-your-game"></a>Lesen Sie: Bewerten Sie Ihr Spiel ständig

Sie können einen Überprüfungsprozess während der Entwicklung einführen, um sicherzustellen, dass die Barrierefreiheit bei jedem Schritt berücksichtigt wird. Erstellen Sie eine Checkliste wie die unten gezeigte, um Ihrem Team zu helfen, laufend zu überprüfen, ob die entwickelten Features barrierefrei sind.

| Prüfliste                                         | Barrierefreiheitsfeatures                                                                                                         |
|---------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| Kinoeffekte im Spiel                                | Hat Untertitel und Untertitelungen, ist auf Fotosensibilität getestet                                                                           |
| Grafiken im Allgemeinen (2D- und 3D-Grafiken)              | Für farbenblinde Spieler geeignete Farben und Optionen, keine ausschließliche Abhängigkeit von Farben für die Identifizierung, sondern auch von Formen und Mustern|
| Startbildschirm, Einstellungsmenü und andere Menüs       | Möglichkeit, Optionen vorzulesen, Möglichkeit, Einstellungen zu merken, alternative Eingabemethode für Befehlssteuerung, anpassbarer UI-Schriftgrad  |
| Spielplan                                          | Umfassend anpassbarere Schwierigkeitsgrade, Untertitel und Untertitelungen, gutes visuelles und Audiofeedback für die Spieler                           |
| HUD-Anzeige                                       | Anpassbare Bildschirmposition, anpassbarer Schriftgrad, Einstellungen für farbenblinde Spieler                                                  |        
| Steuerelementeingabe                                     | Möglichkeit, Steuerelemente dem Eingabegerät zuzuordnen, benutzerdefinierte Controller-Unterstützung, Zulässigkeit vereinfachter Eingaben im Spiel                               |        

### <a name="playtest-and-iterate-get-gamers-feedback"></a>Playtest und durchlaufen: Erhalten Sie Feedback von Spielern

Laden Sie Spieletester mit Behinderungen, für die bei der Spieleentwicklung berücksichtigt wurden, zu den Spieletests ein. Denken Sie daran, Fragen zur Eingabehilfe in die Beta-Tests Fragebögen hinzuzufügen. Lokale Behindertengruppen sind eine hervorragende Informationsquelle als Teilnehmer. Achten Sie darauf, wie sie spielen, und bitten Sie sie um Feedback. Stellen Sie fest, welche Änderungen vorgenommen werden müssen, um das Spiel zu verbessern.

Verwenden Sie soziale Netzwerke und das Forum des Spiels, um Informationen über die Eingabehilfefunktionen zu erhalten, die am wichtigsten sind und wie sie implementiert werden sollten. 

### <a name="shout-it-out-let-the-world-know-your-game-is-accessible"></a>Rufen Sie es heraus: Ermöglichen der ganzen Welt wissen, dass Ihr Spiel zugegriffen werden.

Verbraucher möchten wissen, ob Ihr Spiel von Spielern mit Behinderungen gespielt werden kann. Geben Sie die Barrierefreiheit des Spiels auf der Website des Spiels und über Pressemitteilungen und auf der Verpackung an, um sicherzustellen, dass Verbraucher beim Kauf Ihres Spiels wissen, was sie erwarten können. Denken Sie daran, auch die Website und alle Vertriebskanäle für das Spiel barrierefrei zu gestalten. An wichtigsten ist jedoch, dass Sie die Community von Spielern mit Behinderungen über Ihr Spiel informieren.

## <a name="game-accessibility-features"></a>Barrierefreiheitsfeatures von Spielen

In diesem Abschnitt werden einige Features beschrieben, mit denen Sie die Barrierefreiheit Ihres Spiels verbessern können. Diese Features sind den [Anleitungen für die Barrierefreiheit von Spielen](https://gameaccessibilityguidelines.com/) entnommen, die die Ergebnisse der Zusammenarbeit einer Gruppe von Studios, Spezialisten und Hochschullehrern darstellen. Weitere Informationen finden Sie unter [Anleitungen für die Barrierefreiheit von Spielen](https://gameaccessibilityguidelines.com/). 

### <a name="colorblind-friendly-graphics-and-user-interface"></a>Für farbenblinde Personen geeignete Grafiken und Benutzeroberflächen

Die Netzhaut des Auges besitzt zwei Arten von lichtempfindlichen Zellen: die Zapfen, um zu erkennen, wo Licht ist, und die Stäbchen, um bei schlechten Lichtbedingungen sehen zu können. 

Es gibt drei Arten von Zapfen (für Rot, Grün und Blau), um uns zu ermöglichen, Farben korrekt zu erkennen. Farbenblindheit tritt auf, wenn mindestens einer dieser drei Arten von lichtempfindlichen Zapfen nicht wie erwartet funktioniert. Der Grad der Farbenblindheit kann von einer fast normalen Farbwahrnehmung mit reduzierter Empfindlichkeit für rotes, grünes oder blaues Licht bis zur völligen Unfähigkeit, Farben wahrzunehmen, reichen. 

Da eine reduzierte Empfindlichkeit für blaues Licht seltener ist, werden bei der Entwicklung für farbenblinde Spieler vor allem Personen berücksichtigt, die unempfindlich für rotes oder grünes Licht sind:
 
  + Verwenden Sie Farbkombinationen, die von Benutzern unterschieden werden können, die rotes/grünes Licht nicht wahrnehmen können:
  
    * Farben, die ähnlich wie angezeigt werden: Alle grauschattierungen Rot-Grün einschließlich Brown "und" Orange "
    * Farben, die herausstellen: Blau und Gelb
    
  + Verlassen Sie sich nicht ausschließlich auf Farben, um zu kommunizieren oder um Spielobjekte zu unterscheiden. Verwenden Sie ebenfalls Formen und Muster.
  + Wenn Sie sich nur auf Farben verlassen, gruppieren Sie die Einstellungen mit einer kostenlosen Auswahl an Farben, damit das Spiel vollständig durch den Spieler angepasst werden kann, der diese benötigt, und keine zusätzliche Arbeit für Spieler darstellt, die diese nicht benötigen.
  + Verwenden Sie einen Simulator für Sehschwächen, um Ihre Entwürfe zu testen, damit Sie diese mit den Augen einer Person mit Sehschwäche anzeigen können. Dadurch können Sie Kontrastprobleme vermeiden. [Color Oracle](https://www.colororacle.org) ist ein kostenloser Simulator für farbenblinde Personen, der die drei am häufigsten verwendeten Arten von Farbsehschwäche – Rotgrünblindheit, Protanopia und Tritanopia – simulieren kann.
  
### <a name="closed-captioning-and-subtitles"></a>Untertitelungen und Untertitel

Ziel des Designs von Untertitelungen und Untertiteln für Ihr Spiel ist die optionale Bereitstellung lesbarer Untertitel, damit Ihr Spiel auch ohne Audio gespielt werden kann. Es sollte möglich sein, Spielkomponenten wie Spieldialoge, Spielaudio und Soundeffekte als Text auf dem Bildschirm anzuzeigen.

Im Folgenden werden einige einfache Anleitungen aufgelistet, die Sie beim Design von Untertitelungen und Untertiteln berücksichtigen sollten:

*   Wählen Sie eine einfache, lesbare Schriftart.
*   Wählen Sie einen ausreichend großen Schriftgrad, oder stellen Sie eine Option für die Anpassung des Schriftgrads bereit, um größere Flexibilität zu bieten. (Der ideale Schriftgrad ist von der Bildschirmgröße, dem Abstand vom Bildschirm usw. abhängig.)
*   Schaffen Sie einen hohen Kontrast zwischen Hintergrund und Schriftfarbe. Verwenden Sie starke Gliederungen und Schatten für den Text. Verwenden Sie eine dunkle Hintergrundüberlagerung für die Untertitel, und denken Sie daran, Optionen für das Aktivieren und Deaktivieren bereitzustellen. (Weitere Informationen hierzu finden Sie unter [Informationen zum Kontrastverhältnis](https://msdn.microsoft.com/windows/uwp/accessibility/accessible-text-requirements).)
* Zeigen Sie kurze Sätze auf dem Bildschirm an, maximal 38 Zeichen pro Zeile und maximal 2 bis 3 Zeilen zur gleichen Zeit. (Denken Sie daran, nicht zu viel über das Spiel zu verraten, indem Sie den Text anzeigen, bevor das Ereignis eintritt.)
*   Stellen Sie klar, aus welcher Quelle der Audioeffekt stammt oder wer gerade spricht. (Beispiel: "Daniel: Hallo!")
*   Bieten Sie die Möglichkeit, Untertitelungen und Untertitel ein- und auszuschalten. (Zusätzliche Features: Möglichkeit, auszuwählen, wie viel fundierter Informationen angezeigt werden basiert Bedeutung.)

### <a name="game-chat-transcription"></a>Spiele-Chat-Transkription

Wenn Spieler in Ihrem Spieletitel über die Spracheingabe und SMS-Nachrichten miteinander kommunizieren können, sollte die Funktion Text-zu-Sprache und Spracherkennung-zu-Text als Option verfügbar sein.

Spieler, die kein Mikrofon mit ihren Gaming-Gerät verbunden haben, können trotzdem ein Gespräch mit einer anderen Person haben. Sie können Text in das Chatfenster eingeben und diese Nachrichten in Stimmnachrichten konvertieren. Dies ermöglicht einem Spieler, der nicht gut hört, das transkribierte Lesen von SMS von der Person, mit der sie einen Voice-Chat hat.

Entwicklern in ID@Xbox und dem verwaltetem Partner-Programm stehen die Funktionen Text-zu-Sprache und Sprache-zu-Text als Teil der [Eingabehilfen für Spiele-Chat 2](../xbox-live/multiplayer/chat/using-game-chat-2.md#accessibility) im Xbox Live-Dienst zur Verfügung. Weitere Informationen finden Sie unter [Übersicht über Spiele-Chat 2](../xbox-live/multiplayer/chat/game-chat-2-overview.md).

### <a name="sound-feedback"></a>Audiofeedback

Audio- oder Soundeffekte bieten dem Spieler Feedback, zusätzlich zum visuellen Feedback. Ein gutes Audiodesign des Spiels kann die Barrierefreiheit für Spieler mit Sehschwächen verbessern. Im Folgenden werden einige Anleitungen aufgeführt, die Sie berücksichtigen sollten:

*   Verwenden Sie 3D-Audiohinweise, um zusätzliche räumliche Informationen bereitzustellen.
* Trennen Sie Musik-, Sprach- und Audio- bzw. Soundeffekte.
*   Gestalten Sie die Sprachhinweise so, dass sie den Spielern nützliche Informationen bieten. (Beispiel: "Feinde nähern" im Vergleich zu "Feinde werden von Hintertür eingeben.")
*   Stellen Sie sicher, dass die Sprachhinweise mit einer akzeptablen Geschwindigkeit gesprochen werden, und ermöglichen Sie die Steuerung der Geschwindigkeit, um eine bessere Barrierefreiheit zu erzielen.

### <a name="fully-mappable-controls"></a>Vollständig zuordbare Steuerelemente

Es gibt Unternehmen und Organisationen wie z. B. [Special Effect](https://www.specialeffect.org.uk/), die benutzerdefinierte Steuergeräte für Spiele entwickeln, die mit verschiedenen Gamingsystemen wie Windows und Xbox One verwendet werden können. Diese Anpassung ermöglicht Benutzern mit unterschiedlichen Arten von Behinderungen, Spiele zu spielen, die sie andernfalls möglicherweise nicht spielen könnten. Weitere Informationen zu Personen, die nun mithilfe angepasster Steuergeräte eigenständig Spiele spielen können, finden Sie unter [Unterstützte Personen](https://www.specialeffect.org.uk/who-we-helped).

Als Spieleentwickler können Sie die Barrierefreiheit Ihres Spiels verbessern, indem Sie die vollständige Zuordbarkeit von Steuerelemente ermöglichen, damit Spieler ihre benutzerdefinierten Steuergeräte anschließen und die Tasten entsprechend ihren Bedürfnissen zuordnen können.

Vollständig zuordbare Steuerelemente sind auch von Vorteil für Personen, die Standard-Controller verwenden. Ihre Spieler können ein Layout erstellen, das ihren eindeutigen Wünschen individuell entspricht.

Sowohl die Xbox One-Standardsteuergeräte als auch die Xbox Elite-Steuergeräte ermöglichen die Anpassung der Steuergeräte, um ein präzises Gaming zu bieten. Um die Neuzuordnungsfunktionen vollkommen zu verwenden, __wird empfohlen, dass Entwickler das Neuzuordnen direkt im Spiel miteinbeziehen__. Weitere Informationen hierzu finden Sie unter [Xbox One](https://support.xbox.com/xbox-one/accessories/customize-standard-controller-with-accessories-app) und [Xbox Elite](https://support.xbox.com/xbox-one/accessories/use-accessories-app-configure-elite-controller).

### <a name="wider-selection-of-difficulty-levels"></a>Breitere Auswahl an Schwierigkeitsgraden

Videospiele bieten Unterhaltung. Die Herausforderung für Spieleentwickler besteht darin, den Schwierigkeitsgrad so anzupassen, dass den Spielern der richtige Grad an Herausforderungen geboten wird. Nicht alle Spieler besitzen die gleiche Geschicklichkeit und die gleichen Fähigkeiten, sodass die Entwicklung einer breiteren Auswahl an Schwierigkeitsoptionen die Chance verbessert, Spielern den richtigen Grad an Herausforderungen zu bieten. Gleichzeitig verbessert diese breitere Auswahl auch die Barrierefreiheit Ihres Videospiels, da so eine potenziell größere Zahl von Menschen mit Behinderungen Ihr Spiel spielen kann. Denken Sie daran, dass Spieler in einem Spiel Herausforderungen überwinden und dafür belohnt werden möchten. Sie möchten kein Spiel spielen, in dem sie nicht gewinnen können.

Die Anpassung der Schwierigkeitsgrade Ihres Spiels ist ein heikler Vorgang. Wenn Ihr Spiel zu einfach ist, sind die Spieler möglicherweise gelangweilt. Wenn es zu schwierig ist, geben die Spieler möglicherweise auf und spielen das Spiel nicht mehr. Der Ausgleich zwischen diesen beiden Extremen ist Kunst und Wissenschaft zugleich. Es gibt viele Möglichkeiten, ein Spiellevel mit dem richtigen Grad an Herausforderungen zu entwickeln. Einige Spiele bieten vereinfachte Eingaben wie Ein-Klick-Optionen, eine Rückspulungs- und Wiederholungsoption, um Fehler im Spiel leichter verzeihbar zu machen, oder bieten weniger und schwächere Feinde an, um es einfacher machen, im Spiel weiterzukommen, wenn mehrere Versuche gescheitert sind.

### <a name="photosensitivity-epilepsy-testing"></a>Tests auf Lichtempfindlichkeitsepilepsie

Lichtempfindlichkeitsepilepsie (PSE) ist eine Erkrankung, bei der durch visuelle Reize wie aufblitzendes Licht oder bestimmte sich bewegende visuelle Formen und Muster Anfälle ausgelöst werden. Diese Erkrankung tritt bei ungefähr drei Prozent der Menschen auf und ist besonders bei Kindern und Jugendlichen verbreitet. Wir haben ungefähr [1 Person unter 4000 Personen im Alter von 5 bis 24 Jahren](https://www.epilepsy.com/information/professionals/about-epilepsy-seizures/reflex-seizures-and-related-epileptic-syndromes-3).

Es gibt viele Faktoren, die beim Spielen von Videospielen eine lichtempfindliche Reaktion auslösen können, darunter die Spieldauer, die Häufigkeit des aufblitzenden Lichts, die Intensität des Lichts, der Kontrast zwischen Hintergrund und Licht, der Abstand zwischen Bildschirm und Spieler sowie die Wellenlänge des Lichts.

Viele Benutzer finden heraus, dass sie Epilepsie haben, wenn sie einen epileptischen Anfall haben. Spieler können, und haben, ihre ersten Anfälle über Videospiele, was zum Schaden der Person führen kann Als Entwickler finden Sie hier einige Tipps für das Entwerfen eines Spiels, das auch von Spielern gespielt werden kann, die zur Lichtempfindlichkeitsepilepsie neigen.

Vermeiden Sie Folgendes:
* Licht, das mit einer Frequenz von 5 bis 30 Blitzen pro Sekunde (Hertz) aufblitzt, da blitzendes Licht in diesem Bereich am wahrscheinlichsten Anfälle auslöst.
* Licht, das über eine Frequenz von 5 Blitzen pro Sekunde dauert
* Mehr als drei Blitze in einer einzigen Sekunde über mehr als 25 % des Bildschirms
* Verschieben wiederholter Muster oder uniformen Texts über mehr als 25 % des Bildschirms
* Statisch wiederholte Muster oder uniformer Text über mehr als 40 % des Bildschirms
* Eine unmittelbare hohe Änderung in Helligkeit/Kontrast (einschließlich schneller Schnitte) oder in der Farbe Rot
* Mehr als fünf gleichmäßige wiederholte Streifen mit hohem Kontrast – Zeilen oder Spalten wie Raster und Schachbrettmuster, die aus kleineren Elementen wie z. B. Tupfen zusammengesetzt sind
* Mehr als fünf Zeilen Text in Großbuchstaben ohne viel Abstand zwischen den Buchstaben und Zeilenabstände, die genauso hoch wie die Zeilen selbst sind, effektives Umwandeln in hohen Kontrast mit gleichmäßig alternieRendern Zeilen

Verwenden Sie ein automatisiertes System, um das Spiel auf Reize zu überprüfen, die einen Anfall von Lichtempfindlichkeitsepilepsie auslösen können. (Beispiel: [Die Harding Test](https://www.hardingtest.com/index.php?page=test) und [Harding Flash und Muster Analyzer (FPA) G2](https://www.hardingfpa.com/harding-fpa-for-games/) von Cambridge Research System Ltd. und Professor Graham Harding entwickelt wurde.) 

Fügen Sie **ein- und ausblenden** als Einstellung hinzu und legen Sie **Blitz** standardmäßig auf **Aus** fest. Dadurch schützen Sie Spieler, die noch nicht wissen, dass sie für Anfälle anfällig sind.

Planen Sie Pausen zwischen Spiellevels ein, damit Spieler nicht ohne Unterbrechung spielen.

## <a name="other-accessibility-resources"></a>Weitere Ressourcen für barrierefreie Spiele

Im Folgenden finden Sie einige externe Websites, die zusätzliche Informationen in Bezug auf barrierefreie Spiele bereitstellen.

### <a name="game-accessibility-guidelines"></a>Anleitungen für die Barrierefreiheit von Spielen
* [Richtlinien für Spiele Eingabehilfen](https://gameaccessibilityguidelines.com/)
* [AbleGamers Foundation-Richtlinien](https://www.includification.com/)
* [Entwerfen Sie allgemein verfügbar (UA)-Spiele](https://www.ics.forth.gr/hci/ua-games/index_main.php?l=e&c=555)

### <a name="custom-input-controllers"></a>Benutzerdefinierte Eingabesteuergeräte
* [Spezieller Effekt](https://www.specialeffect.org.uk/)
* [War Kämpfer aktiviert](https://www.warfighterengaged.org/)

## <a name="references-used"></a>Verwendete Quellen
* [Richtlinien für Spiele Eingabehilfen](https://gameaccessibilityguidelines.com/)
* [AbleGamers Foundation-Richtlinien](https://www.includification.com/)
* [Farbe, die Blind Sensibilisierung können Unternehmen Interesse-Community](https://www.colourblindawareness.org/colour-blindness/types-of-colour-blindness/)
* [Wie Sie Untertitel gut – einen Blog-Artikel auf Gamasutra von Ian Hamilton](https://www.gamasutra.com/blogs/IanHamilton/20150715/248571/How_to_do_subtitles_well__basics_and_good_practices.php)
* [Innovationen für alle Programms](https://www.inclusivedesign.no/practical-tools/definitions-article56-127.html)
* [Leiden, Anfälle auslösen foundation](https://www.epilepsy.com/)

## <a name="related-links"></a>Verwandte Links
* [Inklusive Entwurf](https://www.microsoft.com/design/inclusive)
* [Microsoft Accessibility Developer Hub](https://developer.microsoft.com/windows/accessible-apps)
* [Entwickeln von UWP-apps zugegriffen werden kann](https://msdn.microsoft.com/windows/uwp/accessibility/accessibility)
* [E-Book Engineering-Software für Barrierefreiheit](https://www.microsoft.com/download/details.aspx?id=19262)
