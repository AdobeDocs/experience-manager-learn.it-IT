---
title: Comprendere la condivisione delle risorse tra le origini (CORS) con AEM
description: Adobe Experience Manager Cross-Origin Resource Sharing (CORS) facilita le proprietà Web non AEM per effettuare chiamate lato client a AEM, autenticate e non autenticate, per recuperare contenuto o interagire direttamente con AEM.
version: 6.3, 6,4, 6.5
sub-product: servizi di base, servizi di contenuto, siti
feature: null
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
translation-type: tm+mt
source-git-commit: bc14783840a47fb79ddf1876aca1ef44729d097e
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 1%

---


# Informazioni sulla condivisione delle risorse tra le origini ([!DNL CORS])

La condivisione delle risorse tra le origini di Adobe Experience Manager ([!DNL CORS]) facilita le proprietà Web non AEM per effettuare chiamate lato client a AEM, autenticate e non autenticate, per recuperare il contenuto o per interagire direttamente con AEM.

## Configurazione OSGi del criterio di condivisione delle risorse per  Adobe Granite Cross-Origin

Le configurazioni CORS sono gestite come fabbriche di configurazione OSGi in AEM, con ogni criterio rappresentato come un&#39;istanza del produttore.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Configurazione OSGi del criterio di condivisione delle risorse per  Adobe Granite Cross-Origin](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Selezione criteri

Un criterio viene selezionato confrontando il

* `Allowed Origin` con l’intestazione della  `Origin` richiesta
* e `Allowed Paths` con il percorso della richiesta.

Verrà utilizzato il primo criterio corrispondente a questi valori. Se non ne viene trovata alcuna, qualsiasi richiesta [!DNL CORS] verrà rifiutata.

Se non è configurato alcun criterio, non verrà fornita risposta alle richieste [!DNL CORS] poiché il gestore verrà disabilitato e quindi rifiutato in modo efficace, purché nessun altro modulo del server risponda a [!DNL CORS].

### Proprietà dei criteri

#### [!UICONTROL Origini consentite]

* `"alloworigin" <origin> | *`
* Elenco di parametri `origin` che specificano gli URI che possono accedere alla risorsa. Per le richieste senza credenziali, il server può specificare * come carattere jolly, consentendo a qualsiasi origine di accedere alla risorsa. *È assolutamente sconsigliato utilizzare  `Allow-Origin: *` in produzione, in quanto consente a ogni sito web straniero (cioè attaccante) di fare richieste che senza CORS sono rigorosamente vietati dai browser.*

#### [!UICONTROL Origini Consentite (Regexp)]

* `"alloworiginregexp" <regexp>`
* Elenco di espressioni regolari `regexp` che specificano gli URI che possono accedere alla risorsa. *Le espressioni regolari possono causare corrispondenze non intenzionali se non vengono create con attenzione, consentendo a un utente malintenzionato di utilizzare un nome di dominio personalizzato che corrisponda anche al criterio.* È generalmente consigliato disporre di criteri separati per ciascun nome host di origine specifico, utilizzando  `alloworigin`, anche se ciò significa una configurazione ripetuta delle altre proprietà del criterio. Le origini diverse tendono ad avere cicli di vita e requisiti diversi, beneficiando quindi di una netta separazione.

#### [!UICONTROL Percorsi consentiti]

* `"allowedpaths" <regexp>`
* Elenco di espressioni regolari `regexp` che specificano i percorsi di risorse per i quali viene applicato il criterio.

#### [!UICONTROL Intestazioni esposte]

* `"exposedheaders" <header>`
* Elenco di parametri di intestazione che indicano le intestazioni di richiesta a cui i browser possono accedere.

#### [!UICONTROL Età massima]

* `"maxage" <seconds>`
* Un parametro `seconds` che indica per quanto tempo i risultati di una richiesta di verifica preliminare possono essere memorizzati nella cache.

#### [!UICONTROL Intestazioni supportate]

* `"supportedheaders" <header>`
* Elenco di parametri `header` che indicano quali intestazioni HTTP possono essere utilizzate per effettuare la richiesta effettiva.

#### [!UICONTROL Metodi consentiti]

* `"supportedmethods"`
* Elenco di parametri di metodo che indicano quali metodi HTTP possono essere utilizzati per effettuare la richiesta effettiva.

#### [!UICONTROL Supporta le credenziali]

* `"supportscredentials" <boolean>`
* Un `boolean` che indica se la risposta alla richiesta può essere esposta o meno al browser. Se utilizzata come parte di una risposta a una richiesta di verifica preliminare, ciò indica se la richiesta effettiva può essere effettuata utilizzando le credenziali.

