---
title: Best practice del sistema di stile con  AEM Sites
description: Un articolo dettagliato che illustra le procedure ottimali per l'implementazione di Style System con  Adobe Experience Manager Sites.
feature: style-system
topics: development, components, front-end-development
audience: developer
doc-type: article
activity: understand
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '1538'
ht-degree: 2%

---


# Best practice per il sistema di stile{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>Leggi il contenuto disponibile in [Informazioni su come codificare il sistema](style-system-technical-video-understand.md)di stile per comprendere meglio le convenzioni simili a BEM utilizzate da AEM Style System.

Esistono due gusti o stili principali implementati per il AEM Style System:

* **Stili di layout**
* **Stili di visualizzazione**

**Gli stili** di layout influenzano molti elementi di un componente per creare una rappresentazione (progettazione e layout) ben definita e identificabile del componente, spesso allineandosi a uno specifico concetto di marchio riutilizzabile. Ad esempio, un componente Teaser può essere presentato nel layout tradizionale basato su scheda, in uno stile promozionale orizzontale o come layout Hero che sovrappone il testo su un’immagine.

**Gli stili** di visualizzazione vengono utilizzati per influenzare lievi variazioni degli stili di layout, ma non per modificare la natura o l&#39;intento fondamentali dello stile di layout. Ad esempio, uno stile di layout Eroe può avere stili di visualizzazione che modificano la combinazione di colori dalla combinazione di colori del marchio principale alla combinazione di colori del marchio secondario.

## Procedure consigliate per l&#39;organizzazione degli stili {#style-organization-best-practices}

Quando si definiscono i nomi di stile disponibili per AEM autori, è consigliabile:

* Denominare gli stili utilizzando un vocabolario compreso dagli autori
* Ridurre al minimo il numero di opzioni di stile
* Esporre solo le opzioni di stile e le combinazioni consentite dagli standard del marchio
* Esporre solo combinazioni di stile con un effetto
   * Se sono esposte combinazioni inefficaci, assicurarsi che almeno non abbiano un effetto dannoso

Con l&#39;aumento del numero di possibili combinazioni di stile disponibili per AEM autori, più permutazioni devono essere QA e convalidate in base agli standard del marchio. Troppe opzioni possono anche confondere gli autori in quanto potrebbe diventare poco chiaro quale opzione o combinazione è necessaria per produrre l&#39;effetto desiderato.

### Nomi di stile e classi CSS {#style-names-vs-css-classes}

I nomi degli stili, o le opzioni presentate agli autori AEM e i nomi delle classi CSS di implementazione sono separati in AEM.

Questo consente di etichettare le opzioni Stile in un vocabolario in modo chiaro e comprensibile per gli autori AEM, ma consente agli sviluppatori CSS di denominare le classi CSS in modo semantico e sicuro per il futuro. Esempio:

Un componente deve avere le opzioni da colorare con i colori **primari** e **secondari** del marchio, tuttavia, gli autori AEM conoscono i colori come **verdi** e **gialli**, anziché come linguaggio di progettazione primario e secondario.

Il AEM Style System può esporre questi stili di visualizzazione colorati utilizzando etichette descrittive **Green** e **Yellow**, consentendo allo sviluppatore CSS di utilizzare la denominazione semantica `.cmp-component--primary-color` e `.cmp-component--secondary-color` di definire l&#39;implementazione effettiva dello stile in CSS.

Il nome Stile di **Verde** è mappato su `.cmp-component--primary-color`e **Giallo** su `.cmp-component--secondary-color`.

Se in futuro il colore del marchio dell&#39;azienda cambia, tutto ciò che deve essere modificato sono le singole implementazioni di `.cmp-component--primary-color` e `.cmp-component--secondary-color`, e i nomi Stile.

## Il componente Teaser come esempio di utilizzo {#the-teaser-component-as-an-example-use-case}

Di seguito è riportato un esempio di utilizzo dello stile di un componente Teaser per disporre di diversi stili di layout e di visualizzazione.

Verranno esaminati i nomi Stile (esposti agli autori) e l&#39;organizzazione delle classi CSS di supporto.

### Configurazione stili di componente teaser {#component-styles-configuration}

L’immagine seguente mostra la configurazione [!UICONTROL Stili] per il componente Teaser per le varianti discusse nel caso d’uso.

I nomi dei gruppi [!UICONTROL di] stili, Layout e Visualizzazione, per caso corrispondono ai concetti generali degli stili di visualizzazione e di layout utilizzati per classificare concettualmente i tipi di stili in questo articolo.

I nomi dei gruppi [!UICONTROL di] stili e il numero di gruppi [!UICONTROL di] stili devono essere personalizzati in base alle convenzioni di utilizzo dei componenti e alle convenzioni di stile dei componenti specifiche del progetto.

Ad esempio, il nome del gruppo di stili **Display** potrebbe essere stato denominato **Colors**.

