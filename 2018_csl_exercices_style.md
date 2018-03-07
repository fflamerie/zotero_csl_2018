
# Créer des styles bibliographiques personnalisés CSL

# Exercices de style

## Exercice de style 1

Modifiez le style _Elsevier - Harvard (with titles)_ pour que les **auteurs** soient mis en forme selon les consignes du style Garni.

* Attention, il faut bien considérer à la fois les appels de citation **ET** la bibliographie.
* Attention, certains paramétrages par défaut sont **implicites** : si on souhaite les modifier il ne faut pas seulement modifier la valeur d'un attribut mais ajouter un nouvel attribut avec la valeur appropriée.

---

##  Corrigé
### A modifier dans la bibliographie
* prénom en entier = supprimer `initialize-with="." `,
* enlever virgule entre nom et prénom = modifier valeur `sort-separator=", "` ,
*  déduit du formatage des appels de citation : remplacer _et_ par _&_ entre 2 auteurs = modifier valeur `delimiter-precedes-last="always"` et ajouter `and="symbol"`

### Le code
_Avant_
```
<macro name="author">
  <names variable="author">
    <name name-as-sort-order="all" sort-separator=", " initialize-with="." delimiter=", " delimiter-precedes-last="always"/>
    <label form="short" prefix=" (" suffix=")" text-case="capitalize-first"/>
    <substitute>
      <names variable="editor"/>
      <names variable="translator"/>
      <text macro="title"/>
    </substitute>
  </names>
</macro>
```
_Après_
```
<macro name="author">
    <names variable="author">
      <name name-as-sort-order="all" sort-separator=" " delimiter=", " delimiter-precedes-last="never" and="symbol"/>
      <label form="short" prefix=" (" suffix=")" text-case="capitalize-first"/>
      <substitute>
        <names variable="editor"/>
        <names variable="translator"/>
        <text macro="title"/>
      </substitute>
    </names>
  </macro>
```

### A modifier dans les appels de citation
* & entre 2 auteurs = modifier valeur `and="text"`
* Voyez-vous autre chose à modifier dans cette macro?

### Le code
_Avant_
```
<macro name="author-short">
    <names variable="author">
      <name form="short" and="text" delimiter=", " initialize-with=". "/>
      <substitute>
      ...
```
_Après_
```
<macro name="author-short">
    <names variable="author">
      <name form="short" and="symbol" delimiter=", " initialize-with=". "/>
      <substitute>
      ...
```

---

## Exercice de style 2
Modifiez le style _Elsevier - Harvard (with titles)_ pour que les **titres** soient mis en forme selon les consignes du style Garni.

La `macro name="title"` du style est un peu plus complexe que la `macro name="author"`, car elle définit une mise en forme conditionnelle en fonction du type de document. Les conditions sont exprimées dans l'élément `choose`.

*   `if` définit la 1ère condition : si le document est une thèse ou un rapport , le titre est affiché, suivi entre parenthèses du type de thèse ou de rapport et de son numéro.
*   `else-if` définissent les autres conditions : si le document, par exemple, est une page web, le titre est affiché, suivi de la mention entre crochets _WWW Document_.
*   `else` définit ce qui s'applique dans tous les autres cas : si le document est d'un autre type que ceux indiqués dans les éléments `if` et `else-if`, le titre est affiché.

---

### A modifier
* Les titres de livre doivent être en italique = modifier le `else-if `pour les livres.
* Les titres d'article doivent être entre guillemets = créer un `else-if ` pour les articles.
* Les titres de chapitre doivent être entre guillemets = ajouter  au `else-if ` des articles les titres des chapitres.

### Le code
_Avant_
```
<macro name="title">
    <choose>
      <if type="report thesis" match="any">
        <text variable="title"/>
        <group prefix=" (" suffix=")" delimiter=" ">
          <text variable="genre"/>
          <text variable="number" prefix="No. "/>
        </group>
      </if>
      <else-if type="bill book graphic legal_case legislation motion_picture report song speech" match="any">
        <text variable="title"/>
        <text macro="edition" prefix=", "/>
      </else-if>
      <else-if type="webpage">
        <text variable="title"/>
        <text value="WWW Document" prefix=" [" suffix="]"/>
      </else-if>
      <else>
        <text variable="title"/>
      </else>
    </choose>
  </macro>
```
_Après_
```
<macro name="title">
    <choose>
      <if type="report thesis" match="any">
        <text variable="title"/>
        <group prefix=" (" suffix=")" delimiter=" ">
          <text variable="genre"/>
          <text variable="number" prefix="No. "/>
        </group>
      </if>
      <else-if type="bill book graphic legal_case legislation motion_picture report song speech" match="any">
        <text variable="title" font-style="italic"/>
        <text macro="edition" prefix=", "/>
      </else-if>
      <else-if type="chapter article-journal" match="any">
        <text variable="title" quotes="true"/>
      </else-if>
      <else-if type="webpage">
        <text variable="title"/>
        <text value="WWW Document" prefix=" [" suffix="]"/>
      </else-if>
      <else>
        <text variable="title"/>
      </else>
    </choose>
  </macro>
```

