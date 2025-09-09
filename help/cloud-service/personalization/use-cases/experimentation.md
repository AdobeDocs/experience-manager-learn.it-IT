---
title: Sperimentazione (test A/B)
description: Scopri come testare diverse varianti di contenuto in AEM as a Cloud Service (AEMCS) utilizzando Adobe Target per il test A/B.
version: Experience Manager as a Cloud Service
feature: Personalization
topic: Personalization,Content Management, Integrations
role: Developer, Architect, Leader
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18720
thumbnail: KT-18720.jpeg
exl-id: c8a4f0bf-1f80-4494-abe6-9fbc138e4039
source-git-commit: 5b91e7409ff0735bab40d78ad98410ac2ab006ed
workflow-type: tm+mt
source-wordcount: '1493'
ht-degree: 1%

---

# Sperimentazione (test A/B)

Scopri come testare diverse varianti di contenuto su un sito web AEM as a Cloud Service (AEMCS) utilizzando Adobe Target.

Il test A/B consente di confrontare diverse versioni di contenuto per determinare quale funziona meglio nel raggiungimento degli obiettivi aziendali. Gli scenari comuni includono:

- Verifica delle varianti di titoli, immagini o pulsanti call-to-action in una pagina di destinazione
- Confronto di layout o progettazioni diversi per una pagina dei dettagli di un prodotto
- Valutazione delle offerte promozionali o delle strategie di sconto

## Caso di utilizzo demo

In questo tutorial, puoi configurare i test A/B per il **campeggio in Australia occidentale** frammento di esperienza (XF) sul sito Web WKND. Puoi creare tre varianti XF e gestire il test A/B tramite Adobe Target.

Le varianti vengono visualizzate nella home page di WKND, consentendoti di misurare le prestazioni e determinare quale versione favorisce un coinvolgimento e conversioni migliori.

![Test A/B](../assets/use-cases/experiment/view-ab-test-variations.png)

### Demo live

