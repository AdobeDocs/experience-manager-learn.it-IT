---
title: Abilitare i componenti del portale AEM Forms
description: Creare un portale AEM Forms utilizzando i componenti core
solution: Experience Manager
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 10373
source-git-commit: 55583effd0400bac2e38756483d69f5bd114cb21
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# Componenti di Forms Portal

AEM Forms fornisce i seguenti componenti del portale:

**Ricerca e filtro**: Questo componente consente di elencare i moduli dall’archivio moduli nella pagina del portale e fornisce opzioni di configurazione per elencare i moduli in base a criteri specifici.

**Bozze e invii**: Mentre il componente Ricerca e filtro visualizza i moduli resi pubblici dall’autore di Forms, il componente Bozze e invii visualizza i moduli salvati come bozze per essere completati in seguito e inviati. Questo componente offre un’esperienza personalizzata a qualsiasi utente connesso.

**Collegamento**: Questo componente consente di creare un collegamento a un modulo in qualsiasi punto della pagina.

## Abilita componenti del portale Forms

Avvia IntelliJ e apri il progetto BankingApplication creato in [passaggio precedente.](./getting-started.md) Espandi l’interfaccia ui.apps->src->main->content->jcr_root->apps.bankingapplication->components

Per utilizzare qualsiasi componente di base (inclusi i componenti del portale predefiniti) in un sito Adobe Experience Manager (AEM), è necessario creare un componente proxy e abilitarlo per il sito.
Il componente proxy appena creato deve puntare al componente dei moduli pronto all’uso, in modo che ereditino tutto da essi. Questo viene fatto modificando resourceSuperType nel content.xml del componente proxy. Nel file content.xml si specificano anche il titolo e il gruppo di componenti.
>[!NOTE]
>
> Puoi creare il super tipo di risorsa per ciascuno di [questi componenti da qui](https://github.com/adobe/aem-core-forms-components/tree/master/ui.apps/src/main/content/jcr_root/apps/core/fd/components/formsportal)


### Bozze e invii

Crea una copia di un componente esistente (ad esempio `button`) e denominala come _relatori per parere_.
![relatori per parere](assets/forms-portal-components2.png)
Sostituisci il contenuto nel `.content.xml` con il seguente XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Drafts And Submissions"
          sling:resourceSuperType="core/fd/components/formsportal/draftsandsubmissions/v1/draftsandsubmissions"
          componentGroup="BankingApplication - Content"/>
```

### Ricerca e filtro

Crea una copia del componente pulsante e rinominalo in _ricercatrice_.
Sostituisci il contenuto nel `.content.xml` con il seguente XML:


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Search And Lister"
          sling:resourceSuperType="core/fd/components/formsportal/searchlister/v1/searchlister"
          componentGroup="BankingApplication - Content"/>
```

### Componente collegamento

Crea una copia del componente pulsante e rinominalo in _collegamento_.
Sostituisci il contenuto nel `.content.xml` con il seguente XML:


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Link to Adaptive Form"
          sling:resourceSuperType="core/fd/components/formsportal/link/v2/link"
          componentGroup="BankingApplication - Content"/>
```

Una volta implementato il progetto, dovresti essere in grado di utilizzare questi componenti nella pagina di AEM per creare il portale Forms.