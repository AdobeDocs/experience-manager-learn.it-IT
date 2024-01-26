---
title: Certificazione del documento in AEM Forms
description: Utilizzo del servizio Docassurance per certificare i documenti PDF in AEM Forms
feature: Document Security
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1471929f-d269-4adc-88ad-2ad3682305e1
last-substantial-update: 2019-07-07T00:00:00Z
duration: 96
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 0%

---

# Certificazione del documento in AEM Forms

Un documento certificato fornisce ai PDF documenti e forma i destinatari con ulteriori garanzie sulla sua autenticità e integrità.

Per certificare un documento, puoi utilizzare Acrobat DC sul desktop o AEM Forms Document Services come parte di un processo automatizzato su un server.

Questo articolo fornisce un esempio di bundle OSGI per certificare documenti pdf utilizzando AEM Forms Document Services. Il codice utilizzato nell’esempio è [disponibile qui](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Per certificare i documenti tramite AEM Forms, è necessario seguire i passaggi seguenti

## Aggiunta del certificato all&#39;archivio fonti attendibili {#adding-certificate-to-trust-store}

Segui i passaggi indicati di seguito per aggiungere il certificato a keystore in AEM

* [Inizializza archivio fonti attendibili globale](http://localhost:4502/libs/granite/security/content/truststore.html)
* [Cerca fd-service](http://localhost:4502/security/users.html) utente
* **Per trovare l&#39;utente fd-service dovrai scorrere la pagina dei risultati per caricare tutti gli utenti**
* Fare doppio clic sull&#39;utente di fd-service per aprire la finestra delle impostazioni utente
* Fai clic su &quot;Aggiungi chiave privata dal file keystore&quot;.Specifica l’alias e la password specifici per il certificato
  ![add-certificate](assets/adding-certificate-keystore.PNG)
* Salva le modifiche

## Creazione del servizio OSGI

Puoi scrivere il tuo bundle OSGi e utilizzare AEM Forms Client SDK per implementare un servizio per certificare i documenti di PDF. I seguenti collegamenti sarebbero utili per scrivere il tuo bundle OSGi

* [Creazione del primo bundle OSGi](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [Utilizzare l’API Document Service](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Oppure puoi utilizzare il bundle di esempio incluso come parte di queste risorse di esercitazione.

>[!NOTE]
>
>Il bundle di esempio utilizza l’alias denominato &quot;ares&quot; per certificare i documenti. Assicurati quindi che l’alias sia denominato &quot;ares&quot; quando utilizzi questo bundle

## Verifica del campione nel sistema locale

* Scarica e installa [Bundle servizi documenti personalizzati](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Scarica e installa [Sviluppo con Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Assicurati di aver aggiunto la seguente voce nel servizio User Mapper di Apache Sling Service](http://localhost:4502/system/console/configMgr)
  **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** come mostrato nella schermata seguente
  ![User-Mapper](assets/user-mapper-service.PNG)
* [Importa modulo adattivo di esempio](assets/certify-pdf-af.zip)
* [Importare e installare l’invio personalizzato](assets/custom-submit-certify.zip)
* [Aprire il modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* Carica documento PDF che deve essere certificato
  **facoltativo** - Specificare il campo firma da utilizzare per la certificazione del documento
* Fai clic su Invia.
* Certified PDF deve essere restituito al cliente.