![Visualizza gruppo di stili](assets/style-config.png)

### Menu di selezione stili {#style-selection-menu}

Nell&#39;immagine seguente sono visualizzati gli autori del menu [!UICONTROL Stile] con cui interagiscono per selezionare gli stili appropriati per il componente. Si noti che i nomi [!UICONTROL Style Grpi] e i nomi Style sono esposti all&#39;autore.

![Stile, menu a discesa](assets/style-menu.png)

### Default style {#default-style}

Lo stile predefinito è spesso lo stile più comunemente utilizzato per il componente, e la visualizzazione predefinita senza stile del teaser quando viene aggiunto a una pagina.

In base alla comune delle impostazioni di stile predefinite, il CSS può essere applicato direttamente sul `.cmp-teaser` (senza modificatori) o su un `.cmp-teaser--default`.

Se le regole di stile predefinite si applicano più spesso a tutte le varianti, è consigliabile utilizzarle `.cmp-teaser` come classi CSS dello stile predefinito, in quanto tutte le varianti dovrebbero ereditare implicitamente tali classi, purché siano seguite convenzioni simili a BEM. In caso contrario, dovrebbero essere applicati tramite il modificatore predefinito, ad esempio `.cmp-teaser--default`, che a sua volta deve essere aggiunto al campo Classi [CSS predefinite della configurazione di stile del](#component-styles-configuration) componente. In caso contrario, queste regole di stile dovranno essere sostituite in ogni variante.

È anche possibile assegnare uno stile &quot;denominato&quot; come stile predefinito, ad esempio lo stile Eroe `(.cmp-teaser--hero)` definito di seguito, ma è più chiaro implementare lo stile predefinito rispetto alle implementazioni delle classi `.cmp-teaser` o `.cmp-teaser--default` CSS.

>[!NOTE]
>
>Lo stile di layout Predefinito NON ha un nome di stile di visualizzazione, tuttavia l&#39;autore potrà selezionare un&#39;opzione Visualizza nello strumento di selezione AEM Sistema di stile.
>
>Ciò in violazione delle migliori pratiche:
>
>**Esporre solo combinazioni di stile con un effetto**
>
>Se un autore seleziona lo stile di visualizzazione del **verde** , non si verificherà nulla.
>
>In questo caso d’uso, questa violazione viene concessa, in quanto tutti gli altri stili di layout devono essere colorabili utilizzando i colori del marchio.
>
>Nella sezione **Promo (allineata a destra)** che segue viene illustrato come evitare combinazioni di stile indesiderate.

![stile predefinito](assets/default.png)

* **Stile di layout**
   * Predefiniti
* **Stile di visualizzazione**
   * Nessuno
* **Classi** CSS efficaci: `.cmp-teaser--promo` o `.cmp-teaser--default`

### Stile promo {#promo-style}

Lo stile **di layout** Promo viene utilizzato per promuovere contenuti di alto valore sul sito ed è disposto orizzontalmente per occupare una banda di spazio sulla pagina Web e deve essere formattabile in base ai colori del marchio, con lo stile di layout Promo predefinito che utilizza il testo nero.

A questo scopo, nel sistema di stile AEM per il componente Teaser è configurato uno stile **di** layout **Promo** e gli stili **di** visualizzazione **Verde** e **Giallo** .

#### Predefinito promozionale

![promo predefinito](assets/promo-default.png)

* **Stile di layout**
   * Nome stile: **Promo**
   * Classe CSS: `cmp-teaser--promo`
* **Stile di visualizzazione**
   * Nessuno
* **Classi** CSS efficaci: `.cmp-teaser--promo`

#### Promo primario

![promo primario](assets/promo-primary.png)

* **Stile di layout**
   * Nome stile: **Promo**
   * Classe CSS: `cmp-teaser--promo`
* **Stile di visualizzazione**
   * Nome stile: **Verde**
   * Classe CSS: `cmp-teaser--primary-color`
* **Classi** CSS efficaci: `cmp-teaser--promo.cmp-teaser--primary-color`

#### Promo secondario

![Promo secondario](assets/promo-secondary.png)

* **Stile di layout**
   * Nome stile: **Promo**
   * Classe CSS: `cmp-teaser--promo`
* **Stile di visualizzazione**
   * Nome stile: **Giallo**
   * Classe CSS: `cmp-teaser--secondary-color`
* **Classi** CSS efficaci: `cmp-teaser--promo.cmp-teaser--secondary-color`

### Stile allineamento promo a destra {#promo-r-align}

Lo stile di layout **Promo allineato** a destra è una variazione dello stile Promo che riflette la posizione dell&#39;immagine e del testo (immagine a destra, testo a sinistra).

L&#39;allineamento a destra, al centro, è uno stile di visualizzazione, può essere immesso nel sistema di stile AEM come uno stile di visualizzazione selezionato insieme allo stile di layout Promo. Ciò viola la migliore prassi di:

**Esporre solo combinazioni di stile con un effetto**

..già violata nello stile [](#default-style)Predefinito.

Poiché l&#39;allineamento a destra influisce solo sullo stile di layout Promo, e non sugli altri due stili di layout: Per impostazione predefinita ed hero, è possibile creare un nuovo stile di layout Promo (allineato a destra) che include la classe CSS che allinea a destra il contenuto degli stili di layout Promo: `cmp -teaser--alternate`.

Questa combinazione di più stili in un&#39;unica voce Stile può anche aiutare a ridurre il numero di stili e permutazioni di stile disponibili, il che è meglio ridurre a icona.

Notate che il nome della classe CSS `cmp-teaser--alternate`non deve necessariamente corrispondere alla nomenclatura &quot;allineata a destra&quot;, compatibile con l’autore.

#### Predefinito allineamento promo a destra

![promo allineato a destra](assets/promo-alternate-default.png)

* **Stile di layout**
   * Nome stile: **Promo (allineato a destra)**
   * Classi CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Stile di visualizzazione**
   * Nessuno
* **Classi** CSS efficaci: `.cmp-teaser--promo.cmp-teaser--alternate`

#### Allineamento promo principale con allineamento a destra

![Allineamento promo principale con allineamento a destra](assets/promo-alternate-primary.png)

* **Stile di layout**
   * Nome stile: **Promo (allineato a destra)**
   * Classi CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Stile di visualizzazione**
   * Nome stile: **Verde**
   * Classe CSS: `cmp-teaser--primary-color`
* **Classi** CSS efficaci: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--primary-color`

#### Promo Allineato a destra secondario

![Promo Allineato a destra secondario](assets/promo-alternate-secondary.png)

* **Stile di layout**
   * Nome stile: **Promo (allineato a destra)**
   * Classi CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Stile di visualizzazione**
   * Nome stile: **Giallo**
   * Classe CSS: `cmp-teaser--secondary-color`
* **Classi** CSS efficaci: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--secondary-color`

### Stile Hero {#hero-style}

Lo stile di layout Eroe visualizza l&#39;immagine dei componenti come sfondo con il titolo e il collegamento sovrapposti. Lo stile di layout Eroe, come lo stile di layout Promo, deve essere colorabile con i colori del marchio.

Per colorare lo stile di layout Eroe con i colori del marchio, è possibile utilizzare gli stessi stili di visualizzazione utilizzati per lo stile di layout Promo.

Per ogni componente, il nome dello stile viene mappato sul singolo set di classi CSS, il che significa che i nomi delle classi CSS che colorano lo sfondo dello stile di layout Promo, devono colorare il testo e il collegamento dello stile di layout Hero.

Questo può essere ottenuto in modo banale dall&#39;ambito delle regole CSS, tuttavia, ciò richiede agli sviluppatori CSS di comprendere in che modo queste permutazioni verranno applicate in AEM.

CSS per colorare lo sfondo dello stile di layout **Promote** con il colore primario (verde):

```css
.cmp-teaser--promo.cmp-teaser--primary--color {
   ...
   background-color: green;
   ...
}
```

CSS per colorare il testo dello stile di layout **Eroe** con il colore primario (verde):

```css
.cmp-teaser--hero.cmp-teaser--primary--color {
   ...
   color: green;
   ...
}
```

#### Predefinito Esterno

![Stile erotico](assets/hero.png)

* **Stile di layout**
   * Nome stile: **Hero**
   * Classe CSS: `cmp-teaser--hero`
* **Stile di visualizzazione**
   * Nessuno
* **Classi** CSS efficaci: `.cmp-teaser--hero`

#### Hero Primario

![Hero Primario](assets/hero-primary.png)

* **Stile di layout**
   * Nome stile: **Promo**
   * Classe CSS: `cmp-teaser--hero`
* **Stile di visualizzazione**
   * Nome stile: **Verde**
   * Classe CSS: `cmp-teaser--primary-color`
* **Classi** CSS efficaci: `cmp-teaser--hero.cmp-teaser--primary-color`

#### Eroe secondario

![Eroe secondario](assets/hero-secondary.png)

* **Stile di layout**
   * Nome stile: **Promo**
   * Classe CSS: `cmp-teaser--hero`
* **Stile di visualizzazione**
   * Nome stile: **Giallo**
   * Classe CSS: `cmp-teaser--secondary-color`
* **Classi** CSS efficaci: `cmp-teaser--hero.cmp-teaser--secondary-color`

## Risorse aggiuntive {#additional-resources}

* [Documentazione del sistema di stile](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Creazione AEM librerie client](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Sito Web della documentazione BEM (Block Element Modifier)](https://getbem.com/)
* [Sito Web sulla documentazione LESS](https://lesscss.org/)
* [Sito Web jQuery](https://jquery.com/)
