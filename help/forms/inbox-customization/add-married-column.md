---
title: Personalizzazione Inbox
description: Aggiunta di colonne personalizzate per visualizzare dati aggiuntivi sul flusso di lavoro
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 2%

---


# Aggiungere colonne personalizzate

Per visualizzare i dati del flusso di lavoro nella inbox, è necessario definire e compilare le variabili nel flusso di lavoro. Il valore della variabile deve essere impostato prima che un&#39;attività venga assegnata a un utente. Per iniziare, abbiamo fornito un esempio di flusso di lavoro pronto per essere implementato sul server AEM.

* [Accedi a AEM](http://localhost:4502/crx/de/index.jsp)
* [Importazione del flusso di lavoro di revisione](assets/review-workflow.zip)
* [Revisione del flusso di lavoro](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

Questo flusso di lavoro presenta due variabili definite (isMarried e Income) e i relativi valori sono impostati utilizzando il componente della variabile set. Queste variabili saranno rese disponibili come colonne da aggiungere AEM casella in entrata

## Crea servizio

Per ogni colonna che dobbiamo visualizzare nella nostra inbox dovremmo scrivere un servizio. Il servizio seguente consente di aggiungere una colonna per visualizzare il valore della variabile isMarried

```java
import com.adobe.cq.inbox.ui.column.Column;
import com.adobe.cq.inbox.ui.column.provider.ColumnProvider;

import com.adobe.cq.inbox.ui.InboxItem;
import org.osgi.service.component.annotations.Component;

import java.util.Map;

/**
 * This provider does not require any sightly template to be defined.
 * It is used to display the value of 'ismarried' workflow variable as a column in inbox
 */
@Component(service = ColumnProvider.class, immediate = true)
public class MaritalStatusProvider implements ColumnProvider {@Override
public Column getColumn() {
return new Column("married", "Married", Boolean.class.getName());
}

// Return True or False if 'ismarried' is set. Else returns null
private Boolean isMarried(InboxItem inboxItem) {
Boolean ismarried = null;

Map metaDataMap = inboxItem.getWorkflowMetadata();
if (metaDataMap != null) {
if (metaDataMap.containsKey("isMarried")) {
    ismarried = (Boolean) metaDataMap.get("isMarried");
}
}

return ismarried;
}

@Override
public Object getValue(InboxItem inboxItem) {
return isMarried(inboxItem);
}
}
```

>[!NOTE]
>
>Affinché il codice riportato sopra funzioni, è necessario includere AEM 6.5.5 Uber.jar nel progetto

![uber-jar](assets/uber-jar.PNG)

## Eseguire il test sul server

* [Accedere AEM console Web](http://localhost:4502/system/console/bundles)
* [Distribuzione e avvio del bundle di personalizzazione della inbox](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [Aprite la inbox](http://localhost:4502/aem/inbox)
* Aprire Admin Control facendo clic sull&#39;icona _Visualizzazione elenco_ accanto al pulsante _Crea_
* Aggiungi colonna sposata a Posta in arrivo e salva le modifiche
* [Vai all&#39;interfaccia utente di FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Importa il ](assets/snap-form.zip) modulo di esempio selezionando  _File_ caricato da  __ Createmenu
* [Visualizzare l’anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Selezionare lo _stato civile_ e inviare il modulo
   [casella in entrata](http://localhost:4502/aem/inbox)

L&#39;invio del modulo attiverà il flusso di lavoro e un&#39;attività verrà assegnata all&#39;utente &quot;admin&quot;. Dovresti visualizzare un valore sotto la colonna Sposato come mostrato in questa schermata

![matrimoniale](assets/married-column.PNG)
