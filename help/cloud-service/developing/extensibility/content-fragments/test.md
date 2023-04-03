---
title: Verifica di un’estensione AEM console Frammenti di contenuto
description: Scopri come testare un’estensione della console Frammenti di contenuto AEM prima della distribuzione in produzione.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---


# Test di un&#39;estensione

AEM le estensioni della console Frammenti di contenuto possono essere testate in qualsiasi ambiente as a Cloud Service AEM nell’organizzazione Adobe a cui appartiene l’estensione.

Il test di un’estensione viene eseguito tramite un URL appositamente creato che indica alla console Frammento di contenuto AEM di caricare l’estensione.

>[!VIDEO](https://video.tv.adobe.com/v/3412877?quality=12&learn=on)

## URL della console Frammenti di contenuto AEM

![URL della console Frammenti di contenuto AEM](./assets/test/content-fragment-console-url.png){align="center"}

Per creare un URL che imposti l’estensione non di produzione in una console frammento di contenuto AEM, è necessario raccogliere l’URL della console Frammento di contenuto AEM desiderata. Passa all’ambiente AEM as a Cloud Service per testare l’estensione e copia l’URL della console Frammenti di contenuto AEM.

1. Accedi all&#39;env as a Cloud Service AEM desiderata.

   + Utilizzare un ambiente di sviluppo AEM per [verifica delle build di sviluppo](#testing-development-builds)
   + Utilizzare l&#39;ambiente di sviluppo AEM stage o QA per [verifica delle build delle fasi](#testing-stage-builds)

1. Seleziona la __Frammenti di contenuto__ icona.
1. Attendi il caricamento della AEM console Frammenti di contenuto nel browser.
1. Copia l’URL della Console frammenti di contenuto AEM dalla barra degli indirizzi del browser, simile a:

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

Questo URL viene utilizzato di seguito durante la creazione degli URL per lo sviluppo e il test di stage.

## Test delle build di sviluppo locale

1. Apri una riga di comando nella directory principale del progetto di estensione.
1. Esegui l’estensione AEM della console Frammenti di contenuto come app locale App Builder

   ```shell
   $ aio app run
   ...
   No change to package.json was detected. No package manager install will be executed.
   
   To view your local application:
     -> https://localhost:9080
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
   ```

Prendi nota dell’URL dell’applicazione locale, mostrato sopra come `-> https://localhost:9080`

1. Aggiungi i due parametri di query seguenti al [URL della console Frammenti di contenuto AEM](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`, di solito `&ext=https://localhost:9080`.

   Aggiungi i due parametri di query precedenti (`devMode` e `ext`) come __first__ parametri di query nell’URL, in quanto la console Frammenti di contenuto utilizza un percorso hash (`#/@wknd/aem/...`), quindi post-fissaggio errato dei parametri dopo il `#` Non funzionerà.

   L’URL del test dovrebbe essere simile al seguente:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://localhost:9080&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Copia e incolla l’URL del test nel browser.

   + Potrebbe essere necessario inizialmente, e poi periodicamente, [accetta il certificato HTTPS](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) per l&#39;host dell&#39;applicazione locale (`https://localhost:9080`).

1. La AEM console Frammenti di contenuto viene caricata con la versione locale dell’estensione inserita in essa per il test e le modifiche a caldo vengono ricaricate finché l’app locale App Builder è in esecuzione.

>[!IMPORTANT]
>
>Ricorda che quando utilizzi questo approccio, l’estensione in fase di sviluppo influisce solo sull’esperienza e tutti gli altri utenti della console AEM frammenti di contenuto accedono ad essa senza l’estensione inserita.


## Build di un passaggio di test

1. Apri una riga di comando nella directory principale del progetto di estensione.
1. Assicurati che l’area di lavoro Stage sia attiva (o qualsiasi area di lavoro utilizzata per il test).

   ```shell
   $ aio app use -w Stage
   ```

   Unisci eventuali modifiche a `.env` e `.aio`.

1. Distribuisci l&#39;app App Builder aggiornata. Se non hai effettuato l&#39;accesso, esegui `aio login` prima.

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

   Aggiungi i due parametri di query precedenti (`devMode` e `ext`) come __first__ parametri di query nell’URL, in quanto la console Frammenti di contenuto utilizza un percorso hash (`#/@wknd/aem/...`), quindi post-fissaggio errato dei parametri dopo il `#` Non funzionerà.

   L’URL del test dovrebbe essere simile al seguente:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Copia e incolla l’URL del test nel browser.
1. La AEM console Frammenti di contenuto inserisce la versione dell’estensione implementata nell’area di lavoro Stage in. Questo URL della fase può essere condiviso con utenti di QA o business per il test.

Quando si utilizza questo approccio, l’estensione Staged viene inserita solo AEM console Frammenti di contenuto quando si accede all’URL della fase mobile.

1. Le estensioni distribuite possono essere aggiornate eseguendo `aio app deploy` di nuovo, e queste modifiche si riflettono automaticamente quando si utilizza l&#39;URL di test.
1. Per rimuovere un&#39;estensione per il test, esegui `aio app undeploy`.



