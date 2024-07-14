---
title: Utilizzo del modello Sightly per visualizzare i dati della casella in entrata
description: Aggiungi colonne personalizzate per visualizzare dati aggiuntivi del flusso di lavoro utilizzando il modello Sightly
feature: Adaptive Forms
doc-type: article
version: 6.5
jira: KT-5830
topic: Development
role: Developer
level: Experienced
exl-id: d09b46ed-3516-44cf-a616-4cb6e9dfdf41
duration: 68
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# Utilizzo del modello Sightly per visualizzare i dati della casella in entrata

È possibile utilizzare il modello Sightly per formattare i dati da visualizzare nelle colonne della casella in entrata. In questo esempio visualizzeremo icone di interfaccia utente coral a seconda del valore della colonna reddito. La schermata seguente mostra l’uso delle icone nella colonna delle entrate
![icone di reddito](assets/income-column.PNG)

[Il modello Sightly](assets/sightly-template.zip) utilizzato per visualizzare le icone dell&#39;interfaccia utente Coral personalizzate è fornito come parte di questo articolo.

## Modello Sightly

Di seguito è riportato il modello Sightly. Il codice nel modello visualizza l’icona a seconda del reddito. Le icone sono disponibili come parte della [libreria di icone dell&#39;interfaccia utente coral](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons) fornita con AEM.

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

Il codice seguente è l’implementazione del servizio per visualizzare la colonna delle entrate.

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
>Questo articolo presuppone che tu abbia installato il [flusso di lavoro di esempio](assets/review-workflow.zip) e il [modulo di esempio](assets/snap-form.zip) di [articolo precedente](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/inbox-customization/add-married-column.html) in questa serie.

* [Accedi a crx come utente amministratore](http://localhost:4502/crx/de/index.jsp)
* [importa modello sightly](assets/sightly-template.zip)
* [Accesso alla console Web AEM](http://localhost:4502/system/console/bundles)
* [Distribuire e avviare il bundle di personalizzazione della casella in entrata](assets/income-column-customization.jar)
* [Apri la tua casella in entrata](http://localhost:4502/aem/inbox)
* Per aprire Admin Control (Controllo amministratore), fai clic su List View (Vista elenco) accanto al pulsante Create (Crea).
* Aggiungi la colonna delle entrate alla casella in entrata e salva le modifiche
* [Anteprima modulo](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Seleziona lo _stato civile_ e invia il modulo
* [Visualizza casella in entrata](http://localhost:4502/aem/inbox)

L’invio del modulo attiverà il flusso di lavoro e un’attività verrà assegnata all’utente &quot;amministratore&quot;. Dovresti visualizzare l’icona appropriata sotto la colonna delle entrate
