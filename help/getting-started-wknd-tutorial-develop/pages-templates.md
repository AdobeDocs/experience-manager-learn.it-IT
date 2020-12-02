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
source-git-commit: 836ef9b7f6a9dcb2ac78f5d1320797897931ef8c
workflow-type: tm+mt
source-wordcount: '2226'
ht-degree: 2%

---


# Pagine e modelli {#pages-and-template}

In questo capitolo verrà esaminata la relazione tra un componente della pagina di base e modelli modificabili. Verrà creato un modello di articolo non formattato basato su alcuni modelli di [AdobeXD](https://www.adobe.com/products/xd.html). Durante il processo di creazione del modello, vengono trattati i componenti core e le configurazioni di criteri avanzati dei modelli modificabili.

## Prerequisiti {#prerequisites}

Esaminare le istruzioni e gli strumenti necessari per configurare un ambiente di sviluppo locale [](overview.md#local-dev-environment).

### Progetto iniziale

Controlla il codice della riga di base su cui si basa l&#39;esercitazione:

1. Duplicare il repository [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd).
1. Estrarre il ramo `pages-templates/start`.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
   $ cd ~/code/aem-guides-wknd
   $ git checkout pages-templates/start
   ```

1. Distribuisci la base di codice in un&#39;istanza AEM locale utilizzando le tue competenze Paradiso:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd/tree/pages-templates/solution) o estrarre il codice localmente passando al ramo `pages-templates/solution`.

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

Scaricate il [file WKND Article Design File](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd).

## Creare un&#39;intestazione e un piè di pagina con frammenti esperienza {#experience-fragments}

Durante la creazione di contenuto globale, ad esempio un&#39;intestazione o un piè di pagina, è pratica comune utilizzare un [frammento esperienza](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). I frammenti esperienza consentono di combinare più componenti per creare un singolo componente che possa essere utilizzato come riferimento. I frammenti esperienza hanno il vantaggio di supportare la gestione multisito e ci consentono di gestire intestazioni/piè di pagina diversi per lingua.

Quindi, aggiorneremo il frammento esperienza da utilizzare come intestazione e piè di pagina per aggiungere il logo WKND.

>[!VIDEO](https://video.tv.adobe.com/v/30215/?quality=12&learn=on)

>[!NOTE]
>
> I tuoi frammenti esperienza sono diversi rispetto al video? Provate a eliminarli e reinstallare la base di codice del progetto iniziale.

Di seguito sono riportati i passaggi di alto livello eseguiti nel video precedente.

1. Aggiornate l&#39;intestazione del frammento esperienza in [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html) per includere il logo WKND Dark.

   ![Logo WKND Dark](assets/pages-templates/wknd-logo-dk.png)

   *Logo WKND Dark*

1. Aggiornate l&#39;intestazione del frammento esperienza [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html) per includere il logo WKND Light.

   ![Logo WKND Light](assets/pages-templates/wknd-logo-light.png)

   *Logo WKND Light*

## Creare il modello di pagina dell&#39;articolo

Quando si crea una pagina, è necessario selezionare un modello da utilizzare come base per la creazione della nuova pagina. Il modello definisce la struttura della pagina risultante, il contenuto iniziale e i componenti consentiti.

Esistono 3 aree principali di [Modelli modificabili](https://docs.adobe.com/content/help/it-IT/experience-manager-65/developing/platform/templates/page-templates-editable.html):

1. **Struttura** : definisce i componenti che fanno parte del modello. Gli autori dei contenuti non potranno modificarli.
1. **Contenuto**  iniziale: definisce i componenti con i quali il modello inizierà, che possono essere modificati e/o eliminati dagli autori dei contenuti
1. **Criteri** : definisce le configurazioni relative al funzionamento dei componenti e alle opzioni che gli autori avranno a disposizione.

La prossima operazione da eseguire è creare il Modello pagina articolo. Ciò si verificherà in un&#39;istanza locale di AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30217/?quality=12&learn=on)

Di seguito sono riportati i passaggi di alto livello eseguiti nel video precedente.

1. Andate alla cartella Modello Siti WKND: **Strumenti** > **Generale** > **Modelli** > **Sito WKND**
1. Create un nuovo modello utilizzando il tipo di modello **WKND Site Empty Page** con un titolo di **Article Page Template**
1. In modalità **Struttura**, configurate il modello in modo che includa i seguenti elementi:

   * Intestazione frammento esperienza
   * Immagine
   * Breadcrumb
   * Contenitore - largo 8 colonne Desktop, largo 12 colonne Tablet, Mobile
   * Contenitore - largo 4 colonne, largo 12 colonne Tablet, Mobile
   * Frammento esperienza Piè di pagina

   ![Modello di pagina articolo della modalità Struttura](assets/pages-templates/article-page-template-structure.png)

   *Struttura - Modello pagina articolo*

1. Passate alla modalità **Contenuto iniziale** e aggiungete i seguenti componenti come contenuto iniziale:

   * **Contenitore principale**
      * Titolo - Dimensione predefinita di H1
      * Titolo - *&quot;Per nome autore&quot;* con dimensioni H4
      * Testo - vuoto
   * **Contenitore laterale**
      * Titolo - *&quot;Condividi questa storia&quot;* con una dimensione di H5
      * Condivisione social media
      * Separatore
      * Scarica
      * Elenco

   ![Modello pagina articolo modalità contenuto iniziale](assets/pages-templates/article-page-template-initialcontent.png)

   *Contenuto iniziale - Modello pagina articolo*

1. Aggiornate le **Proprietà pagina iniziale** per consentire la condivisione da parte dell&#39;utente sia per **Facebook** che per **Pinterest**.
1. Caricate un&#39;immagine nelle proprietà **Modello pagina articolo** per identificarla facilmente:

   ![Miniatura modello pagina articolo](assets/pages-templates/article-page-template-thumbnail.png)

   *Miniatura modello pagina articolo*

1. Attivate **Modello pagina articolo** nella cartella [Modelli sito WKND](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd/settings/wcm/templates).

## Creare una pagina articolo

Ora che abbiamo un modello, creiamo una nuova pagina utilizzando quel modello.

1. Scaricate il seguente pacchetto zip, [WKND-PagesTemplates-DAM-Assets.zip](assets/pages-templates/WKND-PagesTemplates-DAM-Assets.zip), quindi installatelo tramite [CRX Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   Il pacchetto precedente installa diverse immagini e risorse sotto `/content/dam/wknd/en/magazine/la-skateparks` da utilizzare per compilare una pagina di articolo nei passaggi successivi.

   *Le immagini e le risorse del pacchetto sopra sono licenze gratuite per gentile concessione di  [Unsplash.com](https://unsplash.com/).*

   ![Risorse DAM di esempio](assets/pages-templates/sample-assets-la-skatepark.png)

1. Create una nuova pagina, sotto **WKND** > **US** > **en**, denominata **Magazine**. Utilizzate il modello **Content Page**.

   Questa pagina aggiungerà una struttura al sito e ci consentirà di vedere il componente Breadcrumb in azione.

1. Create quindi una nuova pagina, sotto **WKND** > **US** > **en** > **Magazine**. Utilizzate il modello **Article Page**. Utilizzate un titolo di **Guida finale a LA Skateparks** e un nome di **guide-la-skateparks**.

   ![Pagina dell&#39;articolo creata all&#39;inizio](assets/pages-templates/create-article-page-nocontent.png)

1. Compilare la pagina con il contenuto per far corrispondere i modelli esaminati in [UI Planning with AdobeXD](#adobexd) porzione. Il testo dell&#39;articolo di esempio può essere [scaricato qui](assets/pages-templates/la-skateparks-copy.txt). Dovrebbe essere possibile creare qualcosa di simile a questo:

   ![Pagina articolo compilata](assets/pages-templates/article-page-unstyled.png)

   >[!NOTE]
   >
   > Il componente Immagine nella parte superiore della pagina può essere modificato ma non rimosso. Il componente breadcrumb viene visualizzato sulla pagina ma non può essere modificato o rimosso.

##  Inspect la struttura del nodo {#node-structure}

A questo punto la pagina dell&#39;articolo è chiaramente priva di stile. Tuttavia, la struttura di base è in atto. Esamineremo quindi la struttura dei nodi della pagina dell&#39;articolo per comprendere meglio il ruolo del modello e del componente della pagina responsabile per il rendering del contenuto.

Possiamo farlo usando lo strumento CRXDE-Lite su un&#39;istanza AEM locale.

1. Aprire [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) e utilizzare la struttura di navigazione per passare a `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. Fare clic sul nodo `jcr:content` sotto la pagina `la-skateparks` e visualizzare le proprietà:

   ![Proprietà contenuto JCR](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   Notate il valore di `cq:template`, che indica `/conf/wknd/settings/wcm/templates/article-page`, il Modello pagina articolo creato in precedenza.

   Notate anche il valore di `sling:resourceType`, che punta a `wknd/components/structure/page`. Si tratta del componente pagina creato dall&#39;archetipo AEM progetto ed è responsabile del rendering della pagina in base al modello.

1. Espandere il nodo `jcr:content` sotto `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` e visualizzare la gerarchia dei nodi:

   ![JCR Content LA Skatepars](assets/pages-templates/page-jcr-structure.png)

   È necessario essere in grado di mappare liberamente ciascuno dei nodi ai componenti creati. Verificare se è possibile identificare i diversi Contenitori di layout utilizzati dall&#39;analisi dei nodi con il prefisso `responsivegrid`.

1. Quindi ispezionare il componente pagina in `/apps/wknd/components/structure/page`. Visualizzare le proprietà del componente in CRXDE Lite:

   ![Proprietà dei componenti pagina](assets/pages-templates/page-component-properties.png)

   Il componente pagina si trova sotto una cartella denominata **structure**. Si tratta di una convenzione che corrisponde alla modalità struttura Editor modello e viene utilizzata per indicare che il componente pagina non è qualcosa con cui gli autori interagiscono direttamente.

   Al di sotto del componente della pagina sono presenti solo 2 script HTL, `customfooterlibs.html` e `customheaderlibs.html`. Quindi, in che modo questo componente esegue il rendering della pagina?

   Notare la proprietà `sling:resourceSuperType` e il valore di `core/wcm/components/page/v2/page`. Questa proprietà consente al componente pagina del WKND di ereditare tutte le funzionalità del componente Pagina del componente Core. Questo è il primo esempio di un elemento chiamato modello di componente proxy[. ](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern) Ulteriori informazioni sono disponibili [qui.](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html).

1.  Inspect un altro componente all’interno dei componenti WKND, il componente `Breadcrumb` che si trova in: `/apps/wknd/components/content/breadcrumb`. Notate che è possibile trovare la stessa proprietà `sling:resourceSuperType`, ma questa volta indica `core/wcm/components/breadcrumb/v2/breadcrumb`. Si tratta di un altro esempio di utilizzo del pattern del componente Proxy per includere un componente Core. Infatti, tutti i componenti della base di codice WKND sono proxy di componenti core AEM (ad eccezione del nostro famoso componente HelloWorld). È consigliabile provare a riutilizzare il maggior numero possibile di funzionalità dei componenti core *prima di* per scrivere codice personalizzato.

1. Quindi ispezionare la pagina del componente di base in `/apps/core/wcm/components/page/v2/page` utilizzando CRXDE Lite:

   ![Pagina componente di base](assets/pages-templates/core-page-component-properties.png)

   Si noti che sotto questa pagina sono inclusi molti altri script. La pagina dei componenti core contiene molte funzionalità. Questa funzionalità è suddivisa in più script per semplificare la manutenzione e la leggibilità. Per tracciare l&#39;inclusione degli script HTL, aprire `page.html` e cercare `data-sly-include`:

   ```html
   <!--/* /apps/core/wcm/components/page/v2/page/page.html */-->
   <!DOCTYPE HTML>
   <html data-sly-use.page="com.adobe.cq.wcm.core.components.models.Page" lang="${page.language}"
           data-sly-use.head="head.html"
           data-sly-use.footer="footer.html"
           data-sly-use.redirect="redirect.html">
       <head data-sly-call="${head.head @ page = page}"></head>
       <body class="${page.cssClassNames}">
           <sly data-sly-test.isRedirectPage="${page.redirectTarget && (wcmmode.edit || wcmmode.preview)}"
               data-sly-call="${redirect.redirect @ redirectTarget = page.redirectTarget}"></sly>
           <sly data-sly-test="${!isRedirectPage}">
               <sly data-sly-include="body.socialmedia_begin.html"></sly>
               <sly data-sly-include="body.html"></sly>
               <sly data-sly-call="${footer.footer @ page = page}"></sly>
               <sly data-sly-include="body.socialmedia_end.html"></sly>
           </sly>
       </body>
   </html>
   ```

   L&#39;altro motivo per suddividere l&#39;HTL in più script è consentire ai componenti proxy di ignorare i singoli script per implementare la logica aziendale personalizzata. Gli script HTL, `customfooterlibs.html` e `customheaderlibs.html`, vengono creati affinché lo scopo esplicito sia ignorato dall&#39;implementazione dei progetti.

   Per saperne di più su come i fattori di modello modificabile possono essere utilizzati per il rendering della pagina di contenuto [leggendo questo articolo](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/templates/page-templates-editable.html#resultant-content-pages).

1.  Inspect come altro componente core, come Breadcrumb in `/apps/core/wcm/components/breadcrumb/v2/breadcrumb`. Visualizzare lo script `breadcrumb.html` per comprendere in che modo viene generata la marcatura per il componente Breadcrumb.

## Salvataggio delle configurazioni nel controllo del codice sorgente {#configuration-persistence}

In molti casi, specialmente all&#39;inizio di un progetto AEM, è utile mantenere configurazioni, come modelli e relativi criteri di contenuto, al controllo del codice sorgente. In questo modo tutti gli sviluppatori lavorano con lo stesso set di contenuti e configurazioni e possono garantire un&#39;ulteriore coerenza tra gli ambienti. Una volta raggiunto un certo livello di maturità, la gestione dei modelli può essere affidata a un gruppo speciale di utenti.

Per ora i modelli saranno trattati come altri elementi di codice e sincronizzeremo il **Modello pagina articolo** verso il basso come parte del progetto. Fino ad ora abbiamo **inviato** il codice dal nostro progetto AEM a un&#39;istanza locale di AEM. Il **Modello pagina articolo** è stato creato direttamente su un&#39;istanza locale di AEM, quindi è necessario **pull** o importare il modello nel progetto AEM. Il modulo **ui.content** è incluso nel progetto AEM per questo scopo specifico.

I passaggi successivi verranno eseguiti utilizzando l&#39;IDE Eclipse, ma potrebbe essere possibile utilizzare qualsiasi IDE configurato per **pull** o importare contenuto da un&#39;istanza locale di AEM.

1. Nell&#39;IDE Eclipse, assicurarsi che un server sia stato avviato il plug-in dello strumento di sviluppo AEM che si collega all&#39;istanza locale di AEM e che il modulo **ui.content** sia stato aggiunto alla configurazione Server.

   ![Connessione Eclipse Server](assets/pages-templates/eclipse-server-started.png)

1. Espandete il modulo **ui.content** in Project Explorer. Espandete la cartella `src` (quella con l&#39;icona GLOS piccola) e passate a `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL Fare clic con il pulsante destro del ] mouse sul  `templates` nodo e selezionare  **Importa dal server...**:

   ![Modello di importazione Eclipse](assets/pages-templates/eclipse-import-templates.png)

   Confermare la finestra di dialogo **Importa da repository** e fare clic su **Fine**. A questo punto, sotto la cartella `templates` è visibile la cartella `article-page-template`.

1. Ripetere i passaggi per importare il contenuto, ma selezionare il nodo **policy** che si trova in `/conf/wknd/settings/wcm/policies`.

   ![Criteri di importazione Eclipse](assets/pages-templates/policies-article-page-template.png)

1.  Inspect il file `filter.xml` che si trova in `src/main/content/META-INF/vault/filter.xml`.

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

Visualizzate il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd) oppure rivedete e distribuite il codice localmente in corrispondenza del blocco Git `pages-templates/solution`.

1. Duplicare il repository [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Estrarre il ramo `pages-templates/solution`.
