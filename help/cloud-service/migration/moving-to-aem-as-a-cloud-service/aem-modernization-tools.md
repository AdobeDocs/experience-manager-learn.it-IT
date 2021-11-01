---
title: Utilizzo degli strumenti di modernizzazione AEM per il passaggio a AEM as a Cloud Service
description: Scopri come AEM gli strumenti di modernizzazione vengono utilizzati per aggiornare un progetto AEM esistente e i contenuti per renderli AEM compatibili con l’as a Cloud Service.
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8629
thumbnail: 336965.jpeg
exl-id: 310f492c-0095-4015-81a4-27d76f288138
source-git-commit: 3657e7798774f9cc673ff6ccd8af1a555b1d4013
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 1%

---


# Strumenti di modernizzazione AEM

Scopri come AEM gli strumenti di modernizzazione vengono utilizzati per aggiornare un contenuto AEM Sites esistente in modo che sia AEM compatibile con l’as a Cloud Service e in linea con le best practice.

>[!VIDEO](https://video.tv.adobe.com/v/336965/?quality=12&learn=on)

## Utilizzo degli strumenti di modernizzazione AEM

![Ciclo di vita degli strumenti di modernizzazione AEM](./assets/aem-modernization-tools.png)

Gli strumenti di modernizzazione AEM convertono automaticamente le pagine AEM esistenti composte da modelli statici, componenti di base e parsys legacy, per utilizzare approcci moderni quali modelli modificabili, componenti AEM WCM core e contenitori di layout.

### Attività chiave

+ Clona la produzione AEM 6.x per eseguire gli strumenti di modernizzazione AEM contro
+ Scarica e installa la [strumenti di modernizzazione AEM più recenti](https://github.com/adobe/aem-modernize-tools/releases/latest) sul clone di produzione AEM 6.x tramite Gestione pacchetti

+ [Convertitore struttura pagina](https://opensource.adobe.com/aem-modernize-tools/pages/tools/page-structure.html) aggiorna il contenuto della pagina esistente da un modello statico a un modello modificabile mappato utilizzando i contenitori layout
   + Definire le regole di conversione utilizzando la configurazione OSGi
   + Esegui Convertitore struttura pagina rispetto alle pagine esistenti

+ [Convertitore componente](https://opensource.adobe.com/aem-modernize-tools/pages/tools/component.html) aggiorna il contenuto della pagina esistente da un modello statico a un modello modificabile mappato utilizzando i contenitori layout
   + Definire le regole di conversione tramite le definizioni dei nodi JCR/XML
   + Esegui lo strumento Convertitore componenti rispetto alle pagine esistenti

+ [Importatore di regole](https://opensource.adobe.com/aem-modernize-tools/pages/tools/policy-importer.html) crea criteri dalla configurazione di progettazione
   + Definire le regole di conversione utilizzando le definizioni dei nodi JCR/XML
   + Esegui Importazione criteri rispetto alle definizioni di progettazione esistenti
   + Applicare criteri importati a componenti e contenitori AEM

+ [Convertitore finestra di dialogo](https://opensource.adobe.com/aem-modernize-tools/pages/tools/dialog.html) converte le finestre di dialogo dei componenti basati su Classic (ExtJS) e CoralUI 2 in finestre di dialogo basate su CoralUI 3 TouchUI.
   + Esegui lo strumento Convertitore finestra di dialogo rispetto alle finestre di dialogo esistenti basate sull’interfaccia utente ExtJS o Coral2
   + Sincronizza nuovamente le finestre di dialogo convertite nell’archivio Git

### Altre risorse

+ [Download degli strumenti di modernizzazione AEM](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [Documentazione sugli strumenti di modernizzazione AEM](https://opensource.adobe.com/aem-modernize-tools/)
+ [AEM Gems - Introduzione alla suite di modernizzazione AEM](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)
