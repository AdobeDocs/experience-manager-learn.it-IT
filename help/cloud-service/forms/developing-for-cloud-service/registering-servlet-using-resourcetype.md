---
title: Registrazione di un servlet tramite il tipo di risorsa
description: Mappatura di un servlet a un tipo di risorsa in AEM Forms CS
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
jira: KT-14581
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 2%

---

# Introduzione

I servlet di binding per percorsi presentano diversi svantaggi rispetto all’associazione per tipi di risorse, ovvero:

* I servlet associati al percorso non possono essere controllati per l’accesso utilizzando gli ACL predefiniti dell’archivio JCR
* I servlet associati al percorso possono essere registrati solo in un percorso e non in un tipo di risorsa (ovvero senza gestione dei suffissi)
* Se un servlet associato al percorso non è attivo, ad esempio se il bundle manca o non viene avviato, un POST potrebbe produrre risultati imprevisti. in genere, creazione di un nodo in corrispondenza di `/bin/xyz` che in seguito sovrappone il percorso dei servlet. Il binding della mappatura non è trasparente per uno sviluppatore che guarda solo all’archivio. Dati questi inconvenienti, si consiglia vivamente di associare i servlet ai tipi di risorse anziché ai percorsi

## Crea servlet

Avvia il progetto di aem-banking in IntelliJ. Crea un servlet denominato GetFieldChoices nella cartella servlet, come illustrato nella schermata seguente.
![scelte](assets/fetchchoices.png)

## Servlet di esempio

Il seguente servlet è associato al tipo di risorsa Sling: _**azure/fetchoptions**_



```java
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.jcr.Session;
import javax.servlet.Servlet;
import java.io.IOException;
import java.io.Serializable;

@Component(
        service={Servlet.class }
)

        @SlingServletResourceTypes(
                resourceTypes="azure/fetchchoices",
                methods= "GET",
                extensions="json"
                )


public class GetFieldChoices extends SlingAllMethodsServlet implements Serializable {
    private static final long serialVersionUID = 1L;
    private final  transient Logger log = LoggerFactory.getLogger(this.getClass());


   

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {

        log.debug("The form path I got was "+request.getParameter("formPath"));

    }
}
```

## Creare risorse in CRX

* Accedi al tuo SDK AEM locale.
* Crea una risorsa denominata `fetchchoices` (puoi denominare il nodo come preferisci) di tipo `cq:Page` sotto il nodo del contenuto.
* Salva le modifiche
* Crea un nodo denominato `jcr:content` di tipo `cq:PageContent` e salva le modifiche
* Aggiungi le seguenti proprietà alla `jcr:content` nodo

| Nome proprietà | Valore proprietà |
|--------------------|--------------------|
| jcr:title | Servlet di utilità |
| sling:resourceType | `azure/fetchchoices` |


Il `sling:resourceType` il valore deve corrispondere a resourceTypes=&quot;azure/fetchoptions specificato nel servlet.

Ora puoi richiamare il servlet richiedendo la risorsa con `sling:resourceType` = `azure/fetchchoices` nel suo percorso completo, con eventuali selettori o estensioni registrati nel servlet Sling.

```html
http://localhost:4502/content/fetchchoices/jcr:content.json?formPath=/content/forms/af/forrahul/jcr:content/guideContainer
```

Il percorso `/content/fetchchoices/jcr:content` è il percorso della risorsa e dell’estensione `.json` è quanto specificato nel servlet

## Sincronizza il progetto AEM

1. Apri il progetto AEM nel tuo editor preferito. Ho usato intelliJ per questo.
1. Crea una cartella denominata `fetchchoices` in `\aem-banking-application\ui.content\src\main\content\jcr_root\content`
1. Clic destro `fetchchoices` cartella e seleziona `repo | Get Command` Questa voce di menu è impostata in un capitolo precedente di questa esercitazione.

Questo dovrebbe sincronizzare il nodo dall’AEM al progetto AEM locale.

La struttura del progetto AEM deve essere simile alla seguente
![risolutore risorse](assets/mapping-servlet-resource.png)
Aggiorna il file filter.xml nella cartella aem-banking-application\ui.content\src\main\content\META-INF\vault con la voce seguente

```xml
<filter root="/content/fetchchoices" mode="merge"/>
```

Ora puoi inviare le modifiche a un ambiente as a Cloud Service AEM utilizzando Cloud Manager.

## Passaggi successivi

[Abilita componenti di Forms Portal](./forms-portal-components.md)



