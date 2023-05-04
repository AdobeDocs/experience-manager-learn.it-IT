---
title: Configurazione del modello dati modulo
description: Creazione di un modello dati modulo basato su un’origine dati RDBMS
feature: Adaptive Forms
version: 6.4,6.5
kt: 5812
thumbnail: kt-5812.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 5fa4638f-9faa-40e0-a20d-fdde3dbb528a
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 1%

---

# Configurazione del modello dati modulo

## Origine dati in pool di connessione Apache Sling

Il primo passaggio nella creazione del modello dati del modulo basato su RDBMS consiste nella configurazione dell’origine dati in pool di connessione Apache Sling. Per configurare l’origine dati, segui i passaggi elencati di seguito:

* Posiziona il browser su [configMgr](http://localhost:4502/system/console/configMgr)
* Cerca **Origine dati in pool di connessione Apache Sling**
* Aggiungi una nuova voce e fornisci i valori come mostrato nella schermata .
* ![sorgente dati](assets/data-source.png)
* Salva le modifiche

>[!NOTE]
>L&#39;URI di connessione JDBC, il nome utente e la password cambieranno a seconda della configurazione del database MySQL.


## Creazione di un modello dati modulo

* Posiziona il browser su [Integrazioni dati](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm)
* Fai clic su _Crea_->_Modello dati modulo_
* Fornire un nome e un titolo significativi al modello di dati del modulo, ad esempio **Dipendente**
* Fai clic su _Avanti_
* Seleziona l’origine dati creata nella sezione (forum) precedente
* Fai clic su _Crea_->Modifica per aprire il modello dati del modulo appena creato in modalità di modifica
* Espandi la _forum_ per visualizzare lo schema del dipendente. Espandi il nodo dipendente per visualizzare le 2 tabelle

## Aggiungere entità al modello

* Assicurati che il nodo dipendente sia espanso
* Seleziona il newhire e le entità beneficiarie e fai clic su _Aggiungi selezionati_

## Aggiungi servizio di lettura all&#39;entità successiva

* Seleziona entità nuova
* Fai clic su _Modifica proprietà_
* Seleziona Ottieni dall’elenco a discesa Servizio di lettura .
* Fai clic sull’icona + per aggiungere il parametro al servizio get
* Specifica i valori come mostrato nella schermata
* ![get-service](assets/get-service.png)
>[!NOTE]
> Il servizio get prevede un valore mappato alla colonna empID di una nuova entità. Esistono diversi modi per trasmettere questo valore e in questa esercitazione l&#39;empID viene trasmesso tramite il parametro di richiesta chiamato empID.
* Fai clic su _Fine_ per salvare gli argomenti del servizio get
* Fai clic su _Fine_ per salvare le modifiche al modello dati del modulo

## Aggiungi associazione tra 2 entità

Le associazioni definite tra le entità di database non vengono create automaticamente nel modello dati del modulo. Le associazioni tra entità devono essere definite utilizzando l’editor del modello dati del modulo. Ogni nuova entità può avere uno o più beneficiari, dobbiamo definire un&#39;associazione uno-a-molti tra gli enti di nuova generazione e quelli beneficiari.
La procedura seguente illustra il processo di creazione dell’associazione uno-a-molti

* Seleziona l’entità successiva e fai clic su _Aggiungi associazione_
* Fornisci un titolo e un identificatore significativi all&#39;associazione e ad altre proprietà come mostrato nella schermata seguente
   ![associazione](assets/association-entities-1.png)

* Fai clic sul pulsante _modifica_ icona nella sezione Argomenti

* Specifica i valori come mostrato in questa schermata
* ![associazione-2](assets/association-entities.png)
* **Stiamo collegando le due entità utilizzando la colonna empID dei beneficiari e delle entità successive.**
* Fai clic su _Fine_ per salvare le modifiche

## Verificare il modello dati del modulo

Il modello dati del modulo ora è **_get_** servizio che accetta empID e restituisce i dettagli del newhire e dei relativi beneficiari. Per testare il servizio get, segui i passaggi elencati di seguito.

* Seleziona entità nuova
* Fai clic su _Oggetto modello di test_
* Specifica un empID valido e fai clic su _Test_
* Dovresti ottenere risultati come mostrato nella schermata sottostante
* ![test-fdm](assets/test-form-data-model.png)

## Passaggi successivi

[Ottieni empID dall&#39;URL](./get-request-parameter.md)