---
title: Come utilizzare i progetti principali in AEM
description: Project Masters semplifica notevolmente la gestione di utenti e team con AEM Projects.
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Content Management, Collaboration
feature: Projects
level: Intermediate
role: User
jira: KT-256
thumbnail: 17740.jpg
doc-type: Feature Video
exl-id: 78ff62ad-1017-4a02-80e9-81228f9e01eb
duration: 260
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---

# Usa progetto principale

Project Masters semplifica notevolmente la gestione di utenti e team con [!DNL AEM Projects].

>[!VIDEO](https://video.tv.adobe.com/v/3410331?quality=12&learn=on&captions=ita)

Gli amministratori ora possono creare un **[!DNL Master Project]** e assegnare gli utenti a ruoli/autorizzazioni come parte di un team di progetto. I progetti possono essere creati da un progetto principale ed ereditano automaticamente l’iscrizione Team. Questo offre diversi vantaggi:

* Riutilizzare i team esistenti in più progetti
* Accelera la creazione dei progetti in quanto non è necessario ricreare i team a mano
* Gestisci l’iscrizione al Team da una posizione centrale; eventuali aggiornamenti ai Team vengono ereditati automaticamente dai Progetti
* evita la creazione di ACL duplicati che possono causare problemi di prestazioni

È possibile creare [!DNL Master Projects] nella cartella [!UICONTROL Masters] in [!UICONTROL AEM Projects]. Una volta creato, il progetto principale viene visualizzato come opzione insieme ai modelli disponibili nella procedura guidata al momento della creazione di nuovi progetti.

URL [!DNL Project Masters] (istanza Autore AEM locale): [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## Elimina [!DNL Project Masters]

L’eliminazione di un progetto principale determina progetti derivati inutilizzabili.

Prima di eliminare un progetto principale, accertati che tutti i progetti derivati siano stati completati e rimossi da AEM. Prima di rimuovere i progetti derivati, assicurati di salvare tutti i dati di progetto necessari. Una volta rimossi tutti i progetti derivati da AEM, il progetto principale può essere eliminato in modo sicuro.

## Contrassegna [!DNL Project Masters] come inattivo

Modificando lo stato del progetto principale in inattivo nelle proprietà del progetto, i progetti principali inattivi scompaiono dall&#39;elenco dei progetti principali.

Per visualizzare i progetti principali inattivi, attiva/disattiva il pulsante di filtro &quot;mostra attivo&quot; nella barra superiore (accanto all’interruttore di visualizzazione dell’elenco). Per rendere nuovamente attivo il progetto inattivo, è sufficiente selezionare il progetto principale inattivo, modificare le proprietà del progetto e impostarlo nuovamente come attivo.

## Comprendere [!DNL Project Masters]

![Visualizzazione tecnica progetti principali](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] consente di definire un set di gruppi di utenti AEM (proprietari, editor e osservatori) e di consentire ai progetti derivati di fare riferimento e riutilizzare tali gruppi di utenti definiti a livello centrale.

Questo riduce il numero complessivo di gruppi di utenti richiesti in AEM. Prima di [!DNL Project Masters], ogni progetto creava 3 gruppi di utenti con le ACE associate per applicare il rilascio delle autorizzazioni. Di conseguenza, 100 progetti generavano 300 gruppi di utenti. Project Masters consente a un numero qualsiasi di progetti di riutilizzare gli stessi tre gruppi, supponendo che l&#39;appartenenza condivisa sia allineata ai requisiti aziendali del progetto.
