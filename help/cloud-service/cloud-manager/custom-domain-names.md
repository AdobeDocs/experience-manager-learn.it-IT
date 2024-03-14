---
title: Aggiungi nome di dominio personalizzato
description: Scopri come aggiungere un nome di dominio personalizzato a AEM come sito ospitato da Cloud Service.
version: Cloud Service
feature: Cloud Manager, Operations
topic: Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
duration: null
last-substantial-update: 2024-03-12T00:00:00Z
jira: KT-15121
thumbnail: KT-15121.jpeg
source-git-commit: 8230991cebf1a9e994f0dfe96c5590d0c19ef887
workflow-type: tm+mt
source-wordcount: '701'
ht-degree: 0%

---


# Aggiungi nome di dominio personalizzato

Scopri come aggiungere un nome di dominio personalizzato al sito web AEM as a Cloud Service.

In questa esercitazione, il branding dell’esempio [WKND AEM](https://github.com/adobe/aem-guides-wknd) viene migliorato aggiungendo un nome di dominio personalizzato indirizzabile HTTPS `wknd.enablementadobe.com` con Transport Layer Security (TLS).

>[!VIDEO](https://video.tv.adobe.com/v/3427903?quality=12&learn=on)

I passaggi di alto livello sono i seguenti:

![Nome dominio personalizzato elevato](./assets/add-custom-domain-name-steps.png){width="800" zoomable="yes"}

## Prerequisiti

>[!VIDEO](https://video.tv.adobe.com/v/3427909?quality=12&learn=on)

- [OpenSSL](https://www.openssl.org/) e [scavare](https://www.isc.org/blogs/dns-checker/) sono installati nel computer locale.
- Accesso a servizi di terze parti:
   - Autorità di certificazione (CA): per richiedere il certificato firmato per il dominio del sito, come [CifraCertificato](https://www.digicert.com/)
   - Servizio di hosting DNS (Domain Name System): consente di aggiungere record DNS per il dominio personalizzato, ad esempio DNS di Azure o Route 53 di AWS.
- Accesso a [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/) come Proprietario business o Manager implementazione.
- Esempio [WKND AEM](https://github.com/adobe/aem-guides-wknd) il sito viene distribuito nell’ambiente AEMCS di [programma di produzione](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs) tipo.

Se non hai accesso a servizi di terze parti, _collaborare con il team di sicurezza o hosting per completare i passaggi_.

## Genera certificato SSL

>[!VIDEO](https://video.tv.adobe.com/v/3427908?quality=12&learn=on)

Sono disponibili due opzioni:

- Utilizzo di `openssl` strumento da riga di comando: è possibile generare una chiave privata e una richiesta di firma del certificato (CSR, Certificate Signing Request) per il dominio del sito. Per richiedere un certificato firmato, invia la CSR a un’autorità di certificazione (CA).

- Il team di hosting fornisce la chiave privata e il certificato firmato richiesti per il sito.

Esaminiamo i passaggi per la prima opzione.

Per generare una chiave privata e una CSR, esegui i seguenti comandi e fornisci le informazioni richieste quando richiesto:

```bash
# Generate a private key and a CSR
$ openssl req -newkey rsa:2048 -keyout <YOUR-SITE-NAME>.key -out <YOUR-SITE-NAME>.csr -nodes
```

Per richiedere un certificato firmato, fornisci alla CA la CSR generata seguendo la relativa documentazione. Una volta che la CA firma la CSR, riceverai il file del certificato firmato.

### Verifica certificato firmato

È consigliabile rivedere il certificato firmato prima di aggiungerlo a Cloud Manager. Puoi rivedere i dettagli del certificato utilizzando il seguente comando:

```bash
# Review the certificate details
$ openssl crl2pkcs7 -nocrl -certfile <YOUR-SIGNED-CERT>.crt | openssl pkcs7 -print_certs -noout
```

Il certificato firmato può contenere la catena di certificati, che include i certificati radice e intermedi insieme al certificato dell’entità finale.

Adobe Cloud Manager accetta il certificato dell’entità finale e la catena di certificati _in campi modulo separati_, quindi è necessario estrarre il certificato dell’entità finale e la catena di certificati dal certificato firmato.

In questa esercitazione, il [CifraCertificato](https://www.digicert.com/) certificato firmato rilasciato contro `*.enablementadobe.com` Il dominio viene utilizzato come esempio. L’entità finale e la catena di certificati vengono estratte aprendo il certificato firmato in un editor di testo e copiando il contenuto tra `-----BEGIN CERTIFICATE-----` e `-----END CERTIFICATE-----` marcatori.

## Aggiungere un certificato SSL in Cloud Manager

>[!VIDEO](https://video.tv.adobe.com/v/3427906?quality=12&learn=on)

Per aggiungere il certificato SSL in Cloud Manager, segui la [Aggiungi certificato SSL](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-ssl-certificates/add-ssl-certificate) documentazione.

## Verifica del nome di dominio

>[!VIDEO](https://video.tv.adobe.com/v/3427905?quality=12&learn=on)

Per verificare il nome di dominio, effettua le seguenti operazioni:

- Aggiungi il nome di dominio in Cloud Manager seguendo la [Aggiungi nome di dominio personalizzato](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-custom-domain-name) documentazione.
- AEM Aggiungere un’ [Record TXT](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-text-record) nel servizio di hosting DNS.
- Verificare i passaggi precedenti eseguendo una query sui server DNS utilizzando `dig` comando.

```bash
# General syntax, the `_aemverification` is prefix provided by Adobe
$ dig _aemverification.[YOUR-DOMAIN-NAME] -t txt

# This tutorial specific example, as the subdomain `wknd.enablementadobe.com` is used
$ dig _aemverification.wknd.enablementadobe.com -t txt
```

La risposta di esempio corretta è simile alla seguente:

```bash
; <<>> DiG 9.10.6 <<>> _aemverification.wknd.enablementadobe.com -t txt
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 8636
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1220
;; QUESTION SECTION:
;_aemverification.wknd.enablementadobe.com. IN TXT

;; ANSWER SECTION:
_aemverification.wknd.enablementadobe.com. 3600    IN TXT "adobe-aem-verification=wknd.enablementadobe.com/105881/991000/bef0e843-9280-4385-9984-357ed9a4217b"

;; Query time: 81 msec
;; SERVER: 153.32.14.247#53(153.32.14.247)
;; WHEN: Tue Mar 12 15:54:25 EDT 2024
;; MSG SIZE  rcvd: 181
```

In questa esercitazione, il DNS di Azure viene utilizzato come esempio. Per aggiungere il record TXT, devi seguire la documentazione del servizio di hosting DNS.

Rivedi [Verifica dello stato del nome di dominio](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-domain-name-status) in caso di problemi.

## Configura record DNS

>[!VIDEO](https://video.tv.adobe.com/v/3427907?quality=12&learn=on)

Per configurare il record DNS per il dominio personalizzato, eseguire la procedura seguente:

- Determina il tipo di record DNS (CNAME o APEX) in base al tipo di dominio, ad esempio dominio radice (APEX) o sottodominio (CNAME), e segui la [Configurazione delle impostazioni DNS](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/configure-dns-settings) documentazione.
- Aggiungi il record DNS nel servizio di hosting DNS.
- Attiva la convalida del record DNS seguendo [Verifica dello stato del record DNS](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-dns-record-status) documentazione.

In questo tutorial, as a **sottodominio** `wknd.enablementadobe.com` viene utilizzato il tipo di record CNAME che punta a `cdn.adobeaemcloud.com` viene aggiunto.

Tuttavia, se utilizzi il **dominio principale**, è necessario aggiungere il tipo di record APEX (A, ALIAS o ANAME) che punta agli indirizzi IP specifici forniti da Adobe.

## Verifica del sito

>[!VIDEO](https://video.tv.adobe.com/v/3427904?quality=12&learn=on)

Per verificare che il sito sia accessibile utilizzando il nome di dominio personalizzato, apri un browser web e passa all’URL del dominio personalizzato. Assicurati che il sito sia accessibile e che il browser mostri una connessione sicura con l’icona del lucchetto.

## Video end-to-end

Puoi anche guardare il video end-to-end che illustra la panoramica, i prerequisiti e i passaggi precedenti per aggiungere un nome di dominio personalizzato al sito ospitato dall’AEM in modalità as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/3427817?quality=12&learn=on)


