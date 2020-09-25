---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: 'AEM utilizza coppie di chiavi pubblica/privata per comunicare in modo sicuro con  I/O Adobe e altri servizi Web. Questa breve esercitazione illustra come è possibile generare chiavi e keystore compatibili utilizzando lo strumento della riga di comando openssl che funziona sia con AEM che con  I/O Adobe. '
version: 6.4, 6.5
feature: authentication
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
translation-type: tm+mt
source-git-commit: c85a59a8bd180d5affe2a5bf5939dabfb2776d73
workflow-type: tm+mt
source-wordcount: '728'
ht-degree: 0%

---


# Configurazione di chiavi pubbliche e private da utilizzare con  Adobe I/O

AEM utilizza coppie di chiavi pubblica/privata per comunicare in modo sicuro con  I/O Adobe e altri servizi Web. Questa breve esercitazione illustra come è possibile generare chiavi e keystore compatibili con lo strumento della [!DNL openssl] riga di comando che funziona sia con AEM che con  I/O Adobe.

>[!CAUTION]
>
>Questa guida crea chiavi autofirmate utili per lo sviluppo e l&#39;utilizzo in ambienti più bassi. Negli scenari di produzione, le chiavi vengono generalmente generate e gestite dal team di sicurezza IT di un&#39;organizzazione.

## Genera coppia chiave pubblica/privata {#generate-the-public-private-key-pair}

Il [comando](https://www.openssl.org/docs/man1.0.2/man1/openssl.html) dello strumento della riga di comando [[!DNL req] [!DNL openssl]](https://www.openssl.org/docs/man1.0.2/man1/req.html) può essere utilizzato per generare una coppia di chiavi compatibile con  Adobe I/O e Adobe Experience Manager.

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

Per completare il [!DNL openssl generate] comando, fornite le informazioni sul certificato quando richiesto.  I/O Adobe e AEM non si preoccupano di quali siano questi valori, ma devono allinearsi e descrivere la chiave.

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

## Aggiungi coppia di chiavi a un nuovo keystore {#add-key-pair-to-a-new-keystore}

Le coppie di chiavi possono essere aggiunte a un nuovo [!DNL PKCS12] archivio chiavi. Come parte del [[!DNL openssl]'s [!DNL pcks12] comando,](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html) vengono definiti il nome dell&#39;archivio di chiavi (via `-  caname`), il nome della chiave (via `-name`) e la password dell&#39;archivio di chiavi (via `-  passout`).

Questi valori sono necessari per caricare l&#39;archivio di chiavi e le chiavi in AEM.

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

L&#39;output di questo comando è un `keystore.p12` file.

>[!NOTE]
>
>I valori dei parametri di **[!DNL my-keystore]**, **[!DNL my-key]** e **[!DNL my-password]** devono essere sostituiti da valori personalizzati.

## Verificare il contenuto dell&#39;archivio di chiavi {#verify-the-keystore-contents}

Lo strumento [[!DNL keytool] della riga di](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818) comando Java fornisce visibilità in un archivio di chiavi per verificare che le chiavi siano state caricate correttamente nel file dell&#39;archivio di chiavi ([!DNL keystore.p12]).

```shell
$ keytool -keystore keystore.p12 -list

Enter keystore password: my-password

Keystore type: jks
Keystore provider: SUN

Your keystore contains 1 entry

my-key, Feb 5, 2019, PrivateKeyEntry,
Certificate fingerprint (SHA1): 7C:6C:25:BD:52:D3:3B:29:83:FD:A2:93:A8:53:91:6A:25:1F:2D:52
```

![Verifica archivio chiavi in  I/O Adobe](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

## Aggiunta di un archivio di chiavi a AEM {#adding-the-keystore-to-aem}

AEM utilizza la chiave **** privata generata per comunicare in modo sicuro con  Adobe I/O e altri servizi Web. Affinché la chiave privata possa essere AEM, deve essere installata in un archivio di chiavi AEM utente.

Andate a **AEM >[!UICONTROL Strumenti]>[!UICONTROL Protezione]>[!UICONTROL Utenti]** e **modificate l&#39;utente** a cui deve essere associata la chiave privata.

### Creare un AEM keystore {#create-an-aem-keystore}

![Crea KeyStore in AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)*AEM >[!UICONTROL Strumenti]>[!UICONTROL Protezione]>[!UICONTROL Utenti]> Modifica utente*

Se viene richiesto di creare un archivio di chiavi, eseguire questa operazione. Questo keystore esiste solo in AEM e NON è il keystore creato tramite openssl. La password può essere qualsiasi cosa e non deve essere la stessa utilizzata nel [!DNL openssl] comando.

### Installare la chiave privata tramite l&#39;archivio di chiavi {#install-the-private-key-via-the-keystore}

![Aggiungi chiave privata in AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)*[!UICONTROL Utente]>[!UICONTROL Keystore]>[!UICONTROL Aggiungi chiave privata da keystore]*

Nella console dell&#39;utente, fare clic su **[!UICONTROL Aggiungi chiave privata dal file]** KeyStore e aggiungere le seguenti informazioni:

* **[!UICONTROL Nuovo alias]**: l&#39;alias della chiave in AEM. Può essere qualsiasi cosa e non deve corrispondere al nome dell&#39;archivio di chiavi creato con il comando openssl.
* **[!UICONTROL File]** archivio chiavi: l&#39;output del comando openssl pkcs12 (keystore.p12)
* **[!UICONTROL Alias]** chiave privata: La password impostata nel comando openssl pkcs12 tramite `-  passout` argomento.

* **[!UICONTROL Password]** chiave privata: La password impostata nel comando openssl pkcs12 tramite `-  passout` argomento.

### Verifica che la chiave privata sia caricata nell&#39;archivio chiavi AEM {#verify-the-private-key-is-loaded-into-the-aem-keystore}

![Verifica chiave privata in AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)*[!UICONTROL utente]>[!UICONTROL Keystore]*

Quando la chiave privata viene caricata correttamente dall&#39;archivio di chiavi fornito nell&#39;archivio di chiavi AEM, i metadati della chiave privata vengono visualizzati nella console dell&#39;archivio di chiavi dell&#39;utente.

## Aggiunta della chiave pubblica a  I/O Adobe {#adding-the-public-key-to-adobe-i-o}

La chiave pubblica corrispondente deve essere caricata  Adobe I/O per consentire all&#39;utente del servizio AEM, che ha la chiave pubblica corrispondente privata per comunicare in modo sicuro.

### Creare una nuova integrazione di I/O  Adobe {#create-a-adobe-i-o-new-integration}

![Creare una nuova integrazione di I/O  Adobe](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL Crea integrazione]](https://console.adobe.io/)I/O  Adobe >[!UICONTROL Nuova integrazione]*

La creazione di una nuova integrazione in  I/O Adobe richiede il caricamento di un certificato pubblico. Caricate il **certificate.crt** generato dal `openssl req` comando.

### Verificare che le chiavi pubbliche siano caricate nell&#39;I/O  Adobe {#verify-the-public-keys-are-loaded-in-adobe-i-o}

![Verifica chiavi pubbliche in  I/O Adobe](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

Le chiavi pubbliche installate e le relative date di scadenza sono elencate nella console [!UICONTROL Integrazioni]  Adobe I/O. È possibile aggiungere più chiavi pubbliche tramite il pulsante **[!UICONTROL Aggiungi una chiave]** pubblica.

Ora AEM tenere la chiave privata e l&#39;integrazione I/O del Adobe  detiene la chiave pubblica corrispondente, consentendo AEM comunicare in modo sicuro con  Adobe I/O.
