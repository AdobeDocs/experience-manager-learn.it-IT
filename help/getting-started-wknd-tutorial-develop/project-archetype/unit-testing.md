---
title: Test unitario
description: Implementare un test unitario che convalida il comportamento del modello Sling del componente Byline, creato nel tutorial del componente personalizzato.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '2923'
ht-degree: 100%

---

# Test unitario {#unit-testing}

Questo tutorial descrive l’implementazione di un test unitario che convalida il comportamento del modello Sling del componente Byline, creato nel tutorial [Componente personalizzato](./custom-component.md).

## Prerequisiti {#prerequisites}

Revisione degli strumenti e delle istruzioni necessari per configurare un [ambiente di sviluppo locale](overview.md#local-dev-environment).

_Se nel sistema sono installati sia Java™ 8 che Java™ 11, l&#39;esecuzione dei test di VS Code potrebbe scegliere il runtime Java™ inferiore durante l’esecuzione dei test, causando errori. In tal caso, disinstallare Java™ 8._

### Progetto iniziale

>[!NOTE]
>
> Se hai completato correttamente il capitolo precedente, puoi riutilizzare il progetto e saltare i passaggi per controllare il progetto iniziale.

Controlla il codice della riga di base su cui si fonda l’esercitazione:

1. Controlla il ramo `tutorial/unit-testing-start` da [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/unit-testing-start
   ```

1. Distribuisci la base di codice in un’istanza AEM locale utilizzando le abilità Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se utilizzi AEM 6.5 o 6.4, aggiungi il profilo `classic` a qualsiasi comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start) o estrarre il codice localmente passando al ramo `tutorial/unit-testing-start`.

## Obiettivo

1. Informazioni sulle le nozioni di base sul test unitario.
1. Scopri i framework e gli strumenti comunemente utilizzati per testare il codice AEM.
1. Informazioni sulle le opzioni per creare risorse AEM fittizie o simulate durante la scrittura di test unitari.

## Esperienza pregressa {#unit-testing-background}

In questo tutorial verrà illustrato come scrivere [test unitari](https://en.wikipedia.org/wiki/Unit_testing) per il [modello Sling](https://sling.apache.org/documentation/bundles/models.html) del componente Byline (creato nella sezione [Creazione di un componente AEM personalizzato](custom-component.md)). I test unitari sono test scritti durante la fase di compilazione in Java™ che verificano il comportamento previsto del codice Java™. Ciascun test unitario è in genere piccolo e convalida l’output di un metodo (o unità di lavoro) in base ai risultati previsti.

Utilizziamo le best practice di AEM insieme a:

* [JUnit 5](https://junit.org/junit5/)
* [Framework di test Mockito](https://site.mockito.org/)
* [Framework di test di wcm.io](https://wcm.io/testing/) (che si basa su [Mock di Apache Sling](https://sling.apache.org/documentation/development/sling-mock.html))

## Test unitario e Adobe Cloud Manager {#unit-testing-and-adobe-cloud-manager}

[Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html?lang=it) integra l’esecuzione di test unitari e il [reporting sulla copertura del codice](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-quality-testing.html?lang=it) nella propria pipeline CI/CD per incoraggiare e promuovere la best practice per il codice AEM dei test unitari.

Anche se il codice del test unitario è una prassi ideale per qualsiasi base di codice, quando si utilizza Cloud Manager è importante sfruttarne i servizi di test e reporting della qualità del codice fornendo test unutario per l’esecuzione di Cloud Manager.

## Aggiornare le dipendenze Maven del test {#inspect-the-test-maven-dependencies}

Il primo passaggio consiste nell’esaminare le dipendenze Maven per supportare la scrittura e l’esecuzione dei test. Sono necessarie quattro dipendenze:

1. JUnit5
1. Framework di test per Mokito
1. Apache Sling Mocks
1. Infrastruttura di test di AEM Mocks (di io.wcm)

Le dipendenze dei test **JUnit5**, **Mockito e **AEM Mocks** vengono aggiunte automaticamente al progetto durante l’installazione utilizzando l’[archetipo AEM Maven](project-setup.md).

1. Per visualizzare queste dipendenze, apri il POM del reattore principale in **aem-guides-wknd/pom.xml**, passa a `<dependencies>..</dependencies>` e visualizza le dipendenze dei test JUnit, Mockito, Apache Sling Mocks e AEM Mock di io.wcm in `<!-- Testing -->`.
1. Verifica che `io.wcm.testing.aem-mock.junit5` sia impostato su **4.1.0**:

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
   > Archetipo **35** genera il progetto con `io.wcm.testing.aem-mock.junit5` versione **4.1.8**. Effettua il downgrade a **4.1.0** per seguire il resto di questo capitolo.

1. Apri **aem-guides-wknd/core/pom.xml** e osserva che le dipendenze di test corrispondenti sono disponibili.

   Una cartella di origine parallela nel progetto **core** conterrà i test di unità ed eventuali file di test di supporto. Questa cartella **test** assicura la separazione delle classi di test dal codice sorgente, ma consente ai test di agire come se si trovassero negli stessi pacchetti del codice sorgente.

## Creazione del test JUnit {#creating-the-junit-test}

I test di unità tipicamente sono mappati 1-a-1 con le classi Java™. In questo capitolo verrà scritto un test JUnit per **BylineImpl.java**, che è il modello Sling che supporta il componente Byline.

![Cartella del test di unità src](assets/unit-testing/core-src-test-folder.png)

*Percorso in cui sono archiviati i test di unità.*

1. Crea un test di unità per `BylineImpl.java` creando una nuova classe Java™ in `src/test/java` in una struttura di cartelle di pacchetti Java™ che rispecchia la posizione della classe Java™ da testare.

   ![Creare un nuovo file BylineImplTest.java](assets/unit-testing/new-bylineimpltest.png)

   Poiché stiamo testando

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   creare una classe Java™ per test di unità corrispondente in

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   Il suffisso `Test` nel file di test di unità `BylineImplTest.java` è una convenzione che consente di

   1. Identificarlo facilmente come file di test _per_ `BylineImpl.java`
   1. Ma anche differenziare il file di test _dalla_ classe in fase di test, `BylineImpl.java`

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

1. Il primo metodo `public void setUp() { .. }` è annotato con `@BeforeEach` di JUnit, che indica a chi esegue il test JUnit di eseguire questo metodo prima di ogni metodo di test in questa classe. Ciò permette comodamente di inizializzare lo stato di test comune richiesto da tutti i test.

1. I metodi successivi sono i metodi di test, i cui nomi sono preceduti da `test` per convenzione e contrassegnati con l’annotazione `@Test`. Per impostazione predefinita, tutti i nostri test sono impostati per non riuscire, poiché non li abbiamo ancora implementati.

   Innanzitutto, iniziamo con un singolo metodo di test per ogni metodo pubblico sulla classe che stiamo testando, quindi:

   | BylineImpl.java |              | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | è testato da | testGetName() |
   | getOccupations() | è testato da | testGetOccupations() |
   | isEmpty() | è testato da | testIsEmpty() |

   Questi metodi possono essere ampliati in base alle esigenze, come vedremo più avanti in questo capitolo.

   Quando viene eseguita questa classe di test JUnit (nota anche come caso di test JUnit), ogni metodo contrassegnato con `@Test` verrà eseguito come test che può essere superato o non superato.

![BylineImplTest è stato generato](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. Eseguire il caso di test JUnit facendo clic con il pulsante destro del mouse sul file `BylineImplTest.java` e toccando **Esegui**.
Come previsto, tutti i test hanno esito negativo poiché non sono ancora stati implementati.

   ![Esegui come test Junit](assets/unit-testing/run-junit-tests.png)

   *Fai clic con il pulsante destro del mouse su BylineImplTests.java > Esegui*

## Revisione di BylineImpl.java {#reviewing-bylineimpl-java}

Durante la scrittura dei test unitari, esistono due approcci principali:

* [TDD o sviluppo basato su test](https://en.wikipedia.org/wiki/Test-driven_development), che prevede la scrittura dei test unitari in modo incrementale, immediatamente prima dello sviluppo dell’implementazione; si scrive un test, poi se ne scrive l’implementazione affinché il test venga superato.
* Sviluppo con implementazione immediata, che comporta prima lo sviluppo del codice funzionante, quindi la scrittura di test che convalidano tale codice.

In questo tutorial viene utilizzato quest’ultimo approccio (in quanto è già stato creato un **BylineImpl.java** funzionante in un capitolo precedente). Per questo motivo, dobbiamo rivedere e comprendere i comportamenti dei relativi metodi pubblici, ma anche alcuni dei dettagli di implementazione. Questo può sembrare una contraddizione, poiché un buon test dovrebbe preoccuparsi solo degli input e degli output. Tuttavia quando si utilizza AEM, ci sono varie considerazioni di implementazione che devono essere comprese per creare test operativi.

Il TDD nel contesto di AEM richiede un livello di esperienza ed è adottato al meglio da sviluppatori AEM esperti nello sviluppo AEM e nel test unitario del codice AEM.

## Configurazione del contesto di test di AEM  {#setting-up-aem-test-context}

La maggior parte del codice scritto per AEM si basa sulle API JCR, Sling o AEM, che a loro volta richiedono la corretta esecuzione del contesto di AEM in esecuzione.

Poiché i test unitari vengono eseguiti durante il processo di build, al di fuori del contesto di un’istanza AEM in esecuzione, tale contesto non esiste. Per facilitare questa fase, [AEM Mock di wcm.io](https://wcm.io/testing/aem-mock/usage.html) crea un contesto fittizio che consente a queste API di agire _principalmente_ come se fossero in esecuzione su AEM.

1. Creare un contesto AEM utilizzando **wcm.io&#39;s** `AemContext` in **BylineImplTest.java** aggiungendolo come estensione JUnit annotata con `@ExtendWith` al file **BylineImplTest.java**. L&#39;estensione si occupa di tutte le attività di inizializzazione e pulizia necessarie. Crea una variabile di classe per `AemContext` che può essere utilizzata per tutti i metodi di test.

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
   * Fornisce vari oggetti e helper fittizi richiesti, come oggetti SlingHttpServletRequest, vari servizi Sling e AEM OSGi fittizi come ModelFactory, PageManager, Pagina, Modello, ComponentManager, Componente, TagManager, Tag, ecc.
      * *Non tutti i metodi per questi oggetti sono implementati.*
   * E [altro ancora](https://wcm.io/testing/aem-mock/usage.html).

   L&#39;oggetto **`ctx`** fungerà da punto di ingresso per la maggior parte del contesto fittizio.

1. Nel metodo `setUp(..)`, eseguito prima di ogni metodo `@Test`, definisci uno stato di test fittizio comune:

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** registra il modello Sling da testare nel contesto AEM fittizio, in modo da crearne un&#39;istanza nei metodi `@Test`.
   * **`load().json`** carica le strutture delle risorse nel contesto fittizio, consentendo al codice di interagire con tali risorse come se fossero fornite da un archivio reale. Le definizioni delle risorse nel file **`BylineImplTest.json`** sono caricate nel contesto JCR fittizio in **/content**.
   * **`BylineImplTest.json`** non esiste ancora, quindi verrà creato e le strutture di risorse JCR necessarie per il test verranno definite.

1. I file JSON che rappresentano le strutture di risorse fittizie sono archiviati in **core/src/test/resources** seguendo lo stesso percorso del pacchetto del file di test Java™ JUnit.

   Crea un file JSON in `core/test/resources/com/adobe/aem/guides/wknd/core/models/impl` denominato **BylineImplTest.json** con il contenuto seguente:

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   Questo JSON definisce una risorsa fittizia (nodo JCR) per il test unitario del componente Byline. A questo punto, il JSON dispone del set minimo di proprietà necessarie per rappresentare una risorsa di contenuto del componente Byline, `jcr:primaryType` e `sling:resourceType`.

   Una regola generale quando vengono utilizzati i test unitari consiste nel creare il set minimo di contenuti, contesto e codice fittizi necessari per soddisfare ogni test. Evita la tentazione di creare un contesto fittizio completo prima di scrivere i test, in quanto spesso genera artefatti non necessari.

   Ora con l’esistenza di **BylineImplTest.json**, quando viene eseguito `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")`, le definizioni delle risorse fittizie vengono caricate nel contesto nel percorso **/contenuto.**

## Test di getName() {#testing-get-name}

Ora che abbiamo configurato un contesto fittizio di base, scriviamo il primo test per **BylineImpl&#39;s getName()**. Questo test deve garantire che il metodo **getName()** restituisca il nome creato correttamente archiviato nella proprietà &quot;**name**&quot; della risorsa.

1. Aggiorna il metodo **testGetName**() in **BylineImplTest.java** nel modo seguente:

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

   * **`String expected`** imposta il valore previsto. Verrà impostato su &quot;**Jane Done**&quot;.
   * **`ctx.currentResource`** imposta il contesto della risorsa fittizia su cui valutare il codice, quindi viene impostato su **/content/byline** in quanto è dove viene caricata la risorsa di contenuto fittizia.
   * **`Byline byline`** crea un’istanza del modello Sling Byline adattandolo dall’oggetto Richiesta fittizio.
   * **`String actual`** richiama il metodo in fase di test, `getName()`, sull’oggetto modello Sling Byline.
   * **`assertEquals`** afferma che il valore previsto corrisponde al valore restituito dall’oggetto modello Sling Byline. Se questi valori non sono uguali, il test avrà esito negativo.

1. Esegui il test... e non riesce con `NullPointerException`.

   Il test NON ha esito negativo perché non è mai stata definita una proprietà `name` nel JSON fittizio. Il test non verrà eseguito correttamente, tuttavia l’esecuzione del test non è arrivata a questo punto. Il test non riesce a causa di un `NullPointerException` nell’oggetto Byline stesso.

1. In `BylineImpl.java`, se `@PostConstruct init()` genera un&#39;eccezione, impedisce al modello Sling di creare un&#39;istanza e causa l’impostazione dell’oggetto modello Sling come null.

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   Si scopre che mentre il servizio OSGi ModelFactory viene fornito tramite `AemContext` (per mezzo di Apache Sling Context), non tutti i metodi vengono implementati, incluso `getModelFromWrappedRequest(...)` che viene chiamato nel metodo `init()` di BylineImpl. Ciò determina un [AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.htm), che nel termine causa il mancato funzionamento di `init()` e il conseguente adattamento di `ctx.request().adaptTo(Byline.class)` è un oggetto null.

   Poiché i contesti fittizi forniti non possono contenere il nostro codice, è necessario implementare direttamente il contesto fittizio. Per questo è possibile utilizzare Mockito per creare un oggetto ModelFactory fittizio, che restituisce un oggetto Image fittizio quando `getModelFromWrappedRequest(...)` viene richiamato su di esso.

   Poiché per creare un’istanza del modello Sling Byline è necessario che il contesto fittizio sia stato creato, è possibile aggiungerlo al metodo `@Before setUp()`. È inoltre necessario aggiungere `MockitoExtension.class` all’`@ExtendWith`annotazione sopra la classe **BylineImplTest**.

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

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** contrassegna la classe caso di test da eseguire con l’estensione [Mockito JUnit Jupiter](https://www.javadoc.io/static/org.mockito/mockito-junit-jupiter/4.11.0/org/mockito/junit/jupiter/MockitoExtension.html) che consente l’utilizzo delle annotazioni @Mock per definire oggetti fittizi a livello di classe.
   * **`@Mock private Image`** crea un oggetto fittizio di tipo `com.adobe.cq.wcm.core.components.models.Image`. Viene definito a livello di classe in modo che, se necessario, i metodi `@Test` possano modificarne il comportamento in base alle esigenze.
   * **`@Mock private ModelFactory`** crea un oggetto fittizio di tipo ModelFactory. Ecco un perfetto oggetto fittizio di Mockito sul quale non risultano metodi implementati. Viene definito a livello di classe in modo che, se necessario, i metodi `@Test` possano modificarne il comportamento in base alle esigenze.
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** registra il comportamento fittizio per quando `getModelFromWrappedRequest(..)` viene chiamato sull’oggetto ModelFactory fittizio. Il risultato definito in `thenReturn (..)` è quello di restituire l&#39;oggetto immagine fittizio. Questo comportamento viene richiamato solo quando: il primo parametro è uguale all’oggetto richiesta di `ctx`, il secondo parametro è qualsiasi oggetto risorsa e il terzo parametro deve essere la classe immagine dei componenti core. Accettiamo qualsiasi risorsa perché durante i test impostiamo `ctx.currentResource(...)` su varie risorse fittizie definite in **BylineImplTest.json**. Verrà aggiunta la rigidità **lenient()** perché in seguito vorremmo sostituire questo comportamento di ModelFactory.
   * **`ctx.registerService(..)`.** registra l’oggetto ModelFactory fittizio in AemContext, con la classificazione di servizio più elevata. L’operazione è necessaria in quanto il ModelFactory utilizzato in `init()` di BylineImpl viene inserito tramite il campo `@OSGiService ModelFactory model`. Affinché AemContext inserisca il **nostro** oggetto fittizio, che gestisce le chiamate a `getModelFromWrappedRequest(..)`, dobbiamo registrarlo come servizio di classificazione più elevata di quel tipo (ModelFactory).

1. Riesegui il test, che non andrà di nuovo a buon fine, ma questa volta il messaggio indicherà chiaramente il motivo dell’errore.

   ![asserzione errore nome test](assets/unit-testing/testgetname-failure-assertion.png)

   *errore testGetName() a causa di un’asserzione*

   Riceviamo un **AssertionError** che indica che la condizione di asserzione nel test non è riuscita e che il **valore previsto è “Jane Doe”** ma il **valore effettivo è nullo**. In effetti, la proprietà “**name”** non è stata aggiunta alla definizione fittizia della risorsa **/content/byline** in **BylineImplTest.json**, quindi dobbiamo aggiungerla noi:

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

1. Riesegui il test ed ecco che **`testGetName()`** lo supera correttamente.

   ![test del nome superato](assets/unit-testing/testgetname-pass.png)


## Test di getOccupations() {#testing-get-occupations}

Ok, perfetto. Il primo test è stato superato. Passiamo al test di `getOccupations()`. Poiché l’inizializzazione del contesto fittizio è stata eseguita secondo il metodo `@Before setUp()`, questo è disponibile per tutti i metodi `@Test` in questo caso di test, incluso `getOccupations()`.

Tieni presente che questo metodo deve restituire un elenco alfabetico ordinato delle occupazioni (in ordine decrescente) memorizzate nella proprietà occupazioni.

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

   * **`List<String> expected`** definiscono il risultato previsto.
   * **`ctx.currentResource`** imposta la risorsa corrente per la valutazione del contesto sulla definizione di risorsa fittizia in /content/byline. In questo modo **BylineImpl.java** viene eseguito nel contesto della risorsa fittizia.
   * **`ctx.request().adaptTo(Byline.class)`** crea un’istanza del modello Sling Byline adattandolo dal finto oggetto Richiesta.
   * **`byline.getOccupations()`** richiama il metodo in fase di test, `getOccupations()`, sull’oggetto del modello Sling Byline.
   * **`assertEquals(expected, actual)`** asserisce che l’elenco previsto è uguale all’elenco effettivo.

1. Ricorda che, proprio come **`getName()`** in precedenza, **BylineImplTest.json** non definisce le occupazioni, pertanto questo test non andrà a buon fine se viene eseguito, poiché `byline.getOccupations()` restituirà un elenco vuoto.

   Aggiorna **BylineImplTest.json** per includere un elenco di occupazioni impostate in ordine non alfabetico affinché i test convalidino che le occupazioni sono in ordine alfabetico per **`getOccupations()`**.

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

1. Esegui il test ed ecco che lo superi. Sembra che le occupazioni messe in ordine funzionino.

   ![Supera il test delle occupazioni](assets/unit-testing/testgetoccupations-pass.png)

   *testGetOccupations() superato*

## Test di isEmpty() {#testing-is-empty}

Ultimo metodo per testare **`isEmpty()`**.

Il test `isEmpty()` è interessante in quanto richiede di testare varie condizioni. Esaminando il metodo `isEmpty()` di **BylineImpl.java**, è necessario verificare le seguenti condizioni:

* Restituisce true quando il nome è vuoto
* Restituisce true se le occupazioni sono nulle o vuote
* Restituisce true se l’immagine è null o non dispone di un URL src
* Restituisce false quando sono presenti il nome, le occupazioni e l’immagine (con un URL src)

A tale scopo, è necessario creare metodi di test, ciascuno dei quali esegue il test di una condizione specifica e di nuove strutture di risorse fittizie in `BylineImplTest.json` per eseguire questi test.

Questo controllo ci ha permesso di ignorare i test per quando `getName()`, `getOccupations()` e `getImage()` sono vuoti, poiché il comportamento previsto di tale stato è testato tramite `isEmpty()`.

1. Il primo test verifica la condizione di un componente nuovo, che non dispone di proprietà impostate.

   Aggiungi una nuova definizione di risorsa a `BylineImplTest.json`, assegnandole il nome semantico &quot;**empty**&quot;

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

   **`"empty": {...}`** definiscono una nuova definizione di risorsa denominata &quot;empty&quot; che dispone solo di `jcr:primaryType` e di `sling:resourceType`.

   Ricorda che, prima dell’esecuzione di ciascun metodo di test in `@setUp`, abbiamo caricato `BylineImplTest.json` in `ctx`, quindi questa nuova definizione di risorsa è immediatamente disponibile nei test in **/content/empty.**

1. Aggiorna `testIsEmpty()` come segue, impostando la risorsa corrente sulla nuova definizione di risorsa fittizia &quot;**empty**&quot;.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   Esegui il test e assicurati che venga superato.

1. Crea quindi un set di metodi per garantire che, se uno qualsiasi dei punti di dati richiesti (nome, occupazioni o immagine) è vuoto, `isEmpty()` restituisca true.

   Per ogni test viene utilizzata una definizione di risorsa fittizia. Aggiorna **BylineImplTest.json** con le definizioni di risorsa aggiuntive per **without-name** e **without-occupations**.

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

   **`testIsEmpty()`** verifica la definizione della risorsa fittizia vuota e asserisce che `isEmpty()` è true.

   **`testIsEmpty_WithoutName()`** esegue il test in base a una definizione di risorsa fittizia con occupazioni ma senza nome.

   **`testIsEmpty_WithoutOccupations()`** esegue il test in base a una definizione di risorsa fittizia con un nome ma senza occupazioni.

   **`testIsEmpty_WithoutImage()`** esegue il test in base a una definizione di risorsa fittizia con un nome e occupazioni, ma imposta l’immagine fittizia su null. Tieni presente che desideriamo sostituire il comportamento `modelFactory.getModelFromWrappedRequest(..)` definito in `setUp()` per assicurarsi che l’oggetto Immagine restituito da questa chiamata sia nullo. La funzione di stubbing di Mockito è rigida e non richiede codice duplicato. Pertanto, abbiamo impostato le impostazioni dei valori simulati con **`lenient`** in modo che venga notato esplicitamente che stiamo sostituendo il comportamento nel metodo `setUp()`.

   **`testIsEmpty_WithoutImageSrc()`** esegue test su una definizione di risorsa fittizia con un nome e occupazioni, ma imposta l’immagine fittizia in modo che restituisca una stringa vuota quando viene richiamato `getSrc()`.

1. Infine, scrivi un test per assicurarti che **isEmpty()** restituisca false quando il componente è configurato correttamente. Per questa condizione, è possibile riutilizzare **/content/byline** che rappresenta un componente Byline completamente configurato.

   ```java
   @Test
   public void testIsNotEmpty() {
       ctx.currentResource("/content/byline");
       when(image.getSrc()).thenReturn("/content/bio.png");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertFalse(byline.isEmpty());
   }
   ```

1. Ora esegui tutti i test unitari nel file BylineImplTest.java ed esamina l’output del rapporto sui test Java™.

![Tutti i test superano](./assets/unit-testing/all-tests-pass.png)

## Esecuzione dei test unitari come parte della build {#running-unit-tests-as-part-of-the-build}

I test unitari vengono eseguiti e devono essere superati come parte della build Maven. In questo modo, tutti i test vengono superati correttamente prima che un’applicazione venga distribuita. L’esecuzione degli obiettivi Maven, ad esempio pacchetto o installazione, richiama automaticamente e richiede il superamento di tutti i test unitari nel progetto.

```shell
$ mvn package
```

![mvn package riuscito](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

Allo stesso modo, se si modifica un metodo di test in modo che fallisca, la build avrà esito negativo e segnalerà quali test sonno falliti e perché.

![mvn package fallito](assets/unit-testing/mvn-package-fail.png)

## Rivedi il codice {#review-the-code}

Visualizza il codice finito in [GitHub](https://github.com/adobe/aem-guides-wknd) oppure rivedi e distribuisci il codice localmente nel ramo Git `tutorial/unit-testing-solution`.
