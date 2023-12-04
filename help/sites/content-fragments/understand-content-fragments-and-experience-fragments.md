---
title: Frammenti di contenuto e frammenti di esperienza
description: Scopri le somiglianze e le differenze tra Frammenti di contenuto e Frammenti di esperienza, e quando e come utilizzare ciascun tipo.
sub-product: Experience Manager Assets, Experience Manager Sites
feature: Content Fragments, Experience Fragments
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Article
exl-id: ccbc68d1-a83e-4092-9a49-53c56c14483e
duration: 242
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '829'
ht-degree: 2%

---

# Frammenti di contenuto e frammenti di esperienza

I frammenti di contenuto e i frammenti di esperienza di Adobe Experience Manager possono sembrare simili in superficie, ma ciascuno di essi svolge ruoli chiave in diversi casi d’uso. Scopri come i frammenti di contenuto e i frammenti di esperienza sono simili, diversi e quando e come utilizzarli.

## Confronto

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>Frammenti di contenuto (CF)</strong></td>
<td><strong>Frammenti esperienza (XF)</strong></td>
</tr><tr><td><strong>Definizione</strong></td>
<td><ul>
<li>Riutilizzabile, indipendente dalle presentazioni <strong>contenuto</strong>, composto da elementi dati strutturati (testo, date, riferimenti, ecc.)</li>
</ul>
</td>
<td><ul>
<li>Un composito riutilizzabile di uno o più componenti AEM che definiscono il contenuto e la presentazione che costituiscono un <strong>esperienza</strong> che ha senso da solo</li>
</ul>
</td>
</tr><tr><td><strong>Tenant core</strong></td>
<td><ul>
<li>Incentrato sui contenuti</li>
<li>Definito da un <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">modello dati strutturato basato su moduli.</a></li>
<li>Indipendente dal design e dal layout.</li>
<li>Il canale è responsabile della presentazione del contenuto del frammento di contenuto (layout e progettazione)</li>
</ul>
</td>
<td><ul>
<li>Presentation-centric</li>
<li>Definita dalla composizione non strutturata dei componenti dell’AEM</li>
<li>Definisce la progettazione e il layout del contenuto</li>
<li>Utilizzato "così com’è" nei canali</li>
</ul>
</td>
</tr><tr><td><strong>Dettagli tecnici</strong></td>
<td><ul>
<li>Implementato come <strong>dam:Asset</strong></li>
<li>Definito da un <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">Modello per frammenti di contenuto</a></li>
</ul>
</td>
<td><ul>
<li>Implementato come <strong>cq:Page</strong></li>
<li>Definito da modelli modificabili</li>
<li>Rendering di HTML nativo</li>
</ul>
</td>
</tr><tr><td><strong>Varianti</strong></td>
<td><ul>
<li>La variante principale è la variante canonica</li>
<li>Le varianti sono specifiche per ogni caso d’uso, che possono essere allineate con i canali.</li>
</ul>
</td>
<td><ul>
<li>Le varianti sono specifiche per canale o contesto</li>
<li>Le varianti sono mantenute sincronizzate tramite AEM Live Copy</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html" target="_blank">Blocchi predefiniti</a> consenti il riutilizzo dei contenuti tra varianti</li>
</ul>
</td>
</tr><tr><td><strong>Funzioni</strong></td>
<td><ul>
<li>Varianti</li>
<li>Versioni</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#synchronizing-with-master" target="_blank">Ssincronizzazione</a> di contenuti tra varianti</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-managing.html?lang=en#comparing-fragment-versions" target="_blank">Differenza visiva</a> delle versioni dei frammenti di contenuto</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#annotating-a-content-fragment" target="_blank">Annotazioni</a> di elementi di testo su più righe</li>
<li>Intelligente <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#summarizing-text" target="_blank">riepilogo</a> di elementi di testo su più righe.</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/creating-translation-projects-for-content-fragments.html?lang=en" target="_blank">Traduzione/localizzazione</a></li>
</ul>
</td>
<td><ul>
<li>Varianti</li>
<li>Varianti come Live Copy</li>
<li>Versioni</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en#building-blocks" target="_blank">Blocchi predefiniti</a></li>
<li>Annotazioni</li>
<li>Layout e anteprima reattivi</li>
<li>Traduzione/localizzazione</li>
<li>Modello dati complesso tramite riferimenti ai frammenti di contenuto</li>
<li>Anteprima in-app</li>
</ul>
</td>
</tr><tr><td><strong>Utilizzare</strong></td>
<td><ul>
<li>Esportazione JSON tramite <a href="https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html?lang=it">API GraphQL headless AEM</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=it" target="_blank">Componente Frammento di contenuto dei componenti core AEM</a> da utilizzare in AEM Sites, AEM Screens o in Frammenti di esperienza.</li>
<li>Esportazione JSON tramite <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en" target="_blank">Servizi di contenuti AEM</a> per utilizzo di terze parti</li>
<li>Esportazione JSON in Adobe Target per offerte mirate</li>
<li>JSON tramite API di risorse HTTP AEM per consumo di terze parti</li>
</ul>
</td>
<td><ul>
<li>Componente Frammento esperienza AEM da utilizzare in AEM Sites, AEM Screens o altri Frammenti esperienza.</li>
<li>Esporta come <a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">Plain HTML</a> per l'utilizzo da parte di sistemi di terze parti</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/experience-fragments-target.html?lang=en" target="_blank">Esportazione HTML in Adobe Target</a> per le offerte mirate</li>
<li>Esportazione JSON in Adobe Target per offerte mirate</li>
</ul>
</td>
</tr><tr><td><strong>Casi d’uso comuni</strong></td>
<td><ul>
<li>Casi d’uso headless basati su GraphQL</li>
<li>Inserimento strutturato di dati/contenuti basati su moduli</li>
<li>Contenuto editoriale in formato lungo (elemento su più righe)</li>
<li>Contenuti gestiti al di fuori del ciclo di vita dei canali di distribuzione</li>
</ul>
</td>
<td><ul>
<li>Gestione centralizzata del materiale promozionale multicanale utilizzando le varianti per canale.</li>
<li>Contenuto riutilizzato in più pagine di un sito Web.</li>
<li>Chrome sito Web (es. header e footer)</li>
<li>Un’esperienza gestita al di fuori del ciclo di vita dei canali che la forniscono</li>
</ul>
</td>
</tr><tr><td><strong>Documentazione</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">Guida utente per i frammenti di contenuto di AEM</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en" target="_blank">Utilizzo di frammenti di contenuto nell’AEM</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">Adobe di documentazione su Frammenti esperienza</a></li>
</ul>
</td>
</tr></tbody></table>

