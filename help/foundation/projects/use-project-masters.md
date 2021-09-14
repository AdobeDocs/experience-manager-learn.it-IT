---
title: Come utilizzare i master di progetto in AEM
description: I project manager semplificano notevolmente la gestione di utenti e team con i progetti AEM.
version: 6.4, 6.5, Cloud Service
topic: Content Management, Collaboration
feature: Projects
level: Intermediate
role: User
kt: 256
thumbnail: 17740.jpg
exl-id: 78ff62ad-1017-4a02-80e9-81228f9e01eb
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 0%

---

# Usa master progetti

I project manager semplificano notevolmente la gestione di utenti e team con [!DNL AEM Projects].

>[!VIDEO](https://video.tv.adobe.com/v/17740/?quality=12&learn=on)

Gli amministratori possono ora creare un **[!DNL Master Project]** e assegnare gli utenti a ruoli/autorizzazioni come parte di un team di progetto. I progetti possono essere creati da un progetto principale e ereditano automaticamente l&#39;iscrizione al team. Questo offre diversi vantaggi:

* Riutilizzare i team esistenti in più progetti
* Consente di accelerare la creazione dei progetti in quanto i team non devono essere ricreati manualmente
* L’iscrizione a Gestione team da una posizione centrale e gli eventuali aggiornamenti a Team vengono ereditati automaticamente da Progetti
* evita la creazione di ACL duplicati che possono causare problemi di prestazioni

[!DNL Master Projects] può essere creato nella cartella   Mastersfolder in  [!UICONTROL AEM Projects] (Progetti). Una volta creato un progetto principale, questo viene visualizzato come opzione insieme ai modelli disponibili nella procedura guidata quando vengono creati nuovi progetti.

[!DNL Project Masters] URL (istanza AEM Author locale):  [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## Elimina [!DNL Project Masters]

Se si elimina un progetto principale, i progetti derivati non sono utilizzabili.

Prima di eliminare un progetto principale, assicurati che tutti i progetti derivati siano completati e rimossi da AEM. Assicurati di salvare i dati di progetto richiesti prima di rimuovere i progetti derivati. Una volta rimossi tutti i progetti derivati da AEM, il progetto principale può essere eliminato in modo sicuro.

## Contrassegna [!DNL Project Masters] come inattivo

Modificando lo stato del progetto principale in inattivo nelle proprietà del progetto, i progetti master inattivi scompaiono dall’elenco dei progetti principali.

Per mostrare i progetti master inattivi, attiva il pulsante del filtro &quot;Mostra attivo&quot; nella barra superiore (accanto all’interruttore di visualizzazione dell’elenco). Per attivare nuovamente il progetto inattivo, è sufficiente selezionare il progetto principale inattivo, modificare le proprietà del progetto e impostarlo nuovamente per l’attivazione.

## Comprendere [!DNL Project Masters]

![Vista tecnica dei master del progetto](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] consente di definire un set di gruppi di utenti AEM (proprietari, editor e osservatori) e di consentire ai progetti derivati di fare riferimento e riutilizzare tali gruppi di utenti definiti a livello centrale.

Questo riduce il numero complessivo di gruppi di utenti richiesti in AEM. Prima di [!DNL Project Masters], ogni progetto ha creato 3 gruppi di utenti con gli ACE di accompagnamento per applicare il processo di autorizzazione, così 100 progetti hanno prodotto 300 gruppi di utenti. I project manager consentono a qualsiasi numero di progetti di riutilizzare gli stessi tre gruppi, purché l&#39;appartenenza condivisa sia allineata ai requisiti aziendali in tutto il progetto.
