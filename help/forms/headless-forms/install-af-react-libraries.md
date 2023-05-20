---
title: Installare le librerie di reazione dei moduli adattivi richieste
description: Aggiungi le dipendenze richieste al progetto react
feature: Adaptive Forms
version: 6.5
kt: 13285
topic: Development
role: User
level: Intermediate
source-git-commit: c6e83a627743c40355559d9cdbca2b70db7f23ed
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 1%

---


# Installazione delle dipendenze richieste

Per iniziare a utilizzare i moduli adattivi headless nel progetto react, installa le dipendenze seguenti nel progetto react

* @aemforms/af-react-components
* @aemforms/af-react-renderer

Aggiorna il file package.json per includere le dipendenze seguenti. Al momento della stesura di 0.22.41 era la versione corrente

```json
"@aemforms/af-react-components": "^0.22.41",
"@aemforms/af-react-renderer": "^0.22.41",
```

## Imposta proxy

La condivisione CORS (Cross-Origin Resource Sharing) è un meccanismo di sicurezza che impedisce ai browser web di effettuare richieste a un dominio diverso da quello su cui è ospitata l’app. Possono verificarsi errori CORS quando si tenta di recuperare dati da un’API ospitata su un dominio diverso. Configurando un proxy, puoi aggirare le restrizioni CORS e effettuare richieste all’API dall’app React. Ho utilizzato il seguente codice in un file denominato setUpProxy.js nella cartella src. **Assicurati di modificare la destinazione in modo che punti all’istanza Publish.**

```
const { createProxyMiddleware } = require('http-proxy-middleware');
const proxy = {
    target: 'https://mypublishinstance:4503/',
    changeOrigin: true
}
module.exports = function(app) {
  app.use(
    '/adobe',
    createProxyMiddleware(proxy)
  ),
  app.use(
    '/content',
    createProxyMiddleware(proxy)
  );
};
```

È inoltre necessario installare e aggiungere **http-proxy-middleware** al progetto.

## Passaggi successivi

[Recupera il modulo da incorporare](./fetch-the-form.md)