---
title: Crea componente flusso di lavoro per salvare gli allegati del modulo nel file system
description: Scrittura di allegati di moduli adattivi nel file system tramite un componente flusso di lavoro personalizzato
feature: Workflow
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-11-28T00:00:00Z
exl-id: acc701ec-b57d-4c20-8f97-a5a69bb180cd
duration: 70
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '360'
ht-degree: 0%

---

# Componente flusso di lavoro personalizzato

Questo tutorial è destinato ai clienti di AEM Forms che necessitano di creare un componente flusso di lavoro personalizzato. Il componente flusso di lavoro verrà configurato per eseguire il codice scritto nel passaggio precedente. Il componente del flusso di lavoro può specificare gli argomenti del processo per il codice. In questo articolo esploreremo il componente del flusso di lavoro associato al codice.


[Scarica il componente del flusso di lavoro personalizzato](assets/saveFiles.zip)
Importa il componente del flusso di lavoro [tramite Gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp)

Il componente del flusso di lavoro personalizzato si trova in /apps/AEMFormsDemoListings/workflowcomponent/SaveFiles

Selezionare il nodo SaveFiles ed esaminarne le proprietà

**componentGroup** - Il valore di questa proprietà determina la categoria del componente del flusso di lavoro.

**jcr:Title** - Titolo del componente del flusso di lavoro.

**sling:resourceSuperType** Il valore di questa proprietà determinerà l&#39;ereditarietà di questo componente. In questo caso ereditiamo dal componente processo


![proprietà-componente](assets/component-properties1.png)

## cq:dialog

Le finestre di dialogo vengono utilizzate per consentire all’autore di interagire con il componente. La finestra cq:dialog si trova sotto il nodo SaveFiles
![cq-dialog](assets/cq-dialog.png)

I nodi sotto il nodo elementi rappresentano le schede del componente attraverso le quali gli autori interagiscono con il componente. Le schede comuni e di processo sono nascoste. Sono visibili le schede Comuni e Argomenti.

Gli argomenti di processo per il processo si trovano sotto il nodo processargs

![process-args](assets/process-arguments.png)

L’autore specifica gli argomenti come mostrato nella schermata seguente
![componente-flusso di lavoro](assets/custom-workflow-component.png)

I valori vengono memorizzati come proprietà del nodo di metadati. Ad esempio, il valore **c:\formsattachments** verrà memorizzato nella proprietà saveToLocation del nodo metadati
![salva-posizione](assets/save-to-location.png)

## cq:editConfig

Cq:EditConfig è semplicemente un nodo con il tipo principale cq:EditConfig e il nome cq:editConfig sotto la radice del componente
Il comportamento di modifica di un componente viene configurato aggiungendo un nodo cq:editConfig di tipo cq:EditConfig sotto il nodo del componente (di tipo cq:Component)

![edit-config](assets/cq-edit-config.png)

cq:formParameters (tipo di nodo nt:unstructured): definisce parametri aggiuntivi che vengono aggiunti al modulo della finestra di dialogo.


Osserva le proprietà del nodo cq:formParameters
![da-parametri-proprietà](assets/form-parameters-properties.png)

Il valore della proprietà PROCESS indica il codice Java che verrà associato al componente del flusso di lavoro.
