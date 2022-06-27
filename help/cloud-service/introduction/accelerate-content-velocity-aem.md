---
title: Accelerazione della velocità dei contenuti con i sistemi di stile AEM
description: Scopri come utilizzare i sistemi di stile AEM per consentire a designer, autori di contenuti e sviluppatori di creare e distribuire esperienze alla velocità e alle dimensioni che i clienti si aspettano.
solution: Experience Manager
exl-id: 449cd133-6ab6-456e-a0ad-30e3dea9b75b
source-git-commit: 471f0fe940abb8241428beb14896d83e140136b3
workflow-type: tm+mt
source-wordcount: '830'
ht-degree: 0%

---

# Accelerazione della velocità dei contenuti con i sistemi di stile AEM

Questo articolo spiega come utilizzare i sistemi in stile AEM per consentire a designer, autori di contenuti e sviluppatori di creare e distribuire esperienze alla velocità e alla scala previste dai clienti.

## Panoramica

I sistemi di stile AEM hanno quattro vantaggi principali:

* Gli autori di modelli possono definire le classi di stile nel criterio del contenuto di un componente o di una pagina
* Gli autori di contenuti possono selezionare gli stili da applicare a un’intera pagina o quando si modifica un componente in una pagina
* I componenti e i modelli sono resi più flessibili grazie alla possibilità per gli autori di eseguire il rendering di varianti visive alternative
* La necessità di sviluppare un componente personalizzato e/o finestre di dialogo complesse per presentare varianti di componenti viene ridotta o eliminata completamente

## Configurazione e utilizzo iniziali

La configurazione in 5 passaggi è molto simile a un flusso di lavoro standard per lo sviluppo di componenti.

| **Leadership** | **Designer** | **Sviluppatore/Architetto** | **Autore del modello** | **Autore del contenuto** |
| --- | --- | --- | --- | --- |
| Determina il contenuto e gli obiettivi del componente | Determina la presentazione visiva ed esperienziale del contenuto | Sviluppa CSS e JS per supportare l’esperienza; definisce e fornisce i nomi delle classi da utilizzare | Configura i criteri dei modelli per i componenti con stili aggiungendo i nomi delle classi CSS definiti dagli sviluppatori. Per ogni stile è necessario utilizzare nomi descrittivi. | Durante l’authoring delle pagine, applica gli stili in base alle esigenze per ottenere l’aspetto desiderato |

Anche se questa è la configurazione iniziale, molti dei nostri clienti hanno raggiunto un’ulteriore agilità semplificando questo processo, ad esempio caricando i propri CSS nel DAM, che consente gli aggiornamenti agli stili senza la necessità di distribuzione. Altri clienti hanno un set completo di classi di utilità, che consente loro di sviluppare componenti e stili che possono quindi essere utilizzati senza implementazione o sviluppo.

I sistemi di stile sono caratterizzati da alcuni gusti diversi:

1. Stili di layout

   * Modifiche multifacet alla progettazione e al layout

   * Utilizzato per rendering ben definito e identificabile

1. Stili di visualizzazione
   * Variazioni minori che non cambiano la natura fondamentale dello stile

   * Ad esempio, modifica della combinazione di colori, del font, dell’orientamento dell’immagine, ecc.

1. Stili informativi

   * Mostra/nascondi campi

>[!NOTE]
>
>Per una demo di queste funzioni, si consiglia di guardare il nostro [Webinar Customer Success](https://adobecustomersuccess.adobeconnect.com/pob610c9mffjmp4/) con Will Brisbane e Joseph Van Buskirk.

## Best practice

* Solidifica prima lo stile predefinito
   * Layout e visualizzazione del componente quando viene rilasciato sulla pagina prima dell’applicazione di sistemi di stile
   * Questa deve essere la rappresentazione più utilizzata
* Prova a mostrare solo le opzioni di stile che hanno un effetto quando possibile
   * Se sono esposte combinazioni inefficaci, assicurati che non causino effetti negativi
   * Ad esempio, uno stile di layout che determina la posizione dell’immagine ed è accompagnato da uno stile di visualizzazione inefficace che controlla la posizione dell’immagine
* Opzione per gli stili di layout rispetto agli stili di visualizzazione combinati
   * Riduce il numero di permutazioni che devono essere controllate dalla qualità
   * Garantisce il rispetto degli standard del marchio
   * Authoring semplificato per gli autori di contenuti
   * Consente di creare un’identità del marchio del sito coerente
* Conservare con stili combinati
   * Sia tra le categorie sia all’interno di esse
* Allocare il tempo necessario per testare accuratamente gli stili combinati
   * Aiuta ad evitare effetti indesiderati
* Ridurre al minimo il numero di opzioni e permutazioni di stile
   * Troppe opzioni possono portare a una mancanza di coerenza del marchio per aspetto e sensazione
   * Può creare confusione per gli autori di contenuti su cui le combinazioni sono necessarie per ottenere l’effetto desiderato
   * Aumenta le permutazioni che devono essere controllate dalla qualità
* Utilizzare etichette e categorie di stile facili da usare per le aziende
   * &quot;Blu&quot; e &quot;Rosso&quot; invece di &quot;primario&quot; e &quot;secondario&quot;
   * &quot;Carta&quot; e &quot;Eroe&quot; invece di &quot;Variazione A&quot; e &quot;Variazione B&quot;
   * Ciò può essere più generalità per alcuni clienti; il team addetto al design, il business team e il team addetto ai contenuti hanno familiarità con i colori primari e secondari o con quali varianti stanno testando. Ma per la flessibilità e per ogni potenziale cambiamento futuro, usare termini specifici può essere più efficiente.

## Principali aspetti

I sistemi di stile riducono la necessità di finestre di dialogo complesse ma non sostituiscono le finestre di dialogo. Sono utili per semplificare le cose, ma in alcuni casi può essere utile utilizzare le proprietà o la finestra di dialogo del componente, anziché creare un sistema di stili per questo.

Possono semplificare i processi dal punto di vista dello sviluppo. È possibile ottenere più look dello stesso contenuto con un unico sistema di stili. Allo stesso modo, dal punto di vista dell&#39;authoring, invece di formare gli autori e gli autori che devono ricordare quale componente usare in quale palazzo, è possibile accelerare la velocità di authoring.

Le cose sono semplicemente più pulite. Il HTML all’interno dei componenti core è altamente dettagliato. Tutto questo a livello di CSS rende le build dei componenti più veloci e anche il codice è più pulito.

Infine, l&#39;uso dei Sistemi di Stile è più arte che scienza. Come accennato, sono disponibili diverse best practice, ma puoi scegliere in modo flessibile come personalizzare la configurazione dell’organizzazione.

Per ulteriori informazioni, consulta la nostra [Webinar Customer Success](https://adobecustomersuccess.adobeconnect.com/pob610c9mffjmp4/) con Will Brisbane e Joseph Van Buskirk.

Per saperne di più sulla strategia e la leadership del pensiero, consulta [Successo del cliente](https://experienceleague.corp.adobe.com/docs/customer-success/customer-success/overview.html) hub
