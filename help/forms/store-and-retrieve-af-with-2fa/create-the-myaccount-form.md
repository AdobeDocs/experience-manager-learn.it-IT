---
title: Creare MyAccountForm
description: Crea il modulo myaccount per recuperare il modulo parzialmente completato in seguito alla verifica dell’ID applicazione e del numero di telefono.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6599
thumbnail: 6599.jpg
topic: Development
role: User
level: Beginner
exl-id: 1ecd8bc0-068f-4557-bce4-85347c295ce0
duration: 68
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# Creare MyAccountForm

Il modulo **ModuloAccount** viene utilizzato per recuperare il modulo adattivo parzialmente completato dopo che l’utente ha verificato l’id applicazione e il numero di cellulare associato all’id applicazione.

![modulo del mio account](assets/6599.JPG)

Quando l’utente immette l’ID applicazione e fa clic su **FetchApplication** , il numero di cellulare associato all’ID applicazione viene recuperato dal database utilizzando l’operazione Get del modello dati del modulo.

Questo modulo utilizza la chiamata POST del modello dati del modulo per verificare il numero di cellulare utilizzando OTP. L’azione di invio del modulo viene attivata in seguito alla verifica del numero di cellulare eseguita correttamente utilizzando il seguente codice. Stiamo attivando l’evento clic del pulsante di invio denominato **submitForm**.

>[!NOTE]
> Dovrai fornire la chiave API e i valori del segreto API specifici per il tuo [Nexmo](https://dashboard.nexmo.com/) account nei campi appropriati di MyAccountForm

![trigger-submit](assets/trigger-submit.JPG)



Questo modulo è associato a un’azione di invio personalizzata che inoltra l’invio del modulo al servlet installato su **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

Il codice nel servlet installato su **/bin/renderaf** inoltra la richiesta di eseguire il rendering del modulo adattivo storeafwithattachments precompilato con i dati salvati.


* MyAccountForm può essere [scaricato da qui](assets/my-account-form.zip)

* I moduli di esempio si basano su [modello di modulo adattivo personalizzato](assets/custom-template-with-page-component.zip) che deve essere importato nell&#39;AEM affinché i moduli di esempio vengano riprodotti correttamente.

* [Gestore di invio personalizzato](assets/custom-submit-my-account-form.zip) associato all’invio MyAccountForm deve essere importato in AEM.

## Passaggi successivi

[Testare la soluzione distribuendo le risorse di esempio](./deploy-this-sample.md)
