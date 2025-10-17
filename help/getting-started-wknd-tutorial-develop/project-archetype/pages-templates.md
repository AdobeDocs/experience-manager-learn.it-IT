---
title: 'Guida introduttiva ad AEM Sites: pagine e modelli'
description: Scopri la relazione tra un componente della pagina base e i modelli modificabili. Scopri in che modo i Componenti core vengono inseriti come proxy nel progetto. Scopri le configurazioni avanzate dei criteri dei modelli modificabili per creare un modello di pagina di articolo ben strutturato basato su un modello di Adobe XD.
feature: Core Components, Editable Templates, Page Editor
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-4082
thumbnail: 30214.jpg
doc-type: Tutorial
exl-id: e9d06dc2-ac3b-48c5-ae00-fdaf5bb45b54
recommendations: noDisplay, noCatalog
duration: 2049
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '2898'
ht-degree: 100%

---

# Pagine e modelli {#pages-and-template}

{{edge-delivery-services-and-page-editor}}

In questo capitolo, esaminiamo la relazione tra un componente della pagina base e i modelli modificabili. Scopri come creare un modello di articolo senza stili basato su alcuni modelli di [Adobe XD](https://helpx.adobe.com/it/support/xd.html). Durante la creazione del modello, sono trattati i Componenti core e le configurazioni avanzate dei criteri dei modelli modificabili.

## Prerequisiti {#prerequisites}

Revisione degli strumenti e delle istruzioni necessari per configurare un [ambiente di sviluppo locale](overview.md#local-dev-environment).

### Progetto iniziale

>[!NOTE]
>
> Se hai completato correttamente il capitolo precedente, puoi riutilizzare il progetto e saltare i passaggi per controllare il progetto iniziale.

Controlla il codice della riga di base su cui si fonda l’esercitazione:

1. Controlla il ramo `tutorial/pages-templates-start` da [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
   ```

1. Distribuisci la base di codice in un’istanza AEM locale utilizzando le abilità Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se utilizzi AEM 6.5 o 6.4, aggiungi il profilo `classic` a qualsiasi comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/pages-templates-solution) o estrarlo localmente passando al ramo `tutorial/pages-templates-solution`.

## Obiettivo

1. Esamina la progettazione di una pagina creata in Adobe XD e mappala sui Componenti core.
1. Scopri i dettagli dei modelli modificabili e come utilizzare i criteri per applicare un controllo granulare del contenuto della pagina.
1. Scopri come vengono collegati modelli e pagine

## Cosa stai per creare {#what-build}

In questa parte dell’esercitazione, creerai un nuovo Modello per pagina di articolo che può essere utilizzato per creare pagine di articoli e allinearle con una struttura comune. Il modello per pagina di articolo si basa sulle progettazioni e su un kit di interfaccia utente prodotto in Adobe XD. Questo capitolo si concentra solo sulla creazione della struttura o dell’ossatura del modello. Non vengono implementati stili, ma il modello e le pagine funzionano.

![Progettazione pagina articolo e versione non formattata](assets/pages-templates/what-you-will-build.png)

## Pianificazione dell’interfaccia utente con Adobe XD {#adobexd}

Di solito, la pianificazione di un nuovo sito web inizia con modelli e progetti statici. [Adobe XD](https://helpx.adobe.com/it/support/xd.html) è uno strumento di progettazione che crea l’esperienza utente. Ora sarà esaminato un kit di interfaccia utente e modelli che consentono di pianificare la struttura del modello per la pagina dell’articolo.

>[!VIDEO](https://video.tv.adobe.com/v/30214?quality=12&learn=on)

**Scarica il [file dell’articolo della progettazione WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**.

>[!NOTE]
>
> Un [kit di interfaccia utente per componenti core di AEM generico è disponibile](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd?lang=it) come punto di partenza per i progetti personalizzati.

## Creare il modello per la pagina dell’articolo

Quando crei una pagina, devi selezionare un modello che viene utilizzato come base per la creazione. Il modello definisce la struttura della pagina risultante, il contenuto iniziale e i componenti che possono essere utilizzati.

Sono disponibili tre aree principali di [modelli modificabili](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=it):

1. **Struttura**: definisce i componenti che fanno parte del modello. Non sono modificabili da chi crea il contenuto.
1. **Contenuto iniziale**: definisce i componenti con cui inizia il modello, che possono essere modificati e/o eliminati dagli autori del contenuto
1. **Criteri**: definisce le configurazioni sul comportamento dei componenti e sulle opzioni di cui gli autori dispongono.

Quindi, crea un modello in AEM che corrisponda alla struttura dei modelli. Ciò si verifica in un’istanza locale di AEM. Segui i passaggi descritti nel video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/330991?quality=12&learn=on)

Passaggi principali per il video precedente:

### Configurazioni della struttura

1. Crea un modello utilizzando il **tipo di modello della pagina**, denominato **Pagina articolo**.
1. Passa alla modalità **Struttura**.
1. Aggiungi un componente **Frammento di esperienza** che funga da **intestazione** nella parte superiore del modello.
   * Configurare il componente in modo che punti a `/content/experience-fragments/wknd/us/en/site/header/master`.
   * Imposta il criterio su **Intestazione pagina** e assicurati che l’**elemento predefinito** sia impostato su `header`. L’elemento `header`ha come destinazione CSS nel capitolo successivo.
1. Aggiungi un componente **Frammento di esperienza** che funga da **piè di pagina** nella parte inferiore del modello.
   * Configurare il componente in modo che punti a `/content/experience-fragments/wknd/us/en/site/footer/master`.
   * Imposta il criterio su **Piè di pagina** e assicurati che l’**elemento predefinito** sia impostato su `footer`. L’elemento `footer`ha come destinazione CSS nel capitolo successivo.
1. Blocca il contenitore **principale** incluso al momento della creazione iniziale del modello.
   * Imposta il criterio su **Pagina principale** e assicurati che l’**elemento predefinito** sia impostato su `main`. L’elemento `main` ha come destinazione CSS nel capitolo successivo.
1. Aggiungi un componente **immagine** al contenitore **principale**.
   * Sblocca il componente **Immagine**.
1. Aggiungi un componente **Breadcrumb** sotto il componente **Immagine** nel contenitore principale.
   * Crea un criterio per il componente **Breadcrumb** denominato **Pagina articolo: Breadcrumb**. Imposta il **livello iniziale di navigazione** su **4**.
1. Aggiungi un componente **Contenitore** sotto il componente **Breadcrumb** e all’interno del contenitore **principale**. Questo funge da **contenitore del contenuto** per il modello.
   * Sblocca il contenitore del **contenuto**.
   * Imposta il criterio su **Contenuto della pagina**.
1. Aggiungi un altro componente **Contenitore** sotto il **contenitore del contenuto**. Questo funge da contenitore della **barra laterale** per il modello.
   * Sblocca il contenitore della **barra laterale**.
   * Crea un criterio denominato **Pagina articolo: barra laterale**.
   * Configura i **componenti consentiti** in **Progetto WKND di Sites: contenuto** per includere: **Pulsante**, **Download**, **Immagine**, **Elenco**, **Separatore**, **Condivisione social media**, **Testo** e **Titolo**.
1. Aggiorna il criterio del contenitore Pagina principale. Questo è il contenitore più esterno del modello. Imposta il criterio su **Pagina principale**.
   * Nelle **impostazioni del contenitore**, imposta il **layout** su **Griglia reattiva**.
1. Integra la modalità layout per il **contenitore di contenuti**. Trascina la maniglia da destra a sinistra e riduci il contenitore in modo che abbia una larghezza di otto colonne.
1. Integra la modalità layout per il **contenitore della barra laterale**. Trascina la maniglia da destra a sinistra e riduci il contenitore in modo che abbia una larghezza di quattro colonne. Trascina quindi la maniglia sinistra da sinistra a destra di una colonna per rendere il contenitore largo 3 colonne e lascia uno spazio di 1 colonna tra il **contenitore di contenuti**.
1. Apri l’emulatore mobile e passa a un punto di interruzione mobile. Integra la modalità layout e imposta il **contenitore di contenuti** e il **contenitore della barra laterale** per l’intera larghezza della pagina. In questo modo i contenitori vengono impilati verticalmente nel punto di interruzione mobile.
1. Aggiorna il criterio del componente **Testo** nel **contenitore di contenuti**.
   * Imposta il criterio su **Testo contenuto**.
   * In **Plug-in** > **Stili paragrafo**, seleziona **Abilita stili paragrafo** e assicurati che il **blocco citazione** sia abilitato.

### Configurazioni del contenuto iniziale

1. Passa alla modalità **Contenuto iniziale**.
1. Aggiungi un componente **Titolo** al **contenitore di contenuti**. Questo funge da titolo dell’articolo. Quando viene lasciato vuoto, visualizza automaticamente il Titolo della pagina corrente.
1. Aggiungi un secondo componente **Titolo** sotto il primo componente Titolo.
   * Configura il componente con il testo: &quot;Per autore&quot;. È il segnaposto per il testo.
   * Imposta il tipo su `H4`.
1. Aggiungi un componente **Testo** sotto il componente Titolo **Autore**.
1. Aggiungi un componente **Titolo** al **Contenitore barra laterale**.
   * Configura il componente con il testo: &quot;Condividi questa storia&quot;.
   * Imposta il tipo su `H5`.
1. Aggiungi un componente **Condivisione social media** sotto il componente titolo **Condividi questa storia**.
1. Aggiungi un componente **Separatore** sotto il componente **Condivisione social media**.
1. Aggiungi un componente **Scarica** sotto il componente **Separatore**.
1. Aggiungi un componente **Elenco** sotto il componente **Scarica**.
1. Aggiorna le **Proprietà pagina iniziale** per il modello.
   * In **Social Media** > **Condivisione social media**, seleziona **Facebook** e **Pinterest**

### Abilitare il modello e aggiungere una miniatura

1. Visualizza il modello nella console Modelli passando a [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)
1. **Abilita** il modello della pagina dell&#39;articolo.
1. Modifica le proprietà del modello Pagina articolo e carica la miniatura seguente per identificare rapidamente le pagine create utilizzando il modello Pagina articolo:

   ![Miniatura modello pagina articolo](assets/pages-templates/article-page-template-thumbnail.png)

## Aggiornare intestazione e piè di pagina con frammenti di esperienza {#experience-fragments}

Per la creazione di un contenuto globale, ad esempio un&#39;intestazione o un piè di pagina, è in genere consigliabile utilizzare un [Frammento di esperienza](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=it). I frammenti di esperienza consentono agli utenti di combinare più componenti per creare un singolo componente di riferimento. I frammenti di esperienza hanno il vantaggio di supportare la gestione multisito e la [localizzazione](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/experience-fragment.html?lang=it).

L’Archetipo progetto AEM ha generato un’intestazione e un piè di pagina. Quindi, aggiorna i Frammenti di esperienza in modo che corrispondano ai modelli. Segui i passaggi descritti nel video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/330992?quality=12&learn=on)

Passaggi principali per il video precedente:

1. Scarica il pacchetto di contenuti di esempio **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets-1.1.zip)**.
1. Carica e installa il pacchetto di contenuti utilizzando Gestione pacchetti all&#39;indirizzo [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. Aggiorna il modello Variante Web, che è il modello utilizzato per i frammenti di esperienza in [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)
   * Aggiorna il criterio del componente **Contenitore** nel modello.
   * Imposta il criterio su **XF Root**.
   * In **Componenti consentiti** seleziona il gruppo di componenti **Progetto WKND Sites - Struttura** per includere i componenti **Navigazione lingua**, **Navigazione** e **Ricerca rapida**.

### Aggiornare il frammento di esperienza dell&#39;intestazione

1. Apri il frammento di esperienza che esegue il rendering dell’intestazione in [http://localhost:4502/editor.html/content/experience-fragments/wknd/it/it/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)
1. Configura il **contenitore** principale del frammento. Questo è il **contenitore** più esterno.
   * Imposta il **layout** su **Griglia reattiva**
1. Aggiungi il **logo WKND Dark** come immagine nella parte superiore del **contenitore**. Il logo è stato incluso nel pacchetto installato in un passaggio precedente.
   * Modifica il layout del **logo WKND Dark** in modo che sia largo **due** colonne. Trascina le maniglie da destra a sinistra.
   * Configura il logo con il **testo alternativo** “Logo WKND”.
   * Configura il logo per **collegare** `/content/wknd/us/en` alla pagina Home.
1. Configura il componente **Navigazione** già inserito nella pagina.
   * Imposta **Escludi livelli principali** su **1**.
   * Imposta la **profondità della struttura di navigazione** su **1**.
   * Modifica il layout del componente **Navigazione** in modo che sia largo **otto** colonne. Trascina le maniglie da destra a sinistra.
1. Rimuovi il componente **Navigazione lingua**.
1. Modifica il layout del componente **Ricerca** in modo che sia largo **due** colonne. Trascina le maniglie da destra a sinistra. Ora tutti i componenti devono essere allineati orizzontalmente su un’unica riga.

### Aggiornare il frammento di esperienza del piè di pagina

1. Apri il frammento di esperienza che esegue il rendering del piè di pagina in [http://localhost:4502/editor.html/content/experience-fragments/wknd/it/it/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)
1. Configura il **contenitore** principale del frammento. Questo è il **contenitore** più esterno.
   * Imposta il **layout** su **Griglia reattiva**
1. Aggiungi il **logo WKND Light** come immagine nella parte superiore del **contenitore**. Il logo è stato incluso nel pacchetto installato in un passaggio precedente.
   * Modifica il layout del **logo WKND Light** in modo che sia largo **due** colonne. Trascina le maniglie da destra a sinistra.
   * Configura il logo con il **testo alternativo** “WKND Logo Light”.
   * Configura il logo per **collegare** `/content/wknd/us/en` alla pagina Home.
1. Aggiungi un componente **Navigazione** sotto il logo. Configura il componente **Navigazione**:
   * Imposta **Escludi livelli principali** su **1**.
   * Deseleziona **Raccogli tutte le pagine secondarie**.
   * Imposta la **profondità della struttura di navigazione** su **1**.
   * Modifica il layout del componente **Navigazione** in modo che sia largo **otto** colonne. Trascina le maniglie da destra a sinistra.

## Creare una pagina dell’articolo

Quindi, crea una pagina utilizzando il modello Pagina articolo. Crea il contenuto della pagina in modo che corrisponda ai modelli del sito. Segui i passaggi descritti nel video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/330993?quality=12&learn=on)

Passaggi principali per il video precedente:

1. Passa alla console di Sites all’indirizzo [http://localhost:4502/sites.html/content/wknd/it/it/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine).
1. Crea una pagina sotto **WKND** > **IT** > **IT** > **Magazine**.
   * Scegli il modello **Pagina articolo**.
   * In **Proprietà** imposta il **Titolo** su “Ultimate Guide to LA Skateparks”
   * Imposta il **Nome** su “guide-la-skateparks”
1. Sostituisci il titolo **Autore** con il testo “Stacey Roswells”.
1. Aggiorna il componente **Testo** per includere un paragrafo per compilare l’articolo. Puoi utilizzare il seguente file di testo come copia: [la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt).
1. Aggiungi un altro componente **Testo**.
   * Aggiorna il componente per includere la citazione: &quot;Non c&#39;è posto migliore da condividere di Los Angeles&quot;.
   * Modifica l&#39;Editor Rich Text in modalità a schermo intero e modifica le virgolette per utilizzare l&#39;elemento **Blocco virgolette**.
1. Continua a popolare il corpo dell’articolo in modo che corrisponda ai modelli.
1. Configura il componente **Scarica** per utilizzare una versione PDF dell&#39;articolo.
   * In **Scarica** > **Proprietà**, fai clic sulla casella di controllo per **Ottieni il titolo dalla risorsa DAM**.
   * Imposta la **Descrizione** su: &quot;Ottieni la storia completa&quot;.
   * Imposta il **Testo azione** su: &quot;Scarica PDF&quot;.
1. Configura il componente **Elenco**.
   * In **Impostazioni elenco** > **Genera elenco con**, seleziona **Pagine secondarie**.
   * Imposta la **Pagina principale** su `/content/wknd/us/en/magazine`.
   * In, **Impostazioni elemento** seleziona **Collega elementi** e **Mostra data**.

## Controllare la struttura del nodo {#node-structure}

A questo punto, la pagina dell’articolo è chiaramente senza formattazione. Tuttavia la struttura di base è creata. Quindi, esamina la struttura dei nodi della pagina dell’articolo per comprendere meglio il ruolo del modello, della pagina e dei componenti.

Utilizza lo strumento CRXDE-Lite su un’istanza AEM locale per visualizzare la struttura del nodo sottostante.

1. Apri [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) e utilizza la struttura di navigazione per passare a `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. Fai clic sul nodo `jcr:content` sotto la pagina `la-skateparks` per visualizzare le proprietà:

   ![Proprietà contenuto JCR](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   Osserva il valore per `cq:template`, che punta a `/conf/wknd/settings/wcm/templates/article-page`, il modello della pagina dell&#39;articolo creato in precedenza.

   Tieni presente anche il valore di `sling:resourceType`, che punta a `wknd/components/page`. Questo è il componente pagina creato dall’archetipo del progetto AEM ed è responsabile del rendering della pagina in base al modello.

1. Espandi il nodo `jcr:content` sotto `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` per visualizzare la gerarchia dei nodi:

   ![JCR Content LA Skateparks](assets/pages-templates/page-jcr-structure.png)

   Dovresti essere in grado di mappare liberamente ciascuno dei nodi ai componenti creati. Verifica se è possibile identificare i diversi contenitori di layout utilizzati esaminando i nodi con prefisso `container`.

1. Esamina quindi il componente pagina in `/apps/wknd/components/page`. Visualizza le proprietà del componente in CRXDE Lite:

   ![Proprietà componente pagina](assets/pages-templates/page-component-properties.png)

   Ci sono solo due script HTL, `customfooterlibs.html` e `customheaderlibs.html` sotto il componente pagina. *Come viene eseguito il rendering della pagina da questo componente?*

   La proprietà `sling:resourceSuperType` punta a `core/wcm/components/page/v2/page`. Questa proprietà consente al componente pagina del WKND di ereditare **tutte** le funzionalità del componente pagina del componente core. Questo è il primo esempio di un [Modello di componente proxy](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html?lang=it#ProxyComponentPattern). Ulteriori informazioni sono disponibili [qui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html?lang=it).

1. Esamina un altro componente all&#39;interno dei componenti WKND, il componente `Breadcrumb` di: `/apps/wknd/components/breadcrumb`. Puoi trovare la stessa proprietà `sling:resourceSuperType`, ma questa volta punta a `core/wcm/components/breadcrumb/v2/breadcrumb`. Questo è un altro esempio di utilizzo del modello di componente Proxy per includere un Componente core. Infatti, tutti i componenti nella base di codice WKND sono proxy dei Componenti core di AEM (ad eccezione del componente demo personalizzato HelloWorld). È consigliabile riutilizzare quante più funzionalità dei Componenti core possibili *prima* della scrittura di codice personalizzato.

1. Esamina quindi la pagina dei Componenti core in `/libs/core/wcm/components/page/v2/page` utilizzando CRXDE Lite:

   >[!NOTE]
   >
   > In AEM 6.5/6.4, i Componenti core si trovano in `/apps/core/wcm/components`. In AEM as a Cloud Service, i Componenti core si trovano in `/libs` e vengono aggiornati automaticamente.

   ![Pagina dei Componenti core](assets/pages-templates/core-page-component-properties.png)

   Tieni presente che molti file di script sono inclusi in questa pagina. La pagina dei Componenti core contiene numerose funzionalità. Questa funzionalità è suddivisa in più script per facilitarne la manutenzione e la leggibilità. Per tracciare l&#39;inclusione degli script HTL, apri `page.html` e cerca `data-sly-include`:

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

   L’altro motivo per suddividere HTL in più script è quello di consentire ai componenti proxy di ignorare singoli script per implementare una logica di business personalizzata. Gli script HTL `customfooterlibs.html` e `customheaderlibs.html` sono creati allo scopo esplicito di essere sostituiti dall’implementazione dei progetti.

   Leggi questo articolo ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=it) per saperne di più sul modo in cui il modello modificabile influisce sul rendering della pagina del contenuto [.

1. Esaminare un altro Componente core, ad esempio Breadcrumb in `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`. Visualizza lo script `breadcrumb.html` per comprendere come viene generato il markup per il componente Breadcrumb.

## Salvataggio delle configurazioni in Controllo codice sorgente {#configuration-persistence}

Spesso, soprattutto all’inizio di un progetto AEM, è utile mantenere le configurazioni, come i modelli e i criteri dei contenuti correlati, nel controllo del codice sorgente. In questo modo tutti gli sviluppatori lavorano sullo stesso set di contenuti e configurazioni e possono garantire ulteriore coerenza tra gli ambienti. Quando un progetto raggiunge un certo livello di maturità, la pratica di gestione dei modelli può essere affidata a uno speciale gruppo di utenti esperti.


Per il momento, i modelli vengono trattati come altre parti di codice e sincronizzano il **Modello pagina articolo** come parte del progetto.
Finora il codice viene inviato dal progetto AEM a un’istanza locale di AEM. Il **Modello pagina articolo** è stato creato direttamente in un&#39;istanza locale di AEM, pertanto è necessario **importare** il modello nel progetto AEM. Il modulo **ui.content** è incluso nel progetto AEM per questo scopo specifico.

I passaggi successivi vengono eseguiti nell&#39;IDE VSCode utilizzando il plug-in [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&ssr=false#overview). Tuttavia, potrebbero utilizzare qualsiasi IDE configurato per **importare** o importare contenuto da un&#39;istanza locale di AEM.

1. In, VSCode apri il progetto `aem-guides-wknd`.

1. Espandi il modulo **ui.content** in Esplora progetti. Espandi la cartella `src` e passa a `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL Fai clic con il pulsante destro del mouse] sulla cartella `templates` e seleziona **Importa da AEM Server**:

   ![Modello di importazione VSCode](assets/pages-templates/vscode-import-templates.png)

   È necessario importare `article-page` e aggiornare anche i modelli `page-content` e `xf-web-variation`.

   ![Modelli aggiornati](assets/pages-templates/updated-templates.png)

1. Ripeti i passaggi per importare il contenuto, ma seleziona la cartella **criteri** da `/conf/wknd/settings/wcm/policies`.

   ![Criteri di importazione VSCode](assets/pages-templates/policies-article-page-template.png)

1. Controlla il file `filter.xml` da `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   Il file `filter.xml` è responsabile dell&#39;identificazione dei percorsi dei nodi installati con il pacchetto. Osserva `mode="merge"` su ciascuno dei filtri che indica che il contenuto esistente non deve essere modificato, solo il nuovo contenuto viene aggiunto. Poiché gli autori di contenuto potrebbero aggiornare questi percorsi, è importante che una distribuzione del codice **non** sovrascriva il contenuto. Per ulteriori informazioni sull&#39;utilizzo degli elementi del filtro, consulta la [documentazione di FileVault](https://jackrabbit.apache.org/filevault/filter.html).

   Confronta `ui.content/src/main/content/META-INF/vault/filter.xml` e `ui.apps/src/main/content/META-INF/vault/filter.xml` per comprendere i diversi nodi gestiti da ciascun modulo.

   >[!WARNING]
   >
   > Per garantire distribuzioni coerenti per il sito di riferimento WKND, alcuni rami del progetto sono configurati in modo che `ui.content` sovrascriva eventuali modifiche nel JCR. È parte della progettazione, ovvero per i rami della soluzione, in quanto il codice e gli stili sono scritti per criteri specifici.

## Congratulazioni. {#congratulations}

Congratulazioni, hai creato un modello e una pagina con Adobe Experience Manager Sites.

### Passaggi successivi {#next-steps}

A questo punto, la pagina dell’articolo è chiaramente senza formattazione. Segui l&#39;esercitazione [Librerie client e flusso di lavoro front-end](client-side-libraries.md) per scoprire le best practice per includere CSS e JavaScript per applicare stili globali al sito e integrare una build front-end dedicata.

Visualizza il codice finito in [GitHub](https://github.com/adobe/aem-guides-wknd) oppure rivedi e distribuisci il codice localmente nel ramo Git `tutorial/pages-templates-solution`.

1. Clona l&#39;archivio [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Controlla il ramo `tutorial/pages-templates-solution`.
