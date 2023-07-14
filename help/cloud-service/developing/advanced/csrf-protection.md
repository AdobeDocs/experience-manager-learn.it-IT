---
title: Protezione CSRF
description: Scopri come generare e aggiungere token AEM CSRF alle richieste consentite di POST, PUT ed Delete all’AEM per gli utenti autenticati.
version: Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-07-14T00:00:00Z
jira: KT-13651
thumbnail: KT-13651.jpeg
source-git-commit: b5f6314e0462b952b8749a766f55f34af625eb07
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---


# Protezione CSRF

Scopri come generare e aggiungere token AEM CSRF alle richieste consentite di POST, PUT ed Delete all’AEM per gli utenti autenticati.

AEM richiede un token CSRF valido per l’invio di __autenticato__ __POST__, __PUT o __DELETE__ Richieste HTTP ai servizi Author e Publish di AEM.

Il token CSRF non è richiesto per __GET__ richieste, oppure __anonimo__ richieste.

Se un token CSRF non viene inviato con una richiesta POST, PUT o DELETE, l’AEM restituisce una risposta 403 Forbidden e l’AEM registra il seguente errore:

```log
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] isValidRequest: empty CSRF token - rejecting
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] doFilter: the provided CSRF token is invalid
```

Consulta la [documentazione per maggiori dettagli sulla protezione da AEM CSRF](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html).


## Libreria client CSRF

AEM fornisce una libreria client che può essere utilizzata per generare e aggiungere token CSRF XHR e richieste di POST, tramite l’applicazione di patch alle funzioni principali del prototipo. La funzionalità è fornita da `granite.csrf.standalone` categoria di librerie client.

Per utilizzare questo approccio, aggiungi `granite.csrf.standalone` come dipendenza dal caricamento della libreria client sulla pagina. Ad esempio, se utilizzi il `wknd.site` categoria libreria client, aggiungi `granite.csrf.standalone` come dipendenza dal caricamento della libreria client sulla pagina.

## Invio di moduli personalizzati con protezione CSRF

Se l&#39;uso di [`granite.csrf.standalone` libreria client](#csrf-client-library) non è adatto al tuo caso d’uso, puoi aggiungere manualmente un token CSRF a un invio di modulo. L’esempio seguente mostra come aggiungere un token CSRF all’invio di un modulo.

Questo frammento di codice illustra come, all’invio del modulo, il token CSRF può essere recuperato dall’AEM e aggiunto a un input di modulo denominato `:cq_csrf_token`. Poiché il token CSRF ha una durata breve, è consigliabile recuperarlo e impostarlo immediatamente prima dell’invio del modulo, per garantirne la validità.

```javascript
// Attach submit handler event to form onSubmit
document.querySelector('form').addEventListener('submit', async (event) => {
    event.preventDefault();

    const form = event.target;
    const response = await fetch('/libs/granite/csrf/token.json');
    const json = await response.json();
    
    // Create a form input named ``:cq_csrf_token`` with the CSRF token.
    let csrfTokenInput = form.querySelector('input[name=":cq_csrf_token"]');
    if (!csrfTokenInput?.value) {
        // If the form does not have a CSRF token input, add one.
        form.insertAdjacentHTML('afterend', `<input type="hidden" name=":cq_csrf_token" value="${json.token}">`);
    } else {
        // If the form already has a CSRF token input, update the value.
        csrfTokenInput.value = json.token;
    }
    // Submit the form with the hidden input containing the CSRF token
    form.submit();
});
```

## Recupera con protezione CSRF

Se l&#39;uso di [`granite.csrf.standalone` libreria client](#csrf-client-library) non è adatto al tuo caso d’uso, puoi aggiungere manualmente un token CSRF a una richiesta XHR o fetch. L’esempio seguente mostra come aggiungere un token CSRF a un XHR creato con il recupero.

Questo frammento di codice illustra come recuperare un token CSRF dall’AEM e aggiungerlo a quello di una richiesta di recupero `CSRF-Token` Intestazione della richiesta HTTP. Poiché il token CSRF ha una durata breve, è consigliabile recuperarlo e impostarlo immediatamente prima che venga effettuata la richiesta di recupero, garantendone la validità.

```javascript
/**
 * Get CSRF token from AEM.
 * CSRF token expire after a few minutes, so it is best to get the token before each request.
 * 
 * @returns {Promise<string>} that resolves to the CSRF token.
 */
async function getCsrfToken() {
    const response = await fetch('/libs/granite/csrf/token.json');
    const json = await response.json();
    return json.token;
}

// Fetch from AEM with CSRF token in a header named 'CSRF-Token'.
await fetch('/path/to/aem/endpoint', {
    method: 'POST',
    headers: {
        'CSRF-Token': await getCsrfToken()
    },
});
```

## Configurazione del Dispatcher

Quando si utilizzano i token CSRF nel servizio AEM Publish, è necessario aggiornare la configurazione di Dispatcher per consentire le richieste di GET all’endpoint del token CSRF. La seguente configurazione consente le richieste GET all’endpoint token CSRF nel servizio AEM Publish. Se questa configurazione non viene aggiunta, l’endpoint del token CSRF restituisce una risposta 404 Not Found.

* `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
/0120 { /type "allow" /method "GET" /url "/libs/granite/csrf/token.json" }
...
```
