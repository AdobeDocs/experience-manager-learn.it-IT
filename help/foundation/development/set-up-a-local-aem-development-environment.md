---
title: Configurare un ambiente di sviluppo AEM locale
description: Guida alla configurazione di uno sviluppo locale per Adobe Experience Manager, AEM. Sono trattati argomenti importanti come installazione locale, Apache Maven, ambienti di sviluppo integrati e debug/risoluzione dei problemi. Si parla di sviluppo con Eclipse IDE, CRXDE-Lite, Visual Studio Code e IntelliJ.
version: 6.4, 6.5
feature: maven-archetype
topics: development
activity: develop
audience: developer
translation-type: tm+mt
source-git-commit: c85a59a8bd180d5affe2a5bf5939dabfb2776d73
workflow-type: tm+mt
source-wordcount: '2590'
ht-degree: 1%

---


# Configurare un ambiente di sviluppo AEM locale

Guida alla configurazione di uno sviluppo locale per Adobe Experience Manager, AEM. Sono trattati argomenti importanti come installazione locale, Apache Maven, ambienti di sviluppo integrati e debug/risoluzione dei problemi. Vengono discussi lo sviluppo con **[!DNL Eclipse IDE], [!DNL CRXDE Lite], [!DNL Visual Studio Code] e[!DNL IntelliJ]**.

## Panoramica

La configurazione di un ambiente di sviluppo locale è il primo passo nello sviluppo per Adobe Experience Manager o AEM. Dedica il tempo necessario a configurare un ambiente di sviluppo di qualità per aumentare la produttività e scrivere codice migliore, più velocemente. Possiamo rompere un ambiente di sviluppo AEM locale in 4 aree:

* Istanze AEM locali
* [!DNL Apache Maven] project
* Ambienti di sviluppo integrati (IDE)
* Risoluzione dei problemi

## Installare istanze AEM locali

Quando facciamo riferimento a un’istanza AEM locale, stiamo parlando di una copia di Adobe Experience Manager in esecuzione sul computer personale di uno sviluppatore. ***Lo sviluppo*** AllAEM deve iniziare scrivendo ed eseguendo il codice in base a un&#39;istanza AEM locale.

Se non avete mai AEM, è possibile installare due modalità di esecuzione di base: ***Autore*** e ***Pubblica***. L&#39; ***Autore*** [modalità di esecuzione](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/configure-runmodes.html) è l&#39;ambiente che gli esperti di marketing digitale utilizzeranno per creare e gestire i contenuti. Nello sviluppo di **la maggior parte** del tempo, distribuirete il codice in un&#39;istanza Author. Questo consente di creare nuove pagine e di aggiungere e configurare componenti.  AEM Sites è un CMS di authoring WYSIWYG e pertanto la maggior parte dei CSS e JavaScript può essere testata rispetto a un’istanza di authoring.

È anche *codice di test critico* rispetto a un&#39;istanza ***Publish*** locale. L&#39;istanza ***Pubblica*** è l&#39;ambiente AEM con cui i visitatori del sito Web interagiscono. Anche se l&#39;istanza ***Publish*** è lo stesso stack di tecnologia dell&#39;istanza ***Author***, esistono alcune importanti distinzioni con le configurazioni e le autorizzazioni. Prima di essere promossi a ambienti di livello superiore, il codice deve essere sempre sottoposto a test *always* in base a un&#39;istanza ***Publish*** locale.

### Passaggi

