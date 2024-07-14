---
title: Capitolo 2 - Definizione dei modelli per frammenti di contenuto dell’evento - Content Services
description: Il capitolo 2 del tutorial AEM Headless tratta dell’abilitazione e della definizione dei modelli per frammenti di contenuto utilizzati per definire una struttura di dati normalizzata e un’interfaccia di authoring per la creazione di eventi.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 8b05fc02-c0c5-48ad-a53e-d73b805ee91f
duration: 378
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '940'
ht-degree: 1%

---

# Capitolo 2 - Utilizzo dei modelli per frammenti di contenuto

I modelli per frammenti di contenuto dell’AEM definiscono schemi di contenuto che possono essere utilizzati per modellare la creazione di contenuti non elaborati da parte di autori AEM. Questo approccio è simile allo scaffolding o all’authoring basato su moduli. Il concetto chiave dei frammenti di contenuto è che il contenuto creato non dipende dalla presentazione, ovvero è destinato all’uso multicanale laddove l’applicazione utente, che sia AEM, un’applicazione a pagina singola o un’app mobile, controlla il modo in cui il contenuto viene visualizzato all’utente.

La preoccupazione principale del frammento di contenuto è garantire:

1. Il contenuto corretto viene raccolto dall’autore
2. Il contenuto può essere esposto in un formato strutturato e ben conosciuto alle applicazioni che consumano.

Questo capitolo tratta come abilitare e definire i modelli per frammenti di contenuto utilizzati per definire una struttura di dati normalizzata e un’interfaccia di authoring per la modellazione e la creazione di &quot;Eventi&quot;.

## Abilita modelli per frammenti di contenuto

I modelli per frammenti di contenuto **devono** essere abilitati tramite **[AEM con [!UICONTROL Browser configurazioni]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=it)**.

Se i modelli per frammenti di contenuto sono **non** abilitati per una configurazione, il pulsante **[!UICONTROL Crea] > [!UICONTROL Frammento di contenuto]** non verrà visualizzato per la configurazione AEM pertinente.

>[!NOTE]
>
>Le configurazioni AEM rappresentano un set di [configurazioni tenant in base al contesto](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) memorizzate in `/conf`. In genere le configurazioni AEM sono correlate a un particolare sito Web gestito in AEM Sites o a una business unit responsabile di un sottoinsieme di contenuti (risorse, pagine, ecc.) nell&#39;AEM.
>
>Affinché una configurazione influisca su una gerarchia di contenuti, è necessario fare riferimento alla configurazione tramite la proprietà `cq:conf` di tale gerarchia di contenuti. Questa operazione viene eseguita per la configurazione [!DNL WKND Mobile] in **Passaggio 5** di seguito.
>
>Quando si utilizza la configurazione `global`, questa si applica a tutto il contenuto e non è necessario impostare `cq:conf`.
>
>Per ulteriori informazioni, vedere la [[!UICONTROL documentazione del browser di configurazione]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=it).

1. Accedi a AEM Author come utente con le autorizzazioni appropriate per modificare la configurazione pertinente.
   * Per questa esercitazione, è possibile utilizzare l&#39;utente **admin**.
