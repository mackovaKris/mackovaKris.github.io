---
layout: page
title: WPUB zadanie 3
permalink: /documentation/wpub-3/

category: Documentation
category_navigation_order: 3
---
## [Zadanie 3: XML prezentácia](https://wiki.fiit.stuba.sk/study/bc/info/wp/2017-18/zadanie3/) ##
Analyzujte možnosti zápisu jednoduchej prezentácie v jazyku XML. Identifikujte základné súčasti prezentácie a navrhnite XML elementy pre ich označkovanie (metadátové, štrukturálne, inline). Dbajte na znovupoužiteľnosť a vyvarujte sa redundancií. Návrh elementov zrealizujte opísaním typu dokumentu pomocou vybraného jazyka (DTD, XSD, RELAX NG) spolu s vysvetlením účelu jednotlivých elementov. Vo vami navhrnutom jazyku vytvorte ukážkovú prezentáciu, ktorá bude demonštrovať možnosti tvorby prezentácií podľa definície typu dokumentu.

Navrhnite a vytvorte XSLT šablóny pre konverziu prezentácie z XML do XHTML+CSS a pre konverziu prezentácie z XML do PDF. Klaďte dôraz na znovupoužitie jednotlivých šablon pre viaceré výstupné formáty. Umožnite zadávanie parametrov transformácií.

Súčasťou požiadaviek na zadanie je vytvorenie správy o zadaní 3, v ktorej zdokumentujete splenie jednotlivých bodov zadania. Správa bude súčasťou vašej stránky o Webovom publikovaní na GitHube.

## Dokumentácia druhého zadania ##
Výsledkom tretieho zadania sú:
* RELAX NG schéma prezentacia.rnc
* prezentácia zapísaná v XML z3.xml
* XHTML
	* XSLT šablóna prezentaciahtml.xsl   
	* súbor parametrov params.xsl
	* style.css
	* dávkový súbor to_xhtml.bat
	* a súbor xhtml slidov prezentácie s_X.html
		* všetky súbory sú valídne XML
* PDF
	* XSLT šablóna prezentaciapdf.xsl   
	* súbor parametrov params_pdf.xsl
	* dávkový súbor to_pdf.bat
	* výsledný PDF súbor prezentacia.pdf



### Opis typu dokumentu###
Dokument bol opísaný pommocou RELAX NG Compact Syntax. Je tvorený pomocou pomenovaných šablôn (named patterns), a každá šablóna je v súbore opísaná komentárom. Vzniklo 16 šablón, ktoré predstavijú elementy.

Zoznam šablón:
* Author
* Slide
* Background-color
* Background
* Content
* Layout-intro
* Layout-simple
* Layout-double 
* Layout-no
* Layout-sources
* Layout-end
* Font-color
* Text
* Ordered-list
* Unordered-list
* Image

### XML prezentácia###
Prezentácia má sedem slidov. Každý slide demonštruje možnosti návrhu definície.

* layout-intro je rozloženie pre úvodný slide. Obsahuje nadpis a podnadpis.
* layout-simple obsahuje nadpis a rôzny obsah.
* layout-double obsahuje nadpis a rôzny obsah v dvoch stĺpcoch.
* layout-no obsahuje nadpis a rôzny obsah, bez daného rozloženia.
* layout-end je posledný snímok, obsahuje jeden nadpis.
* layout-sources je prázdny, a označuje kde sa v prezentácií vygeneruje zoznam odkazov.

Ďalej demonštruje nastavovanie pozadia (farba alebo obrázok), farbu textu, pozície obrázkov, textu a zoznamov - číslovaných a nečíslovaných, aj s vnáraním.

