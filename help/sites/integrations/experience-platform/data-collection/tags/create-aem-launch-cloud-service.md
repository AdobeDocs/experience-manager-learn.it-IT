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
duration: 99
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 0%

---

# Creare una configurazione del Cloud Service di tag nell’AEM {#create-launch-cloud-service}

Scopri come creare una configurazione del Cloud Service di tag in Adobe Experience Manager. La configurazione del Cloud Service di tag AEM può quindi essere applicata a un sito esistente ed è possibile osservare il caricamento delle librerie di tag sia negli ambienti di authoring che in quelli di Publish.

## Crea servizio cloud di tag

Crea la configurazione del servizio cloud di tag utilizzando i passaggi seguenti.

1. Dal menu **Strumenti**, seleziona la sezione **Cloud Service** e fai clic su **Adobe configurazioni lancio**
1. Seleziona la cartella di configurazione del tuo sito o seleziona **Sito WKND** (se utilizzi il progetto della guida WKND), quindi fai clic su **Crea**
1. Dalla scheda _Generale_, assegna un nome alla configurazione utilizzando il campo **Titolo** e seleziona **Avvio Adobe** dal menu a discesa _Configurazione Adobe IMS associata_. Quindi, seleziona il nome della tua società dal menu a discesa _Società_ e seleziona la proprietà creata in precedenza dal menu a discesa _Proprietà_.
1. Dalla scheda _Gestione temporanea_ e _Produzione_, mantieni le configurazioni predefinite. Tuttavia, si consiglia di rivedere e modificare le configurazioni per la configurazione di produzione reale, in particolare l&#39;interruttore _Carica libreria in modo asincrono_ in base ai requisiti di prestazioni e ottimizzazione. Inoltre, il valore _URI libreria_ è diverso per Staging e Produzione.
1. Infine, fai clic su **Crea** per completare i servizi cloud di tag.

   ![Configurazione Cloud Service tag](assets/launch-cloud-services-config.png)

## Applicare il servizio cloud di tag al sito

Per caricare la proprietà Tag e le relative librerie sul sito AEM, al sito viene applicata la configurazione del servizio cloud tags. Nel passaggio precedente la configurazione del servizio cloud viene creata nella cartella del nome del sito (sito WKND), in modo che venga applicata automaticamente. Verificiamola.

1. Dal menu **Navigazione**, seleziona l&#39;icona **Siti**.

1. Selezionare la pagina principale del sito AEM e fare clic su **Proprietà**. Quindi, passa alla scheda **Avanzate** e nella sezione **Configurazione** verifica che il valore Configurazione cloud punti alla cartella `conf` specifica del sito.

   ![Applica configurazione Cloud Service al sito](assets/apply-cloud-services-config-to-site.png)

## Verificare il caricamento della proprietà Tag nelle pagine Author e Publish

Ora è il momento di verificare che la proprietà Tag e le relative librerie siano caricate sulla pagina del sito AEM.

1. Apri la pagina del tuo sito preferito in modalità **Visualizza come pubblicato**; nella console del browser dovresti visualizzare il messaggio di registro. È lo stesso messaggio dello snippet di codice JavaScript della regola della proprietà Tag che viene attivato quando viene attivato l&#39;evento _Library Loaded (Page Top)_.

1. Per verificare su Publish, pubblica innanzitutto la configurazione di **tags cloud service** e apri la pagina del sito nell&#39;istanza di Publish.

   ![Proprietà tag nelle pagine Author e Publish](assets/tag-property-on-author-publish-pages.png)

Congratulazioni Hai completato l’integrazione dei tag AEM e raccolta dati che inserisce il codice JavaScript nel sito AEM senza aggiornare il codice del progetto AEM.

## Sfida: aggiorna e pubblica regola nella proprietà Tag

Utilizza le lezioni apprese dal precedente [Crea una proprietà tag](./create-tag-property.md) per completare la semplice sfida, aggiorna la regola esistente per aggiungere un&#39;ulteriore istruzione console e utilizza _Flusso di pubblicazione_ per distribuirla sul sito AEM.

## Passaggi successivi

[Debug di un’implementazione di tag](debug-tags-implementation.md)
