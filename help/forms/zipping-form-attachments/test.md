---
title: 'Test della soluzione: passaggi necessari per testare i due approcci'
description: Verifica la soluzione aggiungendo allegati al modulo e attiva il flusso di lavoro per inviare l’e-mail.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: ce9b9203-b44c-4a52-821c-ae76e93312d2
duration: 41
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---

# Passaggi necessari per testare i due approcci

* Configura [Day CQ Mail Service](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service) per inviare messaggi di posta elettronica dal server AEM Forms
* Distribuisci il bundle [formattachments](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) tramite [console Web Felix](http://localhost:4502/system/console/bundles)

## Invia file zip come allegato e-mail



* Distribuire il flusso di lavoro [SendFormAttachmentsViaEmail.](assets/zipped-form-attachments-model.zip) Questo flusso di lavoro utilizza il componente Invia e-mail per inviare il file zipped_attachments.zip salvato nella cartella del payload dal passaggio del processo personalizzato. Configura gli indirizzi e-mail dei mittenti e dei destinatari in base alle tue esigenze.
* Importa il [modulo di esempio](assets/zip-form-attachments-form.zip) da [interfaccia utente di Forms e Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Visualizzare l&#39;anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled), aggiungere un paio di allegati e inviare il modulo.
* Il flusso di lavoro deve essere attivato e deve essere inviata una notifica e-mail con il file zip.

## Invia allegati modulo come singoli file

* Distribuire il flusso di lavoro [SendForm.](assets/send-form-attachments-model.zip) Questo flusso di lavoro utilizza il componente Invia e-mail per inviare gli allegati del modulo come singoli file. Configura l&#39;indirizzo e-mail di mittenti e destinatari in base alle tue esigenze.
* Importa il [modulo di esempio](assets/send-list-attachments-form.zip) da [interfaccia utente di Forms e Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Visualizzare l&#39;anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled), aggiungere un paio di allegati e inviare il modulo.
* Il flusso di lavoro deve essere attivato e viene inviata una notifica e-mail con gli allegati del modulo.
