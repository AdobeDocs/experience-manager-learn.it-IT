---
title: Creazione del primo bundle OSGi con AEM Forms
description: Crea il tuo primo bundle OSGi utilizzando Maven ed Eclipse
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
exl-id: 307cc3b2-87e5-4429-8f21-5266cf03b78f
last-substantial-update: 2021-04-23T00:00:00Z
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '672'
ht-degree: 1%

---

# Crea il tuo primo bundle OSGi

Un bundle OSGi è un file di archivio Java™ contenente codice Java, risorse e un manifesto che descrive il bundle e le sue dipendenze. Il bundle è l&#39;unità di distribuzione per un&#39;applicazione. Questo articolo è destinato agli sviluppatori che desiderano creare un servizio OSGi o un servlet utilizzando AEM Forms 6.4 o 6.5. Per creare il tuo primo bundle OSGi, segui i seguenti passaggi:


## Installa JDK

Installa la versione supportata di JDK. Ho usato JDK1.8. Assicurati di aver aggiunto **JAVA_HOME** nelle variabili di ambiente e punta alla cartella principale dell&#39;installazione JDK.
Aggiungi il percorso %JAVA_HOME%/bin

![sorgente dati](assets/java-home.JPG)

>[!NOTE]
> Non utilizzare JDK 15. Non è supportato da AEM.

### Verifica la versione JDK

Apri una nuova finestra del prompt dei comandi e digita: `java -version`. Devi recuperare la versione di JDK identificata da `JAVA_HOME` variable

![sorgente dati](assets/java-version.JPG)

## Installa Maven

Maven è uno strumento di automazione della build utilizzato principalmente per i progetti Java. Segui i seguenti passaggi per installare maven sul tuo sistema locale.

* Crea una cartella denominata `maven` nell&#39;unità C
* Scarica la [archivio zip binario](https://maven.apache.org/download.cgi)
* Estrai il contenuto dell’archivio zip in `c:\maven`
* Creare una variabile di ambiente denominata `M2_HOME` con un valore di `C:\maven\apache-maven-3.6.0`. Nel mio caso, il **mvn** La versione è 3.6.0. Al momento della stesura di questo articolo la versione più recente di maven è 3.6.3
* Aggiungi il `%M2_HOME%\bin` al tuo percorso
* Salva le modifiche
* Apri un nuovo prompt dei comandi e digita `mvn -version`. Dovresti vedere la **mvn** versione elencata come mostrato nella schermata seguente

![sorgente dati](assets/mvn-version.JPG)


## Installa Eclipse

Installa la versione più recente di [eclissi](https://www.eclipse.org/downloads/)

## Crea il primo progetto

Archetype è un toolkit per modelli di progetto Maven. Un archetipo è definito come un modello o modello originale dal quale vengono realizzate tutte le altre cose dello stesso tipo. Il nome si adatta quando cerchiamo di fornire un sistema che fornisca un mezzo coerente per generare progetti Maven. Archetype aiuta gli autori a creare modelli di progetto Maven per gli utenti e fornisce agli utenti i mezzi per generare versioni con parametri di tali modelli di progetto.
Per creare il tuo primo progetto Maven, segui i seguenti passaggi:

* Crea una nuova cartella denominata `aemformsbundles` nell&#39;unità C
* Apri un prompt dei comandi e passa a `c:\aemformsbundles`
* Esegui il seguente comando nel prompt dei comandi

```java
mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate -D archetypeGroupId=com.adobe.aem -D archetypeArtifactId=aem-project-archetype -D archetypeVersion=36 -D appTitle="My Site" -D appId="mysite" -D groupId="com.mysite" -D aemVersion=6.5.13
```

Al termine del completamento, nella finestra del comando dovrebbe essere visualizzato un messaggio di completamento della compilazione

## Crea un progetto eclipse dal tuo progetto Maven

* Cambia la directory di lavoro in `mysite`
* Esegui `mvn eclipse:eclipse` dalla riga di comando. Il comando legge il file pom e crea progetti Eclipse con metadati corretti in modo che Eclipse comprenda tipi di progetto, relazioni, percorsi di classe, ecc.

## Importa il progetto in eclissi

Launch **Eclipse**

Vai a **File -> Importa** e seleziona **Progetti Maven esistenti** come mostrato qui

![sorgente dati](assets/import-mvn-project.JPG)

Fai clic su Avanti

Seleziona il c:\aemformsbundles\mysite by clicking the **Sfoglia** pulsante

![sorgente dati](assets/mysite-eclipse-project.png)

>[!NOTE]
>Puoi scegliere di importare i moduli appropriati in base alle tue esigenze. Seleziona e importa solo il modulo principale, se hai intenzione di creare solo codice Java nel tuo progetto.

Fai clic su **Fine** per avviare il processo di importazione

Il progetto viene importato in Eclipse e sono disponibili diverse opzioni `mysite.xxxx` cartelle

Espandi la `src/main/java` in `mysite.core` cartella. Questa è la cartella in cui si scrive la maggior parte del codice.

![sorgente dati](assets/mysite-core-project.png)

## Includi SDK client AEM FD

Devi includere l’sdk client AEMFD nel tuo progetto per sfruttare i vari servizi forniti con AEM Forms. Fai riferimento a [SDK client AEMFD](https://mvnrepository.com/artifact/com.adobe.aemfd/aemfd-client-sdk) per includere l’SDK client appropriato nel progetto Maven. Devi includere l’SDK client FD AEM nella sezione dipendenze di `pom.xml` del progetto principale come mostrato di seguito.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

Per creare il progetto, effettua le seguenti operazioni:

* Apri **finestra del prompt dei comandi**
* Accedi a `c:\aemformsbundles\mysite\core`
* Esegui il comando `mvn clean install -PautoInstallBundle`
Il comando precedente crea e installa il bundle nel server AEM in esecuzione su `http://localhost:4502`. Il bundle è disponibile anche sul file system all&#39;indirizzo
   `C:\AEMFormsBundles\mysite\core\target` e può essere distribuito utilizzando [Console web Felix](http://localhost:4502/system/console/bundles)

## Passaggi successivi

[Crea servizio OSGi](./create-osgi-service.md)

