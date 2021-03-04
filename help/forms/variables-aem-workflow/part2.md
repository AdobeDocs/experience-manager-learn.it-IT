---
title: Variabili nel flusso di lavoro AEM[Parte2]
seo-title: Variabili nel flusso di lavoro AEM[Parte2]
description: Utilizzo di variabili di tipo xml,json,arraylist,document nel flusso di lavoro aem
seo-description: Utilizzo di variabili di tipo xml,json,arraylist,document nel flusso di lavoro aem
feature: Flusso di lavoro
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: Sviluppo
role: Developer (Sviluppatore)
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '300'
ht-degree: 1%

---

# Variabili di tipo JSON nel flusso di lavoro AEM

A partire da AEM Forms 6.5, ora possiamo creare variabili di tipo JSON nel flusso di lavoro AEM. In genere si creano variabili di tipo JSON se si inviano moduli adattivi basati su schema JSON a un flusso di lavoro AEM o si desidera archiviare i risultati di un’operazione di chiamata a un modello dati modulo. Il video seguente illustra i passaggi necessari per creare e utilizzare una variabile di tipo JSON nel flusso di lavoro AEM

**Se utilizzi AEM Forms 6.5.0**

Quando crei una variabile di tipo JSON per acquisire i dati inviati nel modello di flusso di lavoro, non associare lo schema JSON alla variabile. Questo perché quando si invia un modulo adattivo basato sullo schema JSON, i dati inviati non sono conformi allo schema JSON. I dati dei reclami dello schema JSON sono racchiusi nell&#39;elemento afData.afBoundData.data .

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**Se utilizzi AEM Forms 6.5.1 e versioni successive**

Puoi mappare lo schema con la variabile di tipo JSON nel modello di flusso di lavoro. Puoi quindi utilizzare il browser schema per mappare gli elementi dello schema con le variabili stringa/numero nel modello di flusso di lavoro

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

Per far funzionare le risorse sul sistema, effettua le seguenti operazioni:

* [Scarica e importa le risorse in AEM utilizzando il gestore di pacchetti](assets/jsonandstringvariable.zip)
* [Esplora il ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) modello di flusso di lavoro per comprendere le variabili utilizzate nel flusso di lavoro
* [Configurare il servizio e-mail](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Apri il modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* Compila i dettagli e invia il modulo
