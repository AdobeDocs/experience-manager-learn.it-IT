---
title: Utilizzo del reporting delle transazioni in AEM Forms
seo-title: Utilizzo del reporting delle transazioni in AEM Forms
description: I rapporti sulle transazioni in AEM Forms ti consentono di tenere un conteggio di tutte le transazioni effettuate da una data specificata nell’implementazione di AEM Forms.
seo-description: I rapporti sulle transazioni in AEM Forms ti consentono di tenere un conteggio di tutte le transazioni effettuate da una data specificata nell’implementazione di AEM Forms.
uuid: e6133f7e-c79c-4006-89e7-3bebf7b8229e
feature: moduli adattivi
topics: developing
audience: administrator
doc-type: article
activity: setup
version: 6.4.1,6.5
discoiquuid: 1abdf07a-b9f0-4c58-a1c6-08ae57db2014
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 0%

---


# Utilizzo del reporting delle transazioni in AEM Forms{#using-transaction-reporting-in-aem-forms}

Con AEM Forms 6.4.1 è stato introdotto il reporting delle transazioni per acquisire il numero di invii dei moduli, il rendering dei documenti utilizzando document services e il rendering delle comunicazioni interattive (canali web e di stampa). Questa funzionalità è principalmente destinata ai clienti che desiderano concedere la licenza per il software in base al numero di invii e/o documenti di cui è stato eseguito il rendering. Questa funzionalità è attualmente disponibile solo sullo stack OSGI di AEM Forms.

## Abilitazione del reporting delle transazioni {#enabling-transaction-reporting}

Per impostazione predefinita, la registrazione delle transazioni è disabilitata. Per abilitare la registrazione delle transazioni, segui i passaggi indicati di seguito:

* [Apri configMgr](http://localhost:4502/system/console/configMgr)
* Cerca &quot;Report sulle transazioni dei moduli&quot;
* Selezionare la casella di controllo &quot;Registra transazioni&quot;
* Salva le modifiche

Una volta abilitato il reporting delle transazioni, è possibile inviare Moduli adattivi, generare documenti utilizzando document services o eseguire il rendering di documenti di comunicazione interattiva per visualizzare il reporting delle transazioni in azione.

## Visualizzazione del rapporto sulle transazioni {#viewing-transaction-report}

Per visualizzare il rapporto sulle transazioni, accedi ad AEM Forms come amministratore. Solo i membri del gruppo fd-Administrator possono visualizzare il rapporto sulle transazioni.

Seleziona Strumenti | Moduli | Visualizza rapporto transazioni

o visualizzare il rapporto sulle transazioni facendo clic su [qui](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)

![Reporting sulle transazioni](assets/transactionreporting.gif)

Nella schermata precedente Documento elaborato è il numero di documenti generati utilizzando document services. Documenti resi indica il numero di documenti di comunicazione interattiva (Web e stampa) sottoposti a rendering. Moduli inviati indica il numero di invii di moduli adattivi.

Una transazione rimane nel buffer per un periodo specificato (tempo buffer scaricamento + tempo di replica inversa). Per impostazione predefinita, il conteggio delle transazioni impiega circa 90 secondi per riflettere nel report delle transazioni.

Azioni come l’invio di un modulo PDF, l’utilizzo dell’interfaccia utente dell’agente per visualizzare in anteprima una comunicazione interattiva o l’utilizzo di metodi di invio di moduli non standard non vengono contabilizzate come transazioni. AEM Forms fornisce un’API per registrare tali transazioni. Chiama l&#39;API dalle tue implementazioni personalizzate per registrare una transazione.

Se visualizzi il rapporto sulle transazioni nell’istanza di authoring, assicurati che la replica inversa sia configurata su tutte le istanze di pubblicazione.

Per ulteriori informazioni sul reporting delle transazioni [fare clic qui](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)

