---
title: Capitolo 2 - Definizione dei modelli di frammento di contenuto evento - Content Services
seo-title: Guida introduttiva a AEM Content Services - Capitolo 2 - Definizione dei modelli di frammenti di contenuto evento
description: Il capitolo 2 dell’esercitazione AEM headless descrive l’abilitazione e la definizione di modelli di frammenti di contenuto utilizzati per definire una struttura dati normalizzata e un’interfaccia di authoring per la creazione di eventi.
seo-description: Il capitolo 2 dell’esercitazione AEM headless descrive l’abilitazione e la definizione di modelli di frammenti di contenuto utilizzati per definire una struttura dati normalizzata e un’interfaccia di authoring per la creazione di eventi.
feature: Frammenti di contenuto, API
topic: Senza testa, gestione dei contenuti
role: Developer
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '998'
ht-degree: 7%

---


# Capitolo 2 - Utilizzo dei modelli di frammenti di contenuto

AEM Modelli per frammenti di contenuto definiscono schemi di contenuto che possono essere utilizzati per modellare la creazione di contenuti non elaborati da parte degli autori di AEM. Questo approccio è simile all’impalcatura o all’authoring basato su moduli. Il concetto chiave con Frammenti di contenuto è che il contenuto creato non è agnostico alla presentazione, il che significa che è destinato all’uso multicanale in cui l’applicazione che consuma, sia che AEM, un’applicazione a pagina singola o un’app mobile, controlla come il contenuto viene visualizzato all’utente.

La principale preoccupazione del frammento di contenuto è garantire:

1. Il contenuto corretto viene raccolto dall’autore
2. I contenuti possono essere esposti in un formato strutturato e ben conosciuto alle applicazioni più diffuse.

Questo capitolo illustra l’abilitazione e la definizione dei modelli di frammento di contenuto utilizzati per definire una struttura dati normalizzata e un’interfaccia di authoring per la modellazione e la creazione di &quot;Eventi&quot;.

## Abilita modelli di frammenti di contenuto

I modelli di frammenti di contenuto **devono essere abilitati** tramite **[AEM [!UICONTROL Browser di configurazione]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html)**.

Se i modelli di frammento di contenuto sono **non** abilitati per una configurazione, il pulsante **[!UICONTROL Crea] > [!UICONTROL Frammento di contenuto]** non verrà visualizzato per la configurazione AEM pertinente.

>[!NOTE]
>
>Le configurazioni di AEM rappresentano un set di [configurazioni tenant basate sul contesto](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) memorizzate in `/conf`. In genere le configurazioni AEM sono correlate a un particolare sito Web gestito in AEM Sites o a un’unità aziendale responsabile di un sottoinsieme di contenuti (risorse, pagine, ecc.) in AEM.
>
>Affinché una configurazione influisca su una gerarchia di contenuto, è necessario fare riferimento alla configurazione tramite la proprietà `cq:conf` nella gerarchia di contenuto specificata. (Questo si ottiene per la configurazione [!DNL WKND Mobile] in **Passaggio 5** qui sotto).
>
>Quando si utilizza la configurazione `global`, questa si applica a tutti i contenuti e non è necessario impostare `cq:conf`.
>
>Per ulteriori informazioni, consulta la documentazione [[!UICONTROL Browser configurazioni]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html) .

1. Accedi ad AEM Author come utente con le autorizzazioni appropriate per modificare la configurazione pertinente.
   * Per questa esercitazione, è possibile utilizzare l&#39;utente **admin** .
