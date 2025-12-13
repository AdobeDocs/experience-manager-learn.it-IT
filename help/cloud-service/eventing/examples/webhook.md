---
title: Webhook ed eventi AEM
description: Scopri come ricevere eventi AEM su un webhook e rivedere i dettagli dell’evento come payload, intestazioni e metadati.
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
duration: 358
last-substantial-update: 2023-01-29T00:00:00Z
jira: KT-14732
thumbnail: KT-14732.jpeg
exl-id: 00954d74-c4c7-4dac-8d23-7140c49ae31f
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '523'
ht-degree: 1%

---

# Webhook ed eventi AEM

Scopri come ricevere eventi AEM su un webhook e rivedere i dettagli dell’evento come payload, intestazioni e metadati.


>[!VIDEO](https://video.tv.adobe.com/v/3449756?captions=ita&quality=12&learn=on)


>[!IMPORTANT]
>
>Il video fa riferimento a un endpoint del webhook ospitato da Glitch. Poiché Glitch ha interrotto il servizio di hosting, il webhook è stato migrato al servizio app di Azure.
>
>La funzionalità rimane la stessa, solo la piattaforma di hosting è cambiata.


Invece di utilizzare il webhook di esempio fornito da Adobe, puoi utilizzare anche il tuo endpoint per ricevere eventi AEM.

## Prerequisiti

Per completare questa esercitazione, è necessario:

- Ambiente AEM as a Cloud Service con [evento AEM abilitato](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- [Progetto Adobe Developer Console configurato per gli eventi AEM](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).


## Accedi al webhook

Per accedere al webhook di esempio fornito da Adobe, effettua le seguenti operazioni:

- Verifica di poter accedere al [webhook di esempio fornito da Adobe](https://aemeventing-webhook.azurewebsites.net/) in una nuova scheda del browser.

  ![Esempio di webhook fornito da Adobe](../assets/examples/webhook/adobe-provided-webhook.png)

- Immetti un nome univoco per il webhook, ad esempio `<YOUR_PETS_NAME>-aem-eventing` e fai clic su **Connetti**. Dovresti vedere `Connected to: ${YOUR-WEBHOOK-URL}` messaggio visualizzato sullo schermo.

  ![Crea l&#39;endpoint del webhook](../assets/examples/webhook/create-webhook-endpoint.png)

- Prendere nota dell&#39;**URL webhook**. Ne avrai bisogno più avanti in questa esercitazione.

## Configurare il webhook in Adobe Developer Console Project

Per ricevere eventi AEM dall’URL del webhook precedente, effettua le seguenti operazioni:

- In [Adobe Developer Console](https://developer.adobe.com), passa al progetto e fai clic per aprirlo.

- Nella sezione **Prodotti e servizi**, fai clic sui puntini di sospensione `...` accanto alla scheda degli eventi desiderati che dovrebbe inviare gli eventi di AEM al webhook e seleziona **Modifica**.

  ![Modifica progetto Adobe Developer Console](../assets/examples/webhook/adobe-developer-console-project-edit.png)

- Nella finestra di dialogo **Configura registrazione evento** appena aperta, fai clic su **Avanti** per passare al passaggio **Come ricevere gli eventi**.

  ![Configurazione progetto Adobe Developer Console](../assets/examples/webhook/adobe-developer-console-project-configure.png)

- Nel passaggio **Ricezione degli eventi**, seleziona l&#39;opzione **Webhook** e incolla il **URL del webhook** copiato in precedenza dal webhook di esempio fornito da Adobe, quindi fai clic su **Salva eventi configurati**.

  ![Webhook progetto Adobe Developer Console](../assets/examples/webhook/adobe-developer-console-project-webhook.png)

- Nella pagina di esempio del webhook fornito da Adobe, dovresti trovare una richiesta GET, una richiesta di verifica inviata da Adobe I/O Events per verificare l’URL del webhook.

  ![Webhook - richiesta di verifica](../assets/examples/webhook/webhook-challenge-request.png)


## Attivare eventi AEM

Per attivare gli eventi di AEM dall’ambiente AEM as a Cloud Service registrato nel progetto Adobe Developer Console precedente, effettua le seguenti operazioni:

- Accedi all&#39;ambiente di authoring AEM as a Cloud Service tramite [Cloud Manager](https://my.cloudmanager.adobe.com/).

- A seconda dei **eventi sottoscritti**, crea, aggiorna, elimina, pubblica o annulla la pubblicazione di un frammento di contenuto.

## Rivedi dettagli evento

Dopo aver completato i passaggi precedenti, dovresti vedere gli eventi AEM consegnati al webhook. Cerca la richiesta POST nella pagina di esempio del webhook fornita da Adobe.

![Webhook - richiesta POST](../assets/examples/webhook/webhook-post-request.png)

Di seguito sono riportati i dettagli chiave della richiesta POST:

- percorso: `/webhook/${YOUR-WEBHOOK-URL}`, ad esempio `/webhook/AdobeTM-aem-eventing`

- intestazioni: richiedi le intestazioni inviate da Adobe I/O Events, ad esempio:

```json
{
  "host": "aemeventing-webhook.azurewebsites.net",
  "user-agent": "Adobe/1.0",
  "accept-encoding": "deflate,compress,identity",
  "max-forwards": "10",
  "x-adobe-public-key2-path": "/prod/keys/pub-key-kruhWwu4Or.pem",
  "x-adobe-delivery-id": "25c36f70-9238-4e4c-b1d8-4d9a592fed9d",
  "x-adobe-provider": "aemsites_7ABB3E6A5A7491460A495D61@AdobeOrg_acct-aem-p63947-e1249010@adobe.com",
  "x-adobe-public-key1-path": "/prod/keys/pub-key-lyTiz3gQe4.pem",
  "x-adobe-event-id": "b555a1b1-935b-4541-b410-1915775338b5",
  "x-adobe-event-code": "aem.sites.contentFragment.modified",
  "x-adobe-digital-signature-2": "Lvw8+txbQif/omgOamJXJaJdJMLDH5BmPA+/RRLhKG2LZJYWKiomAE9DqKhM349F8QMdDq6FXJI0vJGdk0FGYQa6JMrU+LK+1fGhBpO98LaJOdvfUQGG/6vq8/uJlcaQ66tuVu1xwH232VwrQOKdcobE9Pztm6UX0J11Uc7vtoojUzsuekclKEDTQx5vwBIYK12bXTI9yLRsv0unBZfNRrV0O4N7KA9SRJFIefn7hZdxyYy7IjMdsoswG36E/sDOgcnW3FVM+rhuyWEizOd2AiqgeZudBKAj8ZPptv+6rZQSABbG4imOa5C3t85N6JOwffAAzP6qs7ghRID89OZwCg==",
  "x-adobe-digital-signature-1": "ZQywLY1Gp/MC/sXzxMvnevhnai3ZG/GaO4ThSGINIpiA/RM47ssAw99KDCy1loxQyovllEmN0ifAwfErQGwDa5cuJYEoreX83+CxqvccSMYUPb5JNDrBkG6W0CmJg6xMeFeo8aoFbePvRkkDOHdz6nT0kgJ70x6mMKgCBM+oUHWG13MVU3YOmU92CJTzn4hiSK8o91/f2aIdfIui/FDp8U20cSKKMWpCu25gMmESorJehe4HVqxLgRwKJHLTqQyw6Ltwy2PdE0guTAYjhDq6AUd/8Fo0ORCY+PsS/lNxim9E9vTRHS7TmRuHf7dpkyFwNZA6Au4GWHHS87mZSHNnow==",
  "x-arr-log-id": "881073f0-7185-4812-9f17-4db69faf2b68",
  "client-ip": "52.37.214.82:46066",
  "disguised-host": "aemeventing-webhook.azurewebsites.net",
  "x-site-deployment-id": "aemeventing-webhook",
  "was-default-hostname": "aemeventing-webhook.azurewebsites.net",
  "x-forwarded-proto": "https",
  "x-appservice-proto": "https",
  "x-arr-ssl": "2048|256|CN=Microsoft Azure RSA TLS Issuing CA 03, O=Microsoft Corporation, C=US|CN=*.azurewebsites.net, O=Microsoft Corporation, L=Redmond, S=WA, C=US",
  "x-forwarded-tlsversion": "1.3",
  "x-forwarded-for": "52.37.214.82:46066",
  "x-original-url": "/webhook/AdobeTechMarketing-aem-eventing",
  "x-waws-unencoded-url": "/webhook/AdobeTechMarketing-aem-eventing",
  "x-client-ip": "52.37.214.82",
  "x-client-port": "46066",
  "content-type": "application/cloudevents+json; charset=UTF-8",
  "content-length": "1178"
}
```

- corpo/payload: corpo della richiesta inviato dal Adobe I/O Events, ad esempio:

```json
{
  "specversion": "1.0",
  "id": "83b0eac0-56d6-4499-afa6-4dc58ff6ac7f",
  "source": "acct:aem-p63947-e1249010@adobe.com",
  "type": "aem.sites.contentFragment.modified",
  "datacontenttype": "application/json",
  "dataschema": "https://ns.adobe.com/xdm/aem/sites/events/content-fragment-modified.json",
  "time": "2025-07-24T13:53:23.994109827Z",
  "eventid": "b555a1b1-935b-4541-b410-1915775338b5",
  "event_id": "b555a1b1-935b-4541-b410-1915775338b5",
  "recipient_client_id": "606d4074c7ea4962aaf3bc2a5ac3b7f9",
  "recipientclientid": "606d4074c7ea4962aaf3bc2a5ac3b7f9",
  "data": {
    "user": {
      "imsUserId": "ims-933E1F8A631CAA0F0A495E53@80761f6e631c0c7d495fb3.e",
      "principalId": "xx@adobe.com",
      "displayName": "Sachin Mali"
    },
    "path": "/content/dam/wknd-shared/en/adventures/beervana-portland/beervana-in-portland",
    "sourceUrl": "https://author-p63947-e1249010.adobeaemcloud.com",
    "model": {
      "id": "L2NvbmYvd2tuZC1zaGFyZWQvc2V0dGluZ3MvZGFtL2NmbS9tb2RlbHMvYWR2ZW50dXJl",
      "path": "/conf/wknd-shared/settings/dam/cfm/models/adventure"
    },
    "id": "9e1e9835-64c8-42dc-9d36-fbd59e28f753",
    "tags": [
      "wknd-shared:region/nam/united-states",
      "wknd-shared:activity/social",
      "wknd-shared:season/fall"
    ],
    "properties": [
      {
        "name": "price",
        "changeType": "modified"
      }
    ]
  }
}
```

Puoi vedere che i dettagli dell’evento AEM contengono tutte le informazioni necessarie per elaborare l’evento nel webhook. Ad esempio, il tipo di evento (`type`), l&#39;origine evento (`source`), l&#39;ID evento (`event_id`), l&#39;ora evento (`time`) e i dati evento (`data`).

## Risorse aggiuntive

- [Il codice sorgente del webhook con eventi AEM](../assets/examples/webhook/aemeventing-webhook.tgz) è disponibile come riferimento.
