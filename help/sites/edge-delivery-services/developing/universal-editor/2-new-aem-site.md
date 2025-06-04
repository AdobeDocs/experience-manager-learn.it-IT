---
title: Creare un sito AEM
description: Crea un sito in AEM Sites per Edge Delivery Services, modificabile tramite l’editor universale.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 500
exl-id: d1ebcaf4-cea6-4820-8b05-3a0c71749d33
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '302'
ht-degree: 100%

---

# Creare un sito AEM

Il sito AEM è il luogo da cui il contenuto del sito web viene modificato, gestito e pubblicato. Per creare un sito AEM distribuito tramite Edge Delivery Services e creato utilizzando l’editor universale, utilizza il [modello del sito di authoring AEM con Edge Delivery Service](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases) per creare un nuovo sito in AEM Author.

Il sito AEM è il luogo in cui il contenuto del sito web viene archiviato e creato. L’esperienza finale è una combinazione del contenuto del sito AEM con il [codice del sito web](./1-new-code-project.md).

![Nuovo sito AEM per Edge Delivery Services ed editor universale](./assets/2-new-aem-site/new-site.png)

Segui i [passaggi dettagliati descritti nella documentazione](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-aem-site) per creare un nuovo sito AEM.  Di seguito è riportato un elenco riepilogativo dei passaggi, inclusi i valori utilizzati in questo tutorial.
1. **Crea un nuovo sito** in AEM Author. Questo tutorial utilizza la seguente denominazione del sito:
   * Titolo del sito: `WKND (Universal Editor)`
   * Nome del sito: `aem-wknd-eds-ue`

      * Il valore del nome del sito deve corrispondere al nome del percorso del sito [aggiunto a `paths.json`](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/path-mapping).

2. **Importa il modello più recente** da [Edge Delivery Services con il modello del sito di authoring di AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases).
3. **Assegna un nome al sito** in modo che corrisponda al nome dell’archivio GitHub e imposta l’URL GitHub come URL dell’archivio.

## Pubblicare il nuovo sito in anteprima

Dopo aver creato il sito in AEM Author, pubblicalo nell’anteprima di Edge Delivery Services rendendo il contenuto disponibile per l’[ambiente di sviluppo locale](./3-local-development-environment.md).

1. Accedi ad **AEM Author** e passa a **Sites**.
2. Seleziona il **nuovo sito** (`WKND (Universal Editor)`) e fai clic su **Gestisci pubblicazioni**.
3. Scegli **Anteprima** in **Destinazioni** e fai clic su **Avanti**.
4. In **Includi impostazioni figlio**, seleziona **Includi elementi figlio**, deseleziona altre opzioni e fai clic su **OK**.
5. Fai clic su **Pubblica** per pubblicare il contenuto del sito in anteprima.
6. Una volta pubblicate in anteprima, le pagine sono disponibili nell’ambiente di anteprima di Edge Delivery Services (non verranno visualizzate nel servizio Preview AEM).