1. Passa a **[!UICONTROL Strumento] > [!UICONTROL Generale] > [!UICONTROL Browser configurazioni]**
1. Tocca l&#39;icona **cartella** accanto a **[!DNL WKND Mobile]** per selezionarla, quindi tocca il pulsante **[!UICONTROL Modifica]** in alto a sinistra.
1. Seleziona **[!UICONTROL Modelli per frammenti di contenuto]** e tocca **[!UICONTROL Salva e chiudi]** in alto a destra.

   Ciò consente l’utilizzo di modelli per frammenti di contenuto nelle strutture del contenuto della cartella risorse a cui è applicata la configurazione [!DNL WKND Mobile].

   >[!NOTE]
   >
   >Questa modifica alla configurazione non è reversibile dall&#39;interfaccia utente Web [!UICONTROL Configurazione AEM]. Per annullare questa configurazione:
   >    
   >    1. Apri [CRXDE Liti](http://localhost:4502/crx/de)
   >    1. Passa a `/conf/wknd-mobile/settings/dam/cfm`
   >    1. Elimina il nodo `models`
   >    
   >Tutti i modelli per frammenti di contenuto esistenti creati con questa configurazione vengono eliminati e le relative definizioni memorizzate in `/conf/wknd-mobile/settings/dam/cfm/models`.

1. Applica la configurazione **[!DNL WKND Mobile]** alla cartella **[!DNL WKND Mobile]di Assets** per consentire la creazione di frammenti di contenuto da modelli per frammenti di contenuto all&#39;interno della gerarchia di cartelle di Assets:

   1. Passa a **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL File]**
   1. Seleziona la cartella **[!UICONTROL WKND Mobile]**
   1. Tocca il pulsante **[!UICONTROL Proprietà]** nella barra delle azioni superiore per aprire [!UICONTROL Proprietà cartella]
   1. In [!UICONTROL Proprietà cartella], toccare la scheda **[!UICONTROL Cloud Service]**
   1. Verificare che il campo **[!UICONTROL Configurazione cloud]** sia impostato su **/conf/wknd-mobile**
   1. Tocca **[!UICONTROL Salva e chiudi]** in alto a destra per mantenere le modifiche