### XSL transformácie###
Nasledujú iniciálne navrhnuté XSL transfomrácie, ktoré sú použité pri transformácií do PDF aj XHTML. Implementácie týchto transformácií sa líšia podla cieľového formátu, podrobný prehľad konkrétnych transformácií je v nasledujúcich sekciách.

	<xsl:template name="intro-author">

    <xsl:template match="//content/layout-sources">
	<xsl:template match="//content/layout-no">
    <xsl:template match="//content/layout-double">	
    <xsl:template match="//content/layout-simple">
	<xsl:template match="//content/layout-end"> 
	<xsl:template match="//content/layout-intro">	    	

	<xsl:template match="//ordered-list">
    <xsl:template match="//unordered-list">
	<xsl:template match="//item"> 
    <xsl:template match="//image">	  	

    <xsl:template match="//slide-properties/@ref">
	<xsl:template match="//image/@ref">
	<xsl:template match="//link">	

Parametrizácia:

XHTML:

	<xsl:param name="slide.width" select="'1000px'"/>
	<xsl:param name="slide.height" select="'700px'"/>

PDF:

	<xsl:param name="slide.width" select="'210mm'"/>
	<xsl:param name="slide.height" select="'140mm'"/>

	<xsl:param name="label-end" select="'15mm'"/>
	<xsl:param name="body-start" select="'3mm'"/>

	<xsl:param name="text.size" select="'12pt'"/>
	<xsl:param name="title.size" select="'28pt'"/>

### XSLT XML -> HTML###
Vypísanie mena na potrebné miesto, používa sa pri vypísaní autora na úvodnú priesvitku.

    <xsl:template name="intro-author">
    	<h3 class="intro-author"> <xsl:value-of select="//author"/> </h3>
	</xsl:template>
	
Vygenerovanie navigácie - odkazov na ďaľší, predchádzajúci, prvý a posledný slide. Generuje sa pod každý slide.

	<xsl:template name="navigation">
	<div class="navigation left">
    	<a class="button" href="s_0.html">First</a>

		<xsl:if test="count(preceding-sibling::slide) &gt; 0">
		  <a class="button" href="s_{count(preceding-sibling::slide) - 1}.html">Previous</a>
		</xsl:if>
	</div>

		<p class="meta-title"><xsl:value-of select="count(preceding-sibling::slide)"/>
		<xsl:if test="./slide-properties[@name]">
		  - <xsl:value-of select="./slide-properties/@name"/> 
		</xsl:if>
		</p>

	<div class="navigation right">
		<xsl:if test="count(following-sibling::slide) &gt; 0">
		  <a class="button" href="s_{count(preceding-sibling::slide) + 1}.html">Next</a>
		</xsl:if>
    	<a class="button" href="s_{count(//slide) - 1}.html">Last</a>
	</div>
	</xsl:template>

Vygenerovanie súboru s_X.html, kde X je číslo slidu. Do súboru sa vykreslí obsah slidu a navigácia.

	<xsl:template match="//slide">
	    <xsl:result-document href="s_{count(preceding-sibling::slide)}.html" method="xhtml">
	    	<html xmlns="http://www.w3.org/1999/xhtml">
				<head> 
					<link rel="stylesheet" href="style.css" type="text/css"/>
				</head>	  
				<body class="presentation">  		
		    	<xsl:apply-templates select="./slide-properties"/>
		    	<xsl:call-template name="navigation"/>
		    	</body>
		    </html>
	    </xsl:result-document>
    </xsl:template>  

Aplikovanie slide properties - nastavenie pozadia a farby fontu.

    <xsl:template match="//slide/slide-properties[not(@background-color) and not(@background)]">
		<div class="slide" align="center" style="color:{./@font-color};width:{$slide.width};height:{$slide.height};">
    		<xsl:apply-templates select="./../content"/>
    	</div>
    </xsl:template>

	<xsl:template match="//slide/slide-properties[@background-color]">
		<div class="slide" align="center" style="background-color:{./@background-color};color:{./@font-color};width:{$slide.width};height:{$slide.height};">
    		<xsl:apply-templates select="./../content"/>
    	</div>
	</xsl:template>

	<xsl:template match="//slide/slide-properties[@background]">
		<div class="slide" align="center" style="background:url({./@background});color:{./@font-color};width:{$slide.width};height:{$slide.height};">
    		<xsl:apply-templates select="./../content"/>
    	</div>
	</xsl:template>