### Configurazioni di esempio

Il sito 1 è uno scenario di base, anonimamente accessibile e di sola lettura in cui il contenuto viene consumato tramite [!DNL GET] richieste:

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

A partire da Dispatcher 4.1.1+ le intestazioni di risposta possono essere memorizzate nella cache. Questo consente di memorizzare nella cache le intestazioni [!DNL CORS] con le risorse [!DNL CORS] richieste, a condizione che la richiesta sia anonima.

Generalmente, le stesse considerazioni per il caching del contenuto in Dispatcher possono essere applicate alle intestazioni di risposta CORS nella cache del dispatcher. La tabella seguente definisce quando è possibile memorizzare nella cache le intestazioni [!DNL CORS] (e quindi le richieste [!DNL CORS]).

| Registrabile | di authoring | Stato autenticazione | Spiegazione |
|-----------|-------------|-----------------------|-------------|
| No | AEM Publish | Autenticato | Il caching del dispatcher su AEM Author è limitato alle risorse statiche non create. Ciò rende difficile e poco pratico memorizzare nella cache la maggior parte delle risorse su AEM Author, comprese le intestazioni delle risposte HTTP. |
| No | AEM Publish | Autenticato | Evitate di memorizzare nella cache le intestazioni CORS sulle richieste autenticate. Ciò si allinea alle linee guida comuni per non memorizzare nella cache le richieste autenticate, in quanto è difficile determinare in che modo lo stato di autenticazione/autorizzazione dell&#39;utente richiedente influirà sulla risorsa consegnata. |
| Sì | AEM Publish | Anonimo | Le richieste anonime che possono essere memorizzate nella cache del dispatcher possono contenere anche le relative intestazioni di risposta, garantendo che le future richieste CORS possano accedere al contenuto memorizzato nella cache. Qualsiasi modifica alla configurazione CORS in AEM Publish **deve essere seguita da un&#39;annullamento della validità delle risorse memorizzate nella cache.** Le procedure ottimali impongono l&#39;eliminazione della cache del dispatcher nelle distribuzioni di codice o di configurazione, in quanto è difficile determinare quale contenuto memorizzato nella cache potrebbe essere eseguito. |

Per consentire il caching delle intestazioni CORS, aggiungete la seguente configurazione a tutti i file AEM Publish dispatcher.any che supportano.

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

Ricordare di **riavviare l&#39;applicazione server Web** dopo aver apportato modifiche al file `dispatcher.any`.

Probabilmente la cancellazione completa della cache sarà necessaria per garantire che le intestazioni siano correttamente memorizzate nella cache alla richiesta successiva dopo un aggiornamento di configurazione `/clientheaders`.

## Risoluzione dei problemi CORS

La registrazione è disponibile in `com.adobe.granite.cors`:

* abilitare `DEBUG` per visualizzare i dettagli sul motivo per cui una richiesta [!DNL CORS] è stata rifiutata
* abilitare `TRACE` per visualizzare i dettagli su tutte le richieste che passano attraverso il gestore CORS

### Suggerimenti:

* ricreare manualmente le richieste XHR utilizzando l&#39;espressione curl, ma assicurarsi di copiare tutte le intestazioni e i dettagli, in quanto ciascuna di esse può fare la differenza; alcune console del browser consentono di copiare il comando curl
* Verifica se la richiesta è stata rifiutata dal gestore CORS e non dall&#39;autenticazione, dal filtro token CSRF, dai filtri dispatcher o da altri livelli di protezione
   * Se il gestore CORS risponde con 200, ma l&#39;intestazione `Access-Control-Allow-Origin` è assente nella risposta, controllare i file di registro per verificare la presenza di eventuali negazioni sotto [!DNL DEBUG] in `com.adobe.granite.cors`
* Se è abilitato il caching del dispatcher delle richieste [!DNL CORS]
   * Verificare che la configurazione `/clientheaders` sia applicata a `dispatcher.any` e che il server Web sia riavviato correttamente
   * Verificare che la cache sia stata cancellata correttamente dopo qualsiasi modifica OSGi o dispatcher.
* se necessario, controllate la presenza delle credenziali di autenticazione nella richiesta.

## Materiali di supporto

* [AEM fabbrica di configurazione OSGi per i criteri di condivisione delle risorse tra origini](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Condivisione risorse tra le origini (W3C)](https://www.w3.org/TR/cors/)
* [Controllo degli accessi HTTP (MDN Mozilla)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
