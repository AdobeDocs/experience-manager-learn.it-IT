---
title: Crea un componente flusso di lavoro per salvare gli allegati al file system
description: Scrittura di allegati di moduli adattivi nel file system utilizzando un componente flusso di lavoro personalizzato
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-11-28T00:00:00Z
source-git-commit: 09b00a7edf2f4c90c6cb2178161c6d7e0c9432e8
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 1%

---

# Componente flusso di lavoro personalizzato

Questa esercitazione è destinata ai clienti di AEM Forms che devono creare un componente flusso di lavoro personalizzato. Il componente del flusso di lavoro sarà configurato per eseguire il codice scritto nel passaggio precedente. Il componente del flusso di lavoro può specificare argomenti di processo per il codice. In questo articolo esploreremo il componente del flusso di lavoro associato al codice.


[Scarica il componente flusso di lavoro personalizzato](assets/saveFiles.zip)
Importare il componente flusso di lavoro [utilizzo di package manager](http://localhost:4502/crx/packmgr/index.jsp)

Il componente flusso di lavoro personalizzato si trova in /apps/AEMFormsDemoListings/workflowcomponent/SaveFiles

Selezionare il nodo SaveFiles ed esaminarne le proprietà

**componentGroup** - Il valore di questa proprietà determina la categoria del componente del flusso di lavoro.

**jcr:Title** - Titolo del componente del flusso di lavoro.

**sling:resourceSuperType** Il valore di questa proprietà determina l&#39;ereditarietà del componente. In questo caso ereditiamo dal componente processo


![proprietà dei componenti](assets/component-properties1.png)

## cq:dialog

Le finestre di dialogo consentono all’autore di interagire con il componente. Il cq:dialog si trova sotto il nodo SaveFiles
![cq-dialog](assets/cq-dialog.png)

I nodi sotto il nodo elementi rappresentano le schede del componente attraverso le quali gli autori interagiscono con il componente. Le schede comuni e di processo sono nascoste. Le schede Comune e Argomenti sono visibili.

Gli argomenti del processo sono sotto il nodo processargs

![array](assets/process-arguments.png)

L&#39;autore specifica gli argomenti come mostrato nella schermata sottostante
![componente flusso di lavoro](assets/custom-workflow-component.png)

I valori vengono memorizzati come proprietà del nodo di metadati. Ad esempio, il valore **c:\formsattachments** verranno memorizzati nella proprietà saveToLocation del nodo di metadati
![percorso di salvataggio](assets/save-to-location.png)

## cq:editConfig

Il cq:EditConfig è semplicemente un nodo con il tipo primario cq:EditConfig e il nome cq:editConfig sotto la radice del componente Il comportamento di modifica di un componente viene configurato aggiungendo un nodo cq:editConfig di tipo cq:EditConfig sotto il nodo del componente (di tipo cq:Component)

![edit-config](assets/cq-edit-config.png)

cq:formParameters (tipo di nodo nt:unstructured): definisce parametri aggiuntivi aggiunti al modulo di dialogo.


Osserva le proprietà del nodo cq:formParameters
![from-parameters-properties](assets/form-parameters-properties.png)

Il valore della proprietà PROCESS indica il codice java che sarà associato al componente del flusso di lavoro.






