---
title: Comprendere la condivisione di risorse tra le origini (CORS) con AEM
description: La condivisione delle risorse tra le origini di Adobe Experience Manager (CORS) facilita le proprietà web non AEM per effettuare chiamate lato client ad AEM, sia autenticate che non autenticate, per recuperare contenuti o per interagire direttamente con AEM.
version: 6.3, 6,4, 6.5
sub-product: fondazione, content-services, siti
feature: null
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 1%

---


# Comprendere la condivisione risorse tra origini ([!DNL CORS])

La condivisione delle risorse tra le origini di Adobe Experience Manager ([!DNL CORS]) facilita le proprietà web non AEM per effettuare chiamate lato client ad AEM, autenticate e non autenticate, per recuperare contenuti o per interagire direttamente con AEM.

## Configurazione OSGi dei criteri di condivisione risorse multiorigine di Adobe Granite

Le configurazioni CORS vengono gestite come fabbriche di configurazione OSGi in AEM, e ogni criterio viene rappresentato come un’istanza della fabbrica.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Configurazione OSGi dei criteri di condivisione risorse multiorigine di Adobe Granite](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Selezione criteri

Per selezionare un criterio, confronta

* `Allowed Origin` con l’intestazione della  `Origin` richiesta
* e `Allowed Paths` con il percorso della richiesta.

Verrà utilizzato il primo criterio corrispondente a questi valori. Se non viene trovato nessuno, qualsiasi richiesta [!DNL CORS] verrà negata.

Se non è configurato alcun criterio, alle richieste [!DNL CORS] non verrà data risposta poiché il gestore verrà disabilitato e quindi negato in modo efficace, purché nessun altro modulo del server risponda a [!DNL CORS].

### Proprietà dei criteri

#### [!UICONTROL Origini consentite]

* `"alloworigin" <origin> | *`
* Elenco di parametri `origin` che specificano gli URI che possono accedere alla risorsa. Per le richieste senza credenziali, il server può specificare * come carattere jolly, consentendo così a qualsiasi origine di accedere alla risorsa. *È assolutamente sconsigliato utilizzare  `Allow-Origin: *` in produzione in quanto consente a ogni sito web straniero (cioè aggressore) di fare richieste che senza CORS sono severamente vietati dai browser.*

#### [!UICONTROL Origini consentite (Regexp)]

* `"alloworiginregexp" <regexp>`
* Elenco di espressioni regolari `regexp` che specificano gli URI che possono accedere alla risorsa. *Le espressioni regolari possono causare corrispondenze non desiderate se non vengono create con cura, consentendo a un autore dell’attacco di utilizzare un nome di dominio personalizzato che corrisponda anche al criterio.* È generalmente consigliato disporre di criteri separati per ogni nome host di origine specifico, utilizzando  `alloworigin`, anche se ciò significa una configurazione ripetuta delle altre proprietà dei criteri. Le diverse origini tendono ad avere cicli di vita e requisiti diversi, beneficiando così di una netta separazione.

#### [!UICONTROL Percorsi consentiti]

* `"allowedpaths" <regexp>`
* Elenco di espressioni regolari `regexp` che specificano i percorsi delle risorse per i quali il criterio si applica.

#### [!UICONTROL Intestazioni esposte]

* `"exposedheaders" <header>`
* Elenco di parametri di intestazione che indicano le intestazioni di richiesta a cui i browser possono accedere.

#### [!UICONTROL Età massima]

* `"maxage" <seconds>`
* Un parametro `seconds` che indica per quanto tempo i risultati di una richiesta di pre-volo possono essere memorizzati nella cache.

#### [!UICONTROL Intestazioni supportate]

* `"supportedheaders" <header>`
* Elenco di parametri `header` che indicano quali intestazioni HTTP possono essere utilizzate quando si effettua la richiesta effettiva.

#### [!UICONTROL Metodi consentiti]

* `"supportedmethods"`
* Elenco di parametri del metodo che indicano quali metodi HTTP possono essere utilizzati per effettuare la richiesta effettiva.

#### [!UICONTROL Supporta le credenziali]

* `"supportscredentials" <boolean>`
* Un `boolean` che indica se la risposta alla richiesta può essere esposta o meno al browser. Se utilizzato come parte di una risposta a una richiesta di pre-volo, indica se la richiesta effettiva può essere effettuata o meno utilizzando le credenziali.

### Configurazioni di esempio

Il sito 1 è uno scenario di base, anonimamente accessibile e di sola lettura in cui il contenuto viene consumato tramite richieste [!DNL GET]:

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

## Problemi e configurazione della cache del dispatcher {#dispatcher-caching-concerns-and-configuration}

A partire da Dispatcher 4.1.1+ le intestazioni di risposta possono essere memorizzate nella cache. Questo consente di memorizzare in cache le intestazioni [!DNL CORS] insieme alle risorse [!DNL CORS] richieste, purché la richiesta sia anonima.

Generalmente, le stesse considerazioni per la memorizzazione in cache del contenuto in Dispatcher possono essere applicate alla memorizzazione in cache delle intestazioni di risposta CORS sul dispatcher. La tabella seguente definisce quando è possibile memorizzare nella cache le intestazioni [!DNL CORS] (e quindi le richieste [!DNL CORS]).

| In cache | di authoring | Stato di autenticazione | Spiegazione |
|-----------|-------------|-----------------------|-------------|
| No | AEM Publish | Autenticato | Il caching del Dispatcher su AEM Author è limitato alle risorse statiche e non create. Questo rende difficile e poco pratico memorizzare nella cache la maggior parte delle risorse su AEM Author, comprese le intestazioni di risposta HTTP. |
| No | Pubblicazione AEM | Autenticato | Evita di memorizzare in cache le intestazioni CORS sulle richieste autenticate. Questo si allinea alle linee guida comuni per non memorizzare in cache le richieste autenticate, in quanto è difficile determinare in che modo lo stato di autenticazione/autorizzazione dell’utente richiedente influirà sulla risorsa consegnata. |
| Sì | Pubblicazione AEM | Anonimo | Le richieste anonime che possono essere memorizzate nella cache del dispatcher possono avere anche le loro intestazioni di risposta memorizzate nella cache, garantendo che le future richieste CORS possano accedere al contenuto memorizzato nella cache. Eventuali modifiche alla configurazione CORS in AEM Publish **devono essere seguite da un&#39;invalidazione delle risorse memorizzate nella cache interessate.** Le best practice determinano le implementazioni di codice o di configurazione che la cache del dispatcher viene eliminata, in quanto è difficile determinare quale contenuto memorizzato nella cache può essere eseguito. |

Per consentire il caching delle intestazioni CORS, aggiungi la seguente configurazione a tutti i file AEM Publish dispatcher.any che supportano.

```
/cache { 
  ...
  /clientheaders {
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

Ricorda di **riavviare l&#39;applicazione server Web** dopo aver apportato modifiche al file `dispatcher.any`.

È probabile che la cancellazione completa della cache sarà necessaria per garantire che le intestazioni siano correttamente memorizzate nella cache nella richiesta successiva dopo un `/clientheaders` aggiornamento della configurazione.

## Risoluzione dei problemi CORS

La registrazione è disponibile in `com.adobe.granite.cors`:

* abilita `DEBUG` per visualizzare i dettagli sul motivo per cui una richiesta [!DNL CORS] è stata negata
* abilita `TRACE` per visualizzare i dettagli su tutte le richieste che passano attraverso il gestore CORS

### Suggerimenti:

* ricreare manualmente le richieste XHR utilizzando curl, ma assicurati di copiare tutte le intestazioni e i dettagli, in quanto ciascuno può fare la differenza; alcune console del browser consentono di copiare il comando curl
* Verifica se la richiesta è stata negata dal gestore CORS e non dall’autenticazione, dal filtro token CSRF, dai filtri del dispatcher o da altri livelli di sicurezza
   * Se il gestore CORS risponde con 200, ma l&#39;intestazione `Access-Control-Allow-Origin` è assente nella risposta, controlla i registri per eventuali rinunce sotto [!DNL DEBUG] in `com.adobe.granite.cors`
* Se è abilitato il caching del dispatcher per le richieste [!DNL CORS]
   * Assicurati che la configurazione `/clientheaders` sia applicata a `dispatcher.any` e che il server web sia riavviato correttamente
   * Assicurati che la cache sia stata svuotata correttamente dopo eventuali modifiche di configurazione di OSGi o dispatcher.any.
* se necessario, controlla la presenza delle credenziali di autenticazione nella richiesta.

## Materiali di supporto

* [Fabbrica di configurazione AEM OSGi per i criteri di condivisione delle risorse tra origini diverse](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Condivisione risorse tra le origini (W3C)](https://www.w3.org/TR/cors/)
* [Controllo degli accessi HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
