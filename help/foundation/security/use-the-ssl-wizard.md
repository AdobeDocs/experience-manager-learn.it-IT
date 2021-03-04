---
title: Utilizzare la procedura guidata SSL in AEM
description: La procedura guidata di configurazione SSL di Adobe Experience Manager facilita la configurazione di un’istanza AEM da eseguire su HTTPS.
seo-description: La procedura guidata di configurazione SSL di Adobe Experience Manager facilita la configurazione di un’istanza AEM da eseguire su HTTPS.
version: 6.3, 6,4, 6.5
topics: security, operations
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
topic: Sicurezza
role: Developer (Sviluppatore)
level: Principiante
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---


# Utilizzare la procedura guidata SSL in AEM

La procedura guidata di configurazione SSL di Adobe Experience Manager facilita la configurazione di un’istanza AEM da eseguire su HTTPS.

>[!VIDEO](https://video.tv.adobe.com/v/17993/?quality=12&learn=on)

>[!NOTE]
>
>Per gli ambienti gestiti, è consigliabile che il reparto IT fornisca certificati e chiavi attendibili per le CA.
>
>I certificati autofirmati devono essere utilizzati solo a scopo di sviluppo.

## Scaricare chiave privata e certificato autofirmato

Il seguente file zip contiene i file [!DNL DER] e [!DNL CRT] necessari per la configurazione di AEM SSL su localhost e destinati solo a scopi di sviluppo locale.

I file [!DNL DER] e [!DNL CERT] vengono forniti per comodità e generati utilizzando i passaggi descritti nella sezione Genera chiave privata e Certificato autofirmato di seguito.

Se necessario, la frase di passaggio del certificato è **admin**.

localhost - chiave privata e certificato.zip autofirmato (scadenza luglio 2028)

[Scarica il file del certificato](assets/use-the-ssl-wizard/certificate.zip)

## Generazione di certificati con chiave privata e autofirma

Il video precedente illustra la configurazione e la configurazione di SSL su un’istanza di authoring di AEM utilizzando certificati autofirmati. I comandi seguenti che utilizzano [[!DNL OpenSSL]](https://www.openssl.org/) possono generare una chiave privata e un certificato da utilizzare nel passaggio 2 della procedura guidata.

```shell
### Create Private Key
$ openssl genrsa -aes256 -out localhostprivate.key 4096

### Generate Certificate Signing Request using private key
$ openssl req -sha256 -new -key localhostprivate.key -out localhost.csr -subj '/CN=localhost'

### Generate the SSL certificate and sign with the private key, will expire one year from now
$ openssl x509 -req -days 365 -in localhost.csr -signkey localhostprivate.key -out localhost.crt

### Convert Private Key to DER format - SSL wizard requires key to be in DER format
$ openssl pkcs8 -topk8 -inform PEM -outform DER -in localhostprivate.key -out localhostprivate.der -nocrypt
```
