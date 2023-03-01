---
title: Verifica di un’estensione della console Frammenti di contenuto AEM
description: Scopri come verificare un’estensione della console Frammenti di contenuto AEM prima di distribuirla in produzione.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: 1a4ee470a650aacc5412fbd27062ca14ccdb1967
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---


# Testare un’estensione

Le estensioni della Console Frammenti di contenuto AEM possono essere testate in base a qualsiasi ambiente as a Cloud Service AEM nell’organizzazione di Adobi a cui appartiene l’estensione.

Il test di un’estensione viene eseguito tramite un URL appositamente creato che indica alla console Frammenti di contenuto AEM di caricare l’estensione.

>[!VIDEO](https://video.tv.adobe.com/v/3412877/?quality=12&learn=on)

## URL della console Frammenti di contenuto AEM

![URL della console Frammenti di contenuto AEM](./assets/test/content-fragment-console-url.png){align="center"}

Per creare un URL che monti l’estensione non di produzione in una console Frammenti di contenuto AEM, è necessario raccogliere l’URL della console Frammenti di contenuto AEM desiderata. Passa all’ambiente as a Cloud Service dell’AEM per testare l’estensione su e copiare l’URL della relativa console per frammenti di contenuto AEM.

1. Accedi all’ambiente AEM as a Cloud Service desiderato.

   + Utilizzare un ambiente di sviluppo AEM per [test delle build di sviluppo](#testing-development-builds)
   + Utilizzare l’ambiente di staging AEM o di sviluppo del controllo qualità per [build della fase di test](#testing-stage-builds)

1. Seleziona la __Frammenti di contenuto__ icona.
1. Attendi che la console Frammenti di contenuto AEM venga caricata nel browser.
1. Copia l’URL della console Frammenti di contenuto AEM dalla barra degli indirizzi del browser; dovrebbe essere simile al seguente:

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

Questo URL viene utilizzato di seguito per creare gli URL per lo sviluppo e il test nell’ambiente di staging.

## Testare le build di sviluppo locale

1. Apri una riga di comando nella directory principale del progetto di estensione.
1. Eseguire l’estensione Console Frammenti di contenuto AEM come app locale di App Builder

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

1. Aggiungi i due parametri di query seguenti al [URL della console Frammenti di contenuto AEM](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`, di solito `&ext=https://localhost:9080`.

   Aggiungi i due parametri di query sopra riportati (`devMode` e `ext`) come __primo__ parametri di query nell’URL, poiché la console Frammenti di contenuto utilizza una route hash (`#/@wknd/aem/...`), in modo che i parametri dopo il `#` non funzionerà.

   L’URL del test deve essere simile al seguente:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://localhost:9080&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Copia e incolla l’URL di prova nel browser.

   + All&#39;inizio potrebbe essere necessario, e poi periodicamente, [accetta il certificato HTTPS](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) per l&#39;host dell&#39;applicazione locale (`https://localhost:9080`).

1. La console Frammenti di contenuto AEM viene caricata con la versione locale dell’estensione inserita per il test e le modifiche mediante ricaricamento a caldo finché l’app locale di App Builder è in esecuzione.

>[!IMPORTANT]
>
>Quando utilizzi questo approccio, l’estensione in fase di sviluppo influisce solo sull’esperienza e tutti gli altri utenti della console Frammenti di contenuto AEM vi accedono senza l’estensione inserita.


## Build della fase di test

1. Apri una riga di comando nella directory principale del progetto di estensione.
1. Assicurati che l’area di lavoro Stage sia attiva (o qualsiasi area di lavoro venga utilizzata per i test).

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

1. Aggiungi i due parametri di query seguenti al [URL della console Frammenti di contenuto AEM](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   Aggiungi i due parametri di query sopra riportati (`devMode` e `ext`) come __primo__ parametri di query nell’URL, poiché la console Frammenti di contenuto utilizza una route hash (`#/@wknd/aem/...`), in modo che i parametri dopo il `#` non funzionerà.

   L’URL del test deve essere simile al seguente:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Copia e incolla l’URL di prova nel browser.
1. La console Frammenti di contenuto AEM inserisce la versione dell’estensione distribuita nell’area di lavoro Stage in. L’URL di questa fase può essere condiviso per il controllo qualità o per gli utenti aziendali per i test.

Quando si utilizza questo approccio, l’estensione Staged viene inserita solo nella console Frammenti di contenuto AEM quando si accede con l’URL della fase dell’mestiere.

1. È possibile aggiornare le estensioni implementate eseguendo `aio app deploy` e queste modifiche si riflettono automaticamente quando si utilizza l’URL di test.
1. Per rimuovere un’estensione per il test, esegui `aio app undeploy`.



