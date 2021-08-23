---
title: Test di unità
description: Questa esercitazione descrive l'implementazione di un unit test che convalida il comportamento del modello Sling del componente Byline, creato nell'esercitazione sui componenti personalizzati.
sub-product: sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: API, AEM Archetipo di progetto
topic: Gestione dei contenuti, sviluppo
role: Developer
level: Beginner
kt: 4089
mini-toc-levels: 1
thumbnail: 30207.jpg
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '3013'
ht-degree: 0%

---


# Test di unità {#unit-testing}

Questa esercitazione descrive l&#39;implementazione di un unit test che convalida il comportamento del modello Sling del componente Byline, creato nell&#39;esercitazione [Custom Component](./custom-component.md) .

## Prerequisiti {#prerequisites}

Rivedi gli strumenti e le istruzioni necessari per configurare un [ambiente di sviluppo locale](overview.md#local-dev-environment).

_Se sul sistema sono installati sia Java 8 che Java 11, il runner di test del codice VS può scegliere il runtime Java inferiore durante l’esecuzione dei test, dando luogo a errori di test. In questo caso, disinstalla Java 8._

### Progetto iniziale

>[!NOTE]
>
> Se hai completato con successo il capitolo precedente, puoi riutilizzare il progetto e saltare i passaggi per il check-out del progetto iniziale.

Controlla il codice della riga di base su cui si basa l&#39;esercitazione:

1. Estrai il ramo `tutorial/unit-testing-start` da [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/unit-testing-start
   ```

1. Distribuisci la base di codice in un&#39;istanza AEM locale utilizzando le tue competenze Maven:

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

1. Comprendere le nozioni di base del testing di unità.
1. Scopri i framework e gli strumenti comunemente utilizzati per testare il codice AEM.
1. Comprendere le opzioni per schermare o simulare le risorse AEM durante la scrittura di unit test.

## Sfondo {#unit-testing-background}

In questa esercitazione, scopriremo come scrivere [Test di unità](https://en.wikipedia.org/wiki/Unit_testing) per il [Modello Sling](https://sling.apache.org/documentation/bundles/models.html) del componente Byline (creato in [Creazione di un componente AEM personalizzato](custom-component.md)). I test di unità sono test di build-time scritti in Java che verificano il comportamento previsto del codice Java. Ogni unit test è generalmente di piccole dimensioni e convalida l&#39;output di un metodo (o unità di lavoro) in base ai risultati previsti.

Useremo AEM best practice e utilizzeremo:

* [JUnit 5](https://junit.org/junit5/)
* [Framework di test di Mockito](https://site.mockito.org/)
* [Framework di test wcm.io](https://wcm.io/testing/)  (che si basa su  [Apache Sling Mocks](https://sling.apache.org/documentation/development/sling-mock.html))

## Test di unità e Adobe Cloud Manager {#unit-testing-and-adobe-cloud-manager}

[Adobe Cloud ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html?lang=it) Manager integra l’esecuzione dei test di unità e la copertura del  [codice nella ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/understand-your-test-results.html#code-quality-testing) sua pipeline CI/CD per favorire e promuovere le best practice relative ai test di unità AEM codice.

Anche se il codice di testing di unità è una buona pratica per qualsiasi base di codice, quando utilizzi Cloud Manager è importante sfruttare i suoi servizi di testing e reporting per la qualità del codice, fornendo test di unità per l’esecuzione di Cloud Manager.

## Inspect per testare le dipendenze Maven {#inspect-the-test-maven-dependencies}

Il primo passo è quello di controllare le dipendenze Maven per supportare la scrittura e l’esecuzione dei test. Sono necessarie quattro dipendenze:

1. JUnit5
1. Framework di test di Mockito
1. Apache Sling Mocks
1. Framework di test di AEM (di io.wcm)

Le dipendenze di test **JUnit5**, **Mockito** e **AEM Mocks** vengono aggiunte automaticamente al progetto durante la configurazione utilizzando il [AEM Maven archetype](project-setup.md).

1. Per visualizzare queste dipendenze, apri il POM del reattore principale in **aem-guides-wknd/pom.xml**, vai a `<dependencies>..</dependencies>` e assicurati che siano definite le seguenti dipendenze:

   ```xml
   <dependencies>
       ...       
       <!-- Testing -->
       <dependency>
           <groupId>org.junit</groupId>
           <artifactId>junit-bom</artifactId>
           <version>5.6.2</version>
           <type>pom</type>
           <scope>import</scope>
       </dependency>
       <dependency>
           <groupId>org.mockito</groupId>
           <artifactId>mockito-core</artifactId>
           <version>3.3.3</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>org.mockito</groupId>
           <artifactId>mockito-junit-jupiter</artifactId>
           <version>3.3.3</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>junit-addons</groupId>
           <artifactId>junit-addons</artifactId>
           <version>1.4</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>io.wcm</groupId>
           <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
           <!-- Prefer the latest version of AEM Mock Junit5 dependency -->
           <version>3.0.2</version>
           <scope>test</scope>
       </dependency>        
       ...
   </dependencies>
   ```

1. Apri **aem-guides-wknd/core/pom.xml** e osserva che sono disponibili le dipendenze di test corrispondenti:

   ```xml
   ...
   <!-- Testing -->
   <dependency>
       <groupId>org.junit.jupiter</groupId>
       <artifactId>junit-jupiter</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>org.mockito</groupId>
       <artifactId>mockito-core</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>org.mockito</groupId>
       <artifactId>mockito-junit-jupiter</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>junit-addons</groupId>
       <artifactId>junit-addons</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>io.wcm</groupId>
       <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
       <exclusions>
           <exclusion>
               <groupId>org.apache.sling</groupId>
               <artifactId>org.apache.sling.models.impl</artifactId>
           </exclusion>
           <exclusion>
               <groupId>org.slf4j</groupId>
               <artifactId>slf4j-simple</artifactId>
           </exclusion>
       </exclusions>
       <scope>test</scope>
   </dependency>
   <!-- Required to be able to support injection with @Self and @Via -->
   <dependency>
       <groupId>org.apache.sling</groupId>
       <artifactId>org.apache.sling.models.impl</artifactId>
       <version>1.4.4</version>
       <scope>test</scope>
   </dependency>
   ...
   ```

   Una cartella di origine parallela nel progetto **core** conterrà i test di unità ed eventuali file di test di supporto. Questa cartella **test** consente di separare le classi di test dal codice sorgente, ma consente ai test di agire come se si trovassero negli stessi pacchetti del codice sorgente.

## Creazione del test JUnit {#creating-the-junit-test}

I test di unità generalmente vengono mappati da 1 a 1 con classi Java. In questo capitolo, scriveremo un test JUnit per il **BylineImpl.java**, che è il modello Sling che supporta il componente Byline.

![Cartella src di prova unità](assets/unit-testing/core-src-test-folder.png)

*Posizione in cui sono archiviati i test di unità.*

1. Crea un test di unità per il `BylineImpl.java` creando una nuova classe Java sotto `src/test/java` in una struttura di cartelle di pacchetti Java che rispecchi la posizione della classe Java da testare.

   ![Crea un nuovo file BylineImplTest.java](assets/unit-testing/new-bylineimpltest.png)

   Dal momento che stiamo testando

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   crea una classe Java di test di unità corrispondente in

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   Il suffisso `Test` sul file di prova dell&#39;unità, `BylineImplTest.java` è una convenzione che ci consente di:

   1. Identificalo facilmente come file di prova _per_ `BylineImpl.java`
   1. Ma anche, differenziare il file di prova _da_ la classe in fase di test, `BylineImpl.java`



## Revisione di BylineImplTest.java {#reviewing-bylineimpltest-java}

A questo punto, il file di test JUnit è una classe Java vuota. Aggiorna il file con il seguente codice:

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

1. Il primo metodo `public void setUp() { .. }` viene annotato con l&#39;elemento `@BeforeEach` di JUnit, che indica all&#39;operatore di test JUnit di eseguire questo metodo prima di eseguire ogni metodo di test in questa classe. Questo fornisce un comodo punto in cui inizializzare lo stato di test comune richiesto da tutti i test.

2. I metodi successivi sono i metodi di prova, i cui nomi hanno il prefisso `test` per convenzione e sono contrassegnati con l&#39;annotazione `@Test`. Per impostazione predefinita, tutti i nostri test sono impostati per non riuscire, in quanto non sono ancora stati implementati.

   Per iniziare, iniziamo con un singolo metodo di test per ogni metodo pubblico sulla classe che stiamo testando, quindi:

   | BylineImpl.java |  | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | è testato da | testGetName() |
   | getOccupations() | è testato da | testGetOccupations() |
   | isEmpty() | è testato da | testIsEmpty() |

   Questi metodi possono essere ampliati in base alle esigenze, come vedremo più avanti in questo capitolo.

   Quando si esegue questa classe di test JUnit (nota anche come JUnit Test Case), ogni metodo contrassegnato con `@Test` viene eseguito come test che può essere superato o non superato.

![generato BylineImplTest](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. Esegui il Test Case di JUnit facendo clic con il pulsante destro del mouse sul file `BylineImplTest.java` e toccando **Esegui**.
Come previsto, tutti i test falliscono, in quanto non sono ancora stati implementati.

   ![Esegui come test junit](assets/unit-testing/run-junit-tests.png)

   *Fai clic con il pulsante destro del mouse su BylineImplTests.java > Esegui*

## Revisione di BylineImpl.java {#reviewing-bylineimpl-java}

Durante la scrittura di unit test, esistono due approcci principali:

* [sviluppo](https://en.wikipedia.org/wiki/Test-driven_development) basato su TDD o su test, che comporta la scrittura incrementale dei test per unità di misura, immediatamente prima dello sviluppo dell’implementazione; scrivi un test, scrivi l&#39;implementazione per far passare il test.
* Implementazione-primo sviluppo, che comporta lo sviluppo prima del codice di lavoro e poi la scrittura di test che convalidano tale codice.

In questa esercitazione viene utilizzato l&#39;ultimo approccio (in quanto abbiamo già creato un funzionamento **BylineImpl.java** in un capitolo precedente). Per questo motivo, dobbiamo rivedere e comprendere i comportamenti dei suoi metodi pubblici, ma anche alcuni dei relativi dettagli di implementazione. Ciò può sembrare contrario, poiché un buon test dovrebbe preoccuparsi solo degli input e dei risultati, tuttavia, quando si lavora in AEM, è necessario comprendere una serie di considerazioni di attuazione al fine di costruire test di lavoro.

Il TDD nel contesto di AEM richiede un livello di competenza ed è meglio adottato dagli sviluppatori AEM esperti nello sviluppo AEM e nel testing di unità del codice AEM.

## Impostazione AEM contesto di test  {#setting-up-aem-test-context}

La maggior parte del codice scritto per AEM si basa su API JCR, Sling o AEM, che a loro volta richiedono che il contesto di un AEM in esecuzione venga eseguito correttamente.

Poiché i test di unità vengono eseguiti nella build, al di fuori del contesto di un&#39;istanza AEM in esecuzione, non esiste un contesto simile. Per facilitare questa fase, i AEM Mocks di ](https://wcm.io/testing/aem-mock/usage.html) di wcm.io creano un contesto fittizio che consente a queste API di _per lo più_ agire come se fossero in esecuzione in AEM.[

1. Crea un contesto AEM utilizzando **wcm.io&#39;s** `AemContext` in **BylineImplTest.java** aggiungendolo come estensione JUnit decorata con `@ExtendWith` al file **BylineImplTest.java**. L&#39;estensione si occupa di tutte le attività di inizializzazione e pulizia richieste. Crea una variabile di classe per `AemContext` che può essere utilizzata per tutti i metodi di test.

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   Questa variabile, `ctx`, espone un contesto AEM fittizio che fornisce una serie di astrazioni AEM e Sling:

   * Il modello Sling BylineImpl verrà registrato in questo contesto
   * Le strutture di contenuto JCR bloccate vengono create in questo contesto
   * I servizi OSGi personalizzati possono essere registrati in questo contesto
   * Fornisce una varietà di oggetti e assistenti comuni richiesti come oggetti SlingHttpServletRequest, una varietà di servizi Sling e OSGi di simulazione come ModelFactory, PageManager, Page, Template, ComponentManager, ComponentManager, Component, TagManager, Tag, ecc.
      * *Non tutti i metodi per questi oggetti sono implementati.*
   * E [molto di più](https://wcm.io/testing/aem-mock/usage.html)!

   L’oggetto **`ctx`** fungerà da punto di ingresso per la maggior parte del contesto fittizio.

1. Nel metodo `setUp(..)`, eseguito prima di ogni metodo `@Test`, definisci uno stato comune di test del modello:

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** registra il modello Sling da testare, nel contesto di AEM simulato, in modo che possa essere istanziato nei  `@Test` metodi .
   * **`load().json`** carica le strutture delle risorse nel contesto fittizio, consentendo al codice di interagire con tali risorse come se fossero fornite da un archivio reale. Le definizioni delle risorse nel file **`BylineImplTest.json`** vengono caricate nel contesto JCR fittizio in **/content**.
   * **`BylineImplTest.json`** non esiste ancora, quindi creiamolo e definiamo le strutture di risorse JCR necessarie per il test.

1. I file JSON che rappresentano le strutture delle risorse fittizie vengono memorizzati in **core/src/test/resources** seguendo lo stesso percorso del pacchetto del file di test Java JUnit.

   Crea un nuovo file JSON in **core/test/resources/com/adobe/aem/guides/wknd/core/models/impl** denominato **BylineImplTest.json** con il seguente contenuto:

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   Questo JSON definisce una risorsa fittizia (nodo JCR) per il test dell’unità del componente Byline. A questo punto, il JSON dispone del set minimo di proprietà necessarie per rappresentare una risorsa di contenuto del componente Byline, i valori `jcr:primaryType` e `sling:resourceType`.

   Una regola generale quando si lavora con i test di unità consiste nel creare un set minimo di contenuto, contesto e codice di simulazione necessari per soddisfare ogni test. Evita la tentazione di creare un contesto completo e fittizio prima di scrivere i test, in quanto spesso si traduce in artefatti non necessari.

   Ora con l&#39;esistenza di **BylineImplTest.json**, quando viene eseguito `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")`, le definizioni delle risorse fittizie vengono caricate nel contesto nel percorso **/content.**

## Verifica di getName() {#testing-get-name}

Ora che disponiamo di una configurazione di base del contesto fittizio, scriviamo il nostro primo test per **BylineImpl&#39;s getName()**. Questo test deve garantire che il metodo **getName()** restituisca il nome di authoring corretto memorizzato nella proprietà &quot;**name&quot;** della risorsa.

1. Aggiorna il metodo **testGetName**() in **BylineImplTest.java** come segue:

   ```java
   import com.adobe.aem.guides.wknd.core.components.Byline;
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

   * **`String expected`** imposta il valore previsto. Imposta questo su &quot;**Jane Done**&quot;.
   * **`ctx.currentResource`** imposta il contesto della risorsa di simulazione per valutare il codice in base a, quindi questo è impostato su  **/content/** bylineas che è il punto in cui viene caricata la risorsa di contenuto del byline di simulazione.
   * **`Byline byline`** Crea un&#39;istanza del modello Sling Byline adattandolo dall&#39;oggetto di richiesta mock.
   * **`String actual`** richiama il metodo che stiamo testando,  `getName()`, sull&#39;oggetto Byline Sling Model.
   * **`assertEquals`** afferma che il valore previsto corrisponde al valore restituito dall&#39;oggetto del modello Sling della riga precedente. Se questi valori non sono uguali, il test avrà esito negativo.

1. Esegui il test... e non riesce con un `NullPointerException`.

   Tieni presente che il test NON ha esito negativo perché non abbiamo mai definito una proprietà `name` nel JSON fittizio, il che causerà il mancato funzionamento del test, tuttavia l’esecuzione del test non è arrivata a quel punto! Il test non riesce a causa di un `NullPointerException` sull&#39;oggetto byline stesso.

1. In `BylineImpl.java`, se `@PostConstruct init()` genera un&#39;eccezione, impedisce la creazione di istanze del modello Sling e fa sì che l&#39;oggetto modello Sling sia nullo.

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   Si scopre che mentre il servizio OSGi ModelFactory viene fornito tramite `AemContext` (tramite il contesto Sling Apache), non tutti i metodi vengono implementati, incluso `getModelFromWrappedRequest(...)` che viene chiamato nel metodo `init()` di BylineImpl. Questo si traduce in un [AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html), che nel termine causa il mancato funzionamento di `init()` e l&#39;adattamento risultante di `ctx.request().adaptTo(Byline.class)` è un oggetto null.

   Dal momento che le schermate fornite non possono contenere il nostro codice, dobbiamo implementare noi stessi il contesto di scherno Per questo, possiamo utilizzare Mockito per creare un oggetto ModelFactory fittizio, che restituisce un oggetto Immagine simulato quando `getModelFromWrappedRequest(...)` viene richiamato su di esso.

   Poiché per creare un&#39;istanza del modello Sling Byline, questo contesto fittizio deve essere attivo, possiamo aggiungerlo al metodo `@Before setUp()` . È inoltre necessario aggiungere il tag `MockitoExtension.class` all&#39;annotazione `@ExtendWith` sopra la classe **BylineImplTest** .

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

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** contrassegna la classe Test Case da eseguire con l&#39; [estensione JUnit Jupiter ](https://www.javadoc.io/page/org.mockito/mockito-junit-jupiter/latest/org/mockito/junit/jupiter/MockitoExtension.html) Mockito, che consente l&#39;utilizzo delle annotazioni @Mock per definire oggetti fittizi a livello di Classe.
   * **`@Mock private Image`** crea un oggetto fittizio di tipo  `com.adobe.cq.wcm.core.components.models.Image`. Tieni presente che questo viene definito a livello di classe in modo che, se necessario, i metodi `@Test` possano modificarne il comportamento in base alle esigenze.
   * **`@Mock private ModelFactory`** crea un oggetto fittizio di tipo ModelFactory. Tieni presente che questa è una zanzara pura Mockito e non ha metodi implementati su di essa. Tieni presente che questo viene definito a livello di classe in modo che, se necessario, i metodi `@Test`possano modificarne il comportamento in base alle esigenze.
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** registra un comportamento fittizio per quando  `getModelFromWrappedRequest(..)` viene chiamato sull&#39;oggetto ModelFactory di simulazione. Il risultato definito in `thenReturn (..)` è quello di restituire l&#39;oggetto immagine fittizia. Questo comportamento viene richiamato solo quando: il primo parametro è uguale all&#39;oggetto di richiesta di `ctx`, il secondo parametro è qualsiasi oggetto Resource e il terzo parametro deve essere la classe Immagine dei componenti core. Accettiamo qualsiasi risorsa perché durante i nostri test imposteremo il `ctx.currentResource(...)` su varie risorse fittizie definite in **BylineImplTest.json**. Si noti che si aggiunge la rigorosità **lenient()** perché in seguito si desidera ignorare questo comportamento di ModelFactory.
   * **`ctx.registerService(..)`.** registra l&#39;oggetto ModelFactory in AemContext, con la classificazione di servizio più alta. Questo è necessario in quanto il ModelFactory utilizzato nel `init()` di BylineImpl viene inserito tramite il campo `@OSGiService ModelFactory model`. Affinché AemContext possa inserire **il nostro oggetto fittizio** che gestisce le chiamate a `getModelFromWrappedRequest(..)`, è necessario registrarlo come servizio di classificazione più alta di quel tipo (ModelFactory).

1. Esegui nuovamente il test e non riesce, ma questa volta il messaggio è chiaro perché non è riuscito.

   ![asserzione di errore del nome del test](assets/unit-testing/testgetname-failure-assertion.png)

   *testGetName() non riuscito a causa di un&#39;asserzione*

   Riceviamo un **AssertionError** che significa che la condizione di asserzione nel test non è riuscita e che indica che il valore **previsto è &quot;Jane Doe&quot;** ma il valore **effettivo è null**. Questo ha senso perché la proprietà &quot;**name&quot;** non è stata aggiunta alla definizione di risorsa **/content/byline** in **BylineImplTest.json**, quindi aggiungilo:

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

1. Esegui nuovamente il test e **`testGetName()`** ora passa!

   ![test name pass](assets/unit-testing/testgetname-pass.png)


## Verifica di getOccupations() {#testing-get-occupations}

Ok ottimo! Il nostro primo test è passato! Andiamo avanti e testiamo `getOccupations()`. Poiché l&#39;inizializzazione del contesto fittizio è stata eseguita nel metodo `@Before setUp()`, questo sarà disponibile per tutti i metodi `@Test` in questo caso di test, incluso `getOccupations()`.

Tenere presente che questo metodo deve restituire un elenco alfabetico delle occupazioni (decrescente) memorizzate nella proprietà occupazioni.

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

   * **`List<String> expected`** definire il risultato previsto.
   * **`ctx.currentResource`** imposta la risorsa corrente per valutare il contesto in base alla definizione della risorsa simulata in /content/byline. Questo assicura che il **BylineImpl.java** venga eseguito nel contesto della nostra risorsa fittizia.
   * **`ctx.request().adaptTo(Byline.class)`** Crea un&#39;istanza del modello Sling Byline adattandolo dall&#39;oggetto di richiesta mock.
   * **`byline.getOccupations()`** richiama il metodo che stiamo testando,  `getOccupations()`, sull&#39;oggetto Byline Sling Model.
   * **`assertEquals(expected, actual)`** l&#39;elenco previsto degli asseriti è lo stesso dell&#39;elenco effettivo.

1. Ricorda, proprio come **`getName()`** sopra, il **BylineImplTest.json** non definisce le occupazioni, pertanto questo test avrà esito negativo se eseguito, dal momento che `byline.getOccupations()` restituirà un elenco vuoto.

   Aggiorna **BylineImplTest.json** per includere un elenco di occupazioni e saranno impostati in ordine non alfabetico per garantire che i nostri test verifichino che le occupazioni siano ordinate alfabeticamente per **`getOccupations()`**.

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

1. Esegui il test, e di nuovo passiamo! Sembra che le occupazioni ordinate funzionino!

   ![Ottieni passaggio di occupazione](assets/unit-testing/testgetoccupations-pass.png)

   *testGetOccupations() pass*

## Test isEmpty() {#testing-is-empty}

Ultimo metodo di test **`isEmpty()`**.

Il test `isEmpty()` è interessante in quanto richiede test per diverse condizioni. Esaminando il metodo **BylineImpl.java** `isEmpty()`, è necessario verificare le seguenti condizioni:

* Restituisce true quando il nome è vuoto
* Restituisce true quando le occupazioni sono nulle o vuote
* Restituisce true quando l&#39;immagine è null o non ha un URL src
* Restituisce false se sono presenti nome, occupazioni e immagine (con un URL src)

A questo scopo, è necessario creare nuovi metodi di test, ogni test di una condizione specifica e nuove strutture di risorse di simulazione in `BylineImplTest.json` per eseguire questi test.

Questo controllo ci ha permesso di saltare i test per i casi in cui `getName()`, `getOccupations()` e `getImage()` sono vuoti, in quanto il comportamento previsto di tale stato viene testato tramite `isEmpty()`.

1. Il primo test verifica la condizione di un componente nuovo, che non ha proprietà impostate.

   Aggiungi una nuova definizione di risorsa a `BylineImplTest.json`, assegnandogli il nome semantico &quot;**empty**&quot;

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

   **`"empty": {...}`** definisci una nuova definizione di risorsa denominata &quot;empty&quot; che presenta solo un  `jcr:primaryType` e  `sling:resourceType`.

   Ricorda che `BylineImplTest.json` viene caricato in `ctx` prima dell’esecuzione di ciascun metodo di test in `@setUp`, pertanto questa nuova definizione di risorsa è immediatamente disponibile nei test in **/content/empty.**

1. Aggiorna `testIsEmpty()` come segue, impostando la risorsa corrente sulla nuova definizione di risorsa di simulazione &quot;**empty**&quot;.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   Esegui il test e assicurati che venga superato.

1. Quindi, crea un set di metodi per garantire che se uno dei punti dati richiesti (nome, occupazioni o immagine) è vuoto, `isEmpty()` restituisca true.

   Per ogni test, viene utilizzata una definizione discreta di risorsa di simulazione, aggiorna **BylineImplTest.json** con le definizioni aggiuntive di risorse per **without-name** e **without-occupations**.

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

   Crea i seguenti metodi di test per verificare ciascuno di questi stati.

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

   **`testIsEmpty()`** esegue il test rispetto alla definizione di risorsa fittizia vuota e afferma che  `isEmpty()` è true.

   **`testIsEmpty_WithoutName()`** esegue il test rispetto a una definizione di risorsa fittizia con occupazioni ma senza nome.

   **`testIsEmpty_WithoutOccupations()`** esegue il test rispetto a una definizione di risorsa fittizia con un nome ma senza occupazioni.

   **`testIsEmpty_WithoutImage()`** esegue il test rispetto a una definizione di risorsa fittizia con un nome e occupazioni, ma imposta l&#39;immagine fittizia affinché ritorni a null. Per verificare che l’oggetto Immagine restituito da questa chiamata sia Null, sovrascriviamo il comportamento `modelFactory.getModelFromWrappedRequest(..)`definito in `setUp()` . La funzione stub Mockito è rigida e non vuole codice duplicato. Pertanto, impostiamo la simulazione con le impostazioni **`lenient`** per notare esplicitamente che stiamo ignorando il comportamento nel metodo `setUp()` .

   **`testIsEmpty_WithoutImageSrc()`** esegue il test rispetto a una definizione di risorsa fittizia con un nome e occupazioni, ma imposta l&#39;immagine fittizia per restituire una stringa vuota quando  `getSrc()` viene richiamato.

1. Infine, scrivi un test per garantire che **isEmpty()** restituisca false quando il componente è configurato correttamente. Per questa condizione, possiamo riutilizzare **/content/byline** che rappresenta un componente Byline completamente configurato.

   ```java
   @Test
   public void testIsNotEmpty() {
       ctx.currentResource("/content/byline");
       when(image.getSrc()).thenReturn("/content/bio.png");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertFalse(byline.isEmpty());
   }
   ```

1. Ora esegui tutti i test di unità nel file BylineImplTest.java e controlla l&#39;output del rapporto di test Java.

![Tutte le prove](./assets/unit-testing/all-tests-pass.png)

## Esecuzione di unit test come parte della build {#running-unit-tests-as-part-of-the-build}

I test di unità eseguiti devono essere trasmessi come parte della build Maven. In questo modo tutti i test verranno superati correttamente prima della distribuzione di un&#39;applicazione. L’esecuzione degli obiettivi Maven, ad esempio il pacchetto o l’installazione, richiama automaticamente e richiede il passaggio di tutti i test di unità nel progetto.

```shell
$ mvn package
```

![pacchetto mvn riuscito](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

Allo stesso modo, se modifichiamo un metodo di test in modo che non funzioni, la build non riesce e segnala quale test non è riuscito e perché.

![errore del pacchetto mvn](assets/unit-testing/mvn-package-fail.png)

## Rivedi il codice {#review-the-code}

Visualizza il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd) oppure rivedi e distribuisci il codice localmente in sulla barra Git `tutorial/unit-testing-solution`.
