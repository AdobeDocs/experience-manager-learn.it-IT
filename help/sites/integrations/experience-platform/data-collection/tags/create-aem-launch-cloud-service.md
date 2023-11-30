---
title: Creare una configurazione del Cloud Service Launch in AEM Sites
description: Scopri come creare una configurazione del Cloud Service Launch nell’AEM. La configurazione del Cloud Service Launch può quindi essere applicata a un sito esistente ed è possibile osservare il caricamento delle librerie di tag sia nell’ambiente di authoring che in quello di pubblicazione.
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
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '556'
ht-degree: 0%

---

# Creare una configurazione del Cloud Service Launch nell’AEM {#create-launch-cloud-service}

>[!NOTE]
>
>Il processo di ridenominazione di Adobe Experience Platform Launch come set di tecnologie di raccolta dati è in fase di implementazione nell’interfaccia utente, nel contenuto e nella documentazione del prodotto AEM, pertanto il termine Launch viene ancora utilizzato qui.

Scopri come creare una configurazione del Cloud Service Launch in Adobe Experience Manager. La configurazione del Cloud Service AEM Launch può quindi essere applicata a un sito esistente ed è possibile osservare il caricamento delle librerie di tag sia nell’ambiente di authoring che in quello di pubblicazione.

## Crea servizio cloud Launch

Crea la configurazione del servizio cloud Launch seguendo la procedura riportata di seguito.

1. Dalla sezione **Strumenti** menu, seleziona **Cloud Service** e fai clic su **Configurazioni di Adobe Launch**

1. Seleziona la cartella di configurazione del sito o fai clic su **Sito WKND** (se utilizzi il progetto della guida WKND) e fai clic su **Crea**

1. Dalla sezione _Generale_ , assegna alla configurazione il nome utilizzando la scheda **Titolo** e seleziona **Adobe lancio** dal _Configurazione Adobe IMS associata_ a discesa. Quindi, seleziona il nome della tua azienda da _Azienda_ e seleziona la proprietà creata in precedenza dal menu a discesa _Proprietà_ a discesa.

1. Dalla sezione _Staging_ e _Produzione_ mantieni le configurazioni predefinite. Tuttavia, si consiglia di rivedere e modificare le configurazioni per la configurazione di produzione reale, in particolare _Carica libreria in modo asincrono_ in base ai requisiti di prestazioni e ottimizzazione. Si noti inoltre che la _URI libreria_ è diverso per Staging e Produzione.

1. Infine, fai clic su **Crea** per completare Launch Cloud Services.

   ![Configurazione Cloud Service di avvio](assets/launch-cloud-services-config.png)

## Applicare Launch Cloud Service al sito

Per caricare la proprietà Tag e le relative librerie sul sito AEM, al sito viene applicata la configurazione del servizio cloud Launch. Nel passaggio precedente la configurazione del servizio cloud viene creata nella cartella del nome del sito (sito WKND), in modo che venga applicata automaticamente. Verificiamola.

1. Dalla sezione **Navigazione** menu, seleziona **Sites** icona.

1. Seleziona la pagina root del sito AEM e fai clic su **Proprietà**. Quindi, passa alla **Avanzate** scheda e sotto **Configurazione** , verifica che il valore Configurazione cloud punti a specifici siti `conf` cartella.

   ![Applica configurazione Cloud Service al sito](assets/apply-cloud-services-config-to-site.png)

## Verificare il caricamento della proprietà Tag nelle pagine Author e Publish

Ora è il momento di verificare che la proprietà Tag e le relative librerie siano caricate sulla pagina del sito AEM.

1. Apri la pagina del tuo sito preferito in **Visualizza come pubblicato** modalità, nella console del browser dovresti visualizzare il messaggio di registro. È lo stesso messaggio dello snippet di codice JavaScript della regola della proprietà Tag che viene attivato quando _Library Loaded (Page Top)_ viene attivato.

1. Per verificare durante la pubblicazione, pubblica innanzitutto il **Avvia servizio cloud** e aprire la pagina del sito nell’istanza Publish.

   ![Proprietà Tag nelle pagine di authoring e pubblicazione](assets/tag-property-on-author-publish-pages.png)

Congratulazioni. Hai completato l’integrazione dei tag AEM e raccolta dati che inserisce il codice JavaScript nel sito AEM senza aggiornare il codice del progetto AEM.

## Sfida: aggiorna e pubblica regola nella proprietà Tag

Utilizzare gli insegnamenti tratti dai precedenti [Creare una proprietà tag](./create-tag-property.md) per completare la semplice sfida, aggiorna la regola esistente aggiungendo un’ulteriore istruzione della console e utilizzando _Flusso di pubblicazione_ implementarlo sul sito AEM.

## Passaggi successivi

[Debug di un’implementazione di tag](debug-tags-implementation.md)
