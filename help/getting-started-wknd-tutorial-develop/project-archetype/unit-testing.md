---
title: Test di unità
description: Implementa uno unit test che convalida il comportamento del modello Sling del componente Byline, creato nell’esercitazione del componente personalizzato.
version: 6.5, Cloud Service
feature: APIs, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4089
mini-toc-levels: 1
thumbnail: 30207.jpg
doc-type: Tutorial
exl-id: b926c35e-64ad-4507-8b39-4eb97a67edda
recommendations: noDisplay, noCatalog
duration: 706
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2923'
ht-degree: 0%

---

# Test di unità {#unit-testing}

Questa esercitazione descrive l&#39;implementazione di uno unit test che convalida il comportamento del modello Sling del componente Byline, creato in [Componente personalizzato](./custom-component.md) esercitazione.

## Prerequisiti {#prerequisites}

Esaminare gli strumenti e le istruzioni necessari per l&#39;impostazione di un [ambiente di sviluppo locale](overview.md#local-dev-environment).

_Se nel sistema sono installati sia Java™ 8 che Java™ 11, l’esecuzione dei test di VS Code potrebbe scegliere il runtime Java™ inferiore durante l’esecuzione dei test, con conseguente errore dei test. In tal caso, disinstallare Java™ 8._

### Progetto iniziale

>[!NOTE]
>
> Se hai completato correttamente il capitolo precedente, puoi riutilizzare il progetto e saltare i passaggi per estrarre il progetto iniziale.

Consulta il codice della riga di base su cui si basa l’esercitazione:

