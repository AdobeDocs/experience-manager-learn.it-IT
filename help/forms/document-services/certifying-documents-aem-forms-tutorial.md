---
title: Documento di certificazione in AEM Forms
seo-title: Documento di certificazione in AEM Forms
description: Utilizzo del servizio DocAssurance per la certificazione di documenti PDF in AEM Forms
seo-description: Utilizzo del servizio DocAssurance per la certificazione di documenti PDF in AEM Forms
uuid: ecb1f9b6-bbb3-43a3-a0e0-4c04411acc9f
feature: document-security
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 0%

---


# Documento di certificazione in AEM Forms

Un documento certificato fornisce ai destinatari di documenti PDF e moduli ulteriori garanzie di autenticità e integrità.

Per certificare un documento, è possibile utilizzare Acrobat DC sul desktop o AEM Forms Document Services come parte di un processo automatizzato su un server.

Questo articolo fornisce un esempio di bundle OSGI per certificare documenti pdf utilizzando AEM Forms Document Services.Il codice utilizzato nell&#39;esempio è [disponibile qui](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Per certificare i documenti utilizzando AEM Forms, è necessario seguire i seguenti passaggi

## Aggiunta del certificato all&#39;archivio attendibilità {#adding-certificate-to-trust-store}

Segui i passaggi indicati di seguito per aggiungere il certificato al keystore in AEM

* [Inizializza archivio di attendibilità globale](http://localhost:4502/libs/granite/security/content/truststore.html)
* [Cerca fd-](http://localhost:4502/security/users.html) serviceuser
* **Sarà necessario scorrere la pagina dei risultati per caricare tutti gli utenti per trovare l&#39;utente del servizio fd**
* Fare doppio clic sull&#39;utente del servizio fd per aprire la finestra delle impostazioni utente
* Fai clic su &quot;Aggiungi chiave privata dal file del keystore&quot;.Specifica l&#39;alias e la password specifici del certificato
   ![certificato aggiuntivo](assets/adding-certificate-keystore.PNG)
* Salva le modifiche

## Creazione del servizio OSGI

Puoi scrivere il tuo bundle OSGi e utilizzare l’SDK client di AEM Forms per implementare un servizio per la certificazione dei documenti PDF. I seguenti collegamenti sarebbero utili per scrivere il tuo bundle OSGi

* [Creazione del primo bundle OSGi](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [Utilizzare l’API di Document Service](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Oppure puoi utilizzare il bundle di esempio incluso come parte di questa esercitazione.

>[!NOTE]
>
>Il bundle di esempio utilizza un alias chiamato &quot;ares&quot; per certificare i documenti. Quindi, assicurati che il tuo alias sia chiamato &quot;ares&quot; quando utilizzi questo bundle

## Verifica del campione sul sistema locale

* Scarica e installa [Bundle servizi documenti personalizzati](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Scarica e installa [Sviluppo con Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Assicurati di aver aggiunto la seguente voce nel servizio User Mapper di Apache Sling Service](http://localhost:4502/system/console/configMgr)

   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-** servicecome mostrato nella schermata seguente
   ![User-Mapper](assets/user-mapper-service.PNG)
* [Importa modulo adattivo di esempio](assets/certify-pdf-af.zip)
* [Importare e installare l’invio personalizzato](assets/custom-submit-certify.zip)
* [Apri il modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* Carica un documento PDF che deve essere certificato
   **facoltativo** : specifica il campo firma da utilizzare per la certificazione del documento.
* Fare clic su Invia.
* Il PDF certificato deve essere restituito all&#39;utente.


