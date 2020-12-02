---
title: Comprendere i frammenti di contenuto e i frammenti esperienza
description: I frammenti di contenuto e i frammenti esperienza di Adobe Experience Manager possono apparire simili sulla superficie, ma ogni ruolo chiave viene giocato in diversi casi di utilizzo. Scopri come i frammenti di contenuto e i frammenti esperienza sono simili, diversi e come e quando utilizzarli.
sub-product: risorse, siti, servizi di contenuto
feature: content fragments, experience fragments
topics: headless
version: 6.3, 6.4, 6.5
doc-type: article
activity: understand
audience: all
translation-type: tm+mt
source-git-commit: 03db12de4d95ced8fabf36b8dc328581ec7a2749
workflow-type: tm+mt
source-wordcount: '998'
ht-degree: 3%

---


# Comprendere i frammenti di contenuto e i frammenti esperienza

I frammenti di contenuto e i frammenti esperienza di Adobe Experience Manager possono apparire simili sulla superficie, ma ogni ruolo chiave viene giocato in diversi casi di utilizzo. Scopri come i frammenti di contenuto e i frammenti esperienza sono simili, diversi e come e quando utilizzarli.

## Confronto di frammenti di contenuto e frammenti esperienza

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>Frammenti di contenuto (CF)</strong></td>
<td><strong>Frammenti esperienza (XF)</strong></td>
</tr><tr><td><strong>Definizione</strong></td>
<td><ul>
<li>Contenuto <strong>riutilizzabile, indipendente dalla presentazione</strong>, composto da elementi di dati strutturati (testo, date, riferimenti, ecc.)</li>
</ul>
</td>
<td><ul>
<li>Un composito riutilizzabile di uno o più componenti AEM che definiscono il contenuto e la presentazione che forma un'esperienza <strong>esperienza</strong> che ha senso da sola</li>
</ul>
</td>
</tr><tr><td><strong>Tenant di base</strong></td>
<td><ul>
<li>Contenuti incentrati</li>
<li>Definito da un modello dati <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-models.html" target="_blank">strutturato, basato su modulo.</a></li>
<li>Design e layout non sono agnostici.</li>
<li>Il canale possiede la presentazione del contenuto del frammento di contenuto (layout e progettazione)</li>
</ul>
</td>
<td><ul>
<li>Presentazione incentrata</li>
<li>Definito da una composizione non strutturata di componenti AEM</li>
<li>Definisce la progettazione e il layout del contenuto</li>
<li>Usato "così com'è" nei canali</li>
</ul>
</td>
</tr><tr><td><strong>Dettagli tecnici</strong></td>
<td><ul>
<li>Implementato come <strong>dam:Asset</strong></li>
<li>Definito da un <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-models.html" target="_blank">modello di frammento di contenuto</a></li>
</ul>
</td>
<td><ul>
<li>Implementato come <strong>cq:Page</strong></li>
<li>Definito da modelli modificabili</li>
<li>Rendering HTML nativo</li>
</ul>
</td>
</tr><tr><td><strong>Varianti</strong></td>
<td><ul>
<li>La variante principale è la variante canonica</li>
<li>Le varianti sono specifiche per l’uso, che possono essere allineate con i canali.</li>
</ul>
</td>
<td><ul>
<li>Le varianti sono specifiche per canale o contesto</li>
<li>Le varianti vengono mantenute sincronizzate tramite AEM Live Copy</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">Creazione di contenuti </a> blocksallow riutilizzabili tra le varianti</li>
</ul>
</td>
</tr><tr><td><strong>Funzioni</strong></td>
<td><ul>
<li>Varianti</li>
<li>Versioni</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SynchronizingwithMaster" target="_blank"></a> Sincronizzazione del contenuto tra varianti</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-managing.html#ComparingFragmentVersions" target="_blank">Versioni visive </a> con frammenti di contenuto</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#AnnotatingaContentFragment" target="_blank">Annotazioni </a> di elementi di testo con più righe</li>
<li>Descrizione <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SummarizingText" target="_blank">intelligente </a> degli elementi di testo multiriga.</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/creating-translation-projects-for-content-fragments.html" target="_blank">Traduzione/localizzazione</a></li>
</ul>
</td>
<td><ul>
<li>Varianti</li>
<li>Variazioni come Live Copy</li>
<li>Versioni</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">Blocchi di generazione</a></li>
<li>Annotazioni</li>
<li>Layout reattivo e anteprima</li>
<li>Traduzione/localizzazione</li>
</ul>
</td>
</tr><tr><td><strong>Utilizzo</strong></td>
<td><ul>
<li><a href="https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">AEM componenti core Frammento di contenuto </a> da utilizzare in  AEM Sites,  AEM Screens o in frammenti esperienza.</li>
<li>Esportazione JSON tramite <a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html" target="_blank">AEM Content Services</a> per consumo di terze parti</li>
<li>JSON tramite AEM HTTP Assets APIs per l’uso di terze parti.</li>
</ul>
</td>
<td><ul>
<li>AEM componente Frammento esperienza da utilizzare in  AEM Sites,  AEM Screens o altri frammenti esperienza.</li>
<li>Esportazione come <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#ThePlainHTMLRendition" target="_blank">HTML semplice</a> da utilizzare in sistemi di terze parti</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/experience-fragments-target.html" target="_blank">Esportazione HTML in  Adobe </a> Target per offerte mirate</li>
<li>Esportazione JSON in  Adobe Target per offerte mirate</li>
</ul>
</td>
</tr><tr><td><strong>Casi di utilizzo comuni</strong></td>
<td><ul>
<li>Immissione dati altamente strutturata/contenuto basato su moduli</li>
<li>Contenuto editoriale a forma lunga (elemento con più righe)</li>
<li>Contenuto gestito al di fuori del ciclo di vita dei canali che lo forniscono</li>
</ul>
</td>
<td><ul>
<li>Gestione centralizzata di materiale promozionale multicanale utilizzando varianti per canale.</li>
<li>Contenuto riutilizzato tra più pagine in un sito Web.</li>
<li>Chrome del sito Web (ad es. intestazione e piè di pagina)</li>
<li>Un'esperienza gestita al di fuori del ciclo di vita dei canali che la forniscono</li>
</ul>
</td>
</tr><tr><td><strong>Documentazione</strong></td>
<td><ul>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">Guida utente relativa ai frammenti di contenuto AEM</a></li>
<li><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html" target="_blank">Utilizzo di frammenti di contenuto in AEM</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html" target="_blank">Documentazione  Adobe sui frammenti esperienza</a></li>
</ul>
</td>
</tr></tbody></table>

