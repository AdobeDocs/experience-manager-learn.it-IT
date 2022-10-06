---
title: Configurazione di OKTA con AEM
description: Comprendere varie impostazioni di configurazione per l’utilizzo del single sign-on con okta
feature: Adaptive Forms
version: 6.5
topic: Administration
role: Admin
level: Experienced
exl-id: 85c9b51e-92bb-4376-8684-57c9c3204b2f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '756'
ht-degree: 1%

---

# Autenticazione in AEM Author tramite OKTA

Il primo passo è quello di configurare la tua app sul portale OKTA. Una volta approvata l’app dal tuo amministratore OKTA avrai accesso al certificato IdP e all’URL di accesso single sign on. Di seguito sono riportate le impostazioni tipicamente utilizzate per la registrazione di una nuova applicazione.

* **Nome applicazione:** Questo è il nome dell&#39;applicazione. Assicurati di assegnare un nome univoco all&#39;applicazione.
* **Destinatario SAML:** Dopo l’autenticazione da OKTA, questo è l’URL che verrebbe colpito sulla tua istanza AEM con la risposta SAML. Il gestore di autenticazione SAML normalmente intercetta tutti gli URL con / saml_login, ma sarebbe preferibile aggiungerlo dopo la radice dell&#39;applicazione.
* **Pubblico SAML**: Questo è l&#39;URL di dominio dell&#39;applicazione. Non utilizzare il protocollo (http o https) nell&#39;URL del dominio.
* **ID nome SAML:** Seleziona E-mail dall’elenco a discesa.
* **Ambiente**: Scegli l’ambiente appropriato.
* **Attributi**: Questi sono gli attributi che ottieni sull&#39;utente nella risposta SAML. Specificali in base alle tue esigenze.


![okta-application](assets/okta-app-settings-blurred.PNG)


## Aggiungi il certificato OKTA (IdP) all’archivio AEM trust

Poiché le asserzioni SAML sono crittografate, è necessario aggiungere il certificato IdP (OKTA) all’archivio AEM trust per consentire una comunicazione sicura tra OKTA e AEM.
[Inizializza archivio attendibilità](http://localhost:4502/libs/granite/security/content/truststore.html), se non inizializzato già.
Ricordare la password dell&#39;archivio di attendibilità. Sarà necessario utilizzare questa password in un secondo momento in questa procedura.

* Passa a [Archivio fonti attendibili globale](http://localhost:4502/libs/granite/security/content/truststore.html).
* Fai clic su &quot;Aggiungi certificato dal file CER&quot;. Aggiungi il certificato IdP fornito da OKTA e fai clic su invia.

   >[!NOTE]
   >
   >Non mappare il certificato ad alcun utente

Al momento dell’aggiunta del certificato all’archivio attendibili, devi ottenere l’alias del certificato come mostrato nella schermata sottostante. Il nome dell’alias potrebbe essere diverso nel tuo caso.

![Alias certificato](assets/cert-alias.PNG)

**Prendi nota dell’alias del certificato. Ne hai bisogno nei passaggi successivi.**

### Configura il gestore di autenticazione SAML

Passa a [configMgr](http://localhost:4502/system/console/configMgr).
Cerca e apri &quot;Adobe Granite SAML 2.0 Authentication Handler&quot;.
Fornire le seguenti proprietà come specificato di seguito Le seguenti sono le proprietà chiave che devono essere specificate:

* **path** - Questo è il percorso in cui viene attivato il gestore di autenticazione
* **Url IdP**:Questo è il tuo URL IdP fornito da OKTA
* **Alias certificato IDP**:Questo alias ricevuto quando hai aggiunto il certificato IdP nell’archivio AEM trust
* **ID entità fornitore di servizi**:Questo è il nome del server AEM
* **Password dell&#39;archivio chiavi**:Password dell&#39;archivio attendibili utilizzata
* **Reindirizzamento predefinito**:URL a cui reindirizzare in caso di autenticazione riuscita
* **Attributo UserID**:uid
* **Usa crittografia**:false
* **Creazione automatica utenti CRX**:true
* **Aggiungi ai gruppi**:true
* **Gruppi predefiniti**:oktausers(Questo è il gruppo a cui vengono aggiunti gli utenti. Puoi fornire qualsiasi gruppo esistente all&#39;interno di AEM)
* **NamedIDPolicy**: Specifica i vincoli relativi all&#39;identificatore del nome da utilizzare per rappresentare l&#39;oggetto richiesto. Copia e incolla la seguente stringa evidenziata **abbandono:oasis:nomi:tc:SAML:2.0:nameidformat:emailAddress**
* **Attributi sincronizzati** - Questi sono gli attributi che vengono memorizzati dall&#39;asserzione SAML nel profilo AEM

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### Configurare il filtro di riferimento Apache Sling

Passa a [configMgr](http://localhost:4502/system/console/configMgr).
Cerca e apri &quot;Apache Sling Referrer Filter&quot;.Imposta le seguenti proprietà come specificato di seguito:

* **Consenti vuoto**: false
* **Consenti host**: Nome host dell&#39;IdP (nel tuo caso è diverso)
* **Consenti host regexp**: Nome host dell&#39;IdP (questo è diverso nel tuo caso) Schermata delle proprietà del referente del filtro Sling Referrer

![referrer-filter](assets/okta-referrer.png)

#### Configurare la registrazione DEBUG per l’integrazione OKTA

Quando si imposta l&#39;integrazione OKTA su AEM, può essere utile rivedere i registri DEBUG per AEM gestore di autenticazione SAML. Per impostare il livello di registro su DEBUG, crea una nuova configurazione Sling Logger tramite la AEM OSGi Web Console.

Ricorda di rimuovere o disabilitare questo logger su Stage e Production per ridurre il rumore del log.

Quando si imposta l&#39;integrazione OKTA su AEM, può essere utile rivedere i registri DEBUG per AEM gestore di autenticazione SAML. Per impostare il livello di registro su DEBUG, crea una nuova configurazione Sling Logger tramite la AEM OSGi Web Console.
**Ricorda di rimuovere o disabilitare questo logger su Stage e Production per ridurre il rumore del log.**
* Passa a [configMgr](http://localhost:4502/system/console/configMgr)

* Cerca e apri &quot;Apache Sling Logging Logger Configuration&quot; (Configurazione logger di registrazione Apache Sling)
* Crea un logger con la seguente configurazione:
   * **Livello di log**: Debug
   * **File di log**: logs/saml.log
   * **Registratore**: com.adobe.granite.auth.saml
* Fai clic su salva per salvare le impostazioni

#### Verifica la configurazione OKTA

Disconnessione dall&#39;istanza AEM. Prova ad accedere al collegamento. Dovresti vedere OKTA SSO in azione.
