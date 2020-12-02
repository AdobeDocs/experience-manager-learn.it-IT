---
title: Guida introduttiva a AEM headless - Capitolo 7 - Utilizzo di AEM Content Services da un'app mobile
description: Il capitolo 7 dell'esercitazione esegue l'app mobile Android per utilizzare il contenuto creato da AEM Content Services.
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '1404'
ht-degree: 0%

---


# Capitolo 7 - Utilizzo di AEM Content Services da un&#39;app mobile

Il capitolo 7 dell&#39;esercitazione utilizza un&#39;app mobile Android nativa per utilizzare il contenuto da AEM Content Services.

## L&#39;app mobile Android

Questa esercitazione utilizza una **semplice app mobile Android nativa** per utilizzare e visualizzare il contenuto dell&#39;evento esposto da AEM Content Services.

L&#39;utilizzo di [Android](https://developer.android.com/) è in gran parte irrilevante e l&#39;app mobile utilizzata potrebbe essere scritta in qualsiasi framework per qualsiasi piattaforma mobile, ad esempio iOS.

Android è utilizzato per esercitazione a causa della capacità di eseguire un Emulatore Android su Windows, macOs e Linux, la sua popolarità, e che può essere scritto come Java, una lingua ben compresa dagli sviluppatori AEM.

*L&#39;app mobile Android dell&#39;esercitazione ****non ha lo scopo di indicare come creare le app Android Mobile o trasmettere le best practice di sviluppo Android, ma piuttosto di illustrare come AEM Content Services può essere utilizzato da un&#39;applicazione mobile.*

### Come AEM Content Services stimola l&#39;esperienza dell&#39;app mobile

![Mappatura app mobile a Content Services](assets/chapter-7/content-services-mapping.png)

1. Il **logo** come definito dal [!DNL Events API] componente immagine **della pagina**.
1. Linea di tag **come definita nel [!DNL Events API] componente di testo** della pagina **.**
1. Questo **elenco eventi** è derivato dalla serializzazione dei frammenti di contenuto evento, esposti tramite il componente **Elenco frammenti di contenuto** configurato.

## Dimostrazione delle app mobili

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### Configurazione dell’app mobile per uso non localhost

Se AEM Publish non viene eseguito su **http://localhost:4503**, l&#39;host e la porta possono essere aggiornati nell&#39;app mobile [!DNL Settings] per puntare alla proprietà AEM Publish host/port.

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## Esecuzione locale dell&#39;app mobile

