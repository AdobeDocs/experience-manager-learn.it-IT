---
title: Aggiunta di articoli al componente gruppo di scelta
description: Aggiungi elementi al componente gruppo di scelta in modo dinamico
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: User
level: Beginner
exl-id: 8fbea634-7949-417f-a4d6-9e551fff63f3
last-substantial-update: 2021-09-10T00:00:00Z
duration: 337
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '456'
ht-degree: 0%

---

# Aggiunta dinamica di elementi al componente gruppo di scelta

In AEM Forms 6.5 è stata introdotta la possibilità di aggiungere elementi in modo dinamico a un componente del gruppo di scelta di Forms adattivo, ad esempio CheckBox, Pulsante di scelta ed Elenco immagini.


Puoi aggiungere elementi utilizzando l’editor visivo e l’editor di codice, a seconda del caso d’uso.

**Utilizzo dell&#39;editor visivo:** È possibile popolare gli elementi del gruppo di scelta dai risultati di una chiamata di funzione o di servizio. Ad esempio, puoi impostare gli elementi del gruppo di scelta utilizzando la risposta di una chiamata API REST.

Nella schermata seguente, stiamo impostando le opzioni di Periodo(anni) di prestito sui risultati di una chiamata al servizio denominata getLoanPeriods.

![Editor di regole](assets/ruleeditor.png)

**Utilizzo dell&#39;editor di codice**: quando si desidera impostare gli elementi nel gruppo di scelta in modo dinamico in base ai valori immessi nel modulo. Ad esempio, il seguente frammento di codice imposta gli elementi della casella di controllo sui valori immessi nei campi del nome del candidato e del coniuge del modulo adattivo.

Nel frammento di codice si impostano gli elementi di WorkingMembers che è un componente casella di controllo. L’array per gli elementi viene generato dinamicamente recuperando i valori dei campi di testo di nome richiedente e coniuge dei moduli adattivi

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

**Utilizzo dell&#39;editor di codice per aggiungere elementi**

* [Scaricare le risorse](assets/usingthecodeeditor.zip)
* [Apri Forms E Documenti](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Fai clic su &quot;Crea&quot; | File Upload&quot; e carica il file scaricato nel passaggio precedente
* [Anteprima moduli](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* Inserire il nome del candidato e selezionare lo stato civile da sposato
* Inserisci il nome del coniuge
* Fai clic su Successivo
* Dovresti vedere la casella di controllo compilata con il nome del richiedente e con il nome del coniuge se lo stato civile è sposato

**Utilizzo dell&#39;editor visivo per aggiungere elementi**

* [Scaricare le risorse](assets/usingthevisualeditor.zip)
* Installa Tomcat se non lo hai già. [Le istruzioni per installare tomcat sono disponibili qui](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)
* [Distribuisci il file SampleRest.war contenuto in questo file zip nel tuo Tomcat](assets/sample-rest.zip)
* [Apri Forms E Documenti](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Fai clic su &quot;Crea&quot; | File Upload&quot; e carica il file scaricato nel passaggio precedente
* [Anteprima moduli](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* Inserire l&#39;importo del prestito e la tabulazione del campo. Ciò attiverà la regola che visualizza il campo del periodo del prestito.
* Selezionare il periodo di prestito appropriato (gli articoli per il periodo di prestito sono compilati dalla chiamata rest)
* Selezionare il tasso di interesse e fare clic su &quot;Ottieni programma di ammortamento&quot;
* Compilare la tabella di ammortamento. La pianificazione dell’ammortamento viene recuperata utilizzando una chiamata REST.

>[!NOTE]
> Si presume che tomcat funzioni sulla porta 8080 e AEM sulla porta 4502
