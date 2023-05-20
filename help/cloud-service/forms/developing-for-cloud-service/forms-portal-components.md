---
title: Abilitare i componenti di AEM Forms Portal
description: Creare un portale AEM Forms utilizzando i componenti core
solution: Experience Manager
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 10373
exl-id: ab01573a-e95f-4041-8ccf-16046d723aba
source-git-commit: 69cd5022d136e9fa84f33d2fc5ca249ac0fb6490
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# Componenti di Forms Portal

AEM Forms fornisce i seguenti componenti portale pronti all’uso:

**Ricerca ed elenco**: questo componente consente di elencare i moduli dal repository dei moduli alla pagina del portale e fornisce opzioni di configurazione per elencare i moduli in base a criteri specificati.

**Bozze e invii**: mentre il componente Ricerca ed Elenco visualizza i moduli resi pubblici da Forms Author, il componente Bozze e invii visualizza i moduli salvati come bozza da compilare in un secondo momento e quelli inviati. Questo componente fornisce un’esperienza personalizzata a qualsiasi utente connesso.

**Collegamento**: questo componente consente di creare un collegamento a un modulo in qualsiasi punto della pagina.

## Abilita componenti di Forms Portal

Avviare IntelliJ e aprire il progetto BankingApplication creato in [passaggio precedente.](./getting-started.md) Espandi ui.apps->src->main->content->jcr_root->apps.bankingapplication->components

Per utilizzare qualsiasi componente core (inclusi i componenti portale predefiniti) in un sito Adobe Experience Manager (AEM), è necessario creare un componente proxy e abilitarlo per il sito.
Il componente proxy appena creato deve puntare al componente moduli pronto all’uso, in modo che erediti tutto da lui. Questa operazione viene eseguita modificando resourceSuperType nel file content.xml del componente proxy. Nel content.xml vengono inoltre specificati il titolo e il gruppo di componenti.
>[!NOTE]
>
> Puoi creare il super tipo di risorsa per ciascuno di [questi componenti da qui](https://github.com/adobe/aem-core-forms-components/tree/master/ui.apps/src/main/content/jcr_root/apps/core/fd/components/formsportal)


### Bozze e invii

Creare una copia di un componente esistente (ad esempio `button`) e denominalo come _bozze e presentazioni_.
![bozze e presentazioni](assets/forms-portal-components2.png)
Sostituisci il contenuto nella `.content.xml` con il seguente XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Drafts And Submissions"
          sling:resourceSuperType="core/fd/components/formsportal/draftsandsubmissions/v1/draftsandsubmissions"
          componentGroup="BankingApplication - Content"/>
```

### Ricerca ed elenco

Creare una copia del componente pulsante e rinominarlo _searchandlister_.
Sostituisci il contenuto nella `.content.xml` con il seguente XML:


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Search And Lister"
          sling:resourceSuperType="core/fd/components/formsportal/searchlister/v1/searchlister"
          componentGroup="BankingApplication - Content"/>
```

### Componente collegamento

Creare una copia del componente pulsante e rinominarlo _link_.
Sostituisci il contenuto nella `.content.xml` con il seguente XML:


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Link to Adaptive Form"
          sling:resourceSuperType="core/fd/components/formsportal/link/v2/link"
          componentGroup="BankingApplication - Content"/>
```

Una volta implementato il progetto, dovresti essere in grado di utilizzare questi componenti nella pagina AEM per creare il portale Forms.