>[!VIDEO](https://video.tv.adobe.com/v/28336?quality=12&learn=on)

>[!WARNING]
>
> __I modelli per frammenti di contenuto__ sono stati spostati da __Strumenti > Assets__ a __Strumenti > Generale__.

## Informazioni sul modello per frammenti di contenuto da creare

Prima di definire il modello per frammenti di contenuto, esaminiamo l’esperienza che utilizzeremo per assicurarci di acquisire tutti i punti di dati necessari. A questo scopo, esamineremo la progettazione delle applicazioni mobili e mapperemo gli elementi di progettazione in base al contenuto da raccogliere.

Possiamo suddividere i punti di dati che definiscono un Evento come segue:

![Creazione del modello per frammenti di contenuto](assets/chapter-2/design-to-model-mapping.png)

Grazie alla mappatura è possibile definire i frammenti di contenuto utilizzati per raccogliere e infine esporre i dati dell’evento.

## Creazione del modello per frammenti di contenuto

1. Passa a **[!UICONTROL Strumenti] > [!UICONTROL Generale] > [!UICONTROL Modelli per frammenti di contenuto]**.
1. Toccare la cartella **[!DNL WKND Mobile]** per aprirla.
1. Tocca **[!UICONTROL Crea]** per aprire la procedura guidata per la creazione del modello per frammenti di contenuto.
1. Immetti **[!DNL Event]** come **[!UICONTROL Titolo modello]** *(la descrizione è facoltativa)* e tocca **[!UICONTROL Crea]** per salvare.

>[!VIDEO](https://video.tv.adobe.com/v/28337?quality=12&learn=on)

## Definizione della struttura del modello per frammenti di contenuto

1. Passa a **[!UICONTROL Strumenti] > [!UICONTROL Generale] > [!UICONTROL Modelli per frammenti di contenuto] >[!DNL WKND]**.
1. Seleziona il modello per frammenti di contenuto **[!DNL Event]** e tocca **[!UICONTROL Modifica]** nella barra delle azioni superiore.
1. Dalla scheda **[!UICONTROL Tipi di dati]** a destra, trascina **[!UICONTROL L&#39;input di testo a riga singola]** nella zona di rilascio a sinistra per definire il campo **[!DNL Question]**.
1. Verificare che il nuovo **[!UICONTROL input testo a riga singola]** sia selezionato a sinistra e che la scheda **[!UICONTROL Proprietà] sia selezionata a destra.** Compila i campi Proprietà come segue:

   * [!UICONTROL Rendering come]: `textfield`
   * [!UICONTROL Etichetta campo]: `Event Title`
   * [!UICONTROL Nome proprietà]: `eventTitle`
   * [!UICONTROL Lunghezza massima] : 25
   * [!UICONTROL Obbligatorio]: `Yes`

Ripeti questi passaggi utilizzando le definizioni di input definite di seguito per creare il resto del modello per frammenti di contenuto dell’evento.

>[!NOTE]
>
> I campi **Nome proprietà** DEVONO corrispondere esattamente, in quanto l&#39;applicazione Android è programmata per escludere questi nomi.

### Descrizione evento

* [!UICONTROL Tipo di dati]: `Multi-line text`
* [!UICONTROL Etichetta campo]: `Event Description`
* [!UICONTROL Nome proprietà]: `eventDescription`
* [!UICONTROL Tipo predefinito]: `Rich text`

### Data e ora evento

* [!UICONTROL Tipo di dati]: `Date and time`
* [!UICONTROL Etichetta campo]: `Event Date and Time`
* [!UICONTROL Nome proprietà]: `eventDateAndTime`
* [!UICONTROL Obbligatorio]: `Yes`

### Tipo evento

* [!UICONTROL Tipo di dati]: `Enumeration`
* [!UICONTROL Etichetta campo]: `Event Type`
* [!UICONTROL Nome proprietà]: `eventType`
* [!UICONTROL Opzioni] : `Art,Music,Performance,Photography`

### Prezzo del biglietto

* [!UICONTROL Tipo di dati]: `Number`
* [!UICONTROL Rendering come]: `numberfield`
* [!UICONTROL Etichetta campo]: `Ticket Price`
* [!UICONTROL Nome proprietà]: `eventPrice`
* [!UICONTROL Tipo]: `Integer`
* [!UICONTROL Obbligatorio]: `Yes`

### Immagine evento

* [!UICONTROL Tipo di dati]: `Content Reference`
* [!UICONTROL Rendering come]: `contentreference`
* [!UICONTROL Etichetta campo]: `Event Image`
* [!UICONTROL Nome proprietà]: `eventImage`
* [!UICONTROL Percorso principale]: `/content/dam/wknd-mobile/images`
* [!UICONTROL Obbligatorio]: `Yes`

### Nome sede

* [!UICONTROL Tipo di dati]: `Single-line text`
* [!UICONTROL Rendering come]: `textfield`
* [!UICONTROL Etichetta campo]: `Venue Name`
* [!UICONTROL Nome proprietà]: `venueName`
* [!UICONTROL Lunghezza massima] : 20
* [!UICONTROL Obbligatorio]: `Yes`

### Città del luogo

* [!UICONTROL Tipo di dati]: `Enumeration`
* [!UICONTROL Etichetta campo]: `Venue City`
* [!UICONTROL Nome proprietà]: `venueCity`
* [!UICONTROL Opzioni] : `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335?quality=12&learn=on)

>[!NOTE]
>
>**[!UICONTROL Nome proprietà]** indica **entrambi** il nome della proprietà JCR in cui è memorizzato il valore e la chiave nel file JSON. Deve essere un nome semantico che non cambierà durante il ciclo di vita del modello per frammenti di contenuto.

Dopo aver completato la creazione del modello per frammenti di contenuto, devi ottenere una definizione simile alla seguente:


![Modello per frammenti di contenuto evento](assets/chapter-2/event-content-fragment-model.png)

## Passaggio successivo

Se necessario, installa il pacchetto di contenuti [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) su AEM Author tramite [Gestione pacchetti AEM](http://localhost:4502/crx/packmgr/index.jsp). Questo pacchetto contiene le configurazioni e il contenuto descritti in questa parte dell’esercitazione.

* [Capitolo 3 - Creazione di frammenti di contenuto di eventi](./chapter-3.md)
