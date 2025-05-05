---
title: Capitolo 7 - Utilizzo di servizi di contenuti AEM da un’app mobile - Content Services
description: Il capitolo 7 del tutorial esegue l’app mobile di Android per utilizzare contenuti creati da AEM Content Services.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
duration: 467
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1350'
ht-degree: 0%

---

# Capitolo 7 - Utilizzo di servizi di contenuti AEM da un’app mobile

Il capitolo 7 del tutorial utilizza un’app mobile nativa di Android per utilizzare contenuti di AEM Content Services.

## L’app mobile Android

Questo tutorial utilizza una **semplice app mobile nativa di Android** per utilizzare e visualizzare il contenuto degli eventi esposto da AEM Content Services.

L&#39;utilizzo di [Android](https://developer.android.com/) è in gran parte poco importante e l&#39;app mobile utilizzata potrebbe essere scritta in qualsiasi framework per qualsiasi piattaforma mobile, ad esempio iOS.

Android è utilizzato per i tutorial a causa della capacità di eseguire un emulatore Android su Windows, macOs e Linux, della sua popolarità e del fatto che può essere scritto come Java, un linguaggio ben compreso dagli sviluppatori AEM.

*L&#39;app mobile Android del tutorial è&#x200B;**non**&#x200B;concepita per istruire su come creare app mobili Android o trasmettere le best practice per lo sviluppo Android, ma piuttosto per illustrare come AEM Content Services può essere utilizzato da un&#39;applicazione mobile.*

### Come AEM Content Services guida l&#39;esperienza dell&#39;app mobile

![Mappatura app mobile a Content Services](assets/chapter-7/content-services-mapping.png)

1. Il **logo** definito dal **componente immagine** della pagina [!DNL Events API].
1. La **riga di tag** definita nel **componente testo** della pagina [!DNL Events API].
1. Questo **elenco eventi** è derivato dalla serializzazione dei frammenti di contenuto evento, esposti tramite il **componente elenco frammenti di contenuto** configurato.

## Dimostrazione sull’app mobile

