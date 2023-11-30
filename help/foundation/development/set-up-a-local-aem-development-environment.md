---
title: Configurare un ambiente locale di sviluppo AEM
description: Scopri come impostare un ambiente di sviluppo locale, ad Experience Manager. Acquisisci familiarità con l’installazione locale, Apache Maven, gli ambienti di sviluppo integrati e il debug e la risoluzione dei problemi. Utilizzare Eclipse IDE, CRXDE-Lite, Visual Studio Code e IntelliJ.
version: 6.5
feature: Developer Tools
topic: Development
role: Developer
level: Beginner
exl-id: 58851624-71c9-4745-aaaf-305acf6ccb14
last-substantial-update: 2022-07-20T00:00:00Z
doc-type: Tutorial
thumbnail: aem-local-dev-env.jpg
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '2603'
ht-degree: 2%

---

# Configurare un ambiente di sviluppo AEM locale

Guida alla configurazione di uno sviluppo locale per Adobe Experience Manager, AEM. Include argomenti importanti sull’installazione locale, Apache Maven, gli ambienti di sviluppo integrati e il debug/risoluzione dei problemi. Sviluppo con **Eclipse IDE, CRXDE Liti, Visual Studio Code e IntelliJ** sono discussi.

## Panoramica

La creazione di un ambiente di sviluppo locale rappresenta il primo passo per lo sviluppo per Adobe Experience Manager o AEM. Prenditi il tempo necessario per configurare un ambiente di sviluppo di qualità che aumenti la produttività e scriva codice migliore, più rapidamente. Possiamo suddividere l&#39;ambiente di sviluppo locale dell&#39;AEM in quattro aree:

* Istanze AEM locali
* [!DNL Apache Maven] progetto
* Ambienti di sviluppo integrato (IDE)
* Risoluzione dei problemi

## Installare istanze AEM locali

Quando si fa riferimento a un’istanza AEM locale, si parla di una copia di Adobe Experience Manager in esecuzione sul computer personale di uno sviluppatore. ***Tutti*** Lo sviluppo dell’AEM deve iniziare scrivendo ed eseguendo il codice rispetto a un’istanza AEM locale.

Se non si ha familiarità con l’AEM, è possibile installare due modalità di esecuzione di base: ***Autore*** e ***Pubblica***. Il ***Autore*** [runmode](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/configuring/configure-runmodes.html?lang=en)  è l’ambiente che gli esperti di marketing digitale utilizzano per creare e gestire i contenuti. Quando si sviluppa la maggior parte del tempo, si distribuisce il codice in un’istanza di authoring. Questo consente di creare pagine e aggiungere e configurare componenti. AEM Sites è un CMS di authoring WYSIWYG e quindi la maggior parte dei CSS e JavaScript può essere testata rispetto a un’istanza di authoring.

È anche *critico* test del codice in base a un ***Pubblica*** dell&#39;istanza. Il ***Pubblica*** è l’ambiente AEM con cui i visitatori del sito web interagiscono. Mentre il ***Pubblica*** è lo stesso stack di tecnologia del ***Autore*** Ad esempio, esistono alcune distinzioni importanti tra configurazioni e autorizzazioni. Il codice deve essere testato in base a un ***Pubblica*** prima di essere promossi in ambienti di livello superiore.

### Passaggi

