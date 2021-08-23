---
title: Best practice per il sistema di stili con AEM Sites
description: Un articolo dettagliato che illustra le best practice per l’implementazione del sistema di stili con Adobe Experience Manager Sites.
feature: Sistema di stili
topics: development, components, front-end-development
audience: developer
doc-type: article
activity: understand
version: 6.4, 6.5
topic: Sviluppo
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1541'
ht-degree: 2%

---


# Best practice per il sistema di stili{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>Controlla il contenuto in [Informazioni su come codificare il sistema di stili](style-system-technical-video-understand.md), per comprendere le convenzioni simili a BEM utilizzate dal sistema di stili di AEM.

Sono implementati due tipi di gusto o stili principali per il sistema di stili AEM:

* **Stili di layout**
* **Visualizzare gli stili**

**Gli** stili di layout influiscono su molti elementi di un componente per creare una rappresentazione (progettazione e layout) ben definita e identificabile del componente, spesso allineata a uno specifico concetto di marchio riutilizzabile. Ad esempio, un componente Teaser può essere presentato nel layout tradizionale basato su schede, in uno stile Promozionale orizzontale o come layout Eroe che sovrappone il testo su un’immagine.

**Gli** stili di visualizzazione vengono utilizzati per influenzare lievi varianti degli stili di layout, tuttavia non modificano la natura o l’intento fondamentali dello stile di layout. Ad esempio, uno stile di layout eroe può avere stili di visualizzazione che modificano lo schema di colori dalla combinazione di colori del marchio principale alla combinazione di colori del marchio secondario.

## Best practice per l’organizzazione di stili {#style-organization-best-practices}

Quando definisci i nomi degli stili disponibili per AEM autori, è meglio:

* Denominare gli stili utilizzando un vocabolario compreso dagli autori
* Ridurre al minimo il numero di opzioni di stile
* Esporre solo le opzioni di stile e le combinazioni consentite dagli standard di marchio
* Esporre solo combinazioni di stile con un effetto
   * Se sono esposte combinazioni inefficaci, assicurarsi che almeno non abbiano un effetto negativo

Con l’aumento del numero di possibili combinazioni di stile disponibili per gli autori di AEM, più permutazioni devono essere verificate e convalidate in base agli standard di marchio. Troppe opzioni possono confondere gli autori in quanto potrebbe non essere chiaro quale opzione o combinazione è necessaria per produrre l&#39;effetto desiderato.

### Nomi di stile e classi CSS {#style-names-vs-css-classes}

I nomi degli stili o le opzioni presentate agli autori AEM e i nomi delle classi CSS di implementazione vengono disaccoppiati in AEM.

Questo consente alle opzioni di stile di essere etichettate in un vocabolario chiaro e compreso dagli autori AEM, ma consente agli sviluppatori CSS di denominare le classi CSS in modo semantico e a prova di futuro. Esempio:

Un componente deve avere le opzioni da colorare con i colori **primari** e **secondari** del marchio, tuttavia, gli autori AEM conoscono i colori come **verde** e **giallo**, anziché il linguaggio di progettazione del primario e secondario.

Il sistema di stili AEM può esporre questi stili di visualizzazione colorati utilizzando le etichette compatibili con gli autori **Verde** e **Giallo**, consentendo agli sviluppatori CSS di utilizzare la denominazione semantica di `.cmp-component--primary-color` e `.cmp-component--secondary-color` per definire l’implementazione effettiva dello stile in CSS.

Il nome dello stile di **Verde** è mappato su `.cmp-component--primary-color` e **Giallo** su `.cmp-component--secondary-color`.

Se in futuro il colore del marchio dell&#39;azienda cambia, tutto ciò che deve essere modificato sono le singole implementazioni di `.cmp-component--primary-color` e `.cmp-component--secondary-color` e i nomi di stile.

## Esempio di utilizzo del componente Teaser {#the-teaser-component-as-an-example-use-case}

Di seguito è riportato un esempio di utilizzo dello stile di un componente Teaser per impostare diversi stili di layout e visualizzazione.

Verranno esaminati il modo in cui i nomi di stile (esposti agli autori) e le modalità di organizzazione delle classi CSS di supporto.

### Configurazione degli stili dei componenti teaser {#component-styles-configuration}

L’immagine seguente mostra la configurazione [!UICONTROL Stili] per il componente Teaser per le varianti discusse nel caso d’uso.

I nomi [!UICONTROL Gruppo di stili], Layout e Visualizzazione, in base ai concetti generali di Stili di visualizzazione e Stili di layout utilizzati per classificare concettualmente i tipi di stili in questo articolo.

I nomi [!UICONTROL Gruppo di stili] e il numero di [!UICONTROL Gruppi di stili] devono essere personalizzati in base al caso d’uso del componente e alle convenzioni di stile del componente specifico del progetto.

