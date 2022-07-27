---
source-git-commit: 10032375a23ba99f674fb59a6f6e48bf93908421
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 0%

---



# Implementazioni senza titolo AEM

AEM le implementazioni client headless si presentano in molti modi; SPA in hosting AEM, SPA esterno o sito web, app mobile o persino processo server-to-server.

A seconda del client e della modalità di distribuzione, le distribuzioni senza intestazione AEM hanno considerazioni diverse.

## Architettura del servizio AEM

Prima di esaminare le considerazioni relative all’implementazione, è fondamentale comprendere AEM’architettura logica e la separazione e i ruoli dei livelli di servizio di AEM as a Cloud Service. AEM as a Cloud Service è composto da due servizi logici:

+ __AEM Author__ è il servizio in cui i team creano, collaborano e pubblicano frammenti di contenuto (e altre risorse)
+ __Pubblicazione AEM__ è il servizio in cui i frammenti di contenuto pubblicati (e altre risorse) vengono replicati per il consumo generale

I client AEM headless che operano in una capacità di produzione generalmente interagiscono con AEM Publish, che contiene il contenuto approvato e pubblicato. Il client che interagisce con AEM Author deve tenere in particolare considerazione la protezione di AEM Author per impostazione predefinita, richiedendo l’autorizzazione per tutte le richieste e può contenere contenuti incompleti o non approvati.

## Implementazioni client headless

+ [App a pagina singola](./spa.md)
+ [Componente Web/JS](./web-component.md)
+ [App mobile](./mobile.md)
+ [Server-to-server](./server-to-server.md)

