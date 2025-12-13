---
title: Eseguire un processo sull’istanza leader in AEM as a Cloud Service
description: Scopri come eseguire un processo sull’istanza leader in AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
topic: Development
feature: OSGI, Cloud Manager
role: Developer
level: Intermediate, Experienced
doc-type: Article
duration: 0
last-substantial-update: 2024-10-23T00:00:00Z
jira: KT-16399
thumbnail: KT-16399.jpeg
exl-id: b8b88fc1-1de1-4b5e-8c65-d94fcfffc5a5
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '557'
ht-degree: 0%

---

# Eseguire un processo sull’istanza leader in AEM as a Cloud Service

Scopri come eseguire un processo sull’istanza leader nel servizio AEM Author come parte di AEM as a Cloud Service e come configurarlo per eseguirlo una sola volta.

I processi Sling sono attività asincrone che operano in background, progettate per gestire eventi attivati dal sistema o dall’utente. Per impostazione predefinita, questi processi vengono distribuiti equamente in tutte le istanze (pod) del cluster.

Per ulteriori informazioni, consulta [Gestione eventi e processi Sling Apache](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html).

## Creare ed elaborare processi

A scopo dimostrativo, creiamo un semplice _processo che indica al processore del processo di registrare un messaggio_.

### Creare un processo

Utilizza il codice seguente per _creare_ un processo Apache Sling:

```java
package com.adobe.aem.guides.wknd.core.sling.jobs.impl;

import java.util.HashMap;
import java.util.Map;

import org.apache.sling.event.jobs.JobManager;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(immediate = true)
public class SimpleJobCreaterImpl {

    private static final Logger log = LoggerFactory.getLogger(SimpleJobCreaterImpl.class);

    // Define the topic on which the job will be created
    protected static final String TOPIC = "wknd/simple/job/topic";

    // Inject a JobManager
    @Reference
    private JobManager jobManager;

    @Activate
    protected final void activate() throws Exception {
        log.info("SimpleJobCreater activated successfully");
        createJob();
        log.info("SimpleJobCreater created a job");
    }

    private void createJob() {
        // Create a job and add it on the above defined topic
        Map<String, Object> jobProperties = new HashMap<>();
        jobProperties.put("action", "log");
        jobProperties.put("message", "Job metadata is: Created in activate method");
        jobManager.addJob(TOPIC, jobProperties);
    }
}
```

I punti chiave da notare nel codice precedente sono:

- Il payload del processo ha due proprietà: `action` e `message`.
- Utilizzando il metodo [ di ](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/apache/sling/event/jobs/JobManager.html)JobManager`addJob(...)`, il processo viene aggiunto all&#39;argomento `wknd/simple/job/topic`.

### Elabora un processo

Utilizza il codice seguente per _elaborare_ il processo Apache Sling precedente:

```java
package com.adobe.aem.guides.wknd.core.sling.jobs.impl;

import org.apache.sling.event.jobs.Job;
import org.apache.sling.event.jobs.consumer.JobConsumer;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(service = JobConsumer.class, property = {
        JobConsumer.PROPERTY_TOPICS + "=" + SimpleJobCreaterImpl.TOPIC
}, immediate = true)
public class SimpleJobConsumerImpl implements JobConsumer {

    private static final Logger log = LoggerFactory.getLogger(SimpleJobConsumerImpl.class);

    @Override
    public JobResult process(Job job) {
        // Get the action and message properties
        String action = job.getProperty("action", String.class);
        String message = job.getProperty("message", String.class);

        // Log the message
        if ("log".equals(action)) {
            log.info("Processing WKND Job, and {}", message);
        }

        // Return a successful result
        return JobResult.OK;
    }

}
```

I punti chiave da notare nel codice precedente sono:

- La classe `SimpleJobConsumerImpl` implementa l&#39;interfaccia `JobConsumer`.
- È un servizio registrato per utilizzare i processi dell&#39;argomento `wknd/simple/job/topic`.
- Il metodo `process(...)` elabora il processo registrando la proprietà `message` del payload del processo.

### Elaborazione processo predefinita

Quando distribuisci il codice riportato sopra in un ambiente AEM as a Cloud Service e lo esegui sul servizio AEM Author, che funziona come cluster con più JVM di AEM Author, il processo viene eseguito una volta su ogni istanza di AEM Author (pod), il che significa che il numero di processi creati corrisponderà al numero di pod. Il numero di pod sarà sempre maggiore di uno (per ambienti non RDE), ma fluttuerà in base alla gestione interna delle risorse di AEM as a Cloud Service.

Il processo viene eseguito su ogni istanza di AEM Author (pod) perché `wknd/simple/job/topic` è associato alla coda principale di AEM, che distribuisce i processi tra tutte le istanze disponibili.

Questo è spesso problematico se il lavoro è responsabile del cambiamento dello stato, come la creazione o l&#39;aggiornamento di risorse o servizi esterni.

Se si desidera eseguire il processo una sola volta nel servizio AEM Author, aggiungere la [configurazione della coda processi](#how-to-run-a-job-on-the-leader-instance) descritta di seguito.

Puoi verificarlo consultando i registri del servizio AEM Author in [Cloud Manager](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/logs#cloud-manager).

![Processo elaborato da tutte le istanze](./assets/run-job-once/job-processed-by-all-instances.png)


Dovresti vedere:

```
<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-68775db964-nxxcx] *INFO* [sling-oak-observation-15] org.apache.sling.event.impl.jobs.queues.JobQueueImpl.<main queue> Starting job queue <main queue>
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method

<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-68775db964-r4zk7] *INFO* [sling-oak-observation-11] org.apache.sling.event.impl.jobs.queues.JobQueueImpl.<main queue> Starting job queue <main queue>
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
```

Sono disponibili due voci di registro, una per ogni istanza di AEM Author (`68775db964-nxxcx` e `68775db964-r4zk7`), che indica che ogni istanza (pod) ha elaborato il processo.

## Eseguire un processo sull’istanza leader

Per eseguire un processo _una sola volta_ nel servizio AEM Author, creare una nuova coda processi Sling di tipo **Ordinato** e associare l&#39;argomento processo (`wknd/simple/job/topic`) a questa coda. Con questa configurazione, solo l’istanza Autore AEM principale (pod) potrà elaborare il processo.

Nel modulo `ui.config` del progetto AEM, crea un file di configurazione OSGi (`org.apache.sling.event.jobs.QueueConfiguration~wknd.cfg.json`) e memorizzalo nella cartella `ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author`.

```json
{
    "queue.name":"WKND Queue - ORDERED",
    "queue.topics":[
      "wknd/simple/job/topic"
    ],
    "queue.type":"ORDERED",
    "queue.retries":1,
    "queue.maxparallel":1.0
  }
```

I punti chiave da considerare nella configurazione precedente sono:

- Argomento coda impostato su `wknd/simple/job/topic`.
- Tipo di coda impostato su `ORDERED`.
- Numero massimo di processi paralleli impostato su `1`.

Dopo aver distribuito la configurazione di cui sopra, il processo verrà elaborato esclusivamente dall’istanza principale, garantendo che venga eseguito una sola volta nell’intero servizio AEM Author.

```
<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-7475cf85df-qdbq5] *INFO* [FelixLogListener] Events.Service.org.apache.sling.event Service [QueueMBean for queue WKND Queue - ORDERED,7755, [org.apache.sling.event.jobs.jmx.StatisticsMBean]] ServiceEvent REGISTERED
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
<DD.MM.YYYY HH:mm:ss.SSS> [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
```
