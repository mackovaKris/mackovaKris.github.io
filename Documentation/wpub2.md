---
layout: page
title: WPUB zadanie 2
permalink: /documentation/wpub-2/

category: Documentation
category_navigation_order: 2
---
## [Zadanie 2: Transformácia vybraného dokumentu do formátu DocBook](https://wiki.fiit.stuba.sk/study/bc/info/wp/2017-18/zadanie2/) ##
Predmetom 2. zadania je spracovanie vybraného dokumentu (ideálne bakalárskeho projektu) z pôvodného ľubovoľného (Word, OpenOffice, LaTeX, …) formátu do formátu DocBook a vygenerovanie cieľového tvaru v PDF. Výsledný dokument bude mať rozsah minimálne 10 a maximálne 15 strán. Do rozsahu sa nezapočítavajú úvodné strany (obsah, zoznamy obrázkov a tabuliek), použitá literatúra a prílohy.

Požadované a kontrolné konštrukcie sú:

* štandardné členenie textu na kapitola, podkapitola, podpodkapitola, príloha, generovaný obsah,

* V rámci šablon musí byť použité:

* zvýraznenie slov, zvýraznenie členenia textu odrážkami alebo číslovaním,
* odkazy na iné časti vlastného dokumentu, prípadne odkazy na URL,
* poznámka pod čiarou,
* zoznam použitej literatúry a zdrojov vrátane ich citácie v texte,
* vloženie obrázku a tabuliek, odkazy na ne v texte; zoznam obrázkov a tabuliek v úvode alebo závere textu,
* vytvorenie registra pojmov (indexu) s pojmami hierarchicky usporiadanými do dvoch úrovni, napríklad „cykly, while“, „cykly, for“ (najmenej ako ukážku na 10-15 pojmoch na predvedenie práce s registrom).

## Dokumentácia druhého zadania ##
Výstupom práce na druhom zadaní sú upravené šablóny thesis.xsl, thesis-tp-xsl a xml formát mojej bakálárskej práce spolu s vygenerovaným pdf dokumentom.

### Použité elementy ###
* členenie textu na kapitoly a podkapitoly pomocou chapter, section, para,
* emphasis na zvýraznenie textu
* členenie na odrážky pomocou itemizedlist a listitem, v dokumente tiež použitý vnorene
* odkazy na iné časti textu pomocou xref a atribútom linkend
* poznámky pod čiarov sú prvky footnote s prvkom ulink na url odkazy
* citácie sú tvorené ako xref odkazy na zaznamy v bibliografií
	* bibliografia je obalena tagom bibliography
	* jednotlivé záznamy sú biblioentry
	* väčšina záznamov sú bibliosety (article a journal), s:
		* title
		* authorgroup a author
		* copyright
		* pagenums
		* publisher
* obrázky sú figure s title, titleabbrev na odkazovanie v texte, mediaobject, imageobject a imagedata,
* tabulka table s tgroup, colspec, thead, tbody, row a entries
* register je generovaný tagom index, pojmy v indexe sú vytvorené tagmi indexterm, primary, secondary a tertiary,

### Zmeny v šablóne	###
*mojabp.xml
	* zmena jazyku z cs na sk
	* odstránenie nepodporovaných znakov
* thesis.xsl
	* pridanie figure,table do generate.toc aby sa generoval zoznam figúr a tabuliek
* thesis-tp-fo.xsl
	* prepísanie "vedoucí" na "vedúci"
	* pridanie ```hyphenate=”false”``` podľa návodu na stránke predmetu
	* odstránenie watermark obrázka z book.titlepage.before.recto
