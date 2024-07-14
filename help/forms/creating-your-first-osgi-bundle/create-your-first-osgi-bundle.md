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
duration: 145
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '665'
ht-degree: 0%

---

# Creare il primo bundle OSGi

Un bundle OSGi è un file di archivio Java™ che contiene codice Java, risorse e un manifesto che descrive il bundle e le sue dipendenze. Il bundle è l’unità di distribuzione di un’applicazione. Questo articolo è destinato agli sviluppatori che desiderano creare un servizio OSGi o un servlet utilizzando AEM Forms 6.4 o 6.5. Per creare il primo bundle OSGi, segui i passaggi seguenti:


## Installare JDK

Installa la versione supportata di JDK. Ho usato JDK1.8. Assicurati di aver aggiunto **JAVA_HOME** nelle variabili di ambiente e di puntare alla cartella principale dell&#39;installazione JDK.
Aggiungere %JAVA_HOME%/bin al percorso

![origine dati](assets/java-home.JPG)

>[!NOTE]
> Non usi JDK 15. Non è supportato dall&#39;AEM.

### Verifica la versione JDK

Aprire una nuova finestra del prompt dei comandi e digitare: `java -version`. È necessario recuperare la versione di JDK identificata dalla variabile `JAVA_HOME`

![origine dati](assets/java-version.JPG)

## Installare Maven

Maven è uno strumento di automazione della build utilizzato principalmente per i progetti Java. Per installare Maven nel sistema locale, segui la procedura riportata di seguito.

* Crea una cartella denominata `maven` nell&#39;unità C
* Scarica l&#39;[archivio zip binario](https://maven.apache.org/download.cgi)
* Estrai il contenuto dell&#39;archivio zip in `c:\maven`
* Creare una variabile di ambiente denominata `M2_HOME` con valore `C:\maven\apache-maven-3.6.0`. Nel mio caso, la versione **mvn** è 3.6.0. Al momento di scrivere questo articolo, l’ultima versione di Maven è la 3.6.3
* Aggiungi `%M2_HOME%\bin` al tuo percorso
* Salva le modifiche
* Aprire un nuovo prompt dei comandi e digitare in `mvn -version`. Dovresti vedere la versione **mvn** elencata come mostrato nella schermata seguente

![origine dati](assets/mvn-version.JPG)


## Installare Eclipse

Installa la versione più recente di [eclipse](https://www.eclipse.org/downloads/)

## Crea il primo progetto

Archetipo è un toolkit per modelli di progetto Maven. Un archetipo è definito come un modello originale da cui vengono fatte tutte le altre cose dello stesso tipo. Il nome corrisponde a quello che stiamo cercando di fornire per fornire un sistema che fornisca un mezzo coerente per generare progetti Maven. Archetipo consente agli autori di creare modelli di progetto Maven per gli utenti e fornisce loro i mezzi per generare versioni con parametri di tali modelli di progetto.
Per creare il primo progetto Maven, segui i passaggi seguenti:

* Crea una nuova cartella denominata `aemformsbundles` nell&#39;unità C
* Apri un prompt dei comandi e passa a `c:\aemformsbundles`
* Esegui il comando seguente al prompt dei comandi

```java
mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate -D archetypeGroupId=com.adobe.aem -D archetypeArtifactId=aem-project-archetype -D archetypeVersion=36 -D appTitle="My Site" -D appId="mysite" -D groupId="com.mysite" -D aemVersion=6.5.13
```

Una volta completato correttamente, dovresti visualizzare un messaggio di generazione riuscita nella finestra di comando

## Crea un progetto di eclissi dal progetto Maven

* Cambia la directory di lavoro in `mysite`
* Eseguire `mvn eclipse:eclipse` dalla riga di comando. Il comando legge il file POM e crea progetti Eclipse con metadati corretti, in modo che Eclipse comprenda i tipi di progetto, le relazioni, il percorso di classe e così via.

## Importa il progetto nell&#39;eclissi

Avvia **Eclipse**

Vai a **File -> Importa** e seleziona **Progetti Maven esistenti** come mostrato qui

![origine dati](assets/import-mvn-project.JPG)

Fai clic su Successivo

Selezionare c:\aemformsbundles\miosito facendo clic sul pulsante **Sfoglia**

![origine dati](assets/mysite-eclipse-project.png)

>[!NOTE]
>Puoi scegliere di importare i moduli appropriati in base alle tue esigenze. Seleziona e importa solo il modulo core, se desideri creare solo codice Java nel progetto.

Fai clic su **Fine** per avviare il processo di importazione

Il progetto viene importato in Eclipse e sono presenti diverse `mysite.xxxx` cartelle

Espandere `src/main/java` nella cartella `mysite.core`. Questa è la cartella in cui si sta scrivendo la maggior parte del codice.

![origine dati](assets/mysite-core-project.png)

## Includi AEMFD Client SDK

Devi includere l’sdk del client AEMFD nel progetto per sfruttare i vari servizi forniti con AEM Forms. Fai riferimento a [AEMFD Client SDK](https://mvnrepository.com/artifact/com.adobe.aemfd/aemfd-client-sdk) per includere l&#39;SDK client appropriato nel progetto Maven. È necessario includere l&#39;SDK del client AEM FD nella sezione delle dipendenze di `pom.xml` del progetto principale, come illustrato di seguito.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

Per creare il progetto, effettua le seguenti operazioni:

* Apri **finestra del prompt dei comandi**
* Passa a `c:\aemformsbundles\mysite\core`
* Esegui il comando `mvn clean install -PautoInstallBundle`
Il comando precedente crea e installa il bundle nel server AEM in esecuzione su `http://localhost:4502`. Il bundle è disponibile anche sul file system all’indirizzo
  `C:\AEMFormsBundles\mysite\core\target` e può essere distribuito utilizzando [Console Web Felix](http://localhost:4502/system/console/bundles)

## Passaggi successivi

[Crea servizio OSGi](./create-osgi-service.md)

