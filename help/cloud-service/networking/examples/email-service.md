---
title: Servizio e-mail
description: Scopri come configurare AEM as a Cloud Service per la connessione a un servizio e-mail utilizzando le porte di uscita.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
duration: 76
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---

# Servizio e-mail

Inviare e-mail da AEM as a Cloud Service configurando AEM `DefaultMailService` per utilizzare porte di uscita di rete avanzate.

Poiché la maggior parte dei servizi di posta elettronica non viene eseguita su HTTP/HTTPS, le connessioni ai servizi di posta da AEM as a Cloud Service devono essere escluse.

+ `smtp.host` è impostato sulla variabile di ambiente OSGi `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` quindi viene instradato attraverso l&#39;uscita.
   + `$[env:AEM_PROXY_HOST]` è una variabile riservata che l’AEM as a Cloud Service mappa sul `proxy.tunnel` host.
   + NON tentare di impostare `AEM_PROXY_HOST` tramite Cloud Manager.
+ `smtp.port` è impostato su `portForward.portOrig` porta mappata sull’host e sulla porta del servizio e-mail di destinazione. In questo esempio viene utilizzata la mappatura: `AEM_PROXY_HOST:30465` → `smtp.sendgrid.com:465`.
   + Il `smpt.port` è impostato su `portForward.portOrig` e NON la porta effettiva del server SMTP. Mappatura tra `smtp.port` e `portForward.portOrig` porta stabilita da Cloud Manager `portForwards` (come illustrato di seguito).

Poiché i segreti non devono essere memorizzati nel codice, è consigliabile fornire il nome utente e la password del servizio e-mail utilizzando [variabili di configurazione OSGi segrete](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values), impostato utilizzando AIO CLI o l’API di Cloud Manager.

In genere, [uscita porta flessibile](../flexible-port-egress.md) viene utilizzato per soddisfare l’integrazione con un servizio e-mail, a meno che non sia necessario per `allowlist` IP Adobe, nel qual caso [indirizzo ip in uscita dedicato](../dedicated-egress-ip-address.md) possono essere utilizzati.

Inoltre, consulta la documentazione AEM su [invio di posta elettronica](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email).

## Supporto di rete avanzato

Il codice di esempio seguente è supportato dalle seguenti opzioni di rete avanzate.

Assicurati che [appropriato](../advanced-networking.md#advanced-networking) la configurazione di rete avanzata è stata impostata prima di seguire questa esercitazione.

| Nessuna rete avanzata | [Uscita porta flessibile](../flexible-port-egress.md) | [Indirizzo IP in uscita dedicato](../dedicated-egress-ip-address.md) | [Virtual Private Network](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## Configurazione OSGi

Questo esempio di configurazione OSGi configura il servizio OSGi di posta AEM per l’utilizzo di un servizio di posta esterno tramite Cloud Manager `portForwards` regola del [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) operazione.

```json
...
"portForwards": [{
    "name": "smtp.mymail.com",
    "portDest": 465,
    "portOrig": 30465
}]
...
```

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.cq.mailer.DefaultMailService.cfg.json`

Configurare AEM [DefaultMailService](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email) come richiesto dal provider di posta elettronica (ad es. `smtp.ssl`, ecc.).

```json
{
    "smtp.host": "$[env:AEM_PROXY_HOST;default=proxy.tunnel]",
    "smtp.port": "30465",
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

Il `EMAIL_USERNAME` e `EMAIL_PASSWORD` È possibile impostare la variabile OSGi e il segreto per ogni ambiente utilizzando:

+ [Configurazione ambiente Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html)
+ o utilizzando `aio CLI` comando

  ```shell
  $ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "myApiKey" --secret EMAIL_PASSWORD "password123"
  ```
