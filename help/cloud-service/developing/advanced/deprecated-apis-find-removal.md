---
title: Trovare e rimuovere le API obsolete in AEM as a Cloud Service
description: Scopri come trovare e rimuovere le API obsolete in AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
role: Developer, Architect
level: Beginner
doc-type: tutorial
duration: null
jira: KT-20288
thumbnail: KT-20288.png
last-substantial-update: 2026-02-09T00:00:00Z
source-git-commit: 6c5b911d1d59573338dd1a30eb95289bc1339f19
workflow-type: tm+mt
source-wordcount: '522'
ht-degree: 3%

---


# Trovare e rimuovere le API obsolete in AEM as a Cloud Service

Scopri come trovare e rimuovere le API obsolete in AEM as a Cloud Service.

## Panoramica

AEM as a Cloud Service **Centro operativo** ti notifica circa _API obsolete_ nel tuo progetto. Per ottenere le funzioni più recenti, gli aggiornamenti di sicurezza e le distribuzioni senza problemi del codice in AEM as a Cloud Service tramite le pipeline di Cloud Manager, rimuovi le API obsolete dal progetto.

In questo tutorial imparerai a trovare e rimuovere le API obsolete nel tuo ambiente AEM as a Cloud Service utilizzando il [plug-in Maven di AEM Analyzer](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md).

## Come trovare API obsolete

Per trovare API obsolete nel progetto AEM as a Cloud Service, segui la procedura riportata di seguito.

1. **Usa il plug-in Maven più recente di AEM Analyzer**

   Nel progetto AEM, utilizza la versione più recente del [plug-in Maven di AEM Analyzer](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md).

   - Nel `pom.xml` principale, la versione del plug-in è in genere dichiarata. Confronta la tua versione con l&#39;ultima [versione rilasciata](https://mvnrepository.com/artifact/com.adobe.aem/aemanalyser-maven-plugin).

     ```xml
     ...
     <aemanalyser.version>1.6.14</aemanalyser.version> <!-- Latest released version as of 09-Feb-2026 -->
     ...
     <!-- AEM Analyser Plugin -->
     <plugin>
         <groupId>com.adobe.aem</groupId>
         <artifactId>aemanalyser-maven-plugin</artifactId>
         <version>${aemanalyser.version}</version>
         <extensions>true</extensions>
     </plugin>
     ...
     ```

   - Il plug-in verifica la disponibilità dell’ultima versione di AEM SDK. Usa la versione più recente di AEM SDK nel file `pom.xml` del tuo progetto. Questo aiuta a rendere visibili le API obsolete come avvisi IDE.

     ```xml
     ...
     <aem.sdk.api>2026.2.24288.20260204T121510Z-260100</aem.sdk.api> <!-- Latest available AEM SDK version as of 09-Feb-2026 -->
     ...
     ```

   - Assicurarsi che il modulo `all` esegua il plug-in nella fase `verify`.

     ```xml
     ...
     <build>
         <plugins>
             ...
             <plugin>
                 <groupId>com.adobe.aem</groupId>
                 <artifactId>aemanalyser-maven-plugin</artifactId>
                 <extensions>true</extensions>
                 <executions>
                     <execution>
                         <id>analyse-project</id>
                         <phase>verify</phase>
                         <goals>
                             <goal>project-analyse</goal>
                         </goals>
                     </execution>
                 </executions>
             </plugin>
             ...
         </plugins>
     </build>
     ...
     ```

2. **Esegui una compilazione e verifica la presenza di avvisi**

   Quando esegui `mvn clean install`, l&#39;analizzatore segnala le API obsolete come **[messaggi DI AVVISO]** nell&#39;output. Ad esempio:

   ```shell
   ...
   [WARNING] The analyser found the following warnings for author and publish :
   [WARNING] [region-deprecated-api] com.adobe.aem.guides:aem-guides-wknd.core:4.0.5-SNAPSHOT: Usage of deprecated package found : org.apache.commons.lang : Commons Lang 2 is in maintenance mode. Commons Lang 3 should be used instead. Deprecated since 2021-04-30 For removal : 2021-12-31 (com.adobe.aem.guides:aem-guides-wknd.all:4.0.5-SNAPSHOT)
   ...
   ```

   È facile ignorare questi messaggi quando ci si concentra sul successo o sul fallimento della build.

3. **Cancella l&#39;elenco delle API obsolete**

   Anche il passaggio precedente fornisce le stesse informazioni. Eseguire tuttavia la fase `verify` sul modulo `all` per visualizzare tutti i **[MESSAGGI DI AVVISO]** in un&#39;unica posizione. Ad esempio:

   ```shell
   $ mvn clean verify -pl all
   ```

   I messaggi **[WARNING]** nell&#39;output di compilazione elencano le API obsolete nel progetto.

## Come rimuovere le API obsolete

AEM Analyzer segnala **cosa** è obsoleto e fornisce **consigli** su come correggerlo. Tuttavia, utilizza la tabella seguente per scegliere l’azione giusta e, quando hai bisogno di ulteriori dettagli, segui la documentazione collegata.

### Strategia di correzione API obsoleta

| Tipo di avviso analizzatore | Cosa indica | Azione consigliata | Riferimento |
| --------------------- | ----------------- | ------------------ | --------- |
| API AEM obsoleta | L’API deve essere rimossa da AEM as a Cloud Service | Sostituisci l’utilizzo con l’API pubblica supportata | [Guida alla rimozione API](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| Pacchetto o classe AEM obsoleti | Il pacchetto o la classe non è più supportato | Refactoring del codice per utilizzare l&#39;alternativa consigliata | [API obsolete](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#aem-apis) |
| Libreria di terze parti obsoleta | La libreria non sarà supportata negli SDK futuri | Aggiornare la dipendenza e l’utilizzo del refactoring | [Linee guida generali](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| Pattern Sling/OSGi obsoleti | Annotazioni o API legacy rilevate | Migrare alle API Sling e OSGi moderne | [Rimozione dei pattern Sling / OSGi](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| Rimozione pianificata (data futura) | L’API funziona ancora, ma la rimozione viene imposta in un secondo momento | Pianifica la pulizia prima dell’applicazione della pipeline | [Note sulla versione](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/release-notes/home) |

### Indicazioni pratiche

- Considera gli avvisi dell&#39;analizzatore come **errori futuri della pipeline**, non come messaggi facoltativi.
- Correggi le API obsolete localmente utilizzando la **versione più recente di AEM SDK**.
- Mantenere pulito l’output dell’analizzatore per evitare problemi durante i futuri aggiornamenti di AEM.

La correzione anticipata delle API obsolete mantiene il progetto **sicuro per l&#39;aggiornamento e pronto per la distribuzione**.

## Risorse aggiuntive

- [Plug-in Maven di AEM Analyzer](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md)
- [Funzioni e API obsolete e rimosse](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance)

