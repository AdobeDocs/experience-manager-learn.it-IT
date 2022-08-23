---
title: Invio della configurazione dei servizi cloud e del modello dati del modulo all’istanza cloud
description: Crea e invia un modulo adattivo basato sul modello dati del modulo di archiviazione di Azure all’istanza cloud.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 9006
exl-id: 77c00a35-43bf-485f-ac12-0fffb307dc16
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# Includere la configurazione dei servizi cloud nel progetto

Crea un contenitore di configurazione denominato &#39;FormTutorial&#39; per contenere la configurazione dei servizi cloud Crea una configurazione di servizi cloud per l&#39;archiviazione Azure denominata &#39;FormsCSAndAzureBlob&#39; nel contenitore &#39;FormTutorial&#39; fornendo i dettagli dell&#39;account di archiviazione Azure e la chiave di accesso di Azure.

Apri il tuo progetto AEM in IntelliJ. Assicurati di aggiungere la cartella FormTutorial come mostrato di seguito nel progetto ui.content
![cloud-services-configuration](assets/cloud-services-configuration.png)

Assicurati di aggiungere la seguente voce nel filtro.xml del progetto ui.content

```xml
<filter root="/conf/FormTutorial" mode="replace"/>
```

![filter-xml](assets/ui-content-filter.png)

## Includere il modello dati modulo nel progetto

Crea un modello dati modulo basato sulla configurazione dei servizi cloud creata nel passaggio precedente. Per includere nel progetto il modello dati modulo, creare la struttura di cartelle appropriata nel progetto AEM in intelliJ. Ad esempio, il modello dati del modulo si trova in una cartella denominata registrazioni
![contenuto fdm](assets/ui-content-fdm.png)

Includi la voce appropriata nel filtro.xml del progetto ui.content

```xml
<filter root="/content/dam/formsanddocuments-fdm/registrations" mode="replace"/>
```


>[!NOTE]
>
>Ora, quando crei e distribuisci il progetto utilizzando cloud manager, dovrai immettere nuovamente la chiave di accesso di Azure nella configurazione dei servizi cloud. Per evitare di reimmettere la chiave di accesso, è consigliabile creare una configurazione in base al contesto utilizzando le variabili di ambiente come spiegato in [articolo successivo](./context-aware-fdm.md)