## Architettura dei frammenti di contenuto

Il diagramma seguente illustra l’architettura generale dei frammenti di contenuto AEM

![Architettura dei frammenti di contenuto](./assets/content-fragments-architecture.png)

+ **Modelli per frammenti di contenuto** definisci gli elementi (o campi) che definiscono quali contenuti il frammento di contenuto può acquisire ed esporre.
+ Il **Frammento di contenuto** è un’istanza di un modello per frammenti di contenuto che rappresenta un’entità di contenuto logico.
+ Frammento di contenuto **varianti** Tuttavia, per aderire al modello per frammenti di contenuto sono disponibili varianti di contenuto.
+ I frammenti di contenuto possono essere esposti/utilizzati da:
   + Utilizzo dei frammenti di contenuto in **AEM Sites** (o AEM Screens) tramite il componente Frammento di contenuto dei Componenti core WCM dell’AEM.
   + Consumo **Frammento di contenuto** da app headless tramite API GraphQL headless dell’AEM.
   + Esposizione di un contenuto di varianti di frammento di contenuto come JSON tramite **Servizi di contenuti AEM** e pagine API per casi di utilizzo di sola lettura.
   + Esponendo direttamente i contenuti dei frammenti di contenuto (tutte le varianti) come JSON tramite chiamate dirette ad AEM Assets tramite **API HTTP di AEM Assets** per i casi d’uso CRUD.

## Architettura dei frammenti esperienza

![Architettura dei frammenti esperienza](./assets/experience-fragments-architecture.png)

+ **Modelli modificabili**, che a loro volta sono definiti da **Tipi di modelli modificabili** e un **Implementazione del componente Pagina AEM**, definisci i Componenti AEM consentiti che possono essere utilizzati per comporre un Frammento di esperienza.
+ Il **Frammento esperienza** è un’istanza di un modello modificabile che rappresenta un’esperienza logica.
+ Frammento esperienza **varianti** Tuttavia, è possibile aderire al Modello modificabile con varianti di esperienza (contenuto e progettazione).
+ I frammenti di esperienza possono essere esposti/utilizzati da:
   + Utilizzo di Frammenti esperienza su AEM Sites (o AEM Screens) tramite il componente Frammento esperienza AEM.
   + Esposizione di un contenuto di varianti di frammento esperienza come JSON (con HTML incorporato) tramite **Servizi di contenuti AEM** e pagine API.
   + Esposizione diretta di una variante del frammento esperienza come **&quot;Plain HTML&quot;**.
   + Esportazione di frammenti esperienza in **Adobe Target** come offerte HTML o JSON.
   + AEM Sites supporta le offerte HTML in modalità nativa; tuttavia, le offerte JSON richiedono uno sviluppo personalizzato.

## Risorsa di supporto per i frammenti di contenuto

+ [Guida utente sui frammenti di contenuto](https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Introduzione a Adobe Experience Manager come CMS headless](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/introduction.html?lang=it)
+ [Utilizzo di frammenti di contenuto nell’AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en)
+ [Componente Frammento di contenuto dei componenti core WCM dell’AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=it)
+ [Utilizzo di frammenti di contenuto e AEM headless](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=en)
+ [Guida introduttiva di AEM Content Services](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en)

## Risorsa di supporto per Frammenti esperienza

+ [Adobe di documentazione su Frammenti esperienza](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en)
+ [Frammenti esperienza AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [Utilizzo dei frammenti esperienza AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [Utilizzo dei frammenti esperienza AEM con Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
