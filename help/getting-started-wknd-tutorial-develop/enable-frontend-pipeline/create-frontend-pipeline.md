---
title: Implementazione mediante la pipeline front-end
description: Scopri come creare ed eseguire una pipeline front-end che crea risorse front-end e distribuisce nella rete CDN incorporata in AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: d6da05e4-bd65-4625-b9a4-cad8eae3c9d7
duration: 225
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: ht
source-wordcount: '685'
ht-degree: 100%

---

# Implementazione mediante la pipeline front-end

In questo capitolo viene creata ed eseguita una pipeline front-end in Adobe Cloud Manager. Crea i file solo dal modulo `ui.frontend` e li implementa nel CDN incorporato in AEM as a Cloud Service. In questo modo si esce dall&#39;implementazione delle risorse front-end basata su `/etc.clientlibs`.


## Obiettivi {#objectives}

* Creare ed eseguire una pipeline front-end.
* Verifica che le risorse front-end NON vengano recapitate da `/etc.clientlibs` ma da un nuovo nome host che inizia con `https://static-`

## Utilizzo della pipeline front-end

>[!VIDEO](https://video.tv.adobe.com/v/3409420?quality=12&learn=on)

## Prerequisiti {#prerequisites}

Questo è un tutorial in più parti e si presume che i passaggi descritti in [Aggiornare il progetto AEM standard](./update-project.md) siano stati completati.

Assicurati di disporre di [privilegi per la creazione e l&#39;implementazione di pipeline in Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=it#role-definitions) e di [accesso a un ambiente AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=it).

## Rinominare la pipeline esistente

Rinomina la pipeline esistente da __Implementa in ambiente di sviluppo__ a __Implementazione WKND FullStack in ambiente di sviluppo__ andando al campo __Nome pipeline non di produzione__ della scheda __Configurazione__. Questo per rendere esplicito se una pipeline è full stack o front-end semplicemente osservandone il nome.

![Rinominare la pipeline](assets/fullstack-wknd-deploy-dev-pipeline.png)


Inoltre, nella scheda __Codice origine__, verifica che i valori dei campi Archivio e Ramo Git siano corretti e che il ramo presenti le modifiche al contratto della pipeline front-end.

![Pipeline di configurazione del codice origine](assets/fullstack-wknd-source-code-config.png)


## Creare una pipeline front-end

Per __ONLY__ generare e distribuire le risorse front-end dal modulo `ui.frontend`, effettuare le seguenti operazioni:

1. Nell&#39;interfaccia utente di Cloud Manager, dalla sezione __Pipeline__, fai clic sul pulsante __Aggiungi__, quindi seleziona __Aggiungi pipeline non di produzione__ (o __Aggiungi pipeline di produzione__) in base all&#39;ambiente AEM as a Cloud Service in cui desideri eseguire la distribuzione.

1. Nella finestra di dialogo __Aggiungi pipeline non di produzione__, come parte dei passaggi di __Configurazione__, seleziona l&#39;opzione __Pipeline di distribuzione__, assegna il nome __Implementazione WKND FrontEnd in ambiente di sviluppo__, quindi fai clic su __Continua__

![Creare configurazioni pipeline front-end](assets/create-frontend-pipeline-configs.png)

1. Come parte dei passaggi per __Codice origine__, seleziona l&#39;opzione __Codice front-end__ e scegli l&#39;ambiente da __Ambienti di implementazione idonei__. Nella sezione __Codice origine__ verifica che i valori dei campi Archivio e Ramo Git siano corretti e che il ramo presenti le modifiche al contratto della pipeline front-end.
E __soprattutto__ per il campo __Posizione codice__ il valore è `/ui.frontend`. Infine, fai clic su __Salva__.

![Creare codice origine per pipeline front-end](assets/create-frontend-pipeline-source-code.png)


## Sequenza di distribuzione

* Esegui prima la pipeline __Implementazione WKND FullStack in ambiente di sviluppo__ rinominata per rimuovere i file clientlib WKND dall&#39;archivio AEM. E soprattutto prepara AEM per il contratto della pipeline front-end aggiungendo i file __di configurazione Sling__ (`SiteConfig`, `HtmlPageItemsConfig`).

![Sito WKND non formattato](assets/unstyled-wknd-site.png)

>[!WARNING]
>
>Dopo il completamento della pipeline __Implementazione WKND FullStack in ambiente di sviluppo__, avrai un sito WKND __non formattato__, che potrebbe apparire danneggiato. Pianifica un&#39;interruzione o esegui l&#39;implementazione nelle ore dispari; si tratta di un&#39;interruzione una tantum da pianificare durante il passaggio iniziale dall&#39;utilizzo di una singola pipeline full stack alla pipeline front-end.


* Infine, esegui la pipeline __Implementazione WKND FrontEnd in ambiente di sviluppo__ per compilare solo il modulo `ui.frontend` e implementare le risorse front-end direttamente nella rete CDN.

>[!IMPORTANT]
>
>Il sito WKND __non formattato__ è tornato alla normalità e questa volta l&#39;esecuzione della pipeline __FrontEnd__ è stata molto più veloce della pipeline full stack.

## Verificare le modifiche di stile e il nuovo paradigma di implementazione

* Apri la pagina qualsiasi del sito WKND, potrai vedere il colore del testo __Adobe Red__ e i file delle risorse front-end (CSS, JS) vengono implementati dalla rete CDN. Il nome host della richiesta di risorsa inizia con `https://static-pXX-eYY.p123-e456.adobeaemcloud.com/$HASH_VALUE$/theme/site.css` e allo stesso modo il site.js o qualsiasi altra risorsa statica a cui si fa riferimento nel file `HtmlPageItemsConfig`.


![Sito WKND formattato](assets/newly-styled-wknd-site.png)



>[!TIP]
>
>Il `$HASH_VALUE$` è lo stesso visualizzato nel campo __HASH CONTENUTO__ della pipeline __Implementazione WKND FrontEnd in ambiente di sviluppo__. AEM riceve una notifica dell&#39;URL CDN della risorsa front-end. Il valore viene archiviato in `/conf/wknd/sling:configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/jcr:content` nella proprietà __prefixPath__.


![Correlazione valore hash](assets/hash-value-correlartion.png)



## Congratulazioni. {#congratulations}

Congratulazioni, hai creato, eseguito e verificato la pipeline front-end che crea e implementa solo il modulo &#39;ui.frontend&#39; del progetto WKND Sites. Ora il team front-end può eseguire rapidamente l&#39;iterazione sulla progettazione e sul comportamento front-end del sito, al di fuori dell&#39;intero ciclo di vita del progetto AEM.

## Passaggi successivi {#next-steps}

Nel prossimo capitolo, [Considerazioni](considerations.md), verrà esaminato l&#39;impatto sul processo di sviluppo front-end e back-end.
