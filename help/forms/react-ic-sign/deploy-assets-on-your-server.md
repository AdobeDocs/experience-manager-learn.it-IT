---
title: Distribuire le risorse di esempio sul server
description: Ottieni il caso d’uso funzionante sul server locale
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: f12f83fa-673a-454c-aa52-6ea769a182b7
duration: 58
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# Distribuire le risorse

Le seguenti risorse/configurazioni sono state distribuite su un server di pubblicazione AEM Forms.

* [Bundle wrapper Adobe Sign](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [Esempio di modello di comunicazione interattiva](assets/waiver-interactive-communication.zip)
* [Distribuire il bundle DevelopingWithServiceUser](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)
* Aggiungi la seguente voce nel servizio User Mapper di Apache Sling Service utilizzando OSGi configMgr
  **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**

## Distribuire l’app di reazione di esempio

* [Scarica l’app di reazione di esempio](assets/mult-step-form1.zip)
* Decomprimi il contenuto dell’app React in una nuova cartella
* Passa alla cartella ed esegui i seguenti comandi

```java
npm install
npm start
```

Apri il file EmergencyContact.js e modifica l’URL nel metodo di recupero in base all’ambiente in uso.


```javascript
 const getWebForm=async()=>
     {
        setSpinner(true)
        console.log("inside widgetURL function emergency contact");
        // NOTE: replace the `aemforms.azure.com:4503` with your AEM FORM server
        let res = await fetch("http://aemforms.azure.com:4503/bin/getwidgeturl",
          {
            method: "POST",
            body: JSON.stringify({"icTemplate":"/content/forms/af/waiver/waiver/channels/print","waiver":formData})
                     
         })
 
```

Per abilitare l’esecuzione di chiamate POST all’endpoint AEM dall’app REACT, è necessario specificare le voci appropriate nel campo Origini consentite nella configurazione dei criteri di condivisione risorse tra origini diverse di Adobe Granite.

![cors-setting](assets/cors-settings.png)

Consulta [CORS con AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html) per ulteriori dettagli sulle opzioni di configurazione CORS.
