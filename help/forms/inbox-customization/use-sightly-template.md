---
title: Personalizzazione della casella in entrata
description: Aggiungi colonne personalizzate per visualizzare i dati aggiuntivi del flusso di lavoro utilizzando il modello Sightly
feature: Moduli adattivi
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
topic: Sviluppo
role: Developer
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 3%

---

# Utilizzo del modello Sightly per visualizzare i dati della casella in entrata

È possibile utilizzare un modello Sightly per formattare i dati da visualizzare nelle colonne della casella in entrata. In questo esempio verranno visualizzate le icone coral-ui a seconda del valore della colonna reddito . La schermata seguente mostra l&#39;uso di icone nella colonna reddito
![icone di reddito](assets/income-column.PNG)

[Questo articolo include il ](assets/sightly-template.zip) modello Sightly utilizzato per visualizzare le icone dell’interfaccia utente coral personalizzata.

## Modello Sightly

Segue il modello Sightly. Il codice nel modello visualizza l&#39;icona a seconda del reddito. Le icone sono disponibili come parte della [libreria di icone dell&#39;interfaccia utente coral](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons) che viene fornita con AEM.

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

Il codice seguente è l&#39;implementazione del servizio per la visualizzazione della colonna reddito .

La riga 12 associa la colonna al modello Sightly

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

## Test sul server

>[!NOTE]
>
>Questo articolo presuppone che sia stato installato il [flusso di lavoro di esempio](assets/review-workflow.zip) e [modulo di esempio](assets/snap-form.zip) da [articolo precedente](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/inbox-customization/add-married-column.md) in questa serie.

* [Accedi a crx come utente amministratore](http://localhost:4502/crx/de/index.jsp)
* [modello Importazione guidata](assets/sightly-template.zip)
* [Accedi a AEM console Web](http://localhost:4502/system/console/bundles)
* [Distribuzione e avvio del bundle di personalizzazione della casella in entrata](assets/income-column-customization.jar)
* [Apri la inbox](http://localhost:4502/aem/inbox)
* Apri Admin Control facendo clic su Vista a elenco accanto al pulsante Crea
* Aggiungi la colonna reddito alla casella in entrata e salva le modifiche
* [Visualizzare l’anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Selezionare lo _stato civile_ e inviare il modulo
* [Visualizza casella in entrata](http://localhost:4502/aem/inbox)

L’invio del modulo attiverà il flusso di lavoro e un’attività verrà assegnata all’utente &quot;amministratore&quot;. Dovresti trovare l&#39;icona appropriata nella colonna reddito