Ad esempio, il nome del gruppo di stili **Display** potrebbe essere stato denominato **Colori**.

![Visualizza gruppo di stili](assets/style-config.png)

### Menu di selezione stile {#style-selection-menu}

L’immagine seguente mostra l’interazione degli autori del menu [!UICONTROL Stile] per selezionare gli stili appropriati per il componente. I nomi [!UICONTROL Grafici di stile] e i nomi Stile sono tutti esposti all’autore.

![Menu a discesa Stile](assets/style-menu.png)

### Stile predefinito {#default-style}

Lo stile predefinito è spesso lo stile più comunemente utilizzato nel componente e la visualizzazione predefinita e non formattata del teaser quando viene aggiunto a una pagina.

A seconda della commonalità dello stile predefinito, il CSS può essere applicato direttamente sul `.cmp-teaser` (senza modificatori) o su un `.cmp-teaser--default`.

Se le regole di stile predefinite si applicano più spesso di quanto non sia per tutte le varianti, è consigliabile utilizzare `.cmp-teaser` come classi CSS dello stile predefinito, in quanto tutte le varianti dovrebbero ereditare implicitamente, presupponendo che siano seguite convenzioni simili a BEM. In caso contrario, devono essere applicate tramite il modificatore predefinito, ad esempio `.cmp-teaser--default`, che a sua volta deve essere aggiunto al campo Classi CSS predefinite ](#component-styles-configuration) della configurazione dello stile del [componente, altrimenti queste regole di stile dovranno essere ignorate in ogni variante.

È anche possibile assegnare uno stile &quot;denominato&quot; come stile predefinito, ad esempio lo stile eroe `(.cmp-teaser--hero)` definito di seguito, tuttavia è più chiaro implementare lo stile predefinito rispetto alle implementazioni delle classi CSS `.cmp-teaser` o `.cmp-teaser--default`.

>[!NOTE]
>
>Osserva che lo stile di layout predefinito NON ha un nome di stile di visualizzazione, tuttavia l’autore potrà selezionare un’opzione Visualizzazione nello strumento di selezione del sistema di stile AEM.
>
>Ciò in violazione delle migliori pratiche:
>
>**Esporre solo combinazioni di stile con un effetto**
>
>Se un autore seleziona lo stile di visualizzazione di **Verde**, non succederà nulla.
>
>In questo caso d’uso, verrà applicata questa violazione, in quanto tutti gli altri stili di Layout devono essere colorabili utilizzando i colori del marchio.
>
>Nella sezione **Promo (allineato a destra)** seguente verrà illustrato come evitare combinazioni di stile indesiderate.

![stile predefinito](assets/default.png)

* **Stile di layout**
   * Predefiniti
* **Stile di visualizzazione**
   * Nessuna
* **Classi** CSS efficaci:  `.cmp-teaser--promo` o  `.cmp-teaser--default`

### Stile promo {#promo-style}

Lo **stile layout promo** viene utilizzato per promuovere contenuti di alto valore sul sito e viene disposto orizzontalmente per occupare una banda di spazio sulla pagina web e deve essere formattato in base ai colori del marchio, con lo stile predefinito di layout Promo utilizzando il testo nero.

A questo scopo, nel sistema di stili AEM per il componente Teaser sono configurati uno **stile layout** di **Promo** e uno **stile di visualizzazione** di **Verde** e **Giallo**.

#### Predefinito promozionale

![predefinito promozionale](assets/promo-default.png)

* **Stile di layout**
   * Nome stile: **Promo**
   * Classe CSS: `cmp-teaser--promo`
* **Stile di visualizzazione**
   * Nessuna
* **Classi** CSS efficaci:  `.cmp-teaser--promo`

#### Principale promozionale

![primario promozionale](assets/promo-primary.png)

* **Stile di layout**
   * Nome stile: **Promo**
   * Classe CSS: `cmp-teaser--promo`
* **Stile di visualizzazione**
   * Nome stile: **Verde**
   * Classe CSS: `cmp-teaser--primary-color`
* **Classi** CSS efficaci:  `cmp-teaser--promo.cmp-teaser--primary-color`

#### Promo secondario

![Promo secondario](assets/promo-secondary.png)

* **Stile di layout**
   * Nome stile: **Promo**
   * Classe CSS: `cmp-teaser--promo`
* **Stile di visualizzazione**
   * Nome stile: **Giallo**
   * Classe CSS: `cmp-teaser--secondary-color`
* **Classi** CSS efficaci:  `cmp-teaser--promo.cmp-teaser--secondary-color`

### Stile progressivo allineato a destra {#promo-r-align}

Lo stile di layout **Promo allineato a destra** è una variante dello stile Promo che capovolge la posizione dell&#39;immagine e del testo (immagine a destra, testo a sinistra).

L’allineamento a destra, al suo centro, è uno stile di visualizzazione, può essere inserito nel sistema di stili AEM come stile di visualizzazione selezionato in combinazione con lo stile di layout Promo. Questo viola la migliore pratica di:

**Esporre solo combinazioni di stile con un effetto**

.che è già stato violato nello stile [Predefinito](#default-style).

Poiché l’allineamento a destra influisce solo sullo stile di layout Promo e non sugli altri 2 stili di layout: Per impostazione predefinita ed eroe, possiamo creare un nuovo stile di layout Promo (allineato a destra) che include la classe CSS che allinea a destra il contenuto degli stili di layout Promo: `cmp -teaser--alternate`.

Questa combinazione di più stili in una singola voce Stile consente inoltre di ridurre il numero di stili e permutazioni di stile disponibili, il che è meglio ridurre al minimo.

Osserva che il nome della classe CSS `cmp-teaser--alternate` non deve necessariamente corrispondere alla nomenclatura &quot;allineata a destra&quot; per l’authoring.

#### Predefinito allineato a destra

![allineamento a destra](assets/promo-alternate-default.png)

* **Stile di layout**
   * Nome stile: **Promo (allineato a destra)**
   * Classi CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Stile di visualizzazione**
   * Nessuna
* **Classi** CSS efficaci:  `.cmp-teaser--promo.cmp-teaser--alternate`

#### Principale allineata a destra

![Principale allineata a destra](assets/promo-alternate-primary.png)

* **Stile di layout**
   * Nome stile: **Promo (allineato a destra)**
   * Classi CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Stile di visualizzazione**
   * Nome stile: **Verde**
   * Classe CSS: `cmp-teaser--primary-color`
* **Classi** CSS efficaci:  `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--primary-color`

#### Secondaria allineata a destra

![Secondaria allineata a destra](assets/promo-alternate-secondary.png)

* **Stile di layout**
   * Nome stile: **Promo (allineato a destra)**
   * Classi CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Stile di visualizzazione**
   * Nome stile: **Giallo**
   * Classe CSS: `cmp-teaser--secondary-color`
* **Classi** CSS efficaci:  `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--secondary-color`

### Stile eroe {#hero-style}

Lo stile di layout Eroe visualizza l’immagine dei componenti come sfondo con titolo e collegamento sovrapposti. Lo stile di layout Eroe, come lo stile di layout Promo, deve essere colorato con i colori del marchio.

Per colorare lo stile di layout Eroe con i colori del marchio, è possibile sfruttare gli stessi stili di visualizzazione utilizzati per lo stile di layout Promo.

Per componente, il nome dello stile viene mappato sul singolo set di classi CSS, il che significa che i nomi delle classi CSS che colorano lo sfondo dello stile di layout Promo, devono colorare il testo e il collegamento dello stile di layout Eroe.

Questo può essere ottenuto in modo banale tramite l’ambito delle regole CSS, tuttavia, questo richiede agli sviluppatori CSS di comprendere in che modo queste permutazioni verranno emanate in AEM.

CSS per colorare lo sfondo dello stile di layout **Promuovi** con il colore primario (verde):

```css
.cmp-teaser--promo.cmp-teaser--primary--color {
   ...
   background-color: green;
   ...
}
```

CSS per la colorazione del testo dello stile di layout **Eroe** con il colore primario (verde):

```css
.cmp-teaser--hero.cmp-teaser--primary--color {
   ...
   color: green;
   ...
}
```

#### Predefinito eroe

![Stile eroe](assets/hero.png)

* **Stile di layout**
   * Nome stile: **Eroe**
   * Classe CSS: `cmp-teaser--hero`
* **Stile di visualizzazione**
   * Nessuna
* **Classi** CSS efficaci:  `.cmp-teaser--hero`

#### Hero primario

![Hero primario](assets/hero-primary.png)

* **Stile di layout**
   * Nome stile: **Promo**
   * Classe CSS: `cmp-teaser--hero`
* **Stile di visualizzazione**
   * Nome stile: **Verde**
   * Classe CSS: `cmp-teaser--primary-color`
* **Classi** CSS efficaci:  `cmp-teaser--hero.cmp-teaser--primary-color`

#### Eroe secondario

![Eroe secondario](assets/hero-secondary.png)

* **Stile di layout**
   * Nome stile: **Promo**
   * Classe CSS: `cmp-teaser--hero`
* **Stile di visualizzazione**
   * Nome stile: **Giallo**
   * Classe CSS: `cmp-teaser--secondary-color`
* **Classi** CSS efficaci:  `cmp-teaser--hero.cmp-teaser--secondary-color`

## Risorse aggiuntive {#additional-resources}

* [Documentazione del sistema di stili](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Creazione AEM librerie client](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Sito web della documentazione di BEM (modificatore di elemento isolato)](https://getbem.com/)
* [Sito web della documentazione SNELESS](https://lesscss.org/)
* [Sito web jQuery](https://jquery.com/)
