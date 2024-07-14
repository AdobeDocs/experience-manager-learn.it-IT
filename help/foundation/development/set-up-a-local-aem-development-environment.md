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
duration: 4537
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2413'
ht-degree: 0%

---

# Configurare un ambiente locale di sviluppo AEM

Guida alla configurazione di uno sviluppo locale per Adobe Experience Manager, AEM. Include argomenti importanti sull’installazione locale, Apache Maven, gli ambienti di sviluppo integrati e il debug/risoluzione dei problemi. Vengono discusse le attività di sviluppo con **Eclipse IDE, CRXDE Lite, Visual Studio Code e IntelliJ**.

## Panoramica

La creazione di un ambiente di sviluppo locale rappresenta il primo passo per lo sviluppo per Adobe Experience Manager o AEM. Prenditi il tempo necessario per configurare un ambiente di sviluppo di qualità che aumenti la produttività e scriva codice migliore, più rapidamente. Possiamo suddividere l&#39;ambiente di sviluppo locale dell&#39;AEM in quattro aree:

* Istanze AEM locali
* [!DNL Apache Maven] progetto
* Ambienti di sviluppo integrato (IDE)
* Risoluzione dei problemi

## Installare istanze AEM locali

Quando si fa riferimento a un’istanza AEM locale, si parla di una copia di Adobe Experience Manager in esecuzione sul computer personale di uno sviluppatore. ***Tutti*** gli sviluppatori AEM devono iniziare scrivendo ed eseguendo il codice su un&#39;istanza AEM locale.

Se non si ha familiarità con AEM, è possibile installare due modalità di esecuzione di base: ***Autore*** e ***Publish***. ***Autore*** [modalità di esecuzione](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/configuring/configure-runmodes.html?lang=en) è l&#39;ambiente utilizzato dagli esperti di marketing digitale per creare e gestire i contenuti. Quando si sviluppa la maggior parte del tempo, si distribuisce il codice in un’istanza di authoring. Questo consente di creare pagine e aggiungere e configurare componenti. AEM Sites è un CMS di authoring WYSIWYG e quindi la maggior parte dei CSS e JavaScript può essere testata rispetto a un’istanza di authoring.

È anche il codice di test *critico* per un&#39;istanza locale di ***Publish***. L&#39;istanza ***Publish*** è l&#39;ambiente AEM con cui i visitatori del sito Web interagiscono. Anche se l&#39;istanza ***Publish*** è lo stesso stack tecnologico dell&#39;istanza ***Author***, esistono alcune distinzioni importanti con le configurazioni e le autorizzazioni. Il codice deve essere testato su un&#39;istanza locale di ***Publish*** prima di essere promosso ad ambienti di livello superiore.

### Passaggi

