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
duration: 564
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
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

Passa a __Autore AEM > Strumenti > Protezione > Configurazione SSL__ e apri la __Configurazione guidata SSL__.

![Configurazione guidata SSL](assets/use-the-ssl-wizard/ssl-config-wizard.png)

### Crea credenziali archivio

Per creare un _archivio chiavi_ associato all&#39;utente di sistema `ssl-service` e un _archivio fonti attendibili_ globale, utilizzare il passaggio della procedura guidata __Credenziali archivio__.

1. Immettere la password e confermare la password per l&#39;__archivio chiavi__ associato all&#39;utente di sistema `ssl-service`.
1. Immettere la password e confermare la password per l&#39;archivio fonti attendibili __globale__. Si tratta di un archivio fonti attendibili a livello di sistema e, se è già stato creato, la password immessa viene ignorata.

   ![Configurazione SSL - Archivia credenziali](assets/use-the-ssl-wizard/store-credentials.png)

### Carica chiave privata e certificato

Per caricare la _chiave privata_ e il _certificato SSL_, utilizza il passaggio della procedura guidata __Chiave e certificato__.

In genere, il reparto IT fornisce il certificato e la chiave attendibili della CA, tuttavia è possibile utilizzare il certificato autofirmato per scopi di __sviluppo__ e __test__.

Per creare o scaricare il certificato autofirmato, vedere [Chiave privata autofirmata e certificato](#self-signed-private-key-and-certificate).

1. Carica __Chiave privata__ nel formato DER (Regole di codifica distinte). A differenza di PEM, i file con codifica DER non contengono istruzioni di testo normale come `-----BEGIN CERTIFICATE-----`
1. Carica il __certificato SSL__ associato nel formato `.crt`.

   ![Configurazione SSL - Chiave privata e certificato](assets/use-the-ssl-wizard/privatekey-and-certificate.png)

### Aggiorna i dettagli del connettore SSL

Per aggiornare _nome host_ e _porta_, utilizza il passaggio della procedura guidata __Connettore SSL__.

1. Aggiorna o verifica il valore __HTTPS Hostname__, che deve corrispondere al `Common Name (CN)` del certificato.
1. Aggiorna o verifica il valore della __porta HTTPS__.

   ![Configurazione SSL - Dettagli connettore SSL](assets/use-the-ssl-wizard/ssl-connector-details.png)

### Verificare la configurazione SSL

1. Per verificare l&#39;SSL, fare clic sul pulsante __Vai all&#39;URL HTTPS__.
1. Se si utilizza un certificato autofirmato, verrà visualizzato l&#39;errore `Your connection is not private`.

   ![Configurazione SSL - Verifica AEM tramite HTTPS](assets/use-the-ssl-wizard/verify-aem-over-ssl.png)

## Chiave privata e certificato autofirmati

Il seguente file ZIP contiene [!DNL DER] e [!DNL CRT] file necessari per la configurazione locale di SSL AEM e destinati esclusivamente allo sviluppo locale.

I file [!DNL DER] e [!DNL CERT] vengono forniti per comodità e generati utilizzando i passaggi descritti nella sezione Generate Private Key and Self-Signed Certificate (Genera chiave privata e certificato autofirmato) di seguito.

Se necessario, la passphrase del certificato è **admin**.

Questo localhost - chiave privata e certificato autofirmato.zip (scade a luglio 2028)

[Scarica il file del certificato](assets/use-the-ssl-wizard/certificate.zip)

### Generazione di chiavi private e certificati autofirmati

Il video precedente illustra la configurazione e l’impostazione di SSL in un’istanza di authoring AEM utilizzando certificati autofirmati. I seguenti comandi che utilizzano [[!DNL OpenSSL]](https://www.openssl.org/) possono generare una chiave privata e un certificato da utilizzare nel passaggio 2 della procedura guidata.

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
