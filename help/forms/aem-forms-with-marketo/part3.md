---
title: AEM Forms con Marketo (Parte 3)
description: Tutorial per integrare AEM Forms con Marketo utilizzando AEM Forms Form Data Model.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 7096340b-8ccf-4f5e-b264-9157232e96ba
duration: 97
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 1%

---

# Configura origine dati

L’integrazione dei dati di AEM Forms consente di configurare e connettersi a diverse origini dati. Sono supportati i seguenti tipi pronti all’uso. Tuttavia, con una piccola personalizzazione, puoi anche integrare con altre origini dati.

1. Database relazionali: MySQL, Microsoft SQL Server, IBM DB2 e Oracle RDBMS
1. Profilo utente AEM
1. Servizi Web RESTful
1. Servizi web basati su SOAP
1. Servizi OData

Per l’integrazione di AEM Forms con Marketo, utilizziamo i servizi web RESTful. Il primo passaggio nell’integrazione consiste nel configurare una [origine dati.](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) Utilizza il file swagger fornito come parte di questa esercitazione. La schermata seguente mostra le proprietà importanti da specificare durante la configurazione dell’origine dati.
![datasource](assets/datasource.jfif)

Il &quot;marketo.json&quot; è il file swagger e viene fornito come parte delle risorse di questo tutorial.
L’host delle proprietà è specifico per l’istanza Marketo.
Il tipo di autenticazione è personalizzato e l’implementazione dell’autenticazione deve corrispondere a &quot;AemForms With Marketo&quot;. (a meno che non sia stato modificato nel codice).

## Crea modello dati modulo

Dopo aver configurato l’origine dati, il passaggio successivo consiste nel creare un modello dati del modulo basato sull’origine dati configurata nel passaggio precedente. Per creare un modello dati modulo, segui i passaggi seguenti:

Puntare il browser al [pagina delle integrazioni di dati.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) Elenca tutte le integrazioni di dati create nell’istanza AEM.

1. Fai clic su Crea. | Modello dati modulo
1. Fornisci un titolo significativo come FormsAndMarketo e fai clic su Avanti
1. Seleziona l’origine dati configurata nel passaggio precedente, quindi fai clic su Crea e modifica per aprire il modello dati del modulo in modalità di modifica
1. Espandere il nodo &quot;FormsAndMarketo&quot;. Espandere il nodo Servizi
1. Seleziona la prima operazione &quot;Get&quot;
1. Fai clic su Aggiungi selezionati
1. Fate clic su &quot;Seleziona tutto&quot; nella finestra di dialogo &quot;Aggiungi oggetti modello associati&quot;, quindi fate clic su Aggiungi
1. Salvare il modello dati del modulo facendo clic sul pulsante Salva
1. Scheda della scheda Servizi
1. Seleziona l’unico servizio elencato e fai clic su Test Service
1. Specifica un leadId valido e fai clic su Prova. Se tutto va bene, dovresti recuperare i dettagli del lead come mostrato nella schermata seguente
   ![risultati test](assets/testresults.jfif)

## Passaggi successivi

[Tutti gli elementi sono stati messi insieme per i test](./part4.md)