---

## Exercice de style 3
Modifiez le style _Elsevier - Harvard (with titles)_ pour que le formatage des volumes, numéros et pages pour les articles de revues corresponde aux consignes du style Garni.

---

### A modifier
* Ajouter le préfixe _vol._
* Ajouter le numéro précédé du préfixe _no_
* Ajouter le préfixe _p._
* ... sans oublier la ponctuation!

### Le code
_Avant_
```
<macro name="locators">
    <choose>
      <if type="article-journal article-magazine article-newspaper" match="any">
        <group prefix=" " delimiter=", ">
          <group>
            <text variable="volume"/>
          </group>
          <text variable="page"/>
        </group>
      </if>
      ....
```

_Après_
```
<macro name="locators">
    <choose>
      <if type="article-journal article-magazine article-newspaper" match="any">
        <group prefix=" " delimiter=", ">
          <group delimiter=" ">
		        <text term="volume" form="short"/>
            <text variable="volume"/>
          </group>
  	       <group delimiter=" ">
		          <text term="issue" form="short"/>
              <text variable="issue"/>
          </group>
          <group delimiter=" ">
	         <text term="page" form="short"/>
            <text variable="page"/>
          </group>
        </group>
      </if>
      ...
```

Le résultat est presque satisfaisant.

Que reste-t-il à corriger?

Le préfixe par défaut pour le numéro ne correspond pas aux consignes du style Garni. Pour le modifier tout en respectant au mieux les règles d'écriture CSL, nous allons ajouter après l'élément `info` un élément `locale`.

```
<locale xml:lang="fr">
    <terms>
      <term name="issue" form="short">no</term>
    </terms>
  </locale>
```

---

## Exercice de style 4

```
<citation collapse="citation-number">
    <sort>
      <key variable="citation-number"/>
    </sort>
    <layout vertical-align="sup" delimiter=",">
      <text variable="citation-number"/>
    </layout>
  </citation>
```

Comment les appels de citation sont-ils formatés dans ce style?

---

### Corrigé
Les appels de citation sont insérés :

* en exposant `vertical-align="sup"`,
* séparés par une virgule `delimiter=","`,
* compactés lorsqu'ils sont adjacents `collapse="citation-number"`.

L'attribut `collapse` permet en effet de compacter des références adjacentes dans un même appel de citation. Ainsi, avec la valeur `"citation-number"` : [1, 2, 3] devient [1-3].

---

## Exercice de style 5

Modifiez le style _Elsevier - Harvard (with titles)_ pour que la mise en forme des appels de citation corresponde aux consignes du style Garni.

---

## Corrigé
### A modifier
* Ce n'est pas explicite dans les exemples, on le déduit (peut-être à tort...) :  enlever le _et al_, pour afficher uniquement le nom du premier auteur, quel que soit le nombre d'auteurs = supprimer les 2 attributs `et-al-min="3" et-al-use-first="1"`.

### Le code
_Avant_

```
<citation et-al-min="3" et-al-use-first="1" disambiguate-add-givenname="true" disambiguate-add-year-suffix="true" collapse="year" cite-group-delimiter=", ">
    <sort>
      <key macro="author"/>
      <key macro="issued" sort="descending"/>
    </sort>
    <layout prefix="(" suffix=")" delimiter="; ">
      <group delimiter=", ">
        <text macro="author-short"/>
        <text macro="issued"/>
        <group delimiter=" ">
          <label variable="locator" form="short"/>
          <text variable="locator"/>
        </group>
      </group>
    </layout>
  </citation>
```

_Après_

```
<citation disambiguate-add-givenname="true" disambiguate-add-year-suffix="true" collapse="year" cite-group-delimiter=", ">
    <sort>
      <key macro="author"/>
      <key macro="issued" sort="descending"/>
    </sort>
    <layout prefix="(" suffix=")" delimiter="; ">
      <group delimiter=", ">
        <text macro="author-short"/>
        <text macro="issued"/>
        <group delimiter=" ">
          <label variable="locator" form="short"/>
          <text variable="locator"/>
        </group>
      </group>
    </layout>
  </citation>
```

---
## Exercice de style 6

Voici l'élément `bibliography` du style _Elsevier-Harvard (with titles)_.
* Pouvez-vous comprendre le paramétrage défini par chacun des 3 attributs de `bibliography`?
* Quels sont les critères de classement des références dans la bibliographie?
* Quel est le dernier caractère affiché dans une référence bibliographique? Est-il toujours identique? Pourquoi?

```
<bibliography hanging-indent="true" entry-spacing="0" line-spacing="1">
    <sort>
      <key macro="author"/>
      <key macro="issued" sort="descending"/>
    </sort>
    <layout>
      <group suffix=".">
        <text macro="author" suffix=","/>
        <text macro="issued" prefix=" "/>
        <group prefix=". ">
          <text macro="title"/>
          <text macro="container"/>
          <text macro="locators"/>
        </group>
      </group>
      <text macro="access" prefix=". "/>
    </layout>
  </bibliography>
```

