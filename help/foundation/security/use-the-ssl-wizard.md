---
title: Utilizzare la procedura guidata SSL nell’AEM
description: Configurazione SSL guidata di Adobe Experience Manager per semplificare la configurazione di un’istanza AEM da eseguire su HTTPS.
seo-description: Adobe Experience Manager's SSL setup wizard to make it easier to set up an AEM instance to run over HTTPS.
version: 6.4, 6.5
topics: security, operations
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
topic: Security
role: Developer
level: Beginner
exl-id: 4e69e115-12a6-4a57-90da-b91e345c6723
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# Utilizzare la procedura guidata SSL nell’AEM

Configurazione SSL guidata di Adobe Experience Manager per semplificare la configurazione di un’istanza AEM da eseguire su HTTPS.

>[!VIDEO](https://video.tv.adobe.com/v/17993?quality=12&learn=on)

Apri __Configurazione SSL guidata__ può essere aperto direttamente passando a __AEM Author > Strumenti > Sicurezza > Configurazione SSL__.

>[!NOTE]
>
>Per gli ambienti gestiti, è consigliabile che il reparto IT fornisca certificati e chiavi attendibili da CA.
>
>I certificati autofirmati devono essere utilizzati solo a scopo di sviluppo.

## Download di chiavi private e certificati autofirmati

Il seguente file zip contiene [!DNL DER] e [!DNL CRT] file necessari per la configurazione di AEM SSL su localhost e destinati esclusivamente allo sviluppo locale.

Il [!DNL DER] e [!DNL CERT] I file vengono forniti per comodità e generati seguendo i passaggi descritti nella sezione Generate Private Key (Genera chiave privata) e Self-Signed Certificate (Certificato autofirmato) di seguito.

Se necessario, la passphrase del certificato è **admin**.

localhost - chiave privata e certificato autofirmato.zip (scadenza: luglio 2028)

[Scarica il file del certificato](assets/use-the-ssl-wizard/certificate.zip)

## Generazione di chiavi private e certificati autofirmati

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
