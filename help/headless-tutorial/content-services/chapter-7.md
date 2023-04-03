---
title: Capitolo 7 - Utilizzo di AEM Content Services da un’app mobile - Content Services
description: Il capitolo 7 dell’esercitazione esegue l’app mobile Android per utilizzare contenuti creati da AEM Content Services.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1391'
ht-degree: 0%

---

# Capitolo 7 - Utilizzo di AEM Content Services da un’app mobile

Il capitolo 7 dell&#39;esercitazione utilizza un&#39;app mobile Android nativa per utilizzare contenuti da AEM Content Services.

## App mobile Android

Questa esercitazione utilizza un **app mobile nativa semplice** per utilizzare e visualizzare il contenuto Evento esposto da AEM Content Services.

L&#39;uso di [Android](https://developer.android.com/) è in gran parte poco importante e l&#39;app mobile consueta può essere scritta in qualsiasi framework per qualsiasi piattaforma mobile, ad esempio iOS.

Android è utilizzato per tutorial a causa della capacità di eseguire un emulatore Android su Windows, macOs e Linux, la sua popolarità, e che può essere scritto come Java, un linguaggio ben capito dagli sviluppatori AEM.

*L’app mobile Android del tutorial è&#x200B;**not**inteso per istruire su come creare app Android Mobile o trasmettere le best practice di sviluppo Android, ma piuttosto per illustrare come AEM Content Services può essere utilizzato da un’applicazione mobile.*

### Come AEM Content Services guida l’esperienza dell’app mobile

![Mappatura di app mobili su Content Services](assets/chapter-7/content-services-mapping.png)

1. La **logo** come definito dalla [!DNL Events API] della pagina **Componente immagine**.
1. La **riga di tag** come definito nella [!DNL Events API] della pagina **Componente testo**.
1. Questo **Elenco eventi** è derivato dalla serializzazione dei frammenti di contenuto evento, esposti tramite la configurazione **Componente Elenco frammenti di contenuto**.

## Dimostrazione dell&#39;app mobile

