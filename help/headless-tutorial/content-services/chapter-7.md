---
title: Capitolo 7 - Utilizzo di servizi di contenuti AEM da un’app mobile - Content Services
description: Il capitolo 7 del tutorial esegue l’app mobile Android per utilizzare contenuti creati da AEM Content Services.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1391'
ht-degree: 0%

---

# Capitolo 7 - Utilizzo di servizi di contenuti AEM da un’app mobile

Il capitolo 7 del tutorial utilizza un’app mobile Android nativa per utilizzare contenuti da AEM Content Services.

## L&#39;app mobile Android

Questa esercitazione utilizza un **app mobile Android nativa semplice** per utilizzare e visualizzare il contenuto di eventi esposto da AEM Content Services.

L&#39;uso di [Android](https://developer.android.com/) è in gran parte poco importante e l’app mobile che consuma potrebbe essere scritta in qualsiasi framework per qualsiasi piattaforma mobile, ad esempio iOS.

Android è utilizzato per i tutorial a causa della capacità di eseguire un emulatore Android su Windows, macOs e Linux, della sua popolarità, e che può essere scritto come Java, un linguaggio ben compreso dagli sviluppatori AEM.

*L&#39;app mobile Android del tutorial è&#x200B;**non**Con lo scopo di istruire su come creare app Android Mobile o trasmettere le best practice di sviluppo Android, ma piuttosto per illustrare come i Content Services per l’AEM possono essere utilizzati da un’applicazione mobile.*

### Come AEM Content Services guida l&#39;esperienza dell&#39;app mobile

![Mappatura tra app mobile e Content Services](assets/chapter-7/content-services-mapping.png)

1. Il **logo** come definito dal [!DNL Events API] della pagina **Componente immagine**.
1. Il **linea di tag** come definito nella [!DNL Events API] della pagina **Componente testo**.
1. Questo **Elenco eventi** è derivato dalla serializzazione dei frammenti di contenuto dell’evento, esposti tramite il **Componente Elenco frammenti di contenuto**.

## Dimostrazione sull’app mobile

