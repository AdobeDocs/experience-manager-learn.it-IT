---
title: Visualizzare i moduli recuperati nella vista a schede
description: Utilizzare l’API listforms per visualizzare i moduli
feature: Adaptive Forms
version: 6.5
kt: 13311
topic: Development
role: User
level: Intermediate
source-git-commit: 6aa3dff44a7e6f1f8ac896e30319958d84ecf57f
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 0%

---


# Recupera e visualizza i moduli in formato scheda

Il formato di visualizzazione delle schede è un modello di progettazione che presenta informazioni o dati sotto forma di schede. Ogni scheda rappresenta una parte discreta di contenuto o immissione di dati e in genere è costituita da un contenitore visivamente distinto con elementi specifici disposti al suo interno. In questo articolo utilizzeremo [API listforms](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms) per recuperare i moduli e visualizzarli in formato scheda, come illustrato di seguito

![vista a schede](./assets/card-view-forms.png)

## Modello scheda

Il codice seguente è stato utilizzato per progettare il modello di scheda. Nel modello di scheda vengono visualizzati il titolo e la descrizione del modulo adattivo e il logo di Adobe. [Componenti dell’interfaccia utente materiale](https://mui.com/) sono stati utilizzati nella creazione di questo layout.

```javascript
import Paper from "@mui/material/Paper";
import Grid from "@mui/material/Grid";
import Container from "@mui/material/Container";
import { Typography } from "@mui/material";
import { Box } from "@mui/system";
const FormCard =({headlessForm}) => {
    return (
              <Grid item xs={3}>
                <Paper elevation={3}>
                    <img src="/content/dam/formsanddocuments/registrationform/jcr:content/renditions/cq5dam.thumbnail.48.48.png" className="img"/>
                    <Box padding={3}>
                    <Typography variant="subtititle2" component="h2">
                        {headlessForm.title}
                    
                    </Typography>
                    <Typography variant="subtititle3" component="h4">
                        {headlessForm.description}
                    
                    </Typography>
                    </Box>
                </Paper>
                </Grid>
          


    );
    

};
export default FormCard;
```

## Recuperare i moduli

L’API listforms è stata utilizzata per recuperare i moduli dal server AEM. L’API restituisce un array di oggetti JSON; ogni oggetto JSON rappresenta un modulo.

```javascript
import { useState,useEffect } from "react";
import React, { Component } from "react";
import FormCard from "./components/FormCard";
import Grid from "@mui/material/Grid";
import Paper from "@mui/material/Paper";
import Container from "@mui/material/Container";
 
export default function ListForm(){
    const [fetchedForms,SetHeadlessForms] = useState([])
    const getForms=async()=>{
        const response = fetch("/adobe/forms/af/listforms")
        let headlessForms = await (await response).json();
        console.log(headlessForms.items);
        SetHeadlessForms(headlessForms.items);
    }
    useEffect( ()=>{
        getForms()
        

    },[]);
    return(
        <div>
             <div>
                <Container>
                   <Grid container spacing={3}>
                       {
                            fetchedForms.map( (afForm,index) =>
                                <FormCard headlessForm={afForm} key={index}/>
                         
                            )
                        }
                    </Grid>
                </Container>
             </div>

        </div>
    )
}
```

Nel codice riportato sopra, scorriamo i Form recuperati utilizzando la funzione di mappatura e per ogni elemento nell’array fetchedForms viene creato e aggiunto al contenitore Grid un componente FormCard. Ora puoi utilizzare il componente ListForm nell’app React in base alle tue esigenze.
