---
title: Distribuire le risorse di esempio sul server
description: Ottieni il caso d’uso che funziona sul server locale
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
source-git-commit: 155e6e42d4251b731d00e2b456004016152f81fe
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# Distribuire le risorse

Le risorse/configurazioni seguenti sono state distribuite su un server di pubblicazione AEM Forms.

* [Adobe Sign Wrapper Bundle](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [Modello di comunicazione interattiva di esempio](assets/waiver-interactive-communication.zip)
* [Distribuire il bundle DevelopingWithServiceUser](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)
* Aggiungi la seguente voce nel servizio User Mapper di Apache Sling Service utilizzando OSGi configMgr
   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**
* [Il codice App React di esempio può essere scaricato da qui](assets/src.zip)



L’app di esempio react deve essere distribuita nell’ambiente locale

Dovrai modificare l’URL dell’endpoint in modo che corrisponda al tuo ambiente. Apri il file EmergencyContact.js e modifica l&#39;URL nel metodo fetch

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

Per abilitare l’esecuzione di chiamate POST all’endpoint AEM dall’app REACT, dovrai specificare le voci appropriate nel campo delle origini consentite nella configurazione dei criteri di condivisione risorse tra le origini di Adobe Granite

![impostazione cors](assets/cors-settings.png)



