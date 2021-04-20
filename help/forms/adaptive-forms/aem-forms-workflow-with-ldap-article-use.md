---
title: Utilizzo di ldap con Flusso di lavoro di Aem Forms
seo-title: Utilizzo di ldap con Flusso di lavoro di Aem Forms
description: Assegnazione dell’attività del flusso di lavoro di AEM Forms al responsabile del mittente
seo-description: Assegnazione dell’attività del flusso di lavoro di AEM Forms al responsabile del mittente
feature: Adaptive Forms,Workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
uuid: 3e32c3a7-387f-4652-8a94-4e6aa6cd5ab8
discoiquuid: 671872b3-3de0-40da-9691-f8b7e88a9443
topic: Development
role: Administrator
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 0%

---


# Utilizzo di LDAP con Flusso di lavoro di AEM Forms

Assegnazione dell’attività del flusso di lavoro di AEM Forms al responsabile del mittente.

Quando si utilizza Modulo adattivo nel flusso di lavoro AEM, è consigliabile assegnare dinamicamente un’attività al gestore dell’utente che ha inviato il modulo. Per eseguire questo caso d’uso, dovremo configurare AEM con Ldap.

I passaggi necessari per configurare AEM con LDAP sono descritti in [dettaglio qui.](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)

Ai fini del presente articolo, allego i file di configurazione utilizzati per configurare AEM con Adobe Ldap. Questi file sono inclusi nel pacchetto che può essere importato utilizzando il gestore dei pacchetti.

Nella schermata seguente, stiamo recuperando tutti gli utenti appartenenti a un particolare centro di costo. Se desideri recuperare tutti gli utenti nel tuo LDAP non puoi usare il filtro aggiuntivo.

![Configurazione LDAP](assets/costcenterldap.gif)

Nella schermata seguente, assegniamo i gruppi agli utenti recuperati da LDAP ad AEM. Tenere presente il gruppo utenti-moduli assegnato agli utenti importati. L’utente deve essere membro di questo gruppo per l’interazione con AEM Forms. Archiviiamo anche la proprietà manager sotto il nodo profilo/gestore in AEM.

![Synchandler](assets/synchandler.gif)

Una volta configurato LDAP e importato gli utenti in AEM, possiamo creare un flusso di lavoro che assegnerà l&#39;attività al manager dei sottomettitori. Ai fini di questo articolo, abbiamo sviluppato un semplice flusso di lavoro di approvazione in un solo passaggio.

Il primo passaggio nel flusso di lavoro imposta il valore di initialstep su No. La regola business nel modulo adattivo disabiliterà il pannello &quot;Dettagli mittente&quot; e mostrerà il pannello &quot;Approvato da&quot; in base al valore del passaggio iniziale.

Il secondo passaggio assegna l&#39;attività al responsabile del mittente. Otteniamo il responsabile del mittente utilizzando il codice personalizzato.

![Assegna attività](assets/assigntask.gif)

```java
public String getParticipant(WorkItem workItem, WorkflowSession wfSession, MetaDataMap arg2) throws WorkflowException{
resourceResolver = wfSession.adaptTo(ResourceResolver.class);
UserManager userManager = resourceResolver.adaptTo(UserManager.class);
Authorizable workflowInitiator = userManager.getAuthorizable(workItem.getWorkflow().getInitiator());
.
.
String managerPorperty = workflowInitiator.getProperty("profile/manager")[0].getString();
.
.

}
```

Lo snippet di codice è responsabile del recupero dell&#39;ID manager e dell&#39;assegnazione dell&#39;attività al manager.

Individuiamo la persona che ha avviato il flusso di lavoro. Poi otteniamo il valore della proprietà manager.

A seconda di come la proprietà manager viene memorizzata nel tuo LDAP, potrebbe essere necessario eseguire una qualche manipolazione di stringa per ottenere l&#39;id manager.

Leggi questo articolo per implementare il tuo [ ParticipantChooser .](https://helpx.adobe.com/experience-manager/using/dynamic-steps.html)

Per eseguire il test sul sistema (per i dipendenti Adobe puoi utilizzare questo esempio preconfigurato)

* [Scarica e distribuisci il bundle setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Questo è il bundle OSGI personalizzato per l&#39;impostazione della proprietà del manager.
* [Scarica e installa DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Importa in AEM le risorse associate a questo articolo utilizzando il gestore di pacchetti](assets/aem-forms-ldap.zip). Incluso come parte di questo pacchetto sono i file di configurazione LDAP, il flusso di lavoro e un modulo adattivo.
* Configura AEM con il tuo LDAP utilizzando le credenziali LDAP appropriate.
* Accedi ad AEM utilizzando le tue credenziali LDAP.
* Apri [timeoffrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Compila il modulo e invia.
* Il responsabile dell&#39;autore del sottomissione deve ottenere il modulo per la revisione.

>[!NOTE]
>
>Questo codice personalizzato per l&#39;estrazione del nome del manager è stato testato su Adobe LDAP. Se esegui questo codice su un LDAP diverso, dovrai modificare o scrivere la tua implementazione getParticipant per ottenere il nome del manager.
