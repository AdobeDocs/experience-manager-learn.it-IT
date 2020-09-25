---
title: Sviluppare progetti in AEM
description: Un’esercitazione sullo sviluppo che illustra come sviluppare AEM progetti.  In questa esercitazione verrà creato un modello di progetto personalizzato che potrà essere utilizzato per creare nuovi progetti all'interno di AEM per gestire flussi di lavoro e attività di authoring dei contenuti.
version: 6.3, 6.4, 6.5
feature: projects, workflow
topics: collaboration, development, governance
activity: develop
audience: developer, implementer, administrator
doc-type: tutorial
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '4652'
ht-degree: 0%

---


# Sviluppare progetti in AEM

Questa è un&#39;esercitazione sullo sviluppo che illustra come svilupparsi per [!DNL AEM Projects].  In questa esercitazione verrà creato un modello di progetto personalizzato che potrà essere utilizzato per creare nuovi progetti all&#39;interno di AEM per gestire flussi di lavoro e attività di authoring dei contenuti.

>[!VIDEO](https://video.tv.adobe.com/v/16904/?quality=12&learn=on)

*Questo video offre una breve dimostrazione del flusso di lavoro completato creato nell’esercitazione seguente.*

## Introduzione {#introduction}

[[!DNL AEM Projects]](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html) è una funzione di AEM progettata per semplificare la gestione e il raggruppamento di tutti i flussi di lavoro e le attività associate alla creazione di contenuto nell&#39;ambito di un&#39;implementazione  di AEM Sites o Assets.

AEM progetti viene fornito con diversi modelli [di progetto](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#ProjectTemplates)OOTB. Quando create un nuovo progetto, gli autori possono scegliere tra i modelli disponibili. Le implementazioni di grandi AEM con requisiti aziendali specifici dovranno creare modelli di progetto personalizzati, adattati alle loro esigenze. Creando un modello di progetto personalizzato, gli sviluppatori possono configurare il dashboard del progetto, collegarsi ai flussi di lavoro personalizzati e creare ruoli aziendali aggiuntivi per un progetto. Esamineremo la struttura di un modello di progetto e ne creeremo uno di esempio.

![Scheda progetto personalizzata](./assets/develop-aem-projects/custom-project-card.png)

## Configurazione

Questa esercitazione illustra il codice necessario per creare un modello di progetto personalizzato. Potete scaricare e installare il pacchetto [](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip) allegato in un ambiente locale e seguire l&#39;esercitazione. Potete anche accedere all&#39;intero progetto Maven ospitato su [GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide).

* [Pacchetto di esercitazioni completato](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [Repository del codice completo su GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)

Questa esercitazione presuppone una conoscenza di base delle procedure [di sviluppo](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/the-basics.html) AEM e una certa familiarità con la configurazione [del progetto](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ht-projects-maven.html)AEM Maven. Tutti i codici menzionati sono destinati ad essere utilizzati come riferimento e devono essere distribuiti solo a un&#39;istanza [AEM di sviluppo](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingStarted)locale.

## Struttura di un modello di progetto

I modelli di progetto devono essere posti sotto il controllo del codice sorgente e devono risiedere sotto la cartella dell&#39;applicazione in /apps. Idealmente dovrebbero essere inseriti in una sottocartella con la convenzione di denominazione di ***/projects/templates/**&lt;my-template>. Seguendo questa convenzione di denominazione, tutti i nuovi modelli personalizzati saranno automaticamente disponibili per gli autori durante la creazione di un progetto. La configurazione dei modelli di progetto disponibili è impostata su: **/content/projects/jcr:content** node by the **cq:allowTemplates** property. Per impostazione predefinita, si tratta di un&#39;espressione regolare: **/(apps|libs)/.*/projects/templates/.***

Il nodo principale di un modello di progetto avrà un **jcr:PrimaryType** di **cq:Template**. Sotto il nodo principale di ci sono 3 nodi: **gadget**, **ruoli** e **flussi di lavoro**. Questi nodi **non sono tutti:non strutturati**. Sotto il nodo principale può anche essere un file thumbnail.png che viene visualizzato quando si seleziona il modello nella procedura guidata Crea progetto.

Struttura nodo completa:

```shell
/apps/<my-app>
    + projects (nt:folder)
         + templates (nt:folder)
              + <project-template-root> (cq:Template)
                   + gadgets (nt:unstructured)
                   + roles (nt:unstructured)
                   + workflows (nt:unstructured)
```

### Radice modello di progetto

Il nodo principale del modello di progetto sarà di tipo **cq:Template**. Su questo nodo è possibile configurare le proprietà **jcr:title** e **jcr:description** che verranno visualizzate nella Creazione guidata progetto. È inoltre disponibile una proprietà denominata **procedura guidata** che punta a un modulo per la compilazione delle proprietà del progetto. Il valore predefinito è: **/libs/cq/core/content/projects/wizard/steps/defaultproject.html** funziona bene per la maggior parte dei casi, in quanto consente all&#39;utente di compilare le proprietà di base di Project e aggiungere membri del gruppo.

**La Creazione guidata progetto non utilizza il servlet POST Sling. I valori vengono invece inviati a un servlet personalizzato:**com.adobe.cq.project.impl.servlet.ProjectServlet**. È consigliabile tenerne conto quando si aggiungono campi personalizzati.*

Un esempio di procedura guidata personalizzata è disponibile per il modello di progetto di traduzione: **/libs/cq/core/content/projects/Wizard/translationproject/defaultproject**.

### Gadget {#gadgets}

Non esistono proprietà aggiuntive su questo nodo, ma gli elementi secondari del nodo gadget controllano quali sezioni progetto popolano il dashboard del progetto quando viene creato un nuovo progetto. [Le sezioni](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#ProjectTiles) del progetto (dette anche gadget o contenitori) sono semplici schede che popolano il posto di lavoro di un progetto. Un elenco completo delle sezioni di barre è disponibile in: **/libs/cq/gui/components/projects/admin/pod. **I proprietari dei progetti possono sempre aggiungere o rimuovere sezioni dopo la creazione di un progetto.

### Ruoli {#roles}

Per ogni progetto sono disponibili 3 ruoli [](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#UserRolesinaProject) predefiniti: **Osservatori**, **editor** e **proprietari**. Aggiungendo nodi secondari sotto il nodo dei ruoli è possibile aggiungere ulteriori ruoli di progetto specifici per l&#39;azienda per il modello. Potete quindi collegare questi ruoli a flussi di lavoro specifici associati al progetto.

### Flussi di lavoro {#workflows}

Uno dei motivi più interessanti per creare un modello di progetto personalizzato è la possibilità di configurare i flussi di lavoro disponibili per l&#39;utilizzo con il progetto. Questi possono essere flussi di lavoro OOTB o flussi di lavoro personalizzati. Sotto il nodo **workflow** devono essere presenti il nodo **models** (anch&#39;esso `nt:unstructured`) e i nodi secondari sotto devono specificare i modelli di workflow disponibili. La proprietà **modelId **punta al modello di flusso di lavoro in /etc/workflow e la **procedura guidata** delle proprietà fa riferimento alla finestra di dialogo utilizzata per avviare il flusso di lavoro. Un grande vantaggio di Progetti è la possibilità di aggiungere una finestra di dialogo personalizzata (procedura guidata) per acquisire metadati aziendali specifici all&#39;inizio del flusso di lavoro, che può stimolare ulteriori azioni all&#39;interno del flusso di lavoro.

```shell
<projects-template-root> (cq:Template)
    + workflows (nt:unstructured)
         + models (nt:unstructured)
              + <workflow-model> (nt:unstructured)
                   - modelId = points to the workflow model
                   - wizard = dialog used to start the workflow
```

## Creating a project template {#creating-project-template}

Poiché verranno copiati/configurati principalmente i nodi, utilizzeremo i CRXDE Lite. Nell’istanza AEM locale, aprite il [CRXDE Lite](http://localhost:4502/crx/de/index.jsp).

1. Per iniziare, create una nuova cartella sotto `/apps/&lt;your-app-folder&gt;` nome `projects`. Create un’altra cartella sotto alla quale viene assegnato il nome `templates`.

   ```shell
   /apps/aem-guides/projects-tasks/
                       + projects (nt:folder)
                                + templates (nt:folder)
   ```

1. Per semplificare le cose, inizieremo il nostro modello personalizzato dal modello di progetto semplice esistente.

   1. Copiate e incollate il nodo **/libs/cq/core/content/projects/templates/default** sotto la cartella dei *modelli* creata nel passaggio 1.

   ```shell
   /apps/aem-guides/projects-tasks/
                + templates (nt:folder)
                     + default (cq:Template)
   ```

1. È ora necessario disporre di un percorso come **/app/aem-guide/progetti-task/progetti/modelli/authoring-project**.

   1. Modificate le proprietà **jcr:title** e **jcr:description** del nodo author-project in valori di titolo e descrizione personalizzati.

      1. Lasciare la proprietà **Wizard** che indica le proprietà predefinite di Project.

   ```shell
   /apps/aem-guides/projects-tasks/projects/
            + templates (nt:folder)
                 + authoring-project (cq:Template)
                      - jcr:title = "Authoring Project"
                      - jcr:description = "A project to manage approval and publish process for AEM Sites or Assets"
                      - wizard = "/libs/cq/core/content/projects/wizard/steps/defaultproject.html"
   ```

1. Per questo modello di progetto vogliamo utilizzare Attività.
   1. Aggiungi un nuovo nodo **nt:unstructure** sotto authoring-project/gadget denominato **task**.
   1. Aggiungete proprietà stringa al nodo attività per **cardWeight** = &quot;100&quot;, **jcr:title**=&quot;Tasks&quot; e **sling:resourceType**=&quot;cq/gui/components/projects/admin/pod/task pod&quot;.

   Ora la sezione [](https://docs.adobe.com/docs/en/aem/6-3/author/projects.html#Tasks) Attività viene visualizzata per impostazione predefinita quando viene creato un nuovo progetto.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
            + team (nt:unstructured)
            + asset (nt:unstructured)
            + work (nt:unstructured)
            + experiences (nt:unstructured)
            + projectinfo (nt:unstructured)
            ..
            + tasks (nt:unstructured)
                 - cardWeight = "100"
                 - jcr:title = "Tasks"
                 - sling:resourceType = "cq/gui/components/projects/admin/pod/taskpod"
   ```

1. Aggiungeremo un ruolo di approver personalizzato al modello di progetto.

   1. Sotto il nodo modello di progetto (authoring-project) aggiungete un nuovo nodo **nt:unstructure** con **ruoli** etichettati.
   1. Aggiungere un altro nodo **non strutturato** con etichetta di approvatori come figlio del nodo dei ruoli.
   1. Aggiungete le proprietà stringa **jcr:title** = &quot;**Approvers**&quot;, **roleclass** =&quot;**owner**&quot;, **roleid******=&quot;Approvers&quot;.
      1. Il nome del nodo degli approvatori, nonché jcr:title e roleid possono essere qualsiasi valore di stringa (purché roleid sia univoco).
      1. **roleclass** regola le autorizzazioni applicate per quel ruolo in base ai [3 ruoli]OOOTB (https://docs.adobe.com/docs/en/aem/6-3/author/projects.html#User Ruoli in un progetto): **proprietario**, **editor** e **osservatore**.
      1. In generale, se il ruolo personalizzato è più di un ruolo manageriale, il ruolo può essere **proprietario;** se si tratta di un ruolo di authoring più specifico, come Fotografo o o Designer, dovrebbero essere sufficienti le **fasi dell&#39;editor** . La grande differenza tra **proprietario** ed **editor** è che i proprietari dei progetti possono aggiornare le proprietà del progetto e aggiungere nuovi utenti al progetto.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
           + approvers (nt:unstructured)
                - jcr:title = "Approvers"
                - roleclass = "owner"
                - roleid = "approver"
   ```

1. Copiando il modello di progetto semplice si ottengono 4 flussi di lavoro OOTB configurati. Ogni nodo sotto flussi di lavoro/modelli punta a un flusso di lavoro specifico e a una finestra di dialogo di avvio guidata per tale flusso di lavoro. Più avanti in questa esercitazione verrà creato un flusso di lavoro personalizzato per il progetto. Per ora eliminare i nodi sotto il flusso di lavoro/modelli:

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
            + models (nt:unstructured)
               - (remove ootb models)
   ```

1. Per facilitare l’identificazione del modello di progetto da parte degli autori dei contenuti, potete aggiungere una miniatura personalizzata. La dimensione consigliata è 319x319 pixel.
   1. In CRXDE Lite create un nuovo file come elemento di pari livello di gadget, ruoli e nodi di flussi di lavoro denominati **thumbnail.png**.
   1. Salva, quindi accedi al `jcr:content` nodo e fai doppio clic sulla `jcr:data` proprietà (evita di fare clic su &#39;view&#39;).
      1. Viene visualizzata una finestra di dialogo con un `jcr:data` file di modifica e potete caricare una miniatura personalizzata.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
       + thumbnail.png (nt:file)
   ```

Rappresentazione XML del modello di progetto completata:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
    jcr:description="A project to manage approval and publish process for AEM Sites or Assets"
    jcr:primaryType="cq:Template"
    jcr:title="Authoring Project"
    ranking="{Long}1"
    wizard="/libs/cq/core/content/projects/wizard/steps/defaultproject.html">
    <jcr:content
        jcr:primaryType="nt:unstructured"
        detailsHref="/projects/details.html"/>
    <gadgets jcr:primaryType="nt:unstructured">
        <team
            jcr:primaryType="nt:unstructured"
            jcr:title="Team"
            sling:resourceType="cq/gui/components/projects/admin/pod/teampod"
            cardWeight="60"/>
        <tasks
            jcr:primaryType="nt:unstructured"
            jcr:title="Tasks"
            sling:resourceType="cq/gui/components/projects/admin/pod/taskpod"
            cardWeight="100"/>
        <work
            jcr:primaryType="nt:unstructured"
            jcr:title="Workflows"
            sling:resourceType="cq/gui/components/projects/admin/pod/workpod"
            cardWeight="80"/>
        <experiences
            jcr:primaryType="nt:unstructured"
            jcr:title="Experiences"
            sling:resourceType="cq/gui/components/projects/admin/pod/channelpod"
            cardWeight="90"/>
        <projectinfo
            jcr:primaryType="nt:unstructured"
            jcr:title="Project Info"
            sling:resourceType="cq/gui/components/projects/admin/pod/projectinfopod"
            cardWeight="100"/>
    </gadgets>
    <roles jcr:primaryType="nt:unstructured">
        <approvers
            jcr:primaryType="nt:unstructured"
            jcr:title="Approvers"
            roleclass="owner"
            roleid="approvers"/>
    </roles>
    <workflows
        jcr:primaryType="nt:unstructured"
        tags="[]">
        <models jcr:primaryType="nt:unstructured">
        </models>
    </workflows>
</jcr:root>
```

## Verifica del modello di progetto personalizzato

Ora possiamo testare il nostro modello di progetto creando un nuovo progetto.

1. Dovresti vedere il modello personalizzato come una delle opzioni per la creazione del progetto.

   ![Scegli modello](./assets/develop-aem-projects/choose-template.png)

1. Dopo aver selezionato il modello personalizzato, fai clic su &quot;Successivo&quot; e osserva che durante la compilazione dei membri del progetto puoi aggiungerli come ruolo di approver.

   ![Approva](./assets/develop-aem-projects/user-approver.png)

1. Fate clic su &#39;Crea&#39; per completare la creazione del progetto basato sul modello personalizzato. Nella Dashboard progetto si noterà che la sezione Attività e le altre sezioni configurate sotto i gadget vengono visualizzate automaticamente.

   ![Riquadro attività](./assets/develop-aem-projects/tasks-tile.png)


## Perché Workflow?

Tradizionalmente, AEM flussi di lavoro centrati su un processo di approvazione hanno utilizzato i passaggi del flusso di lavoro Partecipanti. AEM Casella in entrata include informazioni dettagliate su Attività e Flusso di lavoro e integrazione con AEM progetti. Queste funzioni rendono più attraente l&#39;utilizzo dei passaggi del processo di creazione dei progetti.

### Perché eseguire le attività?

L&#39;utilizzo di un passaggio di creazione di attività sui passaggi tradizionali dei partecipanti offre un paio di vantaggi:

* **Data** di inizio e scadenza: consente agli autori di gestire facilmente il proprio tempo, la nuova funzione Calendario utilizza queste date.
* **Priorità** - costruite in priorità di Basso, Normale e Alta permette agli autori di dare priorità al lavoro
* **Commenti** filettati - come gli autori lavorano su un&#39;attività hanno la possibilità di lasciare commenti in maggiore collaborazione
* **Visibilità** : le sezioni delle attività e le viste con Progetti consentono ai manager di visualizzare il tempo trascorso
* **Integrazione** dei progetti: le attività sono già integrate con i ruoli e le dashboard di Project

Come per i passaggi Partecipante, le attività possono essere assegnate e indirizzate in modo dinamico. I metadati dell&#39;attività come Titolo, Priorità possono essere impostati in modo dinamico in base alle azioni precedenti, come vedremo con l&#39;esercitazione seguente.

Anche se le attività presentano alcuni vantaggi rispetto ai passaggi partecipanti, comportano un sovraccarico aggiuntivo e non sono utili al di fuori di un progetto. Inoltre, tutti i comportamenti dinamici delle attività devono essere codificati utilizzando script ecma con proprie limitazioni.

## Requisiti del caso d’uso di esempio {#goals-tutorial}

![Diagramma del processo del flusso di lavoro](./assets/develop-aem-projects/workflow-process-diagram.png)

Il diagramma riportato sopra illustra i requisiti di alto livello per il flusso di lavoro di approvazione del campione.

Il primo passaggio consiste nel creare un’attività per terminare la modifica di un contenuto. L&#39;iniziatore del flusso di lavoro potrà scegliere l&#39;assegnatario della prima attività.

Una volta completata la prima attività, l&#39;assegnatario avrà tre opzioni per instradare il flusso di lavoro:

**Normale **- Il ciclo normale crea un&#39;attività assegnata al gruppo di approvatori del progetto per la revisione e l&#39;approvazione. La priorità dell&#39;attività è Normale e la data di scadenza è di 5 giorni dalla creazione.

**Rush** : il ciclo rapido crea anche un&#39;attività assegnata al gruppo Approver del progetto. La priorità dell&#39;attività è Alta e la data di scadenza è solo 1 giorno.

**Bypass** - In questo flusso di lavoro di esempio, il partecipante iniziale ha la possibilità di bypassare il gruppo di approvazione. (sì, questo potrebbe vanificare lo scopo di un flusso di lavoro di approvazione, ma ci permette di illustrare ulteriori funzionalità di routing)

Il gruppo di approver può approvare il contenuto o inviarlo nuovamente all&#39;assegnatario iniziale per la rielaborazione. Nel caso in cui venga inviato per la rielaborazione, viene creata una nuova attività con l&#39;etichetta &quot;Inviato indietro per la rielaborazione&quot;.

L’ultimo passaggio del flusso di lavoro utilizza la fase del processo ootb Activate Page/Asset e replica il payload.

## Creare il modello di workflow

1. Dal menu di avvio AEM passare a Strumenti > Flusso di lavoro > Modelli. Fai clic su &quot;Crea&quot; nell&#39;angolo in alto a destra per creare un nuovo modello di workflow.

   Assegna al nuovo modello un titolo: &quot;Flusso di lavoro di approvazione del contenuto&quot; e un nome URL: &quot;content-Approval-workflow&quot;.

   ![Finestra di dialogo Crea flusso di lavoro](./assets/develop-aem-projects/workflow-create-dialog.png)

   Per ulteriori informazioni sulla [creazione di flussi di lavoro, consulta](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-models.html).

1. Come procedura ottimale, i flussi di lavoro personalizzati devono essere raggruppati nella propria cartella al di sotto di /etc/workflow/models. In CRXDE Lite create un nuovo **&#39;nt:folder&#39;** sotto /etc/workflow/models denominato **&quot;aem-guide&quot;**. L&#39;aggiunta di una sottocartella garantisce che i flussi di lavoro personalizzati non vengano sovrascritti accidentalmente durante gli aggiornamenti o le installazioni di Service Pack.

   *È importante non posizionare mai la cartella o i flussi di lavoro personalizzati sotto le sottocartelle di ootb come /etc/workflow/models/dam o /etc/workflow/models/projects, in quanto l&#39;intera sottocartella può anche essere sovrascritta da aggiornamenti o Service Pack.

   ![Posizione del modello di workflow in 6.3](./assets/develop-aem-projects/custom-workflow-subfolder.png)

   Posizione del modello di workflow in 6.3

   >[!NOTE]
   >
   >Se si utilizza AEM 6.4+, la posizione di Workflow è cambiata. Per ulteriori informazioni, [consulta.](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-best-practices.html#LocationsWorkflowModels)

   Se si utilizza AEM 6.4+, il modello di workflow verrà creato in `/conf/global/settings/workflow/models`. Ripetete i passaggi indicati sopra con la directory /conf e aggiungete una sottocartella denominata `aem-guides` e spostate il `content-approval-workflow` sotto.

   ![Posizione](./assets/develop-aem-projects/modern-workflow-definition-location.png)del flusso di lavoro moderno Posizione del modello di workflow in 6.4+

1. Introdotto nella AEM 6.3 è la possibilità di aggiungere le fasi del flusso di lavoro a un determinato flusso di lavoro. Le fasi vengono visualizzate all’utente dalla casella in entrata nella scheda Informazioni sul flusso di lavoro. L&#39;utente visualizza la fase corrente del flusso di lavoro, nonché le fasi precedenti e successive.

   Per configurare le fasi, aprire la finestra di dialogo Proprietà pagina dal pulsante SideKick. La quarta scheda è etichettata &quot;Stages&quot;. Aggiungete i seguenti valori per configurare le tre fasi di questo flusso di lavoro:

   1. Modifica contenuto
   1. Approvazione
   1. Pubblicazione

   ![configurazione delle fasi del flusso di lavoro](./assets/develop-aem-projects/workflow-model-stage-properties.png)

   Configurare le fasi del flusso di lavoro dalla finestra di dialogo Proprietà pagina.

   ![barra di avanzamento del flusso di lavoro](./assets/develop-aem-projects/workflow-info-progress.png)

   Barra di avanzamento del flusso di lavoro visualizzata dalla AEM Inbox.

   Facoltativamente, potete caricare un’ **immagine** nelle proprietà pagina che verranno utilizzate come miniatura del flusso di lavoro quando gli utenti la selezionano. Le dimensioni dell’immagine devono essere 319x319 pixel. Se si aggiunge una **descrizione** a Proprietà pagina, viene visualizzato anche quando un utente accede al flusso di lavoro.

1. Il processo del flusso di lavoro Crea attività progetto è progettato per creare un&#39;attività come passaggio nel flusso di lavoro. Solo dopo aver completato l&#39;attività il flusso di lavoro procederà. Un aspetto importante del passaggio Crea attività progetto è che può leggere i metadati del flusso di lavoro e usarli per creare l&#39;attività in modo dinamico.

   Innanzitutto, eliminate il Passaggio partecipante che viene creato per impostazione predefinita. Dalla barra laterale nel menu dei componenti, espandete la sottotitola **&quot;Progetti&quot;** e trascinate il cursore **&quot;Crea attività progetto&quot;** sul modello.

   Fate doppio clic sul passaggio &quot;Crea attività progetto&quot; per aprire la finestra di dialogo del flusso di lavoro. Configurare le seguenti proprietà:

   Questa scheda è comune per tutti i passaggi del processo del flusso di lavoro e verranno impostati il Titolo e la Descrizione (che non saranno visibili all’utente finale). La proprietà importante che verrà impostata è l&#39;area di flusso di lavoro su **&quot;Modifica contenuto&quot;** dal menu a discesa.

   ```shell
   Common Tab
   -----------------
       Title = "Start Task Creation"
       Description = "This the first task in the Workflow"
       Workflow Stage = "Edit Content"
   ```

   Il processo del flusso di lavoro Crea attività progetto è progettato per creare un&#39;attività come passaggio nel flusso di lavoro. La scheda Attività consente di impostare tutti i valori dell&#39;attività. Nel nostro caso vogliamo che l&#39;Assegnatario sia dinamico, in modo da lasciarlo vuoto. Gli altri valori delle proprietà:

   ```shell
   Task Tab
   -----------------
       Name* = "Edit Content"
       Task Priority = "Medium"
       Description = "Edit the content and finalize for approval. Once finished submit for approval."
       Due In - Days = "2"
   ```

   La scheda di routing è una finestra di dialogo opzionale che consente di specificare le azioni disponibili per l&#39;utente che completa l&#39;attività. Queste azioni sono solo valori stringa e verranno salvate nei metadati del flusso di lavoro. Questi valori possono essere letti dagli script e/o elaborati più avanti nel flusso di lavoro per &quot;indirizzare&quot; in modo dinamico il flusso di lavoro. In base agli obiettivi [del](#goals-tutorial) flusso di lavoro verranno aggiunte tre azioni a questa scheda:

   ```shell
   Routing Tab
   -----------------
       Actions =
           "Normal Approval"
           "Rush Approval"
           "Bypass Approval"
   ```

   Questa scheda consente di configurare uno script di attività pre-creazione in cui è possibile decidere a livello di programmazione vari valori dell&#39;attività prima che venga creata. È possibile indirizzare lo script a un file esterno o incorporare uno script breve direttamente nella finestra di dialogo. In questo caso, lo script pre-creazione attività verrà indirizzato a un file esterno. Nel passaggio 5 verrà creato lo script.

   ```shell
   Advanced Settings Tab
   -----------------
      Pre-Create Task Script = "/apps/aem-guides/projects/scripts/start-task-config.ecma"
   ```

1. Nel passaggio precedente abbiamo fatto riferimento a uno script di attività pre-creazione. Verrà creato lo script in cui verrà impostato l&#39;assegnatario dell&#39;attività in base al valore dei metadati del flusso di lavoro &quot;**assegnatario**&quot;. Il valore **&quot;assegnatario&quot;** verrà impostato all&#39;avvio del flusso di lavoro. Leggeremo anche i metadati del flusso di lavoro per scegliere dinamicamente la priorità dell&#39;attività leggendo il valore &quot;**taskPriority&quot;** dei metadati del flusso di lavoro, così come il valore **&quot;taskDueDate&quot; **per impostare dinamicamente quando è scaduta la prima attività.

   A scopo organizzativo abbiamo creato una cartella sotto la nostra cartella dell&#39;app in cui si trovano tutti gli script relativi al progetto: **/apps/aem-guide/projects-task/projects/scripts**. Create un nuovo file sotto questa cartella denominato **&quot;start-task-config.ecma&quot;**. *Accertarsi che il percorso del file start-task-config.ecma corrisponda al percorso impostato nella scheda Impostazioni avanzate al punto 4.

   Aggiungete quanto segue come contenuto del file:

   ```
   // start-task-config.ecma
   // Populate the task using values stored as workflow metadata originally posted by the start workflow wizard
   
   // set the assignee based on start workflow wizard
   var assignee = workflowData.getMetaDataMap().get("assignee", Packages.java.lang.String);
   task.setCurrentAssignee(assignee);
   
   //Set the due date for the initial task based on start workflow wizard
   var dueDate = workflowData.getMetaDataMap().get("taskDueDate", Packages.java.util.Date);
   if (dueDate != null) {
       task.setProperty("taskDueDate", dueDate);
   }
   
   //Set the priority based on start workflow wizard
   var taskPriority = workflowData.getMetaDataMap().get("taskPriority", "Medium");
   task.setProperty("taskPriority", taskPriority);
   ```

1. Tornate al flusso di lavoro di approvazione dei contenuti. Trascinare il componente **OR Split** (nella barra laterale sotto la categoria &quot;Workflow&quot;) sotto il passaggio **Avvia attività** . Nella finestra di dialogo Comune selezionare il pulsante di scelta per 3 rami. La divisione OR leggerà il valore dei metadati del flusso di lavoro **&quot;lastTaskAction&quot;** per determinare la route del flusso di lavoro. La proprietà **&quot;lastTaskAction&quot;** verrà impostata su uno dei valori della scheda Routing configurata al punto 4. Per ciascuna scheda Filiale, compilare l&#39;area di testo **Script** con i seguenti valori:

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Normal Approval") {
       return true;
   }
   
   return false;
   }
   ```

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Rush Approval") {
       return true;
   }
   
   return false;
   }
   ```

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Bypass Approval") {
       return true;
   }
   
   return false;
   }
   ```

   *Si noti che stiamo effettuando una corrispondenza diretta tra le stringhe per determinare il percorso, quindi è importante che i valori impostati negli script di ramo corrispondano ai valori del ciclo di lavorazione impostati al punto 4.

1. Trascinare e rilasciare un altro passaggio &quot;**Crea attività** progetto&quot; sul modello all&#39;estrema sinistra (ramo 1) sotto la divisione OR. Compilate la finestra di dialogo con le seguenti proprietà:

   ```
   Common Tab
   -----------------
       Title = "Approval Task Creation"
       Description = "Create a an approval task for Project Approvers. Priority is Medium."
       Workflow Stage = "Approval"
   
   Task Tab
   ------------
       Name* = "Approve Content for Publish"
       Task Priority = "Medium"
       Description = "Approve this content for publication."
       Days = "5"
   
   Routing Tab - Actions
   ----------------------------
       "Approve and Publish"
       "Send Back for Revision"
   ```

   Poiché si tratta del percorso di approvazione normale, la priorità dell&#39;attività è impostata su Media. Inoltre, assegniamo al gruppo di approvatori 5 giorni per completare il task. L&#39;assegnatario viene lasciato vuoto nella scheda Attività in quanto verrà assegnato in modo dinamico nella scheda Impostazioni avanzate. Per completare questo compito, diamo al gruppo Approvatori due possibili percorsi: **&quot;Approva e pubblica&quot;** se approvano il contenuto e possono essere pubblicati e **&quot;Porta indietro per revisione&quot;** in caso di problemi da correggere nell’editor originale. L&#39;approvatore può lasciare dei commenti che l&#39;editor originale vedrà se il flusso di lavoro gli verrà restituito.

In questa esercitazione abbiamo creato un modello di progetto che includeva un ruolo di approvatori. Ogni volta che da questo modello viene creato un nuovo progetto, verrà creato un gruppo specifico per il progetto per il ruolo Approvatori. Come un Passaggio partecipante, un&#39;attività può essere assegnata solo a un utente o a un gruppo. Vogliamo assegnare questa attività al gruppo di progetti che corrisponde al gruppo di approvatori. Tutti i flussi di lavoro avviati da un progetto avranno metadati che mappano i ruoli del progetto al gruppo specifico del progetto.

Copiare e incollare il seguente codice nell&#39;area di testo **Script** della scheda **Advanced Settings **o. Questo codice leggerà i metadati del flusso di lavoro e assegnerà l&#39;attività al gruppo Approvatori del progetto. Se non riesce a trovare il valore del gruppo di approvatori, tornerà all&#39;assegnazione dell&#39;attività al gruppo Amministratori.

```
var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");

task.setCurrentAssignee(projectApproverGrp);
```

1. Trascinare e rilasciare un altro passaggio &quot;**Crea attività** progetto&quot; sul modello fino al ramo centrale (ramo 2) sotto la divisione OR. Compilate la finestra di dialogo con le seguenti proprietà:

   ```
   Common Tab
   -----------------
       Title = "Rush Approval Task Creation"
       Description = "Create a an approval task for Project Approvers. Priority is High."
       Workflow Stage = "Approval"
   
   Task Tab
   ------------
       Name* = "Rush Approve Content for Publish"
       Task Priority = "High"
       Description = "Rush approve this content for publication."
       Days = "1"
   
   Routing Tab - Actions
   ----------------------------
       "Approve and Publish"
       "Send Back for Revision"
   ```

   Poiché si tratta del percorso di approvazione Rush, la priorità dell&#39;attività è impostata su Alta. Inoltre, assegniamo al gruppo di approvatori solo un giorno per completare l&#39;attività. L&#39;assegnatario viene lasciato vuoto nella scheda Attività in quanto verrà assegnato in modo dinamico nella scheda Impostazioni avanzate.

   È possibile riutilizzare lo stesso frammento di script del passaggio 7 per compilare l&#39;area di testo **Script** nella scheda** Advanced Settings **. Copia+Incolla nel codice seguente:

   ```
   var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");
   
   task.setCurrentAssignee(projectApproverGrp);
   ```

1. Trascinare un componente** Nessuna operazione** sul ramo a destra (ramo 3). Il componente Nessuna operazione non esegue alcuna azione e procede immediatamente, rappresentando il desiderio dell&#39;editor originale di bypassare il passaggio di approvazione. Tecnicamente potremmo uscire da questo ramo senza alcuna fase del flusso di lavoro, ma come procedura ottimale aggiungeremo un passaggio No Operation. Questo chiarisce agli altri sviluppatori qual è lo scopo della Ramo 3.

   Fate doppio clic sul passaggio del flusso di lavoro e configurate Titolo e Descrizione:

   ```
   Common Tab
   -----------------
       Title = "Bypass Approval"
       Description = "Placeholder step to indicate that the original editor decided to bypass the approver group."
   ```

   ![modello di workflow O divisione](./assets/develop-aem-projects/workflow-stage-after-orsplit.png)

   L&#39;aspetto del modello di flusso di lavoro dovrebbe essere simile a questo dopo che tutti e tre i rami nella divisione OR sono stati configurati.

1. Poiché il gruppo Approvatori ha la possibilità di inviare il flusso di lavoro all&#39;editor originale per ulteriori revisioni, ci affidiamo al passaggio **Vai** per leggere l&#39;ultima azione eseguita e indirizzare il flusso di lavoro all&#39;inizio o lasciarlo continuare.

   Trascinate e rilasciate il componente Passaggio a (nella barra laterale sotto Workflow) sotto la divisione OR, in cui si collega di nuovo. Fate doppio clic e configurate le seguenti proprietà nella finestra di dialogo:

   ```
   Common Tab
   ----------------
       Title = "Goto Step"
       Description = "Based on the Approver groups action route the workflow to the beginning or continue and publish the payload."
   
   Process Tab
   ---------------
       The step to go to. = "Start Task Creation"
   ```

   L&#39;ultimo elemento che configureremo è lo Script come parte del passaggio del processo Goto. Il valore dello script può essere incorporato tramite la finestra di dialogo o configurato per puntare a un file esterno. Lo script Goto deve contenere una **funzione check()** e restituire true se il flusso di lavoro deve passare al passaggio specificato. Se si restituisce false, il flusso di lavoro continua.

   Se il gruppo di approver sceglie l&#39;azione **&quot;Porta indietro per revisione&quot;** (configurata nei passaggi 7 e 8), sarà necessario riportare il flusso di lavoro al passaggio **&quot;Avvia creazione attività&quot;** .

   Nella scheda Processo aggiungere lo snippet di codice seguente all&#39;area di testo Script:

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Send Back for Revision") {
       return true;
   }
   
   return false;
   }
   ```

1. Per pubblicare il payload utilizzeremo il passaggio ootb **Attiva pagina/Processo risorsa** . Questo passaggio del processo richiede poca configurazione e aggiunge il payload del flusso di lavoro alla coda di replica per l&#39;attivazione. Aggiungeremo il passaggio sotto il passaggio Goto e in questo modo sarà possibile raggiungerlo solo se il gruppo di approvatori ha approvato il contenuto per la pubblicazione o se l&#39;editor originale ha scelto la procedura Bypass Approval.

   Trascinate e rilasciate il passaggio **Attiva processo pagina/risorsa** (disponibile nella barra laterale in Flusso di lavoro WCM) sotto il passaggio Vai nel modello.

   ![modello di workflow completato](assets/develop-aem-projects/workflow-model-final.png)

   Aspetto del modello di workflow dopo l’aggiunta del passaggio Vai e Attiva pagina/risorsa.

1. Se il gruppo Approver invia nuovamente il contenuto per la revisione, desideriamo informarlo sull&#39;editor originale. A tal fine, è possibile modificare dinamicamente le proprietà di creazione delle attività. Verrà visualizzato il valore della proprietà lastActionTaken di **&quot;Send Back for Revision&quot;**. Se questo valore è presente, modificheremo il titolo e la descrizione per indicare che l&#39;attività è il risultato del contenuto che viene inviato di nuovo per la revisione. Inoltre aggiorneremo la priorità su **&quot;Alta&quot;** in modo che sia il primo elemento su cui l&#39;editor lavora. Infine, la data di scadenza dell&#39;attività verrà impostata su un giorno a partire dal momento in cui il flusso di lavoro viene inviato di nuovo per la revisione.

   Sostituisce lo `start-task-config.ecma` script iniziale (creato al punto 5) con quanto segue:

   ```
   // start-task-config.ecma
   // Populate the task using values stored as workflow metadata originally posted by the start workflow wizard
   
   // set the assignee based on start workflow wizard
   var assignee = workflowData.getMetaDataMap().get("assignee", Packages.java.lang.String);
   task.setCurrentAssignee(assignee);
   
   //Set the due date for the initial task based on start workflow wizard
   var dueDate = workflowData.getMetaDataMap().get("taskDueDate", Packages.java.util.Date);
   if (dueDate != null) {
       task.setProperty("taskDueDate", dueDate);
   }
   
   //Set the priority based on start workflow wizard
   var taskPriority = workflowData.getMetaDataMap().get("taskPriority", "Medium");
   task.setProperty("taskPriority", taskPriority);
   
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   //change the title and priority if the approver group sent back the content
   if(lastAction == "Send Back for Revision") {
     var taskName = "Review and Revise Content";
   
     //since the content was rejected we will set the priority to High for the revison task
     task.setProperty("taskPriority", "High"); 
   
     //set the Task name (displayed as the task title in the Inbox) 
     task.setProperty("name", taskName);
     task.setProperty("nameHierarchy", taskName);
   
     //set the due date of this task 1 day from current date
     var calDueDate = Packages.java.util.Calendar.getInstance();
     calDueDate.add(Packages.java.util.Calendar.DATE, 1);
     task.setProperty("taskDueDate", calDueDate.getTime());
   
   }
   ```

## Creazione della procedura guidata di avvio del flusso di lavoro {#start-workflow-wizard}

Quando si sceglie di avviare un flusso di lavoro da un progetto, è necessario specificare una procedura guidata per avviare il flusso di lavoro. Procedura guidata predefinita: `/libs/cq/core/content/projects/workflowwizards/default_workflow` consente all&#39;utente di inserire un titolo del flusso di lavoro, un commento iniziale e un percorso di payload per l&#39;esecuzione del flusso di lavoro. Sono inoltre disponibili diversi altri esempi: `/libs/cq/core/content/projects/workflowwizards`.

La creazione di una procedura guidata personalizzata può essere molto efficace in quanto è possibile raccogliere informazioni importanti prima dell&#39;avvio del flusso di lavoro. I dati vengono memorizzati come parte dei metadati del flusso di lavoro e i processi di flusso di lavoro possono leggerli e modificare in modo dinamico il comportamento in base ai valori immessi. Verrà creata una procedura guidata personalizzata per assegnare dinamicamente la prima attività del flusso di lavoro in base al valore di avvio della procedura guidata.

1. In CRXDE-Lite verrà creata una sottocartella sotto la `/apps/aem-guides/projects-tasks/projects` cartella denominata &quot;procedure guidate&quot;. Copiate la procedura guidata predefinita da: `/libs/cq/core/content/projects/workflowwizards/default_workflow` sotto la cartella delle procedure guidate appena creata, rinominatela **content-Approval-start**. Il percorso completo dovrebbe ora essere: `/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start`.

   La procedura guidata predefinita è una procedura guidata a due colonne con la prima colonna che mostra Titolo, Descrizione e Miniatura del modello di workflow selezionato. La seconda colonna include i campi per Titolo flusso di lavoro, Commento iniziale e Percorso payload. La procedura guidata è un modulo standard per l’interfaccia utente touch e utilizza i componenti [modulo](https://docs.adobe.com/docs/en/aem/6-5/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/index.html) Granite per l’interfaccia utente standard per compilare i campi.

   ![procedura guidata di approvazione del contenuto](./assets/develop-aem-projects/content-approval-start-wizard.png)

1. Verrà aggiunto un campo aggiuntivo alla procedura guidata che verrà utilizzato per impostare l&#39;assegnatario della prima attività nel flusso di lavoro (vedere [Creazione del modello](#create-workflow-model)di flusso di lavoro: Passo 5).

   Sotto `../content-approval-start/jcr:content/items/column2/items` create un nuovo nodo di tipo `nt:unstructured` &quot;assign&quot; ****. Verrà utilizzato il componente Selezione utente progetti (basato sul componente [Selezione utente](https://docs.adobe.com/docs/en/aem/6-5/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/userpicker/index.html)Granite). Questo campo consente di limitare facilmente la selezione di utenti e gruppi solo a quelli appartenenti al progetto corrente.

   Di seguito è riportata la rappresentazione XML del nodo di **assegnazione** :

   ```xml
   <assign
       granite:class="js-cq-project-user-picker"
       jcr:primaryType="nt:unstructured"
       sling:resourceType="cq/gui/components/projects/admin/userpicker"
       fieldLabel="Assign To"
       hideServiceUsers="{Boolean}true"
       impersonatesOnly="{Boolean}true"
       showOnlyProjectMembers="{Boolean}true"
       name="assignee"
       projectPath="${param.project}"
       required="{Boolean}true"/>
   ```

1. Verrà inoltre aggiunto un campo di selezione delle priorità che determinerà la priorità della prima attività nel flusso di lavoro (vedere [Creazione del modello](#create-workflow-model)di flusso di lavoro: Passo 5).

   Sotto `/content-approval-start/jcr:content/items/column2/items` create un nuovo nodo di tipo `nt:unstructured` denominato **priority**. Per compilare il campo modulo verrà utilizzato il componente [Selezione interfaccia](https://docs.adobe.com/docs/en/aem/6-2/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/select/index.html) Granite.

   Sotto il nodo di **priorità** verrà aggiunto un nodo di **elementi** **nt:unstructure**. Sotto il nodo **items** aggiungere altri 3 nodi per compilare le opzioni di selezione per Alta, Media e Bassa. Ogni nodo è di tipo **nt:unstructure** e deve avere una proprietà **text** e **value** . Il testo e il valore devono avere lo stesso valore:

   1. Alta
   1. Media
   1. Bassa

   Per il nodo Medium aggiungere una proprietà booleana aggiuntiva denominata &quot;**selected&quot;** con un valore impostato su **true**. In questo modo il valore predefinito Medio nel campo di selezione sarà &quot;Medium&quot;.

   Di seguito è riportata una rappresentazione XML della struttura del nodo e delle proprietà:

   ```xml
   <priority
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/select"
       fieldLabel="Task Priority"
       name="taskPriority">
           <items jcr:primaryType="nt:unstructured">
               <high
                   jcr:primaryType="nt:unstructured"
                   text="High"
                   value="High"/>
               <medium
                   jcr:primaryType="nt:unstructured"
                   selected="{Boolean}true"
                   text="Medium"
                   value="Medium"/>
               <low
                   jcr:primaryType="nt:unstructured"
                   text="Low"
                   value="Low"/>
               </items>
   </priority>
   ```

1. L&#39;iniziatore del flusso di lavoro potrà impostare la data di scadenza dell&#39;attività iniziale. Per acquisire questo input verrà utilizzato il campo [Granite UI DatePicker](https://docs.adobe.com/docs/en/aem/6-5/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/datepicker/index.html) . Verrà inoltre aggiunto un campo nascosto con un [TypeHint](https://sling.apache.org/documentation/bundles/manipulating-content-the-slingpostservlet-servlets-post.html#typehint) per garantire che l&#39;input sia memorizzato come proprietà del tipo Date nel JCR.

   Aggiungete due nodi **nt:unstructure** con le seguenti proprietà rappresentate in XML:

   ```xml
   <duedate
       granite:rel="project-duedate"
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/datepicker"
       displayedFormat="YYYY-MM-DD HH:mm"
       fieldLabel="Due Date"
       minDate="today"
       name="taskDueDate"
       type="datetime"/>
   <duedatetypehint
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/hidden"
       name="taskDueDate@TypeHint"
       type="datetime"
       value="Calendar"/>
   ```

1. È possibile visualizzare il codice completo della finestra di dialogo di avvio della procedura guidata [qui](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/projects-tasks-guide/ui.apps/src/main/content/jcr_root/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start/.content.xml).

## Collegamento del flusso di lavoro e del modello di progetto {#connecting-workflow-project}

L&#39;ultima cosa da fare è garantire che il modello di workflow sia disponibile per essere avviato da uno dei progetti. A tal fine, è necessario visitare nuovamente il modello di progetto che abbiamo creato nella parte 1 di questa serie.

La configurazione Workflow è un&#39;area di un modello di progetto che specifica i flussi di lavoro disponibili da utilizzare con tale progetto. La configurazione è inoltre responsabile della specifica della procedura guidata Avvia flusso di lavoro quando si avvia il flusso di lavoro (creato nei passaggi [precedenti)](#start-workflow-wizard). La configurazione del flusso di lavoro di un modello di progetto è &quot;live&quot;, ovvero l&#39;aggiornamento della configurazione del flusso di lavoro avrà effetto sia sui nuovi progetti creati che su quelli esistenti che utilizzano il modello.

1. In CRXDE-Lite passa al modello di progetto di authoring creato in precedenza in `/apps/aem-guides/projects-tasks/projects/templates/authoring-project/workflows/models`.

   Sotto il nodo dei modelli aggiungete un nuovo nodo denominato **content Approval** con un tipo di nodo **nt:unstructure**. Aggiungi le seguenti proprietà al nodo:

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/etc/workflow/models/aem-guides/content-approval-workflow/jcr:content/model"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

   >[!NOTE]
   >
   >Se si utilizza AEM 6.4, il percorso del flusso di lavoro è cambiato. Puntare la `modelId` proprietà sulla posizione del modello di flusso di lavoro di runtime in `/var/workflow/models/aem-guides/content-approval-workflow`
   >
   >
   >Per ulteriori informazioni sulla modifica della posizione del flusso di lavoro, consultate [qui.](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-best-practices.html#LocationsWorkflowModels)

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/var/workflow/models/aem-guides/content-approval-workflow"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

1. Una volta aggiunto il flusso di lavoro di approvazione del contenuto al modello di progetto, questo dovrebbe essere disponibile per l&#39;avvio dalla sezione Workflow del progetto. Avviate e giocate con i vari cicli che abbiamo creato.

## Materiali di supporto

* [Download del pacchetto di esercitazione completato](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [Repository del codice completo su GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)
* [Documentazione AEM progetti](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html)
