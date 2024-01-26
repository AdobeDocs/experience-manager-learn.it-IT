---
title: Utilizzare la procedura guidata SSL nell’AEM
description: Configurazione SSL guidata di Adobe Experience Manager per semplificare la configurazione di un’istanza AEM da eseguire su HTTPS.
version: 6.5, Cloud Service
jira: KT-13839
doc-type: Technical Video
topic: Security
role: Developer
level: Beginner
exl-id: 4e69e115-12a6-4a57-90da-b91e345c6723
last-substantial-update: 2023-08-08T00:00:00Z
duration: 595
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 0%

---

# Utilizzare la procedura guidata SSL nell’AEM

Scopri come impostare SSL in Adobe Experience Manager per eseguirlo su HTTPS utilizzando la procedura guidata SSL integrata.

>[!VIDEO](https://video.tv.adobe.com/v/17993?quality=12&learn=on)


>[!NOTE]
>
>Per gli ambienti gestiti, è consigliabile che il reparto IT fornisca certificati e chiavi attendibili da CA.
>
>I certificati autofirmati devono essere utilizzati solo a scopo di sviluppo.

## Utilizzo della Configurazione guidata SSL

Accedi a __AEM Author > Tools > Security > SSL Configuration (Creazione > Strumenti > Sicurezza > Configurazione SSL)__ e aprire __Configurazione SSL guidata__.

![Configurazione SSL guidata](assets/use-the-ssl-wizard/ssl-config-wizard.png)

### Crea credenziali archivio

Per creare un _Archivio chiavi_ associato al `ssl-service` utente di sistema e un _Archivio fonti attendibili_, utilizza __Memorizza credenziali__ passaggio della procedura guidata.

1. Immettere la password e confermarla per __Archivio chiavi__ associato al `ssl-service` utente di sistema.
1. Immetti la password e conferma la password per il __Archivio fonti attendibili__. Si tratta di un archivio fonti attendibili a livello di sistema e, se è già stato creato, la password immessa viene ignorata.

   ![Configurazione SSL - Memorizza credenziali](assets/use-the-ssl-wizard/store-credentials.png)

### Carica chiave privata e certificato

Per caricare _chiave privata_ e _Certificato SSL_, utilizza __Chiave e certificato__ passaggio della procedura guidata.

In genere, il reparto IT fornisce il certificato e la chiave attendibili della CA, tuttavia è possibile utilizzare il certificato autofirmato per __sviluppo__ e __test__ finalità.

Per creare o scaricare il certificato autofirmato, vedi la [Chiave privata e certificato autofirmati](#self-signed-private-key-and-certificate).

1. Carica __Chiave privata__ nel formato DER (Distinguished Encoding Rules). A differenza di PEM, i file con codifica DER non contengono istruzioni di testo normale come `-----BEGIN CERTIFICATE-----`
1. Carica il associato __Certificato SSL__ nel `.crt` formato.

   ![Configurazione SSL - Chiave privata e certificato](assets/use-the-ssl-wizard/privatekey-and-certificate.png)

### Aggiorna i dettagli del connettore SSL

Per aggiornare _nome host_ e _porta_ utilizzare il __Connettore SSL__ passaggio della procedura guidata.

1. Aggiornare o verificare __Nome host HTTPS__ , deve corrispondere al valore `Common Name (CN)` dal certificato.
1. Aggiornare o verificare __Porta HTTPS__ valore.

   ![Configurazione SSL - Dettagli connettore SSL](assets/use-the-ssl-wizard/ssl-connector-details.png)

### Verificare la configurazione SSL

1. Per verificare l’SSL, fai clic su __Vai all’URL HTTPS__ pulsante.
1. Se utilizzi un certificato con firma autografa, visualizzerai `Your connection is not private` errore.

   ![Configurazione SSL - Verifica AEM tramite HTTPS](assets/use-the-ssl-wizard/verify-aem-over-ssl.png)

## Chiave privata e certificato autofirmati

Il seguente file zip contiene [!DNL DER] e [!DNL CRT] file necessari per la configurazione locale di AEM SSL e destinati esclusivamente allo sviluppo locale.

Il [!DNL DER] e [!DNL CERT] I file vengono forniti per comodità e generati seguendo i passaggi descritti nella sezione Generate Private Key (Genera chiave privata) e Self-Signed Certificate (Certificato autofirmato) di seguito.

Se necessario, la passphrase del certificato è **admin**.

Questo localhost - chiave privata e certificato autofirmato.zip (scade a luglio 2028)

[Scarica il file del certificato](assets/use-the-ssl-wizard/certificate.zip)

### Generazione di chiavi private e certificati autofirmati

Il video precedente illustra la configurazione e l’impostazione di SSL in un’istanza di authoring AEM utilizzando certificati autofirmati. I seguenti comandi utilizzano [[!DNL OpenSSL]](https://www.openssl.org/) può generare una chiave privata e un certificato da utilizzare nel passaggio 2 della procedura guidata.

```shell
### Create Private Key
$ openssl genrsa -aes256 -out localhostprivate.key 4096

### Generate Certificate Signing Request using private key
$ openssl req -sha256 -new -key localhostprivate.key -out localhost.csr -subj '/CN=localhost'

### Generate the SSL certificate and sign with the private key, will expire one year from now
$ openssl x509 -req -extfile <(printf "subjectAltName=DNS:localhost") -days 365 -in localhost.csr -signkey localhostprivate.key -out localhost.crt

### Convert Private Key to DER format - SSL wizard requires key to be in DER format
$ openssl pkcs8 -topk8 -inform PEM -outform DER -in localhostprivate.key -out localhostprivate.der -nocrypt
```
