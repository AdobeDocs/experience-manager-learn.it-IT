---
title: Protezione CSRF
description: Scopri come generare e aggiungere token AEM CSRF alle richieste POST, PUT ed Delete consentite ad AEM per gli utenti autenticati.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-07-14T00:00:00Z
jira: KT-13651
thumbnail: KT-13651.jpeg
exl-id: 747322ed-f01a-48ba-a4a0-483b81f1e904
duration: 125
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 0%

---

# Protezione CSRF

Scopri come generare e aggiungere token AEM CSRF alle richieste POST, PUT ed Delete consentite ad AEM per gli utenti autenticati.

AEM richiede l&#39;invio di un token CSRF valido per __richieste HTTP autenticate__ __POST__, __PUT o __DELETE__ sia al servizio Author che Publish di AEM.

Il token CSRF non è necessario per __richieste GET__ o __richieste anonime__.

Se un token CSRF non viene inviato con una richiesta POST, PUT o DELETE, AEM restituisce una risposta 403 Forbidden e AEM registra il seguente errore:

```log
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] isValidRequest: empty CSRF token - rejecting
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] doFilter: the provided CSRF token is invalid
```

Per ulteriori informazioni sulla protezione CSRF di AEM[&#128279;](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html), consulta la documentazione di .


## Libreria client CSRF

AEM fornisce una libreria client che può essere utilizzata per generare e aggiungere token CSRF XHR e formare richieste POST, tramite l’applicazione di patch alle funzioni principali del prototipo. La funzionalità è fornita dalla categoria libreria client `granite.csrf.standalone`.

Per utilizzare questo approccio, aggiungi `granite.csrf.standalone` come dipendenza al caricamento della libreria client sulla pagina. Ad esempio, se utilizzi la categoria di librerie client `wknd.site`, aggiungi `granite.csrf.standalone` come dipendenza al caricamento della libreria client sulla pagina.

## Invio di moduli personalizzati con protezione CSRF

Se l&#39;utilizzo della libreria client [`granite.csrf.standalone`](#csrf-client-library) non è compatibile con il caso d&#39;uso, è possibile aggiungere manualmente un token CSRF all&#39;invio di un modulo. L’esempio seguente mostra come aggiungere un token CSRF all’invio di un modulo.

Questo frammento di codice illustra come, all&#39;invio del modulo, il token CSRF può essere recuperato da AEM e aggiunto a un input di modulo denominato `:cq_csrf_token`. Poiché il token CSRF ha una durata breve, è consigliabile recuperarlo e impostarlo immediatamente prima dell’invio del modulo, per garantirne la validità.

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
        form.insertAdjacentHTML('beforeend', `<input type="hidden" name=":cq_csrf_token" value="${json.token}">`);
    } else {
        // If the form already has a CSRF token input, update the value.
        csrfTokenInput.value = json.token;
    }
    // Submit the form with the hidden input containing the CSRF token
    form.submit();
});
```

## Recupera con protezione CSRF

Se l&#39;utilizzo della libreria client [`granite.csrf.standalone`](#csrf-client-library) non è compatibile con il caso d&#39;uso, è possibile aggiungere manualmente un token CSRF a una richiesta XHR o fetch. L’esempio seguente mostra come aggiungere un token CSRF a un XHR creato con il recupero.

Questo frammento di codice illustra come recuperare un token CSRF da AEM e aggiungerlo all&#39;intestazione di richiesta HTTP `CSRF-Token` di una richiesta di recupero. Poiché il token CSRF ha una durata breve, è consigliabile recuperarlo e impostarlo immediatamente prima che venga effettuata la richiesta di recupero, garantendone la validità.

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

## Configurazione Dispatcher

Quando si utilizzano i token CSRF nel servizio AEM Publish, è necessario aggiornare la configurazione di Dispatcher per consentire le richieste di GET all’endpoint del token CSRF. La seguente configurazione consente le richieste di GET all’endpoint del token CSRF nel servizio AEM Publish. Se questa configurazione non viene aggiunta, l’endpoint del token CSRF restituisce una risposta 404 Not Found.

* `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
/0120 { /type "allow" /method "GET" /url "/libs/granite/csrf/token.json" }
...
```
