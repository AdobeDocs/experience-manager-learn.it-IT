---
title: Utilizzo del reporting delle transazioni in AEM Forms
description: I rapporti sulle transazioni in AEM Forms ti consentono di tenere un conteggio di tutte le transazioni effettuate a partire da una data specifica nella distribuzione AEM Forms.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 36c38cb6-6f6a-4328-abf5-7a30059b66ce
last-substantial-update: 2019-03-20T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 1%

---

# Utilizzo del reporting delle transazioni in AEM Forms{#using-transaction-reporting-in-aem-forms}

Con AEM Forms 6.4.1 è stato introdotto il reporting delle transazioni per acquisire il numero di invii dei moduli, il rendering dei documenti utilizzando document services e il rendering delle comunicazioni interattive (canali web e di stampa). Questa funzionalità è principalmente destinata ai clienti che desiderano concedere in licenza il software in base al numero di invii e/o documenti di moduli sottoposti a rendering. Questa funzionalità è attualmente disponibile solo sullo stack AEM Forms OSGI.

## Abilitazione del reporting delle transazioni {#enabling-transaction-reporting}

Per impostazione predefinita, la registrazione delle transazioni è disabilitata. Per abilitare la registrazione delle transazioni, segui i passaggi indicati di seguito:

* [Apri configMgr](http://localhost:4502/system/console/configMgr)
* Cerca &quot;Report transazioni Forms&quot;
* Selezionare la casella di controllo &quot;Registra transazioni&quot;
* Salva le modifiche

Una volta abilitato il reporting delle transazioni, è possibile inviare Adaptive Forms, generare documenti utilizzando document services o eseguire il rendering di documenti di comunicazione interattiva per visualizzare il reporting delle transazioni in azione.

## Visualizzazione del rapporto transazioni {#viewing-transaction-report}

Per visualizzare il rapporto sulle transazioni, accedi ad AEM Forms come amministratore. Solo i membri del gruppo fd-Administrator possono visualizzare il rapporto sulle transazioni.

Seleziona Strumenti | Forms | Visualizza rapporto transazioni

o visualizzare il rapporto sulla transazione facendo clic su [qui](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)

![Reporting sulle transazioni](assets/transactionreporting.gif)

Nella schermata precedente Documento elaborato è il numero di documenti generati utilizzando document services. Documenti resi indica il numero di documenti di comunicazione interattiva (Web e stampa) sottoposti a rendering. Forms Inviato indica il numero di invii di moduli adattivi.

Una transazione rimane nel buffer per un periodo specificato (tempo buffer scaricamento + tempo di replica inversa). Per impostazione predefinita, il conteggio delle transazioni impiega circa 90 secondi per riflettere nel report delle transazioni.

Azioni come l’invio di un modulo PDF, l’utilizzo dell’interfaccia utente dell’agente per visualizzare in anteprima una comunicazione interattiva o l’utilizzo di metodi di invio di moduli non standard non vengono contabilizzate come transazioni. AEM Forms fornisce un’API per registrare tali transazioni. Chiama l&#39;API dalle tue implementazioni personalizzate per registrare una transazione.

Se visualizzi il rapporto sulle transazioni nell’istanza di authoring, assicurati che la replica inversa sia configurata su tutte le istanze di pubblicazione.

Per ulteriori informazioni sulla generazione di rapporti sulle transazioni [fai clic qui](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)
