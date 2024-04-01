---
title: Creare una configurazione del Cloud Service di tag in AEM Sites
description: Scopri come creare una configurazione del Cloud Service di tag nell’AEM.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5982
thumbnail: 38566.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: a72ddced-37de-4b62-9e28-fa5b6c8ce5b7
duration: 139
source-git-commit: adf3fe30474bcfe5fc1a1e2a8a3d49060067726d
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 0%

---

# Creare una configurazione del Cloud Service di tag nell’AEM {#create-launch-cloud-service}

Scopri come creare una configurazione del Cloud Service di tag in Adobe Experience Manager. La configurazione del Cloud Service di tag AEM può quindi essere applicata a un sito esistente ed è possibile osservare il caricamento delle librerie di tag sia nell’ambiente di authoring che in quello di pubblicazione.

## Crea servizio cloud di tag

Crea la configurazione del servizio cloud di tag utilizzando i passaggi seguenti.

1. Dalla sezione **Strumenti** menu, seleziona **Cloud Service** e fai clic su **Configurazioni di Adobe Launch**
1. Seleziona la cartella di configurazione del sito o fai clic su **Sito WKND** (se utilizzi il progetto della guida WKND) e fai clic su **Crea**
1. Dalla sezione _Generale_ , assegna alla configurazione il nome utilizzando la scheda **Titolo** e seleziona **Adobe lancio** dal _Configurazione Adobe IMS associata_ a discesa. Quindi, seleziona il nome della tua azienda da _Azienda_ e seleziona la proprietà creata in precedenza dal menu a discesa _Proprietà_ a discesa.
1. Dalla sezione _Staging_ e _Produzione_ mantieni le configurazioni predefinite. Tuttavia, si consiglia di rivedere e modificare le configurazioni per la configurazione di produzione reale, in particolare _Carica libreria in modo asincrono_ in base ai requisiti di prestazioni e ottimizzazione. Si noti inoltre che la _URI libreria_ è diverso per Staging e Produzione.
1. Infine, fai clic su **Crea** per completare i servizi cloud di tag.

   ![Configurazione Cloud Service di tag](assets/launch-cloud-services-config.png)

## Applicare il servizio cloud di tag al sito

Per caricare la proprietà Tag e le relative librerie sul sito AEM, al sito viene applicata la configurazione del servizio cloud tags. Nel passaggio precedente la configurazione del servizio cloud viene creata nella cartella del nome del sito (sito WKND), in modo che venga applicata automaticamente. Verificiamola.

1. Dalla sezione **Navigazione** menu, seleziona **Sites** icona.

1. Seleziona la pagina root del sito AEM e fai clic su **Proprietà**. Quindi, passa alla **Avanzate** scheda e sotto **Configurazione** , verifica che il valore Configurazione cloud punti a specifici siti `conf` cartella.

   ![Applica configurazione Cloud Service al sito](assets/apply-cloud-services-config-to-site.png)

## Verificare il caricamento della proprietà Tag nelle pagine Author e Publish

Ora è il momento di verificare che la proprietà Tag e le relative librerie siano caricate sulla pagina del sito AEM.

1. Apri la pagina del tuo sito preferito in **Visualizza come pubblicato** modalità, nella console del browser dovresti visualizzare il messaggio di registro. È lo stesso messaggio dello snippet di codice JavaScript della regola della proprietà Tag che viene attivato quando _Library Loaded (Page Top)_ viene attivato.

1. Per verificare durante la pubblicazione, pubblica innanzitutto il **servizio cloud tag** e aprire la pagina del sito nell’istanza Publish.

   ![Proprietà Tag nelle pagine di authoring e pubblicazione](assets/tag-property-on-author-publish-pages.png)

Congratulazioni. Hai completato l’integrazione dei tag AEM e raccolta dati che inserisce il codice JavaScript nel sito AEM senza aggiornare il codice del progetto AEM.

## Sfida: aggiorna e pubblica regola nella proprietà Tag

Utilizzare gli insegnamenti tratti dai precedenti [Creare una proprietà tag](./create-tag-property.md) per completare la semplice sfida, aggiorna la regola esistente aggiungendo un’ulteriore istruzione della console e utilizzando _Flusso di pubblicazione_ implementarlo sul sito AEM.

## Passaggi successivi

[Debug di un’implementazione di tag](debug-tags-implementation.md)
