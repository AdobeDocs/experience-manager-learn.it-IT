---
title: Capitolo 2 - Definizione dei modelli di frammento di contenuto evento
seo-title: Guida introduttiva a AEM Content Services - Capitolo 2 - Definizione dei modelli di frammenti di contenuto evento
description: Il capitolo 2 dell'esercitazione AEM Headless descrive come abilitare e definire i modelli di frammenti di contenuto utilizzati per definire una struttura dati normalizzata e un'interfaccia di authoring per la creazione di eventi.
seo-description: Il capitolo 2 dell'esercitazione AEM Headless descrive come abilitare e definire i modelli di frammenti di contenuto utilizzati per definire una struttura dati normalizzata e un'interfaccia di authoring per la creazione di eventi.
translation-type: tm+mt
source-git-commit: 885e30dea2a21dff789c98bdc5beb2f758b806f3
workflow-type: tm+mt
source-wordcount: '968'
ht-degree: 7%

---


# Capitolo 2 - Utilizzo dei modelli di frammenti di contenuto

AEM Modelli di frammenti di contenuto definiscono schemi di contenuto che possono essere utilizzati per modellare la creazione di contenuto non elaborato da AEM autori. Questo approccio è simile alla creazione di ponteggi o di moduli. Il concetto chiave con i frammenti di contenuto è che il contenuto creato è agnostico alla presentazione, il che significa che è destinato all&#39;uso multicanale, dove l&#39;applicazione che consuma, sia che AEM, un&#39;applicazione a pagina singola o un&#39;app mobile, controlla come il contenuto viene visualizzato all&#39;utente.

La preoccupazione principale del frammento di contenuto è garantire quanto segue:

1. Il contenuto corretto viene raccolto dall’autore
2. I contenuti possono essere esposti in un formato strutturato e comprensibile alle applicazioni più utilizzate.

Questo capitolo descrive l&#39;abilitazione e la definizione dei modelli di frammenti di contenuto utilizzati per definire una struttura dati normalizzata e un&#39;interfaccia di authoring per la modellazione e la creazione di &quot;Eventi&quot;.

## Abilita modelli di frammenti di contenuto

I modelli di frammento di contenuto **devono** essere attivati tramite **AEM[!UICONTROL browser]** di configurazione.

Se i modelli di frammento di contenuto **non** sono abilitati per una configurazione, il pulsante **[!UICONTROL Crea]> Frammento[!UICONTROL di]** contenuto non verrà visualizzato per la configurazione AEM pertinente.

>[!NOTE]
>
>AEM configurazioni rappresentano una serie di configurazioni [tenant basate sul](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) contesto memorizzate in `/conf`. In genere AEM configurazioni si correlano a un particolare sito Web gestito in  AEM Sites o in un&#39;unità aziendale responsabile di un sottoinsieme di contenuti (risorse, pagine, ecc.) in AEM.
>
>Affinché una configurazione influisca sulla gerarchia del contenuto, è necessario fare riferimento alla configurazione tramite la `cq:conf` proprietà nella gerarchia del contenuto. (Questo viene ottenuto per la [!DNL WKND Mobile] configurazione nel **passaggio 5** seguente).
>
>Quando si utilizza la `global` configurazione, questa si applica a tutto il contenuto e non `cq:conf` è necessario impostarla.

1. Accedete ad AEM Author come utente con le autorizzazioni appropriate per modificare la configurazione pertinente.
   * Per questa esercitazione, è possibile utilizzare l&#39;utente **admin** .
