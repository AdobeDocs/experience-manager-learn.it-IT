---
title: Creare una proprietà tag
description: Scopri come creare una proprietà Tag con la configurazione minima da integrare con l’AEM. Gli utenti vengono introdotti nell’interfaccia utente Tag e scopri le estensioni, le regole e i flussi di lavoro di pubblicazione.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5980
thumbnail: 38553.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: d5de62ef-a2aa-4283-b500-e1f7cb5dec3b
duration: 640
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 0%

---

# Creare una proprietà tag {#create-tag-property}

Scopri come creare una proprietà Tag con la configurazione minima da integrare con Adobe Experience Manager. Gli utenti vengono introdotti nell’interfaccia utente Tag e scopri le estensioni, le regole e i flussi di lavoro di pubblicazione.

>[!VIDEO](https://video.tv.adobe.com/v/38553?quality=12&learn=on)

## Creazione di proprietà tag

La procedura seguente illustra come creare una proprietà Tag.

1. Nel browser, passa a [Home di Adobe Experience Cloud](https://experience.adobe.com/) e accedi utilizzando Adobe ID.

1. Fai clic su **Raccolta dati** applicazione dal _Accesso rapido_ nella home page di Adobe Experience Cloud.

1. Fai clic su **Tag** voce di menu dalla navigazione a sinistra, quindi fai clic su **Nuova proprietà** dall&#39;angolo superiore destro.

1. Denomina la proprietà Tag utilizzando **Nome** campo obbligatorio. Per il campo Domini, immetti il nome di dominio o, se utilizzi l’ambiente as a Cloud Service AEM, immetti `adobeaemcloud.com` e fai clic su **Salva**.

   ![Proprietà tag](assets/tag-properties.png)

## Creare una nuova regola

Apri la proprietà Tag appena creata facendo clic sul nome nella **Proprietà tag** visualizzazione. Anche sotto _La mia attività recente_ Dovresti vedere che è stata aggiunta l’estensione Core. L’estensione tag Core è l’estensione predefinita e fornisce tipi di eventi fondamentali come caricamento pagina, browser, modulo e altri tipi di eventi. Consulta [Panoramica dell’estensione Core](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/core/overview.html) per ulteriori informazioni.

Le regole ti consentono di specificare cosa deve accadere quando il visitatore interagisce con il tuo sito AEM. Per semplificare, registriamo due messaggi nella console del browser per dimostrare come l’integrazione dei tag di raccolta dati possa inserire codice JavaScript nel sito AEM senza aggiornare il codice del progetto AEM.

Per creare una regola, completa i passaggi seguenti.

1. Clic **Regole** dal _AUTHORING_ nella barra di navigazione a sinistra, quindi fai clic su **Crea nuova regola**

1. Denomina la regola utilizzando **Nome** campo obbligatorio.

1. Clic **Aggiungi** dal _EVENTI_ , quindi nella sezione _Configurazione evento_ modulo, nella **Tipo di evento** selezione a discesa _Library Loaded (Page Top)_ e fai clic su **Mantieni modifiche**.

1. Clic **Aggiungi** dal _AZIONI_ , quindi nella sezione _Configurazione azione_ modulo, nella **Tipo di azione** selezione a discesa _Codice personalizzato_ e fai clic su **Apri editor**.

1. In _Modifica codice_ , immetti il seguente snippet di codice JavaScript, quindi fai clic su **Salva** e infine fare clic su **Mantieni modifiche**.

   ```javascript
   console.log('Tags Property loaded, all set for...');
   console.log('capabilities such as capturing data, conversion tracking and delivering unique and personalized experiences');
   ```

1. Clic **Salva** per completare il processo di creazione della regola.

   ![Nuova regola](assets/new-rule.png)

## Aggiungi libreria e pubblicala

La proprietà Tag _Regole_ vengono attivati utilizzando una libreria, considera la libreria come un pacchetto contenente codice JavaScript. Attiva la regola appena creata seguendo la procedura riportata di seguito.

1. Clic **Flusso di pubblicazione** dal _PUBBLICAZIONE_ nella barra di navigazione a sinistra, quindi fai clic su **Aggiungi libreria**

1. Denomina la libreria utilizzando **Nome** e seleziona _Sviluppo (sviluppo)_ opzione per **Ambiente** a discesa.

1. Per selezionare tutte le risorse modificate dalla creazione della proprietà Tag, fai clic su **+ Aggiungi tutte le risorse modificate**. Questa azione aggiunge la regola appena creata e la risorsa dell’estensione core alla libreria. Infine fai clic su **Salva e genera in sviluppo**.

1. Una volta creata la libreria per **Sviluppo** corsia di nuoto, utilizzo _ellissi_ seleziona la **Invia per approvazione**

1. Quindi nel **Inviato** corsia di nuoto _ellissi_ seleziona la **Approva per la pubblicazione**, allo stesso modo **Genera e pubblica in produzione** nel **Approvato** corsia di nuoto.

![Libreria pubblicata](assets/published-library.png)


Il passaggio precedente completa la semplice creazione della proprietà Tag che dispone di una regola per registrare un messaggio nella console del browser quando la pagina viene caricata. Anche la regola e l&#39;estensione core vengono pubblicate creando una libreria.

## Passaggi successivi

[Connettere l’AEM con la proprietà Tag utilizzando IMS](connect-aem-tag-property-using-ims.md)


## Risorse aggiuntive {#additional-resources}

* [Creare una proprietà tag](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html)
