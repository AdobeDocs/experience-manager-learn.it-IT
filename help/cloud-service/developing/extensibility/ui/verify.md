---
title: Verificare un'estensione dell'interfaccia utente AEM
description: Scopri come visualizzare in anteprima, testare e verificare un’estensione dell’interfaccia utente AEM prima di distribuirla in produzione.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603, KT-13382
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: c5c1df23-1c04-4c04-b0cd-e126c31d5acc
source-git-commit: 6b5c755bd8fe6bbf497895453b95eb236f69d5f6
workflow-type: tm+mt
source-wordcount: '719'
ht-degree: 0%

---

# Verificare un’estensione

Le estensioni dell’interfaccia utente AEM possono essere verificate in base a qualsiasi ambiente as a Cloud Service AEM nell’organizzazione di Adobi a cui appartiene l’estensione.

Il test di un’estensione viene eseguito tramite un URL appositamente creato che indica all’AEM di caricarla, solo per tale richiesta.

>[!VIDEO](https://video.tv.adobe.com/v/3412877?quality=12&learn=on)

>[!IMPORTANT]
>
> Il video precedente illustra l’utilizzo di un’estensione della Console Frammenti di contenuto per illustrare l’anteprima e la verifica dell’app dell’estensione App Builder. Tuttavia, è importante notare che i concetti descritti possono essere applicati a tutte le estensioni dell’interfaccia utente dell’AEM.

## URL interfaccia utente AEM

![URL della console Frammenti di contenuto AEM](./assets/verify/content-fragment-console-url.png){align="center"}

Per creare un URL che monti l’estensione non di produzione in AEM, è necessario ottenere l’URL dell’interfaccia utente dell’AEM in cui viene inserita l’estensione. Passa all’ambiente AEM as a Cloud Service per verificare l’estensione su e apri l’interfaccia utente su cui deve essere visualizzata l’estensione.

Ad esempio, per visualizzare in anteprima un’estensione per la console Frammenti di contenuto:

1. Accedi all’ambiente AEM as a Cloud Service desiderato.
2. Seleziona la __Frammenti di contenuto__ icona.
3. Attendi che la console Frammenti di contenuto AEM venga caricata nel browser.
4. Copia l’URL della console Frammenti di contenuto AEM dalla barra degli indirizzi del browser; dovrebbe essere simile al seguente:

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

Questo URL viene utilizzato di seguito per la creazione degli URL per lo sviluppo e la verifica dell’area di visualizzazione. Se verifichi l’estensione rispetto ad altre interfacce utente dell’AEM, ottieni tali URL e applica gli stessi passaggi indicati di seguito.

## Verificare le build di sviluppo locali

1. Apri una riga di comando nella directory principale del progetto di estensione.
1. Eseguire l’estensione dell’interfaccia utente AEM come app locale di App Builder

   ```shell
   $ aio app run
   ...
   No change to package.json was detected. No package manager install will be executed.
   
   To view your local application:
     -> https://localhost:9080
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
   ```

Prendi nota dell’URL dell’applicazione locale, indicato qui sopra come `-> https://localhost:9080`

1. Aggiungi i due parametri di query seguenti al [URL dell’interfaccia utente AEM](#aem-ui-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`, di solito `&ext=https://localhost:9080`.

   Aggiungi i due parametri di query sopra riportati (`devMode` e `ext`) come __primo__ parametri di query nell’URL. Percorsi hash di utilizzo dell’interfaccia utente estensibile dell’AEM (`#/@wknd/aem/...`), in modo che i parametri dopo il `#` non funziona.

   L’URL di anteprima deve essere simile al seguente:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://localhost:9080&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

2. Copia e incolla l’URL di anteprima nel browser.

   + All&#39;inizio potrebbe essere necessario, e poi periodicamente, [accetta il certificato HTTPS](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) per l&#39;host dell&#39;applicazione locale (`https://localhost:9080`).

3. L’interfaccia utente dell’AEM viene caricata con la versione locale dell’estensione inserita per la verifica.

>[!IMPORTANT]
>
>Quando si utilizza questo approccio, l’estensione in fase di sviluppo influisce solo sull’esperienza e tutti gli altri utenti dell’interfaccia utente dell’AEM usufruiscono dell’interfaccia utente senza l’estensione inserita.

## Verifica le build della fase

1. Apri una riga di comando nella directory principale del progetto di estensione.
1. Assicurati che l’area di lavoro Stage sia attiva (o qualsiasi area di lavoro venga utilizzata per la verifica).

   ```shell
   $ aio app use -w Stage
   ```

   Unisci eventuali modifiche apportate a `.env` e `.aio`.

1. Distribuisci l&#39;app App Builder aggiornata dell&#39;estensione. Se non è stato eseguito l&#39;accesso, eseguire `aio login` prima.

   ```shell
   $ aio app deploy
   ...
   Your deployed actions:
   web actions:
     -> https://98765-123aquarat.adobeio-static.net/api/v1/web/aem-cf-console-admin-1/generic 
   To view your deployed application:
     -> https://98765-123aquarat.adobeio-static.net/index.html
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://98765-123aquarat.adobeio-static.net/index.html
   New Extension Point(s) in Workspace 'Production': 'aem/cf-console-admin/1'
   Successful deployment 🏄
   ```

1. Aggiungi i due parametri di query seguenti al [URL dell’interfaccia utente AEM](#aem-ui-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   Aggiungi i due parametri di query sopra riportati (`devMode` e `ext`) come __primo__ parametri di query nell’URL, poiché le interfacce utente AEM estensibili utilizzano una route hash (`#/@wknd/aem/...`), in modo che i parametri dopo il `#` non funziona.

   L’URL di anteprima deve essere simile al seguente:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Copia e incolla l’URL di anteprima nel browser.
1. La console Frammenti di contenuto AEM inserisce la versione dell’estensione distribuita nell’area di lavoro Stage in. L’URL di questa fase può essere condiviso per il controllo qualità o per gli utenti aziendali per la verifica.

Quando si utilizza questo approccio, l’estensione Staged viene inserita solo nella console Frammenti di contenuto AEM quando si accede con l’URL della fase dell’mestiere.

1. È possibile aggiornare le estensioni implementate eseguendo `aio app deploy` e queste modifiche si riflettono automaticamente quando si utilizza l’URL di anteprima.
1. Per rimuovere un’estensione per la verifica, esegui `aio app undeploy`.

## Anteprima bookmarklet

Per semplificare la creazione degli URL di anteprima e anteprima descritti sopra, è possibile creare un bookmarklet JavaScript che carica l’estensione.

Il bookmarklet seguente visualizza l&#39;anteprima del [build di sviluppo locale](#verify-local-development-builds) dell&#39;estensione il `https://localhost:9080`. Per visualizzare l&#39;anteprima [build stage](#verify-stage-builds), crea un bookmarklet con `previewApp` variabile impostata sull’URL dell’app App Builder implementata.

1. Crea un segnalibro nel browser.
2. Modifica il segnalibro.
3. Assegna a un segnalibro un nome significativo, ad esempio `AEM UI Extension Preview (localhost:9080)`.
4. Impostare l&#39;URL del segnalibro sul codice seguente:

   ```javascript
   javascript: (() => {
       /* Change this to the URL of the local App Builder app if not using https://localhost:9080 */
       const previewApp = 'https://localhost:9080';
   
       const repo = new URL(window.location.href).searchParams.get('repo');
   
       if (window.location.href.match(/https:\/\/experience\.adobe\.com\/.*\/aem\/cf\/(editor|admin)\/.*/i)) {
           window.location = `https://experience.adobe.com/?devMode=true&ext=${previewApp}&repo=${repo}${window.location.hash}`;
       } 
   })();
   ```

5. Passa a un’interfaccia utente AEM estensibile su cui caricare l’estensione di anteprima, quindi fai clic sul bookmarklet.

>[!TIP]
>
> Se l&#39;estensione App Builder non viene caricata, quando si utilizza, `&ext=https://localhost:9080`, apri l’host e la porta direttamente in una scheda del browser e accetta il certificato autofirmato. Riprovare con il bookmarklet.
