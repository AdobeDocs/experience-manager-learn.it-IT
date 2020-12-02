---
title: Creare MyAccountForm
description: Creare il modulo per recuperare il modulo parzialmente completato in seguito alla verifica corretta dell'ID applicazione e del numero di telefono.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---



# Creare MyAccountForm

Il modulo **MyAccountForm** viene utilizzato per recuperare il modulo adattivo parzialmente completato dopo che l&#39;utente ha verificato l&#39;ID applicazione e il numero di cellulare associati all&#39;ID applicazione.

![modulo del mio account](assets/6599.JPG)

Quando l&#39;utente immette l&#39;ID applicazione e fa clic sul pulsante **FetchApplication**, il numero mobile associato all&#39;ID applicazione viene recuperato dal database utilizzando l&#39;operazione Get del modello dati del modulo.

In questo modulo viene utilizzata la chiamata POST del modello dati modulo per verificare il numero di cellulare utilizzando l&#39;OTP. L&#39;azione di invio del modulo viene attivata in seguito alla verifica del numero di cellulare effettuata con successo utilizzando il seguente codice. È in corso l&#39;attivazione dell&#39;evento click del pulsante di invio denominato **submitForm**.

>[!NOTE]
> Dovrete fornire la chiave API e i valori Segreto API specifici per l&#39;account [Nexmo](https://dashboard.nexmo.com/) nei campi appropriati di MyAccountForm

![trigger-submit](assets/trigger-submit.JPG)



Questo modulo è associato a un&#39;azione di invio personalizzata che inoltra l&#39;invio del modulo al servlet installato su **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

Il codice nel servlet montato su **/bin/renderaf** inoltra la richiesta di eseguire il rendering del storeafwith attachments adaptive form precompilato con i dati salvati.


* MyAccountForm può essere [scaricato da qui](assets/my-account-form.zip)

* I moduli di esempio si basano su [modello di modulo adattivo personalizzato](assets/custom-template-with-page-component.zip) che è necessario importare in AEM affinché il rendering dei moduli di esempio sia corretto.

* [È ](assets/custom-submit-my-account-form.zip) necessario importare in AEM il gestore di invio personalizzato associato all&#39;invio MyAccountForm.
