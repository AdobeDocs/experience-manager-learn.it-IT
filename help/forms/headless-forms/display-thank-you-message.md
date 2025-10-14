---
title: Visualizza messaggio di ringraziamento all'invio del modulo
description: Utilizza il gestore onSubmitSuccess per visualizzare il messaggio di ringraziamento configurato nell’app react
feature: Adaptive Forms
version: Experience Manager 6.5
jira: KT-13490
topic: Development
role: User
level: Intermediate
exl-id: 489970a6-1b05-4616-84e8-52b8c87edcda
duration: 61
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 0%

---

# Visualizza il messaggio di ringraziamento configurato

Un messaggio di ringraziamento all&#39;invio del modulo è un modo accorto per riconoscere ed esprimere gratitudine all&#39;utente che ha compilato e inviato il modulo. Ciò conferma che la loro presentazione è stata ricevuta e apprezzata. Il messaggio di ringraziamento viene configurato utilizzando la scheda invio del contenitore della guida del modulo adattivo

![messaggio di ringraziamento](assets/thank-you-message.png)

È possibile accedere al messaggio di ringraziamento configurato nel gestore eventi onSuccess del super componente AdaptiveForm.
Di seguito sono elencati il codice per l&#39;associazione dell&#39;evento onSuccess e il codice per il gestore eventi onSuccess

```javascript
<AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
```

```javascript
const onSuccess=(action) =>{
        let body = action.payload?.body;
        debugger;
        setThankYouMessage(body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));
        console.log("Thank you message "+body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));

      }
```

Di seguito è riportato il codice completo del componente della funzione di contatto.

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import { useState,useEffect } from "react";
import { AdaptiveForm } from "@aemforms/af-react-renderer";
export default function Contact(){
  
    const [selectedForm, setForm] = useState("");
    const [thankYouMessage, setThankYouMessage] = useState("");
    const [formSubmitted, setFormSubmitted] = useState(false);
  
    const extendMappings = {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
      };
     const onSuccess=(action) =>{
        let body = action.payload?.body;
        debugger;
        setFormSubmitted(true);
        setThankYouMessage(body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));
        // Remove any html tags in the thank you message
        console.log("Thank you message "+body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));

      }
      
      const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/L2NvbnRlbnQvZm9ybXMvYWYvY29udGFjdHVz');
        // Get the form id manually using the listform api
        let formJSON = await resp.json();
        setForm(formJSON.afModelDefinition)
      }
      useEffect( ()=>{
        getForm()
        

    },[]);
    
    return(
        
        <div>
           {!formSubmitted ?
            (
                <div>
                    <h1>Get in touch with us!!!!</h1>
                    <AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
                </div>
            ) :
            (
                <div>
                    <div>{thankYouMessage}</div>
                </div>
            )}
        </div>
      
          
        
    )
}
```

Il codice di cui sopra utilizza componenti HTML nativi mappati sui componenti utilizzati nel modulo adattivo. Ad esempio, stiamo mappando il componente del modulo adattivo per l’immissione di testo al componente TextField. I componenti nativi utilizzati nell&#39;articolo [&#x200B; possono essere scaricati da qui](./assets/native-components.zip)
