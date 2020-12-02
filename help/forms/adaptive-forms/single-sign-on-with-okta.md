---
title: Configurazione di OKTA con AEM
description: Comprendere le varie impostazioni di configurazione per l'utilizzo del single sign-on con l'okta
feature: administration
topics: development, authentication, security
audience: developer
doc-type: tutorial
activity: setup
version: 6.5
translation-type: tm+mt
source-git-commit: 0b48ae445f4b32deeec08bcb68f805bf19992c9e
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---


# Autenticazione in AEM Author tramite OKTA

Il primo passaggio consiste nel configurare l&#39;app sul portale OKTA. Dopo l&#39;approvazione dell&#39;app da parte del tuo amministratore OKTA avrai accesso al certificato IdP e all&#39;URL di accesso singolo. Di seguito sono riportate le impostazioni generalmente utilizzate per la registrazione di una nuova applicazione.

* **Nome applicazione:** nome dell’applicazione. Accertatevi di assegnare un nome univoco all’applicazione.
* **Destinatario SAML:** Dopo l&#39;autenticazione da OKTA, questo è l&#39;URL che verrà colpito sull&#39;istanza AEM con la risposta SAML. In genere, il gestore di autenticazione SAML intercetta tutti gli URL con / saml_login, ma sarebbe preferibile aggiungerli dopo la radice dell&#39;applicazione.
* **Pubblico** SAML: Questo è l&#39;URL di dominio dell&#39;applicazione. Non utilizzate il protocollo (http o https) nell&#39;URL del dominio.
* **ID nome SAML:** seleziona E-mail dall’elenco a discesa.
* **Ambiente**: Scegliere l&#39;ambiente appropriato.
* **Attributi**: Questi sono gli attributi che vengono forniti all&#39;utente nella risposta SAML. Specificateli in base alle vostre esigenze.


![okta-application](assets/okta-app-settings-blurred.PNG)


## Aggiungere il certificato OKTA (IdP) all&#39;archivio AEM trust

Poiché le asserzioni SAML sono crittografate, è necessario aggiungere il certificato IdP (OKTA) all&#39;archivio AEM trust, per consentire una comunicazione sicura tra OKTA e AEM.
[Se non è già inizializzato, inizializzare l&#39;archivio](http://localhost:4502/libs/granite/security/content/truststore.html) attendibili.
Ricordare la password dell&#39;archivio attendibili. Dovremo utilizzare questa password più tardi in questa procedura.

* Passare a [Global Trust Store](http://localhost:4502/libs/granite/security/content/truststore.html).
* Fare clic su &quot;Aggiungi certificato da file CER&quot;. Aggiungete il certificato IdP fornito da OKTA e fate clic su Invia.

   >[!NOTE]
   >
   >Non mappare il certificato ad alcun utente

Al momento dell&#39;aggiunta del certificato all&#39;archivio di affidabilità, è necessario ottenere l&#39;alias del certificato come mostrato nella schermata seguente. Il nome alias potrebbe essere diverso nel caso in cui lo si tratti.

![Certificate-alias](assets/cert-alias.PNG)

**Prendete nota dell&#39;alias del certificato. Ne avrete bisogno nei passaggi successivi.**

### Configurare il gestore di autenticazione SAML

Passare a [configMgr](http://localhost:4502/system/console/configMgr).
Cercate e aprite &quot; Adobe Gestore autenticazione SAML 2.0&quot;.
Fornire le seguenti proprietà come specificato di seguito
Di seguito sono riportate le proprietà chiave da specificare:

* **percorso** : percorso in cui verrà attivato il gestore di autenticazione
* **Url** IdP:Questo è il tuo URL IdP fornito da OKTA
* **Alias** del certificato IDP: questo alias ricevuto quando si aggiunge il certificato IdP AEM store attendibile
* **ID** entità provider di servizi:nome del server AEM
* **Password dell&#39;archivio** chiavi:Password dell&#39;archivio attendibili utilizzata
* **Reindirizzamento** predefinito:URL a cui reindirizzare in caso di autenticazione riuscita
* **Attributo** ID utente:uid
* **Usa crittografia**:false
* **Creare automaticamente utenti** CRX:true
* **Aggiungi a gruppi**:true
* **Gruppi** predefiniti:oktausers(Questo è il gruppo al quale verranno aggiunti gli utenti. Potete fornire qualsiasi gruppo esistente all&#39;interno di AEM)
* **NamedIDPolicy**: Specifica i vincoli relativi all&#39;identificatore del nome da utilizzare per rappresentare l&#39;oggetto richiesto. Copiare e incollare la seguente stringa evidenziata **urn:oasis:names:tc:SAML:2.0:nameidformat:emailAddress**
* **Attributi**  sincronizzati: si tratta degli attributi che vengono memorizzati dall&#39;asserzione SAML nel profilo AEM

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### Configurare il filtro di riferimento Apache Sling

Passare a [configMgr](http://localhost:4502/system/console/configMgr).
Cercate e aprite &quot;Apache Sling Referrer Filter&quot;.Impostate le seguenti proprietà come specificato di seguito:

* **Consenti valori vuoti**: true
* **Consenti host**: Nome host dell&#39;IdP (sarà diverso nel tuo caso)
* **Consenti host** regexp: Nome host dell&#39;IdP (sarà diverso nel tuo caso) La schermata delle proprietà del referente del filtro Sling Referrer

![referrer-filter](assets/sling-referrer-filter.PNG)

#### Configurare la registrazione DEBUG per l&#39;integrazione OKTA

Quando si configura l&#39;integrazione OKTA su AEM, può essere utile rivedere i registri DEBUG per AEM gestore autenticazione SAML. Per impostare il livello di registro su DEBUG, create una nuova configurazione Sling Logger tramite la console Web AEM OSGi.

Ricordate di rimuovere o disabilitare questo logger su Stage e Produzione per ridurre il rumore del registro.

Quando si configura l&#39;integrazione OKTA su AEM, può essere utile esaminare i registri DEBUG per AEM gestore autenticazione SAML. Per impostare il livello di registro su DEBUG, create una nuova configurazione Sling Logger tramite la console Web AEM OSGi.
**Ricordate di rimuovere o disabilitare questo logger su Stage e Produzione per ridurre il rumore del registro.**
* Passa a [configMgr](http://localhost:4502/system/console/configMgr)

* Cercare e aprire &quot;Apache Sling Logging Logging Configuration&quot;
* Create un logger con la configurazione seguente:
   * **Livello** di registro: Debug
   * **File** di registro: logs/saml.log
   * **Logger**: com.adobe.granite.auth.saml
* Fate clic su Salva per salvare le impostazioni



#### Verificare la configurazione OKTA

Disconnessione dall’istanza AEM. Prova ad accedere al collegamento. Dovresti vedere l&#39;SSO OKTA in azione.
