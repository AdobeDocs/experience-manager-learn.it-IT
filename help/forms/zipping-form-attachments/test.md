---
title: Verificare la soluzione
description: Testa la soluzione aggiungendo allegati al modulo e attiva il flusso di lavoro per inviare l’e-mail.
sub-product: forms
feature: Flusso di lavoro
topics: adaptive forms
audience: developer
doc-type: article
activity: develop
version: 6.5
topic: Sviluppo
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: e82cc5e5de6db33e82b7c71c73bb606f16b98ea6
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 1%

---


# Verificare la soluzione


* Configura [Day CQ Mail Service](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service) per inviare e-mail dal server AEM Forms
* Distribuisci il bundle [formattachments](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) utilizzando la console Web [felix](http://localhost:4502/system/console/bundles)
* Distribuisci il flusso di lavoro [SendFormAttachmentsViaEmail .](assets/zipped-form-attachments-model.zip) Questo flusso di lavoro utilizza il componente Invia e-mail per inviare il file zip_attachment.zip che viene salvato nella cartella payload dal passaggio del processo personalizzato. Configura l’indirizzo e-mail di mittenti e destinatari in base alle tue esigenze.
* Importa il [modulo di esempio](assets/zip-form-attachments-form.zip) da [Forms And Document UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Visualizzare in anteprima il ](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) modulo, aggiungere un paio di allegati e inviare il modulo.
* Il flusso di lavoro deve essere attivato e deve essere inviata una notifica e-mail con il file zip.

