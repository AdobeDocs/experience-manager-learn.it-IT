---
title: Abilitare i componenti di AEM Forms Portal
description: Creare un portale AEM Forms utilizzando i componenti core
solution: Experience Manager
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Core Components
jira: KT-10373
exl-id: ab01573a-e95f-4041-8ccf-16046d723aba
duration: 78
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 2%

---

# Componenti di Forms Portal

AEM Forms fornisce i seguenti componenti portale pronti all’uso:

**Ricerca ed elenco**: questo componente consente di elencare i moduli dall&#39;archivio dei moduli nella pagina del portale e fornisce opzioni di configurazione per elencare i moduli in base a criteri specificati.

**Bozze e invii**: mentre il componente Ricerca ed elenco visualizza i moduli resi pubblici dall&#39;autore di Forms, il componente Bozze e invii visualizza i moduli salvati come bozza per il completamento successivo e i moduli inviati. Questo componente fornisce un’esperienza personalizzata a qualsiasi utente connesso.

**Collegamento**: questo componente consente di creare un collegamento a un modulo in qualsiasi punto della pagina.

## Abilita componenti di Forms Portal

Avviare IntelliJ e aprire il progetto BankingApplication creato nel passaggio precedente [.](./getting-started.md) Espandere ui.apps->src->main->content->jcr_root->apps.bankingapplication->components

Per utilizzare qualsiasi componente core (inclusi i componenti portale predefiniti) in un sito Adobe Experience Manager (AEM), è necessario creare un componente proxy e abilitarlo per il sito.
Il componente proxy appena creato deve puntare al componente moduli pronto all’uso, in modo che erediti tutto da lui. Questa operazione viene eseguita modificando resourceSuperType nel file content.xml del componente proxy. Nel content.xml vengono inoltre specificati il titolo e il gruppo di componenti.
>[!NOTE]
>
> È possibile creare il super-tipo di risorsa per ciascuno di [questi componenti da qui](https://github.com/adobe/aem-core-forms-components/tree/master/ui.apps/src/main/content/jcr_root/apps/core/fd/components/formsportal)


### Bozze e invii

Creare una copia di un componente esistente (ad esempio `button`) e denominarlo _draftsandsubmissions_.
![invii di bozze](assets/forms-portal-components2.png)
Sostituire il contenuto in `.content.xml` con il seguente XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Drafts And Submissions"
          sling:resourceSuperType="core/fd/components/formsportal/draftsandsubmissions/v1/draftsandsubmissions"
          componentGroup="BankingApplication - Content"/>
```

### Ricerca ed elenco

Creare una copia del componente pulsante e rinominarlo in _searchandlister_.
Sostituire il contenuto in `.content.xml` con il seguente XML:


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
Sostituire il contenuto in `.content.xml` con il seguente XML:


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Link to Adaptive Form"
          sling:resourceSuperType="core/fd/components/formsportal/link/v2/link"
          componentGroup="BankingApplication - Content"/>
```

Una volta implementato il progetto, dovresti essere in grado di utilizzare questi componenti nella pagina AEM per creare il portale Forms.

## Passaggi successivi

[Includi configurazione servizi cloud](./azure-storage-fdm.md)
