---
title: Registrazione di un servlet tramite il tipo di risorsa
description: Mappatura di un servlet a un tipo di risorsa in AEM Forms CS
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Developer Tools
jira: KT-14581
duration: 90
exl-id: 2a33a9a9-1eef-425d-aec5-465030ee9b74
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 2%

---

# Introduzione

I servlet di binding per percorsi presentano diversi svantaggi rispetto all’associazione per tipi di risorse, ovvero:

* I servlet associati al percorso non possono essere controllati per l’accesso utilizzando gli ACL predefiniti dell’archivio JCR
* I servlet associati al percorso possono essere registrati solo in un percorso e non in un tipo di risorsa (ovvero senza gestione dei suffissi)
* Se un servlet associato al percorso non è attivo, ad esempio se il bundle manca o non viene avviato, un POST potrebbe causare risultati imprevisti. in genere viene creato un nodo in `/bin/xyz` che successivamente sovrappone l&#39;associazione del percorso dei servlet
la mappatura non è trasparente per uno sviluppatore che guarda solo l’archivio
Dati questi inconvenienti, si consiglia di associare i servlet ai tipi di risorse anziché ai percorsi

## Crea servlet

Avvia il progetto di aem-banking in IntelliJ. Crea un servlet denominato GetFieldChoices nella cartella servlet, come illustrato nella schermata seguente.
![scelte](assets/fetchchoices.png)

## Servlet di esempio

Il seguente servlet è associato al tipo di risorsa Sling: _&#x200B;**azure/fetchoptions**&#x200B;_



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
* Creare una risorsa denominata `fetchchoices` (è possibile denominare il nodo come desiderato) di tipo `cq:Page` nel nodo di contenuto.
* Salva le modifiche
* Crea un nodo denominato `jcr:content` di tipo `cq:PageContent` e salva le modifiche
* Aggiungi le seguenti proprietà al nodo `jcr:content`

| Nome proprietà | Valore proprietà |
|--------------------|--------------------|
| jcr:title | Servlet di utilità |
| sling:resourceType | `azure/fetchchoices` |


Il valore `sling:resourceType` deve corrispondere a resourceTypes=&quot;azure/fetchoptions specificato nel servlet.

Ora puoi richiamare il servlet richiedendo la risorsa con `sling:resourceType` = `azure/fetchchoices` nel suo percorso completo, con eventuali selettori o estensioni registrate nel servlet Sling.

```html
http://localhost:4502/content/fetchchoices/jcr:content.json?formPath=/content/forms/af/forrahul/jcr:content/guideContainer
```

Il percorso `/content/fetchchoices/jcr:content` è il percorso della risorsa ed estensione `.json` è ciò che è specificato nel servlet

## Sincronizza il progetto AEM

1. Apri il progetto AEM nel tuo editor preferito. Ho usato intelliJ per questo.
1. Crea una cartella denominata `fetchchoices` in `\aem-banking-application\ui.content\src\main\content\jcr_root\content`
1. Fare clic con il pulsante destro del mouse sulla cartella `fetchchoices` e selezionare `repo | Get Command` (questa voce di menu è impostata in un capitolo precedente di questa esercitazione).

Questo dovrebbe sincronizzare questo nodo da AEM al progetto AEM locale.

La struttura del progetto AEM deve essere simile alla seguente
![risolutore risorse](assets/mapping-servlet-resource.png)
Aggiorna il file filter.xml nella cartella aem-banking-application\ui.content\src\main\content\META-INF\vault con la voce seguente

```xml
<filter root="/content/fetchchoices" mode="merge"/>
```

Ora puoi inviare le modifiche a un ambiente AEM as a Cloud Service utilizzando Cloud Manager.

## Passaggi successivi

[Abilita componenti di Forms Portal](./forms-portal-components.md)