---

### Corrigé
#### Pouvez-vous comprendre le paramétrage défini par chacun des 3 attributs de `bibliography`?
*  `hanging-indent="true"` : la 1ère ligne est en retrait  par rapport à la marge.
*  `entry-spacing="0"` : le entrées de la bibliographie ne sont pas séparées par un interligne supplémentaire
* `line-spacing="1"` : l'interligne à l'intérieur d'une entrée de la bibliographie est de 1.

#### Quels sont les critères de classement des références dans la bibliographie?
Les références sont classées :

* par ordre alphabétique d'auteur,
* puis par ordre antéchronoloique.

#### Quel est le dernier caractère affiché dans une entrée de la bibliographie? Est-il toujours identique? Pourquoi?
Le dernier élément de ponctuation commun à toutes les entrées de la bibliographie est le `.`, situé avant le résultat de la `macro text="access"`.

Si on reprend la` macro name="access"`, on constate ainsi que le dernier caractère affiché ne sera pas un `.`, mais un autre caractère uniquement dans le cas suivant :
* document de type article de revue ou acte de conférence,
* pour lequel un **DOI** est renseigné dans le champ DOI.

```
<macro name="access">
    <choose>
      <if variable="DOI">
        <text variable="DOI" prefix="https://doi.org/"/>
      </if>
      <else-if type="webpage">
        <group delimiter=" ">
          <text value="URL"/>
          <text variable="URL"/>
          <group prefix="(" suffix=").">
            <text term="accessed" suffix=" "/>
            <date variable="accessed">
              <date-part name="month" form="numeric" suffix="."/>
              <date-part name="day" suffix="."/>
              <date-part name="year" form="short"/>
            </date>
          </group>
        </group>
      </else-if>
    </choose>
  </macro>
```

---

## Exercice de style 7

Modifiez le style _Elsevier Harvard (with titles)_ pour afficher la date originale selon les consignes du style Garni.

---
### Corrigé
* Il faut modifier la `macro name="issued"` pour ajouter la date originale entre crochets avant la date d'édition.
* Pour ne pas afficher la date originale dans les appels de citation, il faut créer une nouvelle macro `macro name="issued-short"` et appeler cette macro dans l'élément `citation`.
* Afin que le tri par date ne soit pas affecté dans la bibliographie, il convient de modifier également l'élément `key macro="issued"` de l'élément `bibliography`.

### Le code
_Macro `issued` avant_

```
<macro name="issued">
    <choose>
      <if variable="issued">
        <date variable="issued">
          <date-part name="year"/>
        </date>
      </if>
      <else>
        <text term="no date" form="short"/>
      </else>
    </choose>
  </macro>
```

_Macro `issued` après_

```
<macro name="issued">
    <choose>
      <if variable="issued">
        <group delimiter=" ">
          <date variable="original-date">
            <date-part name="year" prefix="[" suffix="]"/>
          </date>
          <date variable="issued">
            <date-part name="year"/>
          </date>
        </group>
      </if>
      <else>
        <text term="no date" form="short"/>
      </else>
    </choose>
  </macro>
```

_Macro `issued-short`_

```
<macro name="issued-short">
    <choose>
      <if variable="issued">
        <date variable="issued">
          <date-part name="year"/>
        </date>
      </if>
      <else>
        <text term="no date" form="short"/>
      </else>
    </choose>
  </macro>
```

_`citation` et `bibliography`avant_

```
<citation et-al-min="3" et-al-use-first="1" disambiguate-add-givenname="true" disambiguate-add-year-suffix="true" collapse="year" cite-group-delimiter=", ">
    <sort>
      <key macro="author"/>
      <key macro="issued" sort="descending"/>
    </sort>
    <layout prefix="(" suffix=")" delimiter="; ">
      <group delimiter=", ">
        <text macro="author-short"/>
        <text macro="issued"/>
        <group delimiter=" ">
          <label variable="locator" form="short"/>
          <text variable="locator"/>
        </group>
      </group>
    </layout>
  </citation>
  <bibliography hanging-indent="true" entry-spacing="0" line-spacing="1">
      <sort>
        <key macro="author"/>
        <key macro="issued" sort="descending"/>
      </sort>
      ...
```

_`citation` et `bibliography`après_

```
<citation et-al-min="3" et-al-use-first="1" disambiguate-add-givenname="true" disambiguate-add-year-suffix="true" collapse="year" cite-group-delimiter=", ">
    <sort>
      <key macro="author"/>
      <key macro="issued-short" sort="descending"/>
    </sort>
    <layout prefix="(" suffix=")" delimiter="; ">
      <group delimiter=", ">
        <text macro="author-short"/>
        <text macro="issued-short"/>
        <group delimiter=" ">
          <label variable="locator" form="short"/>
          <text variable="locator"/>
        </group>
      </group>
    </layout>
  </citation>
  <bibliography hanging-indent="true" entry-spacing="0" line-spacing="1">
      <sort>
        <key macro="author"/>
        <key macro="issued-short" sort="descending"/>
      </sort>
      ...
```
