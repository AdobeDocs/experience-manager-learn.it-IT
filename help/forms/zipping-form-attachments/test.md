---
title: 'Test della soluzione: passaggi necessari per testare i due approcci'
description: Testa la soluzione aggiungendo allegati al modulo e attiva il flusso di lavoro per inviare l’e-mail.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: ce9b9203-b44c-4a52-821c-ae76e93312d2
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 0%

---

# Passaggi necessari per testare i due approcci

* Configura [Servizio e-mail Day CQ](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service) per inviare e-mail dal server AEM Forms
* Distribuisci [formattachment](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) bundle utilizzando [console web felix](http://localhost:4502/system/console/bundles)

## Invia file zip come allegato e-mail



* Distribuisci [Flusso di lavoro SendFormAttachmentsViaEmail .](assets/zipped-form-attachments-model.zip) Questo flusso di lavoro utilizza il componente Invia e-mail per inviare il file zip_attachment.zip che viene salvato nella cartella payload dal passaggio del processo personalizzato. Configura gli indirizzi e-mail dei mittenti e dei destinatari in base alle tue esigenze.
* Importa [modulo di esempio](assets/zip-form-attachments-form.zip) da [Interfaccia utente Forms e Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) e aggiungere un paio di allegati e inviare il modulo.
* Il flusso di lavoro deve essere attivato e deve essere inviata una notifica e-mail con il file zip.

## Inviare allegati modulo come file singoli

* Distribuisci [Flusso di lavoro SendForm.](assets/send-form-attachments-model.zip) Questo flusso di lavoro utilizza il componente Invia e-mail per inviare gli allegati del modulo come singoli file. Configura l’indirizzo e-mail di mittenti e destinatari in base alle tue esigenze.
* Importa [modulo di esempio](assets/send-list-attachments-form.zip) da [Interfaccia utente Forms e Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled) e aggiungere un paio di allegati e inviare il modulo.
* Il flusso di lavoro deve essere attivato e viene inviata una notifica e-mail con gli allegati del modulo.
