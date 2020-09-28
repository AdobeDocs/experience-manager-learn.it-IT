---
title: Certificazione del documento in  AEM Forms
seo-title: Certificazione del documento in  AEM Forms
description: Utilizzo del servizio DocAssurance per la certificazione di documenti PDF in  AEM Forms
seo-description: Utilizzo del servizio DocAssurance per la certificazione di documenti PDF in  AEM Forms
uuid: ecb1f9b6-bbb3-43a3-a0e0-4c04411acc9f
feature: document-security
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: ca4a8f02ea9ec5db15dbe6f322731748da90be6b
workflow-type: tm+mt
source-wordcount: '416'
ht-degree: 0%

---


# Certificazione del documento in  AEM Forms

Un documento certificato fornisce ai destinatari di documenti PDF e moduli ulteriori garanzie di autenticità e integrità.

Per certificare un documento, è possibile utilizzare  Acrobat DC sul desktop o  AEM Forms Document Services come parte di un processo automatizzato su un server.

Questo articolo fornisce un pacchetto OSGI di esempio per certificare i documenti pdf utilizzando  AEM Forms Document Services.Il codice utilizzato nell&#39;esempio è [disponibile qui](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Per certificare i documenti utilizzando  AEM Forms, è necessario seguire i passaggi seguenti

## Aggiunta di un certificato all&#39;archivio attendibili {#adding-certificate-to-trust-store}

Segui i passaggi indicati di seguito per aggiungere il certificato al keystore in AEM

* [Inizializzazione archivio trust globale](http://localhost:4502/libs/granite/security/content/truststore.html)
* [Cerca utente fd-service](http://localhost:4502/security/users.html)
* **Sarà necessario scorrere la pagina dei risultati per caricare tutti gli utenti per trovare l&#39;utente del servizio fd**
* Fate doppio clic sull’utente del servizio fd per aprire la finestra delle impostazioni utente
* Fare clic su &quot;Aggiungi chiave privata dal file dell&#39;archivio di chiavi&quot;.Specificare l&#39;alias e la password specifici per il certificato
   ![add-certificate](assets/adding-certificate-keystore.PNG)
* Salvare le modifiche

## Creazione di servizi OSGI

Potete creare un pacchetto OSGi personalizzato e utilizzare l&#39;SDK client AEM Forms  per implementare un servizio per la certificazione dei documenti PDF. I seguenti collegamenti sono utili per creare un pacchetto OSGi personalizzato

* [Creazione del primo bundle OSGi](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [Usa API Document Service](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Oppure puoi usare il bundle di esempio incluso in questa esercitazione.
>[!NOTE]
Il bundle di esempio utilizza l&#39;alias &quot;ares&quot; per certificare i documenti. Quindi, assicurarsi che il vostro alias sia chiamato &quot;ares&quot; quando si utilizza questo bundle

## Verifica del campione sul sistema locale

* Download e installazione di Document Services Bundle [personalizzato](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Scaricare e installare [Developing with Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Assicuratevi di aver aggiunto la seguente voce nel servizio Apache Sling Service User Mapper Service](http://localhost:4502/system/console/configMgr)

   **DevelopingWithServiceUser.core:getformsresources ceresolver=fd-service** come mostrato nella schermata seguente
   ![User-Mapper](assets/user-mapper-service.PNG)
* [Importa modulo adattivo di esempio](assets/certify-pdf-af.zip)
* [Importa e installa invio personalizzato](assets/custom-submit-certify.zip)
* [Aprire il modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* Caricare un documento PDF da certificare
   **facoltativo** - Specificare il campo firma da utilizzare per la certificazione del documento
* Fate clic su Invia.
* Il PDF certificato deve essere restituito.