1. Consulta la sezione `tutorial/unit-testing-start` ramo da [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/unit-testing-start
   ```

1. Implementa la base di codice in un’istanza AEM locale utilizzando le tue competenze Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se si utilizza AEM 6.5 o 6.4, aggiungere `classic` profilo a qualsiasi comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start) oppure estrarre il codice localmente passando al ramo `tutorial/unit-testing-start`.

## Obiettivo

1. Comprendere le nozioni di base sul testing di unità.
1. Scopri i framework e gli strumenti comunemente utilizzati per testare il codice AEM.
1. Comprendi le opzioni per deridere o simulare le risorse AEM durante la scrittura degli unit test.

## Informazioni di base {#unit-testing-background}

In questa esercitazione verrà illustrato come scrivere [Test di unità](https://en.wikipedia.org/wiki/Unit_testing) per il componente Byline [Modello Sling](https://sling.apache.org/documentation/bundles/models.html) (creato in [Creazione di un componente AEM personalizzato](custom-component.md)). Gli unit test sono test di build-time scritti in Java™ che verificano il comportamento previsto del codice Java™. Ogni unit test è in genere piccolo e convalida l&#39;output di un metodo (o unità di lavoro) in base ai risultati previsti.

Utilizziamo le best practice per l’AEM e utilizziamo:

* [JUnit 5](https://junit.org/junit5/)
* [Framework di test di Mockito](https://site.mockito.org/)
* [Framework di test wcm.io](https://wcm.io/testing/) (basato su [Apache Sling Mocks](https://sling.apache.org/documentation/development/sling-mock.html))

## Test di unità e Adobe Cloud Manager {#unit-testing-and-adobe-cloud-manager}

[Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html?lang=it) integra l’esecuzione di unit test e [reportistica sulla copertura del codice](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-quality-testing.html) nella sua pipeline CI/CD per incoraggiare e promuovere la best practice per il testing di unità del codice AEM.

Anche se il codice di unit test è una buona pratica per qualsiasi base di codice, quando si utilizza Cloud Manager è importante sfruttare le sue funzioni di test e reporting della qualità del codice fornendo unit test da eseguire in Cloud Manager.

## Aggiornare le dipendenze Maven del test {#inspect-the-test-maven-dependencies}

Il primo passaggio consiste nell’esaminare le dipendenze Maven per supportare la scrittura e l’esecuzione dei test. Sono necessarie quattro dipendenze:

1. JUnit5
1. Framework di prova Mockito
1. Apache Sling Mocks
1. Framework di test AEM-Mocks (di io.wcm)

Il **JUnit5**, **Mockito e **Attenti al AEM** le dipendenze dei test vengono aggiunte automaticamente al progetto durante la configurazione utilizzando [Archetipo Maven AEM](project-setup.md).

1. Per visualizzare queste dipendenze, aprire il POM Reactor padre in **aem-guides-wknd/pom.xml**, passare alla `<dependencies>..</dependencies>` e visualizzare le dipendenze per JUnit, Mockito, Apache Sling Mocks e AEM Mock Test di io.wcm in `<!-- Testing -->`.
1. Assicurati che `io.wcm.testing.aem-mock.junit5` è impostato su **4.1.0.**:

   ```xml
   <dependency>
       <groupId>io.wcm</groupId>
       <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
       <version>4.1.0</version>
       <scope>test</scope>
   </dependency>
   ```

   >[!CAUTION]
   >
   > Archetipo **35** genera il progetto con `io.wcm.testing.aem-mock.junit5` version **4.1.8.**. Effettua il downgrade a **4.1.0.** per seguire il resto di questo capitolo.

1. Apri **aem-guides-wknd/core/pom.xml** e verificare che siano disponibili le dipendenze di test corrispondenti.

   Una cartella di origine parallela nel **core** Il progetto conterrà gli unit test ed eventuali file di test di supporto. Questo **test** La cartella fornisce la separazione delle classi di test dal codice sorgente, ma consente ai test di agire come se si trovassero negli stessi pacchetti del codice sorgente.

## Creazione del test JUnit {#creating-the-junit-test}

Gli unit test tipicamente mappano 1-a-1 con le classi Java™. In questo capitolo verrà scritto un test JUnit per **BylineImpl.java**, che è il modello Sling che supporta il componente Byline.

![Cartella src unit test](assets/unit-testing/core-src-test-folder.png)

*Percorso in cui sono archiviati gli unit test.*

1. Creare uno unit test per `BylineImpl.java` creando una nuova classe Java™ in `src/test/java` in una struttura di cartelle di pacchetti Java™ che rispecchia la posizione della classe Java™ da testare.

   ![Creare un nuovo file BylineImplTest.java](assets/unit-testing/new-bylineimpltest.png)

   Poiché stiamo testando

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   crea una classe Java™ unit test corrispondente in

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   Il `Test` suffisso sul file di unit test, `BylineImplTest.java` è una convenzione, che ci permette di

   1. Identificarlo facilmente come file di test _per_ `BylineImpl.java`
   1. Ma differenzia anche il file di test _da_ la classe oggetto della prova, `BylineImpl.java`

## Revisione di BylineImplTest.java {#reviewing-bylineimpltest-java}

A questo punto, il file di test JUnit è una classe Java™ vuota.

1. Aggiorna il file con il seguente codice:

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import static org.junit.jupiter.api.Assertions.*;
   
   import org.junit.jupiter.api.BeforeEach;
   import org.junit.jupiter.api.Test;
   
   public class BylineImplTest {
   
       @BeforeEach
       void setUp() throws Exception {
   
       }
   
       @Test 
       void testGetName() { 
           fail("Not yet implemented");
       }
   
       @Test 
       void testGetOccupations() { 
           fail("Not yet implemented");
       }
   
       @Test 
       void testIsEmpty() { 
           fail("Not yet implemented");
       }
   }
   ```

1. Il primo metodo `public void setUp() { .. }` è annotato con JUnit `@BeforeEach`, che indica all&#39;esecuzione del test JUnit di eseguire questo metodo prima di eseguire ogni metodo di test in questa classe. In questo modo è possibile inizializzare lo stato di test comune richiesto da tutti i test.

1. I metodi successivi sono i metodi di prova, i cui nomi sono preceduti dal prefisso `test` per convenzione e contrassegnato con il simbolo `@Test` annotazione. Per impostazione predefinita, tutti i nostri test sono impostati per non riuscire, poiché non li abbiamo ancora implementati.

   Per iniziare, iniziamo con un singolo metodo di test per ogni metodo pubblico sulla classe che stiamo testando, quindi:

   | BylineImpl.java |              | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | è testato da | testGetName() |
   | getOccupations() | è testato da | testGetOccupations() |
   | isEmpty() | è testato da | testIsEmpty() |

   Questi metodi possono essere ampliati in base alle esigenze, come vedremo più avanti in questo capitolo.

   Quando viene eseguita questa classe di test JUnit (nota anche come JUnit Test Case), ogni metodo contrassegnato con il `@Test` verrà eseguito come test che può avere esito positivo o negativo.

![generato BylineImplTest](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. Esegui il test case JUnit facendo clic con il pulsante destro del mouse sulla `BylineImplTest.java` file e tocco **Esegui**.
Come previsto, tutti i test hanno esito negativo poiché non sono ancora stati implementati.

   ![Esegui come test junit](assets/unit-testing/run-junit-tests.png)

   *Fai clic con il pulsante destro del mouse su BylineImplTests.java > Esegui*

## Revisione di BylineImpl.java {#reviewing-bylineimpl-java}

Durante la scrittura degli unit test, esistono due approcci principali:

* [TDD o sviluppo basato su test](https://en.wikipedia.org/wiki/Test-driven_development), che prevede la scrittura degli unit test in modo incrementale, immediatamente prima che l’implementazione venga sviluppata; scrivi un test, scrivi l’implementazione per far sì che il test venga superato.
* Implementazione-prima Sviluppo, che comporta lo sviluppo del codice di lavoro prima e la scrittura di test che convalidano tale codice.

In questa esercitazione, viene utilizzato quest’ultimo approccio (in quanto è già stato creato un **BylineImpl.java** in un capitolo precedente). Per questo motivo, dobbiamo rivedere e comprendere i comportamenti dei suoi metodi pubblici, ma anche alcuni dei suoi dettagli di implementazione. Ciò può sembrare contrario, poiché un buon test dovrebbe preoccuparsi solo degli input e degli output, tuttavia quando si lavora in AEM, vi sono varie considerazioni di implementazione che devono essere comprese per costruire test di funzionamento.

Il TDD nel contesto dell&#39;AEM richiede un livello di competenza ed è meglio se adottato da sviluppatori dell&#39;AEM esperti nello sviluppo dell&#39;AEM e nel test di unità del codice AEM.

## Impostazione del contesto dei test AEM  {#setting-up-aem-test-context}

La maggior parte del codice scritto per l’AEM si basa sulle API JCR, Sling o AEM, che a loro volta richiedono il contesto di un AEM in esecuzione per essere eseguito correttamente.

Poiché gli unit test vengono eseguiti al momento della compilazione, al di fuori del contesto di un’istanza AEM in esecuzione, tale contesto non esiste. Per facilitare questa fase, [L&#39;AEM di wcm.io prende in giro](https://wcm.io/testing/aem-mock/usage.html) crea un contesto fittizio che consente a queste API di: _principalmente_ comportarsi come se si trovassero in una situazione AEM.

1. Creare un contesto AEM utilizzando **wcm.io** `AemContext` in **BylineImplTest.java** aggiungendolo come estensione JUnit decorata con `@ExtendWith` al **BylineImplTest.java** file. L&#39;estensione si occupa di tutte le attività di inizializzazione e pulizia necessarie. Creare una variabile di classe per `AemContext` che possono essere utilizzati per tutti i metodi di prova.

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   Questa variabile, `ctx`, espone un contesto AEM fittizio che fornisce alcune astrazioni di AEM e Sling:

   * Il modello Sling BylineImpl è registrato in questo contesto
   * In questo contesto vengono create strutture di contenuto JCR fittizie
   * I servizi OSGi personalizzati possono essere registrati in questo contesto
   * Fornisce vari oggetti fittizi e helper comuni richiesti, come oggetti SlingHttpServletRequest, vari servizi fittizi Sling e AEM OSGi come ModelFactory, PageManager, Page, Template, ComponentManager, Component, TagManager, Tag, ecc.
      * *Non tutti i metodi per questi oggetti sono implementati.*
   * E [molto di più](https://wcm.io/testing/aem-mock/usage.html)!

   Il **`ctx`** L&#39;oggetto fungerà da punto di ingresso per la maggior parte del contesto fittizio.

1. In `setUp(..)` , che viene eseguito prima di ogni `@Test` , definire uno stato di test fittizio comune:

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** registra il modello Sling da testare nel finto contesto AEM, in modo che possa essere istanziato nel `@Test` metodi.
   * **`load().json`** carica le strutture di risorse nel contesto fittizio, consentendo al codice di interagire con tali risorse come se fossero fornite da un archivio reale. Definizioni delle risorse nel file **`BylineImplTest.json`** vengono caricati nel contesto JCR fittizio in **/content**.
   * **`BylineImplTest.json`** non esiste ancora, quindi creiamolo e definiamo le strutture di risorse JCR necessarie per il test.

1. I file JSON che rappresentano le strutture di risorse fittizie sono memorizzati in **core/src/test/resources** segue lo stesso percorso del pacchetto del file di test Java™ JUnit.

   Creare un file JSON in `core/test/resources/com/adobe/aem/guides/wknd/core/models/impl` denominato **BylineImplTest.json** con il seguente contenuto:

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   Questo JSON definisce una risorsa fittizia (nodo JCR) per l’unit test del componente Byline. A questo punto, il JSON ha il set minimo di proprietà necessarie per rappresentare una risorsa di contenuto del componente Byline, la `jcr:primaryType` e `sling:resourceType`.

   Una regola generale quando si lavora con gli unit test consiste nel creare il set minimo di contenuti fittizi, contesto e codice necessario per soddisfare ogni test. Evita la tentazione di creare un contesto fittizio completo prima di scrivere i test, in quanto spesso genera artefatti non necessari.

   Ora con l&#39;esistenza di **BylineImplTest.json**, quando `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")` viene eseguito, le definizioni delle risorse fittizie vengono caricate nel contesto nel percorso **/content.**

## Verifica di getName() {#testing-get-name}

Ora che abbiamo una configurazione di base di contesto fittizio, scriviamo il nostro primo test per **BylineImpl getName()**. Questa prova deve garantire che il metodo **getName()** restituisce il nome creato corretto memorizzato nel percorso della risorsa &quot;**name&quot;** proprietà.

1. Aggiornare il **testGetName**() metodo in **BylineImplTest.java** come segue:

   ```java
   import com.adobe.aem.guides.wknd.core.models.Byline;
   ...
   @Test
   public void testGetName() {
       final String expected = "Jane Doe";
   
       ctx.currentResource("/content/byline");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       String actual = byline.getName();
   
       assertEquals(expected, actual);
   }
   ```

   * **`String expected`** imposta il valore previsto. Imposta su &quot;**Jane Done**&quot;.
   * **`ctx.currentResource`** imposta il contesto della risorsa fittizia su cui valutare il codice, in modo che venga impostato su **/content/byline** in quanto è dove viene caricata la risorsa di contenuto fittizia.
   * **`Byline byline`** crea un’istanza del modello Sling Byline adattandolo dal finto oggetto Request.
   * **`String actual`** richiama il metodo che stiamo testando, `getName()`, sull&#39;oggetto Byline Sling Model.
   * **`assertEquals`** asserisce che il valore previsto corrisponde al valore restituito dall’oggetto modello Sling byline. Se questi valori non sono uguali, il test avrà esito negativo.

1. Esegui il test... e genera un errore con un `NullPointerException`.

   Questo test NON ha esito negativo perché non è mai stata definita una `name` proprietà nel JSON fittizio, che causerà il fallimento del test, tuttavia l’esecuzione del test non è arrivata a quel punto! Il test non riesce a causa di un `NullPointerException` sull&#39;oggetto byline stesso.

1. In `BylineImpl.java`, se `@PostConstruct init()` genera un’eccezione che impedisce al modello Sling di creare un’istanza e causa l’null dell’oggetto modello Sling.

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   Risulta che mentre il servizio OSGi di ModelFactory è fornito tramite `AemContext` (tramite il contesto Sling di Apache), non tutti i metodi sono implementati, tra cui `getModelFromWrappedRequest(...)` chiamato nel file BylineImpl `init()` metodo. Questo determina un [AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html), che a termine causa `init()` e il conseguente adeguamento del `ctx.request().adaptTo(Byline.class)` è un oggetto null.

   Poiché le beffe fornite non possono contenere il nostro codice, è necessario implementare il contesto fittizio direttamente. Per questo motivo, è possibile utilizzare Mockito per creare un oggetto ModelFactory fittizio, che restituisce un oggetto Image fittizio quando `getModelFromWrappedRequest(...)` viene richiamato su di esso.

   Poiché per creare un’istanza del modello Byline Sling, è necessario che sia presente questo contesto fittizio, possiamo aggiungerlo al `@Before setUp()` metodo. È inoltre necessario aggiungere `MockitoExtension.class` al `@ExtendWith` annotazione sopra **BylineImplTest** classe.

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import org.mockito.junit.jupiter.MockitoExtension;
   import org.mockito.Mock;
   
   import com.adobe.aem.guides.wknd.core.models.Byline;
   import com.adobe.cq.wcm.core.components.models.Image;
   
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   
   import org.apache.sling.models.factory.ModelFactory;
   import org.junit.jupiter.api.BeforeEach;
   import org.junit.jupiter.api.Test;
   import org.junit.jupiter.api.extension.ExtendWith;
   
   import static org.junit.jupiter.api.Assertions.*;
   import static org.mockito.Mockito.*;
   import org.apache.sling.api.resource.Resource;
   
   @ExtendWith({ AemContextExtension.class, MockitoExtension.class })
   public class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   
       @Mock
       private Image image;
   
       @Mock
       private ModelFactory modelFactory;
   
       @BeforeEach
       public void setUp() throws Exception {
           ctx.addModelsForClasses(BylineImpl.class);
   
           ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   
           lenient().when(modelFactory.getModelFromWrappedRequest(eq(ctx.request()), any(Resource.class), eq(Image.class)))
                   .thenReturn(image);
   
           ctx.registerService(ModelFactory.class, modelFactory, org.osgi.framework.Constants.SERVICE_RANKING,
                   Integer.MAX_VALUE);
       }
   
       @Test
       void testGetName() { ...
   }
   ```

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** contrassegna la classe Test Case da eseguire con [Estensione Mockito JUnit Jupiter](https://www.javadoc.io/static/org.mockito/mockito-junit-jupiter/4.11.0/org/mockito/junit/jupiter/MockitoExtension.html) che consente di utilizzare le annotazioni @Mock per definire oggetti fittizi a livello di classe.
   * **`@Mock private Image`** crea un oggetto fittizio di tipo `com.adobe.cq.wcm.core.components.models.Image`. Questo viene definito a livello di classe in modo che, se necessario, `@Test` i metodi possono modificarne il comportamento in base alle esigenze.
   * **`@Mock private ModelFactory`** crea un oggetto fittizio di tipo ModelFactory. Questa è una pura beffa di Mockito e non ha metodi implementati su di essa. Questo viene definito a livello di classe in modo che, se necessario, `@Test`i metodi possono modificarne il comportamento in base alle esigenze.
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** registra il comportamento fittizio per quando `getModelFromWrappedRequest(..)` viene chiamato sull&#39;oggetto ModelFactory fittizio. Il risultato definito in `thenReturn (..)` restituisce il finto oggetto Image. Questo comportamento viene richiamato solo quando: il primo parametro è uguale al `ctx`dell’oggetto della richiesta di, il secondo parametro è qualsiasi oggetto Resource e il terzo parametro deve essere la classe Immagine dei Componenti core. Accettiamo qualsiasi risorsa perché durante i nostri test stiamo impostando il `ctx.currentResource(...)` alle varie risorse fittizie definite nella **BylineImplTest.json**. Tieni presente che aggiungiamo **lenient()** rigore perché in seguito si desidera ignorare questo comportamento di ModelFactory.
   * **`ctx.registerService(..)`.** registra l&#39;oggetto ModelFactory fittizio in AemContext, con la classificazione di servizio più elevata. Questo è necessario in quanto ModelFactory utilizzato in BylineImpl `init()` viene iniettato attraverso il `@OSGiService ModelFactory model` campo. Per inserire in AemContext **nostro** oggetto fittizio, che gestisce le chiamate a `getModelFromWrappedRequest(..)`, è necessario registrarlo come servizio di livello più alto di quel tipo (ModelFactory).

1. Riesegui il test e di nuovo non riesce, ma questa volta il messaggio indica chiaramente il motivo dell’errore.

   ![asserzione di errore del nome del test](assets/unit-testing/testgetname-failure-assertion.png)

   *errore testGetName() a causa di un’asserzione*

   Riceviamo un **AssertionError** il che significa che la condizione di asserzione nel test non è riuscita e ci dice che **il valore previsto è &quot;Jane Doe&quot;** ma il **il valore effettivo è nullo**. Questo ha senso perché &quot;**name&quot;** la proprietà non è stata aggiunta al modello **/content/byline** definizione di risorsa in **BylineImplTest.json**, quindi aggiungiamolo:

1. Aggiorna **BylineImplTest.json** per definire `"name": "Jane Doe".`

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

1. eseguire nuovamente il test e **`testGetName()`** ora passa!

   ![test name pass](assets/unit-testing/testgetname-pass.png)


## Verifica di getOccupations() {#testing-get-occupations}

Ok, perfetto! Il primo test è stato superato! Andiamo avanti e testiamo `getOccupations()`. Poiché l&#39;inizializzazione del contesto fittizio è stata eseguita in `@Before setUp()`, è disponibile per tutti `@Test` metodi utilizzati in questo test case, tra cui `getOccupations()`.

Tenere presente che questo metodo deve restituire un elenco alfabetico ordinato delle occupazioni (decrescenti) memorizzate nella proprietà occupazioni.

1. Aggiorna **`testGetOccupations()`** come segue:

   ```java
   import java.util.List;
   import com.google.common.collect.ImmutableList;
   ...
   @Test
   public void testGetOccupations() {
       List<String> expected = new ImmutableList.Builder<String>()
                               .add("Blogger")
                               .add("Photographer")
                               .add("YouTuber")
                               .build();
   
       ctx.currentResource("/content/byline");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       List<String> actual = byline.getOccupations();
   
       assertEquals(expected, actual);
   }
   ```

   * **`List<String> expected`** definisci il risultato previsto.
   * **`ctx.currentResource`** imposta la risorsa corrente in base alla quale valutare il contesto in base alla definizione di risorsa fittizia in /content/byline. In questo modo è possibile **BylineImpl.java** viene eseguito nel contesto della nostra finta risorsa.
   * **`ctx.request().adaptTo(Byline.class)`** crea un’istanza del modello Sling Byline adattandolo dal finto oggetto Request.
   * **`byline.getOccupations()`** richiama il metodo che stiamo testando, `getOccupations()`, sull&#39;oggetto Byline Sling Model.
   * **`assertEquals(expected, actual)`** asserisce che l’elenco previsto è uguale all’elenco effettivo.

1. Ricorda, proprio come **`getName()`** sopra, il **BylineImplTest.json** non definisce le occupazioni, pertanto questo test avrà esito negativo se lo eseguiamo, poiché `byline.getOccupations()` restituirà un elenco vuoto.

   Aggiorna **BylineImplTest.json** per includere un elenco di occupazioni, impostate in ordine non alfabetico per garantire che i test convalidi che le occupazioni siano ordinate alfabeticamente in base a **`getOccupations()`**.

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe",
       "occupations": ["Photographer", "Blogger", "YouTuber"]
       }
   }
   ```

1. Esegui il test e passiamo di nuovo! Sembra che le occupazioni ordinate funzionino!

   ![Ottieni Occupazioni passaggio](assets/unit-testing/testgetoccupations-pass.png)

   *testGetOccupations() passa*

## Test isEmpty() {#testing-is-empty}

Ultimo metodo da testare **`isEmpty()`**.

Test `isEmpty()` è interessante in quanto richiede test per varie condizioni. Revisione **BylineImpl.java** di `isEmpty()` metodo devono essere sottoposte a prova le seguenti condizioni:

* Restituisce true quando il nome è vuoto
* Restituisce true se le occupazioni sono nulle o vuote
* Restituisce true se l’immagine è null o non ha un URL src
* Restituisce false quando sono presenti il nome, le occupazioni e l’immagine (con un URL src)

A tal fine, è necessario creare metodi di test, ciascuno dei quali verifichi una condizione specifica e nuove strutture fittizie di risorse in `BylineImplTest.json` per eseguire questi test.

Questo controllo ci ha permesso di saltare i test per quando `getName()`, `getOccupations()` e `getImage()` sono vuoti poiché il comportamento previsto di tale stato viene testato tramite `isEmpty()`.

1. Il primo test verifica la condizione di un componente nuovo, che non ha proprietà impostate.

   Aggiungi una nuova definizione di risorsa a `BylineImplTest.json`, dandogli il nome semantico &quot;**vuoto**&quot;

   ```json
   {
       "byline": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe",
           "occupations": ["Photographer", "Blogger", "YouTuber"]
       },
       "empty": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   **`"empty": {...}`** definisci una nuova definizione di risorsa denominata &quot;empty&quot; che ha solo un `jcr:primaryType` e `sling:resourceType`.

   Ricorda che abbiamo caricato `BylineImplTest.json` in `ctx` prima dell&#39;esecuzione di ciascun metodo di prova `@setUp`, quindi questa nuova definizione di risorsa è immediatamente disponibile nei test in **/content/empty.**

1. Aggiorna `testIsEmpty()` come segue, impostare la risorsa corrente sul nuovo &quot;**vuoto**&quot; simulazione della definizione della risorsa.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   Esegui il test e assicurati che venga superato.

1. Quindi, crea un set di metodi per garantire che, se uno qualsiasi dei punti di dati richiesti (nome, occupazioni o immagine) è vuoto, `isEmpty()` restituisce true.

   Per ogni test, viene utilizzata una definizione di risorsa fittizia discreta, aggiorna **BylineImplTest.json** con le definizioni di risorse aggiuntive per **without-name** e **senza occupazioni**.

   ```json
   {
       "byline": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe",
           "occupations": ["Photographer", "Blogger", "YouTuber"]
       },
       "empty": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline"
       },
       "without-name": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "occupations": "[Photographer, Blogger, YouTuber]"
       },
       "without-occupations": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe"
       }
   }
   ```

   Crea i seguenti metodi di test per testare ciascuno di questi stati.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutName() {
       ctx.currentResource("/content/without-name");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutOccupations() {
       ctx.currentResource("/content/without-occupations");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutImage() {
       ctx.currentResource("/content/byline");
   
       lenient().when(modelFactory.getModelFromWrappedRequest(eq(ctx.request()),
           any(Resource.class),
           eq(Image.class))).thenReturn(null);
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutImageSrc() {
       ctx.currentResource("/content/byline");
   
       when(image.getSrc()).thenReturn("");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   **`testIsEmpty()`** verifica la definizione della risorsa fittizia vuota e asserisce che `isEmpty()` è vero.

   **`testIsEmpty_WithoutName()`** esegue il test in base a una definizione di risorsa fittizia con occupazioni ma senza nome.

   **`testIsEmpty_WithoutOccupations()`** esegue il test in base a una definizione di risorsa fittizia con un nome ma senza occupazioni.

   **`testIsEmpty_WithoutImage()`** esegue la verifica in base a una definizione di risorsa fittizia con un nome e occupazioni, ma imposta la finta immagine su null. Si noti che si desidera ignorare `modelFactory.getModelFromWrappedRequest(..)`comportamento definito in `setUp()` affinché l’oggetto Immagine restituito da questa chiamata sia nullo. La funzione Mockito stubs è rigida e non richiede codice duplicato. Perciò abbiamo impostato il trucco con **`lenient`** impostazioni per notare esplicitamente che stiamo ignorando il comportamento in `setUp()` metodo.

   **`testIsEmpty_WithoutImageSrc()`** esegue una verifica in base a una definizione di risorsa fittizia con un nome e occupazioni, ma imposta l’immagine fittizia in modo che restituisca una stringa vuota quando `getSrc()` viene richiamato.

1. Infine, scrivi un test per assicurarti che **isEmpty()** restituisce false quando il componente è configurato correttamente. Per questa condizione, possiamo riutilizzare **/content/byline** che rappresenta un componente Byline completamente configurato.

   ```java
   @Test
   public void testIsNotEmpty() {
       ctx.currentResource("/content/byline");
       when(image.getSrc()).thenReturn("/content/bio.png");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertFalse(byline.isEmpty());
   }
   ```

1. Ora esegui tutti gli unit test nel file BylineImplTest.java e controlla l’output del rapporto sui test Java™.

![Tutti i test superano](./assets/unit-testing/all-tests-pass.png)

## Esecuzione degli unit test come parte della build {#running-unit-tests-as-part-of-the-build}

Gli unit test vengono eseguiti e sono necessari per passare come parte della build Maven. In questo modo, tutti i test vengono superati correttamente prima che un’applicazione venga distribuita. L’esecuzione degli obiettivi Maven, ad esempio pacchetto o installazione, richiama automaticamente e richiede il passaggio di tutti gli unit test nel progetto.

```shell
$ mvn package
```

![pacchetto mvn completato](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

Allo stesso modo, se si modifica un metodo di test in modo che abbia esito negativo, la build avrà esito negativo e segnalerà quali test non sono riusciti e perché.

![errore pacchetto mvn](assets/unit-testing/mvn-package-fail.png)

## Rivedi il codice {#review-the-code}

Visualizza il codice finito il [GitHub](https://github.com/adobe/aem-guides-wknd) oppure controlla e distribuisci il codice localmente in sul ramo Git `tutorial/unit-testing-solution`.