Nasledujú transformácie definujúce rozloženie slidu.

Úvodný slide

	<xsl:template match="//content/layout-intro">
		<h1 class="intro-title"> <xsl:value-of select="title"/>  </h1>
		<h2 class="intro-subtitle"> <xsl:value-of select="subtitle"/>  </h2>
	<xsl:call-template name="intro-author"/>
	</xsl:template>

Ukončujúci slide

	<xsl:template match="//content/layout-end"> 
			<h1 class="intro-title"> <xsl:value-of select="title"/>  </h1>
    </xsl:template>   

Jednoduché rozloženie

    <xsl:template match="//content/layout-simple">		
    	<h1> <xsl:value-of select="title"/>  </h1>
    	<xsl:apply-templates select="text"/>
    	<xsl:apply-templates select="ordered-list"/>
    	<xsl:apply-templates select="unordered-list"/>
        <xsl:apply-templates select="image"/>
    </xsl:template>

Formátovanie textu pre jednoduché rozloženie

    <xsl:template match="//content/layout-simple/text">		
    	<p class="text"> <xsl:value-of select="."/>  </p>
    </xsl:template>

Rozloženie vo dvoch stĺpcoch - dve sekcie

    <xsl:template match="//content/layout-double">		
    	<h1> <xsl:value-of select="title"/>  </h1>
    	<div class="section">
    		<xsl:apply-templates select="./section-left"/>
    	</div>
    	<div class="section">
    		<xsl:apply-templates select="./section-right"/>
       	</div>
    </xsl:template>

Formátovanie textu pre dvojité rozloženie

    <xsl:template match="//content/layout-double/section-left/text | //content/layout-double/section-right/text">		
    	<p class="text"> <xsl:value-of select="."/>  </p>
    </xsl:template>   

Bez rozloženia(explicitne)

	<xsl:template match="//content/layout-no">
    	<xsl:apply-templates select="title"/>
    	<xsl:apply-templates select="text"/>
    	<xsl:apply-templates select="image"/>
    </xsl:template>

Formátovanie nadpisu pre žiadne rozloženie

	<xsl:template match="//content/layout-no/title">
    	<h1 align="center"> <xsl:apply-templates/>  </h1>
    </xsl:template>

Formátovanie textu pre žiadne rozloženie

	<xsl:template match="//content/layout-no/text">
    	<p align="left"> <xsl:apply-templates/>  </p>
    </xsl:template>

Na tomto slide sa vygeneruje zoznam odkazov na zdroje, podla zdrojov priamo zadaných v daných elementoch

    <xsl:template match="//content/layout-sources">
    	<h1 align="center"> Sources and references </h1>
       	<xsl:apply-templates select="//link"/> 	    	
    	<xsl:apply-templates select="//image/@ref"/>
    	<xsl:apply-templates select="//slide-properties/@ref"/>
    </xsl:template>

Tieto transformácie formátujú jednotlivé odkazy na zdroje.

	<xsl:template match="//link">
		<p class="links">Reference: 
			<a href="{@ref}"><xsl:value-of select="@name"/></a>
		</p>
    </xsl:template>  

	<xsl:template match="//image/@ref">
		<p class="links">Image: 
			<a href="{.}"><xsl:value-of select="preceding::title[position() = 1]"/></a>
		</p>
    </xsl:template>  

	<xsl:template match="//slide-properties/@ref">
		<p class="links">Slide background: 
			<a href="{.}"><xsl:value-of select="following::title[position() = 1]"/></a>
    	</p>
    </xsl:template>  

Nasledujú implicítne transformácie.

Toto zabráni autorovi aby sa vypisoval tam, kde nebol explicítne zavolaný 

	    <xsl:template match="//author">
	    </xsl:template>

Výpis číslovaného zoznamu - nastavý typ a dovoľuje vnorenie

	<xsl:template match="//ordered-list">
		<ol type="{@type}" align="left">
			<xsl:apply-templates/>
		</ol>
    </xsl:template>

