---
title: Comprendere la condivisione di risorse tra origini diverse (CORS) con AEM
description: La condivisione di risorse tra origini diverse (CORS) di Adobe Experience Manager consente alle proprietà web diverse da AEM di effettuare chiamate lato client ad AEM, sia autenticate che non autenticate, per recuperare contenuto o interagire direttamente con AEM.
version: Experience Manager 6.4, Experience Manager 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: Security, APIs
doc-type: Article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
duration: 240
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1011'
ht-degree: 100%

---

# Comprendere la condivisione di risorse tra origini diverse ([!DNL CORS])

La condivisione di risorse tra origini diverse ([!DNL CORS]) di Adobe Experience Manager consente alle proprietà web diverse da AEM di effettuare chiamate lato client ad AEM, sia autenticate che non autenticate, per recuperare contenuto o interagire direttamente con AEM.

La configurazione OSGI descritta in questo documento è sufficiente per:

1. Condivisione delle risorse da un’unica origine in Pubblicazione AEM
2. Accesso CORS all’Authoring AEM

Se in Pubblicazione AEM è richiesto l’accesso CORS per più origin, consulta [questa documentazione](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=it#dispatcher-configuration).

## Configurazione OSGi dei criteri di condivisione di risorse tra origini diverse di Adobe Granite

Le configurazioni CORS sono gestite come factory di configurazione OSGi in AEM, dove ogni criterio è rappresentato come un’istanza della factory.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Configurazione OSGi dei criteri di condivisione di risorse tra origini diverse di Adobe Granite](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Selezione dei criteri

Un criterio viene selezionato confrontando

* `Allowed Origin` con l’intestazione della richiesta `Origin`
* e `Allowed Paths` con il percorso della richiesta.

Viene utilizzato il primo criterio che corrisponde a questi valori. Se non viene trovato alcun criterio, qualsiasi richiesta [!DNL CORS] verrà negata.

Se non è configurato alcun criterio, non verrà fornita risposta neanche alle richieste [!DNL CORS] perché il gestore è disabilitato e quindi negato in modo effettivo, purché nessun altro modulo del server risponda a [!DNL CORS].

### Proprietà dei criteri

#### [!UICONTROL Origini consentite]

* `"alloworigin" <origin> | *`
* Elenco di parametri `origin` che specificano gli URI che possono accedere alla risorsa. Per le richieste senza credenziali, il server può specificare &#42; come carattere jolly, consentendo in tal modo a qualsiasi origine di accedere alla risorsa. *Non è assolutamente consigliabile utilizzare `Allow-Origin: *` in produzione, in quanto consente a qualsiasi sito web esterno (ad esempio, un hacker) di effettuare richieste che senza CORS sono severamente vietate dai browser.*

#### [!UICONTROL Origini consentite (RegExp)]

* `"alloworiginregexp" <regexp>`
* Elenco di espressioni regolari `regexp` che specificano gli URI che possono accedere alla risorsa. *Le espressioni regolari possono causare corrispondenze indesiderate, se non vengono create con attenzione, consentendo a un hacker di utilizzare un nome di dominio personalizzato che potrebbe anch’esso corrispondere al criterio.* In genere si consiglia di disporre di criteri separati per ciascun nome host di origine specifico, utilizzando `alloworigin`, anche se ciò comporta la ripetizione della configurazione delle altre proprietà dei criteri. Origini diverse tendono ad avere cicli di vita e requisiti diversi, beneficiando così di una netta separazione.

#### [!UICONTROL Percorsi consentiti]

* `"allowedpaths" <regexp>`
* Elenco di espressioni regolari `regexp` che specificano i percorsi delle risorse a cui si applica il criterio.

#### [!UICONTROL Intestazioni esposte]

* `"exposedheaders" <header>`
* Elenco di parametri di intestazione che indicano le intestazioni di risposta a cui i browser possono accedere. Per le richieste CORS (non pre-flight), se non sono vuote, questi valori vengono copiati nell’intestazione di risposta `Access-Control-Expose-Headers`. I valori nell’elenco (nomi delle intestazioni) vengono quindi resi accessibili al browser; senza di esso, tali intestazioni non sono leggibili dal browser.

#### [!UICONTROL Età massima]

* `"maxage" <seconds>`
* Un parametro `seconds` che indica per quanto tempo è possibile memorizzare nella cache i risultati di una richiesta pre-flight.

#### [!UICONTROL Intestazioni supportate]

* `"supportedheaders" <header>`
* Elenco dei parametri `header` che indicano quali intestazioni di richiesta HTTP possono essere utilizzate durante l’esecuzione della richiesta effettiva.

#### [!UICONTROL Metodi consentiti]

* `"supportedmethods"`
* Elenco dei parametri di metodo che indicano quali metodi HTTP possono essere utilizzati durante l’esecuzione della richiesta effettiva.

#### [!UICONTROL Credenziali di supporto]

* `"supportscredentials" <boolean>`
* Un `boolean` che indica se la risposta alla richiesta può essere esposta al browser. Se utilizzato come parte di una risposta a una richiesta di pre-flight, indica se la richiesta effettiva può essere effettuata o meno utilizzando le credenziali.

### Configurazioni di esempio

Il sito 1 è uno scenario di base, accessibile in modo anonimo e di sola lettura il cui contenuto viene utilizzato tramite richieste [!DNL GET]:

```json
{
  "supportscredentials":false,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
  ],
  "alloworigin":[
    "http://127.0.0.1:3000",
    "https://site1.com"
    
  ],
  "maxage:Integer": 1800,
  "alloworiginregexp":[
    "http://localhost:.*"
    "https://.*\.site1\.com"
  ],
  "allowedpaths":[
    "/content/_cq_graphql/site1/endpoint.json",
    "/graphql/execute.json.*",
    "/content/site1/.*"
  ],
  "supportedheaders":[
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers"
  ]
}
```

Il sito 2 è più complesso e necessita di richieste autorizzate e mutevoli (POST, PUT, DELETE):

```json
{
  "supportscredentials":true,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
    "POST",
    "DELETE",
    "PUT"
  ],
  "alloworigin":[
    "http://127.0.0.1:3000",
    "https://site2.com"
    
  ],
  "maxage:Integer": 1800,
  "alloworiginregexp":[
    "http://localhost:.*"
    "https://.*\.site2\.com"
  ],
  "allowedpaths":[
    "/content/site2/.*",
    "/libs/granite/csrf/token.json",
  ],
  "supportedheaders":[
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization",
    "CSRF-Token"
  ]
}
```

## Problemi e configurazione della memorizzazione in cache di Dispatcher {#dispatcher-caching-concerns-and-configuration}

A partire da Dispatcher 4.1.1+, le intestazioni di risposta possono essere memorizzate nella cache. Questo rende possibile memorizzare nella cache le intestazioni [!DNL CORS] insieme alle risorse richieste da [!DNL CORS], purché la richiesta sia anonima.

In generale, le stesse considerazioni per la memorizzazione in cache del contenuto in Dispatcher possono essere applicate alla memorizzazione in cache delle intestazioni di risposta CORS in Dispatcher. La tabella seguente definisce quando è possibile memorizzare in cache le intestazioni [!DNL CORS] (e quindi le richieste [!DNL CORS]).

| Memorizzabile in cache | Ambiente | Stato dell’autenticazione | Spiegazione |
|-----------|-------------|-----------------------|-------------|
| No | Pubblicazione AEM | Autenticato | La memorizzazione in cache di Dispatcher nell’Authoring AEM è limitata alle risorse statiche non create. Questo rende difficile e poco pratico memorizzare in cache la maggior parte delle risorse sull’Authoring AEM, incluse le intestazioni di risposta HTTP. |
| No | Pubblicazione AEM | Autenticato | Evita di memorizzare nella cache le intestazioni CORS nelle richieste autenticate. Questo è in linea con il principio guida comune di non memorizzare in cache le richieste autenticate, in quanto è difficile determinare in che modo lo stato di autenticazione/autorizzazione dell’utente richiedente influirà sulla risorsa consegnata. |
| Sì | Pubblicazione AEM | Anonimo | Anche le intestazioni di risposta delle richieste anonime memorizzabili nella cache di Dispatcher possono essere memorizzate nella cache, garantendo che le richieste CORS future possano accedere al contenuto memorizzato nella cache. Qualsiasi modifica alla configurazione CORS nella Pubblicazione AEM **deve** essere seguita da un annullamento della validità delle risorse memorizzate nella cache interessate. Le best practice impongono distribuzioni di codice o configurazione per cui la cache del dispatcher viene eliminata, in quanto è difficile determinare quale contenuto memorizzato in cache può essere interessato. |

### Autorizzare le intestazioni di richiesta CORS

Per consentire alle [intestazioni di richiesta HTTP necessarie per il passthrough ad AEM per l’elaborazione](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=it#specifying-the-http-headers-to-pass-through-clientheaders), le intestazioni devono essere consentite nella configurazione `/clientheaders` di Dispatcher.

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### Memorizzazione in cache le intestazioni di risposta CORS

Per consentire la memorizzazione in cache e la gestione delle intestazioni CORS sul contenuto memorizzato nella cache, aggiungi la seguente [configurazione cache /headers](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=it#caching-http-response-headers) al file `dispatcher.any` di Pubblicazione AEM.

```
/publishfarm {
    ...
    /cache {
        ...
        # CORS HTTP response headers
        # https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_response_headers
        /headers {
            ...
            "Access-Control-Allow-Origin"
            "Access-Control-Expose-Headers"
            "Access-Control-Max-Age"
            "Access-Control-Allow-Credentials"
            "Access-Control-Allow-Methods"
            "Access-Control-Allow-Headers"
        }
    ...
    }
...
}
```

Ricordati di **riavviare l’applicazione server Web** dopo aver apportato modifiche al file `dispatcher.any`.

È probabile che la cancellazione completa della cache sia necessaria per garantire che le intestazioni siano memorizzate nella cache in modo appropriato nella successiva richiesta dopo un aggiornamento della configurazione di `/cache/headers`.

## Risoluzione dei problemi CORS

La registrazione è disponibile in `com.adobe.granite.cors`:

* abilita `DEBUG` per visualizzare i dettagli sul motivo per cui una richiesta [!DNL CORS] è stata negata
* abilita `TRACE` per visualizzare i dettagli di tutte le richieste che passano attraverso il gestore CORS

### Suggerimenti:

* Ricrea manualmente le richieste XHR utilizzando cURL, ma assicurati di copiare tutte le intestazioni e i dettagli, in quanto ciascuno può fare la differenza; alcune console del browser consentono di copiare il comando cURL
* Verifica se la richiesta è stata negata dal gestore CORS e non dall’autenticazione, dal filtro del token CSRF, dai filtri del Dispatcher o da altri livelli di sicurezza
   * Se il gestore CORS risponde con 200, ma l’intestazione `Access-Control-Allow-Origin` è assente nella risposta, esamina i registri per i rifiuti sotto [!DNL DEBUG] in `com.adobe.granite.cors`
* Se la memorizzazione nella cache del Dispatcher delle richieste [!DNL CORS] è abilitata
   * Assicurati che la configurazione `/cache/headers` sia applicata a `dispatcher.any` e che il server web sia stato riavviato correttamente
   * Assicurati che la cache sia stata cancellata correttamente dopo qualsiasi modifica alla configurazione OSGi o a dispatcher.any.
* se necessario, controlla la presenza delle credenziali di autenticazione nella richiesta.

## Materiali di supporto

* [Factory di configurazione OSGI di AEM per i criteri della condivisione CORS (Cross-Orgin Resource Sharing)](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [CORS (Cross-Orgin Resource Sharing) (W3C)](https://www.w3.org/TR/cors/)
* [Controllo degli accessi HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