>[!VIDEO](https://video.tv.adobe.com/v/28345?quality=12&learn=on)

### Configurazione dell’app mobile per un utilizzo non localhost

Se la pubblicazione AEM non viene eseguita su **http://localhost:4503** l&#39;host e la porta possono essere aggiornati nel [!DNL Settings] per puntare alla proprietà AEM Publish host/port.

>[!VIDEO](https://video.tv.adobe.com/v/28344?quality=12&learn=on)

## Esecuzione locale dell’app mobile

1. Scarica e installa [Android Studio](https://developer.android.com/studio/install) per installare l’emulatore Android.
1. **Scarica** Android [!DNL APK] file [GitHub > Risorse > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Apri **Android Studio**
   * Al primo avvio di Android Studio, viene richiesto di installare [!DNL Android SDK] presenterà. Accettare le impostazioni predefinite e completare l&#39;installazione.
1. Apri Android Studio e seleziona **Profilo o debug APK**
1. Selezionare il file APK (**wknd-mobile.x.x.x.apk**) scaricata nel passaggio 2 e fare clic su **OK**
   * Se richiesto **Creare una nuova cartella**, o **Usa esistente**, seleziona **Usa esistente**.
1. All’avvio iniziale di Android Studio, fai clic con il pulsante destro del mouse sul **wknd-mobile.x.x.x** nell&#39;elenco Progetti e selezionare **Impostazioni modulo aperto**.
   * Sotto **Moduli > wknd-mobile.x.x.x > scheda Dipendenze**, seleziona **Piattaforma API 29 Android**. Toccare OK per chiudere e salvare le modifiche.
   * In caso contrario, verrà visualizzato un errore &quot;Please select Android SDK&quot; (Seleziona Android SDK) quando si tenta di avviare l’emulatore.
1. Apri **AVD Manager** selezionando **Strumenti > AVD Manager** o toccando il **AVD Manager** nella barra superiore.
1. In **AVD Manager** finestra, fai clic su **+ Crea dispositivo virtuale...** se il dispositivo non è già stato registrato.
   1. A sinistra, seleziona la **Telefono** categoria.
   1. Seleziona un **Pixel 2**.
   1. Fai clic su **Successivo** pulsante.
   1. Seleziona **Q** con **Livello API 29**.
      * All’avvio iniziale di AVD Manager, ti viene richiesto di scaricare l’API con versione. Fai clic sul collegamento Download accanto alla versione &quot;Q&quot;, quindi completa il download e l’installazione.
   1. Fai clic su **Successivo** pulsante.
   1. Fai clic su **Fine** pulsante.
1. Chiudi **AVD Manager** finestra.
1. Nella barra dei menu superiore, seleziona **wknd-mobile.x.x.x** dal **Esegui/Modifica configurazioni** a discesa.
1. Tocca il **Esegui** accanto al pulsante selezionato **Esegui/Modifica configurazione**
1. Nel pop-up, seleziona il nuovo creato **[!DNL Pixel 2 API 29]** dispositivo virtuale e tocca **OK**
1. Se il [!DNL WKND Mobile] L&#39;app non si carica immediatamente, non trova e tocca il **[!DNL WKND]** dalla schermata iniziale di Android nell’emulatore.
   * Se l’emulatore viene avviato ma lo schermo dell’emulatore rimane nero, tocca il **alimentazione** nella finestra degli strumenti dell’emulatore accanto alla finestra dell’emulatore.
   * Per scorrere all&#39;interno del dispositivo virtuale, fare clic e tenere premuto e trascinare.
   * Per aggiornare il contenuto dall’AEM, spingi verso il basso dall’alto fino a quando viene visualizzata l’icona Aggiorna, quindi rilascia.

>[!VIDEO](https://video.tv.adobe.com/v/28341?quality=12&learn=on)

## Il codice dell’app mobile

Questa sezione evidenzia il codice dell’app mobile Android che più interagisce e dipende da AEM Content Services e dal suo output JSON.

Al momento del caricamento, l’app mobile effettua `HTTP GET` a `/content/wknd-mobile/en/api/events.model.json` che è l’endpoint di AEM Content Services configurato per fornire il contenuto necessario per gestire l’app mobile.

Perché il modello modificabile dell’API degli eventi (`/content/wknd-mobile/en/api/events.model.json`) è bloccata, l’app mobile può essere codificata per cercare informazioni specifiche in posizioni specifiche nella risposta JSON.

### Flusso di codice di alto livello

1. Apertura di [!DNL WKND Mobile] L’app richiama un `HTTP GET` richiesta al AEM Pubblica su `/content/wknd-mobile/en/api/events.model.json` per raccogliere i contenuti da inserire nell’interfaccia utente dell’app mobile.
2. Dopo aver ricevuto il contenuto dall’AEM, ciascuno dei tre elementi di visualizzazione dell’app mobile, **logo, linea di tag ed elenco eventi**, sono inizializzati con il contenuto dell&#39;AEM.
   * Per associare il contenuto AEM all’elemento di visualizzazione dell’app mobile, il JSON che rappresenta ciascun componente AEM è un oggetto mappato su un POJO Java, che a sua volta è associato all’elemento di visualizzazione Android.
      * Componente immagine JSON → Logo POJO → Logo ImageView
      * Componente testo JSON → TagLine POJO → Text ImageView
      * Elenco frammenti di contenuto JSON → eventi POJO →Events RecyclerView
   * *Il codice dell’app mobile è in grado di mappare il JSON ai POJO a causa delle posizioni note all’interno della risposta JSON maggiore. Ricorda che le chiavi JSON di &quot;image&quot;, &quot;text&quot; e &quot;contentfragmentlist&quot; sono determinate dai nomi dei nodi dei Componenti AEM di base. Se questi nomi di nodo cambiano, l’app mobile si interromperà in quanto non saprà come creare l’origine del contenuto richiesto dai dati JSON.*

#### Richiamare l’endpoint di AEM Content Services

Di seguito è riportata una distillazione del codice nell’app mobile `MainActivity` responsabile della chiamata di AEM Content Services per la raccolta dei contenuti utilizzati per l’esperienza dell’app mobile.

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

`onCreate(..)` è l’hook di inizializzazione per l’app mobile e registra i 3 `ViewBinders` responsabile dell’analisi del JSON e dell’associazione dei valori al `View` elementi.

`initApp(...)` GET viene quindi chiamato, il che invia la richiesta HTTP al punto finale di AEM Content Services nella pubblicazione AEM per raccogliere il contenuto. Dopo aver ricevuto una risposta JSON valida, la risposta JSON viene passata a ogni `ViewBinder` responsabile dell’analisi del JSON e del suo binding al dispositivo mobile `View` elementi.

#### Analisi della risposta JSON

Ora esamineremo la `LogoViewBinder`, che è semplice, ma evidenzia diverse considerazioni importanti.

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

La prima riga di `bind(...)` passa alla risposta JSON tramite le chiavi **:items → root → :items** che rappresenta il Contenitore di layout AEM a cui sono stati aggiunti i componenti.

Da qui viene effettuato un controllo per una chiave denominata **immagine**, che rappresenta il componente Immagine (di nuovo, è importante che il nome del nodo → la chiave JSON siano stabili). Se questo oggetto esiste, viene letto e mappato al [POJO immagine personalizzata](#image-pojo) via Jackson `ObjectMapper` libreria. Il POJO Immagine viene illustrato di seguito.

Infine, il logo `src` viene caricato in Android ImageView utilizzando [!DNL Glide] libreria helper.

Tieni presente che è necessario fornire lo schema, l’host e la porta AEM (tramite `aemHost`) all&#39;istanza di pubblicazione dell&#39;AEM, poiché i servizi di contenuto dell&#39;AEM forniranno solo il percorso JCR (ad es. `/content/dam/wknd-mobile/images/wknd-logo.png`) al contenuto di riferimento.

#### POJO immagine{#image-pojo}

Anche se facoltativo, l&#39;utilizzo di [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) o funzionalità simili fornite da altre librerie come Gson, consentono di mappare strutture JSON complesse su Java POJO senza il tedio di trattare direttamente con gli oggetti JSON nativi stessi. In questo semplice caso viene mappato il `src` chiave da `image` oggetto JSON, al `src` nel POJO dell&#39;immagine direttamente tramite `@JSONProperty` annotazione.

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

Il POJO dell’evento, che richiede la selezione di molti più punti dati dall’oggetto JSON, beneficia di questa tecnica più della semplice immagine, che tutto ciò che vogliamo è `src`.

## Esplorare l’esperienza dell’app mobile

Ora che conosci il modo in cui i servizi di contenuto AEM possono promuovere l’esperienza mobile nativa, utilizza ciò che hai imparato per eseguire i passaggi seguenti e vedere le modifiche riportate nell’app mobile.

Dopo ogni passaggio, richiama per aggiornare l’app mobile e verifica l’aggiornamento dell’esperienza mobile.

1. Creare e pubblicare **nuovo [!DNL Event] Frammento di contenuto**
1. Annulla pubblicazione di un **esistente [!DNL Event] Frammento di contenuto**
1. Pubblica un aggiornamento per **Linea tag**

## Congratulazioni

**Hai completato l’esercitazione headless dell’AEM.**

Per ulteriori informazioni su AEM Content Services e AEM as a Headless CMS, consulta l’altra documentazione di Adobe e i materiali di supporto:

* [Utilizzo di frammenti di contenuto](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [Guida utente dei componenti core WCM AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it)
* [Libreria dei componenti core WCM AEM](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [Progetto GitHub dei componenti core WCM dell’AEM](https://github.com/adobe/aem-core-wcm-components)
* [Esempio di codice di Component Exporter](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
