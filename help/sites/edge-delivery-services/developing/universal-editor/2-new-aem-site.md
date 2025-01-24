---
title: Creare un sito AEM
description: Creazione di un sito in AEM Sites per Edge Delivery Services, modificabile mediante l’Editor universale.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 500
source-git-commit: e8ce91b0be577ec6cf8f3ab07ba9ff09c7e7a6ab
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# Creare un sito AEM

Il sito dell&#39;AEM è il luogo da cui il contenuto del sito viene modificato, gestito e pubblicato. Per creare un sito AEM distribuito tramite Edge Delivery Services e creato tramite Universal Editor, utilizzare il modello [Edge Delivery Services con sito di creazione AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases) per creare un nuovo sito in AEM Author.

Il sito AEM è il luogo in cui il contenuto del sito web viene memorizzato e creato. L&#39;esperienza finale è una combinazione del contenuto del sito AEM con il codice del [sito Web](./1-new-code-project.md)

![Nuovo sito AEM per Edge Delivery Services ed editor universale](./assets/2-new-aem-site/new-site.png)

Per creare un nuovo sito AEM, segui i passaggi seguenti:

1. **Crea un nuovo sito** in AEM Author. Questa esercitazione utilizza la seguente denominazione del sito:
   * Titolo sito: `WKND (Universal Editor)`
   * Nome sito: `aem-wknd-eds-ue`
2. **Importa il modello più recente** da [Edge Delivery Services con il modello del sito di creazione AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases).
3. **Assegnare un nome al sito** in modo che corrisponda al nome dell&#39;archivio GitHub e impostare l&#39;URL GitHub come URL dell&#39;archivio.

Per istruzioni dettagliate, consultare la [sezione relativa alla creazione e alla modifica di un nuovo sito AEM](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-aem-site) nella guida introduttiva.

## Publish il nuovo sito da visualizzare in anteprima

Dopo aver creato il sito in AEM Author, pubblicalo nell&#39;anteprima dei Edge Delivery Services rendendo il contenuto disponibile nell&#39;[ambiente di sviluppo locale](./3-local-development-environment.md).

1. Accedi a **AEM Author** e passa a **Sites**.
2. Seleziona il **nuovo sito** (`WKND (Universal Editor)`) e fai clic su **Gestisci pubblicazioni**.
3. Scegli **Anteprima** in **Destinazioni** e fai clic su **Avanti**.
4. In **Includi impostazioni figlio**, selezionare **Includi elementi figlio**, deselezionare altre opzioni e fare clic su **OK**.
5. Fai clic su **Publish** per pubblicare il contenuto del sito in anteprima.
