---
title: Risolvere i problemi relativi alla pipeline CI/CD con l’agente di sviluppo AEM
description: Scopri come risolvere i problemi relativi a una pipeline CI/CD non riuscita utilizzando l’agente di sviluppo AEM.
version: Experience Manager as a Cloud Service
role: Developer, Admin
level: Beginner
doc-type: tutorial
duration: null
jira: KT-20279
thumbnail: KT-20279.png
last-substantial-update: null
source-git-commit: 6fa0f88c231f7b68392a77a60491d4f741140a5a
workflow-type: tm+mt
source-wordcount: '1227'
ht-degree: 1%

---


# Risolvere i problemi relativi alla pipeline CI/CD con l’agente di sviluppo AEM

Scopri come risolvere i problemi relativi a una pipeline CI/CD non riuscita utilizzando l’agente di sviluppo AEM.

AEM Development Agent consente ai team tecnici, inclusi sviluppatori, ingegneri DevOps e amministratori, di **accelerare i propri flussi di lavoro** fornendo _indicazioni e azioni basate sull&#39;intelligenza artificiale_.

>[!TIP]
>
> Vedi anche [Panoramica degli agenti in AEM](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview) per un elenco completo degli agenti disponibili in AEM as a Cloud Service, delle loro funzionalità e di come puoi accedervi.


## Panoramica

AEM Development Agent offre diverse funzionalità, tra cui la possibilità di elencare, risolvere e correggere i problemi relativi alle pipeline CI/CD non riuscite. Puoi richiamare l’agente di sviluppo AEM tramite l’Assistente AI per risolvere casi d’uso specifici.

