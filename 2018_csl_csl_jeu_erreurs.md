<!-- $theme: default-->
<!-- page_number: true -->
<!-- $size: 16:9 -->

# Créer des styles bibliographiques personnalisés CSL
# Le jeu des 7 erreurs CSL
**Urfist Bordeaux**
6 mars 2018
Matériel du cours en ligne [ici](https://github.com/fflamerie/zotero_csl_2018)
<small>
*Frédérique Flamerie* 
*chargée de mission science ouverte/données de la recherche*
*Université de Bordeaux - Direction de la Documentation*
</small>
![licence_cc 50%](img/by-sa.png)

---
# Erreur dans la syntaxe du style :
# Error: File is not valid XML

---

# Erreur 1
`<title>MHRA 3rd edition (note with bibliography)</title>`
`<title-short>MHRA`
`<id>http://www.zotero.org/styles/mhra</id>`
`<link href="http://www.zotero.org/styles/mhra" rel="self"/>`


---


# Erreur 1 - corrigé

`<title>MHRA 3rd edition</title>`
`<title-short>MHRA</title-short>` :point_left:
`<id>http://www.zotero.org/styles/mhra</id>`
`<link href="http://www.zotero.org/styles/mhra" rel="self"/>`

_Oubli de la balise fermante `</title-short>`_

---
# Erreur 2 

`<info>`
    `<title>MHRA 3rd edition</title>`
    `<title-short>MHRA</title-short>`
    `<id>http://www.zotero.org/styles/mhra</id>`
`<link href="http://www.zotero.org/styles/mhra" rel=self/>`

---
# Erreur 2 - corrigé
`<info>`
    `<title>MHRA 3rd edition</title>`
    `<title-short>MHRA</title-short>`
    `<id>http://www.zotero.org/styles/mhra</id>`
`<link href="http://www.zotero.org/styles/mhra" rel="self"/>`:point_left:

_Oubli des `" "` pour encadrer la valeur de l'attribut `rel`_

---
# Erreur 3
`<category citation-format="note"/>`
   `<category field="generic-base">`
   `<summary>MHRA format with full notes and bibliography</summary>`
   `<updated>2016-09-28T13:09:49+00:00</updated>`

---
# Erreur 3 - corrigé
`<category citation-format="note"/>`
   `<category field="generic-base"/>`:point_left:
   `<summary>MHRA format with full notes and bibliography</summary>`
   `<updated>2016-09-28T13:09:49+00:00</updated>`
   
_Oubli de la barre oblique `/` pour refermer l'élément_   

---
# Erreur 4
`<locale xml:lang="en">`
    `<terms>`
      `<term name="et-al">and others</term>`
      `<term name="editor" form="verb-short">ed. by</term>`
      `<term name="edition" form="short">edn</term>`
      `<term name="translator" form="verb-short">trans. by</term>`
      `<term name="folio">`
        `<single>fol.</single>`
        `<multiple>fols</multiple>`
        `</terms>`
	`</term>`
  `</locale>`    

---
# Erreur 4 - corrigé
`<locale xml:lang="en">`
    `<terms>`
      `<term name="et-al">and others</term>`
      `<term name="editor" form="verb-short">ed. by</term>`
      `<term name="edition" form="short">edn</term>`
      `<term name="translator" form="verb-short">trans. by</term>`
      `<term name="folio">`
        `<single>fol.</single>`
        `<multiple>fols</multiple>`
       `</term>` :point_left:
       `</terms>` :point_left:
  `</locale>` 
  
_Mauvaise imbrication (inversion) des balises fermantes `</term>` et `</terms>`_

---
# Erreur 5
`<group>`
	`<names variable="container-author reviewed-author">`
         `<label form="verb-short" text-case="lowercase" suffixe=" "/>`
		`<name and="text" delimiter=", "/>`
	`</names>`
`</group>`

---
# Erreur 5 - corrigé
`<group>`
	`<names variable="container-author reviewed-author">`
         `<label form="verb-short" text-case="lowercase" suffix=" "/>`:point_left:
		`<name and="text" delimiter=", "/>`
	`</names>`
`</group>`

_CSL parle anglais : l'attribut `suffixe` n'existe pas ; le vocabulaire correct est `suffix`_

---
# Erreur 6
`<choose>`
          `<!-- Published Conference Paper >`
          `<if variable="container-title">`
            `<group delimiter=", ">`
              `<group delimiter=" ">`
                `<text term="in"/>`
                `<text variable="container-title" font-style="italic"/>`
              `</group>`
              `<text variable="event-place"/>`
            `</group>`
          `</if>`    

---
# Erreur 6 - corrigé
`<choose>`
          `<!-- Published Conference Paper-->`:point_left:
          `<if variable="container-title">`
            `<group delimiter=", ">`
              `<group delimiter=" ">`
                `<text term="in"/>`
                `<text variable="container-title" font-style="italic"/>`
              `</group>`
              `<text variable="event-place"/>`
            `</group>`
          `</if>`    
          
_Oubli des `--` avant le chevron fermant le commentaire_

---
# Erreur 7
`<macro name="url">`
    `<choose>`
      `<if variable="URL">`
        `<text variable="URL" prefix="<" suffix=">"/>`
        `<group delimiter=", ">`
          `<text term="accessed" suffix=" le "/>`
          `<date variable="accessed" form="text" date-parts="year-month-day"/>`
        `</group>`
      `</if>`
    `</choose>`

---
# Erreur 7 - corrigé
`<macro name="url">`
    `<choose>`
      `<if variable="URL">`
        `<text variable="URL" prefix="&lt;" suffix="&gt;"/>`:point_left:
        `<group delimiter=", ">`
          `<text term="accessed" suffix=" le "/>`
          `<date variable="accessed" form="text" date-parts="year-month-day"/>`
        `</group>`
      `</if>`
    `</choose>`

_Les chevrons sont signifiants en XML et doivent être échappés par `&lt;` pour le chevron ouvrant et `&gt;` pour le chevron fermant_