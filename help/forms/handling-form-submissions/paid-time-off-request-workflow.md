---
title: Workflow semplice della richiesta a pagamento
description: Nascondere e mostrare pannelli di moduli adattivi nel flusso di lavoro AEM
uuid: 28ceb72b-24d9-488e-92af-7e85775dc682
feature: Moduli adattivi
topics: workflow
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
topic: Sviluppo
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '335'
ht-degree: 0%

---


# Workflow semplice della richiesta a pagamento

In questo articolo, osserveremo un semplice flusso di lavoro utilizzato per richiedere il tempo di pagamento disattivato. I requisiti aziendali sono i seguenti:

* L’utente A richiede tempo di inattività compilando un modulo adattivo.
* Il modulo viene indirizzato AEM utente amministratore (in tempo reale verrà indirizzato al responsabile del mittente)
* L’amministratore apre il modulo. L’amministratore non deve essere in grado di modificare le informazioni inserite dal mittente.
* La sezione Approvatore deve essere visibile all’approvatore (In questo caso è l’utente amministratore AEM).

Per soddisfare il requisito di cui sopra, nel modulo viene utilizzato un campo nascosto denominato **initialstep** e il relativo valore predefinito è impostato su Sì. Quando il modulo viene inviato, il primo passaggio nel flusso di lavoro imposta il valore di initialstep su No. Nel modulo sono presenti regole business per nascondere e visualizzare le sezioni appropriate in base al valore iniziale del passaggio.

**Configurare un modulo per attivare AEM flusso di lavoro**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=9&learn=on)

**Flusso di lavoro**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=9&learn=on)

**Visualizzazione dell&#39;autore del modulo di richiesta di disattivazione del tempo**

![initialstep](assets/initialstep.gif)

**Vista dell’approvatore del modulo**

![approvazione](assets/approversview.gif)

Nella visualizzazione approvatore, l&#39;approvatore non è in grado di modificare i dati inviati. È inoltre disponibile una nuova sezione destinata solo agli approvatori.

Per testare questo flusso di lavoro sul sistema, segui i passaggi indicati di seguito:
* [Scarica e distribuisci DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Scarica e distribuisci il bundle OSGI personalizzato SetValue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [Importa in AEM le risorse correlate a questo articolo](assets/helpxworkflow.zip)
* Apri il [modulo di richiesta di disattivazione tempo](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Compila i dettagli e invia
* Apri la [inbox](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html). Dovresti vedere una nuova attività assegnata. Apri il modulo. I dati del notificatore devono essere di sola lettura e dovrebbe essere visibile una nuova sezione approvatore.
* Esplora il [modello di flusso di lavoro](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* Esplora il passaggio del processo. Questo è il passaggio che imposta il valore di initialstep su No.
