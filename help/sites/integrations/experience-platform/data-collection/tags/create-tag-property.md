---
title: Creare una proprietà tag
description: Scopri come creare una proprietà Tag con la configurazione minima a barre da integrare con AEM. Gli utenti vengono introdotti nell’interfaccia utente Tag e scopri estensioni, regole e flussi di lavoro di pubblicazione.
topics: integrations
audience: administrator
doc-type: technical video
activity: setup
version: Cloud Service
kt: 5980
thumbnail: 38553.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: d5de62ef-a2aa-4283-b500-e1f7cb5dec3b
source-git-commit: 2b37ba961e194b47e034963ceff63a0b8e8458ae
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 1%

---

# Creare una proprietà tag {#create-tag-property}

Scopri come creare una proprietà Tag con la configurazione minima da integrare con Adobe Experience Manager. Gli utenti vengono introdotti nell’interfaccia utente Tag e scopri estensioni, regole e flussi di lavoro di pubblicazione.

>[!VIDEO](https://video.tv.adobe.com/v/38553?quality=12&learn=on)

## Creazione di una proprietà tag

Per creare una proprietà Tag , completa i passaggi seguenti.

1. Nel browser, passa alla [Pagina principale di Adobe Experience Cloud](https://experience.adobe.com/) pagina e accesso utilizzando Adobe ID.

1. Fai clic sul pulsante **Raccolta dati** della _Accesso rapido_ della home page di Adobe Experience Cloud.

1. Fai clic sul pulsante **Tag** voce di menu dalla navigazione a sinistra, quindi fare clic su **Nuova proprietà** dall&#39;angolo in alto a destra.

1. Denomina la proprietà Tag utilizzando **Nome** campo obbligatorio. Per il campo Domini, immetti il nome di dominio o se utilizzi AEM ambiente as a Cloud Service immetti `adobeaemcloud.com` e fai clic su **Salva**.

   ![Proprietà tag](assets/tag-properties.png)

## Creare una nuova regola

Apri la nuova proprietà Tag creata facendo clic sul suo nome nel **Proprietà tag** visualizza. Anche sotto _Attività recente_ Dovresti vedere che l’estensione Core è stata aggiunta ad essa. L&#39;estensione Core tag è l&#39;estensione predefinita e fornisce tipi di evento fondamentali come caricamento pagina, browser, modulo e altri tipi di evento, vedi [Panoramica dell&#39;estensione core](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/core/overview.html) per ulteriori informazioni.

Le regole consentono di specificare cosa deve accadere quando il visitatore interagisce con il sito AEM. Per semplificare le cose, registra due messaggi nella console del browser per dimostrare come l’integrazione dei tag di raccolta dati può inserire codice JavaScript nel sito AEM senza aggiornare AEM codice del progetto.

Per creare una regola, completa i passaggi seguenti.

1. Fai clic su **Regole** dal _AUTHORING_ della navigazione a sinistra, quindi fai clic su **Crea nuova regola**

1. Denomina la regola utilizzando **Nome** campo obbligatorio.

1. Fai clic su **Aggiungi** dal _EVENTI_ nella sezione _Configurazione evento_ nel modulo **Tipo evento** selezione a discesa _Libreria caricata (Pagina in alto)_ e fai clic su **Mantieni modifiche**.

1. Fai clic su **Aggiungi** dal _AZIONI_ nella sezione _Configurazione azione_ nel modulo **Tipo di azione** selezione a discesa _Codice personalizzato_ e fai clic su **Open Editor**.

1. In _Modifica codice_ modale, immetti il seguente frammento di codice JavaScript, quindi fai clic su **Salva** e infine fai clic su **Mantieni modifiche**.

   ```javascript
   console.log('Tags Property loaded, all set for...');
   console.log('capabilities such as capturing data, conversion tracking and delivering unique and personalized experiences');
   ```

1. Fai clic su **Salva** per completare il processo di creazione della regola.

   ![Nuova regola](assets/new-rule.png)

## Aggiungi libreria e pubblicala

La proprietà Tag _Regole_ vengono attivate utilizzando una libreria , considera la libreria come un pacchetto contenente codice JavaScript . Attiva la regola appena creata seguendo i passaggi descritti.

1. Fai clic su **Flusso di pubblicazione** dal _PUBBLICAZIONE_ della navigazione a sinistra, quindi fai clic su **Aggiungi libreria**

1. Denomina la libreria utilizzando **Nome** campo e seleziona _Sviluppo (sviluppo)_ opzione per **Ambiente** a discesa.

1. Per selezionare tutte le risorse modificate dalla creazione della proprietà Tag, fai clic su **+ Aggiungi tutte le risorse modificate**. Questa azione aggiunge alla libreria la risorsa di nuova creazione per la regola e l&#39;estensione core. Infine, fai clic su **Salva e genera in sviluppo**.

1. Una volta creata la libreria per **Sviluppo** corsia di nuoto, utilizzando _ellissi_ seleziona la **Invia per approvazione**

1. Poi nella **Inviato** corsia di nuoto _ellissi_ seleziona la **Approva per la pubblicazione** allo stesso modo **Creare e pubblicare in produzione** in **Approvato** pista di nuoto.

![Libreria pubblicata](assets/published-library.png)


Il passaggio precedente completa la semplice creazione della proprietà Tag con una regola per registrare un messaggio nella console del browser quando la pagina viene caricata. Anche la regola e l&#39;estensione core vengono pubblicati creando una libreria .

## Passaggi successivi

[Connetti AEM con la proprietà tag utilizzando IMS](connect-aem-tag-property-using-ims.md)


## Risorse aggiuntive {#additional-resources}

* [Creare una proprietà tag](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html)