1. Scaricate e installate [Android Studio](https://developer.android.com/studio/install) per installare Android Emulator.
1. **** Scaricate il  [!DNL APK] file Android  [GitHub > Risorse > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Apri **Android Studio**
   * All&#39;avvio iniziale di Android Studio, verrà visualizzata una richiesta di installazione di [!DNL Android SDK]. Accettate le impostazioni predefinite e completate l’installazione.
1. Apri Android Studio e seleziona **Profilo o Debug APK**
1. Selezionate il file APK (**wknd-mobile.x.x.apk**) scaricato al passaggio 2 e fate clic su **OK**
   * Se viene richiesto di **Creare una nuova cartella**, oppure **Utilizzare un elemento esistente**, selezionare **Usa elemento esistente**.
1. All&#39;avvio iniziale di Android Studio, fare clic con il pulsante destro del mouse su **wknd-mobile.x.x.x** nell&#39;elenco Progetti, quindi selezionare **Impostazioni modulo aperto**.
   * In **Moduli > wknd-mobile.x.x > scheda Dipendenze**, selezionare **Piattaforma API 29 Android**. Toccate OK per chiudere e salvare le modifiche.
   * In caso contrario, quando si tenta di avviare l&#39;emulatore verrà visualizzato un errore &quot;Seleziona Android SDK&quot;.
1. Aprite il **AVD Manager** selezionando **Strumenti > AVD Manager** oppure toccando l&#39;icona **AVD Manager** nella barra superiore.
1. Nella finestra **AVD Manager**, fare clic su **+ Crea dispositivo virtuale...** se il dispositivo non è già registrato.
   1. A sinistra, selezionate la categoria **Phone**.
   1. Selezionare un **Pixel 2**.
   1. Fare clic sul pulsante **Next**.
   1. Selezionare **Q** con **API Level 29**.
      * All&#39;avvio iniziale di AVD Manager, vi verrà chiesto di scaricare l&#39;API con versione. Fate clic sul collegamento Scarica accanto alla versione &quot;D&quot; e completate il download e l&#39;installazione.
   1. Fare clic sul pulsante **Next**.
   1. Fare clic sul pulsante **Fine**.
1. Chiudere la finestra **AVD Manager**.
1. Nella barra dei menu superiore, selezionate **wknd-mobile.x.x.x** dal menu a discesa **Esegui/Modifica configurazioni**.
1. Toccate il pulsante **Esegui** accanto al **Esegui/Modifica configurazione** selezionato
1. Nella finestra a comparsa, selezionare il dispositivo virtuale appena creato **[!DNL Pixel 2 API 29]** e toccare **OK**
1. Se l&#39;app [!DNL WKND Mobile] non viene caricata immediatamente, trova e tocca l&#39;icona **[!DNL WKND]** dalla schermata iniziale Android nell&#39;emulatore.
   * Se l&#39;emulatore si avvia ma lo schermo dell&#39;emulatore rimane nero, toccate il pulsante **power** nella finestra degli strumenti dell&#39;emulatore accanto alla finestra dell&#39;emulatore.
   * Per scorrere all’interno del dispositivo virtuale, fate clic e tenete premuto e trascinate.
   * Per aggiornare i contenuti da AEM, trascinateli dall’alto fino all’icona Aggiorna
e rilascio.

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## Il codice dell&#39;app mobile

Questa sezione evidenzia il codice dell&#39;app mobile Android che interagisce maggiormente e dipende da AEM Content Services ed è l&#39;output JSON.

Al momento del caricamento, l&#39;app mobile effettua `HTTP GET` su `/content/wknd-mobile/en/api/events.model.json`, che è il punto finale di AEM Content Services configurato per fornire il contenuto per l&#39;unità dell&#39;app mobile.

Poiché il Modello modificabile dell&#39;API Eventi (`/content/wknd-mobile/en/api/events.model.json`) è bloccato, l&#39;app mobile può essere codificata per cercare informazioni specifiche in posizioni specifiche nella risposta JSON.

### Flusso di codice di alto livello

1. L&#39;apertura dell&#39;app [!DNL WKND Mobile] richiama una richiesta `HTTP GET` ad AEM Publish su `/content/wknd-mobile/en/api/events.model.json` per raccogliere il contenuto per compilare l&#39;interfaccia dell&#39;app mobile.
2. Dopo aver ricevuto il contenuto da AEM, ciascuno dei tre elementi di visualizzazione dell&#39;app mobile, il **logo, la riga di tag e l&#39;elenco eventi** vengono inizializzati con il contenuto da AEM.
   * Per eseguire un binding al contenuto AEM con l&#39;elemento di visualizzazione dell&#39;app mobile, il JSON che rappresenta ciascun componente AEM è mappato a un oggetto Java POJO, che a sua volta è associato all&#39;elemento di visualizzazione Android.
      * Componente immagine JSON → Logo POJO → Logo ImageView
      * JSON del componente di testo → POJO della riga di tag → Text ImageView
      * Elenco frammenti di contenuto JSON → Eventi POJO → Eventi RecyclerView
   * *Il codice dell’app mobile è in grado di mappare il JSON ai POJO a causa delle posizioni note all’interno della risposta JSON maggiore. Ricorda che le chiavi JSON di &quot;image&quot;, &quot;text&quot; e &quot;content frammentlist&quot; sono dettate dai nomi dei nodi dei componenti di AEM supporto. Se questi nomi di nodo cambiano, l&#39;app mobile si interrompe perché non sa come sorgente il contenuto richiesto dai dati JSON.*

#### Richiamo del punto finale di AEM Content Services

Di seguito è riportata una distillazione del codice nell&#39;app mobile `MainActivity` responsabile della chiamata AEM Content Services per raccogliere il contenuto che guida l&#39;esperienza App mobile.

```
protected void onCreate(Bundle savedInstanceState) {
    ...
    // Create the custom objects that will map the JSON -> POJO -> View elements
    final List<ViewBinder> viewBinders = new ArrayList<>();

    viewBinders.add(new LogoViewBinder(this, getAemHost(), (ImageView) findViewById(R.id.logo)));
    viewBinders.add(new TagLineViewBinder(this, (TextView) findViewById(R.id.tagLine)));
    viewBinders.add(new EventsViewBinder(this, getAemHost(), (RecyclerView) findViewById(R.id.eventsList)));
    ...
    initApp(viewBinders);
}

private void initApp(final List<ViewBinder> viewBinders) {
    final String aemUrl = getAemUrl(); // -> http://localhost:4503/content/wknd-mobile/en/api/events.mobile.json
    JsonObjectRequest request = new JsonObjectRequest(aemUrl, null,
        new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                for (final ViewBinder viewBinder : viewBinders) {
                    viewBinder.bind(response);
                }
            }
        },
        ...
    );
}
```

`onCreate(..)` è il gancio di inizializzazione per l&#39;app mobile e registra i 3  `ViewBinders` responsabili personalizzati per l&#39;analisi JSON e il binding dei valori agli  `View` elementi.

`initApp(...)` Viene quindi chiamata la richiesta di GET HTTP al punto finale di AEM Content Services su AEM Publish per raccogliere il contenuto. Dopo aver ricevuto una risposta JSON valida, la risposta JSON viene passata a ogni `ViewBinder` responsabile dell&#39;analisi del JSON e del binding degli elementi `View` mobili.

#### Analisi della risposta JSON

In seguito vedremo il `LogoViewBinder`, che è semplice, ma evidenzia diverse considerazioni importanti.

```
public class LogoViewBinder implements ViewBinder {
    ...
    public void bind(JSONObject jsonResponse) throws JSONException, IOException {
        final JSONObject components = jsonResponse.getJSONObject(":items").getJSONObject("root").getJSONObject(":items");

        if (components.has("image")) {
            final Image image = objectMapper.readValue(components.getJSONObject("image").toString(), Image.class);

            final String imageUrl = aemHost + image.getSrc();
            Glide.with(context).load(imageUrl).into(logo);
        } else {
            Log.d("WKND", "Missing Logo");
        }
    }
}
```

La prima riga di `bind(...)` naviga verso il basso nella risposta JSON tramite le chiavi **:items → root → :items** che rappresentano il contenitore AEM layout a cui sono stati aggiunti i componenti.

Da qui viene effettuato un controllo per una chiave denominata **image**, che rappresenta il componente Immagine (ancora, è importante che il nome del nodo → chiave JSON sia stabile). Se questo oggetto esiste, è stato letto e mappato sull&#39; [immagine POJO](#image-pojo) tramite la libreria Jackson `ObjectMapper`. L’immagine POJO è illustrata di seguito.

Infine, il logo `src` viene caricato in Android ImageView utilizzando la libreria helper [!DNL Glide].

È necessario fornire lo schema, l&#39;host e la porta AEM (tramite `aemHost`) all&#39;istanza AEM Publish, in quanto AEM Content Services fornirà solo il percorso JCR (ad esempio. `/content/dam/wknd-mobile/images/wknd-logo.png`) al contenuto di riferimento.

#### Immagine POJO{#image-pojo}

Anche se opzionale, l&#39;utilizzo di [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) o di funzionalità simili fornite da altre librerie come Gson consente di mappare complesse strutture JSON a JSON POJO Java senza il tedio di dover gestire direttamente gli oggetti JSON nativi stessi. In questo caso semplice, mappamo la chiave `src` dall&#39;oggetto JSON `image` all&#39;attributo `src` nel POJO immagine direttamente tramite l&#39;annotazione `@JSONProperty`.

```
package com.adobe.aem.guides.wknd.mobile.android.models;

import com.fasterxml.jackson.annotation.JsonProperty;

public class Image {
    @JsonProperty(value = "src")
    private String src;

    public String getSrc() {
        return src;
    }
}
```

Il POJO evento, che richiede la selezione di molti più punti dati dall&#39;oggetto JSON, beneficia di questa tecnica più che della semplice immagine, che tutto ciò che vogliamo è il `src`.

## Esplora l&#39;esperienza dell&#39;app mobile

Ora che hai una conoscenza di come AEM Content Services possa promuovere l&#39;esperienza mobile nativa, usa ciò che hai imparato a eseguire i seguenti passaggi e vedi le tue modifiche riflesse nell&#39;app mobile.

Dopo ogni passaggio, fai clic per aggiornare l&#39;app mobile e verifica l&#39;aggiornamento all&#39;esperienza mobile.

1. Creare e pubblicare **nuovo [!DNL Event] frammento di contenuto**
1. Annullare la pubblicazione di un **frammento di contenuto esistente [!DNL Event]**
1. Pubblicare un aggiornamento sulla **riga di tag**

## Congratulazioni

**Hai completato l&#39;esercitazione AEM senza testa!**

Per saperne di più su AEM Content Services e AEM come CMS headless, visita  Adobe altro materiale di documentazione e abilitazione:

* [Utilizzo di frammenti di contenuto](../sites/content-fragments/understand-content-fragments-and-experience-fragments.md)
* [Guida utente per i componenti core di AEM WCM](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/introduction.html)
* [AEM libreria dei componenti core di WCM](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM WCM Core Components GitHub Project](https://github.com/adobe/aem-core-wcm-components)
* [AEM componenti core WCM - Chiedi all&#39;esperto](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-content-services.html)
* [Esempio di codice per l’esportazione di componenti](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/bundle/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
