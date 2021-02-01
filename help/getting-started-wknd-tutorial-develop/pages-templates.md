---
title: Guida introduttiva  AEM Sites - Pagine e modelli
seo-title: Guida introduttiva  AEM Sites - Pagine e modelli
description: Scopri la relazione tra un componente di base e modelli modificabili per la pagina. Scopri in che modo i componenti core sono associati al progetto e impara le configurazioni avanzate dei criteri dei modelli modificabili per creare un modello di pagina articolo ben strutturato basato su un modello  Adobe XD.
sub-product: sites
feature: template-editor, core-components
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
translation-type: tm+mt
source-git-commit: e03d84f92be11623704602fb448273e461c70b4e
workflow-type: tm+mt
source-wordcount: '1724'
ht-degree: 1%

---


# Pagine e modelli {#pages-and-template}

In questo capitolo verrà esaminata la relazione tra un componente della pagina di base e modelli modificabili. Verrà creato un modello di articolo non formattato basato su alcuni modelli di [AdobeXD](https://www.adobe.com/products/xd.html). Durante il processo di creazione del modello, vengono trattati i componenti core e le configurazioni di criteri avanzati dei modelli modificabili.

## Prerequisiti {#prerequisites}

Esaminare le istruzioni e gli strumenti necessari per configurare un ambiente di sviluppo locale [](overview.md#local-dev-environment).

### Progetto iniziale

>[!NOTE]
>
> Se il capitolo precedente è stato completato correttamente, è possibile riutilizzare il progetto e ignorare i passaggi per il check-out del progetto iniziale.

Controlla il codice della riga di base su cui si basa l&#39;esercitazione:

1. Estrarre il ramo `tutorial/pages-templates-start` da [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
   ```

1. Distribuisci la base di codice in un&#39;istanza AEM locale utilizzando le tue competenze Paradiso:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se utilizzate AEM 6.5 o 6.4, aggiungete il profilo `classic` a qualsiasi comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd/tree/pages-templates/solution) o estrarre il codice localmente passando al ramo `tutorial/pages-templates-solution`.

## Obiettivo

1.  Inspect una progettazione di pagina creata in  Adobe XD e mapparla su Componenti principali.
1. Comprendere i dettagli di Modelli modificabili e come i criteri possono essere utilizzati per applicare un controllo granulare sul contenuto della pagina.
1. Scopri come i modelli e le pagine sono collegati

## Cosa verrà creato {#what-you-will-build}

In questa parte dell&#39;esercitazione, verrà creato un nuovo Modello pagina articolo che potrà essere utilizzato per creare nuove pagine articolo e allinearsi con una struttura comune. Il Modello pagina articolo si basa sulle progettazioni e su un Kit interfaccia utente prodotto in AdobeXD. Questo capitolo si concentra solo sulla costruzione della struttura o dello scheletro del modello. Non verrà implementato alcun stile, ma il modello e le pagine saranno funzionali.

![Progettazione di pagine articolo e versione non formattata](assets/pages-templates/what-you-will-build.png)

## Pianificazione dell&#39;interfaccia utente con  Adobe XD {#adobexd}

Nella maggior parte dei casi, la pianificazione di un nuovo sito Web inizia con modelli e design statici. [ Adobe ](https://www.adobe.com/products/xd.html) XD è uno strumento di progettazione che crea esperienze utente. Successivamente verranno esaminati un kit di interfaccia utente e i modelli per pianificare la struttura del modello di pagina dell&#39;articolo.

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**Scaricate il file [ ](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)** WKND Article Design.

## Creare il modello di pagina dell&#39;articolo

Quando si crea una pagina, è necessario selezionare un modello da utilizzare come base per la creazione della nuova pagina. Il modello definisce la struttura della pagina risultante, il contenuto iniziale e i componenti consentiti.

Esistono 3 aree principali di [Modelli modificabili](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html):

1. **Struttura** : definisce i componenti che fanno parte del modello. Gli autori dei contenuti non potranno modificarli.
1. **Contenuto**  iniziale: definisce i componenti con i quali il modello inizierà, che possono essere modificati e/o eliminati dagli autori dei contenuti
1. **Criteri** : definisce le configurazioni relative al funzionamento dei componenti e alle opzioni che gli autori avranno a disposizione.

Quindi, create un nuovo modello in AEM che corrisponda alla struttura dei modelli. Ciò si verificherà in un&#39;istanza locale di AEM. Seguite i passaggi descritti nel video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/330991/?quality=12&learn=on)

## Aggiornare l&#39;intestazione e il piè di pagina con i frammenti esperienza {#experience-fragments}

Durante la creazione di contenuto globale, ad esempio un&#39;intestazione o un piè di pagina, è pratica comune utilizzare un [frammento esperienza](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Frammenti esperienza, consente agli utenti di combinare più componenti per creare un singolo componente che possa essere utilizzato come riferimento. I frammenti esperienza hanno il vantaggio di supportare la gestione multisito e la [localizzazione](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure).

Il tipo di archivio AEM progetti generava un&#39;intestazione e un piè di pagina. Quindi, aggiornate i frammenti esperienza in modo che corrispondano ai modelli. Seguite i passaggi descritti nel video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/330992/?quality=12&learn=on)

Scaricate e installate il pacchetto di contenuti di esempio **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets.zip)**.

## Creare una pagina articolo

Quindi, create una nuova pagina utilizzando il modello Pagina articolo. Creare il contenuto della pagina in modo che corrisponda ai modelli del sito. Seguite i passaggi descritti nel video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/330993/?quality=12&learn=on)

##  Inspect la struttura del nodo {#node-structure}

A questo punto la pagina dell&#39;articolo è chiaramente priva di stile. Tuttavia, la struttura di base è in atto. Quindi, esaminate la struttura dei nodi della pagina dell&#39;articolo per comprendere meglio il ruolo del modello, della pagina e dei componenti.

Utilizzare lo strumento CRXDE-Lite su un&#39;istanza AEM locale per visualizzare la struttura del nodo sottostante.

1. Aprire [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) e utilizzare la struttura di navigazione per passare a `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. Fare clic sul nodo `jcr:content` sotto la pagina `la-skateparks` e visualizzare le proprietà:

   ![Proprietà contenuto JCR](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   Notate il valore di `cq:template`, che indica `/conf/wknd/settings/wcm/templates/article-page`, il Modello pagina articolo creato in precedenza.

   Notate anche il valore di `sling:resourceType`, che punta a `wknd/components/page`. Si tratta del componente pagina creato dall&#39;archetipo AEM progetto ed è responsabile del rendering della pagina in base al modello.

1. Espandere il nodo `jcr:content` sotto `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` e visualizzare la gerarchia dei nodi:

   ![JCR Content LA Skatepars](assets/pages-templates/page-jcr-structure.png)

   È necessario essere in grado di mappare liberamente ciascuno dei nodi ai componenti creati. Verificare se è possibile identificare i diversi Contenitori di layout utilizzati dall&#39;analisi dei nodi con il prefisso `container`.

1. Quindi ispezionare il componente pagina in `/apps/wknd/components/page`. Visualizzare le proprietà del componente in CRXDE Lite:

   ![Proprietà dei componenti pagina](assets/pages-templates/page-component-properties.png)

   Al di sotto del componente della pagina sono presenti solo 2 script HTL, `customfooterlibs.html` e `customheaderlibs.html`. *Quindi, in che modo questo componente esegue il rendering della pagina?*

   La proprietà `sling:resourceSuperType` punta a `core/wcm/components/page/v2/page`. Questa proprietà consente al componente pagina del WKND di ereditare **all** la funzionalità del componente pagina del componente core. Questo è il primo esempio di un elemento chiamato modello di componente proxy](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern). [ Ulteriori informazioni sono disponibili [qui.](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html).

1.  Inspect un altro componente all’interno dei componenti WKND, il componente `Breadcrumb` che si trova in: `/apps/wknd/components/breadcrumb`. Notate che è possibile trovare la stessa proprietà `sling:resourceSuperType`, ma questa volta indica `core/wcm/components/breadcrumb/v2/breadcrumb`. Si tratta di un altro esempio di utilizzo del pattern del componente Proxy per includere un componente Core. Infatti, tutti i componenti della base di codice WKND sono proxy di componenti core AEM (ad eccezione del nostro famoso componente HelloWorld). È consigliabile provare a riutilizzare il maggior numero possibile di funzionalità dei componenti core *prima di* per scrivere codice personalizzato.

1. Quindi ispezionare la pagina del componente di base in `/libs/core/wcm/components/page/v2/page` utilizzando CRXDE Lite:

   >[!NOTE]
   >
   > Nella AEM 6.5/6.4 i componenti core si trovano in `/apps/core/wcm/components`. In AEM come Cloud Service, i componenti core si trovano in `/libs` e vengono aggiornati automaticamente.

   ![Pagina componente di base](assets/pages-templates/core-page-component-properties.png)

   Si noti che sotto questa pagina sono inclusi molti altri script. La pagina dei componenti core contiene molte funzionalità. Questa funzionalità è suddivisa in più script per semplificare la manutenzione e la leggibilità. Per tracciare l&#39;inclusione degli script HTL, aprire `page.html` e cercare `data-sly-include`:

   ```html
   <!--/* /libs/core/wcm/components/page/v2/page/page.html */-->
   <!DOCTYPE HTML>
   <html data-sly-use.page="com.adobe.cq.wcm.core.components.models.Page" lang="${page.language}"
       data-sly-use.head="head.html"
       data-sly-use.footer="footer.html"
       data-sly-use.redirect="redirect.html">
       <head data-sly-call="${head.head @ page = page}"></head>
       <body class="${page.cssClassNames}"
           id="${page.id}"
           data-cmp-data-layer-enabled="${page.data ? true : false}">
           <script data-sly-test.dataLayerEnabled="${page.data}">
           window.adobeDataLayer = window.adobeDataLayer || [];
           adobeDataLayer.push({
               page: JSON.parse("${page.data.json @ context='scriptString'}"),
               event:'cmp:show',
               eventInfo: {
                   path: 'page.${page.id @ context="scriptString"}'
               }
           });
           </script>
           <sly data-sly-test.isRedirectPage="${page.redirectTarget && (wcmmode.edit || wcmmode.preview)}"
               data-sly-call="${redirect.redirect @ redirectTarget = page.redirectTarget}"></sly>
           <sly data-sly-test="${!isRedirectPage}">
               <sly data-sly-include="body.skiptomaincontent.html"></sly>
               <sly data-sly-include="body.socialmedia_begin.html"></sly>
               <sly data-sly-include="body.html"></sly>
               <sly data-sly-call="${footer.footer @ page = page}"></sly>
               <sly data-sly-include="body.socialmedia_end.html"></sly>
           </sly>
       </body>
   </html>
   ```

   L&#39;altro motivo per suddividere l&#39;HTL in più script è consentire ai componenti proxy di ignorare i singoli script per implementare la logica aziendale personalizzata. Gli script HTL, `customfooterlibs.html` e `customheaderlibs.html`, vengono creati affinché lo scopo esplicito sia ignorato dall&#39;implementazione dei progetti.

   Per saperne di più su come i fattori di modello modificabile possono essere utilizzati per il rendering della pagina di contenuto [leggendo questo articolo](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html).

1.  Inspect come altro componente core, come Breadcrumb in `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`. Visualizzare lo script `breadcrumb.html` per comprendere in che modo viene generata la marcatura per il componente Breadcrumb.

## Salvataggio delle configurazioni nel controllo del codice sorgente {#configuration-persistence}

In molti casi, specialmente all&#39;inizio di un progetto AEM, è utile mantenere configurazioni, come modelli e relativi criteri di contenuto, al controllo del codice sorgente. In questo modo tutti gli sviluppatori lavorano con lo stesso set di contenuti e configurazioni e possono garantire un&#39;ulteriore coerenza tra gli ambienti. Una volta raggiunto un certo livello di maturità, la gestione dei modelli può essere affidata a un gruppo speciale di utenti.

Per ora i modelli saranno trattati come altri elementi di codice e sincronizzeremo il **Modello pagina articolo** verso il basso come parte del progetto. Fino ad ora abbiamo **inviato** il codice dal nostro progetto AEM a un&#39;istanza locale di AEM. Il **Modello pagina articolo** è stato creato direttamente su un&#39;istanza locale di AEM, quindi è necessario **importare** il modello nel nostro progetto AEM. Il modulo **ui.content** è incluso nel progetto AEM per questo scopo specifico.

I passaggi successivi verranno eseguiti utilizzando l&#39;IDE VSCode utilizzando il plug-in [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview), ma potrebbe essere possibile utilizzare qualsiasi IDE configurato per **importare** o importare contenuti da un&#39;istanza locale di AEM.

1. In VSCode aprire il progetto `aem-guides-wknd`.

1. Espandete il modulo **ui.content** in Project Explorer. Espandete la cartella `src` e passate a `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL Fate ] clic con il pulsante destro del mouse sulla  `templates` cartella e selezionate  **Importa da AEM server**:

   ![Modello di importazione VSCode](assets/pages-templates/vscode-import-templates.png)

   È necessario importare i `article-page` e aggiornare anche i modelli `page-content` `xf-web-variation`.

   ![Modelli aggiornati](assets/pages-templates/updated-templates.png)

1. Ripetere i passaggi per importare il contenuto, ma selezionare la cartella **policy** che si trova in `/conf/wknd/settings/wcm/policies`.

   ![Criteri di importazione VSCode](assets/pages-templates/policies-article-page-template.png)

1.  Inspect il file `filter.xml` che si trova in `ui.content/src/main/content/META-INF/vault/filter.xml`.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
   <workspaceFilter version="1.0">
       <filter root="/conf/wknd" mode="merge"/>
       <filter root="/content/wknd" mode="merge"/>
       <filter root="/content/dam/wknd" mode="merge"/>
       <filter root="/content/experience-fragments/wknd" mode="merge"/>
   </workspaceFilter>
   ```

   Il file `filter.xml` è responsabile dell&#39;identificazione dei percorsi dei nodi che verranno installati con il pacchetto. Notate che `mode="merge"` su ciascuno dei filtri indica che il contenuto esistente non verrà modificato, ma che viene aggiunto solo nuovo contenuto. Poiché gli autori dei contenuti potrebbero aggiornare questi percorsi, è importante che la distribuzione del codice **non** sovrascriva il contenuto. Per ulteriori informazioni sull&#39;utilizzo degli elementi del filtro, consultare la [Documentazione FileVault](https://jackrabbit.apache.org/filevault/filter.html).

   Confrontate `ui.content/src/main/content/META-INF/vault/filter.xml` e `ui.apps/src/main/content/META-INF/vault/filter.xml` per comprendere i diversi nodi gestiti da ciascun modulo.

   >[!WARNING]
   >
   > Per garantire distribuzioni coerenti per il sito WKND Reference, alcuni rami del progetto sono configurati in modo che `ui.content` sovrascrivano eventuali modifiche nel JCR. Questo è per progettazione, vale a dire per i rami della soluzione, in quanto il codice/gli stili saranno scritti per criteri specifici.

## Congratulazioni! {#congratulations}

Congratulazioni, hai appena creato un nuovo modello e una nuova pagina con  Adobe Experience Manager Sites.

### Passaggi successivi {#next-steps}

A questo punto la pagina dell&#39;articolo è chiaramente priva di stile. Seguite l&#39;esercitazione [Client-Side Libraries and Front-end Workflow](client-side-libraries.md) per apprendere le procedure ottimali per l&#39;inclusione di CSS e Javascript per applicare stili globali al sito e integrare una build front-end dedicata.

Visualizzate il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd) oppure rivedete e distribuite il codice localmente in corrispondenza del blocco Git `tutorial/pages-templates-solution`.

1. Duplicare il repository [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Estrarre il ramo `tutorial/pages-templates-solution`.
