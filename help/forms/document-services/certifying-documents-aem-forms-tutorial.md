---
title: Certificazione del documento in AEM Forms
description: Utilizzo del servizio Docassurance per certificare i documenti PDF in AEM Forms
feature: Document Security
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1471929f-d269-4adc-88ad-2ad3682305e1
last-substantial-update: 2019-07-07T00:00:00Z
duration: 75
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 0%

---

# Certificazione del documento in AEM Forms

Un documento certificato fornisce un documento PDF e fornisce ai destinatari ulteriori garanzie sulla sua autenticità e integrità.

Per certificare un documento, è possibile utilizzare Acrobat DC sul desktop o AEM Forms Document Services come parte di un processo automatizzato su un server.

Questo articolo fornisce un esempio di bundle OSGI per certificare documenti PDF tramite AEM Forms Document Services.Il codice utilizzato nell&#39;esempio è [disponibile qui](https://helpx.adobe.com/it/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Per certificare i documenti tramite AEM Forms, è necessario seguire i passaggi seguenti

## Aggiunta del certificato all&#39;archivio fonti attendibili {#adding-certificate-to-trust-store}

Per aggiungere il certificato al keystore in AEM, segui i passaggi indicati di seguito

* [Inizializza archivio fonti attendibili globale](http://localhost:4502/libs/granite/security/content/truststore.html)
* [Cerca utente fd-service](http://localhost:4502/security/users.html)
* **Per trovare l&#39;utente fd-service** dovrai scorrere la pagina dei risultati per caricare tutti gli utenti
* Fare doppio clic sull&#39;utente di fd-service per aprire la finestra delle impostazioni utente
* Fai clic su &quot;Aggiungi chiave privata dal file keystore&quot;.Specifica l’alias e la password specifici per il certificato
  ![certificato aggiuntivo](assets/adding-certificate-keystore.PNG)
* Salva le modifiche

## Creazione del servizio OSGI

Puoi scrivere il tuo bundle OSGi e utilizzare AEM Forms Client SDK per implementare un servizio per certificare i documenti PDF. I seguenti collegamenti sarebbero utili per scrivere il tuo bundle OSGi

* [Creazione del primo bundle OSGi](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [Utilizza Document Service API](https://helpx.adobe.com/it/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Oppure puoi utilizzare il bundle di esempio incluso come parte di queste risorse di esercitazione.

>[!NOTE]
>
>Il bundle di esempio utilizza l’alias denominato &quot;ares&quot; per certificare i documenti. Assicurati quindi che l’alias sia denominato &quot;ares&quot; quando utilizzi questo bundle

## Verifica del campione nel sistema locale

* Scarica e installa [Pacchetto servizi documentali personalizzati](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Scarica e installa [Sviluppo con Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Verifica di aver aggiunto la seguente voce al servizio User Mapper di Apache Sling Service](http://localhost:4502/system/console/configMgr)
  **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** come illustrato nella schermata seguente
  ![Utenti-Mapper](assets/user-mapper-service.PNG)
* [Importa modulo adattivo di esempio](assets/certify-pdf-af.zip)
* [Importare e installare l’invio personalizzato](assets/custom-submit-certify.zip)
* [Apri il modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* Carica il documento PDF da certificare
  **facoltativo** - Specificare il campo firma che si desidera utilizzare per certificare il documento
* Fai clic su Invia.
* PDF certificato deve essere restituito al cliente.
