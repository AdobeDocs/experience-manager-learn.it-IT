---
title: Comprendere la condivisione delle risorse tra le origini (CORS, Cross Origin Resource Sharing) con AEM
description: La condivisione delle risorse tra le origini di Adobe Experience Manager (Cross-Origin Resource Sharing, CORS) consente di effettuare chiamate lato client a AEM, autenticate e non autenticate, per recuperare contenuti o interagire direttamente con AEM.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
source-git-commit: 2f02a4e202390434de831ce1547001b2cef01562
workflow-type: tm+mt
source-wordcount: '910'
ht-degree: 1%

---

# Comprendere la condivisione delle risorse tra le origini ([!DNL CORS])

Condivisione risorse tra le origini di Adobe Experience Manager ([!DNL CORS]) facilita le proprietà web non AEM per effettuare chiamate a AEM, autenticate e non autenticate, per recuperare contenuti o per interagire direttamente con AEM.

## Configurazione OSGi dei criteri di condivisione risorse multiorigine di Adobe Granite

Le configurazioni CORS sono gestite come fabbriche di configurazione OSGi in AEM, con ogni criterio rappresentato come un&#39;istanza della fabbrica.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Configurazione OSGi dei criteri di condivisione risorse multiorigine di Adobe Granite](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Selezione criteri

Per selezionare un criterio, confronta

* `Allowed Origin` con `Origin` intestazione della richiesta
* e `Allowed Paths` con il percorso della richiesta.

Vengono utilizzati i primi criteri che corrispondono a questi valori. Se non viene trovato nessuno, qualsiasi [!DNL CORS] richiesta negata.

Se non è stato configurato alcun criterio, [!DNL CORS] Le richieste non riceveranno risposta anche in quanto il gestore è disabilitato e quindi effettivamente negato, purché nessun altro modulo del server risponda a [!DNL CORS].

### Proprietà dei criteri

#### [!UICONTROL Origini consentite]

* `"alloworigin" <origin> | *`
* Elenco `origin` parametri che specificano gli URI che possono accedere alla risorsa. Per le richieste senza credenziali, il server può specificare &#42; come carattere jolly, consentendo a qualsiasi origine di accedere alla risorsa. *Si consiglia di non utilizzare `Allow-Origin: *` in produzione, poiché consente a ogni sito web straniero (ad es. attacker) di effettuare richieste che senza CORS sono rigorosamente vietate dai browser.*

#### [!UICONTROL Origini consentite (Regexp)]

* `"alloworiginregexp" <regexp>`
* Elenco `regexp` espressioni regolari che specificano gli URI che possono accedere alla risorsa. *Le espressioni regolari possono causare corrispondenze non desiderate se non vengono create con cura, consentendo a un autore dell’attacco di utilizzare un nome di dominio personalizzato che corrisponda anche al criterio.* Si consiglia generalmente di disporre di criteri separati per ogni nome host di origine specifico, utilizzando `alloworigin`, anche se ciò significa una configurazione ripetuta delle altre proprietà dei criteri. Le diverse origini tendono ad avere cicli di vita e requisiti diversi, beneficiando così di una netta separazione.

#### [!UICONTROL Percorsi consentiti]

* `"allowedpaths" <regexp>`
* Elenco `regexp` espressioni regolari che specificano i percorsi delle risorse per i quali viene applicato il criterio.

#### [!UICONTROL Intestazioni esposte]

* `"exposedheaders" <header>`
* Elenco di parametri di intestazione che indicano le intestazioni di richiesta a cui i browser possono accedere.

#### [!UICONTROL Età massima]

* `"maxage" <seconds>`
* A `seconds` parametro che indica per quanto tempo è possibile memorizzare nella cache i risultati di una richiesta di pre-volo.

#### [!UICONTROL Intestazioni supportate]

* `"supportedheaders" <header>`
* Elenco `header` parametri che indicano quali intestazioni HTTP possono essere utilizzate quando si effettua la richiesta effettiva.

#### [!UICONTROL Metodi consentiti]

* `"supportedmethods"`
* Elenco di parametri del metodo che indicano quali metodi HTTP possono essere utilizzati per effettuare la richiesta effettiva.

#### [!UICONTROL Supporta le credenziali]

* `"supportscredentials" <boolean>`
* A `boolean` che indica se la risposta alla richiesta può essere esposta o meno al browser. Se utilizzato come parte di una risposta a una richiesta di pre-volo, indica se la richiesta effettiva può essere effettuata o meno utilizzando le credenziali.

### Configurazioni di esempio

Il sito 1 è uno scenario di base, anonimamente accessibile e di sola lettura in cui il contenuto viene utilizzato tramite [!DNL GET] richieste:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://site1.com]"
    alloworiginregexp="[]"
    allowedpaths="[/content/site1/.*]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers]"
    supportedmethods="[GET]"
    supportscredentials="{Boolean}false"
