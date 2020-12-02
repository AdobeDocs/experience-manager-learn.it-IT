---
title: Usa master progetto in AEM
description: I project master semplificano notevolmente la gestione di utenti e team con AEM progetti.
version: 6.4, 6.5
feature: projects, users-and-groups
topics: administration, collaboration, performance
activity: use
audience: administrator, implementer, architect
doc-type: article
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 0%

---


# Usa master progetti

I project master semplificano notevolmente la gestione di utenti e team con [!DNL AEM Projects].

>[!VIDEO](https://video.tv.adobe.com/v/17740/?quality=9&learn=on)

Adesso, gli amministratori possono creare un **[!DNL Master Project]** e assegnare gli utenti a ruoli/autorizzazioni come parte di un team di progetto. I progetti possono essere creati da un progetto principale e erediteranno automaticamente l&#39;iscrizione Team. Questo offre diversi vantaggi:

* Riutilizzare i team esistenti tra più progetti
* Accelera la creazione dei progetti poiché i team non devono essere ricreati manualmente
* Gestione dell&#39;iscrizione Team da una posizione centrale ed eventuali aggiornamenti ai team vengono automaticamente ereditati da Progetti
* evita la creazione di ACL duplicati che possono causare problemi di prestazioni

[!DNL Master Projects] può essere creato nella cartella   Master in  [!UICONTROL AEM progetti]. Una volta creato [!DNL Master Project], questo verrà visualizzato come opzione insieme ai modelli disponibili nella procedura guidata quando vengono creati nuovi progetti.

[!DNL Project Masters] URL (istanza AEM Author locale):  [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## Elimina [!DNL Project Masters]

L&#39;eliminazione di un progetto principale determina la creazione di progetti derivati non utilizzabili.

Prima di eliminare un progetto principale, accertatevi che tutti i progetti derivati siano completati e rimossi da AEM. Prima di rimuovere i progetti derivati, assicurarsi di salvare eventuali dati di progetto richiesti. Una volta rimossi tutti i progetti derivati dalla AEM, il progetto principale può essere eliminato in modo sicuro.

## Contrassegna [!DNL Project Masters] come non attivo

Modificando lo stato del progetto principale in inattivo nelle proprietà del progetto, i progetti principali inattivi scompaiono dall&#39;elenco dei progetti principali.

Per mostrare i progetti principali inattivi, attivate il pulsante &quot;Mostra attivo&quot; nella barra superiore (accanto all’interruttore di visualizzazione dell’elenco). Per rendere nuovamente attivo il progetto inattivo, è sufficiente selezionare il progetto principale inattivo, modificare le proprietà del progetto e impostarlo di nuovo per essere attivo.

## Comprensione di [!DNL Project Masters]

![Vista tecnica master progetto](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] definire un set di gruppi di utenti AEM (proprietari, editor e osservatori) e consentire ai progetti derivati di fare riferimento a tali gruppi di utenti definiti a livello centrale e di riutilizzarli.

Questo riduce il numero complessivo di gruppi di utenti richiesti in AEM. Prima di [!DNL Project Masters], ogni progetto creava 3 gruppi di utenti con le relative ACE per applicare le autorizzazioni, pertanto 100 progetti ottenevano 300 gruppi di utenti. Project Master consente a qualsiasi numero di progetti di riutilizzare gli stessi 3 gruppi, sempre che l&#39;appartenenza condivisa sia allineata ai requisiti aziendali in tutto il progetto.