Výpis nečíslovaného zoznamu - nastavý typ a dovoľuje vnorenie

    <xsl:template match="//unordered-list">
		<ul type="{@type}" align="left">
			<xsl:apply-templates/>
		</ul>
    </xsl:template>

Výpis položky zoznamu.

	<xsl:template match="//item">
		<li><xsl:apply-templates/></li>
    </xsl:template>    

Formátovanie obrázkov - s pozíciou v texte alebo podla súradníc.

    <xsl:template match="//image[@position]">		
        <img src="{@src}" width="{@width}" heigth="{@heigth}" style="position:"> </img>
    </xsl:template> 

    <xsl:template match="//image[not(@position)]">		
        <img src="{@src}" width="{@width}" heigth="{@heigth}" style="left:{@x}px;bottom:{@y}px"> </img>
    </xsl:template>     

### XSLT XML -> PDF###
Root a nastavenie strán - veľkosť strán podla parametrov, DoublePage má dva stĺpce.

	<xsl:template match="presentation">
		<fo:root xmlns:fo="http://www.w3.org/1999/XSL/Format">
			<fo:layout-master-set>
				<fo:simple-page-master page-height="{$slide.height}" page-width="{$slide.width}"
										margin="0mm 0mm 0mm 0mm" master-name="PageMaster">
					<fo:region-body margin="0mm 0mm 0mm 0mm"/>
			
				</fo:simple-page-master>
				<fo:simple-page-master page-height="{$slide.height}" page-width="{$slide.width}"
										margin="0mm 0mm 0mm 0mm" master-name="DoublePage">
					<fo:region-body margin="0mm 0mm 0mm 0mm" column-count="2"/>
			
				</fo:simple-page-master>				
			</fo:layout-master-set>
				<xsl:apply-templates select="./slide"/>
		</fo:root>
	</xsl:template>

Autor - na úvodnú priesvitku

    <xsl:template name="intro-author">
    	<fo:block 	font-size="18pt"	
    				padding="12pt"
				    text-align="center">
    		<xsl:value-of select="//author"/>
    	</fo:block>    	
	</xsl:template>

Aplikovanie slide-properties - farba textu a pozadie slideu.

	<xsl:template match="//slide[slide-properties/@background and not(content/layout-double)]">
		<fo:page-sequence master-reference="PageMaster">
			<fo:flow flow-name="xsl-region-body" >
			    <fo:block-container absolute-position="absolute"
		            top="0cm" left="0cm" width="{$slide.width}" height="{$slide.height}"
		            background-image="url({./slide-properties/@background})">
				<fo:block color="{slide-properties/@font-color}"
				          padding="12pt"
				          text-align="left"
				          font-family="sans-serif"
				          padding-left="15mm"
				          margin-left="20mm"
				          margin-right="20mm">
					<xsl:apply-templates/>
				</fo:block>
		        </fo:block-container>	
			</fo:flow>
		</fo:page-sequence>
    </xsl:template>    

    <xsl:template match="//slide[slide-properties/@background-color and not(content/layout-double)]">
		<fo:page-sequence master-reference="PageMaster">
			<fo:flow flow-name="xsl-region-body" >
			    <fo:block-container absolute-position="absolute"
		            top="0cm" left="0cm" width="{$slide.width}" height="{$slide.height}"
		            background-color="{./slide-properties/@background-color}">
				<fo:block color="{slide-properties/@font-color}"
				          padding="12pt"
				          text-align="left"
				          font-family="sans-serif"
				          padding-left="15mm"
				          margin-left="10mm"
				          margin-right="10mm">  
					<xsl:apply-templates/>
				</fo:block>
		        </fo:block-container>	
			</fo:flow>
		</fo:page-sequence>
    </xsl:template> 

	<xsl:template match="//slide[slide-properties/@background and content/layout-double]">
		<fo:page-sequence master-reference="DoublePage">
			<fo:flow flow-name="xsl-region-body" >
			    <fo:block-container absolute-position="absolute"
		            top="0cm" left="0cm" width="{$slide.width}" height="{$slide.height}"
		            background-image="url({./slide-properties/@background})">
				<fo:block color="{slide-properties/@font-color}"
				          padding="12pt"
				          text-align="left"
				          font-family="sans-serif"
				          padding-left="15mm"
				          margin-left="10mm"
				          margin-right="10mm">
					<xsl:apply-templates/>
				</fo:block>
		        </fo:block-container>	
			</fo:flow>
		</fo:page-sequence>
    </xsl:template>    

    <xsl:template match="//slide[slide-properties/@background-color and content/layout-double]">
		<fo:page-sequence master-reference="DoublePage">
			<fo:flow flow-name="xsl-region-body" >
			    <fo:block-container absolute-position="absolute"
		            top="0cm" left="0cm" width="{$slide.width}" height="{$slide.height}"
		            background-color="{./slide-properties/@background-color}">
				<fo:block color="{slide-properties/@font-color}"
				          padding="12pt"
				          text-align="left"
				          font-family="sans-serif"
				          padding-left="15mm"
				         margin-left="10mm"
				          margin-right="10mm">
					<xsl:apply-templates/>
				</fo:block>
		        </fo:block-container>	
			</fo:flow>
		</fo:page-sequence>
    </xsl:template> 	