/>
```

Il sito 2 è più complesso e richiede richieste autorizzate e non sicure:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://site2.com]"
    alloworiginregexp="[]"
    allowedpaths="[/content/site2/.*,/libs/granite/csrf/token.json]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers,Authorization,CSRF-Token]"
    supportedmethods="[GET,HEAD,POST,DELETE,OPTIONS,PUT]"
    supportscredentials="{Boolean}true"
/>
```

## Problemi e configurazione della memorizzazione in cache di Dispatcher {#dispatcher-caching-concerns-and-configuration}

A partire da Dispatcher 4.1.1+ le intestazioni di risposta possono essere memorizzate nella cache. Questo rende possibile la memorizzazione in cache [!DNL CORS] intestazioni lungo [!DNL CORS]-risorse richieste, purché la richiesta sia anonima.

Generalmente, le stesse considerazioni per la memorizzazione in cache del contenuto in Dispatcher possono essere applicate alla memorizzazione in cache delle intestazioni di risposta CORS sul dispatcher. La tabella seguente definisce quando [!DNL CORS] intestazioni (e quindi [!DNL CORS] richieste) possono essere memorizzate nella cache.

| In cache | Ambiente | Stato di autenticazione | Spiegazione |
|-----------|-------------|-----------------------|-------------|
| No | AEM Publish | Autenticato | Il caching del Dispatcher su AEM Author è limitato alle risorse statiche e non create. Questo rende difficile e poco pratico memorizzare nella cache la maggior parte delle risorse su AEM Author, comprese le intestazioni di risposta HTTP. |
| No | Pubblicazione AEM | Autenticato | Evita di memorizzare in cache le intestazioni CORS sulle richieste autenticate. Questo si allinea alle linee guida comuni per non memorizzare in cache le richieste autenticate, in quanto è difficile determinare in che modo lo stato di autenticazione/autorizzazione dell’utente richiedente influirà sulla risorsa consegnata. |
| Sì | Pubblicazione AEM | Anonimo | Le richieste anonime che possono essere memorizzate nella cache del dispatcher possono avere anche le loro intestazioni di risposta memorizzate nella cache, garantendo che le future richieste CORS possano accedere al contenuto memorizzato nella cache. Qualsiasi modifica alla configurazione CORS su AEM Publish **deve** seguito da un’invalidazione delle risorse memorizzate nella cache interessate. Le best practice determinano le implementazioni di codice o di configurazione che la cache del dispatcher viene eliminata, in quanto è difficile determinare quale contenuto memorizzato nella cache può essere eseguito. |

Per consentire il caching delle intestazioni CORS, aggiungi la seguente configurazione a tutti i file AEM Publish dispatcher.any che supportano.

```
/cache { 
  ...
  /headers {
      "Origin"
      "Access-Control-Allow-Origin"
      "Access-Control-Expose-Headers"
      "Access-Control-Max-Age"
      "Access-Control-Allow-Credentials"
      "Access-Control-Allow-Methods"
      "Access-Control-Allow-Headers"
  }
  ...
}
```

Ricorda di **riavvia l&#39;applicazione server web** dopo aver apportato modifiche al `dispatcher.any` file.

Probabilmente cancella la cache interamente necessaria per garantire che le intestazioni siano correttamente memorizzate nella cache nella richiesta successiva dopo un `/cache/headers` aggiornamento della configurazione.

## Risoluzione dei problemi CORS

La registrazione è disponibile in `com.adobe.granite.cors`:

* abilita `DEBUG` per visualizzare i dettagli del motivo di un [!DNL CORS] richiesta negata
* abilita `TRACE` per visualizzare i dettagli di tutte le richieste che passano attraverso il gestore CORS

### Suggerimenti:

* ricreare manualmente le richieste XHR utilizzando curl, ma assicurati di copiare tutte le intestazioni e i dettagli, in quanto ciascuno può fare la differenza; alcune console del browser consentono di copiare il comando curl
* Verifica se la richiesta è stata negata dal gestore CORS e non dall’autenticazione, dal filtro token CSRF, dai filtri del dispatcher o da altri livelli di sicurezza
   * Se il gestore CORS risponde con 200, ma `Access-Control-Allow-Origin` l&#39;intestazione è assente nella risposta, controlla i registri per le negazioni sotto [!DNL DEBUG] in `com.adobe.granite.cors`
* Se il dispatcher memorizza in cache [!DNL CORS] le richieste sono abilitate
   * Assicurati che `/cache/headers` la configurazione viene applicata a `dispatcher.any` e il server web viene riavviato correttamente
   * Assicurati che la cache sia stata svuotata correttamente dopo eventuali modifiche di configurazione di OSGi o dispatcher.any.
* se necessario, controlla la presenza delle credenziali di autenticazione nella richiesta.

## Materiali di supporto

* [AEM fabbrica di configurazione OSGi per i criteri di condivisione delle risorse tra origini diverse](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Condivisione risorse tra le origini (W3C)](https://www.w3.org/TR/cors/)
* [Controllo degli accessi HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