Visita il [sito Web WKND Enablement](https://wknd.enablementadobe.com/us/en.html) per visualizzare il test A/B in azione. Nel video seguente trovi tutte e tre le varianti di **Campeggio in Western Australia** visualizzate nella home page tramite browser diversi.

>[!VIDEO](https://video.tv.adobe.com/v/3473005/?learn=on&enablevpops)

## Prerequisiti

Prima di procedere con il caso di utilizzo della sperimentazione, assicurati di aver completato quanto segue:

- [Integrare Adobe Target](../setup/integrate-adobe-target.md): consente al team di creare e gestire contenuti personalizzati centralmente in AEM e attivarli come offerte in Adobe Target.
- [Integrare i tag in Adobe Experience Platform](../setup/integrate-adobe-tags.md): consente al team di gestire e distribuire JavaScript per la personalizzazione e la raccolta dati senza dover ridistribuire il codice AEM.

## Passaggi di alto livello

Il processo di configurazione del test A/B prevede sei passaggi principali per creare e configurare l’esperimento:

1. **Creare varianti di contenuto in AEM**
2. **Esporta le varianti come offerte in Adobe Target**
3. **Creare un&#39;attività di test A/B in Adobe Target**
4. **Creare e configurare uno stream di dati in Adobe Experience Platform**
5. **Aggiorna la proprietà Tags con l&#39;estensione Web SDK**
6. **Verifica l&#39;implementazione del test A/B nelle pagine AEM**

## Creare varianti di contenuto in AEM

In questo esempio, utilizzi il **Campeggio in Australia occidentale** Frammento di esperienza (XF) del progetto AEM WKND per creare tre varianti, che verranno utilizzate nella home page del sito Web WKND per i test A/B.

1. In AEM, fai clic sulla scheda **Frammenti esperienza**, passa a **Campeggio nell&#39;Australia occidentale** e fai clic su **Modifica**.
   ![Frammenti esperienza](../assets/use-cases/experiment/camping-in-western-australia-xf.png)

1. Nell&#39;editor, nella sezione **Varianti**, fare clic su **Crea** e selezionare **Variante**.\
   ![Crea variante](../assets/use-cases/experiment/create-variation.png)

1. Nella finestra di dialogo **Crea variante**:
   - **Modello**: Modello di variante Web frammento esperienza
   - **Titolo**: ad esempio, &quot;Fuori dalla griglia&quot;

   Fai clic su **Fine**.

   ![Crea finestra di dialogo variante](../assets/use-cases/experiment/create-variation-dialog.png)

1. Crea la variante copiando il componente **Teaser** dalla variante principale, quindi personalizza il contenuto (ad esempio, aggiorna il titolo e l&#39;immagine).\
   ![Variante autore-1](../assets/use-cases/experiment/author-variation-1.png)

   >[!TIP]
   >Puoi utilizzare [Genera varianti](https://experience.adobe.com/aem/generate-variations/) per creare rapidamente nuove varianti dall&#39;XF principale.

1. Ripeti i passaggi per creare un’altra variante (ad esempio, &quot;Wandering the Wild&quot;).\
   ![Variante autore-2](../assets/use-cases/experiment/author-variation-2.png)

   Ora disponi di tre varianti di Frammento esperienza per il test A/B.

1. Prima di visualizzare le varianti utilizzando Adobe Target, è necessario rimuovere il teaser statico esistente dalla home page. Evita contenuti duplicati, in quanto le varianti di Frammento esperienza vengono inserite dinamicamente tramite Target.

   - Passa alla home page **Inglese** `/content/wknd/language-masters/en`
   - Nell&#39;editor, elimina il componente teaser **Camping in Australia occidentale**.\
     ![Elimina componente teaser](../assets/use-cases/experiment/delete-teaser-component.png)

1. Eseguire il rollout delle modifiche alla home page di **Stati Uniti > Inglese** (`/content/wknd/us/en`) per propagare gli aggiornamenti.\
   ![Modifiche rollout](../assets/use-cases/experiment/rollout-changes.png)

1. Pubblica la home page **Stati Uniti > Inglese** per rendere attivi gli aggiornamenti.\
   ![Home page pubblicazione](../assets/use-cases/experiment/publish-homepage.png)

## Esportare le varianti come offerte in Adobe Target

Esporta le varianti di Frammento esperienza in modo che possano essere utilizzate come offerte in Adobe Target per il test A/B.

1. In AEM, passa a **Campeggio nell&#39;Australia occidentale**, seleziona le tre varianti e fai clic su **Esporta in Adobe Target**.\
   ![Esporta in Adobe Target](../assets/use-cases/experiment/export-to-adobe-target.png)

2. In Adobe Target, vai a **Offerte** e conferma che le varianti sono state importate.\
   ![Offerte in Adobe Target](../assets/use-cases/experiment/offers-in-adobe-target.png)

## Creare un’attività di test A/B in Adobe Target

Ora crea un’attività di test A/B per eseguire l’esperimento sulla pagina home.

1. Installa l&#39;estensione per Chrome [Adobe Experience Cloud Visual Editing Helper](https://chromewebstore.google.com/detail/adobe-experience-cloud-vi/kgmjjkfjacffaebgpkpcllakjifppnca).

1. In Adobe Target, passa a **Attività** e fai clic su **Crea attività**.\
   ![Crea attività](../assets/use-cases/experiment/create-activity.png)

1. Nella finestra di dialogo **Crea attività test A/B**, immetti quanto segue:
   - **Tipo**: Web
   - **Compositore**: visivo
   - **URL attività**: ad esempio, `https://wknd.enablementadobe.com/us/en.html`

   Fai clic su **Crea**.

   ![Crea attività test A/B](../assets/use-cases/experiment/create-ab-test-activity.png)

1. Rinomina l’attività con un nome significativo (ad esempio, &quot;WKND Homepage A/B Test&quot;).\
   ![Rinomina attività](../assets/use-cases/experiment/rename-activity.png)

1. In **Esperienza A**, aggiungi il componente **Frammento esperienza** sopra la sezione **Articoli recenti**.\
   ![Aggiungi componente frammento esperienza](../assets/use-cases/experiment/add-experience-fragment-component.png)

1. Nella finestra di dialogo del componente, fai clic su **Seleziona un&#39;offerta**.\
   ![Seleziona offerta](../assets/use-cases/experiment/select-offer.png)

1. Scegli la variante **Camping in Western Australia** e fai clic su **Aggiungi**.\
   ![Seleziona campeggio in Australia occidentale XF](../assets/use-cases/experiment/select-camping-in-western-australia-xf.png)

1. Ripeti per **Esperienza B** e **C**, selezionando rispettivamente **Off the Grid** e **Wandering the Wild**.\
   ![Aggiungi esperienza B e C](../assets/use-cases/experiment/add-experience-b-and-c.png)

1. Nella sezione **Targeting**, conferma che il traffico sia suddiviso in modo uniforme tra tutte le esperienze.\
   ![Allocazione traffico](../assets/use-cases/experiment/traffic-allocation.png)

1. In **Obiettivi e impostazioni**, definisci la metrica di successo (ad esempio, CTA fa clic sul frammento di esperienza).\
   ![Obiettivi e impostazioni](../assets/use-cases/experiment/goals-and-settings.png)

1. Fai clic su **Attiva** nell&#39;angolo in alto a destra per avviare il test.\
   ![Attiva attività](../assets/use-cases/experiment/activate-activity.png)

## Creare e configurare uno stream di dati in Adobe Experience Platform

Per collegare Adobe Web SDK ad Adobe Target, crea uno stream di dati in Adobe Experience Platform. Lo stream di dati funge da livello di routing tra Web SDK e Adobe Target.

1. In Adobe Experience Platform, passa a **Datastreams** e fai clic su **Create Datastream**.\
   ![Crea nuovo flusso di dati](../assets/use-cases/experiment/aep-create-datastream.png)

1. Nella finestra di dialogo **Crea flusso di dati**, immetti un **Nome** per il flusso di dati e fai clic su **Salva**.\
   ![Inserisci Il Nome Dello Stream Di Dati](../assets/use-cases/experiment/aep-datastream-name.png)

1. Una volta creato lo stream di dati, fare clic su **Aggiungi servizio**.\
   ![Aggiungi servizio a Datastream](../assets/use-cases/experiment/aep-add-service.png)

1. Nel passaggio **Aggiungi servizio**, seleziona **Adobe Target** dal menu a discesa e immetti l&#39;**ID ambiente di destinazione**. Puoi trovare l&#39;ID ambiente di destinazione in Adobe Target in **Amministrazione** > **Ambienti**. Fai clic su **Salva** per aggiungere il servizio.\
   ![Configura servizio Adobe Target](../assets/use-cases/experiment/aep-target-service.png)

1. Controlla i dettagli dello stream di dati per verificare che il servizio Adobe Target sia elencato e configurato correttamente.\
   ![Revisione dello stream di dati](../assets/use-cases/experiment/aep-datastream-review.png)

## Aggiornare la proprietà Tags con l’estensione Web SDK

Per inviare eventi di personalizzazione e raccolta dati dalle pagine di AEM, aggiungi l’estensione Web SDK alla proprietà Tags e configura una regola che attiva al caricamento della pagina.

1. In Adobe Experience Platform, passa a **Tag** e apri la proprietà creata nel passaggio [Integrare i tag di Adobe](../setup/integrate-adobe-tags.md).
   ![Apri proprietà tag](../assets/use-cases/experiment/open-tags-property.png)

1. Dal menu a sinistra, fare clic su **Estensioni**, passare alla scheda **Catalogo** e cercare **Web SDK**. Fai clic su **Installa** nel pannello a destra.\
   ![Installa estensione Web SDK](../assets/use-cases/experiment/web-sdk-extension-install.png)

1. Nella finestra di dialogo **Installa estensione**, seleziona lo **stream di dati** creato in precedenza e fai clic su **Salva**.\
   ![Seleziona flusso di dati](../assets/use-cases/experiment/web-sdk-extension-select-datastream.png)

1. Dopo l&#39;installazione, verificare che entrambe le estensioni **Adobe Experience Platform Web SDK** e **Core** siano visualizzate nella scheda **Installed**.\
   ![Estensioni installate](../assets/use-cases/experiment/web-sdk-extension-installed.png)

1. Quindi, configura una regola per inviare l&#39;evento Web SDK al caricamento della libreria. Passa a **Regole** dal menu a sinistra e fai clic su **Crea nuova regola**.

   ![Crea nuova regola](../assets/use-cases/experiment/web-sdk-rule-create.png)

   >[!TIP]
   >
   >Una regola consente di definire quando e come i tag si attivano in base alle interazioni dell’utente o agli eventi del browser.

1. Nella schermata **Crea regola**, immetti un nome di regola (ad esempio, `All Pages - Library Loaded - Send Event`) e fai clic su **+ Aggiungi** nella sezione **Eventi**.
   ![Nome regola](../assets/use-cases/experiment/web-sdk-rule-name.png)

1. Nella finestra di dialogo **Configurazione evento**:
   - **Estensione**: Seleziona **Core**
   - **Tipo evento**: seleziona **Libreria caricata (parte superiore della pagina)**
   - **Nome**: Immettere `Core - Library Loaded (Page Top)`

   Fai clic su **Mantieni modifiche** per salvare l&#39;evento.

   ![Crea regola evento](../assets/use-cases/experiment/web-sdk-rule-event.png)

1. Nella sezione **Azioni**, fai clic su **+ Aggiungi** per definire l&#39;azione che si verifica quando l&#39;evento viene generato.

1. Nella finestra di dialogo **Configurazione azione**:
   - **Estensione**: Seleziona **Adobe Experience Platform Web SDK**
   - **Tipo azione**: Seleziona **Invia Evento**
   - **Nome**: Seleziona **AEP Web SDK - Invia evento**

   ![Configura azione evento di invio](../assets/use-cases/experiment/web-sdk-rule-action.png)

1. Nella sezione **Personalization** del pannello di destra, seleziona l&#39;opzione **Rendering delle decisioni di personalizzazione visiva**. Quindi, fai clic su **Mantieni modifiche** per salvare l&#39;azione.\
   ![Crea regola azione](../assets/use-cases/experiment/web-sdk-rule-action.png)

   >[!TIP]
   >
   >   Questa azione invia un evento AEP Web SDK al caricamento della pagina, consentendo ad Adobe Target di distribuire contenuti personalizzati.

1. Rivedi la regola completata e fai clic su **Salva**.
   ![Rivedi regola](../assets/use-cases/experiment/web-sdk-rule-review.png)

1. Per applicare le modifiche, vai a **Flusso di pubblicazione**, aggiungi la regola aggiornata a una **Libreria**.\
   ![Pubblica libreria tag](../assets/use-cases/experiment/web-sdk-rule-publish.png)

1. Infine, promuovi la libreria in **Produzione**.
   ![Pubblica tag in produzione](../assets/use-cases/experiment/web-sdk-rule-publish-production.png)

## Verificare l’implementazione del test A/B nelle pagine di AEM

Una volta che l’attività è live e la libreria Tag è stata pubblicata in produzione, puoi verificare il test A/B sulle pagine AEM.

1. Visita il sito pubblicato (ad esempio, [sito Web di abilitazione WKND](https://wknd.enablementadobe.com/us/en.html)) e osserva quale variante viene visualizzata. Prova ad accedervi da un browser diverso o da un dispositivo mobile per visualizzare varianti alternative.
   ![Visualizza varianti test A/B](../assets/use-cases/experiment/view-ab-test-variations.png)

1. Apri gli strumenti di sviluppo del browser e controlla la scheda **Rete**. Filtra per `interact` per trovare la richiesta Web SDK. La richiesta deve contenere i dettagli dell’evento Web SDK.

   ![Richiesta di rete Web SDK](../assets/use-cases/experiment/web-sdk-network-request.png)

La risposta deve includere le decisioni di personalizzazione prese da Adobe Target, indicando la variante servita.\
![Risposta Web SDK](../assets/use-cases/experiment/web-sdk-response.png)

1. In alternativa, è possibile utilizzare l&#39;estensione [Adobe Experience Platform Debugger](https://chromewebstore.google.com/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) Chrome per controllare gli eventi Web SDK.
   ![AEP Debugger](../assets/use-cases/experiment/aep-debugger-variation.png)

## Demo live

Per visualizzare il test A/B in azione, visita il [sito Web WKND Enablement](https://wknd.enablementadobe.com/us/en.html) e osserva come vengono visualizzate diverse varianti del frammento di esperienza nella home page.

## Risorse aggiuntive

- [Panoramica test A/B](https://experienceleague.adobe.com/it/docs/target/using/activities/abtest/test-ab)
- [Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/it/docs/experience-platform/web-sdk/home)
- [Panoramica sugli stream di dati](https://experienceleague.adobe.com/it/docs/experience-platform/datastreams/overview)
- [Compositore esperienza visivo](https://experienceleague.adobe.com/it/docs/target/using/experiences/vec/visual-experience-composer)
