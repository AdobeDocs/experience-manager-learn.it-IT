---
title: Best practice per i sistemi di stili con AEM Sites
description: Un articolo dettagliato che illustra le best practice per l’implementazione del sistema di stili con Adobe Experience Manager Sites.
feature: Style System
version: 6.4, 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Article
exl-id: c51da742-5ce7-499a-83da-227a25fb78c9
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1536'
ht-degree: 3%

---

# Best practice per il sistema di stili{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>Consulta il contenuto all’indirizzo [Informazioni su come codificare per il sistema di stili](style-system-technical-video-understand.md), per garantire una comprensione delle convenzioni di tipo BEM utilizzate dal sistema di stili dell’AEM.

Per il sistema di stili dell’AEM sono implementati due gusti o stili principali:

* **Stili di layout**
* **Stili visualizzazione**

**Stili di layout** influenzare molti elementi di un componente per creare una rappresentazione ben definita e identificabile (progettazione e layout) del componente, spesso allineata a un concetto di marchio riutilizzabile specifico. Ad esempio, un componente Teaser può essere presentato nel layout tradizionale basato su schede, in uno stile promozionale orizzontale o come layout principale che sovrappone il testo su un’immagine.

**Stili visualizzazione** sono utilizzati per influenzare le varianti minori agli stili di Layout, tuttavia, non modificano la natura o l’intento fondamentale dello stile. Ad esempio, uno stile di layout Eroe può avere stili di visualizzazione che modificano la combinazione di colori dalla combinazione di colori del marchio principale alla combinazione di colori del marchio secondario.

## Best practice per l’organizzazione degli stili {#style-organization-best-practices}

Quando definisci i nomi degli stili disponibili per gli autori AEM, è meglio:

* Assegnare un nome agli stili utilizzando un vocabolario comprensibile per gli autori
* Riduci al minimo il numero di opzioni di stile
* Mostra solo le opzioni e le combinazioni di stile consentite dagli standard del marchio
* Mostra solo le combinazioni di stili con effetto
   * Se vengono esposte combinazioni inefficaci, assicurarsi che non abbiano almeno un effetto dannoso

Con l’aumento del numero di possibili combinazioni di stili disponibili per gli autori di AEM, esistono più permutazioni che devono essere sottoposte a QA e convalidate in base agli standard di marchio. Troppe opzioni possono inoltre confondere gli autori, in quanto potrebbe non essere chiaro quale opzione o combinazione è necessaria per produrre l’effetto desiderato.

### Nomi di stile e classi CSS {#style-names-vs-css-classes}

I nomi degli stili, o le opzioni presentate agli autori AEM, e i nomi delle classi CSS implementanti sono disaccoppiati nell&#39;AEM.

Questo consente alle opzioni di stile di essere etichettate in un vocabolario chiaro e compreso dagli autori dell’AEM, ma consente agli sviluppatori CSS di denominare le classi CSS in modo semantico e a prova di futuro. Ad esempio:

Un componente deve avere le opzioni per essere colorato con il **primario** e **secondario** i colori, tuttavia, gli autori AEM conoscono i colori come **verde** e **giallo**, anziché il linguaggio di progettazione di primario e secondario.

Il sistema di stili dell’AEM può esporre questi stili di visualizzazione colorati utilizzando etichette intuitive **Verde** e **Giallo**, consentendo agli sviluppatori CSS di utilizzare la denominazione semantica di `.cmp-component--primary-color` e `.cmp-component--secondary-color` per definire l’implementazione effettiva dello stile in CSS.

Nome stile di **Verde** è mappato a `.cmp-component--primary-color`, e **Giallo** a `.cmp-component--secondary-color`.

Se il colore del marchio dell’azienda dovesse cambiare in futuro, tutto ciò che deve essere modificato sono le singole implementazioni di `.cmp-component--primary-color` e `.cmp-component--secondary-color`e i nomi degli stili.

## Il componente Teaser come caso d’uso di esempio {#the-teaser-component-as-an-example-use-case}

Di seguito è riportato un esempio di utilizzo dello stile di un componente Teaser per diversi stili di layout e visualizzazione.

Questo esplorerà come vengono organizzati i nomi degli stili (esposti agli autori) e le classi CSS di base.

### Configurazione degli stili dei componenti Teaser {#component-styles-configuration}

L&#39;immagine seguente mostra [!UICONTROL Stili] configurazione del componente Teaser per le varianti descritte nel caso d’uso.

Il [!UICONTROL Gruppo di stili] I nomi, il layout e la visualizzazione, per casualità, corrispondono ai concetti generali degli stili di visualizzazione e degli stili di layout utilizzati per categorizzare concettualmente i tipi di stili in questo articolo.

Il [!UICONTROL Gruppo di stili] nomi e numero di [!UICONTROL Gruppi di stili] devono essere personalizzate in base al caso d’uso del componente e alle convenzioni di stile dei componenti specifiche del progetto.