1. Assicurarsi che [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html) sia installato.
   * Preferisci [Java JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.property.operation=equals&amp;1_group.property.values.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=list p.offset=0&amp;p.limit=14) per AEM 6.5+
   * [Java JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html#JDK8) per AEM versioni precedenti a AEM 6.5
2. Ottenete una copia del [AEM QuickStart Jar e di un [!DNL license.properties]](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware).
3. Create una struttura di cartelle sul computer come segue:

   ```plain
   ~/aem-sdk
       /author
       /publish
   ```

4. Rinominare il JAR [!DNL QuickStart] in ***aem-author-p4502.jar*** e posizionarlo sotto la directory `/author`. Aggiungete il file ***[!DNL license.properties]*** sotto la directory `/author`.
5. Copiate il file [!DNL QuickStart] JAR, rinominatelo in ***aem-publish-p4503.jar*** e inseritelo sotto la directory `/publish`. Aggiungete una copia del file ***[!DNL license.properties]*** sotto la directory `/publish`.

   ```plain
   ~/aem-sdk
       /author
           + aem-author-p4502.jar
           + license.properties
       /publish
           + aem-publish-p4503.jar
           + license.properties
   ```

6. Fare doppio clic sul file ***aem-author-p4502.jar*** per installare l&#39;istanza **Author**. Verrà avviata l&#39;istanza di creazione, in esecuzione sulla porta **4502** del computer locale.

   Fate doppio clic sul file ***aem-publish-p4503.jar*** per installare l&#39;istanza **Pubblica**. Verrà avviata l&#39;istanza Pubblica, in esecuzione sulla porta **4503** del computer locale.

   >[!NOTE]
   >
   >A seconda dell&#39;hardware del computer di sviluppo, potrebbe essere difficile avere un&#39;istanza **Author e Publish** contemporaneamente in esecuzione. Raramente è necessario eseguire entrambi contemporaneamente su una configurazione locale.

   Per ulteriori informazioni, vedere [Implementazione e manutenzione di un&#39;istanza AEM](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html).

## Installazione di Apache Maven

***[!DNL Apache Maven]*** è uno strumento per gestire la procedura di creazione e implementazione per i progetti basati su Java. AEM è una piattaforma basata su Java e [!DNL Maven] è il metodo standard per gestire il codice di un progetto AEM. Quando diciamo ***AEM progetto Maven*** o semplicemente il progetto ***AEM***, ci riferiamo a un progetto Maven che include tutto il codice *personalizzato* del sito.

Tutti i progetti AEM devono essere realizzati sulla base dell&#39;ultima versione di **[!DNL AEM Project Archetype]**: [https://github.com/Adobe-Marketing-Cloud/aem-project-archetype](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype). Il [!DNL AEM Project Archetype] creerà un programma di avvio automatico di un progetto AEM con codice e contenuto di esempio. [!DNL AEM Project Archetype] include anche **[!DNL AEM WCM Core Components]** configurato per essere utilizzato nel progetto.

>[!CAUTION]
>
>Quando si avvia un nuovo progetto, è consigliabile utilizzare la versione più recente dell&#39;archetipo. Tenere presente che esistono più versioni dell&#39;archetype e che non tutte le versioni sono compatibili con le versioni precedenti di AEM.

### Passaggi

1. Scarica [Apache Maven](https://maven.apache.org/download.cgi)
2. Installare [Apache Maven](https://maven.apache.org/install.html) e assicurarsi che l&#39;installazione sia stata aggiunta alla riga di comando `PATH`.
   * [!DNL macOS] gli utenti possono installare Maven utilizzando  [Homebrew](https://brew.sh/)
3. Verificare che **[!DNL Maven]** sia installato aprendo un nuovo terminale della riga di comando ed eseguendo quanto segue:

   ```shell
   $ mvn --version
   Apache Maven 3.3.9
   Maven home: /Library/apache-maven-3.3.9
   Java version: 1.8.0_111, vendor: Oracle Corporation
   Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
   Default locale: en_US, platform encoding: UTF-8
   ```

4. Aggiungete il profilo **[!DNL adobe-public]** al file [!DNL Maven] [settings.xml](https://maven.apache.org/settings.html) in modo da aggiungere automaticamente **[!DNL repo.adobe.com]** al processo di creazione maven.

5. Create un file denominato `settings.xml` in `~/.m2/settings.xml` se non esiste già.

6. Aggiungete il profilo **[!DNL adobe-public]** al file `settings.xml` in base alle [istruzioni qui](https://repo.adobe.com/).

   Un esempio `settings.xml` è elencato di seguito. *Nota: la convenzione di denominazione  `settings.xml` e la posizione al di sotto della  `.m2` directory dell&#39;utente sono importanti.*

   ```xml
   <settings xmlns="https://maven.apache.org/SETTINGS/1.0.0"
     xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="https://maven.apache.org/SETTINGS/1.0.0
                         https://maven.apache.org/xsd/settings-1.0.0.xsd">
   <profiles>
    <!-- ====================================================== -->
    <!-- A D O B E   P U B L I C   P R O F I L E                -->
    <!-- ====================================================== -->
        <profile>
            <id>adobe-public</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <properties>
                <releaseRepository-Id>adobe-public-releases</releaseRepository-Id>
                <releaseRepository-Name>Adobe Public Releases</releaseRepository-Name>
                <releaseRepository-URL>https://repo.adobe.com/nexus/content/groups/public</releaseRepository-URL>
            </properties>
            <repositories>
                <repository>
                    <id>adobe-public-releases</id>
                    <name>Adobe Public Repository</name>
                    <url>https://repo.adobe.com/nexus/content/groups/public</url>
                    <releases>
                        <enabled>true</enabled>
                        <updatePolicy>never</updatePolicy>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </repository>
            </repositories>
            <pluginRepositories>
                <pluginRepository>
                    <id>adobe-public-releases</id>
                    <name>Adobe Public Repository</name>
                    <url>https://repo.adobe.com/nexus/content/groups/public</url>
                    <releases>
                        <enabled>true</enabled>
                        <updatePolicy>never</updatePolicy>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </pluginRepository>
            </pluginRepositories>
        </profile>
   </profiles>
    <activeProfiles>
        <activeProfile>adobe-public</activeProfile>
    </activeProfiles>
   </settings>
   ```

7. Verificare che il profilo **adobe-public** sia attivo eseguendo il comando seguente:

   ```shell
   $ mvn help:effective-settings
   ...
   <activeProfiles>
       <activeProfile>adobe-public</activeProfile>
   </activeProfiles>
   <pluginGroups>
       <pluginGroup>org.apache.maven.plugins</pluginGroup>
       <pluginGroup>org.codehaus.mojo</pluginGroup>
   </pluginGroups>
   </settings>
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  0.856 s
   ```

   Se non viene visualizzato il **[!DNL adobe-public]**, significa che il riferimento al repo del Adobe  non è correttamente riportato nel file `~/.m2/settings.xml`. Rivedete i passaggi precedenti e verificate che il file settings.xml faccia riferimento al repo del Adobe .

## Configurare un ambiente di sviluppo integrato

Un ambiente di sviluppo integrato o IDE è un&#39;applicazione che combina un editor di testo, un supporto della sintassi e strumenti di compilazione. A seconda del tipo di sviluppo che si sta facendo, un IDE potrebbe essere preferibile rispetto a un altro. Indipendentemente dall&#39;IDE, sarà importante essere in grado di ***inviare periodicamente*** il codice a un&#39;istanza AEM locale per testarlo. Sarà inoltre importante ***tirare*** occasionalmente configurazioni da un&#39;istanza AEM locale al progetto AEM per mantenere un sistema di gestione del controllo del codice sorgente come Git.

Di seguito sono riportati alcuni degli IDE più popolari utilizzati con lo sviluppo AEM video corrispondenti che mostrano l&#39;integrazione con un&#39;istanza AEM locale.

### [!DNL Eclipse] IDE

Il **[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** è uno degli IDE più popolari per lo sviluppo Java, in gran parte perché è open source e ***free***!  Adobe fornisce un plugin, **[[!DNL AEM Developer Tools]](https://eclipse.adobe.com/aem/dev-tools/)**, per [!DNL Eclipse] per consentire uno sviluppo più semplice con una bella interfaccia grafica per sincronizzare il codice con un&#39;istanza AEM locale. L&#39;IDE [!DNL Eclipse] è consigliato per gli sviluppatori che non AEM in gran parte a causa del supporto GUI di [!DNL AEM Developer Tools].

#### Installazione e configurazione

1. Scaricare e installare l&#39;IDE [!DNL Eclipse] per [!DNL Java EE Developers]: [https://www.eclipse.org](https://www.eclipse.org/)
1. Seguite le istruzioni per installare il plug-in [!DNL AEM Developer Tools]: [https://eclipse.adobe.com/aem/dev-tools/](https://eclipse.adobe.com/aem/dev-tools/)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 - Importa progetto Paradiso
* 01:24 - Creare e distribuire il codice sorgente con Maven
* 04:33 - Modifiche al codice push con AEM Developer Tool
* 10:55 - Modifiche al codice pull con AEM Developer Tool
* 13:12 - Utilizzo degli strumenti di debug integrati di Eclipse

### IDEA IntelliJ

L&#39; **[IntelliJ IDEA](https://www.jetbrains.com/idea/)** è un IDE potente per lo sviluppo Java professionale. [!DNL IntelliJ IDEA] viene in due sapori, una  ****** [!DNL Community] freeedition e una  [!DNL Ultimate] versione commerciale (pagata). La versione gratuita [!DNL Community] di [!DNL IntellIJ IDEA] è sufficiente per un ulteriore sviluppo AEM, tuttavia [!DNL Ultimate] [espande la propria funzionalità impostata](https://www.jetbrains.com/idea/download).

#### [!DNL Installation and Setup]

1. Scarica e installa [!DNL IntelliJ IDEA]: [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. Installazione di [!DNL Repo] (strumento della riga di comando): [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

* 00:00 - Importa progetto Paradiso
* 05:47 - Creare e distribuire il codice sorgente con Maven
* 08:17 - Modifiche push con Repo
* 14:39 - Modifiche pull con Repo
* 17:25 - Utilizzo degli strumenti di debug integrati di IntelliJ IDEA

### [!DNL Visual Studio Code]

**[Il ](https://code.visualstudio.com/)** codice di Visual Studio è diventato rapidamente uno strumento preferito per  ***gli sviluppatori*** front-end, con supporto JavaScript avanzato  [!DNL Intellisense]e supporto per il debug dei browser. **[!DNL Visual Studio Code]** è open source, gratuito, con molte potenti estensioni. [!DNL Visual Studio Code] può essere impostato per l&#39;integrazione con AEM con l&#39;aiuto di uno strumento di Adobe ,  **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code).** Sono inoltre disponibili diverse estensioni supportate dalla community che possono essere installate per l&#39;integrazione con AEM.

[!DNL Visual Studio Code] è una grande scelta per gli sviluppatori front-end che scriveranno principalmente codice CSS/LESS e JavaScript per creare AEM librerie client. Questo strumento potrebbe non essere la scelta migliore per i nuovi sviluppatori AEM poiché le definizioni dei nodi (finestre di dialogo, componenti) dovranno essere modificate in XML non elaborato. Sono disponibili diverse estensioni Java per [!DNL Visual Studio Code], tuttavia se si preferisce eseguire principalmente lo sviluppo Java [!DNL Eclipse IDE] o [!DNL IntelliJ].

#### Collegamenti importanti

* [**Codice**](https://code.visualstudio.com/Download) **DownloadVisual Studio**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)**  - Strumento simile a FTP per il contenuto JCR
* **[aemfeed](https://aemfed.io/)**  - Velocizza il flusso di lavoro front-end AEM
* **[AEM Sync](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** - Estensione supportata dalla community* per Visual Studio Code

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 - Importa progetto Paradiso
* 00:53 - Creazione e implementazione del codice sorgente con Maven
* 04:03 - Modifiche al codice push con lo strumento della riga di comando Repo
* 08:29 - Modifiche al codice pull con lo strumento della riga di comando Repo
* 10:40 - Modifiche al codice push con lo strumento Invia per e-mail
* 14:24 - Risoluzione dei problemi, Ricostruzione delle librerie client

### [!DNL CRXDE Lite]

[CRXDE ](https://helpx.adobe.com/experience-manager/6-4/sites/developing/using/developing-with-crxde-lite.html) Liteè una visualizzazione basata su browser dell&#39;archivio AEM. [!DNL CRXDE Lite] è incorporato in AEM e consente a uno sviluppatore di eseguire attività di sviluppo standard come la modifica di file, la definizione di componenti, finestre di dialogo e modelli. [!DNL CRXDE Lite] non è  ****** destinato a essere un ambiente di sviluppo completo, ma è molto efficace come strumento di debug. [!DNL CRXDE Lite] è utile per estendere o semplicemente comprendere il codice prodotto al di fuori della base di codice. [!DNL CRXDE Lite] fornisce una vista efficace dell&#39;archivio e un modo efficace per testare e gestire le autorizzazioni.

[!DNL CRXDE Lite] devono essere sempre utilizzati insieme ad altri IDE per testare e debug il codice, ma mai come strumento di sviluppo principale. È dotato di un supporto di sintassi limitato, di funzionalità di auto-completamento e di un&#39;integrazione limitata con i sistemi di gestione del controllo del codice sorgente.

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## Risoluzione dei problemi

***Aiuto!*** Il mio codice non funziona! Come per tutti gli sviluppi, ci saranno tempi (probabilmente molti), in cui il codice non funziona come previsto. AEM è una piattaforma potente, ma con grande potenza... arriva una grande complessità. Di seguito sono riportati alcuni punti di partenza di alto livello per la risoluzione dei problemi e il tracciamento dei problemi (ma ben lungi dall&#39;essere un elenco esaustivo delle cose che possono andare storte):

### Verifica distribuzione codice

Un primo passo utile, quando si verifica un problema, è verificare che il codice sia stato distribuito e installato correttamente per AEM.

1. **Selezionate  [!UICONTROL Gestione]** pacchetti per verificare che il pacchetto di codice sia stato caricato e installato:  [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Controllate la marca temporale per verificare che il pacchetto sia stato installato di recente.
1. Se si eseguono aggiornamenti incrementali dei file utilizzando uno strumento come [!DNL Repo] o [!DNL AEM Developer Tools], **controllare[!DNL CRXDE Lite]** che il file sia stato inviato all&#39;istanza AEM locale e che il contenuto del file sia aggiornato: [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **Verificate che il bundle sia in fase di** caricamento se vengono riscontrati problemi relativi al codice Java in un bundle OSGi. Apri la [!UICONTROL console Web Adobe Experience Manager]: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) e cercate il bundle. Verificate che il bundle abbia uno stato **[!UICONTROL Attivo]**. Per ulteriori informazioni sulla risoluzione dei problemi relativi a un bundle nello stato **[!UICONTROL Installed]**, vedi sotto.

#### Controllare i registri

AEM è una piattaforma chatty e registra molte informazioni utili in **error.log**. È possibile trovare **error.log** dove è stato installato AEM: &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

Una tecnica utile per tenere traccia dei problemi consiste nell’aggiungere istruzioni di registro nel codice Java:

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
...

public class MyClass {
    private final Logger log = LoggerFactory.getLogger(getClass());

    ...

    String myVariable = "My Variable";

    log.debug("Debug statement of myVariable {}", myVariable);

    log.info("Info statement of myVariable {}", myVariable);
}
```

Per impostazione predefinita, il file **error.log** è configurato per registrare le istruzioni *[!DNL INFO]*. Se si desidera modificare il livello di registro è possibile eseguire questa operazione scegliendo [!UICONTROL Supporto log]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). È inoltre possibile che il file **error.log** sia troppo chatty. Potete utilizzare il [!UICONTROL Log Support] per configurare le istruzioni di registro per un pacchetto Java specificato. Si tratta di una procedura consigliata per i progetti, al fine di separare facilmente i problemi di codice personalizzati dai problemi della piattaforma OOOTB AEM.

![Configurazione di registrazione in AEM](./assets/set-up-a-local-aem-development-environment/logging.png)

#### Il pacchetto è in stato Installato {#bundle-active}

Tutti i bundle (esclusi i frammenti) devono essere in stato **[!UICONTROL Attivo]**. Se visualizzi il bundle di codice nello stato [!UICONTROL Installed], si verifica un problema che deve essere risolto. Nella maggior parte dei casi si tratta di un problema di dipendenza:

![Errore nel pacchetto in AEM](assets/set-up-a-local-aem-development-environment/bundle-error.png)

Nella schermata precedente, lo stato [!DNL WKND Core bundle] è [!UICONTROL Installed]. Questo perché il bundle prevede una versione diversa di `com.adobe.cq.wcm.core.components.models` rispetto a quella disponibile nell&#39;istanza AEM.

Uno strumento utile è il [!UICONTROL Finder dipendenze]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). Aggiungete il nome del pacchetto Java per esaminare quale versione è disponibile nell’istanza AEM:

![Componenti core](assets/set-up-a-local-aem-development-environment/core-components.png)

Continuando con l&#39;esempio precedente, si può vedere che la versione installata nell&#39;istanza AEM è **12.2** rispetto a **12.6** che il bundle era previsto. Da qui è possibile lavorare all&#39;indietro e vedere se le dipendenze [!DNL Maven] in AEM corrispondono alle dipendenze [!DNL Maven] nel progetto AEM. Nell&#39;esempio precedente [!DNL Core Components] **v2.2.0** è installato nell&#39;istanza AEM, ma il bundle di codice è stato creato con una dipendenza su **v2.2.2**, di conseguenza il motivo del problema di dipendenza.

#### Verifica registrazione modelli Sling {#osgi-component-sling-models}

AEM componenti devono sempre essere supportati da un [!DNL Sling Model] per racchiudere qualsiasi logica aziendale e assicurarsi che lo script di rendering HTL rimanga pulito. Se si verificano problemi in cui non è possibile trovare il modello Sling, può essere utile controllare la [!DNL Sling Models] dalla console: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). Questo indica se il modello Sling è stato registrato e a quale tipo di risorsa (il percorso del componente) è associato.

![Stato modello Sling](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

Mostra la registrazione di un [!DNL Sling Model], `BylineImpl` associato a un tipo di risorsa componente di `wknd/components/content/byline`.

#### Problemi CSS o JavaScript

Per la maggior parte dei problemi relativi a CSS e JavaScript, l&#39;utilizzo degli strumenti di sviluppo del browser è il modo più efficace per risolvere i problemi. Per limitare il problema durante lo sviluppo rispetto a un’istanza di autore AEM, è utile visualizzare la pagina &quot;come Pubblicato&quot;.

![Problemi CSS o JS](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

Aprire il menu [!UICONTROL Proprietà pagina] e fare clic su [!UICONTROL Visualizza come pubblicato]. Viene aperta la pagina senza l&#39;editor AEM e con un parametro di query impostato su **wcmmode=disabled**. In questo modo si disattiverà l’interfaccia utente di authoring AEM e si faciliteranno la risoluzione dei problemi e il debug dei problemi front-end.

Un altro problema comunemente riscontrato durante lo sviluppo del codice front-end è la vecchia versione o il caricamento di CSS/JS non aggiornato. Come primo passo, accertatevi che la cronologia del browser sia stata cancellata e, se necessario, avviate un browser incognito o una nuova sessione.

#### Debug delle librerie client

Con diversi metodi di categorie e incorpora per includere più librerie client, la risoluzione dei problemi può essere complicata. AEM espone diversi strumenti per aiutarlo. Uno degli strumenti più importanti è [!UICONTROL Ricostruisci librerie client], che costringerà AEM a ricompilare qualsiasi file LESS e generare il CSS.

* [Dump Libs](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  - Elenca tutte le librerie client registrate nell&#39;istanza AEM. &lt;host>/libs/granite/ui/content/dumplibs.html
* [Output](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  test: consente all&#39;utente di visualizzare l&#39;output HTML previsto di clientlib in base alla categoria. &lt;host>/libs/granite/ui/content/dumplibs.test.html
* [Convalida](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  delle dipendenze delle librerie - evidenzia eventuali dipendenze o categorie incorporate che non è possibile trovare. &lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [Rigenerare le librerie](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  client: consente all&#39;utente di forzare la AEM per ricreare tutte le librerie client o di annullare la validità della cache delle librerie client. Questo strumento è particolarmente efficace quando si sviluppa con MENO, in quanto può costringere AEM a ricompilare il CSS generato. In generale è più efficace annullare la validità delle cache, quindi eseguire un aggiornamento delle pagine anziché ricreare tutte le librerie. &lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![Debug di ClientLibs](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>Se dovete continuamente annullare la validità della cache utilizzando lo strumento [!UICONTROL Ricrea librerie client], potrebbe essere utile eseguire una sola volta la ricostruzione di tutte le librerie client. Ciò potrebbe richiedere circa 15 minuti, ma in genere elimina eventuali problemi di cache in futuro.
