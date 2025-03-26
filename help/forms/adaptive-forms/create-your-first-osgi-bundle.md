---
title: Creazione del primo bundle OSGi con AEM Forms
description: Crea il tuo primo bundle OSGi utilizzando Maven ed Eclipse
feature: Adaptive Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2021-06-09T00:00:00Z
duration: 177
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 0%

---


# Creare il primo bundle OSGi

Un bundle OSGi è un file di archivio Java™ che contiene codice Java, risorse e un manifesto che descrive il bundle e le sue dipendenze. Il bundle è l’unità di distribuzione di un’applicazione. Questo articolo è destinato agli sviluppatori che desiderano creare un servizio OSGi o un servlet utilizzando AEM Forms 6.4 o 6.5. Per creare il primo bundle OSGi, segui i passaggi seguenti:


## Installare JDK

Installa la versione supportata di JDK. Ho usato JDK1.8. Assicurati di aver aggiunto **JAVA_HOME** nelle variabili di ambiente e di puntare alla cartella principale dell&#39;installazione JDK.
Aggiungere %JAVA_HOME%/bin al percorso

![origine dati](assets/java-home.JPG)

>[!NOTE]
> Non usi JDK 15. Non è supportato da AEM.

### Verifica la versione JDK

Aprire una nuova finestra del prompt dei comandi e digitare: `java -version`. È necessario recuperare la versione di JDK identificata dalla variabile `JAVA_HOME`

![origine dati](assets/java-version.JPG)

## Installare Maven

Maven è uno strumento di automazione della build utilizzato principalmente per i progetti Java. Per installare Maven nel sistema locale, segui la procedura riportata di seguito.

* Crea una cartella denominata `maven` nell&#39;unità C
* Scarica l&#39;[archivio zip binario](http://maven.apache.org/download.cgi)
* Estrai il contenuto dell&#39;archivio zip in `c:\maven`
* Creare una variabile di ambiente denominata `M2_HOME` con valore `C:\maven\apache-maven-3.6.0`. Nel mio caso, la versione **mvn** è 3.6.0. Al momento di scrivere questo articolo, l’ultima versione di Maven è la 3.6.3
* Aggiungi `%M2_HOME%\bin` al tuo percorso
* Salva le modifiche
* Aprire un nuovo prompt dei comandi e digitare in `mvn -version`. Dovresti vedere la versione **mvn** elencata come mostrato nella schermata seguente

![origine dati](assets/mvn-version.JPG)

## Settings.xml

Un file Maven `settings.xml` definisce valori che configurano l’esecuzione Maven in vari modi. Nella maggior parte dei casi viene utilizzato per definire una posizione di repository locale, server di repository remoti alternativi e informazioni di autenticazione per gli archivi privati.

Passa a `C:\Users\<username>\.m2 folder`
Estrarre il contenuto del file [settings.zip](assets/settings.zip) e inserirlo nella cartella `.m2`.

## Installare Eclipse

Installa la versione più recente di [eclipse](https://www.eclipse.org/downloads/)

## Crea il primo progetto

Archetipo è un toolkit per modelli di progetto Maven. Un archetipo è definito come un modello originale da cui vengono fatte tutte le altre cose dello stesso tipo. Il nome corrisponde a quello che stiamo cercando di fornire per fornire un sistema che fornisca un mezzo coerente per generare progetti Maven. Archetipo consente agli autori di creare modelli di progetto Maven per gli utenti e fornisce loro i mezzi per generare versioni con parametri di tali modelli di progetto.
Per creare il primo progetto Maven, segui i passaggi seguenti:

* Crea una nuova cartella denominata `aemformsbundles` nell&#39;unità C
* Apri un prompt dei comandi e passa a `c:\aemformsbundles`
* Esegui il comando seguente al prompt dei comandi
* `mvn archetype:generate  -DarchetypeGroupId=com.adobe.granite.archetypes  -DarchetypeArtifactId=aem-project-archetype -DarchetypeVersion=19`

Il progetto Maven viene generato in modo interattivo e ti viene chiesto di fornire valori a una serie di proprietà come

| Nome proprietà | Significatività | Valore |
|------------------------|---------------------------------------|---------------------|
| groupId | groupId identifica in modo univoco il progetto in tutti i progetti | com.learningaemforms.adobe |
| appsFolderName | Nome della cartella che contiene la struttura del progetto | learningaemforms |
| artifactId | artifactId è il nome del file jar senza versione. Se lo avete creato, potete scegliere il nome desiderato con lettere minuscole e senza simboli strani. | learningaemforms |
| version | Se la distribuisci, puoi scegliere qualsiasi versione tipica con numeri e punti (1.0, 1.1, 1.0.1, ...). | 1.0 |

Accetta i valori predefiniti per le altre proprietà premendo Invio.
Se tutto va bene, dovresti visualizzare un messaggio di generazione riuscita nella finestra del comando

## Crea un progetto di eclissi dal progetto Maven

Cambia la directory di lavoro in `learningaemforms`.
Esecuzione di `mvn eclipse:eclipse` dalla riga di comando
Il comando precedente legge il file pom e crea progetti Eclipse con metadati corretti, in modo che Eclipse comprenda i tipi di progetto, le relazioni, il percorso di classe e così via.

## Importa il progetto nell&#39;eclissi

Avvia **Eclipse**

Vai a **File -> Importa** e seleziona **Progetti Maven esistenti** come mostrato qui

![origine dati](assets/import-mvn-project.JPG)

Fai clic su Successivo

Selezionare `c:\aemformsbundles\learningaemform` facendo clic sul pulsante **Sfoglia**

![origine dati](assets/select-mvn-project.JPG)

>[!NOTE]
>Puoi scegliere di importare i moduli appropriati in base alle tue esigenze. Seleziona e importa solo il modulo core, se desideri creare solo codice Java nel progetto.

Fai clic su **Fine** per avviare il processo di importazione

Il progetto viene importato in Eclipse e sono presenti diverse `learningaemforms.xxxx` cartelle

Espandere `src/main/java` nella cartella `learningaemforms.core`. Questa è la cartella in cui si sta scrivendo la maggior parte del codice.

![origine dati](assets/learning-core.JPG)

## Creare il progetto

Dopo aver scritto il servizio OSGi, o servlet, devi creare il progetto per generare il bundle OSGi che può essere distribuito utilizzando la console web Felix. Fai riferimento a [SDK client AEMFD](https://repo.adobe.com/nexus/content/repositories/public/com/adobe/aemfd/aemfd-client-sdk/) per includere il SDK client appropriato nel progetto Maven. È necessario includere AEM FD Client SDK nella sezione delle dipendenze di `pom.xml` del progetto di base, come illustrato di seguito.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

Per creare il progetto, effettua le seguenti operazioni:

* Apri **finestra del prompt dei comandi**
* Passa a `c:\aemformsbundles\learningaemforms\core`
* Esegui il comando `mvn clean install`
Se tutto va bene, dovresti vedere il bundle nella seguente posizione `C:\AEMFormsBundles\learningaemforms\core\target`. Questo bundle è ora pronto per essere implementato in AEM utilizzando la console web Felix.
