---
title: Creare un modulo adattivo
description: Crea e configura un modulo adattivo per utilizzare il servizio di precompilazione del modello dati del modulo
feature: Moduli adattivi
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5813
thumbnail: kt-5813.jpg
topic: Sviluppo
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 2%

---


# Creare un modulo adattivo

Finora abbiamo creato quanto segue

* Database con 2 tabelle - `newhire` e `beneficiaries`
* Origine dati in pool di connessione Apache Sling configurata
* Modello dati modulo basato su RDBMS

Il passaggio successivo consiste nel creare e configurare un Modulo adattivo per l’utilizzo del modello dati del modulo.  Per ottenere l&#39;inizio dell&#39;attività, è possibile [scaricare e importare il modulo di esempio ](assets/fdm-demo-af.zip). Nel modulo di esempio è disponibile una sezione per visualizzare i dettagli del dipendente e un&#39;altra sezione per elencare i beneficiari del dipendente.

## Associazione del modulo al modello dati del modulo

Il modulo di esempio fornito con questo corso non è associato ad alcun modello di dati del modulo. Per configurare il modulo in modo che utilizzi il modello dati modulo, è necessario effettuare le seguenti operazioni:

* Selezionare il modulo FDMDemo
* Fai clic su _Proprietà_->_Modello di modulo_
* Seleziona Modello dati modulo dall’elenco a discesa
* Cerca e seleziona il modello dati modulo creato nella lezione precedente.
* Fai clic su _Salva e chiudi_

## Configurare il servizio di precompilazione

Il primo passo consiste nell’associare il servizio di precompilazione per il modulo. Per associare il servizio di precompilazione, segui i passaggi indicati di seguito

* Selezionare il modulo `FDMDemo`
* Fai clic su _Modifica_ per aprire il modulo in modalità di modifica
* Seleziona Contenitore modulo nella gerarchia del contenuto e fai clic sull’icona a forma di chiave inglese per aprire il relativo foglio delle proprietà
* Seleziona _Servizio di precompilazione modello dati modulo_ dall&#39;elenco a discesa Precompilazione servizio
* Fai clic su ☑ blu per salvare le modifiche

* ![servizio di precompilazione](assets/fdm-prefill.png)

## Configura i dettagli del dipendente

Il passaggio successivo consiste nel associare i campi di testo del modulo adattivo agli elementi del modello dati del modulo. Sarà necessario aprire il foglio delle proprietà dei campi seguenti e impostare il relativo bindRef come mostrato di seguito


| Nome campo | Rif. binding |
|------------|--------------------|
| Nome | /newhire/FirstName |
| Cognome | /newhire/lastName |

>[!NOTE]
>
>Puoi aggiungere campi di testo aggiuntivi ed eseguirne il binding con gli elementi appropriati del modello dati del modulo

## Configura tabella beneficiari

Il passo successivo è quello di mostrare i beneficiari del dipendente in modo tabulare. Il modulo di esempio fornito dispone di una tabella con 4 colonne e una riga singola. Dobbiamo configurare la tabella in modo che aumenti in base al numero di beneficiari.

* Apri il modulo in modalità di modifica.
* Espandi pannello principale->I tuoi beneficiari->Tabella
* Selezionare Riga1 e fare clic sull&#39;icona a forma di chiave inglese per aprire la relativa finestra delle proprietà.
* Imposta il riferimento di binding su **/newhire/GetEmployeeBeneficiaries**
* Imposta le impostazioni ripetute - Conteggio minimo su 1 e Conteggio massimo su 5.
* La configurazione della riga1 deve essere simile alla schermata sottostante
   ![configurazione a righe](assets/configure-row.PNG)
* Fai clic sul ☑ blu per salvare le modifiche

## Celle di riga binding

Infine, è necessario eseguire il binding delle celle riga agli elementi del modello dati modulo.

* Espandi pannello principale->I tuoi beneficiari->Tabella->Riga1
* Imposta il Riferimento di binding di ciascuna cella di riga come indicato nella tabella seguente

| Cella riga | Riferimento bind |
|------------|----------------------------------------------|
| Nome | /newhire/GetEmployeeBeneficiaries/firstname |
| Cognome | /newhire/GetEmployeeBeneficiaries/lastname |
| Relazione | /newhire/GetEmployeeBeneficiaries/relation |
| Percentuale | /newhire/GetEmployeeBeneficiaries/percentuale |

* Fai clic sul ☑ blu per salvare le modifiche

## Verificare il modulo

Ora è necessario aprire il modulo con empID appropriato nell’url. I seguenti 2 collegamenti compileranno i moduli con le informazioni del database
[Modulo con empID=207](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=207)
[Modulo con empID=208](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=208)

## Risoluzione dei problemi

Il modulo è vuoto e non contiene dati

* Assicurarsi che il modello dati del modulo restituisca i risultati corretti.
* Il modulo è associato al modello dati modulo corretto
* Controllare i binding dei campi
* Controlla il file di log stdout. Dovresti vedere empID scritto nel file.Se non trovi questo valore, il modulo potrebbe non utilizzare il modello personalizzato fornito.

Tabella non compilata

* Controllare il binding Row1
* Assicurati che le impostazioni di ripetizione per la riga1 siano impostate correttamente (Min = 1 e Max = 5 o più)

