---
title: Capitolo 7 - Utilizzo di AEM Content Services da un’app mobile - Content Services
description: Il capitolo 7 dell’esercitazione esegue l’app mobile Android per utilizzare contenuti creati da AEM Content Services.
feature: Frammenti di contenuto, API
topic: Senza testa, gestione dei contenuti
role: Developer
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1412'
ht-degree: 0%

---


# Capitolo 7 - Utilizzo di AEM Content Services da un’app mobile

Il capitolo 7 dell&#39;esercitazione utilizza un&#39;app mobile Android nativa per utilizzare contenuti da AEM Content Services.

## App mobile Android

Questa esercitazione utilizza una **semplice app mobile nativa Android** per utilizzare e visualizzare il contenuto Evento esposto da AEM Content Services.

L&#39;utilizzo di [Android](https://developer.android.com/) è in gran parte poco importante e l&#39;app mobile utilizzata potrebbe essere scritta in qualsiasi framework per qualsiasi piattaforma mobile, ad esempio iOS.

Android è utilizzato per tutorial a causa della capacità di eseguire un emulatore Android su Windows, macOs e Linux, la sua popolarità, e che può essere scritto come Java, un linguaggio ben capito dagli sviluppatori AEM.

*L’app mobile Android del tutorial ****non ha lo scopo di istruire su come creare app Android Mobile o trasmettere le best practice di sviluppo Android, ma piuttosto di illustrare come AEM Content Services può essere utilizzato da un’applicazione mobile.*

### Come AEM Content Services guida l’esperienza dell’app mobile

![Mappatura di app mobili su Content Services](assets/chapter-7/content-services-mapping.png)

1. Il **logo** come definito dal [!DNL Events API] componente immagine della pagina ****.
1. Il **tag line** come definito nel componente [!DNL Events API] Testo della pagina **Componente testo**.
1. Questo **elenco eventi** è derivato dalla serializzazione dei frammenti di contenuto evento, esposti tramite il componente **Elenco frammenti di contenuto** configurato.

## Dimostrazione dell&#39;app mobile

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### Configurazione dell’app mobile per uso non localhost

Se AEM Publish non viene eseguito su **http://localhost:4503** l&#39;host e la porta possono essere aggiornati nell&#39; [!DNL Settings] dell&#39;app mobile per puntare alla proprietà Host/porta di AEM Publish.

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## Esecuzione locale dell’app mobile