Všetky rozloženia sú ekvivalentné verzií pri HTML, iba s XML FO formátovaním.

	<!--  INTRO  -->
		<xsl:template match="//content/layout-intro">
			<fo:block
						font-size="28pt"
						padding="20pt"
						padding-top="60mm"
			          text-align="center">
			<xsl:value-of select="title"/>
			</fo:block>
			<fo:block
						font-size="20pt"
						padding="20pt"
			          text-align="center">
			<xsl:value-of select="subtitle"/>
			</fo:block>
		<xsl:call-template name="intro-author"/>
	</xsl:template>

	<!--  END  -->
	<xsl:template match="//content/layout-end"> 
		<fo:block
					font-size="28pt"
					padding="20pt"
					padding-top="60mm"
		          text-align="center">
		<xsl:value-of select="title"/>
		</fo:block>
    </xsl:template>   

	<!--  SIMPLE  -->
    <xsl:template match="//content/layout-simple">		
			<fo:block
						font-size="28pt"
						padding="20pt"
			          text-align="center">
			<xsl:value-of select="title"/>
			</fo:block>
    	<xsl:apply-templates select="text"/>
    	<xsl:apply-templates select="ordered-list"/>
    	<xsl:apply-templates select="unordered-list"/>
        <xsl:apply-templates select="image"/>
    </xsl:template>

    <xsl:template match="//content/layout-simple/text">		
			<fo:block
						padding="20pt"
			          text-align="left">
			<xsl:value-of select="."/>
			</fo:block>
    </xsl:template>

	<!--  DOUBLE  -->
    <xsl:template match="//content/layout-double">		
		<fo:block
					font-size="28pt"
					padding="20pt"
		          text-align="center">
		<xsl:value-of select="title"/>
		</fo:block>
		<fo:block break-after="column"
					padding="20pt"
		          text-align="left">
		<xsl:apply-templates select="./section-left"/>
		</fo:block>
		<fo:block break-before="column"
					padding="20pt"
		          text-align="left">
		<xsl:apply-templates select="./section-right"/>
		</fo:block>
    </xsl:template>

    <xsl:template match="//content/layout-double/section-left/text | //content/layout-double/section-right/text">		
			<fo:block 
						padding="20pt"
			          text-align="left">
			<xsl:value-of select="."/>
			</fo:block>
    </xsl:template>   

	<!--  NO  -->
	<xsl:template match="//content/layout-no">
    	<xsl:apply-templates select="title"/>
    	<xsl:apply-templates select="text"/>
    	<xsl:apply-templates select="image"/>
    </xsl:template>

	<xsl:template match="//content/layout-no/title">
		<fo:block
					padding="20pt"
		          text-align="center">
		<xsl:value-of select="."/>
		</fo:block>
    </xsl:template>

	<xsl:template match="//content/layout-no/text">
		<fo:block
					padding="20pt"
		          text-align="left">
		<xsl:apply-templates select="."/>
	</fo:block>
    </xsl:template>    

    <!--  SOURCES  -->
    <xsl:template match="//content/layout-sources">
		<fo:block
			font-size="28pt"
			padding="20pt"
          text-align="center">
		Sources and references
		</fo:block>
       	<xsl:apply-templates select="//link"/> 	    	
    	<xsl:apply-templates select="//image/@ref"/>
    	<xsl:apply-templates select="//slide-properties/@ref"/>
    </xsl:template>

