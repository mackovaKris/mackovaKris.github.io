---
layout: page
title: WPUB zadanie 1
permalink: /documentation/wpub-1/

category: Documentation
category_navigation_order: 1
---

V rámci projektu je použitých šesť rozložení: 
* default - obsahuje head, header a footer. Slúži ako parent pre ostatné rozloženia.
* home - hlavná stránka
* page - hlavné stránky kategórií
* post 
* category - zoznam všetkých stránok (page) danej kategórie
* cv - slúži na výpís CV informácií, ktoré sú čerpané z dát ako YAML súbor.

Projekt bol vytvorený z defaultnej témy poskytnutej jekyllom - [minima](https://github.com/jekyll/minima) a dodatočné grafické štýlovanie bolo prevzaté z témy [minimal mistakes](https://mmistakes.github.io/minimal-mistakes/).

Použité prvky:
* Rozloženie category - prechádza cez všetky stránky (pages), hladá také ktoré majú zadefinovanú premennú *category*
a *category_navigation_order*. Stránky ktoré patria do danej kategórie zoradí podla *category_navigation_order* a vypíše ich ano zoznam odkazov.

* Rozloženie cv - pracuje s dátami *_data/cv.yml*. Súbor *cv.yml* obsahuje životopisné dáta, ku ktorým sa rozloženie dostáva cez *site.data.cv*.

### Tabulka použitých prvkov ###

layout | premenné | tagy | filtre
------------ | ------------- | ------------- | ------
cv | title, permalink, main_navigation_order, site.data.cv, section.name, section.list | for loop | escape 
category | title, permalink, cat.category_navigation_order, category, site.pages, cat.category, page.category | for loop, if, assign | sort 
page | title, permalink, category, category_navigation_order | -- | escape