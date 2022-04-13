---
title: Servizio e-mail
description: Scopri come configurare AEM as a Cloud Service per la connessione a un servizio e-mail utilizzando le porte di uscita.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
source-git-commit: 8da6d5470c702620ee1121fd2688eb8756f0cebd
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 0%

---

# Servizio e-mail

Invia e-mail da AEM as a Cloud Service configurando AEM `DefaultMailService` per utilizzare porte di uscita di rete avanzate.

Poiché (la maggior parte) i servizi di posta non vengono eseguiti tramite HTTP/HTTPS, le connessioni ai servizi di posta da AEM as a Cloud Service devono essere disattivate.

+ `smtp.host` è impostato sulla variabile di ambiente OSGi `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` quindi viene instradato attraverso l&#39;uscita.
   + `$[env:AEM_PROXY_HOST]` è una variabile riservata che AEM as a Cloud Service viene mappata sul valore interno `proxy.tunnel` host.
   + NON tentare di impostare `AEM_PROXY_HOST` tramite Cloud Manager.
+ `smtp.port` è impostato su `portForward.portOrig` porta mappata all&#39;host e alla porta del servizio e-mail di destinazione. Questo esempio utilizza la mappatura: `AEM_PROXY_HOST:30002` → `smtp.sendgrid.com:465`.
   + La `smpt.port` è impostato su `portForward.portOrig` e NON la porta effettiva del server SMTP. La mappatura tra `smtp.port` e `portForward.portOrig` La porta è stabilita da Cloud Manager `portForwards` , come illustrato di seguito.

Poiché i segreti non devono essere memorizzati nel codice, il nome utente e la password del servizio e-mail sono forniti al meglio utilizzando [variabili di configurazione OSGi segrete](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values), impostato utilizzando AIO CLI o l&#39;API Cloud Manager.

In genere, [uscita porta flessibile](../flexible-port-egress.md) è utilizzato per soddisfare l&#39;integrazione con un servizio e-mail a meno che non sia necessario `allowlist` l&#39;IP Adobe, nel qual caso [indirizzo ip dedicato in uscita](../dedicated-egress-ip-address.md) può essere utilizzato.

## Supporto di rete avanzato

L&#39;esempio di codice seguente è supportato dalle seguenti opzioni di rete avanzate.

Assicurati che [appropriato](../advanced-networking.md#advanced-networking) la configurazione di rete avanzata è stata impostata prima di seguire questa esercitazione.

| Nessuna rete avanzata | [Uscita porta flessibile](../flexible-port-egress.md) | [Indirizzo IP in uscita dedicato](../dedicated-egress-ip-address.md) | [Rete privata virtuale](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ↓ | ↓ | ↓ |

## Configurazione OSGi

Questo esempio di configurazione OSGi configura AEM servizio OSGi di posta per l&#39;utilizzo di un servizio di posta esterno, tramite il seguente Cloud Manager `portForwards` regola [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) funzionamento.

```json
...
"portForwards": [{
    "name": "smtp.mymail.com",
    "portDest": 465,
    "portOrig": 30002
}]
...
```

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.cq.mailer.DefaultMailService.cfg.json`

Configurare AEM [DefaultMailService](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email) come richiesto dal provider di posta elettronica (ad es. `smtp.ssl`, ecc.).

```json
{
    "smtp.host": "$[env:AEM_PROXY_HOST;default=proxy.tunnel]",
    "smtp.port": "30002",
    "smtp.user": "$[env:EMAIL_USERNAME;default=myApiKey]",
    "smtp.password": "$[secret:EMAIL_PASSWORD]",
    "from.address": "noreply@wknd.site",
    "smtp.ssl": true,
    "smtp.starttls": false, 
    "smtp.requiretls": false,
    "debug.email": false,
    "oauth.flow": false
}
```

La `EMAIL_USERNAME` e `EMAIL_PASSWORD` La variabile e il segreto OSGi possono essere impostati per ambiente, utilizzando:

+ [Configurazione ambiente Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html)
+ o utilizzando `aio CLI` command

   ```shell
   $ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "myApiKey" --secret EMAIL_PASSWORD "password123"
   ```
