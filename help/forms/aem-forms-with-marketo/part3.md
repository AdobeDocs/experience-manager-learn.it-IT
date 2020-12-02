---
title: ' AEM Forms con Marketo(Parte 3)'
seo-title: ' AEM Forms con Marketo(Parte 3)'
description: Esercitazione per integrare  AEM Forms con Marketo mediante  AEM Forms Form Data Model.
seo-description: Esercitazione per integrare  AEM Forms con Marketo mediante  AEM Forms Form Data Model.
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 1%

---


# Configura origine dati

 Integrazione dei dati AEM Forms consente di configurare e connettersi a origini dati diverse. I seguenti tipi sono supportati out-of-the-box. Tuttavia, con una piccola personalizzazione, è possibile integrarsi anche con altre origini dati.

1. Database relazionali - MySQL, Microsoft SQL Server, IBM DB2 e  Oracle RDBMS
1. AEM profilo utente
1. Servizi Web RESTful
1. Servizi Web basati su SOAP
1. Servizi OData

Per l&#39;integrazione  AEM Forms con Marketo, utilizzeremo i servizi web RESTful. Il primo passo nell&#39;integrazione è configurare un&#39;origine dati [.](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) Utilizzate il file swagger fornito nell&#39;ambito di questa esercitazione. La schermata seguente mostra le proprietà importanti che è necessario specificare durante la configurazione dell&#39;origine dati.
![datasource](assets/datasource.jfif)

&quot;marketo.json&quot; è il file swagger e viene fornito come parte delle risorse di questa esercitazione.
L&#39;host delle proprietà è specifico dell&#39;istanza di Marketo.
Il tipo di autenticazione è personalizzato e l&#39;implementazione dell&#39;autenticazione deve corrispondere a &quot;AemForms With Marketo&quot;. (a meno che questo non sia stato modificato nel codice).

## Crea modello dati modulo

Dopo, la configurazione dell&#39;origine dati il passaggio successivo consiste nella creazione di un modello dati modulo basato sull&#39;origine dati configurata nel passaggio precedente. Per creare un modello dati modulo, procedere come segue:

Posizionare il browser sulla pagina [integrazioni di dati.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) Vengono elencate tutte le integrazioni di dati create nell&#39;istanza AEM.

1. Fate clic su Crea | Modello dati modulo
1. Fornire un titolo significativo, ad esempio FormsAndMarketo e fare clic su Avanti
1. Selezionare l&#39;origine dati configurata nel passaggio precedente e fare clic su Crea e modifica per aprire il modello dati modulo in modalità di modifica
1. Espandere il nodo &quot;FormsAndMarketo&quot;. Espandi il nodo Servizi
1. Selezionare la prima operazione &quot;Get&quot;
1. Fare clic su Aggiungi selezionato
1. Fare clic su &quot;Seleziona tutto&quot; nella finestra di dialogo &quot;Aggiungi oggetti modello associati&quot;, quindi fare clic su Aggiungi
1. Salvare il modello dati del modulo facendo clic sul pulsante Salva
1. Passa alla scheda Servizi
1. Selezionare l&#39;unico servizio elencato e fare clic su Test Service
1. Immettete un leadId valido e fate clic su Test. Se tutto va bene dovreste recuperare i dettagli del lead come mostrato nella videata qui sotto
   ![risultati del test](assets/testresults.jfif)
