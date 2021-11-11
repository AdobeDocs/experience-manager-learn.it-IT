---
title: Invio della configurazione dei servizi cloud e del modello dati del modulo all’istanza cloud
description: Crea e invia un modulo adattivo basato sul modello dati del modulo di archiviazione di Azure all’istanza cloud.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 9006
source-git-commit: 8484897297940ab28619c4b1af5362a5937eadfa
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 0%

---


# Includere la configurazione dei servizi cloud nel progetto

Crea un contenitore di configurazione denominato &quot;FormsTutorial&quot; per contenere la configurazione dei servizi cloud Crea una configurazione di servizi cloud per l’archiviazione di Azure denominata &quot;Archivia invii moduli in Azure&quot; nel contenitore &quot;FormsTutorial&quot;. Fornire i dettagli dell’account di archiviazione di Azure e la chiave dell’account

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
>Ora, quando crei e distribuisci il progetto, il modello dati del modulo sarà basato sulla configurazione dei servizi cloud disponibile nella tua istanza cloud
