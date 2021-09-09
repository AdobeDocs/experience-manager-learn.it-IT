---
title: Configurazione di OKTA con AEM
description: Comprendere varie impostazioni di configurazione per l’utilizzo del single sign-on con okta
feature: Adaptive Forms
version: 6.5
topic: Administration
role: Admin
level: Experienced
source-git-commit: 3109d406ed4788ab492a148d4eac94f7e5ad9f2d
workflow-type: tm+mt
source-wordcount: '759'
ht-degree: 0%

---


# Autenticazione in AEM Author tramite OKTA

Il primo passo è quello di configurare la tua app sul portale OKTA. Una volta approvata l’app dal tuo amministratore OKTA avrai accesso al certificato IdP e all’URL di accesso single sign on. Di seguito sono riportate le impostazioni tipicamente utilizzate per la registrazione di una nuova applicazione.

* **Nome applicazione:** questo è il nome dell&#39;applicazione. Assicurati di assegnare un nome univoco all&#39;applicazione.
* **Destinatario SAML:** dopo l’autenticazione da OKTA, questo è l’URL che verrebbe colpito sulla tua istanza AEM con la risposta SAML. Il gestore di autenticazione SAML normalmente intercetta tutti gli URL con / saml_login, ma sarebbe preferibile aggiungerlo dopo la radice dell&#39;applicazione.
* **Pubblico** SAML: Questo è l&#39;URL di dominio dell&#39;applicazione. Non utilizzare il protocollo (http o https) nell&#39;URL del dominio.
* **ID nome SAML:** seleziona E-mail dall’elenco a discesa.
* **Ambiente**: Scegli l’ambiente appropriato.
* **Attributi**: Questi sono gli attributi che ottieni sull&#39;utente nella risposta SAML. Specificali in base alle tue esigenze.


![okta-application](assets/okta-app-settings-blurred.PNG)


## Aggiungi il certificato OKTA (IdP) all’archivio AEM trust

Poiché le asserzioni SAML sono crittografate, è necessario aggiungere il certificato IdP (OKTA) all’archivio AEM trust per consentire una comunicazione sicura tra OKTA e AEM.
[Inizializza l&#39;archivio](http://localhost:4502/libs/granite/security/content/truststore.html) di attendibilità, se non è già inizializzato.
Ricordare la password dell&#39;archivio di attendibilità. Sarà necessario utilizzare questa password in un secondo momento in questa procedura.

* Passa a [Global Trust Store](http://localhost:4502/libs/granite/security/content/truststore.html).
* Fai clic su &quot;Aggiungi certificato dal file CER&quot;. Aggiungi il certificato IdP fornito da OKTA e fai clic su invia.

   >[!NOTE]
   >
   >Non mappare il certificato ad alcun utente

Al momento dell’aggiunta del certificato all’archivio attendibili, devi ottenere l’alias del certificato come mostrato nella schermata sottostante. Il nome dell’alias potrebbe essere diverso nel tuo caso.

![Alias certificato](assets/cert-alias.PNG)

**Prendi nota dell’alias del certificato. Questo è necessario nei passaggi successivi.**

### Configura il gestore di autenticazione SAML

Passa a [configMgr](http://localhost:4502/system/console/configMgr).
Cerca e apri &quot;Adobe Granite SAML 2.0 Authentication Handler&quot;.
Fornisci le seguenti proprietà come specificato di seguito
Di seguito sono riportate le proprietà chiave da specificare:

* **path**  - Questo è il percorso in cui verrà attivato il gestore di autenticazione
* **Url** IdP: questo è il tuo URL IdP fornito da OKTA
* **Alias** del certificato IDP: alias ricevuto quando hai aggiunto il certificato IdP nell&#39;archivio AEM trust
* **ID entità provider di servizi**: nome del server AEM
* **Password dell&#39;archivio** chiavi: password dell&#39;archivio attendibilità utilizzata
* **Reindirizzamento** predefinito:URL a cui reindirizzare in caso di autenticazione riuscita
* **Attributo** UserID:uid
* **Usa crittografia**: false
* **Creare automaticamente utenti** CRX:true
* **Aggiungi ai gruppi**:true
* **Gruppi** predefiniti:oktausers(Questo è il gruppo a cui verranno aggiunti gli utenti. Puoi fornire qualsiasi gruppo esistente all&#39;interno di AEM)
* **NamedIDPolicy**: Specifica i vincoli relativi all&#39;identificatore del nome da utilizzare per rappresentare l&#39;oggetto richiesto. Copia e incolla la seguente stringa evidenziata **urn:oasis:names:tc:SAML:2.0:nameidformat:emailAddress**
* **Attributi sincronizzati** : si tratta degli attributi che vengono memorizzati dall&#39;asserzione SAML nel profilo AEM

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### Configurare il filtro di riferimento Apache Sling

Passa a [configMgr](http://localhost:4502/system/console/configMgr).
Cerca e apri &quot;Apache Sling Referrer Filter&quot;.Imposta le seguenti proprietà come specificato di seguito:

* **Consenti valori vuoti**: false
* **Consenti host**: Nome host dell&#39;IdP (nel tuo caso sarà diverso)
* **Consenti host** regexp: Nome host dell&#39;IdP(Sarà diverso nel tuo caso) La schermata delle proprietà del referente del filtro Sling Referrer

![referrer-filter](assets/okta-referrer.png)

#### Configurare la registrazione DEBUG per l’integrazione OKTA

Quando si imposta l&#39;integrazione OKTA su AEM, può essere utile rivedere i registri DEBUG per AEM gestore di autenticazione SAML. Per impostare il livello di registro su DEBUG, crea una nuova configurazione Sling Logger tramite la AEM OSGi Web Console.

Ricorda di rimuovere o disabilitare questo logger su Stage e Production per ridurre il rumore del log.

Quando si imposta l&#39;integrazione OKTA su AEM, può essere utile rivedere i registri DEBUG per AEM gestore di autenticazione SAML. Per impostare il livello di registro su DEBUG, crea una nuova configurazione Sling Logger tramite la AEM OSGi Web Console.
**Ricorda di rimuovere o disabilitare questo logger su Stage e Production per ridurre il rumore del log.**
* Passa a [configMgr](http://localhost:4502/system/console/configMgr)

* Cerca e apri &quot;Apache Sling Logging Logger Configuration&quot; (Configurazione logger di registrazione Apache Sling)
* Crea un logger con la seguente configurazione:
   * **Livello** log: Debug
   * **File** di log: logs/saml.log
   * **Logger**: com.adobe.granite.auth.saml
* Fai clic su salva per salvare le impostazioni



#### Verifica la configurazione OKTA

Disconnessione dall&#39;istanza AEM. Prova ad accedere al collegamento. Dovresti vedere OKTA SSO in azione.
