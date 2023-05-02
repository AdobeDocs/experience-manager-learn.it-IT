---
title: AEM Forms con Marketo (parte 3)
description: Esercitazione per integrare AEM Forms con Marketo utilizzando AEM Forms Form Data Model.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 7096340b-8ccf-4f5e-b264-9157232e96ba
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Configura origine dati

L’integrazione dei dati di AEM Forms consente di configurare e connettersi a diverse origini dati. I seguenti tipi sono supportati come predefiniti. Tuttavia, con una piccola personalizzazione, puoi integrarti anche con altre origini dati.

1. Database relazionali - MySQL, Microsoft SQL Server, IBM DB2 e Oracle RDBMS
1. Profilo utente AEM
1. Servizi web RESTful
1. Servizi web basati su SOAP
1. Servizi OData

Per l’integrazione di AEM Forms con Marketo, utilizziamo i servizi web RESTful. Il primo passaggio nell’integrazione consiste nel configurare un [origine dati.](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) Utilizza il file swagger fornito come parte di questa esercitazione. La schermata seguente mostra le proprietà importanti da specificare durante la configurazione dell’origine dati.
![datasource](assets/datasource.jfif)

Il &quot;marketo.json&quot; è il file swagger e ti viene fornito come parte delle risorse di questa esercitazione.
Host proprietà è specifico per la tua istanza Marketo.
Il tipo di autenticazione è personalizzato e l&#39;implementazione dell&#39;autenticazione deve corrispondere a &quot;AemForms With Marketo&quot;. (A meno che questo non sia stato modificato nel codice).

## Crea modello dati modulo

Dopo, la configurazione dell’origine dati il passaggio successivo consiste nella creazione di un modello dati modulo basato sull’origine dati configurata nel passaggio precedente. Per creare un modello dati modulo, procedere come segue:

Posiziona il browser sul [pagina integrazioni dati.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) Vengono elencate tutte le integrazioni di dati create nell’istanza AEM.

1. Fai clic su Crea | Modello dati modulo
1. Fornire un titolo significativo, ad esempio FormsAndMarketo e fare clic su Avanti
1. Selezionare l’origine dati configurata nel passaggio precedente e fare clic su Crea e modifica per aprire il modello dati modulo in modalità di modifica
1. Espandi il nodo &quot;FormsAndMarketo&quot;. Espandi il nodo Servizi
1. Selezionare la prima operazione &quot;Get&quot;
1. Fai clic su Aggiungi selezionati
1. Fare clic su &quot;Seleziona tutto&quot; nella finestra di dialogo &quot;Aggiungi oggetti modello associati&quot;, quindi fare clic su Aggiungi
1. Salvare il modello dati del modulo facendo clic sul pulsante Salva
1. Passa alla scheda Servizi
1. Seleziona l’unico servizio elencato e fai clic su Test Service
1. Specifica un leadId valido e fai clic su Test. Se tutto va bene, dovresti recuperare i dettagli del lead come mostrato nella schermata sottostante
   ![risultati dei test](assets/testresults.jfif)

## Passaggi successivi

[Mettere tutto insieme per i test](./part4.md)