1. Scarica e installa [Android Studio](https://developer.android.com/studio/install) per installare l&#39;emulatore Android.
1. **** Scarica il  [!DNL APK] file Android  [GitHub > Risorse > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Apri **Android Studio**
   * All&#39;avvio iniziale di Android Studio, verrà visualizzato un messaggio per l&#39;installazione di [!DNL Android SDK]. Accettate le impostazioni predefinite e completate l&#39;installazione.
1. Apri Android Studio e seleziona **Profilo o Debug APK**
1. Seleziona il file APK (**wknd-mobile.x.x.x.x.apk**) scaricato nel passaggio 2 e fai clic su **OK**
   * Se viene richiesto di **Creare una nuova cartella** o **Utilizzare esistente**, selezionare **Usa esistente**.
1. All&#39;avvio iniziale di Android Studio, fai clic con il pulsante destro del mouse su **wknd-mobile.x.x** nell&#39;elenco Progetti e seleziona **Impostazioni modulo aperto**.
   * In **Moduli > wknd-mobile.x.x > Scheda Dipendenze**, seleziona **Piattaforma API Android 29**. Toccare OK per chiudere e salvare le modifiche.
   * In caso contrario, quando tenti di avviare l’emulatore viene visualizzato l’errore &quot;Seleziona l’SDK per Android&quot;.
1. Apri **AVD Manager** selezionando **Strumenti > AVD Manager** o toccando l&#39;icona **AVD Manager** nella barra superiore.
1. Nella finestra **AVD Manager** fare clic su **+ Crea dispositivo virtuale...** se il dispositivo non è già registrato.
   1. A sinistra, seleziona la categoria **Telefono** .
   1. Seleziona un **Pixel 2**.
   1. Fare clic sul pulsante **Avanti**.
   1. Seleziona **Q** con **Livello API 29**.
      * All’avvio iniziale di AVD Manager, ti verrà richiesto di Scaricare l’API con versione. Fai clic sul collegamento Scarica accanto alla versione &quot;Q&quot; e completa il download e l’installazione.
   1. Fare clic sul pulsante **Avanti**.
   1. Fare clic sul pulsante **Fine**.
1. Chiudere la finestra **AVD Manager**.
1. Nella barra dei menu principale seleziona **wknd-mobile.x.x.x** dal menu a discesa **Esegui/Modifica configurazioni**.
1. Toccare il pulsante **Esegui** accanto al **Esegui/Modifica configurazione** selezionato
1. Nella finestra a comparsa, seleziona il dispositivo virtuale **[!DNL Pixel 2 API 29]** appena creato e tocca **OK**
1. Se l’app [!DNL WKND Mobile] non viene caricata immediatamente, trova e tocca l’icona **[!DNL WKND]** nella schermata iniziale di Android nell’emulatore.
   * Se l&#39;emulatore si avvia ma lo schermo dell&#39;emulatore rimane nero, toccare il pulsante **power** nella finestra degli strumenti dell&#39;emulatore accanto alla finestra dell&#39;emulatore.
   * Per scorrere all&#39;interno del dispositivo virtuale, fare clic e tenere premuto e trascinare.
   * Per aggiornare il contenuto da AEM, scegli dall’alto fino all’icona Aggiorna
visualizza e rilascia.

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## Il codice dell’app mobile

Questa sezione evidenzia il codice dell’app mobile Android che più interagisce e dipende da AEM Content Services e dall’output JSON di .

Al caricamento, l’app mobile effettua da `HTTP GET` a `/content/wknd-mobile/en/api/events.model.json` che è il punto finale di AEM Content Services configurato per fornire il contenuto per l’app mobile.

Poiché il modello modificabile dell&#39;API degli eventi (`/content/wknd-mobile/en/api/events.model.json`) è bloccato, l&#39;app mobile può essere codificata per cercare informazioni specifiche in posizioni specifiche nella risposta JSON.

### Flusso del codice di alto livello

1. L’apertura dell’ [!DNL WKND Mobile] app richiama una richiesta `HTTP GET` all’AEM Publish at `/content/wknd-mobile/en/api/events.model.json` per raccogliere il contenuto per popolare l’interfaccia utente dell’app mobile.
2. Dopo aver ricevuto il contenuto da AEM, ciascuno dei tre elementi di visualizzazione dell&#39;app mobile, il **logo, la riga tag e l&#39;elenco eventi** vengono inizializzati con il contenuto di AEM.
   * Per eseguire un binding al contenuto AEM all’elemento di visualizzazione dell’app mobile, il JSON che rappresenta ciascun componente AEM è un oggetto mappato a un POJO Java, che a sua volta è associato all’elemento Visualizzazione Android.
      * Componente immagine JSON → Logo POJO → Logo ImageView
      * Componente di testo JSON → TagLine POJO → Text ImageView
      * JSON Elenco frammenti di contenuto → Eventi POJO → Eventi RecyclerView
   * *Il codice dell’app mobile è in grado di mappare il JSON ai POJO a causa delle posizioni ben note all’interno della risposta JSON maggiore. Ricorda che le chiavi JSON di &quot;image&quot;, &quot;text&quot; e &quot;contentfragmentlist&quot; sono dettate dai nomi dei nodi dei componenti AEM di supporto. Se questi nomi di nodo cambiano, allora l&#39;app mobile si interromperà in quanto non saprà come ottenere il contenuto richiesto dai dati JSON.*

#### Richiamo dell&#39;endpoint AEM Content Services

Di seguito è riportata una distillazione del codice nell’ `MainActivity` dell’app mobile responsabile della chiamata AEM Content Services per raccogliere il contenuto che guida l’esperienza dell’app mobile.

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

`onCreate(..)` è l’hook di inizializzazione per l’app mobile e registra i 3  `ViewBinders` responsabili personalizzati per l’analisi del JSON e l’associazione dei valori agli  `View` elementi.

`initApp(...)` viene quindi chiamato , che invia la richiesta HTTP GET al punto finale di AEM Content Services su AEM Publish per raccogliere il contenuto. Dopo aver ricevuto una risposta JSON valida, la risposta JSON viene passata a ogni `ViewBinder` responsabile dell’analisi del JSON e del binding agli elementi mobili `View`.

#### Analisi della risposta JSON

Ora esamineremo il `LogoViewBinder`, che è semplice, ma evidenzia diverse considerazioni importanti.

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

La prima riga di `bind(...)` naviga verso il basso nella risposta JSON tramite le chiavi **:items → root → :items** che rappresentano il contenitore di layout AEM a cui sono stati aggiunti i componenti.

Da qui viene effettuato un controllo per una chiave denominata **image**, che rappresenta il componente Immagine (ancora una volta, è importante che il nome del nodo → la chiave JSON sia stabile). Se questo oggetto esiste, legge e mappa l&#39; [immagine personalizzata POJO](#image-pojo) tramite la libreria Jackson `ObjectMapper`. L&#39;immagine POJO è illustrata di seguito.

Infine, il logo `src` viene caricato in Android ImageView utilizzando la libreria di supporto [!DNL Glide].

Nota che è necessario fornire lo schema, l’host e la porta AEM (tramite `aemHost`) all’istanza di pubblicazione AEM, in quanto AEM Content Services fornirà solo il percorso JCR (ad es. `/content/dam/wknd-mobile/images/wknd-logo.png`) al contenuto a cui si fa riferimento.

#### Immagine POJO{#image-pojo}

Anche se opzionale, l&#39;utilizzo di [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) o di funzionalità simili fornite da altre librerie come Gson, aiuta a mappare complesse strutture JSON a JOs Java senza il tedio di trattare direttamente con gli oggetti JSON nativi stessi. In questo caso semplice mappamo la chiave `src` dall’oggetto JSON `image` all’attributo `src` nell’Image POJO direttamente tramite l’annotazione `@JSONProperty`.

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

Il POJO evento, che richiede la selezione di molti più punti dati dall’oggetto JSON, beneficia di questa tecnica più della semplice immagine, che tutto ciò che vogliamo è il `src`.

## Esplorare l’esperienza con l’app mobile

Ora che hai capito come AEM Content Services è in grado di promuovere l’esperienza mobile nativa, utilizza ciò che hai imparato a eseguire i seguenti passaggi e osserva le modifiche che si riflettono nell’app mobile.

Dopo ogni passaggio, effettua il pull-to-refresh the Mobile App e verifica l’aggiornamento dell’esperienza mobile.

1. Crea e pubblica **nuovo [!DNL Event] frammento di contenuto**
1. Annullare la pubblicazione di un **frammento di contenuto esistente [!DNL Event]**
1. Pubblica un aggiornamento alla **riga tag**

## Congratulazioni

**Hai completato l&#39;esercitazione AEM Headless!**

Per ulteriori informazioni su AEM Content Services e AEM come CMS headless, visita la documentazione e il materiale di abilitazione di Adobe:

* [Utilizzo di frammenti di contenuto](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [Guida utente AEM componenti core di WCM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it)
* [AEM libreria dei componenti core di WCM](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM progetto GitHub per componenti core WCM](https://github.com/adobe/aem-core-wcm-components)
* [AEM componenti core WCM - Chiedi all’esperto](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-content-services.html)
* [Esempio di codice per l’esportazione di componenti](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/bundle/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