1. Passa a **[!UICONTROL Strumento] > [!UICONTROL Generale] > [!UICONTROL Browser configurazioni]**
1. Tocca l’ icona **cartella** accanto a **[!DNL WKND Mobile]** per selezionare, quindi tocca il pulsante **[!UICONTROL Modifica]** in alto a sinistra.
1. Seleziona **[!UICONTROL Modelli di frammento di contenuto]** e tocca **[!UICONTROL Salva e chiudi]** in alto a destra.

   In questo modo è possibile abilitare i modelli di frammenti di contenuto nelle strutture di contenuto delle cartelle di risorse a cui è stata applicata la configurazione [!DNL WKND Mobile] .

   >[!NOTE]
   >
   >Questa modifica alla configurazione non è reversibile dall&#39;interfaccia utente Web [!UICONTROL AEM Configuration]. Per annullare questa configurazione:
   >    
   >    1. Apri [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. Accedi a `/conf/wknd-mobile/settings/dam/cfm`
   >    1. Elimina il nodo `models`

   >    
   >Eventuali modelli di frammenti di contenuto esistenti creati in questa configurazione verranno eliminati e le relative definizioni verranno memorizzate in `/conf/wknd-mobile/settings/dam/cfm/models`.

1. Applica la configurazione **[!DNL WKND Mobile]** alla cartella **[!DNL WKND Mobile]Risorse** per consentire la creazione di frammenti di contenuto dai modelli di frammenti di contenuto all’interno della gerarchia di cartelle Assets:

   1. Passa a **[!UICONTROL AEM] > [!UICONTROL Risorse] > [!UICONTROL File]**
   1. Seleziona la cartella **[!UICONTROL WKND Mobile]**
   1. Tocca il pulsante **[!UICONTROL Proprietà]** nella barra delle azioni superiore per aprire [!UICONTROL Proprietà cartella]
   1. In [!UICONTROL Proprietà cartella], tocca la scheda **[!UICONTROL Cloud Services]**
   1. Verifica che il campo **[!UICONTROL Configurazione cloud]** sia impostato su **/conf/wknd-mobile**
   1. Tocca **[!UICONTROL Salva e chiudi]** in alto a destra per mantenere le modifiche

>[!VIDEO](https://video.tv.adobe.com/v/28336/?quality=12&learn=on)

## Modello dei frammenti di contenuto da creare

Prima di definire il modello Frammento di contenuto, esaminiamo l’esperienza da utilizzare per assicurarci di acquisire tutti i punti di dati necessari. A questo scopo, esamineremo la progettazione delle applicazioni mobili e mapperemo gli elementi di progettazione in base al contenuto da raccogliere.

È possibile suddividere i punti dati che definiscono un evento come segue:

![Creazione di un modello di frammento di contenuto](assets/chapter-2/design-to-model-mapping.png)

Armati della mappatura, possiamo definire il frammento di contenuto che verrà utilizzato per raccogliere ed esporre infine i dati dell’evento.

## Creazione di un modello di frammento di contenuto

1. Passa a **[!UICONTROL Strumenti] > [!UICONTROL Risorse] > [!UICONTROL Modelli di frammenti di contenuto]**.
1. Tocca la cartella **[!DNL WKND Mobile]** per aprire.
1. Tocca **[!UICONTROL Crea]** per aprire la procedura guidata di creazione del modello di frammento di contenuto .
1. Inserisci **[!DNL Event]** come **[!UICONTROL Titolo modello]** *(la descrizione è facoltativa)* e tocca **[!UICONTROL Crea]** per salvare.

>[!VIDEO](https://video.tv.adobe.com/v/28337/?quality=12&learn=on)

## Definizione della struttura del modello di frammento di contenuto

1. Passa a **[!UICONTROL Strumenti] > [!UICONTROL Risorse] > [!UICONTROL Modelli di frammenti di contenuto] >[!DNL WKND]**.
1. Seleziona il **[!DNL Event]** modello di frammento di contenuto e tocca **[!UICONTROL Modifica]** nella barra delle azioni superiore.
1. Dalla scheda **[!UICONTROL Tipi di dati]** a destra, trascina l’ **[!UICONTROL immissione di testo a riga singola]** nella zona di rilascio a sinistra per definire il campo **[!DNL Question]** .
1. Assicurati che il nuovo **[!UICONTROL input di testo a riga singola]** sia selezionato a sinistra e che la scheda **[!UICONTROL Proprietà]** sia selezionata a destra. Compila i campi Proprietà come segue:

   * [!UICONTROL Rendering come] : `textfield`
   * [!UICONTROL Etichetta campo] : `Event Title`
   * [!UICONTROL Nome proprietà] : `eventTitle`
   * [!UICONTROL Lunghezza]  massima: 25
   * [!UICONTROL Obbligatorio] : `Yes`

Ripeti questi passaggi utilizzando le definizioni di input definite di seguito per creare il resto del modello di frammento di contenuto evento.

>[!NOTE]
>
> I campi **Nome proprietà** DEVONO corrispondere esattamente, in quanto l&#39;applicazione Android è programmata per chiave di questi nomi.

### Descrizione evento

* [!UICONTROL Tipo di dati] : `Multi-line text`
* [!UICONTROL Etichetta campo] : `Event Description`
* [!UICONTROL Nome proprietà] : `eventDescription`
* [!UICONTROL Tipo predefinito] : `Rich text`

### Data e ora dell’evento

* [!UICONTROL Tipo di dati] : `Date and time`
* [!UICONTROL Etichetta campo] : `Event Date and Time`
* [!UICONTROL Nome proprietà] : `eventDateAndTime`
* [!UICONTROL Obbligatorio] : `Yes`

### Tipo evento

* [!UICONTROL Tipo di dati] : `Enumeration`
* [!UICONTROL Etichetta campo] : `Event Type`
* [!UICONTROL Nome proprietà] : `eventType`
* [!UICONTROL Opzioni] :  `Art,Music,Performance,Photography`

### Prezzo del biglietto

* [!UICONTROL Tipo di dati] : `Number`
* [!UICONTROL Rendering come] : `numberfield`
* [!UICONTROL Etichetta campo] : `Ticket Price`
* [!UICONTROL Nome proprietà] : `eventPrice`
* [!UICONTROL Tipo] : `Integer`
* [!UICONTROL Obbligatorio] : `Yes`

### Immagine evento

* [!UICONTROL Tipo di dati] : `Content Reference`
* [!UICONTROL Rendering come] : `contentreference`
* [!UICONTROL Etichetta campo] : `Event Image`
* [!UICONTROL Nome proprietà] : `eventImage`
* [!UICONTROL Percorso directory principale] : `/content/dam/wknd-mobile/images`
* [!UICONTROL Obbligatorio] : `Yes`

### Nome evento

* [!UICONTROL Tipo di dati] : `Single-line text`
* [!UICONTROL Rendering come] : `textfield`
* [!UICONTROL Etichetta campo] : `Venue Name`
* [!UICONTROL Nome proprietà] : `venueName`
* [!UICONTROL Lunghezza]  massima: 20
* [!UICONTROL Obbligatorio] : `Yes`

### Città

* [!UICONTROL Tipo di dati] : `Enumeration`
* [!UICONTROL Etichetta campo] : `Venue City`
* [!UICONTROL Nome proprietà] : `venueCity`
* [!UICONTROL Opzioni] :  `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335/?quality=12&learn=on)

>[!NOTE]
>
>Il **[!UICONTROL Nome proprietà]** indica sia **sia** il nome della proprietà JCR in cui verrà memorizzato questo valore, sia la chiave nel file JSON . Questo deve essere un nome semantico che non verrà modificato per tutta la durata del modello di frammento di contenuto.

Dopo aver completato la creazione del modello per frammenti di contenuto, dovresti ritrovarti con una definizione simile a:


![Modello per frammento di contenuto evento](assets/chapter-2/event-content-fragment-model.png)

## Passaggio successivo

Facoltativamente, installa il pacchetto di contenuti [com.adobe.aem.guides.wknd-mobile.content.capitolo-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) su AEM Author tramite [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp). Questo pacchetto contiene le configurazioni e il contenuto descritti in questa parte dell&#39;esercitazione.

* [Capitolo 3 - Creazione di frammenti di contenuto evento](./chapter-3.md)
