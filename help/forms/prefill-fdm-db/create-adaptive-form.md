---
title: Creare un modulo adattivo
description: Creare e configurare un modulo adattivo per utilizzare il servizio di precompilazione del modello dati del modulo
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5813
thumbnail: kt-5813.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 1%

---


# Crea modulo adattivo

Finora abbiamo creato i seguenti

* Database con 2 tabelle - `newhire` e `beneficiaries`
* Origine dati pool connessione Apache Sling configurata
* Modello dati modulo basato su RDBMS

Il passaggio successivo consiste nel creare e configurare un modulo adattivo per l&#39;utilizzo del modello dati del modulo.  Per iniziare, è possibile [scaricare e importare](assets/fdm-demo-af.zip) un modulo di esempio. Nel modulo di esempio è presente una sezione che mostra i dettagli del dipendente e un&#39;altra sezione che elenca i beneficiari del dipendente.

## Associazione di un modulo al modello dati del modulo

Il modulo di esempio fornito con questo corso non è associato ad alcun modello dati del modulo. Per configurare il modulo in modo che utilizzi il modello dati modulo, è necessario effettuare le seguenti operazioni:

* Selezionare il modulo FDMDemo
* Fare clic su _Proprietà_->Modello _modulo_
* Seleziona Modello dati modulo dall&#39;elenco a discesa
* Cercare e selezionare il modello dati del modulo creato nella lezione precedente.
* Click on _Save &amp; Close_

## Configura servizio di precompilazione

Il primo passaggio consiste nell&#39;associare il servizio di precompilazione al modulo. Per associare il servizio di precompilazione, seguire i passaggi indicati di seguito

* Selezionare il `FDMDemo` modulo
* Fare clic su _Modifica_ per aprire il modulo in modalità di modifica
* Selezionare Contenitore modulo nella gerarchia del contenuto e fare clic sull&#39;icona chiave inglese per aprire il relativo foglio delle proprietà
* Selezionare il servizio _di precompilazione modello dati_ modulo dall&#39;elenco a discesa Servizio di precompilazione
* Fate clic su ☑ blu per salvare le modifiche

* ![servizio di precompilazione](assets/fdm-prefill.png)

## Configura dettagli dipendente

Il passaggio successivo consiste nel collegare i campi di testo del modulo adattivo agli elementi del modello dati del modulo. Sarà necessario aprire il foglio delle proprietà dei campi seguenti e impostare il relativo bindRef come mostrato di seguito


| Nome campo | Rif. associazione |
|------------|--------------------|
| Nome | /newhire/FirstName |
| Cognome | /newhire/lastName |

>[!NOTE]
>
>Non esitate ad aggiungere altri campi di testo e ad eseguire un binding con gli elementi appropriati del modello dati del modulo

## Configura tabella beneficiari

Il passo successivo è quello di mostrare i beneficiari del dipendente in modo tabulare. Il modulo di esempio fornito contiene una tabella con 4 colonne e una riga singola. Dobbiamo configurare la tabella affinché cresca in base al numero di beneficiari.

* Aprire il modulo in modalità di modifica.
* Espandi pannello principale->Beneficiari->Tabella
* Selezionare Riga1 e fare clic sull&#39;icona chiave inglese per aprire il relativo foglio delle proprietà.
* Imposta il riferimento del binding su **/newhire/GetDipendeBeneficiaries**
* Impostate le impostazioni di ripetizione - Conteggio minimo su 1 e Conteggio massimo su 5.
* La configurazione Row1 deve essere simile a quella della schermata sottostante
   ![row-configure](assets/configure-row.PNG)
* Fate clic sul ☑ blu per salvare le modifiche

## Binding celle di riga

Infine, è necessario eseguire il binding delle celle Riga con gli elementi del modello dati modulo.

* Espandi pannello principale->Beneficiari->Tabella->Riga1
* Imposta il Riferimento di binding di ogni cella di riga come indicato nella tabella seguente

| Cella riga | Riferimento bind |
|------------|----------------------------------------------|
| Nome | /newhire/GetDipendeBeneficiaries/firstname |
| Cognome | /newhire/GetDipendentBeneficiaries/lastname |
| Relazione | /newhire/GetDipendentBeneficiaries/relation |
| Percentuale | /newhire/GetDipendeBeneficiari/percentuale |

* Fate clic sul ☑ blu per salvare le modifiche

## Verificare il modulo

Ora è necessario aprire il modulo con empID appropriato nell’URL. I seguenti 2 collegamenti compileranno i moduli con informazioni provenienti dal[modulo del database con empID=207](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=207)[Modulo con empID=208](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=208)

## Risoluzione dei problemi

Il modulo è vuoto e non contiene dati

* Assicurarsi che il modello dati del modulo restituisca i risultati corretti.
* Il modulo è associato al modello dati modulo corretto
* Controllare i binding dei campi
* Controllare il file di registro stdout. È necessario che empID sia scritto nel file. Se non viene visualizzato questo valore, il modulo potrebbe non utilizzare il modello personalizzato fornito.

Tabella non compilata

* Controllare il binding Row1
* Verificare che le impostazioni di ripetizione per Row1 siano impostate correttamente (Min=1 e Max = 5 o più)

