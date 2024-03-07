---
title: Salvataggio e recupero di dati di moduli adattivi
description: Salvataggio e recupero dei dati dei moduli adattivi dal database. Questa funzionalità consente ai compilatori di moduli di salvare il modulo e continuare a compilarlo in una data successiva.
feature: Adaptive Forms
topic: Development
role: Developer
type: Tutorial
version: 6.4,6.5
last-substantial-update: 2019-06-09T00:00:00Z
duration: 691
source-git-commit: 4f196539ea73d25b480064f7fc349f0ea29d5e0a
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 0%

---


# Salvataggio e recupero di dati di moduli adattivi

Questo articolo illustra i passaggi necessari per salvare e recuperare i dati dei moduli adattivi dal database. Database MySQL utilizzato per memorizzare i dati del modulo adattivo. Ad alto livello, per ottenere il caso d’uso sono necessari i seguenti passaggi:

* [Configura origine dati](#Configure-Data-Source)
* [Creare un servlet per scrivere i dati nel database](#create-servlet)
* [Crea servizio OSGI per recuperare i dati memorizzati](#create-osgi-service)
* [Crea libreria client](#create-client-library)
* [Creare un modello di modulo adattivo e il componente Pagina](#form-template-and-page-component)
* [Dimostrazione delle funzionalità](#capability-demo)
* [Distribuisci sul server](#deploy-on-your-server)

## Configura origine dati {#Configure-Data-Source}

L’origine dati in pool di connessione Apache Sling è configurata per puntare al database che verrà utilizzato per memorizzare i dati del modulo adattivo. La schermata seguente mostra la configurazione della mia istanza. Le seguenti proprietà possono essere copiate e incollate

* `Datasource Name:aemformstutorial` - Questo è il nome utilizzato nel mio codice.

* `JDBC Driver Class:com.mysql.jdbc.Driver`

* `JDBC Connection URL:jdbc:mysql://localhost:3306/aemformstutorial`

![connection pool](assets/storingdata.PNG)

### Crea servlet {#create-servlet}

Di seguito è riportato il codice del servlet che inserisce/aggiorna i dati del modulo adattivo nel database. L’origine dati in pool di connessione Apache Sling è configurata utilizzando AEM ConfigMgr e alla riga 26 viene fatto riferimento allo stesso. Il resto del codice è abbastanza semplice. Il codice inserisce una nuova riga nel database o aggiorna una riga esistente. I dati del modulo adattivo memorizzati sono associati a un GUID. Lo stesso GUID viene quindi utilizzato per aggiornare i dati del modulo.

```java
package com.techmarketing.core.servlets;
import java.io.IOException;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.UUID;
import javax.servlet.Servlet;
import javax.sql.DataSource;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.json.JSONException;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
 
@Component(service = { Servlet.class}, property = {"sling.servlet.methods=post","sling.servlet.paths=/bin/storeafdata"})
public class StoreDataInDB extends SlingAllMethodsServlet {
     private static final Logger log = LoggerFactory.getLogger(StoreDataInDB.class);
        private static final long serialVersionUID = 1L;
     @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformstutorial))")
        private DataSource dataSource;
    public String updateData(String afdata,String guid)
    {
         String updateTableSQL = "update aemformstutorial.formdata set afdata= ? where guid = ?";
         Connection c = getConnection();
            PreparedStatement pstmt = null;
            try {
      
                pstmt = null;
                pstmt = c.prepareStatement(updateTableSQL);
                pstmt.setString(1,afdata);
                pstmt.setString(2,guid);
                log.debug("Executing the insert statment  " + pstmt.executeUpdate());
                c.commit();
                 
      
            } catch (SQLException e) {
      
                log.error("Getting errors", e);
            } finally {
                if (pstmt != null) {
                    try {
                        pstmt.close();
                    } catch (SQLException e) {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                    }
                }
                if (c != null) {
                    try {
                        c.close();
                    } catch (SQLException e) {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                    }
                }
            }
            return guid;
     
         
    }
     public String insertData(String afdata) {
            log.debug("### Insert Data #### The json object I got to insert was " + afdata);
            String insertTableSQL = "INSERT INTO aemformstutorial.formdata(guid,afdata) VALUES(?,?)";
            UUID uuid = UUID.randomUUID();
            String randomUUIDString = uuid.toString();
            log.debug("The query is " + insertTableSQL);
            Connection c = getConnection();
            PreparedStatement pstmt = null;
            try {
      
                pstmt = null;
                pstmt = c.prepareStatement(insertTableSQL);
                pstmt.setString(1,randomUUIDString);
                pstmt.setString(2,afdata);
                log.debug("Executing the insert statment  " + pstmt.executeUpdate());
                c.commit();
                 
      
            } catch (SQLException e) {
      
                log.error("Getting errors", e);
            } finally {
                if (pstmt != null) {
                    try {
                        pstmt.close();
                    } catch (SQLException e) {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                    }
                }
                if (c != null) {
                    try {
                        c.close();
                    } catch (SQLException e) {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                    }
                }
            }
            return randomUUIDString;
        }
      
     public Connection getConnection() {
            log.debug("Getting Connection ");
            Connection con = null;
            try {
                con = dataSource.getConnection();
                log.debug("got connection");
                return con;
            } catch (Exception e) {
                log.error("not able to get connection ", e);
            }
            return null;
        }
    protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response)
    {
        log.debug("Inside my save af data servlet");
        if(request.getParameter("operation").equalsIgnoreCase("update"))
        {
            log.debug("The operation is update");
            log.debug("The data I got was "+request.getParameter("formdata"));
            String guid = updateData(request.getParameter("formdata"),request.getParameter("guid"));
            log.debug("The guid I got was  "+guid);
            JSONObject jsonResponse = new JSONObject();
            try {
                jsonResponse.put("guid",guid);
                response.setContentType("application/json");
                response.setCharacterEncoding("UTF-8");
                response.getWriter().write(jsonResponse.toString());
            } catch (JSONException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
        if(request.getParameter("operation").equalsIgnoreCase("insert"))
        {
            log.debug("The data I got was +request.getParameter("formdata");
            String guid = insertData(request.getParameter("formdata"));
            log.debug("The guid on inserting data  "+guid);
            JSONObject jsonResponse = new JSONObject();
            try {
                jsonResponse.put("guid",guid);
                response.setContentType("application/json");
                response.setCharacterEncoding("UTF-8");
                response.getWriter().write(jsonResponse.toString());

} catch (JSONException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
 
}
```

## Creare un servizio OSGI per recuperare i dati {#create-osgi-service}

Il seguente codice è stato scritto per recuperare i dati memorizzati del modulo adattivo. Viene utilizzata una query semplice per recuperare i dati del modulo adattivo associati a un determinato GUID. I dati recuperati vengono quindi restituiti all&#39;applicazione chiamante. La stessa origine dati creata nel primo passaggio a cui si fa riferimento in questo codice.

```java
package com.techmarketing.core.impl;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
 
import javax.sql.DataSource;
 
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
 
import com.techmarketing.core.AemFormsAndDB;
 
 
@Component(service=AemFormsAndDB.class,immediate = true)
public class AemformWithDB implements AemFormsAndDB {
    private final Logger log = LoggerFactory.getLogger(getClass());
     @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformstutorial))")
        private DataSource dataSource;
 
    @Override
    public String getData(String guid) {
        System.out.println("### inside my getData of AemformWithDB");
        Connection con = getConnection();
        try {
            Statement st = con.createStatement();
            String query = "SELECT afdata FROM aemformstutorial.formdata where guid = '"+guid+"'"+"";
            log.debug(" Got Result Set"+query);
            ResultSet rs = st.executeQuery(query);
            while(rs.next())
            {
                return rs.getString("afdata");
            }
        } catch (SQLException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
 
        return null;
    }
     public Connection getConnection() {
            log.debug("Getting Connection ");
            Connection con = null;
            try {
                con = dataSource.getConnection();
                log.debug("got connection");
                return con;
            } catch (Exception e) {
                log.debug("not able to get connection ");
                e.printStackTrace();
            }
            return null;
        }
 
 
}
```

## Crea libreria client {#create-client-library}

La libreria client AEM gestisce tutto il codice JavaScript lato client. Per questo articolo, ho creato un semplice JavaScript per recuperare i dati del Modulo adattivo utilizzando l’API del ponte guida. Una volta recuperati i dati del modulo adattivo, viene effettuata la chiamata POST al servlet per inserire o aggiornare i dati del modulo adattivo nel database. La funzione getALLUrlParams restituisce i parametri nell’URL. Viene utilizzato quando si desidera aggiornare i dati. Il resto della funzionalità viene gestito nel codice associato all&#39;evento click della classe .savebutton. Se il parametro guid è presente nell’URL, è necessario eseguire l’operazione di aggiornamento, in caso contrario si tratta di un’operazione di inserimento.

```javascript
function getAllUrlParams(url) {
 
  // get query string from url (optional) or window
  var queryString = url ? url.split('?')[1] : window.location.search.slice(1);
 
  // we'll store the parameters here
  var obj = {};
 
  // if query string exists
  if (queryString) {
 
    // stuff after # is not part of query string, so get rid of it
    queryString = queryString.split('#')[0];
 
    // split our query string into its component parts
    var arr = queryString.split('&');
 
    for (var i = 0; i < arr.length; i++) {
      // separate the keys and the values
      var a = arr[i].split('=');
 
      // set parameter name and value (use 'true' if empty)
      var paramName = a[0];
      var paramValue = typeof (a[1]) === 'undefined' ? true : a[1];
 
      // (optional) keep case consistent
      paramName = paramName.toLowerCase();
      if (typeof paramValue === 'string') paramValue = paramValue.toLowerCase();
 
      // if the paramName ends with square brackets, e.g. colors[] or colors[2]
      if (paramName.match(/\[(\d+)?\]$/)) {
 
        // create key if it doesn't exist
        var key = paramName.replace(/\[(\d+)?\]/, '');
        if (!obj[key]) obj[key] = [];
 
        // if it's an indexed array e.g. colors[2]
        if (paramName.match(/\[\d+\]$/)) {
          // get the index value and add the entry at the appropriate position
          var index = /\[(\d+)\]/.exec(paramName)[1];
          obj[key][index] = paramValue;
        } else {
          // otherwise add the value to the end of the array
          obj[key].push(paramValue);
        }
      } else {
        // we're dealing with a string
        if (!obj[paramName]) {
          // if it doesn't exist, create property
          obj[paramName] = paramValue;
        } else if (obj[paramName] && typeof obj[paramName] === 'string'){
          // if property does exist and it's a string, convert it to an array
          obj[paramName] = [obj[paramName]];
          obj[paramName].push(paramValue);
        } else {
          // otherwise add the property
          obj[paramName].push(paramValue);
        }
      }
    }
  }
 
  return obj;
}
 
$(document).ready(function()
   {
        var linktext = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
        var linktext1 = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktext1[0]");
       linktext.visible = false;
       linktext1.visible = false;
        $(".savebutton").click(function(){
           var params = getAllUrlParams(window.location.href);
           console.log(getAllUrlParams(window.location.href));
            window.guideBridge.getDataXML({
                 success:function(guideResultObject) 
                 {
                     console.log("The data is "+guideResultObject.data);
                     let xhr = new XMLHttpRequest();
                      xhr.open('POST','/bin/storeafdata');
                     let formData = new FormData();
                     if(typeof(params.guid)!="undefined")
                     {
                         formData.append("operation","update");
                         formData.append("guid",params.guid);
 
                     }
                     if(typeof(params.guid)=="undefined")
                     {
                         formData.append("operation","insert");
 
 
                     }
 
 
                formData.append("formdata",guideResultObject.data);
                xhr.send(formData);
                     xhr.onload = function(e)
                {
                    console.log("The data is ready");
                    if (this.status == 200)
                        {
                            var jsonResponse = JSON.parse(this.response);
                            console.log(jsonResponse.guid);
                            var linktext = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
                            var linktext1 = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktext1[0]");
                            linktext1.visible = true;
                            linktext.value = "http://localhost:4502/content/dam/formsanddocuments/saveformdata/jcr:content?wcmmode=disabled&guid="+jsonResponse.guid;
                            linktext.visible = true;
                            guideBridge.setFocus("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
                        }
 
                }
                  }
             });
 
 
       });
 
 
});
```

## Creare un modello di modulo adattivo e il componente Pagina {#form-template-and-page-component}


>[!VIDEO](https://video.tv.adobe.com/v/27828?quality=12&learn=on)

### Dimostrazione della capacità {#capability-demo}

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=12&learn=on)

#### Distribuisci sul server {#deploy-on-your-server}

Per testare questa funzionalità nell’istanza di AEM Forms, segui i passaggi seguenti

* [Scarica e decomprimi DemoAssets.zip sul sistema locale](assets/demoassets.zip)
* Implementa e avvia i bundle techmarketingdemos.jar e mysqldriver.jar utilizzando la console web Felix.
*** Importare aemformstutorial.sql utilizzando MYSQL Workbench. Verranno creati lo schema e le tabelle necessari nel database
* Importa StoreAndRetrieve.zip utilizzando Gestione pacchetti AEM. Questo pacchetto contiene il modello di modulo adattivo, la libreria client del componente Pagina e un esempio di configurazione del modulo adattivo e dell’origine dati.
* Accedi a configMgr. Cerca &quot;Apache Sling Connection Pooled DataSource&quot;. Aprire la voce dell&#39;origine dati associata a aemformstutorial e immettere il nome utente e la password specifici per l&#39;istanza di database.
* Aprire il modulo adattivo
* Immettere alcuni dettagli e fare clic sul pulsante Salva e continua in seguito
* È necessario recuperare un URL contenente un GUID.
* Copia l’URL e incollalo in una nuova scheda del browser
* Il modulo adattivo deve essere compilato con i dati del passaggio precedente**
