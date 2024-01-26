---
title: Configurazione di OKTA con AEM
description: Comprendere le varie impostazioni di configurazione per l'utilizzo del Single Sign-On con l'okta
feature: Adaptive Forms
version: 6.5
topic: Administration
role: Admin
level: Experienced
exl-id: 85c9b51e-92bb-4376-8684-57c9c3204b2f
last-substantial-update: 2021-06-09T00:00:00Z
duration: 193
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '733'
ht-degree: 0%

---

# Autenticazione per l’authoring AEM con OKTA

Il primo passaggio consiste nel configurare l’app sul portale OKTA. Una volta approvata l’app dall’amministratore OKTA, potrai accedere al certificato IdP e all’URL di accesso single sign-on. Di seguito sono riportate le impostazioni utilizzate in genere per la registrazione di una nuova applicazione.

* **Nome applicazione:** Questo è il nome della tua applicazione. Assicurati di assegnare un nome univoco all’applicazione.
* **Destinatario SAML:** Dopo l’autenticazione da OKTA, questo è l’URL che verrebbe visualizzato sull’istanza AEM con la risposta SAML. Il gestore di autenticazione SAML intercetta normalmente tutti gli URL con / saml_login, ma sarebbe preferibile aggiungerli dopo la radice dell’applicazione.
* **Pubblico SAML**: questo è l’URL di dominio dell’applicazione. Non utilizzare il protocollo (http o https) nell’URL del dominio.
* **ID nome SAML:** Seleziona E-mail dall’elenco a discesa.
* **Ambiente**: scegli l’ambiente appropriato.
* **Attributi**: questi sono gli attributi ottenuti dall’utente nella risposta SAML. Specificali in base alle tue esigenze.


![applicazione okta](assets/okta-app-settings-blurred.PNG)


## Aggiungere il certificato OKTA (IdP) all’archivio fonti attendibili AEM

Poiché le asserzioni SAML sono crittografate, è necessario aggiungere il certificato IdP (OKTA) all’archivio fonti attendibili dell’AEM per consentire una comunicazione sicura tra OKTA e AEM.
[Inizializza archivio fonti attendibili](http://localhost:4502/libs/granite/security/content/truststore.html), se non già inizializzato.
Memorizza la password dell&#39;archivio fonti attendibili. La password dovrà essere utilizzata più avanti in questo processo.

* Accedi a [Archivio attendibile globale](http://localhost:4502/libs/granite/security/content/truststore.html).
* Fare clic su &quot;Aggiungi certificato da file CER&quot;. Aggiungi il certificato IdP fornito da OKTA e fai clic su invia.

  >[!NOTE]
  >
  >Non mappare il certificato ad alcun utente

Quando si aggiunge il certificato all’archivio fonti attendibili, si deve ottenere l’alias del certificato come mostrato nella schermata seguente. Il nome dell&#39;alias potrebbe essere diverso nel tuo caso.

![Alias del certificato](assets/cert-alias.PNG)

**Prendi nota dell’alias del certificato. Questo è necessario nei passaggi successivi.**

### Configura gestore autenticazione SAML

Accedi a [configMgr](http://localhost:4502/system/console/configMgr).
Cerca e apri &quot;Adobe Granite SAML 2.0 Authentication Handler&quot;.
Specifica le seguenti proprietà come specificato di seguito. Di seguito sono elencate le proprietà chiave da specificare:

* **percorso** : percorso in cui viene attivato il gestore di autenticazione
* **Url IdP**: questo è l’URL del tuo IdP fornito da OKTA
* **Alias certificato IDP**: alias ottenuto quando si è aggiunto il certificato IdP all&#39;archivio fonti attendibili AEM
* **ID entità provider di servizi**:Questo è il nome del server AEM
* **Password dell’archivio chiavi**: password dell&#39;archivio fonti attendibili utilizzata
* **Reindirizzamento predefinito**: URL a cui reindirizzare in caso di autenticazione riuscita
* **Attributo UserID**:uid
* **Usa crittografia**:false
* **Crea automaticamente utenti CRX**:true
* **Aggiungi a gruppi**:true
* **Gruppi predefiniti**:oktausers(Questo è il gruppo a cui vengono aggiunti gli utenti. È possibile fornire qualsiasi gruppo esistente all’interno di AEM)
* **NamedIDPolicy**: specifica i vincoli sull&#39;identificatore del nome da utilizzare per rappresentare il soggetto richiesto. Copia e incolla la seguente stringa evidenziata **urn:oasis:nomi:tc:SAML:2.0:nameidformat:emailAddress**
* **Attributi sincronizzati** : attributi memorizzati dall’asserzione SAML nel profilo AEM

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### Configurare il filtro Referrer Apache Sling

Accedi a [configMgr](http://localhost:4502/system/console/configMgr).
Cerca e apri &quot;Apache Sling Referrer Filter&quot;.Imposta le seguenti proprietà come specificato di seguito:

* **Consenti vuoto**: false
* **Consenti host**: nome host dell’IdP (nel tuo caso è diverso)
* **Consenti host Regexp**: nome host dell’IdP (nel tuo caso è diverso) Nella schermata delle proprietà del referente del filtro Sling

![referrer-filter](assets/okta-referrer.png)

#### Configurare la registrazione DEBUG per l’integrazione OKTA

Quando si imposta l’integrazione OKTA su AEM, può essere utile rivedere i registri DEBUG per il gestore di autenticazione SAML dell’AEM. Per impostare il livello di registro su DEBUG, crea una nuova configurazione Sling Logger tramite la console web OSGi dell’AEM.

Ricorda di rimuovere o disattivare il logger in Stage e Production per ridurre il disturbo del registro.

Quando si imposta l’integrazione OKTA su AEM, può essere utile rivedere i registri DEBUG per il gestore di autenticazione SAML dell’AEM. Per impostare il livello di registro su DEBUG, crea una nuova configurazione Sling Logger tramite la console web OSGi dell’AEM.
**Ricorda di rimuovere o disattivare il logger in Stage e Production per ridurre il disturbo del registro.**
* Accedi a [configMgr](http://localhost:4502/system/console/configMgr)

* Cerca e apri &quot;Configurazione logger registrazione Sling Apache&quot;
* Crea un logger con la seguente configurazione:
   * **Livello registro**: Debug
   * **File di registro**: logs/saml.log
   * **Logger**: com.adobe.granite.auth.saml
* Fai clic su Salva per salvare le impostazioni

#### Verifica la configurazione OKTA

Esci dall’istanza AEM. Prova ad accedere al collegamento. Dovresti vedere l’SSO OKTA in azione.
