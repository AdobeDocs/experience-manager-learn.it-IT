---
title: Aggiungi colonne personalizzate
description: Aggiungi colonne personalizzate per visualizzare dati aggiuntivi del flusso di lavoro
feature: Adaptive Forms
doc-type: article
version: 6.5
jira: KT-5830
topic: Development
role: Developer
level: Experienced
exl-id: 0b141b37-6041-4f87-bd50-dade8c0fee7d
duration: 91
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 1%

---

# Aggiungi colonne personalizzate

Per visualizzare i dati del flusso di lavoro nella casella in entrata, è necessario definire e popolare le variabili nel flusso di lavoro. Il valore della variabile deve essere impostato prima che un&#39;attività venga assegnata a un utente. Per aiutarti nella tua attività, abbiamo fornito un esempio di flusso di lavoro pronto per essere implementato sul tuo server AEM.

* [Accedi all’AEM](http://localhost:4502/crx/de/index.jsp)
* [Importare il flusso di lavoro di revisione](assets/review-workflow.zip)
* [Rivedere il flusso di lavoro](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

Questo flusso di lavoro presenta due variabili definite (isMarried e income) e i relativi valori vengono impostati utilizzando il componente variabile impostato. Queste variabili sono rese disponibili come colonne da aggiungere alla casella in entrata dell’AEM

## Crea servizio

Per ogni colonna da visualizzare nella casella in entrata, è necessario scrivere un servizio. Il seguente servizio consente di aggiungere una colonna per visualizzare il valore della variabile isMarried

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
>Devi includere AEM 6.5.5 Uber.jar nel progetto affinché il codice di cui sopra funzioni

![uber-jar](assets/uber-jar.PNG)

## Test sul server

* [Accedi alla console web AEM](http://localhost:4502/system/console/bundles)
* [Distribuire e avviare il bundle di personalizzazione della casella in entrata](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [Apri la casella in entrata](http://localhost:4502/aem/inbox)
* Apri Admin Control facendo clic su _Vista a elenco_ icona accanto a _Crea_ pulsante
* Aggiungi colonna Sposato alla casella in entrata e salva le modifiche
* [Passa a FormsAndDocuments UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Importa il modulo di esempio](assets/snap-form.zip) selezionando _Caricamento file_ da _Crea_ menu
* [Visualizzare l’anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Seleziona la _stato civile_ e invia il modulo
  [visualizza casella in entrata](http://localhost:4502/aem/inbox)

L’invio del modulo attiverà il flusso di lavoro e un’attività verrà assegnata all’utente &quot;amministratore&quot;. Dovresti visualizzare un valore sotto la colonna Sposato, come illustrato in questa schermata

![colonna sposata](assets/married-column.PNG)

## Passaggi successivi

[Visualizza colonna sposata](./use-sightly-template.md)