>[!VIDEO](https://video.tv.adobe.com/v/28345?quality=12&learn=on)

### Configurazione dell’app mobile per uso non localhost

Se AEM Publish non viene eseguito su **http://localhost:4503** l’host e la porta possono essere aggiornati nell’app mobile [!DNL Settings] per puntare alla proprietà host/porta di AEM Publish.

>[!VIDEO](https://video.tv.adobe.com/v/28344?quality=12&learn=on)

## Esecuzione locale dell’app mobile

1. Scarica e installa la [Android Studio](https://developer.android.com/studio/install) per installare l’emulatore Android.
1. **Scarica** Android [!DNL APK] file [GitHub > Risorse > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Apri **Android Studio**
   * All&#39;avvio iniziale di Android Studio, viene richiesto di installare il [!DNL Android SDK] presenti. Accettate le impostazioni predefinite e completate l&#39;installazione.
1. Apri Android Studio e seleziona **APK di profilo o debug**
1. Seleziona il file APK (**wknd-mobile.x.x.x.apk**) scaricata nel passaggio 2 e fai clic su **OK**
   * Se viene richiesto di **Creare una nuova cartella** oppure **Usa esistente**, seleziona **Usa esistente**.
1. Al primo avvio di Android Studio, fai clic con il pulsante destro del mouse sul pulsante **wknd-mobile.x.x.x** nell’elenco Progetti e seleziona **Impostazioni modulo aperto**.
   * Sotto **Moduli > wknd-mobile.x.x.x > Scheda Dipendenze**, seleziona **Piattaforma API 29 Android**. Toccare OK per chiudere e salvare le modifiche.
   * In caso contrario, quando tenti di avviare l’emulatore viene visualizzato l’errore &quot;Seleziona l’SDK per Android&quot;.
1. Apri **Gestione AVD** selezionando **Strumenti > Gestione AVD** o toccando **Gestione AVD** nella barra superiore.
1. In **Gestione AVD** finestra, fai clic su **+ Crea dispositivo virtuale...** se il dispositivo non è già registrato.
   1. A sinistra, seleziona la **Telefono** categoria.
   1. Seleziona una **Pixel 2**.
   1. Fai clic sul pulsante **Successivo** pulsante .
   1. Seleziona **Q** con **Livello API 29**.
      * All’avvio iniziale di AVD Manager, ti viene richiesto di Scaricare l’API con versione. Fai clic sul collegamento Scarica accanto alla versione &quot;Q&quot; e completa il download e l’installazione.
   1. Fai clic sul pulsante **Successivo** pulsante .
   1. Fai clic sul pulsante **Fine** pulsante .
1. Chiudi **Gestione AVD** finestra.
1. Nella barra dei menu superiore seleziona **wknd-mobile.x.x.x** dal **Eseguire/modificare le configurazioni** a discesa.
1. Tocca **Esegui** accanto al pulsante selezionato **Esegui/Modifica configurazione**
1. Nella finestra a comparsa, seleziona la nuova creazione **[!DNL Pixel 2 API 29]** dispositivo virtuale e tocca **OK**
1. Se la [!DNL WKND Mobile] l’app non viene caricata, individuata e tocca immediatamente il **[!DNL WKND]** icona dalla schermata iniziale Android nell’emulatore.
   * Se l’emulatore si avvia ma la schermata dell’emulatore rimane nera, tocca il **potenza** nella finestra degli strumenti dell&#39;emulatore accanto alla finestra dell&#39;emulatore.
   * Per scorrere all&#39;interno del dispositivo virtuale, fare clic e tenere premuto e trascinare.
   * Per aggiornare il contenuto da AEM, scegli dall’alto fino a visualizzare l’icona Aggiorna e rilascia.

>[!VIDEO](https://video.tv.adobe.com/v/28341?quality=12&learn=on)

## Il codice dell’app mobile

Questa sezione evidenzia il codice dell’app mobile Android che più interagisce e dipende da AEM Content Services e dall’output JSON di .

Al caricamento, l&#39;app mobile esegue `HTTP GET` a `/content/wknd-mobile/en/api/events.model.json` che è il punto finale di AEM Content Services configurato per fornire il contenuto per l&#39;app mobile.

Perché il modello modificabile dell&#39;API degli eventi (`/content/wknd-mobile/en/api/events.model.json`) è bloccata, l’app mobile può essere codificata per cercare informazioni specifiche in posizioni specifiche nella risposta JSON.

### Flusso del codice di alto livello

1. Apertura [!DNL WKND Mobile] L&#39;app richiama un `HTTP GET` richiedi ad AEM Publish all&#39;indirizzo `/content/wknd-mobile/en/api/events.model.json` per raccogliere il contenuto e compilare l’interfaccia utente dell’app mobile.
2. Dopo aver ricevuto il contenuto da AEM, ciascuno dei tre elementi di visualizzazione dell’app mobile, l’ **logo, riga tag ed elenco eventi**, vengono inizializzati con il contenuto di AEM.
   * Per eseguire un binding al contenuto AEM all’elemento di visualizzazione dell’app mobile, il JSON che rappresenta ciascun componente AEM è un oggetto mappato a un POJO Java, che a sua volta è associato all’elemento Visualizzazione Android.
      * Componente immagine JSON → Logo POJO → Logo ImageView
      * Componente di testo JSON → TagLine POJO → Text ImageView
      * JSON Elenco frammenti di contenuto → Eventi POJO → Eventi RecyclerView
   * *Il codice dell’app mobile è in grado di mappare il JSON ai POJO a causa delle posizioni ben note all’interno della risposta JSON maggiore. Ricorda che le chiavi JSON di &quot;image&quot;, &quot;text&quot; e &quot;contentfragmentlist&quot; sono dettate dai nomi dei nodi dei componenti AEM di supporto. Se questi nomi di nodo cambiano, l&#39;app mobile si interrompe in quanto non sa come sorgente del contenuto richiesto dai dati JSON.*

#### Richiamo dell&#39;endpoint AEM Content Services

Di seguito è riportata una distillazione del codice nell’app mobile `MainActivity` responsabile della chiamata a AEM Content Services per raccogliere il contenuto che guida l’esperienza di app mobile.

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

`onCreate(..)` è l&#39;hook di inizializzazione per l&#39;app mobile e registra i 3 dati personalizzati `ViewBinders` responsabile dell’analisi del JSON e dell’associazione dei valori al `View` elementi.

`initApp(...)` viene quindi chiamato , che invia la richiesta HTTP GET al punto finale di AEM Content Services su AEM Publish per raccogliere il contenuto. Dopo aver ricevuto una risposta JSON valida, la risposta JSON viene passata a ogni `ViewBinder` che è responsabile dell’analisi del JSON e del suo binding al cellulare `View` elementi.

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

La prima riga di `bind(...)` naviga verso il basso nella risposta JSON tramite le chiavi **:items → root → :items** che rappresenta il Contenitore di layout AEM a cui sono stati aggiunti i componenti.

Da qui viene effettuato un controllo per una chiave denominata **immagine**, che rappresenta il componente Immagine (ancora una volta, è importante che il nome del nodo → Chiave JSON sia stabile). Se questo oggetto esiste, viene letto e mappato al [POJO immagine personalizzata](#image-pojo) via il Jackson `ObjectMapper` libreria. L&#39;immagine POJO è illustrata di seguito.

Infine, il logo `src` viene caricato in Android ImageView utilizzando [!DNL Glide] biblioteca di aiuto.

Tieni presente che è necessario fornire lo schema, l’host e la porta AEM (tramite `aemHost`) all’istanza AEM Publish come AEM Content Services fornirà solo il percorso JCR (ad es. `/content/dam/wknd-mobile/images/wknd-logo.png`) al contenuto a cui si fa riferimento.

#### Immagine POJO{#image-pojo}

Anche se facoltativo, l&#39;uso del [Mapper oggetto Jackson](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) Oppure funzionalità simili fornite da altre librerie come Gson, aiutano a mappare strutture JSON complesse a Java POJOs senza il tedio di trattare direttamente con gli oggetti JSON nativi stessi. In questo semplice caso mappiamo la `src` della `image` oggetto JSON, al `src` nell’immagine POJO direttamente tramite il `@JSONProperty` annotazione.

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

Il POJO evento, che richiede la selezione di molti più punti dati dall&#39;oggetto JSON, beneficia di questa tecnica più della semplice immagine, che tutto ciò che vogliamo è la `src`.

## Esplorare l’esperienza con l’app mobile

Ora che hai capito come AEM Content Services è in grado di promuovere l’esperienza mobile nativa, utilizza ciò che hai imparato a eseguire i seguenti passaggi e osserva le modifiche che si riflettono nell’app mobile.

Dopo ogni passaggio, effettua il pull-to-refresh the Mobile App e verifica l’aggiornamento dell’esperienza mobile.

1. Creare e pubblicare **nuovo [!DNL Event] Frammento di contenuto**
1. Annullare la pubblicazione di un **esistente [!DNL Event] Frammento di contenuto**
1. Pubblicare un aggiornamento nel **Riga tag**

## Congratulazioni

**Hai completato l&#39;esercitazione AEM Headless!**

Per ulteriori informazioni su AEM Content Services e AEM come CMS headless, visita la documentazione e il materiale di abilitazione di Adobe:

* [Utilizzo di frammenti di contenuto](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [Guida utente AEM componenti core di WCM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it)
* [AEM libreria dei componenti core di WCM](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM progetto GitHub per componenti core WCM](https://github.com/adobe/aem-core-wcm-components)
* [Esempio di codice per l’esportazione di componenti](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
