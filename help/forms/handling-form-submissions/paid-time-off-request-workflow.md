---
title: Flusso di lavoro semplice con tempo pagato disattivato
description: Nascondere e visualizzare i pannelli dei moduli adattivi nel flusso di lavoro AEM
uuid: 28ceb72b-24d9-488e-92af-7e85775dc682
feature: integrations
topics: workflow
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 0%

---


# Flusso di lavoro semplice con tempo pagato disattivato

In questo articolo, verrà illustrato un semplice flusso di lavoro utilizzato per richiedere il tempo di pagamento disattivato. I requisiti aziendali sono i seguenti:

* L&#39;utente A richiede tempo compilando un modulo adattivo.
* Il modulo viene instradato AEM utente amministratore (in tempo reale verrà instradato al manager del notificatore)
* L&#39;amministratore apre il modulo. L&#39;amministratore non deve essere in grado di modificare le informazioni compilate dal mittente.
* La sezione Approver deve essere visibile all&#39;approver (in questo caso è l&#39;utente amministratore AEM).

Per soddisfare il requisito precedente, nel modulo viene utilizzato un campo nascosto denominato **initialstep** e il relativo valore predefinito è impostato su Sì. Quando il modulo viene inviato, il primo passaggio del flusso di lavoro imposta il valore dell&#39;inizialstep su No. Nel modulo sono presenti regole business per nascondere e mostrare le sezioni appropriate in base al valore dell&#39;istruzione iniziale.

**Configura modulo per attivare AEM flusso di lavoro**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=9&learn=on)

**Flusso di lavoro**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=9&learn=on)

**Visualizzazione dell&#39;utente che ha inviato il modulo di richiesta del tempo di disattivazione**

![initialstep](assets/initialstep.gif)

**Vista approver del modulo**

![approvverview](assets/approversview.gif)

Nella vista approver, l&#39;approver non è in grado di modificare i dati inviati. È inoltre disponibile una nuova sezione destinata solo agli approvatori.

Per testare il flusso di lavoro sul sistema, attenetevi alla procedura indicata di seguito:
* [Download e implementazione di DevelopingWitheServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Scaricare e distribuire il pacchetto OSGI personalizzato SetValue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [Importa le risorse correlate a questo articolo in AEM](assets/helpxworkflow.zip)
* Aprire il modulo [Ora di disattivazione](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Compila i dettagli e invia
* Open the [inbox](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html). Dovrebbe essere assegnata una nuova attività. Aprire il modulo. I dati dell&#39;autore del modulo devono essere di sola lettura e deve essere visibile una nuova sezione dell&#39;approver.
* Esplora il modello di [workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* Esplorate il passaggio del processo. Questo è il passo che imposta il valore di initial step su No.
