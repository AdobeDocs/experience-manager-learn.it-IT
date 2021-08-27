---
title: Creazione del primo bundle OSGi con AEM Forms
description: Crea il tuo primo bundle OSGi utilizzando Maven ed Eclipse
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
source-git-commit: 0049c9fd864bd4dd4f8c33b1e40e94aad3ffc5b9
workflow-type: tm+mt
source-wordcount: '840'
ht-degree: 2%

---


# Crea il tuo primo bundle OSGi

Un bundle OSGi è un file di archivio Java™ contenente codice Java, risorse e un manifesto che descrive il bundle e le sue dipendenze. Il bundle è l&#39;unità di distribuzione per un&#39;applicazione. Questo articolo è destinato agli sviluppatori che desiderano creare un servizio OSGi o un servlet utilizzando AEM Forms 6.4 o 6.5. Per creare il tuo primo bundle OSGi, segui i seguenti passaggi:


## Installa JDK

Installa la versione supportata di JDK. Ho usato JDK1.8. Assicurati di aver aggiunto **JAVA_HOME** nelle variabili di ambiente e stia puntando alla cartella principale dell&#39;installazione JDK.
Aggiungi il percorso %JAVA_HOME%/bin

![sorgente dati](assets/java-home.JPG)

>[!NOTE]
> Non utilizzare JDK 15. Non è supportato da AEM.

### Verifica la versione JDK

Apri una nuova finestra del prompt dei comandi e digita: `java -version`. È necessario recuperare la versione di JDK identificata dalla variabile `JAVA_HOME`

![sorgente dati](assets/java-version.JPG)

## Installa Maven

Maven è uno strumento di automazione della build utilizzato principalmente per i progetti Java. Segui i seguenti passaggi per installare maven sul tuo sistema locale.

* Crea una cartella denominata `maven` nell&#39;unità C
* Scarica l&#39; [archivio zip binario](https://maven.apache.org/download.cgi)
* Estrai il contenuto dell&#39;archivio zip in `c:\maven`
* Crea una variabile di ambiente denominata `M2_HOME` con un valore di `C:\maven\apache-maven-3.6.0`. Nel mio caso, la versione **mvn** è 3.6.0. Al momento di scrivere questo articolo, l’ultima versione di maven è 3.6.3
* Aggiungi `%M2_HOME%\bin` al percorso
* Salva le modifiche
* Apri un nuovo prompt dei comandi e digita `mvn -version`. Dovresti vedere la versione **mvn** elencata come mostrato nella schermata seguente

![sorgente dati](assets/mvn-version.JPG)

## Settings.xml

Un file Maven `settings.xml` definisce i valori che configurano l’esecuzione Maven in diversi modi. Nella maggior parte dei casi, viene utilizzato per definire una posizione di archivio locale, server di archivio remoti alternativi e informazioni di autenticazione per archivi privati.

Passa a `C:\Users\<username>\.m2 folder`
Estrai il contenuto del file [settings.zip](assets/settings.zip) e inseriscilo nella cartella `.m2` .

## Installa Eclipse

Installa la versione più recente di [eclipse](https://www.eclipse.org/downloads/)

## Crea il primo progetto

Archetype è un toolkit per modelli di progetto Maven. Un archetipo è definito come un modello o modello originale dal quale vengono realizzate tutte le altre cose dello stesso tipo. Il nome si adatta quando cerchiamo di fornire un sistema che fornisca un mezzo coerente per generare progetti Maven. Archetype aiuta gli autori a creare modelli di progetto Maven per gli utenti e fornisce agli utenti i mezzi per generare versioni con parametri di tali modelli di progetto.
Per creare il tuo primo progetto Maven, segui i seguenti passaggi:

* Crea una nuova cartella denominata `aemformsbundles` nell&#39;unità C
* Apri un prompt dei comandi e passa a `c:\aemformsbundles`
* Esegui il seguente comando nel prompt dei comandi
* `mvn archetype:generate  -DarchetypeGroupId=com.adobe.granite.archetypes  -DarchetypeArtifactId=aem-project-archetype -DarchetypeVersion=19`

Il progetto Maven verrà generato in modo interattivo e ti verrà chiesto di fornire valori a diverse proprietà, ad esempio:

| Nome proprietà | Significato | Valore |
------------------------|---------------------------------------|---------------------
| groupId | groupId identifica il progetto in modo univoco in tutti i progetti | com.learningaemforms.adobe |
| appsFolderName | Nome della cartella che conterrà la struttura del progetto | apprendimento di aemforms |
| artifactId | artifactId è il nome del jar senza versione. Se lo hai creato, puoi scegliere qualsiasi nome desideri con lettere minuscole e senza strani simboli. | apprendimento di aemforms |
| version | Se lo distribuisci, puoi scegliere qualsiasi versione tipica con numeri e punti (1.0, 1.1, 1.0.1, ...). | 1.0 |

Accettate i valori predefiniti per le altre proprietà premendo Invio chiave.
Se tutto va bene, dovresti visualizzare un messaggio di completamento nella finestra del comando

## Crea un progetto eclipse dal tuo progetto Maven

Cambia la directory di lavoro in `learningaemforms`.
Esegui `mvn eclipse:eclipse` dalla riga di comando
Il comando precedente legge il file pom e crea progetti Eclipse con metadati corretti in modo che Eclipse possa comprendere i tipi di progetto, le relazioni, il percorso di classe, ecc.

## Importa il progetto in eclissi

Lancio **Eclipse**

Vai a **File -> Importa** e seleziona **Progetti Maven esistenti** come mostrato qui

![sorgente dati](assets/import-mvn-project.JPG)

Fai clic su Avanti

Selezionare i `c:\aemformsbundles\learningaemform`s facendo clic sul pulsante **Sfoglia**

![sorgente dati](assets/select-mvn-project.JPG)

>[!NOTE]
>Puoi scegliere di importare i moduli appropriati in base alle tue esigenze. Seleziona e importa solo il modulo principale, se hai intenzione di creare solo codice Java nel tuo progetto.

Fare clic su **Fine** per avviare il processo di importazione

Il progetto viene importato in Eclipse e vengono visualizzate diverse cartelle `learningaemforms.xxxx`

Espandi la cartella `src/main/java` sotto la cartella `learningaemforms.core`. Questa è la cartella in cui scriverai la maggior parte del codice.

![sorgente dati](assets/learning-core.JPG)

## Crea il progetto




Dopo aver scritto il servizio OSGi, o servlet, dovrai generare il progetto per generare il bundle OSGi che può essere distribuito utilizzando la console web Felix. Fai riferimento a [SDK client AEMFD](https://repo.adobe.com/nexus/content/groups/public/com/adobe/aemfd/aemfd-client-sdk-) per includere l&#39;SDK client appropriato nel tuo progetto Maven. Dovrai includere l’SDK client FD AEM nella sezione delle dipendenze di `pom.xml` del progetto principale come mostrato di seguito.







```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

Per creare il progetto, effettua le seguenti operazioni:

* Apri **finestra del prompt dei comandi**
* Accedi a `c:\aemformsbundles\learningaemforms\core`
* Esegui il comando `mvn clean install -PautoInstallBundle`
Il comando precedente crea e installa il bundle nel server AEM in esecuzione su `http://localhost:4502`. Il bundle sarà disponibile anche sul file system all&#39;indirizzo
   `C:\AEMFormsBundles\learningaemforms\core\target` e può essere distribuito utilizzando la console web  [Felix](http://localhost:4502/system/console/bundles)
