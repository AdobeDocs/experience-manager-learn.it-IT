---
title: Verificare un’estensione dell’interfaccia utente di AEM
description: Scopri come visualizzare in anteprima, testare e verificare un’estensione dell’interfaccia utente di AEM prima di distribuirla in produzione.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603, KT-13382
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: c5c1df23-1c04-4c04-b0cd-e126c31d5acc
duration: 600
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '739'
ht-degree: 0%

---

# Verificare un’estensione

Le estensioni dell’interfaccia utente di AEM possono essere verificate in base a qualsiasi ambiente AEM as a Cloud Service nell’organizzazione Adobe a cui appartiene l’estensione.

Il test di un’estensione viene eseguito tramite un URL appositamente creato che indica ad AEM di caricarla, solo per tale richiesta.

>[!VIDEO](https://video.tv.adobe.com/v/3412877?quality=12&learn=on)

>[!IMPORTANT]
>
> Il video precedente illustra l’utilizzo di un’estensione della Console Frammenti di contenuto per illustrare l’anteprima e la verifica dell’app dell’estensione App Builder. Tuttavia, è importante notare che i concetti descritti possono essere applicati a tutte le estensioni dell’interfaccia utente di AEM.

## URL interfaccia utente AEM

![URL console Frammenti di contenuto AEM](./assets/verify/content-fragment-console-url.png){align="center"}

Per creare un URL che monti l’estensione non di produzione in AEM, è necessario ottenere l’URL dell’interfaccia utente di AEM in cui viene inserita l’estensione. Passa all’ambiente AEM as a Cloud Service per verificare l’estensione su e apri l’interfaccia utente su cui deve essere visualizzata l’estensione.

Ad esempio, per visualizzare in anteprima un’estensione per la console Frammenti di contenuto:

1. Accedi all’ambiente AEM as a Cloud Service desiderato.
1. Seleziona l&#39;icona __Frammenti di contenuto__.
1. Attendi che la console Frammenti di contenuto di AEM venga caricata nel browser.
1. Copia l’URL della Console Frammenti di contenuto di AEM dalla barra degli indirizzi del browser; dovrebbe essere simile al seguente:

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

Questo URL viene utilizzato di seguito per la creazione degli URL per lo sviluppo e la verifica dell’area di visualizzazione. Se verifichi l’estensione rispetto ad altre interfacce utente di AEM, ottieni tali URL e applica gli stessi passaggi indicati di seguito.

## Verificare le build di sviluppo locali

1. Apri una riga di comando nella directory principale del progetto di estensione.
1. Eseguire l’estensione dell’interfaccia utente di AEM come app App Builder locale

   ```shell
   $ aio app run
   ...
   No change to package.json was detected. No package manager install will be executed.
   
   To view your local application:
     -> https://localhost:9080
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
   ```

Prendi nota dell&#39;URL dell&#39;applicazione locale, indicato sopra come `-> https://localhost:9080`

1. Inizialmente (e ogni volta che viene visualizzato un errore di connessione) apri `https://localhost:9080` (o qualsiasi URL dell&#39;applicazione locale sia) nel browser Web e accetta manualmente [il certificato HTTPS](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users).
1. Aggiungi i due parametri di query seguenti all&#39;URL dell&#39;interfaccia utente di [AEM](#aem-ui-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`, in genere `&ext=https://localhost:9080`.

   Aggiungere i due parametri di query sopra indicati (`devMode` e `ext`) come __primi__ parametri di query nell&#39;URL. L&#39;interfaccia utente estensibile di AEM utilizza route hash (`#/@wknd/aem/...`), pertanto non corregge correttamente la post-correzione dei parametri dopo che `#` non funziona.

   L’URL di anteprima deve essere simile al seguente:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://localhost:9080&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Copia e incolla l’URL di anteprima nel browser.

   + Potrebbe essere necessario inizialmente e quindi periodicamente [accettare il certificato HTTPS](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) per l&#39;host dell&#39;applicazione locale (`https://localhost:9080`).

1. L’interfaccia utente di AEM viene caricata con la versione locale dell’estensione inserita per la verifica.

>[!IMPORTANT]
>
>Quando utilizzi questo approccio, l’estensione in fase di sviluppo influisce solo sull’esperienza e tutti gli altri utenti dell’interfaccia utente di AEM usufruiscono dell’interfaccia utente senza l’estensione inserita.

## Verifica le build della fase

1. Apri una riga di comando nella directory principale del progetto di estensione.
1. Assicurati che l’area di lavoro Stage sia attiva (o qualsiasi ambiente Workspace venga utilizzato per la verifica).

   ```shell
   $ aio app use -w Stage
   ```

   Unisci le modifiche apportate a `.env` e `.aio`.

1. Distribuisci l’estensione aggiornata dell’app App Builder. Se non si è connessi, eseguire prima `aio login`.

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

1. Aggiungi i due parametri di query seguenti all&#39;URL dell&#39;interfaccia utente di [AEM](#aem-ui-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   Aggiungi i due parametri di query sopra riportati (`devMode` e `ext`) come __primi__ parametri di query nell&#39;URL, poiché le interfacce utente estensibili di AEM utilizzano una route hash (`#/@wknd/aem/...`), pertanto dopo la mancata esecuzione di `#` i parametri non vengono corretti.

   L’URL di anteprima deve essere simile al seguente:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Copia e incolla l’URL di anteprima nel browser.
1. La console Frammenti di contenuto di AEM inserisce la versione dell’estensione distribuita nell’area di lavoro dello stage in. L’URL di questa fase può essere condiviso per il controllo qualità o per gli utenti aziendali per la verifica.

Quando si utilizza questo approccio, l’estensione Staged viene inserita solo nella console Frammenti di contenuto di AEM quando si accede con l’URL di stage dell’mestiere.

1. È possibile aggiornare le estensioni distribuite eseguendo nuovamente `aio app deploy`. Queste modifiche verranno applicate automaticamente quando si utilizza l&#39;URL di anteprima.
1. Per rimuovere un&#39;estensione per la verifica, eseguire `aio app undeploy`.

## Anteprima bookmarklet

Per semplificare la creazione degli URL di anteprima e anteprima descritti sopra, è possibile creare un bookmarklet JavaScript che carica l’estensione.

Il bookmarklet seguente visualizza in anteprima le [build di sviluppo locali](#verify-local-development-builds) dell&#39;estensione in `https://localhost:9080`. Per visualizzare in anteprima [le build della fase](#verify-stage-builds), crea un bookmarklet con la variabile `previewApp` impostata sull&#39;URL dell&#39;app App Builder distribuita.

1. Crea un segnalibro nel browser.
1. Modifica il segnalibro.
1. Assegnare a un segnalibro un nome significativo, ad esempio `AEM UI Extension Preview (localhost:9080)`.
1. Impostare l&#39;URL del segnalibro sul codice seguente:

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

1. Passa a un’interfaccia utente AEM estensibile su cui caricare l’estensione di anteprima, quindi fai clic sul bookmarklet.

>[!TIP]
>
> Se l&#39;estensione App Builder non viene caricata, quando si utilizza, `&ext=https://localhost:9080`, aprire l&#39;host e la porta direttamente in una scheda del browser e accettare il certificato autofirmato. Riprovare con il bookmarklet.