## Architettura dei frammenti di contenuto

Nel diagramma seguente è illustrata l&#39;architettura generale per AEM frammenti di contenuto

!![Architettura dei frammenti di contenuto](./assets/content-fragments-architecture.png)

+ **I** modelli di frammento di contenuto definiscono gli elementi (o i campi) che definiscono il contenuto che il frammento di contenuto può acquisire ed esporre.
+ **Frammento di contenuto** è un&#39;istanza di un modello di frammento di contenuto che rappresenta un&#39;entità di contenuto logico.
+ Il frammento di contenuto **varianti** è conforme al modello di frammento di contenuto, ma presenta varianti di contenuto.
+ I frammenti di contenuto possono essere esposti/utilizzati da:
   + Utilizzo di frammenti di contenuto su **AEM Sites** (o  AEM Screens) tramite il componente Frammento di contenuto dei componenti core AEM WCM.
   + Incorporamento di un frammento di contenuto in un **frammento esperienza** tramite il componente Frammento di contenuto dei componenti core di AEM WCM, da utilizzare in qualsiasi caso di utilizzo di frammenti esperienza.
   + Esposizione di un frammento di contenuto varianti contenuto come JSON tramite **AEM Content Services** e pagine API per casi di utilizzo in sola lettura.
   + Esposizione diretta del contenuto di frammento di contenuto (tutte le varianti) come JSON tramite chiamate dirette a  AEM Assets tramite l&#39;API AEM Assets HTTP **** per i casi di utilizzo CRUD.

## Architettura dei frammenti esperienza

!![Architettura dei frammenti esperienza](./assets/experience-fragments-architecture.png)

+ **I modelli** modificabili, che a loro volta sono definiti da  **Modelli** modificabili e dall’implementazione **di un componente** AEM pagina, definiscono i componenti AEM consentiti che possono essere utilizzati per comporre un frammento esperienza.
+ Il **frammento esperienza** è un&#39;istanza di un modello modificabile che rappresenta un&#39;esperienza logica.
+ Il frammento esperienza **varianti** aderisce al modello modificabile, tuttavia, presenta delle variazioni di esperienza (contenuto e progettazione).
+ I frammenti esperienza possono essere esposti/utilizzati da:
   + Utilizzo di frammenti esperienza su  AEM Sites (o  AEM Screens) tramite il componente AEM frammento esperienza.
   + L&#39;esposizione di un frammento esperienza comporta varianti del contenuto come JSON (con HTML incorporato) tramite **AEM Content Services** e pagine API.
   + Esposizione diretta di una variante del frammento esperienza come **&quot;Plain HTML&quot;**.
   + Esportazione di frammenti esperienza in **Adobe Target** come offerta HTML o JSON.
   +  AEM Sites supporta in modo nativo le offerte HTML, tuttavia, le offerte JSON richiedono uno sviluppo personalizzato.

## Materiali di supporto per i frammenti di contenuto

+ [Guida utente per i frammenti di contenuto](https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Utilizzo di frammenti di contenuto in AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html)
+ [AEM componente Frammento di contenuto dei componenti core WCM](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)
+ [Utilizzo di frammenti di contenuto e AEM Content Services](https://helpx.adobe.com/experience-manager/kt/sites/using/structured-fragments-content-services-feature-video-use.html)
+ [Guida introduttiva a AEM Content Services](https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html)

## Materiali di supporto per i frammenti esperienza

+ [Documentazione  Adobe sui frammenti esperienza](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
+ [Informazioni AEM frammenti esperienza](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-understand.html)
+ [Utilizzo AEM frammenti esperienza](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html)
+ [Utilizzo AEM frammenti esperienza con  Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
