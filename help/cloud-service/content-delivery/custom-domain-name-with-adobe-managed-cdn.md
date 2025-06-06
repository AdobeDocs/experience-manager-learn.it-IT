---
title: Nome di dominio personalizzato con CDN gestito da Adobe
description: Scopri come implementare un nome di dominio personalizzato nel sito web di AEM as a Cloud Service che utilizza una rete CDN gestita da Adobe.
version: Experience Manager as a Cloud Service
feature: Cloud Manager, Operations
topic: Administration, Architecture
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 1042
last-substantial-update: 2024-08-12T00:00:00Z
jira: KT-15121
thumbnail: KT-15121.jpeg
exl-id: 8936c3ae-2daf-4d0f-b260-28376ae28087
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 0%

---

# Nome di dominio personalizzato con Adobe CDN

Scopri come implementare un nome di dominio personalizzato per un sito web AEM as a Cloud Service che utilizza la rete CDN (Content Delivery Network) di Adobe.

In questa esercitazione, il branding del sito [AEM WKND](https://github.com/adobe/aem-guides-wknd) di esempio viene migliorato aggiungendo un nome di dominio personalizzato indirizzabile HTTPS `wknd.enablementadobe.com` con Transport Layer Security (TLS).

>[!VIDEO](https://video.tv.adobe.com/v/3427903?quality=12&learn=on)

I passaggi di alto livello sono i seguenti:

![Nome dominio personalizzato con CDN Adobe](./assets/add-custom-domain-name-with-Adobe-CDN.png){width="800" zoomable="yes"}

## Prerequisiti

>[!VIDEO](https://video.tv.adobe.com/v/3427909?quality=12&learn=on)

- [OpenSSL](https://www.openssl.org/) e [dig](https://www.isc.org/blogs/dns-checker/) sono installati nel computer locale.
- Accesso a servizi di terze parti:
   - Autorità di certificazione (CA): per richiedere il certificato firmato per il dominio del sito, ad esempio [DigitCert](https://www.digicert.com/)
   - Servizio di hosting DNS (Domain Name System): consente di aggiungere record DNS per il dominio personalizzato, ad esempio DNS di Azure o Route 53 di AWS.
- Accesso a [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/) come **Proprietario business** o **Responsabile della distribuzione**.
- Il sito di esempio [AEM WKND](https://github.com/adobe/aem-guides-wknd) è stato distribuito nell&#39;ambiente AEM as a Cloud Service di tipo [programma di produzione](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs).

Se non hai accesso a servizi di terze parti, _collabora con il tuo team di sicurezza o di hosting per completare i passaggi_.

## Genera certificato SSL

>[!VIDEO](https://video.tv.adobe.com/v/3441497?quality=12&learn=on&captions=ita)

Sono disponibili due opzioni:

1. Utilizzare lo strumento da riga di comando `openssl` per generare una chiave privata e una richiesta di firma del certificato (CSR, Certificate Signing Request) per il dominio del sito. Per richiedere un certificato firmato, invia la CSR a un’autorità di certificazione (CA).
1. Il team di hosting fornisce la chiave privata e il certificato firmato richiesti per il sito.

Esaminiamo i passaggi per la prima opzione.

Per generare una chiave privata e una CSR, esegui i seguenti comandi e fornisci le informazioni richieste quando richiesto:

```bash
# Generate a private key and a CSR
$ openssl req -newkey rsa:2048 -keyout <YOUR-SITE-NAME>.key -out <YOUR-SITE-NAME>.csr -nodes
```

Per richiedere un certificato firmato, fornisci alla CA la CSR generata seguendo la documentazione della CA. Una volta che la CA firma la CSR, riceverai il file del certificato firmato.

### Verifica certificato firmato

Rivedi il certificato firmato prima di aggiungerlo al Cloud Manager. Rivedi i dettagli del certificato utilizzando il seguente comando:

```bash
# Review the certificate details
$ openssl crl2pkcs7 -nocrl -certfile <YOUR-SIGNED-CERT>.crt | openssl pkcs7 -print_certs -noout
```

Il certificato firmato può contenere la catena di certificati, che include i certificati radice e intermedi insieme al certificato dell’entità finale.

Adobe Cloud Manager accetta il certificato dell&#39;entità finale e la catena di certificati _in campi di modulo separati_, pertanto è necessario estrarre il certificato dell&#39;entità finale e la catena di certificati dal certificato firmato.

In questa esercitazione, il certificato firmato [DigitCert](https://www.digicert.com/) rilasciato per il dominio `*.enablementadobe.com` viene utilizzato come esempio. L&#39;entità finale e la catena di certificati vengono estratte aprendo il certificato firmato in un editor di testo e copiando il contenuto tra i marcatori `-----BEGIN CERTIFICATE-----` e `-----END CERTIFICATE-----`.

## Aggiungere un certificato SSL in Cloud Manager

>[!VIDEO](https://video.tv.adobe.com/v/3427906?quality=12&learn=on)

Per aggiungere il certificato SSL in Cloud Manager, segui la documentazione [aggiungi certificato SSL](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-ssl-certificates/add-ssl-certificate).

## Verifica del nome di dominio

>[!VIDEO](https://video.tv.adobe.com/v/3427905?quality=12&learn=on)

Per verificare il nome di dominio, effettua le seguenti operazioni:

- Aggiungere un nome di dominio in Cloud Manager seguendo la documentazione di [aggiungi nome di dominio personalizzato](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-custom-domain-name).
- Aggiungi un [record TXT](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-text-record) specifico per AEM nel servizio di hosting DNS.
- Verificare i passaggi precedenti eseguendo una query sui server DNS tramite il comando `dig`.

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

Questa esercitazione utilizza il DNS di Azure, tuttavia è possibile utilizzare qualsiasi provider DNS. Per aggiungere il record TXT, devi seguire la documentazione del servizio di hosting DNS.

In caso di problemi, consulta la documentazione [verifica dello stato del nome di dominio](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-domain-name-status).

## Configura record DNS

>[!VIDEO](https://video.tv.adobe.com/v/3427907?quality=12&learn=on)

Per configurare il record DNS per il dominio personalizzato, eseguire la procedura seguente:

1. Determinare il tipo di record DNS (CNAME o APEX) in base al tipo di dominio, ad esempio dominio radice (APEX) o sottodominio (CNAME), e seguire la documentazione [Configurazione delle impostazioni DNS](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/configure-dns-settings).
1. Aggiungi il record DNS nel servizio di hosting DNS.
1. Attiva la convalida del record DNS seguendo la documentazione di [Verifica dello stato del record DNS](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-dns-record-status).

In questa esercitazione, quando si utilizza un **sottodominio** `wknd.enablementadobe.com`, viene aggiunto il tipo di record CNAME che punta a `cdn.adobeaemcloud.com`.

Tuttavia, se si utilizza il **dominio principale**, è necessario aggiungere un tipo di record APEX (alias A, ALIAS o ANAME) che punti agli indirizzi IP specifici forniti da Adobe.

## Verifica del sito

>[!VIDEO](https://video.tv.adobe.com/v/3427904?quality=12&learn=on)

Per verificare che il sito sia accessibile utilizzando il nome di dominio personalizzato, apri un browser web e passa all’URL del dominio personalizzato. Assicurati che il sito sia accessibile e che il browser mostri una connessione sicura con l’icona del lucchetto.

## Video end to end

Puoi anche guardare il video end-to-end che illustra la panoramica, i prerequisiti e i passaggi precedenti per aggiungere un nome di dominio personalizzato al sito ospitato da AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/3427817?quality=12&learn=on)
