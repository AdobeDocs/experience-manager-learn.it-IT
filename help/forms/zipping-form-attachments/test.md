---
title: Verificare la soluzione
description: Testa la soluzione aggiungendo allegati al modulo e attiva il flusso di lavoro per inviare l’e-mail.
feature: Moduli adattivi
version: 6.5
topic: Sviluppo
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 1%

---


# Passaggi necessari per testare i due approcci

* Configura [Day CQ Mail Service](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service) per inviare e-mail dal server AEM Forms
* Distribuisci il bundle [formattachments](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) utilizzando la console Web [felix](http://localhost:4502/system/console/bundles)

## Invia file zip come allegato e-mail



* Distribuisci il flusso di lavoro [SendFormAttachmentsViaEmail .](assets/zipped-form-attachments-model.zip) Questo flusso di lavoro utilizza il componente Invia e-mail per inviare il file zip_attachment.zip che viene salvato nella cartella payload dal passaggio del processo personalizzato. Configura gli indirizzi e-mail dei mittenti e dei destinatari in base alle tue esigenze.
* Importa il [modulo di esempio](assets/zip-form-attachments-form.zip) da [Forms And Document UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Visualizzare in anteprima il ](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) modulo, aggiungere un paio di allegati e inviare il modulo.
* Il flusso di lavoro deve essere attivato e deve essere inviata una notifica e-mail con il file zip.

## Inviare allegati modulo come file singoli

* Distribuisci il flusso di lavoro [SendForm .](assets/send-form-attachments-model.zip) Questo flusso di lavoro utilizza il componente Invia e-mail per inviare gli allegati del modulo come singoli file. Configura l’indirizzo e-mail di mittenti e destinatari in base alle tue esigenze.
* Importa il [modulo di esempio](assets/send-list-attachments-form.zip) da [Forms And Document UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Visualizzare in anteprima il ](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled) modulo, aggiungere un paio di allegati e inviare il modulo.
* Il flusso di lavoro deve essere attivato e verrà inviata una notifica e-mail con gli allegati del modulo.