1. Verificare che Java™ sia installato.
   * Preferisci [Java™ JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=tipo di software%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) per AEM 6.5+
   * [Java™ JDK 8](https://www.oracle.com/java/technologies/downloads/) per le versioni AEM precedenti alla AEM 6.5
1. Ottieni una copia di [JAR QuickStart per AEM e un [!DNL license.properties]](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html?lang=it).
1. Crea nel computer una struttura di cartelle come quella riportata di seguito:

```plain
~/aem-sdk
    /author
    /publish
```

1. Rinomina il [!DNL QuickStart] JAR a ***aem-author-p4502.jar*** e posizionalo sotto il `/author` directory. Aggiungi il ***[!DNL license.properties]*** file sotto `/author` directory.

1. Crea una copia di [!DNL QuickStart] JAR, rinominalo in ***aem-publish-p4503.jar*** e posizionalo sotto il `/publish` directory. Aggiungi una copia del ***[!DNL license.properties]*** file sotto `/publish` directory.

```plain
~/aem-sdk
    /author
        + aem-author-p4502.jar
        + license.properties
    /publish
        + aem-publish-p4503.jar
        + license.properties
```

1. Fai doppio clic su ***aem-author-p4502.jar*** file per installare **Autore** dell&#39;istanza. Viene avviata l’istanza di authoring, in esecuzione sulla porta **4502** nel computer locale.

Fai doppio clic su ***aem-publish-p4503.jar*** file per installare **Pubblica** dell&#39;istanza. Viene avviata l’istanza Publish, in esecuzione sulla porta **4503** nel computer locale.

>[!NOTE]
>
>A seconda dell&#39;hardware della macchina di sviluppo, potrebbe essere difficile disporre di entrambi i **Creazione e pubblicazione** istanza in esecuzione contemporaneamente. Raramente è necessario eseguire entrambe le operazioni contemporaneamente su una configurazione locale.

### Utilizzo della riga di comando

Un’alternativa al doppio clic sul file JAR è avviare AEM dalla riga di comando o creare uno script (`.bat` o `.sh`) a seconda del sistema operativo locale. Di seguito è riportato un esempio del comando di esempio:

```shell
$ java -Xmx2048M -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=30303 -jar aem-author-p4502.jar -gui -r"author,localdev"
```

Ecco, il `-X` sono opzioni JVM e `-D` sono proprietà di framework aggiuntive. Per ulteriori informazioni, consulta [Distribuzione e manutenzione di un’istanza AEM](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html?lang=it) e [Ulteriori opzioni disponibili nel file Quickstart](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/custom-standalone-install.html#further-options-available-from-the-quickstart-file).

## Installare Apache Maven

***[!DNL Apache Maven]*** è uno strumento per gestire la procedura di compilazione e distribuzione per i progetti basati su Java. AEM è una piattaforma basata su Java e [!DNL Maven] è il metodo standard per gestire il codice per un progetto AEM. Quando diciamo ***Progetto AEM Maven*** o solo il tuo ***Progetto AEM***, si tratta di un progetto Maven che include tutti i *personalizzato* codice per il sito.

Tutti i progetti AEM devono basarsi sulla versione più recente del **[!DNL AEM Project Archetype]**: [https://github.com/adobe/aem-project-archetype](https://github.com/adobe/aem-project-archetype). Il [!DNL AEM Project Archetype] fornisce un avvio di un progetto AEM con codice e contenuto di esempio. Il [!DNL AEM Project Archetype] include anche **[!DNL AEM WCM Core Components]** configurato per essere utilizzato nel progetto.

>[!CAUTION]
>
>All’avvio di un nuovo progetto, è consigliabile utilizzare la versione più recente dell’archetipo. Tieni presente che esistono più versioni dell’archetipo e che non tutte le versioni sono compatibili con le versioni precedenti di AEM.

### Passaggi

1. Scarica [Apache Maven](https://maven.apache.org/download.cgi)
2. Installa [Apache Maven](https://maven.apache.org/install.html) e accertarsi che l&#39;installazione sia stata aggiunta alla riga di comando `PATH`.
   * [!DNL macOS] gli utenti possono installare Maven utilizzando [Homebrew](https://brew.sh/)
3. Verifica che **[!DNL Maven]** viene installato aprendo un nuovo terminale della riga di comando ed eseguendo le operazioni seguenti:

```shell
$ mvn --version
Apache Maven 3.3.9
Maven home: /Library/apache-maven-3.3.9
Java version: 1.8.0_111, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
Default locale: en_US, platform encoding: UTF-8
```

>[!NOTE]
>
> In, l’aggiunta passata di `adobe-public` Il profilo Maven doveva indicare `nexus.adobe.com` per scaricare gli artefatti AEM. Tutti gli artefatti AEM sono ora disponibili tramite Maven Central e il `adobe-public` profilo non necessario.

## Configurare un ambiente di sviluppo integrato

Un ambiente di sviluppo integrato o IDE è un’applicazione che combina un editor di testo, il supporto della sintassi e strumenti di creazione. A seconda del tipo di sviluppo che si sta effettuando, un IDE potrebbe essere preferibile rispetto ad un altro. Indipendentemente dall’IDE, è importante essere in grado di ***push*** a un’istanza AEM locale per testarla. Occasionalmente è importante ***tirare*** configurazioni da un’istanza AEM locale nel progetto AEM al fine di mantenere un sistema di gestione del controllo del codice sorgente come Git.

Di seguito sono riportati alcuni degli IDE più popolari utilizzati con lo sviluppo dell’AEM e i relativi video che mostrano l’integrazione con un’istanza dell’AEM locale.

>[!NOTE]
>
> Il progetto WKND è stato aggiornato per impostare il funzionamento su AEM as a Cloud Service. È stato aggiornato per essere [retrocompatibile con 6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx). Se si utilizza AEM 6.5 o 6.4, aggiungere `classic` profilo a qualsiasi comando Maven.

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Quando, utilizzando un IDE, verificare `classic` nella scheda Profilo Maven.

![Scheda Profilo Maven](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*Profilo IntelliJ Maven*

### [!DNL Eclipse] IDE

Il **[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** è uno degli IDE più popolari per lo sviluppo Java™, in gran parte perché è open source e ***gratuito***! Adobe fornisce un plug-in, **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)**, per [!DNL Eclipse] per consentire uno sviluppo più semplice con una bella interfaccia grafica per sincronizzare il codice con un’istanza AEM locale. Il [!DNL Eclipse] L&#39;IDE è consigliato per sviluppatori che non hanno mai utilizzato AEM in gran parte grazie al supporto GUI fornito da [!DNL AEM Developer Tools].

#### Installazione e configurazione

1. Scarica e installa [!DNL Eclipse] IDE per [!DNL Java™ EE Developers]: [https://www.eclipse.org](https://www.eclipse.org/)
1. Seguire le istruzioni per installare [!DNL AEM Developer Tools] plugin: [https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 - Importa progetto Maven
* 01:24 - Creare e distribuire il codice sorgente con Maven
* 04:33 - Effettuare modifiche al codice push con lo strumento per sviluppatori AEM
* 10:55 - Modifiche al codice di pull con lo strumento per sviluppatori AEM
* 13:12 - Utilizzo degli strumenti di debug integrati di Eclipse

### IDEA IntelliJ

Il **[IDEA IntelliJ](https://www.jetbrains.com/idea/)** è un potente IDE per lo sviluppo Java™ professionale. [!DNL IntelliJ IDEA] si presenta in due gusti, ***gratuito*** [!DNL Community] edizione e uno commerciale (a pagamento) [!DNL Ultimate] versione. Il libero [!DNL Community] versione di [!DNL IntellIJ IDEA] è sufficiente per un maggiore sviluppo dell’AEM, tuttavia [!DNL Ultimate] [espande il set di funzionalità](https://www.jetbrains.com/idea/download).

#### [!DNL Installation and Setup]

1. Scarica e installa [!DNL IntelliJ IDEA]: [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. Installa [!DNL Repo] (strumento da riga di comando): [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

* 00:00 - Importa progetto Maven
* 05:47 - Creare e distribuire il codice sorgente con Maven
* 08:17 - Effettuare il push delle modifiche con l’archivio
* 14:39 - Variazioni pull con archivio
* 17:25 - Utilizzo degli strumenti di debug integrati di IntelliJ IDEA

### [!DNL Visual Studio Code]

**[Codice di Visual Studio](https://code.visualstudio.com/)** è rapidamente diventato uno strumento preferito per ***sviluppatori front-end*** con supporto JavaScript avanzato, [!DNL Intellisense], e il supporto per il debug del browser. **[!DNL Visual Studio Code]** è open source, gratuito, con molte potenti estensioni. [!DNL Visual Studio Code] può essere configurato per integrarsi con l’AEM con l’aiuto di uno strumento Adobe, **[repository](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code).** Sono inoltre disponibili diverse estensioni supportate dalla community che possono essere installate per l’integrazione con AEM.

[!DNL Visual Studio Code] è un’ottima scelta per gli sviluppatori front-end che scrivono principalmente codice CSS/LESS e JavaScript per creare librerie client AEM. Questo strumento potrebbe non essere la scelta migliore per i nuovi sviluppatori AEM, in quanto le definizioni dei nodi (finestre di dialogo, componenti) devono essere modificate in XML non elaborato. Sono disponibili diverse estensioni Java™ per [!DNL Visual Studio Code], tuttavia, se esegue principalmente lo sviluppo Java™ [!DNL Eclipse IDE] o [!DNL IntelliJ] può essere preferibile.

#### Collegamenti importanti

* [**Scarica**](https://code.visualstudio.com/Download) **Codice di Visual Studio**
* **[repository](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** - Strumento simile a FTP per contenuti JCR
* **[aemfed](https://aemfed.io/)** - Velocizzare il flusso di lavoro front-end dell&#39;AEM
* **[Sincronizzazione AEM](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** - Sostegno comunitario&#42; estensione per Visual Studio Code

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 - Importa progetto Maven
* 00:53 - Creare e distribuire il codice sorgente con Maven
* 04:03 - Effettuare il push delle modifiche al codice con lo strumento della riga di comando Repo
* 08:29 - Modifiche al codice di pull con lo strumento della riga di comando Repo
* 10:40 - Cambiamenti nel codice push con lo strumento aemfed
* 14:24 - Risoluzione dei problemi, Rigenera librerie client

### [!DNL CRXDE Lite]

[CRXDE Liti](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/developing-with-crxde-lite.html) è una visualizzazione dell’archivio AEM basata su browser. [!DNL CRXDE Lite] è incorporato nell’AEM e consente a uno sviluppatore di eseguire attività di sviluppo standard come la modifica di file, la definizione di componenti, finestre di dialogo e modelli. [!DNL CRXDE Lite] è ***non*** deve essere un ambiente di sviluppo completo, ma è efficace come strumento di debug. [!DNL CRXDE Lite] è utile quando si estende o si comprende semplicemente il codice del prodotto al di fuori della base di codice. [!DNL CRXDE Lite] fornisce una visualizzazione efficace dell’archivio e un modo per testare e gestire in modo efficace le autorizzazioni.

[!DNL CRXDE Lite] deve essere utilizzato con altri IDE per testare ed eseguire il debug del codice, ma mai come strumento di sviluppo primario. Offre un supporto limitato per la sintassi, non offre funzionalità di completamento automatico e un&#39;integrazione limitata con i sistemi di gestione del controllo del codice sorgente.

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## Risoluzione dei problemi

***Aiuto!*** Il codice non funziona. Come per tutti gli sviluppi, in alcuni casi (probabilmente in molti casi) il codice non funziona come previsto. L&#39;AEM è una piattaforma potente, ma con grande potenza... diventa molto complessa. Di seguito sono riportati alcuni punti di partenza di alto livello per la risoluzione dei problemi e il tracciamento (ma non un elenco esaustivo di possibili errori):

### Verifica distribuzione del codice

Un buon primo passo, quando si verifica un problema, è quello di verificare che il codice sia stato distribuito e installato correttamente nell&#39;AEM.

1. **Verifica [!UICONTROL Gestione pacchetti]** per garantire che il pacchetto di codice sia stato caricato e installato: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Controllare la marca temporale per verificare che il pacchetto sia stato installato di recente.
1. Se esegui aggiornamenti di file incrementali utilizzando uno strumento come [!DNL Repo] o [!DNL AEM Developer Tools], **spunta[!DNL CRXDE Lite]** che il file è stato inviato all’istanza AEM locale e che il contenuto del file viene aggiornato: [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **Verifica che il bundle sia caricato** se visualizzi problemi relativi al codice Java™ in un bundle OSGi. Apri [!UICONTROL Console Web Adobe Experience Manager]: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) e cerca il bundle. Assicurati che il bundle abbia **[!UICONTROL Attivo]** stato. Per ulteriori informazioni sulla risoluzione dei problemi relativi a un bundle in un **[!UICONTROL Installato]** stato.

#### Controlla i registri

L’AEM è una piattaforma chiacchierata che registra informazioni utili nel **error.log**. Il **error.log** dove è stato installato AEM: &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

Una tecnica utile per tenere traccia dei problemi è aggiungere istruzioni di registro nel codice Java™:

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

Per impostazione predefinita, il **error.log** è configurato per registrare *[!DNL INFO]* istruzioni. Se desideri modificare il livello di registro, puoi farlo andando su [!UICONTROL Supporto registro]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). È inoltre possibile che il **error.log** è troppo chiacchierona. È possibile utilizzare [!UICONTROL Supporto registro] per configurare le istruzioni di registro solo per un pacchetto Java™ specificato. Questa è una best practice per i progetti, per separare facilmente i problemi relativi al codice personalizzato dai problemi OOTB della piattaforma AEM.

![Configurazione della registrazione in AEM](./assets/set-up-a-local-aem-development-environment/logging.png)

#### Il bundle è in stato Installato {#bundle-active}

Tutti i bundle (esclusi i frammenti) devono essere in un **[!UICONTROL Attivo]** stato. Se vedi il pacchetto di codice in un [!UICONTROL Installato] quindi c&#39;è un problema che deve essere risolto. Nella maggior parte dei casi si tratta di un problema di dipendenza:

![Errore del bundle nell’AEM](assets/set-up-a-local-aem-development-environment/bundle-error.png)

Nella schermata precedente, l’ [!DNL WKND Core bundle] è un [!UICONTROL Installato] stato. Questo perché nel bundle è prevista una versione diversa di `com.adobe.cq.wcm.core.components.models` rispetto a quelli disponibili nell’istanza AEM.

Un utile strumento che può essere utilizzato è il [!UICONTROL Finder dipendenze]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). Aggiungi il nome del pacchetto Java™ per verificare la versione disponibile nell’istanza AEM:

![Componenti di base](assets/set-up-a-local-aem-development-environment/core-components.png)

Continuando con l’esempio precedente, possiamo vedere che la versione installata nell’istanza AEM è **12,2** vs **12,6** che il bundle si aspettava. Da lì, puoi lavorare all&#39;indietro e vedere se [!DNL Maven] le dipendenze dall&#39;AEM corrispondano al [!DNL Maven] dipendenze nel progetto AEM. In, l’esempio precedente [!DNL Core Components] **v2.2.0** è installato nell’istanza AEM, ma il bundle di codice è stato creato con una dipendenza su **v2.2.2**, da cui il motivo del problema di dipendenza.

#### Verifica registrazione modelli Sling {#osgi-component-sling-models}

I componenti AEM devono essere supportati da un [!DNL Sling Model] per incapsulare qualsiasi logica di business e garantire che lo script di rendering HTL rimanga pulito. In caso di problemi che impediscono di trovare il modello Sling, può essere utile controllare la [!DNL Sling Models] dalla console: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). Questo indica se il modello Sling è stato registrato e a quale tipo di risorsa (il percorso del componente) è associato.

![Stato modello Sling](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

Mostra la registrazione di un [!DNL Sling Model], `BylineImpl` associata a un tipo di risorsa componente `wknd/components/content/byline`.

#### Problemi CSS o JavaScript

Per la maggior parte dei problemi CSS e JavaScript, l’utilizzo degli strumenti di sviluppo del browser è il modo più efficace per risolvere i problemi. Per limitare il problema durante lo sviluppo per un’istanza dell’autore AEM, è utile visualizzare la pagina &quot;così come è stata pubblicata&quot;.

![Problemi CSS o JS](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

Apri [!UICONTROL Proprietà pagina] e fai clic su [!UICONTROL Visualizza come pubblicato]. Viene aperta la pagina senza l’editor AEM e con un parametro di query impostato su **wcmmode=disabled**. Questo disabilita efficacemente l’interfaccia utente di creazione dell’AEM e semplifica notevolmente la risoluzione dei problemi e il debug front-end.

Un altro problema che si verifica di solito durante lo sviluppo del codice front-end è il caricamento di CSS/JS obsoleto o precedente. Come primo passo, assicurati che la cronologia del browser sia stata cancellata e, se necessario, avvia un browser in incognito o una nuova sessione.

#### Debug delle librerie client

Con i diversi metodi di categorie e incorporamenti per includere più librerie client, la risoluzione dei problemi può risultare laboriosa. L&#39;AEM espone diversi strumenti per aiutarlo. Uno degli strumenti più importanti è [!UICONTROL Rigenera librerie client] che obbligano l’AEM a ricompilare eventuali file LESS e generare il CSS.

* [Dump librerie](http://localhost:4502/libs/granite/ui/content/dumplibs.html) : elenca tutte le librerie client registrate nell’istanza AEM. &lt;host>/libs/granite/ui/content/dumplibs.html
* [Output di prova](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) : consente a un utente di visualizzare l’output HTML previsto di clientlib include in base alla categoria. &lt;host>/libs/granite/ui/content/dumplibs.test.html
* [Convalida dipendenze librerie](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) : evidenzia le dipendenze o le categorie incorporate che non sono state trovate. &lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [Rigenera librerie client](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) : consente a un utente di forzare l’AEM a ricreare tutte le librerie client o a invalidare la cache delle librerie client. Questo strumento è efficace quando si sviluppa con MENO in quanto può costringere l’AEM a ricompilare il CSS generato. In generale, è più efficace annullare la validità delle cache ed eseguire quindi l’aggiornamento di una pagina anziché ricompilare tutte le librerie. &lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![Debug di Clientlibs](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>Se devi invalidare costantemente la cache utilizzando [!UICONTROL Rigenera librerie client] potrebbe valere la pena di eseguire una ricostruzione una tantum di tutte le librerie client. Questa operazione può richiedere circa 15 minuti, ma in genere elimina qualsiasi problema di caching in futuro.