Ad esempio, il **Visualizzazione** il nome del gruppo di stili poteva essere denominato **Colori**.

![Visualizza gruppo di stili](assets/style-config.png)

### Menu di selezione stile {#style-selection-menu}

L&#39;immagine seguente mostra [!UICONTROL Stile] gli autori dei menu interagiscono con per selezionare gli stili appropriati per il componente. Osserva [!UICONTROL Grpi stile] I nomi, così come i nomi degli stili, sono tutti esposti all’autore.

![Menu a discesa Stile](assets/style-menu.png)

### Stile predefinito {#default-style}

Lo stile predefinito è spesso lo stile più comunemente utilizzato del componente e la visualizzazione predefinita e non formattata del teaser quando viene aggiunto a una pagina.

A seconda della compatibilità dello stile predefinito, il CSS può essere applicato direttamente sul `.cmp-teaser` (senza modificatori) o su `.cmp-teaser--default`.

Se le regole di stile predefinite si applicano il più delle volte a tutte le varianti, è consigliabile utilizzare `.cmp-teaser` come classi CSS dello stile predefinito, poiché tutte le varianti devono ereditarle implicitamente, supponendo che vengano seguite convenzioni di tipo BEM. In caso contrario, devono essere applicate tramite il modificatore predefinito, ad esempio `.cmp-teaser--default`, che a sua volta deve essere aggiunto al [Classi CSS predefinite della configurazione di stile del componente](#component-styles-configuration) altrimenti queste regole di stile dovranno essere ignorate in ogni variante.

È anche possibile assegnare uno stile &quot;denominato&quot; come stile predefinito, ad esempio lo stile Eroe `(.cmp-teaser--hero)` definito di seguito, tuttavia è più chiaro implementare lo stile predefinito rispetto al `.cmp-teaser` o `.cmp-teaser--default` Implementazioni di classi CSS.

>[!NOTE]
>
>Lo stile di layout predefinito NON ha un nome di stile di visualizzazione, tuttavia l’autore può selezionare un’opzione di visualizzazione nello strumento di selezione del sistema di stili dell’AEM.
>
>In violazione della best practice:
>
>**Mostra solo le combinazioni di stili con effetto**
>
>Se un autore seleziona lo stile di visualizzazione **Verde** non succederà niente.
>
>In questo caso d’uso, concederemo questa violazione, poiché tutti gli altri stili di Layout devono essere colorabili utilizzando i colori del marchio.
>
>In **Promo (Allineato a destra)** La sezione seguente illustra come evitare combinazioni di stili indesiderate.

![stile predefinito](assets/default.png)

* **Stile layout**
   * Predefiniti
* **Stile di visualizzazione**
   * Nessuno
* **Classi CSS effettive**: `.cmp-teaser--promo` o `.cmp-teaser--default`

### Stile promozionale {#promo-style}

Il **Stile layout promozionale** viene utilizzato per promuovere contenuti di alto valore sul sito ed è disposto orizzontalmente per occupare una banda di spazio nella pagina web e deve poter essere formattato in base ai colori del marchio, con lo stile di layout Promo predefinito che utilizza il testo nero.

Per ottenere questo risultato, **stile di layout** di **Promo** e **stili di visualizzazione** di **Verde** e **Giallo** sono configurati nel sistema di stili dell’AEM per il componente Teaser.

#### Valore predefinito promozione

![impostazione predefinita promozionale](assets/promo-default.png)

* **Stile layout**
   * Nome stile: **Promo**
   * Classe CSS: `cmp-teaser--promo`
* **Stile di visualizzazione**
   * Nessuno
* **Classi CSS effettive**: `.cmp-teaser--promo`

#### Promozione principale

![promo primario](assets/promo-primary.png)

* **Stile layout**
   * Nome stile: **Promo**
   * Classe CSS: `cmp-teaser--promo`
* **Stile di visualizzazione**
   * Nome stile: **Verde**
   * Classe CSS: `cmp-teaser--primary-color`
* **Classi CSS effettive**: `cmp-teaser--promo.cmp-teaser--primary-color`

#### Promo secondario

![Promo secondario](assets/promo-secondary.png)

* **Stile layout**
   * Nome stile: **Promo**
   * Classe CSS: `cmp-teaser--promo`
* **Stile di visualizzazione**
   * Nome stile: **Giallo**
   * Classe CSS: `cmp-teaser--secondary-color`
* **Classi CSS effettive**: `cmp-teaser--promo.cmp-teaser--secondary-color`

### Stile promo allineato a destra {#promo-r-align}

Il **Promozione allineata a destra** lo stile di layout è una variante dello stile Promo che inverte la posizione dell’immagine e del testo (immagine a destra, testo a sinistra).

L’allineamento a destra, nel suo nucleo, è uno stile di visualizzazione, che potrebbe essere inserito nel Sistema di stili AEM come uno stile di visualizzazione selezionato insieme allo stile di layout Promo. Ciò viola la best practice di:

**Mostra solo le combinazioni di stili con effetto**

..che è già stato violato in [Stile predefinito](#default-style).

Poiché l’allineamento corretto influisce solo sullo stile di layout Promo e non sugli altri 2 stili di layout: predefinito ed eroe, possiamo creare un nuovo stile di layout Promo (allineato a destra) che include la classe CSS che allinea a destra il contenuto degli stili di layout Promo: `cmp -teaser--alternate`.

Questa combinazione di più stili in una singola voce di stile può anche aiutare a ridurre il numero di stili e permutazioni di stile disponibili, il che è meglio ridurre al minimo.

Osserva il nome della classe CSS, `cmp-teaser--alternate`, non deve necessariamente corrispondere alla nomenclatura descrittiva di &quot;allineato a destra&quot;.

#### Impostazione predefinita promozionale allineata a destra

![promo allineato a destra](assets/promo-alternate-default.png)

* **Stile layout**
   * Nome stile: **Promo (Allineato a destra)**
   * Classi CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Stile di visualizzazione**
   * Nessuno
* **Classi CSS effettive**: `.cmp-teaser--promo.cmp-teaser--alternate`

#### Promozione - Principale allineato a destra

![Promozione - Principale allineato a destra](assets/promo-alternate-primary.png)

* **Stile layout**
   * Nome stile: **Promo (Allineato a destra)**
   * Classi CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Stile di visualizzazione**
   * Nome stile: **Verde**
   * Classe CSS: `cmp-teaser--primary-color`
* **Classi CSS effettive**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--primary-color`

#### Promo Allineato a destra Secondario

![Promo Allineato a destra Secondario](assets/promo-alternate-secondary.png)

* **Stile layout**
   * Nome stile: **Promo (Allineato a destra)**
   * Classi CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Stile di visualizzazione**
   * Nome stile: **Giallo**
   * Classe CSS: `cmp-teaser--secondary-color`
* **Classi CSS effettive**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--secondary-color`

### Stile protagonista {#hero-style}

Lo stile di layout Eroe visualizza l’immagine dei componenti come sfondo con il titolo e il collegamento sovrapposti. Lo stile di layout Eroe, come lo stile di layout Promo, deve essere colorabile con i colori del marchio.

Per colorare lo stile di layout Eroe con i colori del marchio, è possibile utilizzare gli stessi stili di visualizzazione utilizzati per lo stile di layout Promo.

Per ogni componente, il nome dello stile è mappato al singolo set di classi CSS, il che significa che i nomi delle classi CSS che colorano lo sfondo dello stile di layout Promo devono colorare il testo e il collegamento dello stile di layout Eroe.

Questo può essere ottenuto in modo banale definendo l’ambito delle regole CSS, tuttavia, questo richiede agli sviluppatori CSS di capire come queste permutazioni vengono emanate sull’AEM.

CSS per colorare lo sfondo del **Promuovi** stile di layout con il colore principale (verde):

```css
.cmp-teaser--promo.cmp-teaser--primary--color {
   ...
   background-color: green;
   ...
}
```

CSS per colorare il testo della **Eroe** stile di layout con il colore principale (verde):

```css
.cmp-teaser--hero.cmp-teaser--primary--color {
   ...
   color: green;
   ...
}
```

#### Valore predefinito protagonista

![Stile protagonista](assets/hero.png)

* **Stile layout**
   * Nome stile: **Eroe**
   * Classe CSS: `cmp-teaser--hero`
* **Stile di visualizzazione**
   * Nessuno
* **Classi CSS effettive**: `.cmp-teaser--hero`

#### Primaria protagonista

![Primaria protagonista](assets/hero-primary.png)

* **Stile layout**
   * Nome stile: **Promo**
   * Classe CSS: `cmp-teaser--hero`
* **Stile di visualizzazione**
   * Nome stile: **Verde**
   * Classe CSS: `cmp-teaser--primary-color`
* **Classi CSS effettive**: `cmp-teaser--hero.cmp-teaser--primary-color`

#### Eroe secondario

![Eroe secondario](assets/hero-secondary.png)

* **Stile layout**
   * Nome stile: **Promo**
   * Classe CSS: `cmp-teaser--hero`
* **Stile di visualizzazione**
   * Nome stile: **Giallo**
   * Classe CSS: `cmp-teaser--secondary-color`
* **Classi CSS effettive**: `cmp-teaser--hero.cmp-teaser--secondary-color`

## Risorse aggiuntive {#additional-resources}

* [Documentazione del sistema di stili](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Creazione di librerie client AEM](https://helpx.adobe.com/it/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Sito Web della documentazione di BEM (Block Element Modifier)](https://getbem.com/)
* [Sito Web di documentazione LESS](https://lesscss.org/)
* [sito web jQuery](https://jquery.com/)
