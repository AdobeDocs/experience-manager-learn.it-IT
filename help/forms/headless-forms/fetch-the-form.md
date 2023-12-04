---
title: Recupera il JSON del modulo adattivo da incorporare
description: Utilizza l’API per recuperare il json del modulo adattivo
feature: Adaptive Forms
version: 6.5
jira: KT-13285
topic: Development
role: User
level: Intermediate
exl-id: ee534724-54ea-48e1-8c92-de1c56a928d4
duration: 80
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 1%

---

# Recupera il JSON del modulo

Accedi all’istanza Autore AEM Forms e crea un nuovo file adattivo utilizzando **Vuoto con i Componenti core** modello. Pubblica il modulo nell’istanza di pubblicazione.

Per incorporare il modulo, per prima cosa recuperiamo il json del modulo adattivo effettuando una chiamata di accesso al nostro server di pubblicazione.

Il seguente snippet di codice recupera il json del modulo adattivo denominato **contatto**

```javascript
const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/L2NvbnRlbnQvZm9ybXMvYWYvZmlyc3RoZWFkbGVzcw==');
        // Get the form id manually using the listform api
        let formJSON = await resp.json();
        console.log("The contact form json is "+formJSON);
        setForm(formJSON.afModelDefinition)
      }
```

Di seguito è riportato il codice completo del componente della funzione di contatto.

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Panel from './components/Panel';
import { useState,useEffect } from "react";
import { AdaptiveForm } from "@aemforms/af-react-renderer";

export default function Contact(){
   const [selectedForm, setForm] = useState("");
   const extendMappings = {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
      };
    const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/dor/L2NvbnRlbnQvZm9ybXMvYWYvcmlzaGk=');
        let formJSON = await resp.json();
        setForm(formJSON.afModelDefinition)
      
      }
      useEffect( ()=>{
        getForm();
        

    },[]);
    return(
        
        <div>
            <h1>Get in touch with us!!!!</h1>
            <AdaptiveForm mappings={extendMappings} formJson={selectedForm} />
      
          
        </div>
    )
}
```

Il codice di cui sopra utilizza componenti HTML nativi mappati sui componenti utilizzati nel modulo adattivo. Ad esempio, stiamo mappando il componente del modulo adattivo per l’immissione di testo al componente TextField. I componenti nativi utilizzati nell’articolo [può essere scaricato da qui](./assets/native-components.zip)

## Passaggi successivi

[Seleziona un modulo dall’elenco a discesa](./select-form-from-drop-down-list.md)