1. Verificare che Java™ sia installato.
   * Preferisci [Java™ JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=tipo di software%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) per AEM 6.5+
   * [Java™ JDK 8](https://www.oracle.com/java/technologies/downloads/) per le versioni AEM precedenti a AEM 6.5
1. Ottieni una copia del file JAR QuickStart [AEM e di  [!DNL license.properties]](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html?lang=it).
1. Crea nel computer una struttura di cartelle come quella riportata di seguito:

```plain
~/aem-sdk
    /author
    /publish
```

1. Rinomina il file JAR [!DNL QuickStart] in ***aem-author-p4502.jar*** e inseriscilo nella directory `/author`. Aggiungere il file ***[!DNL license.properties]*** sotto la directory `/author`.

1. Creare una copia del file JAR [!DNL QuickStart], rinominarlo in ***aem-publish-p4503.jar*** e posizionarlo sotto la directory `/publish`. Aggiungere una copia del file ***[!DNL license.properties]*** sotto la directory `/publish`.

```plain
~/aem-sdk
    /author
        + aem-author-p4502.jar
        + license.properties
    /publish
        + aem-publish-p4503.jar
        + license.properties
```

1. Fai doppio clic sul file ***aem-author-p4502.jar*** per installare l&#39;istanza **Author**. Verrà avviata l&#39;istanza di authoring, in esecuzione sulla porta **4502** del computer locale.

Fare doppio clic sul file ***aem-publish-p4503.jar*** per installare l&#39;istanza **Publish**. Verrà avviata l&#39;istanza di Publish, in esecuzione sulla porta **4503** del computer locale.

>[!NOTE]
>
>A seconda dell&#39;hardware del computer di sviluppo, potrebbe essere difficile eseguire contemporaneamente un&#39;istanza di **Author e Publish**. Raramente è necessario eseguire entrambe le operazioni contemporaneamente su una configurazione locale.

### Utilizzo della riga di comando

Un&#39;alternativa al doppio clic sul file JAR consiste nell&#39;avviare AEM dalla riga di comando o creare uno script (`.bat` o `.sh`) a seconda della configurazione del sistema operativo locale. Di seguito è riportato un esempio del comando di esempio:

```shell
$ java -Xmx2048M -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=30303 -jar aem-author-p4502.jar -gui -r"author,localdev"
```

In questo caso, `-X` sono opzioni JVM e `-D` sono proprietà di framework aggiuntive. Per ulteriori informazioni, vedere [Distribuzione e manutenzione di un&#39;istanza AEM](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html?lang=it) e [Altre opzioni disponibili nel file Quickstart](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/custom-standalone-install.html#further-options-available-from-the-quickstart-file).

## Installare Apache Maven

***[!DNL Apache Maven]*** è uno strumento per gestire la procedura di compilazione e distribuzione per i progetti basati su Java. AEM è una piattaforma basata su Java e [!DNL Maven] è il metodo standard per gestire il codice per un progetto AEM. Quando si parla di ***progetto Maven per AEM*** o solo del ***progetto AEM***, si fa riferimento a un progetto Maven che include tutto il codice *personalizzato* per il sito.

Tutti i progetti AEM devono essere basati sulla versione più recente di **[!DNL AEM Project Archetype]**: [https://github.com/adobe/aem-project-archetype](https://github.com/adobe/aem-project-archetype). [!DNL AEM Project Archetype] fornisce un bootstrap di un progetto AEM con codice e contenuto di esempio. [!DNL AEM Project Archetype] include anche **[!DNL AEM WCM Core Components]** configurato per essere utilizzato nel progetto.

>[!CAUTION]
>
>All’avvio di un nuovo progetto, è consigliabile utilizzare la versione più recente dell’archetipo. Tieni presente che esistono più versioni dell’archetipo e che non tutte le versioni sono compatibili con le versioni precedenti di AEM.

### Passaggi

1. Scarica [Apache Maven](https://maven.apache.org/download.cgi)
2. Installa [Apache Maven](https://maven.apache.org/install.html) e accertati che l&#39;installazione sia stata aggiunta alla riga di comando `PATH`.
   * [!DNL macOS] utenti possono installare Maven utilizzando [Homebrew](https://brew.sh/)
3. Verificare che **[!DNL Maven]** sia installato aprendo un nuovo terminale della riga di comando ed eseguendo le operazioni seguenti:

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
> In, in passato era necessario aggiungere il profilo Maven `adobe-public` per puntare a `nexus.adobe.com` per scaricare gli artefatti AEM. Tutti gli artefatti AEM sono ora disponibili tramite Maven Central e il profilo `adobe-public` non è necessario.

## Configurare un ambiente di sviluppo integrato

Un ambiente di sviluppo integrato o IDE è un’applicazione che combina un editor di testo, il supporto della sintassi e strumenti di creazione. A seconda del tipo di sviluppo che si sta effettuando, un IDE potrebbe essere preferibile rispetto ad un altro. Indipendentemente dall&#39;IDE, è importante essere in grado di ***inviare periodicamente il codice*** a un&#39;istanza AEM locale per testarlo. Occasionalmente è importante ***richiamare*** configurazioni da un&#39;istanza AEM locale nel progetto AEM per mantenere un sistema di gestione del controllo del codice sorgente come Git.

Di seguito sono riportati alcuni degli IDE più popolari utilizzati con lo sviluppo dell’AEM e i relativi video che mostrano l’integrazione con un’istanza dell’AEM locale.

>[!NOTE]
>
> Il progetto WKND è stato aggiornato per impostazione predefinita e funziona su AEM as a Cloud Service. È stato aggiornato per renderlo [compatibile con le versioni precedenti di 6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx). Se utilizzi AEM 6.5 o 6.4, aggiungi il profilo `classic` a qualsiasi comando Maven.

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Quando, utilizzando un IDE, assicurati di controllare `classic` nella scheda Profilo Maven.

![Scheda Profilo Maven](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*Profilo Maven IntelliJ*

### IDE [!DNL Eclipse]

**[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** è uno degli IDE più popolari per lo sviluppo Java™, in gran parte perché è open source e ***libero***! L&#39;Adobe fornisce un plug-in, **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)**, per [!DNL Eclipse] per consentire uno sviluppo più semplice con un&#39;interfaccia grafica per sincronizzare il codice con un&#39;istanza AEM locale. L&#39;IDE [!DNL Eclipse] è consigliato per sviluppatori che non hanno mai utilizzato AEM in gran parte a causa del supporto GUI di [!DNL AEM Developer Tools].

#### Installazione e configurazione

1. Scarica e installa l&#39;IDE [!DNL Eclipse] per [!DNL Java™ EE Developers]: [https://www.eclipse.org](https://www.eclipse.org/)
1. Segui le istruzioni per installare il plug-in [!DNL AEM Developer Tools]: [https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 - Importa progetto Maven
* 01:24 - Creare e distribuire il codice sorgente con Maven
* 04:33 - Effettuare modifiche al codice push con lo strumento per sviluppatori AEM
* 10:55 - Modifiche al codice di pull con lo strumento per sviluppatori AEM
* 13:12 - Utilizzo degli strumenti di debug integrati di Eclipse

### IDEA IntelliJ

**[IntelliJ IDEA](https://www.jetbrains.com/idea/)** è un potente IDE per lo sviluppo Java™ professionale. [!DNL IntelliJ IDEA] è disponibile in due versioni: ***gratuita*** [!DNL Community] e [!DNL Ultimate] commerciale (a pagamento). La versione gratuita di [!DNL Community] di [!DNL IntellIJ IDEA] è sufficiente per un ulteriore sviluppo AEM, tuttavia [!DNL Ultimate] [espande il set di funzionalità](https://www.jetbrains.com/idea/download).

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

**[Visual Studio Code](https://code.visualstudio.com/)** è diventato rapidamente uno strumento preferito per ***sviluppatori front-end*** con supporto avanzato per JavaScript, [!DNL Intellisense] e supporto per il debug del browser. **[!DNL Visual Studio Code]** è open source, gratuito, con molte potenti estensioni. [!DNL Visual Studio Code] può essere configurato per l&#39;integrazione con AEM con l&#39;aiuto di uno strumento Adobe, **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code).** È inoltre possibile installare diverse estensioni supportate dalla community per l&#39;integrazione con AEM.

[!DNL Visual Studio Code] è un&#39;ottima scelta per gli sviluppatori front-end che scrivono principalmente codice CSS/LESS e codice JavaScript per creare librerie client AEM. Questo strumento potrebbe non essere la scelta migliore per i nuovi sviluppatori AEM, in quanto le definizioni dei nodi (finestre di dialogo, componenti) devono essere modificate in XML non elaborato. Sono disponibili diverse estensioni Java™ per [!DNL Visual Studio Code], tuttavia se si esegue principalmente lo sviluppo Java™ [!DNL Eclipse IDE] o [!DNL IntelliJ] potrebbe essere preferibile.

#### Collegamenti importanti

* [**Scarica**](https://code.visualstudio.com/Download) **Codice Visual Studio**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** - Strumento simile a FTP per il contenuto JCR
* **[aemfed](https://aemfed.io/)** - Velocizza il flusso di lavoro front-end AEM
* **[Sincronizzazione AEM](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** - Estensione &#42; supportata dalla community per Visual Studio Code

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 - Importa progetto Maven
* 00:53 - Creare e distribuire il codice sorgente con Maven
* 04:03 - Effettuare il push delle modifiche al codice con lo strumento della riga di comando Repo
* 08:29 - Modifiche al codice di pull con lo strumento della riga di comando Repo
* 10:40 - Cambiamenti nel codice push con lo strumento aemfed
* 14:24 - Risoluzione dei problemi, Rigenera librerie client

### [!DNL CRXDE Lite]

[CRXDE Liti](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/developing-with-crxde-lite.html) è una visualizzazione dell&#39;archivio AEM basata su browser. [!DNL CRXDE Lite] è incorporato in AEM e consente a uno sviluppatore di eseguire attività di sviluppo standard quali la modifica di file, la definizione di componenti, finestre di dialogo e modelli. [!DNL CRXDE Lite] è ***not*** destinato a essere un ambiente di sviluppo completo, ma è efficace come strumento di debug. [!DNL CRXDE Lite] è utile quando si estende o si comprende semplicemente il codice del prodotto al di fuori della base di codice. [!DNL CRXDE Lite] fornisce una visualizzazione efficace dell&#39;archivio e un modo per testare e gestire in modo efficace le autorizzazioni.

[!DNL CRXDE Lite] deve essere utilizzato con altri IDE per testare ed eseguire il debug del codice, ma mai come strumento di sviluppo primario. Offre un supporto limitato per la sintassi, non offre funzionalità di completamento automatico e un&#39;integrazione limitata con i sistemi di gestione del controllo del codice sorgente.

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## Risoluzione dei problemi

***Guida*** Il mio codice non funziona. Come per tutti gli sviluppi, in alcuni casi (probabilmente in molti casi) il codice non funziona come previsto. L&#39;AEM è una piattaforma potente, ma con grande potenza... diventa molto complessa. Di seguito sono riportati alcuni punti di partenza di alto livello per la risoluzione dei problemi e il tracciamento (ma non un elenco esaustivo di possibili errori):

### Verifica distribuzione del codice

Un buon primo passo, quando si verifica un problema, è quello di verificare che il codice sia stato distribuito e installato correttamente nell&#39;AEM.

1. **Controllare [!UICONTROL Gestione pacchetti]** per verificare che il pacchetto di codice sia stato caricato e installato: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Controllare la marca temporale per verificare che il pacchetto sia stato installato di recente.
1. Se si eseguono aggiornamenti incrementali dei file utilizzando uno strumento come [!DNL Repo] o [!DNL AEM Developer Tools], **verificare[!DNL CRXDE Lite]** che il file sia stato inviato all&#39;istanza AEM locale e che il contenuto del file sia aggiornato: [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **Verifica che il bundle sia stato caricato** in caso di problemi relativi al codice Java™ in un bundle OSGi. Apri [!UICONTROL Console Web Adobe Experience Manager]: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) e cerca il bundle. Assicurati che il bundle abbia lo stato **[!UICONTROL Attivo]**. Per ulteriori informazioni sulla risoluzione dei problemi relativi a un bundle in stato **[!UICONTROL Installato]**, vedi di seguito.

#### Controlla i registri

AEM è una piattaforma chiacchierata e registra informazioni utili nel **error.log**. È possibile trovare il **error.log** in cui è stato installato AEM: &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

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

Per impostazione predefinita, **error.log** è configurato per registrare *[!DNL INFO]* istruzioni. Se si desidera modificare il livello di log, è possibile passare a [!UICONTROL Supporto log]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). È inoltre possibile che **error.log** sia troppo veloce. È possibile utilizzare il [!UICONTROL Supporto log] per configurare le istruzioni di log solo per un pacchetto Java™ specificato. Questa è una best practice per i progetti, per separare facilmente i problemi relativi al codice personalizzato dai problemi OOTB della piattaforma AEM.

![Configurazione di registrazione in AEM](./assets/set-up-a-local-aem-development-environment/logging.png)

#### Il bundle è in stato Installato {#bundle-active}

Tutti i bundle (esclusi i frammenti) devono essere in stato **[!UICONTROL Attivo]**. Se il bundle di codice è nello stato [!UICONTROL Installato], è necessario risolvere un problema. Nella maggior parte dei casi si tratta di un problema di dipendenza:

![Errore bundle in AEM](assets/set-up-a-local-aem-development-environment/bundle-error.png)

Nella schermata precedente, [!DNL WKND Core bundle] è uno stato [!UICONTROL Installato]. Il bundle prevede una versione di `com.adobe.cq.wcm.core.components.models` diversa da quella disponibile nell&#39;istanza AEM.

Uno strumento utile che può essere utilizzato è [!UICONTROL Trova dipendenze]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). Aggiungi il nome del pacchetto Java™ per verificare la versione disponibile nell’istanza AEM:

![Componenti di base](assets/set-up-a-local-aem-development-environment/core-components.png)

Continuando con l’esempio precedente, possiamo vedere che la versione installata nell’istanza AEM è **12.2** rispetto a **12.6** prevista dal bundle. Da qui, puoi lavorare all&#39;indietro e vedere se le dipendenze [!DNL Maven] da AEM corrispondono alle dipendenze [!DNL Maven] nel progetto AEM. In, l&#39;esempio precedente [!DNL Core Components] **v2.2.0** è installato nell&#39;istanza AEM, ma il bundle di codice è stato creato con una dipendenza da **v2.2.2**, da cui il motivo del problema di dipendenza.

#### Verifica registrazione modelli Sling {#osgi-component-sling-models}

I componenti AEM devono essere supportati da [!DNL Sling Model] per incapsulare qualsiasi logica di business e garantire che lo script di rendering HTL rimanga pulito. In caso di problemi in cui non è possibile trovare il modello Sling, controllare [!DNL Sling Models] dalla console: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). Questo indica se il modello Sling è stato registrato e a quale tipo di risorsa (il percorso del componente) è associato.

![Stato modello Sling](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

Mostra la registrazione di un [!DNL Sling Model], `BylineImpl` associato a un tipo di risorsa componente `wknd/components/content/byline`.

#### Problemi CSS o JavaScript

Per la maggior parte dei problemi relativi a CSS e JavaScript, l’utilizzo degli strumenti di sviluppo del browser è il modo più efficace per risolvere i problemi. Per limitare il problema durante lo sviluppo per un’istanza dell’autore AEM, è utile visualizzare la pagina &quot;così come è stata pubblicata&quot;.

![Problemi CSS o JS](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

Apri il menu [!UICONTROL Proprietà pagina] e fai clic su [!UICONTROL Visualizza come pubblicato]. Verrà aperta la pagina senza l&#39;editor AEM e con un parametro di query impostato su **wcmmode=disabled**. Questo disabilita efficacemente l’interfaccia utente di creazione dell’AEM e semplifica notevolmente la risoluzione dei problemi e il debug front-end.

Un altro problema che si verifica di solito durante lo sviluppo del codice front-end è il caricamento di CSS/JS obsoleto o precedente. Come primo passo, assicurati che la cronologia del browser sia stata cancellata e, se necessario, avvia un browser in incognito o una nuova sessione.

#### Debug delle librerie client

Con i diversi metodi di categorie e incorporamenti per includere più librerie client, la risoluzione dei problemi può risultare laboriosa. L&#39;AEM espone diversi strumenti per aiutarlo. Uno degli strumenti più importanti è [!UICONTROL Ricostruisci librerie client] che obbliga AEM a ricompilare i file LESS e generare CSS.

* [Dump Libs](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - Elenca tutte le librerie client registrate nell&#39;istanza AEM. &lt;host>/libs/granite/ui/content/dumplibs.html
* [Output di test](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - consente a un utente di visualizzare l&#39;output HTML previsto di clientlib include in base alla categoria. &lt;host>/libs/granite/ui/content/dumplibs.test.html
* [Convalida dipendenze librerie](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - evidenzia le dipendenze o le categorie incorporate che non è possibile trovare. &lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [Rigenera librerie client](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html): consente a un utente di forzare AEM a ricompilare tutte le librerie client o di invalidare la cache delle librerie client. Questo strumento è efficace quando si sviluppa con MENO in quanto può costringere l’AEM a ricompilare il CSS generato. In generale, è più efficace annullare la validità delle cache ed eseguire quindi l’aggiornamento di una pagina anziché ricompilare tutte le librerie. &lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![Debug di Clientlibs](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>Se devi annullare costantemente la validità della cache utilizzando lo strumento [!UICONTROL Ricostruisci librerie client], potrebbe essere utile eseguire una ricompilazione una tantum di tutte le librerie client. Questa operazione può richiedere circa 15 minuti, ma in genere elimina qualsiasi problema di caching in futuro.
