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
duration: 606
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 0%

---

# Creare una proprietà tag {#create-tag-property}

Scopri come creare una proprietà Tag con la configurazione minima da integrare con Adobe Experience Manager. Gli utenti vengono introdotti nell’interfaccia utente Tag e scopri le estensioni, le regole e i flussi di lavoro di pubblicazione.

>[!VIDEO](https://video.tv.adobe.com/v/38553?quality=12&learn=on)

## Creazione di proprietà tag

La procedura seguente illustra come creare una proprietà Tag.

1. Nel browser, passa alla [home page di Adobe Experience Cloud](https://experience.adobe.com/) e accedi con Adobe ID.

1. Fare clic sull&#39;applicazione **Raccolta dati** dalla sezione _Accesso rapido_ della home page di Adobe Experience Cloud.

1. Fai clic sulla voce di menu **Tag** nell&#39;area di navigazione a sinistra, quindi fai clic su **Nuova proprietà** nell&#39;angolo in alto a destra.

1. Denomina la proprietà Tag utilizzando il campo obbligatorio **Name**. Per il campo Domini, immettere il nome di dominio o, se si utilizza l&#39;ambiente AEM as a Cloud Service, immettere `adobeaemcloud.com` e fare clic su **Salva**.

   ![Proprietà tag](assets/tag-properties.png)

## Creare una nuova regola

Apri la proprietà Tag appena creata facendo clic sul nome nella visualizzazione **Proprietà tag**. Inoltre, sotto l&#39;intestazione _My Recent Activity_ dovresti vedere che l&#39;estensione Core vi è stata aggiunta. L&#39;estensione tag Core è l&#39;estensione predefinita e fornisce tipi di eventi fondamentali come caricamento pagina, browser, modulo e altri tipi di eventi. Per ulteriori informazioni, consulta [Panoramica dell&#39;estensione Core](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/core/overview.html).

Le regole ti consentono di specificare cosa deve accadere quando il visitatore interagisce con il tuo sito AEM. Per semplificare, registriamo due messaggi nella console del browser per dimostrare come l’integrazione dei tag di raccolta dati possa inserire codice JavaScript nel sito AEM senza aggiornare il codice del progetto AEM.

Per creare una regola, completa i passaggi seguenti.

1. Fai clic su **Regole** dalla sezione _CREAZIONE_ della navigazione a sinistra, quindi fai clic su **Crea nuova regola**

1. Denomina la regola utilizzando il campo obbligatorio **Name**.

1. Fai clic su **Aggiungi** dalla sezione _EVENTI_, quindi nel modulo _Configurazione evento_, nel menu a discesa **Tipo evento** seleziona l&#39;opzione _Libreria caricata (parte superiore della pagina)_ e fai clic su **Mantieni modifiche**.

1. Fai clic su **Aggiungi** dalla sezione _AZIONI_, quindi nel modulo _Configurazione azione_, nel menu a discesa **Tipo azione** seleziona l&#39;opzione _Codice personalizzato_ e fai clic su **Apri editor**.

1. Nel modale _Modifica codice_, immetti il seguente snippet di codice JavaScript, quindi fai clic su **Salva** e infine su **Mantieni modifiche**.

   ```javascript
   console.log('Tags Property loaded, all set for...');
   console.log('capabilities such as capturing data, conversion tracking and delivering unique and personalized experiences');
   ```

1. Fai clic su **Salva** per completare il processo di creazione della regola.

   ![Nuova regola](assets/new-rule.png)

## Aggiungi libreria e pubblicala

La proprietà tag _Rules_ è attivata utilizzando una libreria. Considerarla come un pacchetto contenente codice JavaScript. Attiva la regola appena creata seguendo la procedura riportata di seguito.

1. Fai clic su **Flusso di pubblicazione** dalla sezione _PUBBLICAZIONE_ nel menu di navigazione a sinistra, quindi fai clic su **Aggiungi libreria**

1. Assegna un nome alla libreria utilizzando il campo **Nome** e seleziona l&#39;opzione _Sviluppo(sviluppo)_ per il menu a discesa **Ambiente**.

1. Per selezionare tutte le risorse modificate dopo la creazione della proprietà Tag, fare clic su **+ Aggiungi tutte le risorse modificate**. Questa azione aggiunge la regola appena creata e la risorsa dell’estensione core alla libreria. Infine, fare clic su **Salva e genera in sviluppo**.

1. Una volta creata la libreria per la corsia di nuoto **Sviluppo**, utilizzando _puntini di sospensione_, seleziona **Invia per approvazione**

1. Quindi nella corsia di nuoto **Submitted** utilizzando _ellissi_ seleziona la **Approve for Publishing**, allo stesso modo **Build &amp; Publish to Production** nella corsia di nuoto **Approved**.

![Libreria pubblicata](assets/published-library.png)


Il passaggio precedente completa la semplice creazione della proprietà Tag che dispone di una regola per registrare un messaggio nella console del browser quando la pagina viene caricata. Anche la regola e l&#39;estensione core vengono pubblicate creando una libreria.

## Passaggi successivi

[Connettere l’AEM con la proprietà Tag utilizzando IMS](connect-aem-tag-property-using-ims.md)


## Risorse aggiuntive {#additional-resources}

* [Creare una proprietà tag](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html)
