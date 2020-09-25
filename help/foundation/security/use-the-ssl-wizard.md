---
title: Utilizzo della procedura guidata SSL in AEM
description: La procedura guidata di configurazione SSL di Adobe Experience Manager semplifica la configurazione di un’istanza AEM da eseguire tramite HTTPS.
seo-description: La procedura guidata di configurazione SSL di Adobe Experience Manager semplifica la configurazione di un’istanza AEM da eseguire tramite HTTPS.
version: 6.3, 6,4, 6.5
feature: null
topics: security, operations
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 0%

---


# Utilizzo della procedura guidata SSL in AEM

La procedura guidata di configurazione SSL di Adobe Experience Manager semplifica la configurazione di un’istanza AEM da eseguire tramite HTTPS.

>[!VIDEO](https://video.tv.adobe.com/v/17993/?quality=12&learn=on)

>[!NOTE]
>
>Per gli ambienti gestiti, è consigliabile che il reparto IT fornisca certificati e chiavi affidabili CA.
>
>I certificati autofirmati sono utilizzabili solo a scopo di sviluppo.

## Chiave privata e download certificato autofirmato

Il file ZIP seguente contiene [!DNL DER] e [!DNL CRT] i file necessari per impostare AEM SSL su localhost e destinati esclusivamente allo sviluppo locale.

I file [!DNL DER] e [!DNL CERT] i file vengono forniti per comodità e generati utilizzando i passaggi descritti nella sezione Genera chiave privata e Certificato autofirmato di seguito.

Se necessario, la frase di autorizzazione del certificato è **admin**.

localhost - chiave privata e certificato autofirmato.zip (scadenza luglio 2028)

[Scaricare il file del certificato](assets/use-the-ssl-wizard/certificate.zip)

## Chiave privata e generazione di certificati autofirmati

Il video precedente illustra la configurazione e l’impostazione di SSL su un’istanza di autore AEM mediante certificati autofirmati. I comandi seguenti che utilizzano [[!DNL OpenSSL]](https://www.openssl.org/) possono generare una chiave privata e un certificato da utilizzare nel passaggio 2 della procedura guidata.

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