Questo tutorial utilizza il [progetto WKND Sites](https://github.com/adobe/aem-guides-wknd) per dimostrare come risolvere i problemi relativi a una pipeline CI/CD non riuscita utilizzando AEM Development Agent. Gli stessi principi si applicano a qualsiasi progetto AEM.

Per semplicità, questo tutorial introduce un errore di unit test nel file `BylineImpl.java` per mostrare le funzionalità di risoluzione dei problemi della pipeline di AEM Development Agent.

## Prerequisiti

Per seguire questa esercitazione, è necessario:

- Assistente AI e agenti in AEM abilitati. Per informazioni dettagliate, consulta [Configurare IA in AEM](./setup.md) e tieni presente che i parchi gioco menzionati in tale articolo non dispongono delle funzionalità di AEM Development Agent.
- Accesso a Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/) con ruolo Sviluppatore o Responsabile del programma. Per ulteriori informazioni, vedere [definizioni dei ruoli](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-manager/content/requirements/users-and-roles#role-definitions).
- Un ambiente AEM as a Cloud Service
- Accesso agli agenti in AEM tramite il [programma Beta](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/release-notes/release-notes-current#aem-beta-programs)
- Il [progetto WKND Sites](https://github.com/adobe/aem-guides-wknd) clonato nel computer locale

### Funzionalità attuali di AEM Development Agent

Prima di iniziare l’esercitazione, esaminiamo le funzionalità attuali di AEM Development Agent:

- Elencare le pipeline CI/CD e il loro stato
- Risoluzione dei problemi e risoluzione dei problemi relativi a **pipeline full stack** non riuscite, inclusi i tipi di _qualità codice_ e _distribuzione_.
- Sono supportati i passaggi _Build_ (completamento del codice per produrre un artefatto distribuibile) e _Code Quality_ (analisi del codice statico tramite regole SonarQube) delle pipeline **full-stack**.

Le funzionalità di AEM Development Agent vengono continuamente espanse e aggiornate regolarmente. Per commenti e suggerimenti, invia un&#39;e-mail a [aem-devagent@adobe.com](mailto:aem-devagent@adobe.com).

## Configurazione

Segui questi passaggi di alto livello per completare questa esercitazione:

1. Clona il [progetto WKND Sites](https://github.com/adobe/aem-guides-wknd) e invialo all&#39;archivio Git di Cloud Manager
2. Creare e configurare una pipeline di qualità del codice
3. Eseguire la pipeline e osservare l’esecuzione non riuscita
4. Utilizzare AEM Development Agent per risolvere i problemi relativi alla pipeline non riuscita

Passiamo a ogni passaggio nel dettaglio.

### Usa progetto WKND Sites come progetto demo

Questa esercitazione utilizza il ramo `tutorial/dev-agent/unit-test-failure` del progetto WKND Sites per dimostrare come utilizzare l&#39;agente di sviluppo AEM. Gli stessi principi possono essere applicati a qualsiasi progetto AEM.

- Errore di unit test introdotto nel file `BylineImpl.java` nel modo seguente. Se utilizzi un tuo progetto AEM, puoi introdurre un errore simile di unit test.

  ```java
  ...
  @Override
  public String getName() {
      if (name != null) {
          return "Author: " + name; // This line is intentionally incorrect to introduce a unit test failure.
      }
      return name;
  }
  ...
  ```

- Clona il [progetto WKND Sites](https://github.com/adobe/aem-guides-wknd) nel computer locale, passa alla directory del progetto e passa al ramo `tutorial/dev-agent/unit-test-failure`.

  ```shell
  git clone https://github.com/adobe/aem-guides-wknd.git
  cd aem-guides-wknd
  git checkout tutorial/dev-agent/unit-test-failure
  ```

- Crea un nuovo archivio Git di Cloud Manager per il progetto WKND Sites e aggiungilo come remoto all’archivio Git locale:

   - Passa a Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/) e seleziona il tuo programma.
   - Fai clic su **Archivi** nella barra laterale a sinistra.
   - Fai clic su **Aggiungi archivio** nell&#39;angolo superiore destro.
   - Immetti un **Nome archivio** (ad esempio, &quot;wknd-site-tutorial&quot;) e fai clic su **Salva**. Attendi la creazione dell’archivio.

     ![Aggiungi archivio](./assets/dev-agent/add-repository.png)

   - Fai clic su **Accedi a dati archivio** nell&#39;angolo in alto a destra e copia l&#39;URL dell&#39;archivio.

     ![Accedi a dati archivio](./assets/dev-agent/access-repo-info.png)

   - Aggiungi il nuovo archivio Git Cloud Manager creato come remoto all’archivio Git locale:

     ```shell
     git remote add adobe https://git.cloudmanager.adobe.com/<your-adobe-organization>/wknd-site-tutorial/
     ```

- Invia l’archivio Git locale all’archivio Git di Cloud Manager:

  ```shell
  git push adobe
  ```

  Quando vengono richieste le credenziali, fornisci il **Nome utente** e la **Password** dal modale **Informazioni archivio** di Cloud Manager.

### Creare e configurare una pipeline di qualità del codice

Questa esercitazione utilizza una pipeline di qualità del codice (non di produzione) per attivare l’errore della pipeline per la risoluzione dei problemi. Per ulteriori informazioni sulle pipeline di qualità del codice, consulta [Introduzione alle pipeline CI/CD](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#introduction).

- In Cloud Manager, passa alla sezione **Pipeline** e seleziona **Aggiungi** > **Aggiungi pipeline non di produzione**.
- Nella finestra di dialogo **Aggiungi pipeline non di produzione** configura quanto segue:

   - **Configurazione** passaggio:
      - Mantieni i valori predefiniti come **Tipo pipeline** come `Code Quality Pipeline` e **Trigger distribuzione** come `Manual`.
      - Per **Nome pipeline non di produzione**, immetti `Code Quality::Fullstack`

     ![Aggiungi configurazione pipeline non di produzione](./assets/dev-agent/add-non-production-pipeline-configuration.png)

   - **Codice Source** passaggio:
      - Seleziona **Codice full stack**
      - Per l&#39;**archivio**, selezionare il nuovo archivio Git di Cloud Manager creato
      - Per **Ramo Git**, seleziona `tutorial/dev-agent/unit-test-failure`
      - Fai clic su **Salva**

     ![Aggiungi codice Source pipeline non di produzione](./assets/dev-agent/add-non-production-pipeline-source-code.png)

- Eseguire la pipeline di qualità del codice appena creata facendo clic su **Esegui** nel menu a tre punti della voce della pipeline.

  ![Esegui pipeline di qualità del codice](./assets/dev-agent/run-code-quality-pipeline.png)


>[!IMPORTANT]
>
> Questa esercitazione non tratta la pipeline di distribuzione. Tuttavia, puoi seguire gli stessi principi per risolvere i problemi relativi a una pipeline di distribuzione non riuscita.


### Osservare l’esecuzione della pipeline non riuscita

La pipeline di qualità del codice non riesce nel passaggio **Preparazione artefatto** e restituisce un errore:

![Esecuzione pipeline non riuscita](./assets/dev-agent/failed-pipeline-execution.png)

Senza l’agente di sviluppo AEM, questo errore della pipeline richiede la risoluzione manuale dei problemi. Uno sviluppatore deve controllare i registri e rivedere il codice, un processo lungo e laborioso.

Successivamente, scopri come l’intelligenza artificiale agente può risolvere i problemi e correggere l’esecuzione non riuscita della pipeline.

## Utilizzare AEM Development Agent per risolvere i problemi e correggere la pipeline non riuscita

È possibile richiamare AEM Development Agent utilizzando l’Assistente all’intelligenza artificiale in AEM descrivendo l’errore della pipeline in linguaggio naturale.

- Fai clic sull&#39;icona **Assistente AI** in alto a destra.

- Immetti i dettagli dell&#39;errore della pipeline in linguaggio naturale, ovvero **Prompt**. Ad esempio:

  ```text
  I have a failed pipeline execution on %PROGRAM-NAME% program, help me to troubleshoot and fix it.
  ```

  ![Richiama agente di sviluppo AEM](./assets/dev-agent/invoke-aem-development-agent.png)

  **L&#39;agente di sviluppo AEM** viene richiamato per la risoluzione dei problemi e la correzione dell&#39;esecuzione della pipeline non riuscita.

  >[!NOTE]
  >
  > Se il prompt immesso non è chiaro, l&#39;Assistente IA richiede chiarimenti e fornisce informazioni per aiutarti a perfezionare il prompt.

- Al termine del ragionamento, fare clic sull&#39;icona **Apri a schermo intero** per visualizzare la procedura dettagliata di risoluzione dei problemi.

  ![Apri a schermo intero](./assets/dev-agent/open-in-full-screen.png)

  I risultati contengono informazioni importanti, tra cui i dettagli dell&#39;errore, il file di origine, il numero di riga e una sezione **Come correggere** con passaggi chiari per risolvere il problema.

- In questo caso, l&#39;agente ha suggerito correttamente di modificare l&#39;implementazione (`getName()` metodo) o di aggiornare lo unit test (`getNameTest()` metodo) per risolvere il problema. Ha evitato le allucinazioni e ha utilizzato un approccio &quot;human-in-the-loop&quot; fornendo al contempo modifiche del codice utilizzabili per lo sviluppatore.

  ![Copia modifiche al codice](./assets/dev-agent/copy-code-changes.png)

- Aggiorna il file `BylineImpl.java` con le modifiche di codice suggerite, quindi esegui il commit e invia le modifiche all&#39;archivio Git di Cloud Manager.

  ```java
  ...
  @Override
  public String getName() {
      return name;
  }
  ...
  ```

- Esegui nuovamente la pipeline e osserva l’esecuzione riuscita.

## Esempi aggiuntivi

Il progetto WKND Sites include ulteriori esempi di problemi di codice e configurazione interrotti, ad esempio dipendenze mancanti e configurazione errata. Puoi esplorare questi esempi estraendo i [rami che iniziano con `tutorial/dev-agent/`](https://github.com/adobe/aem-guides-wknd/branches/all?query=tutorial%2Fdev-agent&lastTab=overview). Per visualizzare le modifiche di interruzione, è possibile confrontare il ramo `tutorial/dev-agent/unit-test-failure` con quello `main` facendo clic sul pulsante **Confronta**. Cercare quindi la sezione _file modificato_.

![Confronta rami](./assets/dev-agent/compare-branches.png)

Vedi anche [Esempi di prompt](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/development/overview#sample-prompts) per ulteriori idee su come utilizzare l&#39;agente di sviluppo AEM.

## Riepilogo

In questo tutorial, hai imparato a utilizzare AEM Development Agent per risolvere i problemi relativi a una pipeline CI/CD non riuscita utilizzando l’Assistente all’intelligenza artificiale. Hai anche imparato come l’intelligenza artificiale dinamica accelera i flussi di lavoro tecnici fornendo informazioni fruibili e modifiche al codice.

Inizia a utilizzare AEM Development Agent e altri agenti in AEM per accelerare i flussi di lavoro. Per ulteriori informazioni, consulta [Panoramica degli agenti in AEM](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview).

## Risorse aggiuntive

- [IA in Experience Manager](./overview.md)
- [Panoramica degli agenti in AEM](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview)
- [Panoramica dell&#39;agente di sviluppo](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/development/overview)
- [Panoramica degli agenti in AEM](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview)
