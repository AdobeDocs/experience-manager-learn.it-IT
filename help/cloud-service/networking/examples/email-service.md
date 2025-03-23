---
title: Servizio e-mail
description: Scopri come configurare AEM as a Cloud Service per la connessione a un servizio e-mail utilizzando le porte di uscita.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
duration: 76
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---

# Servizio e-mail

Invia e-mail da AEM as a Cloud Service configurando l&#39;elemento `DefaultMailService` di AEM per utilizzare porte di uscita di rete avanzate.

Poiché la maggior parte dei servizi di posta non viene eseguita su HTTP/HTTPS, le connessioni ai servizi di posta da AEM as a Cloud Service devono essere escluse.

+ `smtp.host` è impostato sulla variabile di ambiente OSGi `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` in modo che venga instradato attraverso l&#39;uscita.
   + `$[env:AEM_PROXY_HOST]` è una variabile riservata mappata da AEM as a Cloud Service all&#39;host `proxy.tunnel` interno.
   + NON tentare di impostare `AEM_PROXY_HOST` tramite Cloud Manager.
+ `smtp.port` è impostato sulla porta `portForward.portOrig` mappata all&#39;host e alla porta del servizio e-mail di destinazione. In questo esempio viene utilizzata la mappatura: `AEM_PROXY_HOST:30465` → `smtp.sendgrid.com:465`.
   + `smpt.port` è impostato sulla porta `portForward.portOrig` e NON sulla porta effettiva del server SMTP. Il mapping tra `smtp.port` e la porta `portForward.portOrig` è stabilito dalla regola Cloud Manager `portForwards` (come illustrato di seguito).

Poiché i segreti non devono essere archiviati nel codice, è consigliabile fornire il nome utente e la password del servizio e-mail utilizzando [variabili di configurazione OSGi segrete](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values), impostate utilizzando la CLI AIO o l&#39;API Cloud Manager.

In genere, [l&#39;uscita da porta flessibile](../flexible-port-egress.md) viene utilizzata per soddisfare l&#39;integrazione con un servizio e-mail a meno che non sia necessario `allowlist` l&#39;IP di Adobe, nel qual caso è possibile utilizzare [l&#39;indirizzo IP in uscita dedicato](../dedicated-egress-ip-address.md).

Inoltre, consulta la documentazione di AEM in [invio di posta elettronica](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email).

## Supporto di rete avanzato

Il codice di esempio seguente è supportato dalle seguenti opzioni di rete avanzate.

Prima di seguire questa esercitazione, assicurati che la configurazione di rete avanzata [appropriata](../advanced-networking.md#advanced-networking) sia stata configurata.

| Nessuna rete avanzata | [Uscita porta flessibile](../flexible-port-egress.md) | [Indirizzo IP in uscita dedicato](../dedicated-egress-ip-address.md) | [Rete privata virtuale](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## Configurazione OSGi

Questo esempio di configurazione OSGi configura il servizio OSGi Mail di AEM per l&#39;utilizzo di un servizio di posta esterno, tramite la seguente regola di Cloud Manager `portForwards` dell&#39;operazione [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration).

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

Configura il [DefaultMailService](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email) di AEM come richiesto dal tuo provider di posta elettronica (ad esempio `smtp.ssl`, ecc.).

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

La variabile OSGi `EMAIL_USERNAME` e `EMAIL_PASSWORD` e il segreto possono essere impostati per ambiente utilizzando:

+ [Configurazione ambiente Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html)
+ o utilizzando il comando `aio CLI`

  ```shell
  $ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "myApiKey" --secret EMAIL_PASSWORD "password123"
  ```
