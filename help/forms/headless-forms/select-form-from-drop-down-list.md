---
title: Selezionare un modulo da un elenco di moduli disponibili
description: Utilizzare l’API listforms per compilare l’elenco a discesa
feature: Adaptive Forms
version: 6.5
jira: KT-13346
topic: Development
role: User
level: Intermediate
exl-id: 49b6a172-8c96-4fc6-8d31-c2109f65faac
duration: 88
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 1%

---

# Selezionare un modulo da compilare da un elenco a discesa

Gli elenchi a discesa forniscono un modo compatto e organizzato per presentare un elenco di opzioni agli utenti. Gli elementi nell’elenco a discesa verranno compilati con i risultati di [API listforms](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms)

![vista a schede](./assets/forms-drop-down.png)

## Elenco a discesa

Il seguente codice è stato utilizzato per popolare l’elenco a discesa con i risultati della chiamata API listforms. In base alla selezione dell’utente, viene visualizzato il modulo adattivo che l’utente deve compilare e inviare. [Componenti dell’interfaccia utente materiale](https://mui.com/) sono stati utilizzati per creare questa interfaccia

```javascript
import * as React from 'react';
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Box from '@mui/material/Box';
import InputLabel from '@mui/material/InputLabel';
import MenuItem from '@mui/material/MenuItem';
import FormControl from '@mui/material/FormControl';
import Select, { SelectChangeEvent } from '@mui/material/Select';
import { AdaptiveForm } from "@aemforms/af-react-renderer";

import { useState,useEffect } from "react";
export default function SelectFormFromDropDownList()
 {
    const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };

const[formID, setFormID] = useState('');
const[afForms,SetOptions] = useState([]);
const [selectedForm, setForm] = useState('');
const HandleChange = (event) =>
     {
        console.log("The form id is "+event.target.value) 
    
        setFormID(event.target.value)
        
     };
const getForm = async () =>
     {
        
        console.log("The formID in getForm"+ formID);
        const resp = await fetch(`/adobe/forms/af/${formID}`);
        let formJSON = await resp.json();
        console.log(formJSON.afModelDefinition);
        setForm(formJSON.afModelDefinition);
     }
const getAFForms =async()=>
     {
        const response = await fetch("/adobe/forms/af/listforms")
        //let myresp = await response.status;
        let myForms = await response.json();
        console.log("Got response"+myForms.items[0].title);
        console.log(myForms.items)
        
        //setFormID('test');
        SetOptions(myForms.items)

        
     }
     useEffect( ()=>{
        getAFForms()
        

    },[]);
    useEffect( ()=>{
        getForm()
        

    },[formID]);

  return (
    <Box sx={{ minWidth: 120 }}>
      <FormControl fullWidth>
        <InputLabel id="demo-simple-select-label">Please select the form</InputLabel>
        <Select
          labelId="demo-simple-select-label"
          id="demo-simple-select"
          value={formID}
          label="Please select a form"
          onChange={HandleChange}
          
        >
       {afForms.map((afForm,index) => (
    
        
          <MenuItem  key={index} value={afForm.id}>{afForm.title}</MenuItem>
        ))}
        
       
        </Select>
      </FormControl>
      <div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
    </Box>
    

  );
  

}
```

Per creare questa interfaccia utente sono state utilizzate le due seguenti chiamate API

* [ListForm](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms). La chiamata per recuperare i moduli viene effettuata una sola volta quando viene eseguito il rendering del componente. I risultati della chiamata API sono memorizzati nella variabile afForms.
Nel codice riportato sopra, scorriamo i moduli af utilizzando la funzione di mappatura. Per ogni elemento nell’array afForms, viene creato e aggiunto al componente Select un componente MenuItem.

* Recupera modulo: viene effettuata una chiamata GET al [getForm](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/Get-Form-Definition), in cui l’id è l’id del modulo adattivo selezionato dall’utente nell’elenco a discesa. Il risultato di questa chiamata di GET viene memorizzato in selectedForm.

```
const resp = await fetch(`/adobe/forms/af/${formID}`);
let formJSON = await resp.json();
console.log(formJSON.afModelDefinition);
setForm(formJSON.afModelDefinition);
```

* Visualizza il modulo selezionato. Il seguente codice è stato utilizzato per visualizzare il modulo selezionato. L’elemento AdaptiveForm viene fornito nel pacchetto npm aemforms/af-react-renderer e prevede le mappature e formJson come proprietà

```
<div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
```

## Passaggi successivi

[Visualizza i moduli nel layout scheda](./display-forms-card-view.md)
