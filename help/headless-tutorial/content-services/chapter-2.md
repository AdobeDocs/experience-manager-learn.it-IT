---
title: Capitolo 2 - Definizione dei modelli di frammento di contenuto evento - Content Services
seo-title: Getting Started with AEM Content Services - Chapter 2 - Defining Event Content Fragment Models
description: Il capitolo 2 dell’esercitazione AEM headless descrive l’abilitazione e la definizione di modelli di frammenti di contenuto utilizzati per definire una struttura dati normalizzata e un’interfaccia di authoring per la creazione di eventi.
seo-description: Chapter 2 of the AEM Headless tutorial covers enabling and defining Content Fragment Models used to define a normalized data structure and authoring interface for creating Events.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 8b05fc02-c0c5-48ad-a53e-d73b805ee91f
source-git-commit: 30c882da3a89820b5e11bc2902bb92dd0629efe9
workflow-type: tm+mt
source-wordcount: '962'
ht-degree: 10%

---

# Capitolo 2 - Utilizzo dei modelli di frammenti di contenuto

AEM Modelli per frammenti di contenuto definiscono schemi di contenuto che possono essere utilizzati per modellare la creazione di contenuti non elaborati da parte degli autori di AEM. Questo approccio è simile all’impalcatura o all’authoring basato su moduli. Il concetto chiave con Frammenti di contenuto è che il contenuto creato non è agnostico alla presentazione, il che significa che è destinato all’uso multicanale in cui l’applicazione che consuma, sia che AEM, un’applicazione a pagina singola o un’app mobile, controlla come il contenuto viene visualizzato all’utente.

La principale preoccupazione del frammento di contenuto è garantire:

1. Il contenuto corretto viene raccolto dall’autore
2. I contenuti possono essere esposti in un formato strutturato e ben conosciuto alle applicazioni più diffuse.

Questo capitolo illustra l’abilitazione e la definizione dei modelli di frammento di contenuto utilizzati per definire una struttura dati normalizzata e un’interfaccia di authoring per la modellazione e la creazione di &quot;Eventi&quot;.

## Abilita modelli di frammenti di contenuto

Modelli per frammenti di contenuto **deve** è abilitato tramite **[AEM [!UICONTROL Browser di configurazione]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=it)**.

Se i modelli di frammenti di contenuto sono **not** abilitato per una configurazione, **[!UICONTROL Crea] > [!UICONTROL Frammento di contenuto]** non verrà visualizzato per la configurazione AEM pertinente.

>[!NOTE]
>
>Le configurazioni AEM rappresentano un insieme di [configurazioni tenant basate sul contesto](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) memorizzato in `/conf`. In genere le configurazioni AEM sono correlate a un particolare sito Web gestito in AEM Sites o a un’unità aziendale responsabile di un sottoinsieme di contenuti (risorse, pagine, ecc.) in AEM.
>
>Affinché una configurazione influisca su una gerarchia di contenuti, è necessario fare riferimento alla configurazione tramite il `cq:conf` nella gerarchia del contenuto. (Questo è possibile per [!DNL WKND Mobile] configurazione in **Passaggio 5** qui sotto).
>
>Quando il `global` la configurazione viene utilizzata, la configurazione si applica a tutto il contenuto e `cq:conf` non deve essere impostato.
>
>Consulta la sezione [[!UICONTROL Browser di configurazione] documentazione](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html) per ulteriori informazioni.

1. Accedi ad AEM Author come utente con le autorizzazioni appropriate per modificare la configurazione pertinente.
   * Per questa esercitazione, la **admin** può essere utilizzato.
