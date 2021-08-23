---
title: Comprendere frammenti di contenuto e frammenti esperienza
description: I frammenti di contenuto e i frammenti esperienza di Adobe Experience Manager possono sembrare simili in superficie, ma ciascuno di essi svolge ruoli chiave in diversi casi d’uso. Scopri in che modo i frammenti di contenuto e i frammenti esperienza sono simili, diversi e quando e come utilizzarli.
sub-product: risorse, siti, servizi di contenuto
feature: Frammenti di contenuto, Frammenti di esperienza
topics: headless
version: 6.3, 6.4, 6.5
doc-type: article
activity: understand
audience: all
topic: Gestione dei contenuti
role: User
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1000'
ht-degree: 2%

---


# Comprendere frammenti di contenuto e frammenti esperienza

I frammenti di contenuto e i frammenti esperienza di Adobe Experience Manager possono sembrare simili in superficie, ma ciascuno di essi svolge ruoli chiave in diversi casi d’uso. Scopri in che modo i frammenti di contenuto e i frammenti esperienza sono simili, diversi e quando e come utilizzarli.

## Confronto di frammenti di contenuto e frammenti esperienza

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>Frammenti di contenuto (CF)</strong></td>
<td><strong>Frammenti esperienza (XF)</strong></td>
</tr><tr><td><strong>Definizione</strong></td>
<td><ul>
<li>Riutilizzabile, agnostico della presentazione <strong>content</strong>, composto da elementi di dati strutturati (testo, date, riferimenti, ecc.)</li>
</ul>
</td>
<td><ul>
<li>Un composito riutilizzabile di uno o più componenti AEM che definiscono i contenuti e le presentazioni che formano un'esperienza <strong>a1/&gt; che ha senso per sé</strong></li>
</ul>
</td>
</tr><tr><td><strong>Tenant principali</strong></td>
<td><ul>
<li>incentrato sul contenuto</li>
<li>Definito da un <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-models.html" target="_blank">modello dati strutturato basato su moduli.</a></li>
<li>Indipendenti dal design e dal layout.</li>
<li>Il canale è proprietario della presentazione del contenuto del frammento di contenuto (layout e progettazione)</li>
</ul>
</td>
<td><ul>
<li>incentrato sulla presentazione</li>
<li>Definito da una composizione non strutturata dei componenti AEM</li>
<li>Definisce la progettazione e il layout del contenuto</li>
<li>Utilizzato "così com'è" nei canali</li>
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
<li>Definiti dai modelli modificabili</li>
<li>Rendering HTML nativo</li>
</ul>
</td>
</tr><tr><td><strong>Varianti</strong></td>
<td><ul>
<li>La variante principale è la variante canonica</li>
<li>Le varianti sono specifiche per i casi d’uso, che possono essere allineate ai canali.</li>
</ul>
</td>
<td><ul>
<li>Le varianti sono specifiche del canale o del contesto</li>
<li>Le varianti vengono mantenute sincronizzate tramite AEM Live Copy</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">Creazione di contenuti </a> blocksallow riutilizzabili tra le varianti</li>
</ul>
</td>
</tr><tr><td><strong>Funzioni</strong></td>
<td><ul>
<li>Varianti</li>
<li>Versioni</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SynchronizingwithMaster" target="_blank"></a> Sincronizzazione del contenuto tra più varianti</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-managing.html#ComparingFragmentVersions" target="_blank">Versioni dei frammenti di contenuto </a> a differenza visiva</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#AnnotatingaContentFragment" target="_blank"></a> Annotazioni di elementi di testo su più righe</li>
<li>Riepilogo intelligente <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SummarizingText" target="_blank"></a> di elementi di testo su più righe.</li>
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
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">AEM componente Frammento di contenuto dei componenti core </a> da utilizzare in AEM Sites, AEM Screens o in Frammenti di esperienza.</li>
<li>Esportazione JSON tramite <a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html" target="_blank">AEM Content Services</a> per consumo di terze parti</li>
<li>JSON tramite API di risorse HTTP AEM per l’utilizzo in terze parti.</li>
</ul>
</td>
<td><ul>
<li>AEM componente Frammento esperienza da utilizzare in AEM Sites, AEM Screens o altri frammenti esperienza.</li>
<li>Esporta come <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#ThePlainHTMLRendition" target="_blank">HTML semplice</a> per l'utilizzo da parte di sistemi di terze parti</li>
<li><a href="https://helpx.adobe.com/it/experience-manager/6-5/sites/administering/using/experience-fragments-target.html" target="_blank">Esportazione HTML in Adobe </a> Target per offerte mirate</li>
<li>Esportazione JSON in Adobe Target per offerte mirate</li>
</ul>
</td>
</tr><tr><td><strong>Casi d’uso comuni</strong></td>
<td><ul>
<li>Inserimento di dati altamente strutturati/contenuto basato su moduli</li>
<li>Contenuto editoriale a lungo termine (elemento su più righe)</li>
<li>Contenuto gestito al di fuori del ciclo di vita dei canali che lo forniscono</li>
</ul>
</td>
<td><ul>
<li>Gestione centralizzata di materiale promozionale multicanale utilizzando varianti per canale.</li>
<li>Contenuto riutilizzato su più pagine in un sito Web.</li>
<li>Chrome del sito Web (ad esempio intestazione e piè di pagina)</li>
<li>Un’esperienza gestita al di fuori del ciclo di vita dei canali che la forniscono</li>
</ul>
</td>
</tr><tr><td><strong>Documentazione</strong></td>
<td><ul>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">Guida utente sui frammenti di contenuto AEM</a></li>
<li><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html" target="_blank">Utilizzo di frammenti di contenuto in AEM</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html" target="_blank">Adobe di documentazione sui frammenti esperienza</a></li>
</ul>
</td>
</tr></tbody></table>

## Architettura dei frammenti di contenuto

Il diagramma seguente illustra l’architettura generale per i frammenti di contenuto AEM

!![Architettura dei frammenti di contenuto](./assets/content-fragments-architecture.png)

+ **I** modelli di frammento di contenuto definiscono gli elementi (o campi) che definiscono quali contenuti possono essere acquisiti ed esposti dai frammenti di contenuto.
+ Il **frammento di contenuto** è un&#39;istanza di un modello di frammento di contenuto che rappresenta un&#39;entità di contenuto logico.
+ Tuttavia, i frammenti di contenuto **varianti** aderiscono al modello per frammenti di contenuto presentano varianti di contenuto.
+ I frammenti di contenuto possono essere esposti o utilizzati da:
   + Utilizzo di frammenti di contenuto su **AEM Sites** (o AEM Screens) tramite il componente Frammento di contenuto AEM dei componenti core WCM.
   + Incorporare un frammento di contenuto in un **Frammento esperienza** tramite il componente Frammento di contenuto dei componenti core di AEM WCM, da utilizzare in qualsiasi caso d’uso relativo ai frammenti esperienza.
   + L’esposizione di un frammento di contenuto presenta contenuti come JSON tramite **AEM Content Services** e pagine API per casi d’uso in sola lettura.
   + Esporre direttamente il contenuto dei frammenti di contenuto (tutte le varianti) come JSON tramite chiamate dirette ad AEM Assets tramite l’ **API HTTP AEM Assets** per i casi d’uso CRUD.

## Architettura dei frammenti esperienza

!![Architettura dei frammenti esperienza](./assets/experience-fragments-architecture.png)

+ **I modelli modificabili**, che a loro volta sono definiti da  **Tipi di modelli modificabili** e dall’implementazione **di un componente** AEM pagina, definiscono i componenti AEM consentiti che possono essere utilizzati per comporre un frammento esperienza.
+ Il **Frammento esperienza** è un&#39;istanza di un modello modificabile che rappresenta un&#39;esperienza logica.
+ Il frammento esperienza **varianti** aderisce al modello modificabile, tuttavia, presenta varianti di esperienza (contenuto e progettazione).
+ I frammenti esperienza possono essere esposti/utilizzati da:
   + Utilizzo di frammenti esperienza su AEM Sites (o AEM Screens) tramite il componente Frammento esperienza AEM.
   + L’esposizione di un frammento esperienza presenta contenuti come JSON (con HTML incorporato) tramite **AEM Content Services** e pagine API.
   + Esporre direttamente una variante del frammento esperienza come **&quot;HTML semplice&quot;**.
   + Esportazione di frammenti esperienza in **Adobe Target** come offerte HTML o JSON.
   + AEM Sites supporta in modo nativo le offerte HTML, tuttavia le offerte JSON richiedono uno sviluppo personalizzato.

## Materiali di supporto per i frammenti di contenuto

+ [Guida utente sui frammenti di contenuto](https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Utilizzo di frammenti di contenuto in AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html)
+ [AEM componente Frammento di contenuto dei componenti core WCM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)
+ [Utilizzo di frammenti di contenuto e servizi per i contenuti AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/structured-fragments-content-services-feature-video-use.html)
+ [Guida introduttiva a AEM Content Services](https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html)

## Materiali di supporto per i frammenti esperienza

+ [Adobe di documentazione sui frammenti esperienza](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
+ [Informazioni sui frammenti esperienza AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-understand.html)
+ [Utilizzo di frammenti esperienza AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html)
+ [Utilizzo AEM frammenti esperienza con Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
