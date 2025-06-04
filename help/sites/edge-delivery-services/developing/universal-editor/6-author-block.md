---
title: Authoring di un blocco
description: Authoring di un blocco Edge Delivery Services con l’editor universale.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 500
exl-id: ca356d38-262d-4c30-83a0-01c8a1381ee6
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '376'
ht-degree: 100%

---

# Authoring di un blocco

Dopo aver inviato il [JSON del blocco teaser](./5-new-block.md) al ramo `teaser`, il blocco diventa modificabile nell’editor universale di AEM.

L’authoring di un blocco in fase di sviluppo è importante per diversi motivi:

1. Verifica che la definizione e il modello del blocco siano accurati.
1. Consente agli sviluppatori di rivedere l’HTML semantico del blocco, che funge da base per lo sviluppo.
1. Consente la distribuzione del contenuto e dell’HTML semantico nell’ambiente di anteprima, supportando uno sviluppo più rapido del blocco.

## Apri l’editor universale utilizzando il codice del ramo `teaser`

1. Accedi all’authoring AEM.
2. Passa a **Sites** e seleziona il sito (WKND (editor universale)) creato nel [capitolo precedente](./2-new-aem-site.md).

   ![AEM Sites](./assets/6-author-block/open-new-site.png)

3. Crea o modifica una pagina per aggiungere il nuovo blocco, assicurandoti che il contesto sia disponibile per supportare lo sviluppo locale. Anche se le pagine possono essere create ovunque all’interno del sito, spesso è meglio creare pagine distinte per ogni nuovo corpo di lavoro. Crea una nuova pagina “cartella” denominata **Rami**. Ogni pagina secondaria viene utilizzata per supportare lo sviluppo del ramo Git con lo stesso nome.

   ![AEM Sites: creare una pagina Rami](./assets/6-author-block/branches-page-3.png)

4. Nella pagina **Rami**, crea una nuova pagina con titolo **Teaser**, corrispondente al nome del ramo di sviluppo, quindi fai clic su **Apri** per modificarla.

   ![AEM Sites: creare una pagina teaser](./assets/6-author-block/teaser-page-3.png)

5. Aggiorna l’editor universale per caricare il codice dal ramo `teaser` aggiungendo `?ref=teaser` all’URL. Assicurati di aggiungere il parametro di query **BEFORE** (prima) del simbolo `#`.

   ![Editor universale: selezionare un ramo teaser](./assets/6-author-block/select-branch.png)

6. Seleziona la prima sezione in **Principale**, fai clic sul pulsante **Aggiungi** e scegli il blocco **teaser**.

   ![Editor universale: aggiungere un blocco](./assets/6-author-block/add-teaser-2.png)

7. Nell’area di lavoro, seleziona il teaser appena aggiunto e crea i campi a destra, oppure tramite la funzionalità di modifica in linea.

   ![Editor universale: authoring di un blocco](./assets/6-author-block/author-block.png)

8. Dopo aver completato l’authoring, seleziona il pulsante **Pubblica** in alto a destra dell’editor universale, scegli Pubblica in **Anteprima** e pubblica le modifiche nell’ambiente di anteprima. Le modifiche vengono quindi pubblicate nel dominio `aem.page` del sito web.
   ![AEM Sites: Pubblicazione o Anteprima](./assets/6-author-block/publish-to-preview.png)

9. Attendi che le modifiche vengano pubblicate in anteprima, quindi apri la pagina web tramite [AEM CLI](./3-local-development-environment.md#install-the-aem-cli) all’indirizzo [http://localhost:3000/branches/teaser](http://localhost:3000/branches/teaser).

   ![Sito locale: Aggiornamento](./assets/6-author-block/preview.png)

Ora, il contenuto e l’HTML semantico del blocco teaser creato sono disponibili sul sito web di anteprima, pronti per lo sviluppo utilizzando AEM CLI nell’ambiente di sviluppo locale.
