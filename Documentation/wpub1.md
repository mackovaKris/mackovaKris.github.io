---
layout: page
title: WPUB zadanie 1
permalink: /documentation/wpub-1/

category: Documentation
category_navigation_order: 1
---
## [Zadanie 1: Osobná webová prezentácia na GitHub pages](https://wiki.fiit.stuba.sk/study/bc/info/wp/2017-18/zadanie1/) ##
Vytvorte webovú prezentáciu (webové sídlo) o sebe. Zamerajte sa jednak na vaše profesné záujmy (napr. projekty, ktoré riešite/riešili ste, čo vás v informatike najviac baví, fascinuje = váš developerský profil) a jednak vaše osobné záujmy, hobby.

V rámci developerského profilu vytvorte sekciu Webové publikovanie, kde budete publikovať všetky tri vaše vypracované zadania z predmetu.

Využite pritom technológie Git + GitHub Pages + Jekyll + Markdown. Využite potenciál statického generátora Jekyll a jeho templatovacích možností.

Podrobné požiadavky na vypracovanie a odovzdanie zadania (priemerná úroveň kvality):

* Sídlo musí obsahovať aspoň 5 podstránok, pri využití aspoň 3 rôznych rozložení (layout-ov)

* V rámci šablon musí byť použité:

    * aspoň 5 premenných
    * kolekcie alebo dátové súbory
    * aspoň 5 filtrov alebo tagov
    * aspoň 1 plugin (okrem pagination)

## Dokumentácia prvého zadania ##

Stránka na ktorej sa práve nachádzate predstavuje výstup tohto zadania. Obsahuje moje osobné aj akademické záujmy. S výnimkou stránok týkajúcich sa predmetu Webové publikovnie, je tento blog písaný v anglickom jazyku.

Projekt bol vytvorený z defaultnej témy poskytnutej jekyllom - [minima](https://github.com/jekyll/minima), štýlovanie menu bolo prevzaté z témy [minimal mistakes](https://mmistakes.github.io/minimal-mistakes/).

### Vonkajšia štruktúra ###
Štruktúra tejto stránky sa skladá z troch úrovní:
* hlavné menu
* kategórie vrámci položky menu
* blogové príspevky ktoré patria pod tému (topic) kategórie.

Táto štruktúra sa odzrkadluje na vnútornej štruktúre sídla, hlavne v adresáry Projects.

### Vnútorná štruktúra ###
```
.
├── _data
|   └── cv.html
├── _includes
|   ├── footer.html
|   ├── head.html
|   ├── header.html
|   ├── disqus_comments.html
|   ├── google-analytics.html
|   ├── pinned-pannel.html
|   └── social.html
├── _layouts
|   ├── category.html
|   ├── cv.html
|   ├── default.html
|   ├── home.html
|   ├── page.html
|   ├── post.html
|   └── presentation_page.html
├── _posts
|   └── 2018-03-09-saturday-evening.md
├── _sass
|   ├── minima
|   |   ├── _base.scss
|   |   ├── _layout.scss
|   |   └── _syntax-highlighting.scss
|   └── minima.scss
├── _site
├── About
|   └── about.md
├── assets
├── Documentation
|   ├── documentation.html
|   └── wpub1.md
├── Projects
|   ├── _posts
|   |   ├── 2018-03-09-game-flow-model.md
|   |   └── 2018-03-09-heor-turn-model.md
|   ├── ObjectScape
|   |   └── ObjectScape.md
|   ├── Sburb Tactics
|   |   └── Sburb Tactics
|   └── projects.html
├── script
└── index.md
├── 404.html
├── _config.md
```

V rámci projektu je použitých sedem rozložení: 
* default - obsahuje head, header a footer. Slúži ako parent pre ostatné rozloženia.
* home - hlavná stránka
* page - hlavné stránky kategórií
* post - pre blogové príspevky, rozdelené do topicov
* category - zoznam všetkých stránok (page) danej kategórie
* cv - slúži na výpís CV informácií, ktoré sú čerpané z dát ako YAML súbor.
* presentation_page - rozšírenie page, slúži na prezentáciu projektu

V rámci rozložení sa používa 7 include súborov
* disqus_coments - zabespečenie komentárav na vybraných stránkach (aktívne iba na github pages)
* google-analytics - služby google analytics (iba na github pages)
* footer
* head - tagy pre pluginy
* header - menu stránky
* social - odkazy na sociálne média vo footeri
* pinned-panel - pripnutý panel, zobrazuje sa na domovskej stránke

### Tabulka použitých prvkov ###

layout | premenné | tagy | filtre
------------ | ------------- | ------------- | ------
cv | main_navigation_order | for loop |
category | category, main_navigation_order | for loop, if, assign | sort 
page | category, category_navigation_order | -- | -- |
presentation_page | category, category_navigation_order, topic, list_title | for loop, if | where, sort,
post | comments, importance, topic | if, assign, include |  
site | author, reading, listening, playing |

### Datové súbory ###
Dátový súbor cv.yml obsahuje životopisné informácie, ktoré sú vypísané na podstránke /about.

### Pluginy ###
  - [jekyll-feed](https://github.com/jekyll/jekyll-feed)
  - [jekyll-seo-tag](https://github.com/jekyll/jekyll-seo-tag)
  - [jekyll-mentions](https://github.com/jekyll/jekyll-mentions) @jekyll
  - [jemoji](https://github.com/jekyll/jemoji) :fire::fire::fire: