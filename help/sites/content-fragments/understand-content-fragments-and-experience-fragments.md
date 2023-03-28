---
title: Frammenti di contenuto e frammenti di esperienza
description: Scopri le somiglianze e le differenze tra Frammenti di contenuto e Frammenti esperienza, nonché quando e come utilizzare ciascun tipo.
sub-product: Experience Manager Assets, Experience Manager Sites
feature: Content Fragments, Experience Fragments
topics: headless
version: 6.4, 6.5
doc-type: article
activity: understand
audience: all
topic: Content Management
role: User
level: Beginner
exl-id: ccbc68d1-a83e-4092-9a49-53c56c14483e
source-git-commit: 84fdbaa173a929ae7467aecd031cacc4ce73538a
workflow-type: tm+mt
source-wordcount: '1044'
ht-degree: 4%

---

# Frammenti di contenuto e frammenti di esperienza

I frammenti di contenuto e i frammenti esperienza di Adobe Experience Manager possono sembrare simili in superficie, ma ciascuno di essi svolge ruoli chiave in diversi casi d’uso. Scopri in che modo i frammenti di contenuto e i frammenti esperienza sono simili, diversi e quando e come utilizzarli.

## Confronto

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>Frammenti di contenuto (CF)</strong></td>
<td><strong>Frammenti esperienza (XF)</strong></td>
</tr><tr><td><strong>Definizione</strong></td>
<td><ul>
<li>Riutilizzabile, indipendente dalla presentazione <strong>content</strong>, composto da elementi di dati strutturati (testo, date, riferimenti, ecc.)</li>
</ul>
</td>
<td><ul>
<li>Un composito riutilizzabile di uno o più componenti AEM che definiscono il contenuto e la presentazione che costituiscono un <strong>esperienza</strong> che ha senso da solo</li>
</ul>
</td>
</tr><tr><td><strong>Tenant principali</strong></td>
<td><ul>
<li>incentrato sul contenuto</li>
<li>Definito da un <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">modello dati strutturato basato su moduli.</a></li>
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
<li>Definito da un <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">Modello per frammento di contenuto</a></li>
</ul>
</td>
<td><ul>
<li>Implementato come <strong>cq:Page</strong></li>
<li>Definiti dai modelli modificabili</li>
<li>Rendering nativo di HTML</li>
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
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html" target="_blank">Blocchi di generazione</a> consentire il riutilizzo dei contenuti tra le varianti</li>
</ul>
</td>
</tr><tr><td><strong>Funzioni</strong></td>
<td><ul>
<li>Varianti</li>
<li>Versioni</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#synchronizing-with-master" target="_blank">Sincronizzazione</a> di contenuto tra varianti</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-managing.html?lang=en#comparing-fragment-versions" target="_blank">Differf visivo</a> delle versioni dei frammenti di contenuto</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#annotating-a-content-fragment" target="_blank">Annotazioni</a> di elementi di testo su più righe</li>
<li>Intelligente <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#summarizing-text" target="_blank">riepilogo</a> di elementi di testo su più righe.</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/creating-translation-projects-for-content-fragments.html?lang=en" target="_blank">Traduzione/localizzazione</a></li>
</ul>
</td>
<td><ul>
<li>Varianti</li>
<li>Variazioni come Live Copy</li>
<li>Versioni</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en#building-blocks" target="_blank">Blocchi predefiniti</a></li>
<li>Annotazioni</li>
<li>Layout reattivo e anteprima</li>
<li>Traduzione/localizzazione</li>
<li>Modello dati complesso tramite riferimenti a frammenti di contenuto</li>
<li>Anteprima in-app</li>
</ul>
</td>
</tr><tr><td><strong>Utilizzare</strong></td>
<td><ul>
<li>Esportazione JSON tramite <a href="https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html?lang=it">API GraphQL senza titolo AEM</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=it" target="_blank">Componente Frammento di contenuto AEM componenti core</a> da utilizzare in AEM Sites, AEM Screens o in Frammenti esperienza.</li>
<li>Esportazione JSON tramite <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en" target="_blank">AEM Content Services</a> per consumi di terze parti</li>
<li>Esportazione JSON in Adobe Target per offerte mirate</li>
<li>JSON tramite API di risorse HTTP AEM per l’utilizzo in terze parti</li>
</ul>
</td>
<td><ul>
<li>AEM componente Frammento esperienza da utilizzare in AEM Sites, AEM Screens o altri frammenti esperienza.</li>
<li>Esporta come <a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">HTML normale</a> per l'utilizzo da parte di sistemi di terze parti</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/experience-fragments-target.html?lang=en" target="_blank">Esportazione HTML in Adobe Target</a> per offerte mirate</li>
<li>Esportazione JSON in Adobe Target per offerte mirate</li>
</ul>
</td>
</tr><tr><td><strong>Casi d’uso comuni</strong></td>
<td><ul>
<li>Alimentazione di casi d'uso headless su GraphQL</li>
<li>Inserimento dati strutturato/contenuto basato su moduli</li>
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
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">Guida utente sui frammenti di contenuto AEM</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en" target="_blank">Utilizzo di frammenti di contenuto in AEM</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">Adobe di documentazione sui frammenti esperienza</a></li>
</ul>
</td>
</tr></tbody></table>

## Architettura dei frammenti di contenuto

Il diagramma seguente illustra l’architettura generale per i frammenti di contenuto AEM

![Architettura dei frammenti di contenuto](./assets/content-fragments-architecture.png)

+ **Modelli per frammenti di contenuto** definiscono gli elementi (o i campi) che definiscono il contenuto che il frammento di contenuto può acquisire ed esporre.
+ La **Frammento di contenuto** è un’istanza di un modello di frammento di contenuto che rappresenta un’entità di contenuto logico.
+ Frammento di contenuto **varianti** aderisci al modello per frammenti di contenuto, tuttavia, presenta varianti di contenuto.
+ I frammenti di contenuto possono essere esposti o utilizzati da:
   + Utilizzo di frammenti di contenuto in **AEM Sites** (o AEM Screens) tramite il componente Frammento di contenuto AEM dei componenti core WCM.
   + Consuma **Frammento di contenuto** dalle app headless che utilizzano AEM API GraphQL headless.
   + Esposizione di contenuti di varianti di frammenti di contenuto come JSON tramite **AEM Content Services** e pagine API per casi d’uso in sola lettura.
   + Esporre direttamente il contenuto dei frammenti di contenuto (tutte le varianti) come JSON tramite chiamate dirette ad AEM Assets tramite il **API HTTP AEM Assets** per i casi d’uso CRUD.

## Architettura dei frammenti esperienza

![Architettura dei frammenti esperienza](./assets/experience-fragments-architecture.png)

+ **Modelli modificabili**, che a loro volta sono definiti **Tipi di modelli modificabili** e **Implementazione del componente Pagina AEM**, definisci i componenti AEM consentiti che possono essere utilizzati per comporre un frammento esperienza.
+ La **Frammento esperienza** è un&#39;istanza di un modello modificabile che rappresenta un&#39;esperienza logica.
+ Frammento esperienza **varianti** aderisci al modello modificabile, tuttavia, presenta varianti di esperienza (contenuto e progettazione).
+ I frammenti esperienza possono essere esposti/utilizzati da:
   + Utilizzo di frammenti esperienza su AEM Sites (o AEM Screens) tramite il componente Frammento esperienza AEM.
   + Esposizione di un frammento esperienza varianti di contenuto come JSON (con HTML incorporato) tramite **AEM Content Services** e le pagine API.
   + Esporre direttamente una variante del frammento esperienza come **&quot;HTML normale&quot;**.
   + Esportazione di frammenti esperienza in **Adobe Target** come offerte HTML o JSON.
   + AEM Sites supporta in modo nativo le offerte HTML, tuttavia le offerte JSON richiedono uno sviluppo personalizzato.

## Risorsa di supporto per i frammenti di contenuto

+ [Guida utente sui frammenti di contenuto](https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Introduzione ad Adobe Experience Manager come CMS headless](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/introduction.html?lang=it)
+ [Utilizzo di frammenti di contenuto in AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en)
+ [AEM componente Frammento di contenuto dei componenti core WCM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=it)
+ [Utilizzo di frammenti di contenuto e AEM headless](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=en)
+ [Guida introduttiva a AEM Content Services](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en)

## Risorsa di supporto per i frammenti esperienza

+ [Adobe di documentazione sui frammenti esperienza](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en)
+ [Informazioni sui frammenti esperienza AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [Utilizzo di frammenti esperienza AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [Utilizzo AEM frammenti esperienza con Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
