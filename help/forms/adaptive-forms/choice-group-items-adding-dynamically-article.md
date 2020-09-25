---
title: Aggiunta di elementi al componente gruppo di scelta
seo-title: Aggiunta di elementi al componente gruppo di scelta
description: Aggiunta dinamica di elementi al componente gruppo di scelta
seo-description: Aggiunta dinamica di elementi al componente gruppo di scelta
feature: adaptive-forms
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '527'
ht-degree: 0%

---



# Aggiunta dinamica di elementi al componente gruppo di scelta

 AEM Forms 6.5 ha introdotto la possibilità di aggiungere dinamicamente elementi a un componente di gruppo di scelta Forms adattivo come caselle di controllo, pulsanti di scelta e elenco immagini.

[Questa funzionalità è disponibile dal vivo sul server](https://forms.enablementadobe.com/content/samples/samples.html?query=0)Samples. Cerca elementi casella di controllo dinamica e clicca su &quot;Prova&quot;


È possibile aggiungere elementi utilizzando l&#39;editor visivo e l&#39;editor di codice, a seconda del caso d&#39;uso.

**Utilizzo dell’editor visivo:** È possibile compilare gli elementi del gruppo di scelta dai risultati di una chiamata di funzione o di un servizio. Ad esempio, potete impostare gli elementi del gruppo di scelta utilizzando la risposta di una chiamata REST API.

Nella schermata seguente, stiamo impostando le opzioni del periodo di prestito (anni) sui risultati di una chiamata di servizio chiamata getLoanPeriods.

![Editor regola](assets/ruleeditor.png)

**Utilizzo dell’editor** di codice: Per impostare gli elementi del gruppo di scelta in modo dinamico in base ai valori immessi nel modulo. Ad esempio, il frammento di codice seguente imposta gli elementi della casella di controllo sui valori immessi nei campi del nome del richiedente e del coniuge del modulo adattivo.

Nello snippet di codice, stiamo impostando gli elementi di WorkingMembers che è un componente casella di controllo. L&#39;array degli elementi viene generato dinamicamente recuperando i valori dei campi di testo NomeCandidato e Conte dei moduli adattivi

```javascript
 
 if(MaritalStatus.value=="Married")
  {
WorkingMembers.items =["spouse="+spouse.value,"applicant="+applicantName.value];
  }
else
  {
    WorkingMembers.items =["applicant="+applicantName.value];
  }
```

I dati presentati sono i seguenti

```xml
<afUnboundData>

<data>

<applicantName>John Jacobs</applicantName>

<MaritalStatus>Married</MaritalStatus>

<spouse>Gloria Rios</spouse>

<WorkingMembers>spouse,applicant</WorkingMembers>

</data>

</afUnboundData>
```

**Aggiunta di elementi tramite l&#39;editor di regole**

>[!VIDEO](https://video.tv.adobe.com/v/26847?quality=12&learn=on)

**Aggiunta di elementi tramite l&#39;editor di codice**

>[!VIDEO](https://video.tv.adobe.com/v/26848?quality=12&learn=on)

Per provare questo sul sistema:

**Utilizzo dell’editor di codice per aggiungere elementi**

* [Scaricare le risorse](assets/usingthecodeeditor.zip)
* [Aprire Forms E Documenti](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Fare clic su &quot;Crea | Caricamento file&quot; e caricamento del file scaricato nel passaggio precedente
* [Anteprima dei moduli](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* Immettere il nome del candidato e selezionare lo stato civile da sposare
* Inserire il nome del coniuge
* Fai clic su Avanti
* Se lo stato coniugale è sposato, dovresti vedere la casella di controllo compilata con il nome del candidato e con il nome del coniuge

**Utilizzo dell’editor visivo per aggiungere elementi**

* [Scaricare le risorse](assets/usingthevisualeditor.zip)
* Installate Tomcat se non lo avete già. [Le istruzioni per installare tomcat sono disponibili qui](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)
* [Distribuisci file SampleRest.war in Tomcat](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
* [Aprire Forms E Documenti](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Fare clic su &quot;Crea | Caricamento file&quot; e caricamento del file scaricato nel passaggio precedente
* [Anteprima dei moduli](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* Immettere l&#39;importo del prestito e la tabulazione fuori dal campo. Questo attiverà la regola che visualizza il campo del periodo di prestito.
* Selezionare il periodo di prestito appropriato (gli elementi per il periodo di prestito sono compilati dalla chiamata di riposo)
* Selezionare il tasso di interesse e fare clic su &quot;Ottieni piano di ammortamento&quot;
* La tabella di ammortamento dovrebbe essere compilata. Il programma di ammortamento viene recuperato utilizzando una chiamata REST.

>[!NOTE]
> Si presume che tomcat sia in esecuzione sulla porta 8080 e AEM sulla porta 4502
