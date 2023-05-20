---
title: Accelerare la velocità dei contenuti con i sistemi AEM
description: Scopri come utilizzare i sistemi di stile AEM per consentire a designer, autori di contenuti e sviluppatori della tua organizzazione di creare e distribuire esperienze alla velocità e alla scalabilità che i clienti si aspettano.
solution: Experience Manager
exl-id: 449cd133-6ab6-456e-a0ad-30e3dea9b75b
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '830'
ht-degree: 0%

---

# Accelerare la velocità dei contenuti con i sistemi AEM

In questo articolo, scopri come utilizzare sistemi AEM per consentire a designer, autori di contenuti e sviluppatori della tua organizzazione di creare e distribuire esperienze alla velocità e alla scalabilità che i clienti si aspettano.

## Panoramica

I sistemi di stile dell’AEM presentano quattro vantaggi chiave:

* Gli autori dei modelli possono definire le classi di stile nel criterio del contenuto di un componente o di una pagina
* Gli autori dei contenuti possono selezionare gli stili da applicare a un’intera pagina o quando modificano un componente di una pagina
* I componenti e i modelli sono resi più flessibili consentendo agli autori di eseguire il rendering di varianti visive alternative
* La necessità di sviluppare un componente personalizzato e/o finestre di dialogo complesse per presentare varianti di componenti è ridotta o eliminata completamente

## Configurazione iniziale e utilizzo

La configurazione in 5 passaggi è molto simile a un flusso di lavoro standard per lo sviluppo di componenti.

| **Leader** | **Designer** | **Sviluppatore/Architetto** | **Autore del modello** | **Autore del contenuto** |
| --- | --- | --- | --- | --- |
| Determina il contenuto e gli obiettivi per quel componente | Determina la presentazione visiva ed esperienziale del contenuto | Sviluppa CSS e JS per supportare l’esperienza; definisce e fornisce i nomi delle classi da utilizzare | Configura i criteri dei modelli per i componenti con stili aggiungendo i nomi di classi CSS definiti dagli sviluppatori. I nomi descrittivi devono essere utilizzati per ogni stile. | Durante la creazione di pagine, applica gli stili necessari per ottenere l’aspetto desiderato |

Anche se questa è la configurazione iniziale, molti dei nostri clienti hanno ottenuto ulteriore agilità semplificando questo processo, ad esempio caricando i CSS in DAM, che consente di aggiornare gli stili senza la necessità di distribuzione. Altri clienti dispongono di un set completo di classi di utilità, che consente loro di sviluppare componenti e stili che possono quindi essere utilizzati senza distribuzione o sviluppo.

I sistemi di stile sono disponibili in alcuni gusti diversi:

1. Stili di layout

   * Modifiche multidimensionali alla progettazione e al layout

   * Utilizzato per una rappresentazione ben definita e identificabile

1. Stili di visualizzazione
   * Variazioni minori che non modificano la natura fondamentale dello stile

   * Ad esempio, la modifica della combinazione di colori, del font, dell’orientamento dell’immagine e così via.

1. Stili informativi

   * Mostra/nascondi campi

>[!NOTE]
>
>Per una demo di queste funzioni, consigliamo di guardare [Webinar Customer Success](https://adobecustomersuccess.adobeconnect.com/pob610c9mffjmp4/) con Will Brisbane e Joseph Van Buskirk.

## Best practice

* Solidifica prima lo stile predefinito
   * Layout e visualizzazione del componente quando viene rilasciato sulla pagina prima dell’applicazione dei sistemi di stili
   * Questa dovrebbe essere la rappresentazione più utilizzata
* Prova a visualizzare solo le opzioni di stile che hanno un effetto quando possibile
   * Se vengono esposte combinazioni inefficaci, assicurarsi che non causino effetti negativi
   * Ad es. Stile di layout che determina la posizione dell&#39;immagine ed è accompagnato da uno stile di visualizzazione inefficace che controlla la posizione dell&#39;immagine
* Opzione per gli stili di layout rispetto agli stili di visualizzazione combinati
   * Riduce il numero di permutazioni che devono essere controllate per verificarne la qualità
   * Garantisce il rispetto degli standard del marchio
   * Semplifica l&#39;authoring per gli autori di contenuti
   * Consente di creare un’identità del marchio coerente per il sito
* Sii prudente con gli stili combinati
   * Sia tra le categorie che all’interno delle categorie
* Assegna il tempo necessario per testare accuratamente gli stili combinati
   * Aiuta a evitare effetti indesiderati
* Riduci al minimo il numero di opzioni di stile e permutazioni
   * Troppe opzioni possono portare a una mancanza di coerenza del brand per il look and feel
   * Può causare confusione agli autori di contenuti sulle combinazioni necessarie per ottenere l’effetto desiderato
   * Aumenta le permutazioni che devono essere controllate per verificarne la qualità
* Utilizzare etichette e categorie di stile intuitive per le aziende
   * &quot;Blu&quot; e &quot;Rosso&quot; invece di &quot;Primario&quot; e &quot;Secondario
   * &quot;Scheda&quot; e &quot;Eroe&quot; invece di &quot;Variante A&quot; e &quot;Variante B&quot;
   * Questo può essere più generico per alcuni clienti; il team di progettazione, il team aziendale e il team di contenuti hanno familiarità con i colori primari e secondari o con le varianti che stanno testando. Ma per la flessibilità e per ogni potenziale di cambiamento futuro, l&#39;utilizzo di termini specifici può essere più efficiente.

## Elementi principali da ricordare

I sistemi di stile riducono la necessità di finestre di dialogo complesse, ma non sostituiscono le finestre di dialogo. Semplificano le cose, ma in alcuni casi può essere necessario utilizzare le proprietà dei componenti o la finestra di dialogo invece di creare un apposito sistema di stili.

Possono semplificare i processi dal punto di vista dello sviluppo. Con un sistema di stili è possibile ottenere più effetti visivi dello stesso contenuto. Allo stesso modo, dal punto di vista dell&#39;authoring, piuttosto che addestrare gli autori, e gli autori che devono ricordare quale componente utilizzare in quale palazzo, si può accelerare la velocità di authoring.

Le cose sono semplicemente più pulite. Le HTML all’interno dei Componenti core sono molto dettagliate. Tutto questo a livello CSS rende le build del componente più rapide e anche il codice è più chiaro.

Infine, l&#39;uso dei sistemi di stile è più arte che scienza. Come abbiamo discusso, esistono diverse best practice, ma avrai la possibilità di personalizzare la configurazione della tua organizzazione.

Per ulteriori informazioni, consulta la [Webinar Customer Success](https://adobecustomersuccess.adobeconnect.com/pob610c9mffjmp4/) con Will Brisbane e Joseph Van Buskirk.

Per ulteriori informazioni su strategia e leadership di pensiero, visita [Customer Success](https://experienceleague.adobe.com/docs/customer-success/customer-success/overview.html) hub.
