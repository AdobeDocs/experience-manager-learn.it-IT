---
title: Personalizzazione Inbox
description: Aggiunta di colonne personalizzate per visualizzare dati aggiuntivi sul flusso di lavoro utilizzando il modello "Leggero"
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 2%

---

# Utilizzo di un modello visivo per visualizzare i dati della inbox

Potete utilizzare un modello visivo per formattare i dati da visualizzare nelle colonne della inbox. In questo esempio verranno visualizzate le icone coral-ui a seconda del valore della colonna reddito. La schermata seguente mostra l&#39;uso delle icone nella colonna![reddito-icone](assets/income-column.PNG)

[Il modello](assets/sightly-template.zip) scelto per visualizzare le icone dell&#39;interfaccia utente di corallo personalizzato è disponibile in questo articolo.

## Modello Sightly

Di seguito è riportato il modello per vedute. Il codice nel modello visualizza l&#39;icona a seconda del reddito. Le icone sono disponibili come parte della libreria [di icone](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons) coral ui che viene fornita con AEM.

```java
<template data-sly-template.incomeTemplate="${@ item}>">
    <td is="coral-table-cell" class="payload-income-cell">
         <div data-sly-test="${(item.workflowMetadata && item.workflowMetadata.income)}" data-sly-set.income ="${item.workflowMetadata.income}">
                 <coral-icon icon="confidenceOne" size="M" data-sly-test="${income >=0 && income <10000}"></coral-icon>
                 <coral-icon icon="confidenceTwo" size="M" data-sly-test="${income >=10000 && income <100000}"></coral-icon>
                 <coral-icon icon="confidenceThree" size="M" data-sly-test="${income >=100000 && income <500000}"></coral-icon>
                 <coral-icon icon="confidenceFour" size="M" data-sly-test="${income >=500000}"></coral-icon>
          </div>
    </td>
</template>
```

## Implementazione del servizio

Di seguito è riportata l&#39;implementazione del servizio per visualizzare la colonna reddito.

La riga 12 associa la colonna al modello con vista

```java
import java.util.Map;
import org.osgi.service.component.annotations.Component;
import com.adobe.cq.inbox.ui.InboxItem;
import com.adobe.cq.inbox.ui.column.Column;
import com.adobe.cq.inbox.ui.column.provider.ColumnProvider;

@Component(service = ColumnProvider.class, immediate = true)
public class IncomeProvider implements ColumnProvider {
@Override
public Column getColumn() {

return new Column("income", "Income", String.class.getName(),"inbox/customization/column-templates.html", "incomeTemplate");
}

@Override
public Object getValue(InboxItem inboxItem) {
Object val = null;

Map workflowMetadata = inboxItem.getWorkflowMetadata();

if (workflowMetadata != null && workflowMetadata.containsKey("income"))
    val = workflowMetadata.get("income");

return val;
}
}
```

## Eseguire il test sul server

>[!NOTE]
>
>Questo articolo presuppone che sia stato installato il flusso di lavoro [di](assets/review-workflow.zip) esempio e il modulo [di](assets/snap-form.zip) esempio dall&#39;articolo [](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/inbox-customization/add-married-column.md) precedente di questa serie.

* [Login per crx come utente amministratore](http://localhost:4502/crx/de/index.jsp)
* [importa modello visivo](assets/sightly-template.zip)
* [Accedere AEM console Web](http://localhost:4502/system/console/bundles)
* [Distribuzione e avvio del bundle di personalizzazione della inbox](assets/income-column-customization.jar)
* [Aprite la inbox](http://localhost:4502/aem/inbox)
* Apri Admin Control facendo clic su List View (Visualizzazione elenco) accanto al pulsante Create (Crea)
* Aggiungi colonna reddito a Posta in arrivo e salva le modifiche
* [Visualizzare l’anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Selezionare lo stato _civile_ e inviare il modulo
* [Visualizza inbox](http://localhost:4502/aem/inbox)

L&#39;invio del modulo attiverà il flusso di lavoro e un&#39;attività verrà assegnata all&#39;utente &quot;admin&quot;. Nella colonna reddito dovrebbe essere visualizzata l&#39;icona appropriata
