---
title: Generazione di immagini OpenAI tramite un’estensione personalizzata della Console Frammenti di contenuto
description: Scopri come generare un’immagine digitale dalla descrizione del linguaggio naturale utilizzando OpenAI o DALL-E 2 e caricare l’immagine generata nell’AEM utilizzando un’estensione della console Frammenti di contenuto personalizzata.
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11649
thumbnail: KT-11649.png
doc-type: article
last-substantial-update: 2023-01-04T00:00:00Z
exl-id: f3047f1d-1c46-4aee-9262-7aab35e9c4cb
source-git-commit: 6b5c755bd8fe6bbf497895453b95eb236f69d5f6
workflow-type: tm+mt
source-wordcount: '1396'
ht-degree: 1%

---

# Generazione di risorse di immagini AEM tramite OpenAI

Scopri come generare un’immagine utilizzando OpenAI o DALL.E 2 e caricarla in AEM DAM per velocizzare la trasmissione dei contenuti.

>[!VIDEO](https://video.tv.adobe.com/v/3413093?quality=12&learn=on)

Questo esempio di estensione della console Frammenti di contenuto AEM è un [barra delle azioni](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/) che genera l’immagine digitale dall’input del linguaggio naturale utilizzando [API OpenAI](https://openai.com/api/) o [DALL.E 2](https://openai.com/dall-e-2/). L’immagine generata viene caricata nel DAM dell’AEM e la proprietà immagine del frammento di contenuto selezionato viene aggiornata per fare riferimento a questa immagine appena generata e caricata da DAM.

In questo esempio imparerai:

1. Generazione di immagini tramite [API OpenAI](https://beta.openai.com/docs/guides/images/image-generation-beta) o [DALL.E 2](https://openai.com/dall-e-2/)
2. Caricamento di immagini sull’AEM
3. Aggiornamento proprietà frammento di contenuto

Il flusso funzionale dell’estensione di esempio è il seguente:

![Flusso di azione di Adobe I/O Runtime per la generazione di immagini digitali](./assets/digital-image-generation/flow.png){align="center"}

1. Seleziona Frammento di contenuto e fai clic su dell’estensione `Generate Image` pulsante in [barra delle azioni](#extension-registration) apre il [modale](#modal).
1. Il [modale](#modal) visualizza un modulo di input personalizzato creato con [Spettro di reazione](https://react-spectrum.adobe.com/react-spectrum/).
1. L’invio del modulo invia l’utente fornito `Image Description` testo, il frammento di contenuto selezionato e l’host AEM per [azione Adobe I/O Runtime personalizzata](#adobe-io-runtime-action).
1. Il [Azione Adobe I/O Runtime](#adobe-io-runtime-action) convalida gli input.
1. Poi chiama OpenAI’s [Generazione di immagini](https://beta.openai.com/docs/guides/images/image-generation-beta) API e utilizza `Image Description` testo per specificare l&#39;immagine da generare.
1. Il [generazione di immagini](https://beta.openai.com/docs/guides/images/image-generation-beta) l&#39;endpoint crea un&#39;immagine originale di dimensioni _1024x1024_ pixel utilizzando il valore del parametro della richiesta di prompt e restituisce l’URL dell’immagine generato come risposta.
1. Il [Azione Adobe I/O Runtime](#adobe-io-runtime-action) scarica l’immagine generata in fase di esecuzione di App Builder.
1. Quindi avvia il caricamento dell’immagine dal runtime di App Builder a AEM DAM in un percorso predefinito.
1. L’as a Cloud Service AEM salva l’immagine in DAM e restituisce le risposte di esito positivo o negativo all’azione Adobe I/O Runtime. La risposta di caricamento corretta aggiorna il valore della proprietà dell’immagine del frammento di contenuto selezionato utilizzando un’altra richiesta HTTP all’AEM dall’azione Adobe I/O Runtime.
1. Il modale riceve la risposta dall’azione Adobe I/O Runtime e fornisce il collegamento dei dettagli della risorsa AEM dell’immagine appena generata e caricata.

## Punto di estensione

Questo esempio si estende al punto di estensione `actionBar` per aggiungere un pulsante personalizzato alla console Frammenti di contenuto.

| Interfaccia utente AEM estesa | Punto di estensione |
| ------------------------ | --------------------- | 
| [Console Frammenti di contenuto](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/) | [Barra delle azioni](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/) |

## Estensione di esempio

L’esempio utilizza un progetto Adobe Developer Console esistente e le seguenti opzioni durante l’inizializzazione dell’app di App Builder tramite `aio app init`.

+ Selezionare i modelli da cercare: `All Extension Points`
+ Scegliere i modelli da installare:` @adobe/aem-cf-admin-ui-ext-tpl`
+ Nome dell’estensione: `Image generation`
+ Fornisci una breve descrizione dell’estensione: `An example action bar extension that generates an image using OpenAI and uploads it to AEM DAM.`
+ Quale versione vuoi iniziare?: `0.0.1`
+ Come procedere?
   + `Add a custom button to Action Bar`
      + Fornisci il nome dell’etichetta del pulsante: `Generate Image`
      + È necessario visualizzare un modale per il pulsante? `y`
   + `Add server-side handler`
      + Adobe I/O Runtime consente di richiamare il codice senza server su richiesta. Specificare il nome dell&#39;azione: `generate-image`

L&#39;app generata per l&#39;estensione App Builder viene aggiornata come descritto di seguito.

### Configurazione iniziale

1. Registrati gratis [API OpenAI](https://openai.com/api/) e creare un [Chiave API](https://beta.openai.com/account/api-keys)
1. Aggiungi questa chiave al progetto App Builder `.env` file

   ```
       # Specify your secrets here
       # This file must not be committed to source control
       ## Adobe I/O Runtime credentials
       ...
       AIO_runtime_apihost=https://adobeioruntime.net
       ...
       # OpenAI secret API key
       OPENAI_API_KEY=my-openai-secrete-key-to-generate-images
       ...
   ```

1. Superato `OPENAI_API_KEY` come parametro per l&#39;azione Adobe I/O Runtime, aggiorna il `src/aem-cf-console-admin-1/ext.config.yaml`

   ```yaml
       ...
   
       runtimeManifest:
         packages:
           aem-cf-console-admin-1:
             license: Apache-2.0
             actions:
               generate-image:
                 function: actions/generate-image/index.js
                 web: 'yes'
                 runtime: nodejs:16
                 inputs:
                   LOG_LEVEL: debug
                   OPENAI_API_KEY: $OPENAI_API_KEY
       ...
   ```

1. Installa nelle librerie di Node.js
   1. [Libreria OpenAI Node.js](https://github.com/openai/openai-node#installation) - per richiamare facilmente l’API OpenAI
   1. [Caricamento AEM](https://github.com/adobe/aem-upload#install) - per caricare immagini nelle istanze AEM-CS.


>[!TIP]
>
>Nelle sezioni seguenti vengono fornite informazioni sui file JavaScript delle azioni chiave React e Adobe I/O Runtime. Per riferimento, i file chiave da `web-src` e  `actions` del progetto AppBuilder, vedere [adobe-appbuilder-cfc-ext-image-generation-code.zip](./assets/digital-image-generation/adobe-appbuilder-cfc-ext-image-generation-code.zip).


### Route delle app{#app-routes}

Il `src/aem-cf-console-admin-1/web-src/src/components/App.js` contiene [Router React](https://reactrouter.com/en/main).

Esistono due insiemi logici di route:

1. La prima route mappa le richieste a `index.html`, che richiama il componente React responsabile della [registrazione dell&#39;estensione](#extension-registration).

   ```javascript
   <Route index element={<ExtensionRegistration />} />
   ```

1. Il secondo set di route associa gli URL ai componenti React che eseguono il rendering del contenuto della finestra modale dell’estensione. Il `:selection` param rappresenta un percorso di frammento di contenuto elenco delimitato.

   Se l’estensione dispone di più pulsanti per richiamare azioni discrete, ciascuno [registrazione dell&#39;estensione](#extension-registration) viene mappato su un percorso definito qui.

   ```javascript
   <Route
       exact path="content-fragment/:selection/generate-image-modal"
       element={<GenerateImageModal />}
       />
   ```

### Registrazione dell’estensione

`ExtensionRegistration.js`, mappato a `index.html` è il punto di ingresso per l&#39;estensione AEM e definisce:

1. La posizione del pulsante di estensione viene visualizzata nell’esperienza di authoring AEM (`actionBar` o `headerMenu`)
1. Definizione del pulsante di estensione in `getButton()` funzione
1. Il gestore di clic per il pulsante, nel `onClick()` funzione

+ `src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'generate-image',     // Unique ID for the button
              'label': 'Generate Image',  // Button label 
              'icon': 'PublishCheck'      // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },

          // Click handler for the extension button
          onClick(selections) {
            // Collect the selected content fragment paths 
            const selectionIds = selections.map(selection => selection.id);

            // Create a URL that maps to the 
            const modalURL = "/index.html#" + generatePath(
              "/content-fragment/:selection/generate-image-modal",
              {
                // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
                selection: encodeURIComponent(selectionIds.join('|')),
              }
            );

            // Open the route in the extension modal using the constructed URL
            guestConnection.host.modal.showUrl({
              title: "Generate Image",
              url: modalURL
            })
          }
        },

      }
    })
  }
  init().catch(console.error)
```

### Finestra modale

Ogni percorso dell&#39;estensione, come definito in [`App.js`](#app-routes), viene mappato su un componente React che viene riprodotto nel modale dell’estensione.

In questa app di esempio è presente un componente React modale (`GenerateImageModal.js`) che ha quattro stati:

1. Caricamento, che indica che l’utente deve attendere
1. Il messaggio di avviso che suggerisce agli utenti di selezionare un solo frammento di contenuto alla volta
1. Il modulo Genera immagine che consente all’utente di fornire una descrizione dell’immagine nel linguaggio naturale.
1. La risposta dell’operazione di generazione delle immagini, che fornisce il collegamento dei dettagli delle risorse AEM dell’immagine caricata appena generata.

È importante sottolineare che qualsiasi interazione con l’AEM derivante dall’estensione deve essere delegata ad un [Azione AppBuilder Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/), un processo separato senza server in esecuzione in [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/).
L’utilizzo delle azioni di Adobe I/O Runtime per comunicare con l’AEM ed evitare problemi di connettività CORS (Cross-Origin Resource Sharing).

Quando _Genera immagine_ viene inviato un modulo personalizzato `onSubmitHandler()` richiama l’azione Adobe I/O Runtime, passando la descrizione dell’immagine, l’host AEM corrente (dominio) e il token di accesso AEM dell’utente. L’azione chiama quindi OpenAI’s [Generazione di immagini](https://beta.openai.com/docs/guides/images/image-generation-beta) API per generare un’immagine utilizzando la descrizione dell’immagine inviata. Utilizzo successivo [Caricamento AEM](https://github.com/adobe/aem-upload) del modulo nodo `DirectBinaryUpload` classe carica l&#39;immagine generata su AEM e infine utilizza [API per frammenti di contenuto AEM](https://experienceleague.adobe.com/docs/experience-manager-65/assets/extending/assets-api-content-fragments.html) per aggiornare i frammenti di contenuto.

Quando viene ricevuta la risposta dall’azione Adobe I/O Runtime, il modale viene aggiornato per visualizzare i risultati dell’operazione di generazione dell’immagine.

+ `src/aem-cf-console-admin-1/web-src/src/components/GenerateImageModal.js`

```javascript
export default function GenerateImageModal() {
  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the application state
  const [imageDescription, setImageDescription] = useState(null);
  const [validationState, setValidationState] = useState({});

  const [actionInvokeInProgress, setActionInvokeInProgress] = useState(false);
  const [actionResponse, setActionResponse] = useState();

  // Get the selected content fragment paths from the route parameter `:selection`
  const { selection } = useParams();
  const fragmentIds = selection?.split('|') || [];

  console.log('Selected Fragment Ids', fragmentIds);

  if (!fragmentIds || fragmentIds.length === 0) {
    console.error('The Content Fragments are not selected, can NOT generate images');
    return;
  }

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Determine view to display in the modal
  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />;
  } if (actionInvokeInProgress) {
    // If the 'Generate Image' action has been invoked but not completed, display a loading spinner
    return <Spinner />;
  } if (fragmentIds.length > 1) {
    // If more than one CF selected show warning and suggest to select only one CF
    return renderMoreThanOneCFSelectionError();
  } if (fragmentIds.length === 1 && !actionResponse) {
    // Display the 'Generate Image' modal and ask for image description
    return renderImgGenerationForm();
  } if (actionResponse) {
    // If the 'Generate Image' actio has completed, display the response
    return renderActionResponse();
  }

  /**
   * Renders the message suggesting to select only on CF at a time to not lose credits accidentally
   *
   * @returns the suggestion or error message to select one CF at a time
   */
  function renderMoreThanOneCFSelectionError() {
    return (
      <Provider theme={defaultTheme} colorScheme="light">
        <Content width="100%">
          <Text>
            As this operation
            <strong> uses credits from Generative AI services</strong>
            {' '}
            such as DALL.E 2 (or Stable Dufusion), we allow only one Generate Image at a time.
            <p />
            <strong>So please select only one Content Fragment at this moment.</strong>
          </Text>

          <Flex width="100%" justifyContent="end" alignItems="center" marginTop="size-400">
            <ButtonGroup align="end">
              <Button variant="negative" onPress={() => guestConnection.host.modal.close()}>Close</Button>
            </ButtonGroup>
          </Flex>

        </Content>
      </Provider>
    );
  }

  /**
   * Renders the form asking for image description in the natural language and
   * displays message this action uses credits from Generative AI services.
   *
   *
   * @returns the image description input field and credit usage message
   */
  function renderImgGenerationForm() {
    return (

      <Provider theme={defaultTheme} colorScheme="light">
        <Content width="100%">

          <Flex width="100%">
            <Form
              width="100%"
            >
              <TextField
                label="Image Description"
                description="The image description in natural language, for e.g. Alaskan adventure in wilderness, animals, and flowers."
                isRequired
                validationState={validationState?.propertyName}
                onChange={setImageDescription}
                contextualHelp={(
                  <ContextualHelp>
                    <Heading>Need help?</Heading>
                    <Content>
                      <Text>
                        The
                        <strong>description of an image</strong>
                        {' '}
                        you are looking for in the natural language, for e.g. &quot;Family vacation on the beach with blue ocean, dolphins, boats and drink&quot;
                      </Text>
                    </Content>
                  </ContextualHelp>
                  )}
              />

              <Text>
                <p />
                Please note this will use credits from Generative AI services such as OpenAI/DALL.E 2. The AI-generated images are saved to this AEM as a Cloud Service Author service using logged user access (IMS) token.
              </Text>

              <ButtonGroup align="end">
                <Button variant="accent" onPress={onSubmitHandler}>Use Credits</Button>
                <Button variant="accent" onPress={() => guestConnection.host.modal.close()}>Close</Button>
              </ButtonGroup>
            </Form>
          </Flex>

        </Content>
      </Provider>

    );
  }

  function buildAssetDetailsURL(aemImgURL) {
    const urlParts = aemImgURL.split('.com');
    const aemAssetdetailsURL = `${urlParts[0]}.com/ui#/aem/assetdetails.html${urlParts[1]}`;

    return aemAssetdetailsURL;
  }

  /**
   * Displays the action response received from the App Builder
   *
   * @returns Displays App Builder action and details
   */
  function renderActionResponse() {
    return (
      <Provider theme={defaultTheme} colorScheme="light">
        <Content width="100%">

          {actionResponse.status === 'success'
            && (
              <>
                <Heading level="4">
                  Successfully generated an image, uploaded it to this AEM-CS Author service, and associated it to the selected Content Fragment.
                </Heading>

                <Text>
                  {' '}
                  Please see generated image in AEM-CS
                  {' '}
                  <Link>
                    <a href={buildAssetDetailsURL(actionResponse.aemImgURL)} target="_blank" rel="noreferrer">
                      here.
                    </a>
                  </Link>
                </Text>
              </>
            )}

          {actionResponse.status === 'failure'
            && (
            <Heading level="4">
              Failed to generate, upload image, please check App Builder logs.
            </Heading>
            )}

          <Flex width="100%" justifyContent="end" alignItems="center" marginTop="size-400">
            <ButtonGroup align="end">
              <Button variant="negative" onPress={() => guestConnection.host.modal.close()}>Close</Button>
            </ButtonGroup>
          </Flex>

        </Content>
      </Provider>
    );
  }

  /**
   * Handle the Generate Image form submission.
   * This function calls the supporting Adobe I/O Runtime actions such as
   * - Call the Generative AI service (DALL.E) with 'image description' to generate an image
   * - Download the AI generated image to App Builder runtime
   * - Save the downloaded image to AEM DAM and update Content Fragment's image reference property to use this new image
   *
   * When invoking the Adobe I/O Runtime actions, the following parameters are passed as they're used by the action to connect to AEM:
   * - AEM Host to connect to
   * - AEM access token to connect to AEM with
   * - The Content Fragment path to update
   *
   * @returns In case of success the updated content fragment, otherwise failure message
   */
  async function onSubmitHandler() {
    console.log('Started Image Generation orchestration');

    // Validate the form input fields
    if (imageDescription?.length > 1) {
      setValidationState({ imageDescription: 'valid' });
    } else {
      setValidationState({ imageDescription: 'invalid' });
      return;
    }

    // Mark the extension as invoking the action, so the loading spinner is displayed
    setActionInvokeInProgress(true);

    // Set the HTTP headers to access the Adobe I/O runtime action
    const headers = {
      Authorization: `Bearer ${guestConnection.sharedContext.get('auth').imsToken}`,
      'x-gw-ims-org-id': guestConnection.sharedContext.get('auth').imsOrg,
    };

    // Set the parameters to pass to the Adobe I/O Runtime action
    const params = {

      aemHost: `https://${guestConnection.sharedContext.get('aemHost')}`,

      fragmentId: fragmentIds[0],
      imageDescription,
    };

    const generateImageAction = 'generate-image';

    try {
      const generateImageActionResponse = await actionWebInvoke(allActions[generateImageAction], headers, params);

      // Set the response from the Adobe I/O Runtime action
      setActionResponse(generateImageActionResponse);

      console.log(`Response from ${generateImageAction}:`, actionResponse);
    } catch (e) {
      // Log and store any errors
      console.error(e);
    }

    // Set the action as no longer being invoked, so the loading spinner is hidden
    setActionInvokeInProgress(false);
  }
}
```

>[!NOTE]
>
>In `buildAssetDetailsURL()` funzione, il `aemAssetdetailsURL` valore variabile presuppone che il valore [Unified Shell](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/aem-cloud-service-on-unified-shell.html#overview) è abilitato. Se hai disabilitato Unified Shell, devi rimuovere `/ui#/aem` dal valore della variabile.


### Azione Adobe I/O Runtime

Un’app di App Builder per l’estensione dell’AEM può definire o utilizzare 0 o più azioni Adobe I/O Runtime.
L’azione runtime di Adobe è responsabile del lavoro che richiede l’interazione con AEM, Adobe o servizi web di terze parti.

In questa app di esempio, il `generate-image` L&#39;azione Adobe I/O Runtime è responsabile di:

1. Generazione di un’immagine tramite [Generazione di immagini API OpenAI](https://beta.openai.com/docs/guides/images/image-generation-beta) servizio
1. Caricamento dell’immagine generata nell’istanza AEM-CS tramite [Caricamento AEM](https://github.com/adobe/aem-upload) libreria
1. Effettuare una richiesta HTTP all’API Frammento di contenuto dell’AEM per aggiornare la proprietà immagine del Frammento di contenuto.
1. Restituzione delle informazioni chiave di operazioni riuscite e non riuscite per la visualizzazione da parte del modale (`GenerateImageModal.js`)


#### Punto di ingresso (`index.js`)

Il `index.js` orchestrare le attività superiori a 1-3 utilizzando i rispettivi moduli JavaScript, ovvero `generate-image-using-openai, upload-generated-image-to-aem, update-content-fragement`. Questi moduli e il codice associato sono descritti nella sezione successiva [sottosezioni](#image-generation-module---generate-image-using-openaijs).

+ `src/aem-cf-console-admin-1/actions/generate-image/index.js`

```javascript
/**
 *
 * This action orchestrates an image generation by calling the OpenAI API (DALL.E 2) and saves generated image to AEM.
 *
 * It leverages following modules
 *  - 'generate-image-using-openai' - To generate an image using OpenAI API
 *  - 'upload-generated-image-to-aem' - To upload the generated image into AEM-CS instance
 *  - 'update-content-fragement' - To update the CF image property with generated image's DAM path
 *
 */

const { Core } = require('@adobe/aio-sdk');
const {
  errorResponse, stringParameters, getBearerToken, checkMissingRequestInputs,
} = require('../utils');

const { generateImageUsingOpenAI } = require('./generate-image-using-openai');

const { uploadGeneratedImageToAEM } = require('./upload-generated-image-to-aem');

const { updateContentFragmentToUseGeneratedImg } = require('./update-content-fragement');

// main function that will be executed by Adobe I/O Runtime
async function main(params) {
  // create a Logger
  const logger = Core.Logger('main', { level: params.LOG_LEVEL || 'info' });

  try {
    // 'info' is the default level if not set
    logger.info('Calling the main action');

    // log parameters, only if params.LOG_LEVEL === 'debug'
    logger.debug(stringParameters(params));

    // check for missing request input parameters and headers
    const requiredParams = ['aemHost', 'fragmentId', 'imageDescription'];
    const requiredHeaders = ['Authorization'];
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders);

    if (errorMessage) {
      // return and log client errors
      return errorResponse(400, errorMessage, logger);
    }

    // extract the user Bearer token from the Authorization header
    const token = getBearerToken(params);

    // Call OpenAI (DALL.E 2) API to generate an image using image description
    const generatedImageURL = await generateImageUsingOpenAI(params);
    logger.info(`Generated image using OpenAI API and url is : ${generatedImageURL}`);

    // Upload the generated image to AEM-CS
    const uploadedImagePath = await uploadGeneratedImageToAEM(params, generatedImageURL, token);
    logger.info(`Uploaded image to AEM, path is: ${uploadedImagePath}`);

    // Update Content Fragment with the newly generated image reference
    const updateContentFragmentPath = await updateContentFragmentToUseGeneratedImg(params, uploadedImagePath, token);
    logger.info(`Updated Content Fragment path is: ${updateContentFragmentPath}`);

    let result;
    if (updateContentFragmentPath) {
      result = {
        status: 'success', message: 'Successfully generated and uploaded image to AEM', genTechServiceImageURL: generatedImageURL, aemImgURL: uploadedImagePath, fragmentPath: updateContentFragmentPath,
      };
    } else {
      result = { status: 'failure', message: 'Failed to generated and uploaded image, please check App Builder logs' };
    }

    const response = {
      statusCode: 200,
      body: result,
    };

    logger.info('Adobe I/O Runtime action response', response);

    // Return the response to the caller
    return response;
  } catch (error) {
    // log any server errors
    logger.error(error);
    // return with 500
    return errorResponse(500, 'server error', logger);
  }
}

exports.main = main;
```

#### Generazione di immagini

Questo modulo è responsabile della chiamata di OpenAI [Generazione immagini](https://beta.openai.com/docs/guides/images/image-generation-beta) endpoint che utilizza [openai](https://github.com/openai/openai-node) libreria. Per ottenere la chiave segreta API OpenAI definita nella `.env` file, utilizza `params.OPENAI_API_KEY`.

+ `src/aem-cf-console-admin-1/actions/generate-image/generate-image-using-openai.js`

```javascript
/**
 * This module calls OpenAI API to generate an image based on image description provided to Action
 *
 */

const { Configuration, OpenAIApi } = require('openai');

const { Core } = require('@adobe/aio-sdk');

// Placeholder than actual OpenAI Image
const PLACEHOLDER_IMG_URL = 'https://www.gstatic.com/webp/gallery/2.png';

async function generateImageUsingOpenAI(params) {
  // create a Logger
  const logger = Core.Logger('generateImageUsingOpenAI', { level: params.LOG_LEVEL || 'info' });

  let generatedImageURL = PLACEHOLDER_IMG_URL;

  // create configuration object with the API Key
  const configuration = new Configuration({
    apiKey: params.OPENAI_API_KEY,
  });

  // create OpenAIApi object
  const openai = new OpenAIApi(configuration);

  logger.info(`Generating image for input: ${params.imageDescription}`);

  try {
    // invoke createImage method with details
    const response = await openai.createImage({
      prompt: params.imageDescription,
      n: 1,
      size: '1024x1024',
    });

    generatedImageURL = response.data.data[0].url;

    logger.info(`The OpenAI generate image url is: ${generatedImageURL}`);
  } catch (error) {
    logger.error(`Error while generating image, details are: ${error}`);
  }

  return generatedImageURL;
}

module.exports = {
  generateImageUsingOpenAI,
};
```

#### Carica in AEM

Questo modulo è responsabile del caricamento in AEM dell’immagine generata da OpenAI tramite [Caricamento AEM](https://github.com/adobe/aem-upload) libreria. L’immagine generata viene scaricata per la prima volta in fase di esecuzione per App Builder utilizzando Node.js [File system](https://nodejs.org/api/fs.html) e una volta completato il caricamento su AEM, viene eliminata.

Nel codice sottostante `uploadGeneratedImageToAEM` La funzione orchestra il download delle immagini generate in fase di runtime, lo carica su AEM e lo elimina da runtime. L&#39;immagine viene caricata in `/content/dam/wknd-shared/en/generated` percorso, accertati che in DAM siano presenti tutte le cartelle e che ne siano presenti i prerequisiti da utilizzare [Caricamento AEM](https://github.com/adobe/aem-upload) libreria.

+ `src/aem-cf-console-admin-1/actions/generate-image/upload-generated-image-to-aem.js`

```javascript
/**
 * This module uploads the generated image to AEM-CS instance using current user's IMS token
 *
 */

const { Core } = require('@adobe/aio-sdk');
const fs = require('fs');

const {
  DirectBinaryUploadErrorCodes,
  DirectBinaryUpload,
  DirectBinaryUploadOptions,
} = require('@adobe/aem-upload');

const codes = DirectBinaryUploadErrorCodes;
const IMG_EXTENSION = '.png';

const GENERATED_IMAGES_DAM_PATH = '/content/dam/wknd-shared/en/generated';

async function downloadImageToRuntime(logger, generatedImageURL) {
  logger.log('Downloading generated image to the runtime');

  // placeholder image name
  let generatedImageName = 'generated.png';

  try {
    // Get the generated image name from the image URL
    const justImgURL = generatedImageURL.substring(0, generatedImageURL.indexOf(IMG_EXTENSION) + 4);
    generatedImageName = justImgURL.substring(justImgURL.lastIndexOf('/') + 1);

    // Read image from URL as the buffer
    const response = await fetch(generatedImageURL);
    const buffer = await response.buffer();

    // Write/download image to the runtime
    fs.writeFileSync(generatedImageName, buffer, (err) => {
      if (err) throw err;
      logger.log('Saved the generated image!');
    });
  } catch (error) {
    logger.error(`Error while downloading image on the runtime, details are: ${error}`);
  }

  return generatedImageName;
}

function setupEventHandlers(binaryUpload, logger) {
  binaryUpload.on('filestart', (data) => {
    const { fileName } = data;

    logger.log(`Started file upload ${fileName}`);
  });

  binaryUpload.on('fileprogress', (data) => {
    const { fileName, transferred } = data;

    logger.log(`Fileupload is in progress ${fileName} & ${transferred}`);
  });

  binaryUpload.on('fileend', (data) => {
    const { fileName } = data;

    logger.log(`Finished file upload ${fileName}`);
  });

  binaryUpload.on('fileerror', (data) => {
    const { fileName, errors } = data;

    logger.log(`Error in file upload ${fileName} and ${errors}`);
  });
}

async function getImageSize(downloadedImgName) {
  const stats = fs.statSync(downloadedImgName);
  return stats.size;
}

async function uploadImageToAEMFromRuntime(logger, aemURL, downloadedImgName, accessToken) {
  let aemImageURL;
  try {
    logger.log('Uploading generated image to AEM from the runtime');

    const binaryUpload = new DirectBinaryUpload();

    // setup event handlers to track the progress, success or error
    setupEventHandlers(binaryUpload, logger);

    // get downloaded image size
    const imageSize = await getImageSize(downloadedImgName);
    logger.info(`The image upload size is: ${imageSize}`);

    // The deatils of the file to be uploaded
    const uploadFiles = [
      {
        fileName: downloadedImgName, // name of the file as it will appear in AEM
        fileSize: imageSize, // total size, in bytes, of the file
        filePath: downloadedImgName, // Full path to the local file
      },
    ];

    // Provide AEM URL and DAM Path where images will be uploaded
    const options = new DirectBinaryUploadOptions()
      .withUrl(`${aemURL}${GENERATED_IMAGES_DAM_PATH}`)
      .withUploadFiles(uploadFiles);

    // Add headers like content type and authorization
    options.withHeaders({
      'content-type': 'image/png',
      Authorization: `Bearer ${accessToken}`,
    });

    // Start the upload to AEM
    await binaryUpload.uploadFiles(options)
      .then((result) => {
        // Handle Error
        result.getErrors().forEach((error) => {
          if (error.getCode() === codes.ALREADY_EXISTS) {
            logger.error('The generated image already exists');
          }
        });

        // Handle Upload result and check for errors
        result.getFileUploadResults().forEach((fileResult) => {
          // log file upload result
          logger.info(`File upload result ${JSON.stringify(fileResult)}`);

          fileResult.getErrors().forEach((fileErr) => {
            if (fileErr.getCode() === codes.ALREADY_EXISTS) {
              const fileName = fileResult.getFileName();
              logger.error(`The generated image already exists ${fileName}`);
            }
          });
        });
      })
      .catch((err) => {
        logger.info(`Failed to uploaded generated image to AEM${err}`);
      });

    logger.info('Successfully uploaded generated image to AEM');

    aemImageURL = `${aemURL + GENERATED_IMAGES_DAM_PATH}/${downloadedImgName}`;
  } catch (error) {
    logger.info(`Error while uploading generated image to AEM, see ${error}`);
  }

  return aemImageURL;
}

async function deleteFileFromRuntime(logger, downloadedImgName) {
  try {
    logger.log('Deleting the generated image from the runtime');

    fs.unlinkSync(downloadedImgName);

    logger.log('Successfully deleted the generated image from the runtime');
  } catch (error) {
    logger.error(`Error while deleting generated image from the runtime, details are: ${error}`);
  }
}

async function uploadGeneratedImageToAEM(params, generatedImageURL, accessToken) {
  // create a Logger
  const logger = Core.Logger('uploadGeneratedImageToAEM', { level: params.LOG_LEVEL || 'info' });

  const aemURL = params.aemHost;

  logger.info(`Uploading generated image from ${generatedImageURL} to AEM ${aemURL} by streaming the bytes.`);

  // download image to the App Builder runtime
  const downloadedImgName = await downloadImageToRuntime(logger, generatedImageURL);

  // Upload image to AEM from the App Builder runtime
  const aemImageURL = await uploadImageToAEMFromRuntime(logger, aemURL, downloadedImgName, accessToken);

  // Delete the downloaded image from the App Builder runtime
  await deleteFileFromRuntime(logger, downloadedImgName);

  return aemImageURL;
}

module.exports = {
  uploadGeneratedImageToAEM,
};
```

#### Aggiorna frammento di contenuto

Questo modulo è responsabile dell’aggiornamento della proprietà Immagine del frammento di contenuto specificato con il percorso DAM dell’immagine appena caricata, tramite l’API Frammento di contenuto dell’AEM.

+ `src/aem-cf-console-admin-1/actions/generate-image/update-content-fragement.js`

```javascript
/**
 * This module updates the CF image property with generated image's DAM path
 *
 */
const { Core } = require('@adobe/aio-sdk');

const ADVENTURE_MODEL_IMG_PROPERTY_NAME = 'primaryImage';

const ARTICLE_MODEL_IMG_PROPERTY_NAME = 'featuredImage';

const AUTHOR_MODEL_IMG_PROPERTY_NAME = 'profilePicture';

function findImgPropertyName(fragmenPath) {
  if (fragmenPath && fragmenPath.includes('/adventures')) {
    return ADVENTURE_MODEL_IMG_PROPERTY_NAME;
  } if (fragmenPath && fragmenPath.includes('/magazine')) {
    return ARTICLE_MODEL_IMG_PROPERTY_NAME;
  }
  return AUTHOR_MODEL_IMG_PROPERTY_NAME;
}

async function updateContentFragmentToUseGeneratedImg(params, uploadedImagePath, accessToken) {
  // create a Logger
  const logger = Core.Logger('updateContentFragment', { level: params.LOG_LEVEL || 'info' });

  const fragmenPath = params.fragmentId;
  const imgPropName = findImgPropertyName(fragmenPath);
  const relativeImgPath = uploadedImagePath.substring(uploadedImagePath.indexOf('/content/dam'));

  logger.info(`Update CF ${fragmenPath} to use ${relativeImgPath} image path`);

  const body = {
    properties: {
      elements: {
        [imgPropName]: {
          value: relativeImgPath,
        },
      },
    },
  };

  const res = await fetch(`${params.aemHost}${fragmenPath.replace('/content/dam/', '/api/assets/')}.json`, {
    method: 'put',
    body: JSON.stringify(body),
    headers: {
      Authorization: `Bearer ${accessToken}`,
      'Content-Type': 'application/json',
    },

  });

  if (res.ok) {
    logger.info(`Successfully updated ${fragmenPath}`);
    return fragmenPath;
  }

  logger.info(`Failed to update ${fragmenPath}`);
  return '';
}

module.exports = {
  updateContentFragmentToUseGeneratedImg,
};
```