1. Passa a **[!UICONTROL Strumento] > [!UICONTROL Generale] > [!UICONTROL Browser di configurazione]**
1. Tocca **icona cartella** accanto a **[!DNL WKND Mobile]** per selezionare e quindi toccare il pulsante **[!UICONTROL Modifica] pulsante** in alto a sinistra.
1. Seleziona **[!UICONTROL Modelli per frammenti di contenuto]**, e tocca **[!UICONTROL Salva e chiudi]** in alto a destra.

   In questo modo è possibile abilitare i modelli di frammenti di contenuto nelle strutture di contenuto delle cartelle di risorse che dispongono di [!DNL WKND Mobile] configurazione applicata.

   >[!NOTE]
   >
   >Questa modifica della configurazione non è reversibile dal [!UICONTROL Configurazione AEM] Interfaccia utente web. Per annullare questa configurazione:
   >    
   >    1. Apri [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. Accedi a `/conf/wknd-mobile/settings/dam/cfm`
   >    1. Elimina `models` nodo

   >    
   >Eventuali modelli di frammenti di contenuto esistenti creati in questa configurazione vengono eliminati e le relative definizioni vengono memorizzate in `/conf/wknd-mobile/settings/dam/cfm/models`.

1. Applica la **[!DNL WKND Mobile]** la configurazione **[!DNL WKND Mobile]Cartella risorse** per consentire la creazione di frammenti di contenuto da modelli di frammenti di contenuto all’interno della gerarchia delle cartelle Assets:

   1. Passa a **[!UICONTROL AEM] > [!UICONTROL Risorse] > [!UICONTROL File]**
   1. Seleziona la **[!UICONTROL WKND Mobile] cartella**
   1. Tocca **[!UICONTROL Proprietà]** nella barra delle azioni superiore da aprire [!UICONTROL Proprietà cartella]
   1. In [!UICONTROL Proprietà cartella], tocca **[!UICONTROL Cloud Services]** scheda
   1. Verifica la **[!UICONTROL Configurazione cloud]** campo impostato su **/conf/wknd-mobile**
   1. Tocca **[!UICONTROL Salva e chiudi]** in alto a destra per mantenere le modifiche

>[!VIDEO](https://video.tv.adobe.com/v/28336/?quality=12&learn=on)

>[!WARNING]
>
> __Modelli per frammenti di contenuto__ sono stati spostati da __Strumenti > Risorse__ a __Strumenti > Generale__.

## Modello dei frammenti di contenuto da creare

Prima di definire il modello per i frammenti di contenuto, esaminiamo l’esperienza da utilizzare per assicurarci di acquisire tutti i punti di dati necessari. A questo scopo, esamineremo la progettazione delle applicazioni mobili e mapperemo gli elementi di progettazione in base al contenuto da raccogliere.

È possibile suddividere i punti dati che definiscono un evento come segue:

![Creazione di un modello di frammento di contenuto](assets/chapter-2/design-to-model-mapping.png)

Con la mappatura è possibile definire i frammenti di contenuto utilizzati per raccogliere ed esporre infine i dati dell’evento.

## Creazione di un modello di frammento di contenuto

1. Passa a **[!UICONTROL Strumenti] > [!UICONTROL Generale] > [!UICONTROL Modelli per frammenti di contenuto]**.
1. Tocca **[!DNL WKND Mobile]** cartella da aprire.
1. Tocca **[!UICONTROL Crea]** per aprire la procedura guidata di creazione del modello di frammento di contenuto .
1. Invio **[!DNL Event]** come **[!UICONTROL Titolo modello]** *(descrizione facoltativa)* e toccare **[!UICONTROL Crea]** da salvare.

>[!VIDEO](https://video.tv.adobe.com/v/28337/?quality=12&learn=on)

## Definizione della struttura del modello di frammento di contenuto

1. Passa a **[!UICONTROL Strumenti] > [!UICONTROL Generale] > [!UICONTROL Modelli per frammenti di contenuto] >[!DNL WKND]**.
1. Seleziona la **[!DNL Event]** Modello per frammento di contenuto e tocca **[!UICONTROL Modifica]** nella barra delle azioni superiore.
1. Da **[!UICONTROL Tipi di dati] scheda** a destra, trascina il **[!UICONTROL Ingresso testo a riga singola]** nella zona di rilascio a sinistra per definire il **[!DNL Question]** campo .
1. Assicurati che il nuovo **[!UICONTROL Ingresso testo a riga singola]** è selezionato a sinistra e il **[!UICONTROL Proprietà] scheda** è selezionato a destra. Compila i campi Proprietà come segue:

   * [!UICONTROL Rendering come] : `textfield`
   * [!UICONTROL Etichetta campo] : `Event Title`
   * [!UICONTROL Nome proprietà] : `eventTitle`
   * [!UICONTROL Lunghezza massima] : 25
   * [!UICONTROL Obbligatorio] : `Yes`

Ripeti questi passaggi utilizzando le definizioni di input definite di seguito per creare il resto del modello di frammento di contenuto evento.

>[!NOTE]
>
> La **Nome proprietà** I campi DEVONO corrispondere esattamente, in quanto l&#39;applicazione Android è programmata per chiave di questi nomi.

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
* [!UICONTROL Opzioni] : `Art,Music,Performance,Photography`

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
* [!UICONTROL Lunghezza massima] : 20
* [!UICONTROL Obbligatorio] : `Yes`

### Città

* [!UICONTROL Tipo di dati] : `Enumeration`
* [!UICONTROL Etichetta campo] : `Venue City`
* [!UICONTROL Nome proprietà] : `venueCity`
* [!UICONTROL Opzioni] : `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335/?quality=12&learn=on)

>[!NOTE]
>
>La **[!UICONTROL Nome proprietà]** denota il **entrambi** il nome della proprietà JCR in cui è memorizzato questo valore e la chiave nel file JSON . Questo deve essere un nome semantico che non verrà modificato per tutta la durata del modello di frammento di contenuto.

Dopo aver completato la creazione del modello per frammenti di contenuto, dovresti ritrovarti con una definizione simile a:


![Modello per frammento di contenuto evento](assets/chapter-2/event-content-fragment-model.png)

## Passaggio successivo

Facoltativamente, installa il [com.adobe.aem.guides.wknd-mobile.content.capitolo-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) pacchetto di contenuti su AEM Author tramite [Gestione pacchetti AEM](http://localhost:4502/crx/packmgr/index.jsp). Questo pacchetto contiene le configurazioni e il contenuto descritti in questa parte dell&#39;esercitazione.

* [Capitolo 3 - Creazione di frammenti di contenuto evento](./chapter-3.md)
