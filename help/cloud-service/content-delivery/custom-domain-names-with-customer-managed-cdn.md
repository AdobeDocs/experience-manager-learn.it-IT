---
title: Nome di dominio personalizzato con CDN gestito dal cliente
description: Scopri come implementare un nome di dominio personalizzato nel sito web di AEM as a Cloud Service che utilizza una rete CDN gestita dal cliente.
version: Experience Manager as a Cloud Service
feature: Cloud Manager, Operations
topic: Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-06-21T00:00:00Z
jira: KT-15945
thumbnail: KT-15945.jpeg
exl-id: fa9ee14f-130e-491b-91b6-594ba47a7278
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 0%

---

# Nome di dominio personalizzato con CDN gestito dal cliente

Scopri come aggiungere un nome di dominio personalizzato a un sito Web AEM as a Cloud Service che utilizza una **rete CDN gestita dal cliente**.

In questa esercitazione, il branding del sito [AEM WKND](https://github.com/adobe/aem-guides-wknd) di esempio viene migliorato aggiungendo un nome di dominio personalizzato indirizzabile HTTPS `wkndviaawscdn.enablementadobe.com` con Transport Layer Security (TLS) utilizzando una rete CDN gestita dal cliente. In questo tutorial, AWS CloudFront viene utilizzato come CDN gestito dal cliente, tuttavia qualsiasi provider CDN deve essere compatibile con AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/3432561?quality=12&learn=on)

I passaggi di alto livello sono i seguenti:

![Nome dominio personalizzato con CDN cliente](./assets/add-custom-domain-name-with-customer-CDN.png){width="800" zoomable="yes"}

## Prerequisiti

>[!VIDEO](https://video.tv.adobe.com/v/3432562?quality=12&learn=on)

- [OpenSSL](https://www.openssl.org/) e [dig](https://www.isc.org/blogs/dns-checker/) sono installati nel computer locale.
- Accesso a servizi di terze parti:
   - Autorità di certificazione (CA): per richiedere il certificato firmato per il dominio del sito, ad esempio [DigitCert](https://www.digicert.com/)
   - CDN cliente: per configurare la CDN cliente e aggiungere certificati SSL e dettagli del dominio, come AWS CloudFront, Azure CDN o Akamai.
   - Servizio di hosting DNS (Domain Name System): consente di aggiungere record DNS per il dominio personalizzato, ad esempio DNS di Azure o Route 53 di AWS.
- Accedi a [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/) per distribuire la regola CDN di convalida dell&#39;intestazione HTTP nell&#39;ambiente AEM as a Cloud Service.
- Il sito di esempio [AEM WKND](https://github.com/adobe/aem-guides-wknd) è stato distribuito nell&#39;ambiente AEM as a Cloud Service di tipo [programma di produzione](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs).

Se non hai accesso a servizi di terze parti, _collabora con il tuo team di sicurezza o di hosting per completare i passaggi_.

## Genera certificato SSL

>[!VIDEO](https://video.tv.adobe.com/v/3427908?quality=12&learn=on)

Sono disponibili due opzioni:

1. Utilizzo dello strumento da riga di comando `openssl`: è possibile generare una chiave privata e una richiesta di firma del certificato (CSR, Certificate Signing Request) per il dominio del sito. Per richiedere un certificato firmato, invia la CSR a un’autorità di certificazione (CA).
1. Il team di hosting fornisce la chiave privata e il certificato firmato richiesti per il sito.

Esaminiamo i passaggi per la prima opzione.

Per generare una chiave privata e una CSR, esegui i seguenti comandi e fornisci le informazioni richieste quando richiesto:

```bash
# Generate a private key and a CSR
$ openssl req -newkey rsa:2048 -keyout <YOUR-SITE-NAME>.key -out <YOUR-SITE-NAME>.csr -nodes
```

Per richiedere un certificato firmato, fornisci alla CA la CSR generata seguendo la relativa documentazione. Una volta che la CA firma la CSR, riceverai il file del certificato firmato.

### Verifica certificato firmato

È consigliabile rivedere il certificato firmato prima di aggiungerlo al Cloud Manager. Puoi rivedere i dettagli del certificato utilizzando il seguente comando:

```bash
# Review the certificate details
$ openssl crl2pkcs7 -nocrl -certfile <YOUR-SIGNED-CERT>.crt | openssl pkcs7 -print_certs -noout
```

Il certificato firmato può contenere la catena di certificati, che include i certificati radice e intermedi insieme al certificato dell’entità finale.

Adobe Cloud Manager accetta il certificato dell&#39;entità finale e la catena di certificati _in campi modulo separati_, pertanto è necessario estrarre il certificato dell&#39;entità finale e la catena di certificati dal certificato firmato.

In questa esercitazione, il certificato firmato [DigitCert](https://www.digicert.com/) rilasciato per il dominio `*.enablementadobe.com` viene utilizzato come esempio. L&#39;entità finale e la catena di certificati vengono estratte aprendo il certificato firmato in un editor di testo e copiando il contenuto tra i marcatori `-----BEGIN CERTIFICATE-----` e `-----END CERTIFICATE-----`.

## Configurare la rete CDN gestita dal cliente

>[!VIDEO](https://video.tv.adobe.com/v/3432563?quality=12&learn=on)

Configura la rete CDN del cliente, come AWS CloudFront, Azure CDN o Akamai, e aggiungi il certificato SSL e i dettagli del dominio. In questo tutorial, AWS CloudFront viene utilizzato come esempio. Tuttavia, a seconda del fornitore CDN, i passaggi possono variare. I callout chiave sono:

- Aggiungi il certificato SSL alla rete CDN.
- Aggiungi il nome di dominio personalizzato alla rete CDN.
- Configura la rete CDN per memorizzare in cache il contenuto, come immagini, file CSS e JavaScript.
- Aggiungi l&#39;intestazione HTTP `X-Forwarded-Host` alle impostazioni della rete CDN in modo che la rete CDN includa tale intestazione in tutte le richieste che invia all&#39;origine AEMCD.
- Assicurarsi che il valore dell&#39;intestazione `Host` sia impostato sul dominio AEM as a Cloud Service predefinito contenente l&#39;ID del programma e dell&#39;ambiente e che termini con `adobeaemcloud.com`. Il valore dell’intestazione dell’host HTTP passato dalla rete CDN del cliente alla rete CDN di Adobe deve essere il dominio predefinito di AEM as a Cloud Service. Qualsiasi altro valore restituisce uno stato di errore.

## Configurare i record DNS

>[!VIDEO](https://video.tv.adobe.com/v/3432564?quality=12&learn=on)

Per configurare il record DNS per il dominio personalizzato, eseguire la procedura seguente:

1. Aggiungi un record CNAME per il dominio personalizzato che punta al nome di dominio CDN.

Questa esercitazione aggiunge un record CNAME al DNS di Azure per il dominio personalizzato `wkndviaawscdn.enablementadobe.com` e lo punta al nome del dominio di distribuzione AWS CloudFront.

### Verifica del sito

Verifica il nome di dominio personalizzato accedendo al sito utilizzando il nome di dominio personalizzato.
Può funzionare o meno a seconda della configurazione vhhost nell’ambiente AEM as a Cloud Service.

Un passaggio fondamentale per la sicurezza consiste nel distribuire la regola CDN di convalida dell’intestazione HTTP nell’ambiente AEM as a Cloud Service. La regola assicura che la richiesta provenga dalla rete CDN del cliente e non da qualsiasi altra origine.

## Stato di lavoro corrente senza regola CDN di convalida dell’intestazione HTTP

>[!VIDEO](https://video.tv.adobe.com/v/3432565?quality=12&learn=on)

Senza la regola CDN di convalida dell&#39;intestazione HTTP, il valore dell&#39;intestazione `Host` viene impostato sul dominio AEM as a Cloud Service predefinito contenente l&#39;ID del programma e dell&#39;ambiente e termina con `adobeaemcloud.com`. Adobe CDN trasforma il valore dell&#39;intestazione `Host` nel valore di `X-Forwarded-Host` ricevuto dal CDN del cliente solo se è distribuita la regola CDN di convalida dell&#39;intestazione HTTP. In caso contrario, il valore dell&#39;intestazione `Host` viene passato così com&#39;è all&#39;ambiente AEM as a Cloud Service e l&#39;intestazione `X-Forwarded-Host` non viene utilizzata.

### Esempio di codice servlet per stampare il valore dell’intestazione Host

Il seguente codice servlet stampa i valori di intestazione HTTP `Host`, `X-Forwarded-*`, `Referer` e `Via` nella risposta JSON.

```java
package com.adobe.aem.guides.wknd.core.servlets;

import java.io.IOException;
import java.util.Enumeration;

import javax.servlet.Servlet;
import javax.servlet.ServletException;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.ResourceResolverFactory;
import org.apache.sling.api.servlets.HttpConstants;
import org.apache.sling.api.servlets.ServletResolverConstants;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

@Component(service = Servlet.class, property = {
        ServletResolverConstants.SLING_SERVLET_PATHS + "=/bin/verify-headers",
        ServletResolverConstants.SLING_SERVLET_METHODS + "=" + HttpConstants.METHOD_GET
})
public class VerifyHeadersServlet extends SlingSafeMethodsServlet {

    @Reference
    private ResourceResolverFactory resourceResolverFactory;

    @Override
    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response)
            throws ServletException, IOException {
        response.setContentType("application/json");
        response.setCharacterEncoding("UTF-8");

        // Create JSON response
        StringBuilder jsonResponse = new StringBuilder();
        jsonResponse.append("{");

        Enumeration<String> headerNames = request.getHeaderNames();
        boolean firstHeader = true;

        while (headerNames.hasMoreElements()) {
            String headerName = headerNames.nextElement();

            if (headerName.startsWith("X-Forwarded-") || headerName.startsWith("Host")
                    || headerName.startsWith("Referer") || headerName.startsWith("Via")) {
                if (!firstHeader) {
                    jsonResponse.append(",");
                }
                jsonResponse.append("\"").append(headerName).append("\": \"").append(request.getHeader(headerName))
                        .append("\"");
                firstHeader = false;
            }
        }

        jsonResponse.append("}");

        response.getWriter().write(jsonResponse.toString());
    }
}
```

Per verificare il servlet, aggiornare il file `../dispatcher/src/conf.dispatcher.d/filters/filters.any` con la seguente configurazione. Verificare inoltre che la rete CDN sia configurata per **NOT cache** del percorso `/bin/*`.

```plaintext
# Testing purpose bin
/0300 { /type "allow" /extension "json" /path "/bin/*"}
/0301 { /type "allow" /path "/bin/*"}
/0302 { /type "allow" /url "/bin/*"}
```

## Configurare e distribuire la regola CDN di convalida dell’intestazione HTTP

>[!VIDEO](https://video.tv.adobe.com/v/3432566?quality=12&learn=on)

Per configurare e distribuire la regola CDN di convalida dell’intestazione HTTP, effettua le seguenti operazioni:

- Aggiungere la regola CDN di convalida dell&#39;intestazione HTTP nel file `cdn.yaml`. Di seguito è riportato un esempio.

  ```yaml
  kind: "CDN"
  version: "1"
  metadata:
    envTypes: ["prod"]
  data:
    authentication:
      authenticators:
        - name: edge-auth
          type: edge
          edgeKey1: ${{CDN_EDGEKEY_080124}}
          edgeKey2: ${{CDN_EDGEKEY_110124}}
      rules:
        - name: edge-auth-rule
          when: { reqProperty: tier, equals: "publish" }
          action:
            type: authenticate
            authenticator: edge-auth
  ```

- Creare variabili di ambiente di tipo segreto (CDN_EDGEKEY_080124, CDN_EDGEKEY_110124) utilizzando l’interfaccia utente di Cloud Manager.
- Distribuisci la regola CDN di convalida dell’intestazione HTTP nell’ambiente AEM as a Cloud Service utilizzando la pipeline Cloud Manager.

## Passare il segreto nell’intestazione HTTP X-AEM-Edge-Key

>[!VIDEO](https://video.tv.adobe.com/v/3432567?quality=12&learn=on)

Aggiorna la rete CDN del cliente in modo che passi il segreto nell&#39;intestazione HTTP `X-AEM-Edge-Key`. Il segreto viene utilizzato dal CDN di Adobe per verificare che la richiesta provenga dal CDN del cliente e trasformare il valore dell&#39;intestazione `Host` nel valore di `X-Forwarded-Host` ricevuto dal CDN del cliente.

## Video end-to-end

Puoi anche guardare il video end-to-end che illustra i passaggi precedenti per aggiungere un nome di dominio personalizzato con una rete CDN gestita dal cliente a un sito ospitato da AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/3432568?quality=12&learn=on)
