---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: 'AEM utilizza coppie di chiavi pubblica/privata per comunicare in modo sicuro con Adobe I/O e altri servizi web. Questa breve esercitazione illustra come è possibile generare chiavi e keystore compatibili utilizzando lo strumento a riga di comando openssl che funziona sia con AEM che con Adobe I/O. '
version: 6.4, 6.5
feature: Utenti e gruppi
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
topic: Sviluppo
role: Developer
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '772'
ht-degree: 0%

---


# Configurare le chiavi pubbliche e private da utilizzare con Adobe I/O

AEM utilizza coppie di chiavi pubblica/privata per comunicare in modo sicuro con Adobe I/O e altri servizi web. Questa breve esercitazione illustra come è possibile generare chiavi e keystore compatibili utilizzando lo strumento della riga di comando [!DNL openssl] che funziona sia con AEM che con Adobe I/O.

>[!CAUTION]
>
>Questa guida crea chiavi autofirmate utili per lo sviluppo e l’utilizzo in ambienti più bassi. In scenari di produzione, le chiavi vengono generalmente generate e gestite dal team di sicurezza IT di un&#39;organizzazione.

## Generare la coppia di chiavi pubblica/privata {#generate-the-public-private-key-pair}

Il comando [[!DNL openssl]](https://www.openssl.org/docs/man1.0.2/man1/openssl.html) dello strumento della riga di comando [[!DNL req] ](https://www.openssl.org/docs/man1.0.2/man1/req.html) può essere utilizzato per generare una coppia di chiavi compatibile con Adobe I/O e Adobe Experience Manager.

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

Per completare il comando [!DNL openssl generate], fornisci le informazioni sul certificato quando richiesto. Adobe I/O e AEM non si interessano a quali sono questi valori, tuttavia dovrebbero allinearsi e descrivere la tua chiave.

```
Generating a 2048 bit RSA private key
...........................................................+++
...+++
writing new private key to 'private.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) []:US
State or Province Name (full name) []:CA
Locality Name (eg, city) []:San Jose
Organization Name (eg, company) []:Example Co
Organizational Unit Name (eg, section) []:Digital Marketing
Common Name (eg, fully qualified host name) []:com.example
Email Address []:me@example.com
```

## Aggiungi una coppia di chiavi a un nuovo keystore {#add-key-pair-to-a-new-keystore}

È possibile aggiungere coppie di chiavi a un nuovo [!DNL PKCS12] keystore. Come parte del [[!DNL openssl]'s [!DNL pcks12] comando,](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html) il nome del keystore (tramite `-  caname`), il nome della chiave (tramite `-name`) e la password del keystore (tramite `-  passout`) sono definiti.

Questi valori sono necessari per caricare il keystore e le chiavi in AEM.

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

L&#39;output di questo comando è un file `keystore.p12`.

>[!NOTE]
>
>I valori dei parametri di **[!DNL my-keystore]**, **[!DNL my-key]** e **[!DNL my-password]** devono essere sostituiti dai valori personalizzati.

## Verifica il contenuto del keystore {#verify-the-keystore-contents}

Lo strumento Java [[!DNL keytool] riga di comando](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818) fornisce visibilità in un keystore per garantire che le chiavi siano caricate correttamente nel file keystore ([!DNL keystore.p12]).

```shell
$ keytool -keystore keystore.p12 -list

Enter keystore password: my-password

Keystore type: jks
Keystore provider: SUN

Your keystore contains 1 entry

my-key, Feb 5, 2019, PrivateKeyEntry,
Certificate fingerprint (SHA1): 7C:6C:25:BD:52:D3:3B:29:83:FD:A2:93:A8:53:91:6A:25:1F:2D:52
```

![Verificare l’archivio chiavi in Adobe I/O](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

## Aggiunta del keystore a AEM {#adding-the-keystore-to-aem}

AEM utilizza la **chiave privata** generata per comunicare in modo sicuro con Adobe I/O e altri servizi web. Affinché la chiave privata sia accessibile a AEM, deve essere installata in un keystore AEM utente.

Passa a **AEM > [!UICONTROL Strumenti] > [!UICONTROL Sicurezza] > [!UICONTROL Utenti]** e **modifica l&#39;utente** a cui deve essere associata la chiave privata.

### Creare un AEM keystore {#create-an-aem-keystore}

![Crea KeyStore in ](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)
*AEM >  [!UICONTROL Strumenti]  >  [!UICONTROL Sicurezza]  >  [!UICONTROL Utenti]  > Modifica utente*

Se viene richiesto di creare un archivio chiavi, eseguire questa operazione. Questo keystore esisterà solo in AEM e NON è il keystore creato tramite openssl. La password può essere qualsiasi cosa e non deve essere la stessa utilizzata nel comando [!DNL openssl] .

### Installare la chiave privata tramite il keystore {#install-the-private-key-via-the-keystore}

![Aggiungi chiave privata in ](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)
*[!UICONTROL AEMUser]  >  [!UICONTROL Registro chiavi] >  [!UICONTROL Aggiungi chiave privata da keystore]*

Nella console dell&#39;archivio chiavi dell&#39;utente, fai clic su **[!UICONTROL Aggiungi chiave privata dal file KeyStore]** e aggiungi le seguenti informazioni:

* **[!UICONTROL Nuovo alias]**: l&#39;alias della chiave in AEM. Questo può essere qualsiasi cosa e non deve corrispondere al nome del keystore creato con il comando openssl.
* **[!UICONTROL File]** KeyStore: l&#39;output del comando openssl pkcs12 (keystore.p12)
* **[!UICONTROL Password]** file KeyStore: La password impostata nel comando openssl pkcs12 tramite  `-passout` argomento.
* **[!UICONTROL Alias]** chiave privata: Il valore fornito all&#39; `-name` argomento nel comando openssl pkcs12 di cui sopra (cioè  `my-key`).
* **[!UICONTROL Password]** chiave privata: La password impostata nel comando openssl pkcs12 tramite  `-passout` argomento.

>[!CAUTION]
>
>La password del file KeyStore e la password della chiave privata sono uguali per entrambi gli input. Se si immette una password non corrispondente, la chiave non verrà importata.

### Verifica che la chiave privata sia caricata nell&#39;archivio chiavi AEM {#verify-the-private-key-is-loaded-into-the-aem-keystore}

![Verificare la chiave privata in ](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)
*[!UICONTROL AEMUser]  >  [!UICONTROL Keystore]*

Quando la chiave privata viene caricata correttamente dall&#39;archivio chiavi fornito nell&#39;archivio chiavi AEM, i metadati della chiave privata vengono visualizzati nella console dell&#39;archivio chiavi dell&#39;utente.

## Aggiunta della chiave pubblica all’Adobe I/O {#adding-the-public-key-to-adobe-i-o}

La chiave pubblica corrispondente deve essere caricata in Adobe I/O per consentire all’utente del servizio AEM, che ha la chiave pubblica corrispondente privata di comunicare in modo sicuro.

### Creare un Adobe I/O di nuova integrazione {#create-a-adobe-i-o-new-integration}

![Creare un Adobe I/O di nuova integrazione](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL Creare un’integrazione]](https://console.adobe.io/)  di Adobe I/O >  [!UICONTROL Nuova integrazione]*

La creazione di una nuova integrazione in Adobe I/O richiede il caricamento di un certificato pubblico. Carica il **certificate.crt** generato dal comando `openssl req`.

### Verifica che le chiavi pubbliche siano caricate in Adobe I/O {#verify-the-public-keys-are-loaded-in-adobe-i-o}

![Verifica chiavi pubbliche nell’Adobe I/O](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

Le chiavi pubbliche installate e le relative date di scadenza sono elencate ad Adobe I/O nella console [!UICONTROL Integrazioni] . È possibile aggiungere più chiavi pubbliche tramite il pulsante **[!UICONTROL Aggiungi una chiave pubblica]** .

Ora AEM tenere la chiave privata e l&#39;integrazione Adobe I/O detiene la chiave pubblica corrispondente, consentendo AEM comunicare in modo sicuro con l&#39;Adobe I/O.