1. Passa a **[!UICONTROL Strumento]>[!UICONTROL Generale]> Browser[!UICONTROL di configurazione]**
1. Toccate l’icona **della** cartella accanto a **[!DNL WKND Mobile]** per selezionarla, quindi toccate il pulsante **Modifica** in alto a sinistra.
1. Selezionate Modelli **[!UICONTROL di frammenti di]** contenuto e toccate **[!UICONTROL Salva e chiudi]** in alto a destra.

   Questo consente di visualizzare i modelli di frammenti di contenuto nelle strutture di contenuto delle cartelle di risorse a cui è stata applicata la [!DNL WKND Mobile] configurazione.

   >[!NOTE]
   >
   >Questa modifica alla configurazione non è reversibile dall&#39;interfaccia utente Web di configurazione  AEM. Per annullare questa configurazione:
   >    
   >    1. Open [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. Accedi a `/conf/wknd-mobile/settings/dam/cfm`
   >    1. Elimina il `models` nodo

   >    
   >Eventuali modelli di frammento di contenuto esistenti creati in questa configurazione verranno eliminati e le relative definizioni verranno memorizzate in `/conf/wknd-mobile/settings/dam/cfm/models`.

1. Applica la **[!DNL WKND Mobile]** configurazione alla cartella **[!DNL WKND Mobile]** Assets per consentire la creazione di frammenti di contenuto dai modelli di frammenti di contenuto all’interno della gerarchia delle cartelle Assets:

   1. Passa a **[!UICONTROL AEM]>[!UICONTROL Risorse]>[!UICONTROL File]**
   1. Selezionate la cartella **[!UICONTROL WKND Mobile]**
   1. Toccate il pulsante **[!UICONTROL Proprietà]** nella barra delle azioni superiore per aprire Proprietà [!UICONTROL cartella]
   1. In Proprietà cartella, toccate la scheda **[!UICONTROL Cloud Services]**
   1. Verifica che il campo Configurazione **** cloud sia impostato su **/conf/wknd-mobile**
   1. Toccate **[!UICONTROL Salva e chiudi]** in alto a destra per salvare le modifiche

>[!VIDEO](https://video.tv.adobe.com/v/28336/?quality=12&learn=on)

## Modello frammento di contenuto da creare

Prima di definire il modello di frammento di contenuto, rivediamo l&#39;esperienza che utilizzeremo per assicurarci di acquisire tutti i punti dati necessari. Per questo, esamineremo la progettazione delle applicazioni per dispositivi mobili e mapperemo gli elementi di progettazione sulla raccolta dei contenuti.

È possibile suddividere i punti dati che definiscono un evento come segue:

![Creazione di un modello di frammento di contenuto](assets/chapter-2/design-to-model-mapping.png)

Con la mappatura è possibile definire il frammento di contenuto che verrà utilizzato per raccogliere ed esporre i dati dell’evento.

## Creazione di un modello di frammento di contenuto

1. Passa a **[!UICONTROL Strumenti]>[!UICONTROL Risorse]> Modelli di frammenti di[!UICONTROL contenuto]**.
1. Toccate la **[!DNL WKND Mobile]** cartella per aprirla.
1. Toccate **[!UICONTROL Crea]** per aprire la procedura guidata di creazione del modello di frammento di contenuto.
1. Immettete **[!DNL Event]** come Titolo **** modello *(la descrizione è facoltativa)* e toccate **[!UICONTROL Crea]** per salvare.

>[!VIDEO](https://video.tv.adobe.com/v/28337/?quality=12&learn=on)

## Definizione della struttura del modello di frammento di contenuto

1. Passa a **[!UICONTROL Strumenti]>[!UICONTROL Risorse]> Modelli[!UICONTROL frammenti di]contenuto >[!DNL WKND]**.
1. Selezionare il modello di frammento di **[!DNL Event]** contenuto e toccare **[!UICONTROL Modifica]** nella barra delle azioni superiore.
1. Dalla scheda **[!UICONTROL Tipi]di dati** a destra, trascina il testo della riga **[!UICONTROL singola]** nella zona di rilascio a sinistra per definire il **[!DNL Question]** campo.
1. Verificare che la nuova immissione **[!UICONTROL di testo su riga]** singola sia selezionata a sinistra, mentre la scheda **Proprietà** sia selezionata a destra. Compilare i campi Proprietà come segue:

   * [!UICONTROL Rendering come] : `textfield`
   * [!UICONTROL Etichetta campo] : `Event Title`
   * [!UICONTROL Nome proprietà] : `eventTitle`
   * [!UICONTROL Lunghezza] massima : 25
   * [!UICONTROL Obbligatorio] : `Yes`

Ripetete questi passaggi utilizzando le definizioni di input definite di seguito per creare il resto del modello di frammento di contenuto evento.

>[!NOTE]
>
> I campi Nome **** proprietà DEVONO corrispondere esattamente, in quanto l&#39;applicazione Android è programmata per chiave di questi nomi.

### Descrizione evento

* [!UICONTROL Tipo di dati] : `Multi-line text`
* [!UICONTROL Etichetta campo] : `Event Description`
* [!UICONTROL Nome proprietà] : `eventDescription`
* [!UICONTROL Tipo predefinito] : `Rich text`

### Data e ora evento

* [!UICONTROL Tipo di dati] : `Date and time`
* [!UICONTROL Etichetta campo] : `Event Date and Time`
* [!UICONTROL Nome proprietà] : `eventDateAndTime`
* [!UICONTROL Obbligatorio] : `Yes`

### Tipo evento

* [!UICONTROL Tipo di dati] : `Enumeration`
* [!UICONTROL Etichetta campo] : `Event Type`
* [!UICONTROL Nome proprietà] : `eventType`
* [!UICONTROL Opzioni] : `Art,Music,Performance,Photography`

### Prezzo biglietto

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
* [!UICONTROL Lunghezza] massima : 20
* [!UICONTROL Obbligatorio] : `Yes`

### Città di Venue

* [!UICONTROL Tipo di dati] : `Enumeration`
* [!UICONTROL Etichetta campo] : `Venue City`
* [!UICONTROL Nome proprietà] : `venueCity`
* [!UICONTROL Opzioni] : `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335/?quality=12&learn=on)

>[!NOTE]
>
>Il Nome **** proprietà indica **sia** il nome della proprietà JCR in cui verrà memorizzato il valore, sia la chiave nel file JSON. Deve essere un nome semantico che non verrà modificato per tutta la durata del modello di frammento di contenuto.

Dopo aver completato la creazione del modello di frammento di contenuto, è necessario terminare con una definizione simile a:


![Modello frammento di contenuto evento](assets/chapter-2/event-content-fragment-model.png)

## Passaggio successivo

Se necessario, installate il pacchetto di contenuti [com.adobe.aem.guide.wknd-mobile.content.Chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) su AEM Author tramite [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp). Questo pacchetto contiene le configurazioni e il contenuto descritti in questa parte dell&#39;esercitazione.

* [Capitolo 3 - Creazione di frammenti di contenuto evento](./chapter-3.md)
