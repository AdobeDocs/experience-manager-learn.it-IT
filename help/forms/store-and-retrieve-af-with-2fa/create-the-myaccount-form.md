---
title: Creare il modulo MyAccount
description: Crea il modulo myaccount per recuperare il modulo parzialmente compilato dopo aver verificato con successo l'ID applicazione e il numero di telefono.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
topic: Development
role: User
level: Beginner
exl-id: 1ecd8bc0-068f-4557-bce4-85347c295ce0
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# Creare il modulo MyAccount

Il modulo **MyAccountForm** viene utilizzato per recuperare il modulo adattivo parzialmente completato dopo che l’utente ha verificato l’ID applicazione e il numero di cellulare associati all’ID applicazione.

![modulo del mio account](assets/6599.JPG)

Quando l’utente immette l’ID applicazione e fa clic sul pulsante **FetchApplication** il numero mobile associato all&#39;ID applicazione viene recuperato dal database utilizzando l&#39;operazione Get del modello dati del modulo.

In questo modulo viene utilizzata la chiamata POST del modello dati del modulo per verificare il numero mobile utilizzando OTP. L’azione di invio del modulo viene attivata dopo la verifica corretta del numero di cellulare utilizzando il seguente codice. È in corso l’attivazione dell’evento clic del pulsante di invio denominato **submitForm**.

>[!NOTE]
> Dovrai fornire la chiave API e i valori del segreto API specifici per il tuo [Nexmo](https://dashboard.nexmo.com/) account nei campi appropriati di MyAccountForm

![trigger-submit](assets/trigger-submit.JPG)



Questo modulo è associato a un’azione di invio personalizzata che inoltra l’invio del modulo al servlet montato su **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

Il codice nel servlet montato su **/bin/renderaf** inoltra la richiesta di eseguire il rendering del modulo adattivo storeafwithattachment precompilato con i dati salvati.


* MyAccountForm può essere [scaricato da qui](assets/my-account-form.zip)

* I moduli di esempio si basano su [modello di modulo adattivo personalizzato](assets/custom-template-with-page-component.zip) che deve essere importato in AEM affinché il rendering dei moduli di esempio sia corretto.

* [Gestore di invio personalizzato](assets/custom-submit-my-account-form.zip) associato all&#39;invio MyAccountForm deve essere importato in AEM.

## Passaggi successivi

[Testa la soluzione distribuendo le risorse di esempio](./deploy-this-sample.md)
