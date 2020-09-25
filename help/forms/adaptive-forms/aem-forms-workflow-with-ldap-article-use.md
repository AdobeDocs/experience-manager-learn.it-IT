---
title: Utilizzo di ldap con Forms Workflow Aem
seo-title: Utilizzo di ldap con Forms Workflow Aem
description: Assegnazione 'attività del flusso di lavoro AEM Forms al manager dell'autore dell'invio
seo-description: Assegnazione 'attività del flusso di lavoro AEM Forms al manager dell'autore dell'invio
feature: adaptive-forms,workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
uuid: 3e32c3a7-387f-4652-8a94-4e6aa6cd5ab8
discoiquuid: 671872b3-3de0-40da-9691-f8b7e88a9443
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 0%

---


# Utilizzo di LDAP con  flusso di lavoro AEM Forms

Assegnazione &#39;attività del flusso di lavoro AEM Forms al manager dell&#39;autore dell&#39;invio.

Quando si utilizza il modulo adattivo nel flusso di lavoro AEM, è necessario assegnare dinamicamente un&#39;attività al manager dell&#39;autore del modulo. Per ottenere questo caso di utilizzo, dovremo configurare AEM con Ldap.

I passaggi necessari per configurare AEM con LDAP sono descritti in [dettaglio qui.](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)

Ai fini di questo articolo, allego i file di configurazione utilizzati per configurare AEM con  Ldap Adobe. Questi file sono inclusi nel pacchetto che può essere importato utilizzando il gestore pacchetti.

Nella schermata seguente, tutti gli utenti appartenenti a un particolare centro costi. Se desiderate recuperare tutti gli utenti nel vostro LDAP, non potete usare il filtro aggiuntivo.

![Configurazione LDAP](assets/costcenterldap.gif)

Nella schermata seguente, assegniamo i gruppi agli utenti recuperati da LDAP a AEM. Tenere presente il gruppo di utenti-moduli assegnato agli utenti importati. L&#39;utente deve essere membro di questo gruppo per l&#39;interazione con  AEM Forms. La proprietà manager viene inoltre memorizzata nel nodo profilo/manager in AEM.

![Synchandler](assets/synchandler.gif)

Dopo aver configurato LDAP e importato gli utenti in AEM, è possibile creare un flusso di lavoro che assegnerà l’attività al manager degli autori. Ai fini di questo articolo, abbiamo sviluppato un semplice flusso di lavoro di approvazione in un solo passaggio.

Il primo passaggio del flusso di lavoro imposta il valore del passaggio iniziale su No. La regola business nel modulo adattivo disattiverà il pannello &quot;Dettagli mittente&quot; e mostrerà il pannello &quot;Approvato da&quot; in base al valore iniziale.

Il secondo passaggio assegna l&#39;attività al manager dell&#39;autore dell&#39;invio. Otteniamo il manager dell&#39;utente che ha posto la domanda utilizzando il codice personalizzato.

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

Lo snippet di codice deve recuperare l’ID manager e assegnare l’attività al manager.

L’utente che ha avviato il flusso di lavoro è tenuto in considerazione. Quindi otteniamo il valore della proprietà manager.

A seconda di come la proprietà manager è memorizzata nel LDAP, potrebbe essere necessario modificare le stringhe per ottenere l’ID manager.

Leggete questo articolo per implementare il vostro [ ParticipantChooser .](https://helpx.adobe.com/experience-manager/using/dynamic-steps.html)

Per eseguire il test sul sistema (per  dipendenti di Adobe è possibile utilizzare questo esempio)

* [Scaricate e distribuite il bundle](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)setvalue. Questo è il bundle OSGI personalizzato per l&#39;impostazione della proprietà del manager.
* [Download e installazione di DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Importa le risorse associate a questo articolo in AEM utilizzando il gestore](assets/aem-forms-ldap.zip)pacchetti. Incluso come parte di questo pacchetto sono i file di configurazione LDAP, il flusso di lavoro e un modulo adattivo.
* Configurare AEM con LDAP utilizzando le credenziali LDAP appropriate.
* Effettuate il login per AEM utilizzando le credenziali LDAP.
* Aprire il modulo [timeoffrequest](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Compila il modulo e invia.
* Il responsabile dell&#39;utente che ha posto la domanda deve ottenere il modulo per la revisione.

>[!NOTE]
>
>Questo codice personalizzato per l’estrazione del nome del manager è stato testato rispetto  LDAP Adobe. Se state eseguendo questo codice rispetto a un altro LDAP, dovrete modificare o scrivere la vostra implementazione getParticipant per ottenere il nome del manager.
