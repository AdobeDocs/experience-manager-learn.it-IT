---
title: Creare MyAccountForm
description: Crea il modulo myaccount per recuperare il modulo parzialmente completato in seguito alla verifica dell’ID applicazione e del numero di telefono.
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6599
thumbnail: 6599.jpg
topic: Development
role: User
level: Beginner
exl-id: 1ecd8bc0-068f-4557-bce4-85347c295ce0
duration: 53
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# Creare MyAccountForm

Il modulo **ModuloAccountPersonale** viene utilizzato per recuperare il modulo adattivo parzialmente completato dopo che l&#39;utente ha verificato l&#39;ID applicazione e il numero di cellulare associato all&#39;ID applicazione.

![modulo del mio account](assets/6599.JPG)

Quando l&#39;utente immette l&#39;ID applicazione e fa clic sul pulsante **FetchApplication**, il numero di cellulare associato all&#39;ID applicazione viene recuperato dal database utilizzando l&#39;operazione Get del modello dati del modulo.

Questo modulo utilizza la chiamata POST del modello dati del modulo per verificare il numero di cellulare utilizzando OTP. L’azione di invio del modulo viene attivata in seguito alla verifica del numero di cellulare eseguita correttamente utilizzando il seguente codice. Stiamo attivando l&#39;evento clic del pulsante di invio denominato **submitForm**.

>[!NOTE]
> Dovrai fornire la chiave API e i valori del segreto API specifici per il tuo account [Nexmo](https://dashboard.nexmo.com/) nei campi appropriati di MyAccountForm

![trigger-submit](assets/trigger-submit.JPG)



Questo modulo è associato a un&#39;azione di invio personalizzata che inoltra l&#39;invio del modulo al servlet installato su **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

Il codice nel servlet installato su **/bin/renderaf** inoltra la richiesta di eseguire il rendering del modulo adattivo storeafwithattachments precompilato con i dati salvati.


* MyAccountForm può essere [scaricato da qui](assets/my-account-form.zip)

* I moduli di esempio si basano su [modello di modulo adattivo personalizzato](assets/custom-template-with-page-component.zip) che deve essere importato in AEM affinché i moduli di esempio vengano riprodotti correttamente.

* [È necessario importare in AEM il gestore di invio personalizzato](assets/custom-submit-my-account-form.zip) associato all&#39;invio MyAccountForm.

## Passaggi successivi

[Testare la soluzione distribuendo le risorse di esempio](./deploy-this-sample.md)
