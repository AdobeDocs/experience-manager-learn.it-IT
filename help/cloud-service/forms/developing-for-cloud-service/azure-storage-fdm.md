---
title: Invio della configurazione dei servizi cloud e del modello per dati modulo all’istanza cloud
description: Crea e invia un modulo adattivo basato sul modello dati del modulo di archiviazione Azure all’istanza cloud.
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

# Includi la configurazione dei servizi cloud nel progetto

Creare un contenitore di configurazione denominato &quot;FormTutorial&quot; per la configurazione dei servizi cloud Creare una configurazione dei servizi cloud per l’archiviazione di Azure denominata &quot;FormsCSAndAzureBlob&quot; nel contenitore &quot;FormTutorial&quot; fornendo i dettagli dell’account di archiviazione di Azure e la chiave di accesso di Azure.

Apri il progetto AEM in IntelliJ. Assicurati di aggiungere la cartella FormTutorial come mostrato di seguito nel progetto ui.content
![cloud-services-configuration](assets/cloud-services-configuration.png)

Assicurati di aggiungere la seguente voce nel file filter.xml del progetto ui.content

```xml
<filter root="/conf/FormTutorial" mode="replace"/>
```

![filter-xml](assets/ui-content-filter.png)

## Includi modello dati modulo nel progetto

Crea un modello di dati modulo basato sulla configurazione dei servizi cloud creata nel passaggio precedente. Per includere il modello dati modulo nel progetto, creare la struttura di cartelle appropriata nel progetto AEM in intelliJ. Ad esempio, il modello dati del modulo si trova in una cartella denominata registrazioni
![fdm-content](assets/ui-content-fdm.png)

Includi la voce appropriata nel file filter.xml del progetto ui.content

```xml
<filter root="/content/dam/formsanddocuments-fdm/registrations" mode="replace"/>
```


>[!NOTE]
>
>Ora, quando crei e distribuisci il progetto utilizzando Cloud Manager, dovrai immettere nuovamente la chiave di accesso di Azure nella configurazione dei servizi cloud. Per evitare di immettere nuovamente la chiave di accesso, si consiglia di creare una configurazione in base al contesto utilizzando le variabili di ambiente come spiegato in [articolo successivo](./context-aware-fdm.md)