Formátovanie odkazov v zozname odkazov.

    	<xsl:template match="//link">
		<fo:block
					font-size="8pt"
					padding="2pt"
		          text-align="left">
		          Reference:
        <fo:basic-link 
			external-destination="url('{@ref}')" 
            color="blue" text-decoration="underline">
             <xsl:value-of select="@name"/>
        </fo:basic-link> 
		</fo:block>
    </xsl:template>  

	<xsl:template match="//image/@ref">
		<fo:block
					font-size="8pt"
					padding="2pt"
		          text-align="left">
		          Image: 
        <fo:basic-link 
			external-destination="url('{.}')" 
            color="blue" text-decoration="underline">
             <xsl:value-of select="preceding::title[position() = 1]"/>
        </fo:basic-link> 
		</fo:block>		
    </xsl:template>  

	<xsl:template match="//slide-properties/@ref">
		<fo:block
					font-size="8pt"
					padding="2pt"
		          text-align="left">
		          Slide background: 
        <fo:basic-link 
			external-destination="url('{.}')" 
            color="blue" text-decoration="underline">
             <xsl:value-of select="following::title[position() = 1]"/>
        </fo:basic-link> 
		</fo:block>			
    </xsl:template>  

V PDF verzií mám iba nečíslované zoznamy - dovoľujú vnáranie

    <xsl:template match="//ordered-list">
		<fo:list-block>
		    <xsl:for-each select="child::*">
				<xsl:choose>
			        <xsl:when test="name() = 'item'">
						<xsl:apply-templates select="."/>
			        </xsl:when>
			        <xsl:otherwise>
						<fo:list-item>
						 <fo:list-item-label end-indent="{$label-end}">
						   <fo:block>*</fo:block>
						 </fo:list-item-label>
						 <fo:list-item-body start-indent="{$body-start}">
						   <xsl:apply-templates select="."/>
						 </fo:list-item-body>
						</fo:list-item>	          
			        </xsl:otherwise>
		        </xsl:choose>
		    </xsl:for-each>
		</fo:list-block>
    </xsl:template>
    <xsl:template match="//unordered-list">
		<fo:list-block>
		    <xsl:for-each select="child::*">
				<xsl:choose>
			        <xsl:when test="name() = 'item'">
						<xsl:apply-templates select="."/>
			        </xsl:when>
			        <xsl:otherwise>
						<fo:list-item>
						 <fo:list-item-label end-indent="{$label-end}">
						   <fo:block>*</fo:block>
						 </fo:list-item-label>
						 <fo:list-item-body start-indent="{$body-start}">
						   <xsl:apply-templates select="."/>
						 </fo:list-item-body>
						</fo:list-item>	          
			        </xsl:otherwise>
		        </xsl:choose>
		    </xsl:for-each>
		</fo:list-block>
    </xsl:template>

Položka zoznamu - s parametrami odsadenia

    	<xsl:template match="//item">
		<fo:list-item>
		 <fo:list-item-label end-indent="{$label-end}">
		   <fo:block>*</fo:block>
		 </fo:list-item-label>
		 <fo:list-item-body start-indent="{$body-start}">
		   <fo:block><xsl:value-of select="text()"/></fo:block>
		 </fo:list-item-body>
		</fo:list-item>
    </xsl:template>    

Formátovanie obrázku

    <xsl:template match="//image">	
		<fo:external-graphic src="url('{@src}')"  content-height="40%"/>    	
    </xsl:template> 