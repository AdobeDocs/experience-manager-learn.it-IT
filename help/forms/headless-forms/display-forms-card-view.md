---
title: Visualizzare i moduli recuperati nella vista a schede
description: Utilizzare l’API listforms per visualizzare i moduli
feature: Adaptive Forms
version: 6.5
kt: 13311
topic: Development
role: User
level: Intermediate
exl-id: c01ad68e-23c9-4564-8e3e-1924af34a493
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---

# Recupera e visualizza i moduli in formato scheda

Il formato di visualizzazione delle schede è un modello di progettazione che presenta informazioni o dati sotto forma di schede. Ogni scheda rappresenta una parte discreta di contenuto o immissione di dati e in genere è costituita da un contenitore visivamente distinto con elementi specifici disposti al suo interno.
Le schede cliccabili in React sono componenti interattivi che assomigliano a schede o tessere e possono essere cliccate o toccate dall’utente. Quando un utente fa clic o tocca una scheda su cui è possibile fare clic, attiva un’azione o un comportamento specifico, ad esempio passare a un’altra pagina, aprire un modulo o aggiornare l’interfaccia utente.

In questo articolo utilizzeremo [API listforms](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms) per recuperare i moduli e visualizzarli in formato scheda e aprire il modulo adattivo all’evento clic.

![vista a schede](./assets/card-view-forms.png)

## Modello scheda

Il codice seguente è stato utilizzato per progettare il modello di scheda. Nel modello di scheda vengono visualizzati il titolo e la descrizione del modulo adattivo e il logo di Adobe. [Componenti dell’interfaccia utente materiale](https://mui.com/) sono stati utilizzati nella creazione di questo layout.



```javascript
import Container from "@mui/material/Container";
import Form from './Form';
import PlainText from './plainText'
import TextField from './TextField'
import Button from './Button';
import { AdaptiveForm } from "@aemforms/af-react-renderer";

import { CardActionArea, Typography } from "@mui/material";
import { Box } from "@mui/system";
import { useState,useEffect } from "react";
import DisplayForm from "../DisplayForm";
import { Link } from "react-router-dom";
export default function FormCard({headlessForm}) {
const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };
   
    return (
        
            <Grid item xs={3}>
                <Paper elevation={3}>
                    <img src="/content/dam/formsanddocuments/registrationform/jcr:content/renditions/cq5dam.thumbnail.48.48.png" className="img"/>
                    <Box padding={3}>
                        <Link style={{ textDecoration: 'none' }} to={`/displayForm${headlessForm.id}`}>
                            <Typography variant="subtititle2" component="h2">
                                {headlessForm.title}
                            </Typography>
                            <Typography variant="subtititle3" component="h4">
                                {headlessForm.description}
                            </Typography>
                        </Link>
                
                    </Box>
                </Paper>
            </Grid>
    );
    

};
```

La seguente route è stata definita in Main.js per passare a DisplayForm.js

```javascript
    <Route path="/displayForm/:formID" element={<DisplayForm/>} exact/>
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

## Passaggi successivi

[Visualizzare il modulo adattivo quando l’utente fa clic su una scheda](./open-form-card-view.md)
