---
title: Creazione del primo bundle OSGi con i moduli AEM
description: Crea il tuo primo bundle OSGi utilizzando il cielo e l'eclisse
feature: administration
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 48060b4d8c4b502e0c099ae8081695f97b423037
workflow-type: tm+mt
source-wordcount: '830'
ht-degree: 1%

---


# Creare il primo bundle OSGi

Un bundle OSGi è un file di archivio Java™ contenente codice Java, risorse e un manifesto che descrive il bundle e le sue dipendenze. Il bundle è l&#39;unità di distribuzione per un&#39;applicazione. Questo articolo è destinato agli sviluppatori che desiderano creare un servizio OSGi o un servlet utilizzando  AEM Forms 6.4 o 6.5. Per creare il primo bundle OSGi, effettuate le seguenti operazioni:


## Installa JDK

Installa la versione supportata di JDK. Ho usato JDK1.8. Assicuratevi di aver aggiunto **JAVA_HOME** nelle variabili di ambiente e di puntare alla cartella principale dell&#39;installazione JDK.
Aggiungere il percorso %JAVA_HOME%/bin

![data-source](assets/java-home.JPG)

>[!NOTE]
> Non utilizzare JDK 15. Non è supportato da AEM.

### Verificare la versione JDK

Aprire una nuova finestra del prompt dei comandi e digitare: `java -version`. È necessario recuperare la versione di JDK identificata dalla variabile `JAVA_HOME`

![data-source](assets/java-version.JPG)

## Installazione di Maven

Maven è uno strumento di automazione di costruzione utilizzato principalmente per i progetti Java. Seguire i passaggi seguenti per installare il Paradiso sul sistema locale.

* Creare una cartella denominata `maven` nell&#39;unità C
* Scaricare l&#39;archivio ZIP binario [zip](http://maven.apache.org/download.cgi)
* Estrarre il contenuto dell&#39;archivio ZIP in `c:\maven`
* Create una variabile di ambiente denominata `M2_HOME` con un valore di `C:\maven\apache-maven-3.6.0`. Nel mio caso, la versione **mvn** è 3.6.0. Al momento della scrittura di questo articolo l&#39;ultima versione del cielo è 3.6.3
* Aggiungi `%M2_HOME%\bin` al percorso
* Salvare le modifiche
* Aprite un nuovo prompt dei comandi e digitate `mvn -version`. Dovresti vedere la versione **mvn** elencata come mostrato nella schermata seguente

![data-source](assets/mvn-version.JPG)

## Settings.xml

Un file Maven `settings.xml` definisce i valori che configurano l&#39;esecuzione Maven in vari modi. In genere, viene utilizzato per definire una posizione di repository locale, server di repository remoti alternativi e informazioni di autenticazione per i repository privati.

Passa a `C:\Users\<username>\.m2 folder`
Estrarre il contenuto del file [settings.zip](assets/settings.zip) e inserirlo nella cartella `.m2`.

## Installazione di Eclipse

Installare la versione più recente di [eclipse](https://www.eclipse.org/downloads/)

## Crea il primo progetto

Archetype è un progetto Maven che modella toolkit. Un archetipo è definito come un modello o un modello originale dal quale vengono realizzate tutte le altre cose dello stesso tipo. Il nome si adatta come stiamo cercando di fornire un sistema che fornisce un mezzo coerente per generare progetti Maven. Archetype aiuterà gli autori a creare modelli di progetto Maven per gli utenti e fornirà agli utenti i mezzi per generare versioni con parametri di tali modelli di progetto.
Per creare il tuo primo progetto &quot;Paradiso&quot;, procedi come segue:

* Creare una nuova cartella denominata `aemformsbundles` nell&#39;unità C
* Aprite un prompt dei comandi e andate a `c:\aemformsbundles`
* Eseguire il comando seguente nel prompt dei comandi
* `mvn archetype:generate  -DarchetypeGroupId=com.adobe.granite.archetypes  -DarchetypeArtifactId=aem-project-archetype -DarchetypeVersion=19`

Il progetto Maven verrà generato in modo interattivo e vi verrà chiesto di fornire valori a una serie di proprietà come

| Nome proprietà | Significatività | Valore |
------------------------|---------------------------------------|---------------------
| groupId | groupId identifica in modo univoco il progetto per tutti i progetti | com.learningaemforms.adobe |
| appsFolderName | Nome della cartella che conterrà la struttura del progetto | apprendimento di emodforms |
| artifactId | artifactId è il nome del jar senza versione. Se lo avete creato, potete scegliere qualsiasi nome vogliate con lettere minuscole e senza strani simboli. | apprendimento di emodforms |
| version | Se lo distribuite, potete scegliere qualsiasi versione tipica con numeri e punti (1.0, 1.1, 1.0.1, ...). | 1.0 |

Accettare i valori predefiniti per le altre proprietà premendo Invio.
Se tutto va bene, dovresti vedere un messaggio di successo build nella finestra di comando

## Crea progetto eclissi dal tuo progetto &quot;Paradiso&quot;

Cambia la directory di lavoro in `learningaemforms`.
Esecuzione di `mvn eclipse:eclipse` dalla riga di comando
Il comando precedente legge il file pom e crea progetti Eclipse con metadati corretti in modo che Eclipse possa comprendere i tipi di progetto, le relazioni, il percorso di classe, ecc.

## Importa il progetto in eclisse

Lancio **Eclipse**

Vai a **File -> Importa** e seleziona **Progetti Paradiso esistenti** come mostrato qui

![data-source](assets/import-mvn-project.JPG)

Fai clic su Avanti

Selezionare `c:\aemformsbundles\learningaemform`s facendo clic sul pulsante **Sfoglia**

![data-source](assets/select-mvn-project.JPG)

>[!NOTE]
>Potete scegliere di importare i moduli appropriati in base alle vostre esigenze. Selezionate e importate il modulo di base solo se intendete creare codice Java solo nel progetto.

Fare clic su **Fine** per avviare il processo di importazione

Il progetto viene importato in Eclipse e vengono visualizzate diverse cartelle `learningaemforms.xxxx`

Espandere la cartella `src/main/java` nella cartella `learningaemforms.core`. Questa è la cartella in cui scriverete la maggior parte del codice.

![data-source](assets/learning-core.JPG)

## Crea progetto

Dopo aver scritto il servizio o il servlet OSGi, sarà necessario creare il progetto per generare il bundle OSGi che può essere distribuito tramite la console Web Felix. Per includere l&#39;SDK client appropriato nel progetto Maven, fare riferimento a [AEMFD Client SDK](https://repo.adobe.com/nexus/content/repositories/public/com/adobe/aemfd/aemfd-client-sdk/). Dovrai includere l&#39;SDK client FD AEM nella sezione delle dipendenze di `pom.xml` del progetto principale, come mostrato di seguito.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

Per creare il progetto, attenetevi alla seguente procedura:

* Apri **finestra del prompt dei comandi**
* Accedi a `c:\aemformsbundles\learningaemforms\core`
* Esegui il comando `mvn clean install`
Se tutto va bene, si dovrebbe vedere il bundle nel seguente percorso `C:\AEMFormsBundles\learningaemforms\core\target`. Questo bundle è ora pronto per essere distribuito in AEM utilizzando la console Web di Felix.