>[!VIDEO](https://video.tv.adobe.com/v/28345?quality=12&learn=on)

### Configurazione dell’app mobile per un utilizzo non localhost

Se il Publish AEM non viene eseguito su **http://localhost:4503**, l&#39;host e la porta possono essere aggiornati in [!DNL Settings] dell&#39;app mobile per puntare alla proprietà AEM Publish host/porta.

>[!VIDEO](https://video.tv.adobe.com/v/28344?quality=12&learn=on)

## Esecuzione locale dell’app mobile

1. Scarica e installa [Android Studio](https://developer.android.com/studio/install) per installare l&#39;emulatore Android.
1. **Scarica** il file [!DNL APK] di Android [GitHub > Assets > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Apri **Android Studio**
   * Al primo avvio di Android Studio, verrà visualizzato un messaggio in cui viene richiesto di installare [!DNL Android SDK]. Accettare le impostazioni predefinite e completare l&#39;installazione.
1. Apri Android Studio e seleziona **Profilo o Debug APK**
1. Seleziona il file APK (**wknd-mobile.x.x.x.apk**) scaricato nel passaggio 2 e fai clic su **OK**
   * Se viene richiesto di **creare una nuova cartella** o **utilizzare una cartella esistente**, selezionare **Usa cartella esistente**.
1. Al primo avvio di Android Studio, fare clic con il pulsante destro del mouse su **wknd-mobile.x.x.x** nell&#39;elenco Progetti e selezionare **Apri impostazioni modulo**.
   * In **Moduli > wknd-mobile.x.x.x > Scheda Dipendenze**, selezionare **Piattaforma API 29 Android**. Toccare OK per chiudere e salvare le modifiche.
   * Se non esegui questa operazione, verrà visualizzato un errore &quot;Seleziona l’SDK di Android&quot; quando tenti di avviare l’emulatore.
1. Apri **AVD Manager** selezionando **Strumenti > AVD Manager** o toccando l&#39;icona **AVD Manager** nella barra superiore.
1. Nella finestra **AVD Manager**, fare clic su **+ Crea dispositivo virtuale...** se il dispositivo non è già stato registrato.
   1. A sinistra, seleziona la categoria **Telefono**.
   1. Selezionare un **pixel 2**.
   1. Fai clic sul pulsante **Avanti**.
   1. Seleziona **Q** con **API Level 29**.
      * All’avvio iniziale di AVD Manager, ti viene richiesto di scaricare l’API con versione. Fai clic sul collegamento Download accanto alla versione &quot;Q&quot;, quindi completa il download e l’installazione.
   1. Fai clic sul pulsante **Avanti**.
   1. Fare clic sul pulsante **Fine**.
1. Chiudere la finestra **AVD Manager**.
1. Nella barra dei menu superiore, seleziona **wknd-mobile.x.x.x** dal menu a discesa **Esegui/Modifica configurazioni**.
1. Tocca il pulsante **Esegui** accanto alla **Configurazione di esecuzione/modifica** selezionata
1. Nel pop-up, seleziona il dispositivo virtuale **[!DNL Pixel 2 API 29]** appena creato e tocca **OK**
1. Se l&#39;app [!DNL WKND Mobile] non viene caricata immediatamente, individua e tocca l&#39;icona **[!DNL WKND]** nella schermata iniziale di Android nell&#39;emulatore.
   * Se l&#39;emulatore viene avviato ma lo schermo dell&#39;emulatore rimane nero, toccare il pulsante **power** nella finestra degli strumenti dell&#39;emulatore accanto alla finestra dell&#39;emulatore.
   * Per scorrere all&#39;interno del dispositivo virtuale, fare clic e tenere premuto e trascinare.
   * Per aggiornare il contenuto dall’AEM, spingi verso il basso dall’alto fino all’icona Aggiorna
e rilasciare.

>[!VIDEO](https://video.tv.adobe.com/v/28341?quality=12&learn=on)

## Il codice dell’app mobile

Questa sezione evidenzia il codice dell’app mobile di Android che interagisce maggiormente e dipende da AEM Content Services e dal suo output JSON.

Al momento del caricamento, l&#39;app mobile rende `HTTP GET` a `/content/wknd-mobile/en/api/events.model.json`, che è l&#39;endpoint di AEM Content Services configurato per fornire il contenuto per l&#39;utilizzo dell&#39;app mobile.

Poiché il modello modificabile dell&#39;API degli eventi (`/content/wknd-mobile/en/api/events.model.json`) è bloccato, è possibile codificare l&#39;app mobile per cercare informazioni specifiche in posizioni specifiche nella risposta JSON.

### Flusso di codice di alto livello

1. L&#39;apertura dell&#39;app [!DNL WKND Mobile] richiama una richiesta `HTTP GET` al Publish dell&#39;AEM in `/content/wknd-mobile/en/api/events.model.json` per raccogliere il contenuto e popolare l&#39;interfaccia utente dell&#39;app mobile.
2. Dopo aver ricevuto il contenuto dall&#39;AEM, ciascuno dei tre elementi di visualizzazione dell&#39;app mobile, il **logo, la linea di tag e l&#39;elenco eventi**, vengono inizializzati con il contenuto dell&#39;AEM.
   * Per associare il contenuto AEM all’elemento di visualizzazione dell’app mobile, il JSON che rappresenta ciascun componente AEM è un oggetto mappato su un POJO Java, che a sua volta è associato all’elemento di visualizzazione Android.
      * Componente immagine JSON → Logo POJO → Logo ImageView
      * Componente testo JSON → TagLine POJO → Text ImageView
      * Elenco frammenti di contenuto JSON → eventi POJO →Events RecyclerView
   * *Il codice dell&#39;app mobile è in grado di mappare il JSON ai POJO a causa delle posizioni note all&#39;interno della risposta JSON maggiore. Ricorda che le chiavi JSON di &quot;image&quot;, &quot;text&quot; e &quot;contentfragmentlist&quot; sono determinate dai nomi dei nodi dei Componenti AEM di base. Se questi nomi di nodo cambiano, l&#39;app mobile si interromperà in quanto non saprà come creare l&#39;origine del contenuto richiesto dai dati JSON.*

#### Richiamare l’endpoint di AEM Content Services

Di seguito è riportata una distillazione del codice in `MainActivity` dell&#39;app mobile responsabile della chiamata di AEM Content Services per raccogliere il contenuto che guida l&#39;esperienza dell&#39;app mobile.

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

`onCreate(..)` è l&#39;hook di inizializzazione per l&#39;app mobile e registra i 3 `ViewBinders` personalizzati responsabili dell&#39;analisi del JSON e dell&#39;associazione dei valori agli elementi `View`.

`initApp(...)` viene quindi chiamato, rendendo la richiesta HTTP di GET all&#39;endpoint di Servizi di contenuto AEM su AEM Publish per raccogliere il contenuto. Dopo aver ricevuto una risposta JSON valida, la risposta JSON viene passata a ogni `ViewBinder` che è responsabile dell&#39;analisi del JSON e del suo binding agli elementi mobili `View`.

#### Analisi della risposta JSON

Esaminiamo ora `LogoViewBinder`, che è semplice, ma evidenzia alcune considerazioni importanti.

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

La prima riga di `bind(...)` passa alla risposta JSON tramite le chiavi **:items → root → :items** che rappresenta il contenitore di layout AEM a cui sono stati aggiunti i componenti.

Da qui viene eseguito un controllo per una chiave denominata **image**, che rappresenta il componente Immagine (di nuovo, è importante che il nome del nodo → la chiave JSON siano stabili). Se questo oggetto esiste, viene letto e mappato al POJO [&#128279;](#image-pojo) dell&#39;immagine personalizzata tramite la libreria Jackson `ObjectMapper`. Il POJO Immagine viene illustrato di seguito.

Infine, il logo `src` viene caricato in Android ImageView utilizzando la libreria helper [!DNL Glide].

Si noti che è necessario fornire lo schema, l&#39;host e la porta AEM (tramite `aemHost`) all&#39;istanza di Publish AEM, poiché AEM Content Services fornirà solo il percorso JCR (ad esempio. `/content/dam/wknd-mobile/images/wknd-logo.png`) al contenuto di riferimento.

#### POJO immagine{#image-pojo}

Anche se facoltativo, l&#39;utilizzo di [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) o di funzionalità simili fornite da altre librerie come Gson consente di mappare strutture JSON complesse su POJO Java senza il tedio di trattare direttamente con gli oggetti JSON nativi stessi. In questo semplice caso, la chiave `src` viene mappata dall&#39;oggetto JSON `image` all&#39;attributo `src` nel POJO dell&#39;immagine direttamente tramite l&#39;annotazione `@JSONProperty`.

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

Il POJO evento, che richiede la selezione di molti più punti dati dall&#39;oggetto JSON, beneficia di questa tecnica più della semplice immagine, che tutto ciò che vogliamo è `src`.

## Esplorare l’esperienza dell’app mobile

Ora che conosci il modo in cui i servizi di contenuto AEM possono promuovere l’esperienza mobile nativa, utilizza ciò che hai imparato per eseguire i passaggi seguenti e vedere le modifiche riportate nell’app mobile.

Dopo ogni passaggio, richiama per aggiornare l’app mobile e verifica l’aggiornamento dell’esperienza mobile.

1. Crea e pubblica **nuovo [!DNL Event] frammento di contenuto**
1. Annulla la pubblicazione di un **frammento di contenuto [!DNL Event] esistente**
1. Publish: aggiornamento della **linea tag**

## Complimenti

**Hai completato l&#39;esercitazione headless per AEM!**

Per ulteriori informazioni su AEM Content Services e AEM as a Headless CMS, consulta l’altra documentazione di Adobe e i materiali di supporto:

* [Utilizzo di frammenti di contenuto](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [Guida utente dei componenti core WCM dell&#39;AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it)
* [Libreria componenti core WCM AEM](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [Progetto GitHub per i componenti core WCM dell&#39;AEM](https://github.com/adobe/aem-core-wcm-components)
* [Esempio di codice di Component Exporter](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
