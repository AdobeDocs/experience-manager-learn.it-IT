---
title: Creare il modulo MyAccount
description: Crea il modulo myaccount per recuperare il modulo parzialmente compilato dopo aver verificato con successo l'ID applicazione e il numero di telefono.
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
topic: Development
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 1%

---



# Creare il modulo MyAccount

Il modulo **MyAccountForm** viene utilizzato per recuperare il modulo adattivo parzialmente completato dopo che l&#39;utente ha verificato l&#39;ID dell&#39;applicazione e il numero di cellulare associati all&#39;ID dell&#39;applicazione.

![modulo del mio account](assets/6599.JPG)

Quando l&#39;utente immette l&#39;ID applicazione e fa clic sul pulsante **FetchApplication**, il numero mobile associato all&#39;ID applicazione viene recuperato dal database utilizzando l&#39;operazione Get del modello dati del modulo.

Questo modulo utilizza la chiamata POST del modello dati del modulo per verificare il numero mobile utilizzando OTP. L’azione di invio del modulo viene attivata dopo la verifica corretta del numero di cellulare utilizzando il seguente codice. Stiamo attivando l&#39;evento clic del pulsante di invio denominato **submitForm**.

>[!NOTE]
> Dovrai fornire la Chiave API e i valori Segreto API specifici per il tuo account [Nexmo](https://dashboard.nexmo.com/) nei campi appropriati di MyAccountForm

![trigger-submit](assets/trigger-submit.JPG)



Questo modulo è associato a un&#39;azione di invio personalizzata che inoltra l&#39;invio del modulo al servlet montato su **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

Il codice nel servlet montato su **/bin/renderaf** inoltra la richiesta di rendering del modulo adattivo storeafwithattachment precompilato con i dati salvati.


* MyAccountForm può essere [scaricato da qui](assets/my-account-form.zip)

* I moduli di esempio si basano su [modello di modulo adattivo personalizzato](assets/custom-template-with-page-component.zip) che è necessario importare in AEM affinché il rendering dei moduli di esempio sia corretto.

* [È necessario importare in AEM i gestori di invio personalizzati ](assets/custom-submit-my-account-form.zip) associati all’invio di MyAccountForm.
