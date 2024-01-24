---
title: Aggiunta corretta di collegamenti simbolici a GIT
description: Istruzioni su come e dove aggiungere symlink quando si lavora sulle configurazioni del Dispatcher.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 6e751586-e92e-482d-83ce-6fcae4c1102c
duration: 364
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1231'
ht-degree: 0%

---

# Aggiunta di collegamenti simbolici a GIT

[Sommario](./overview.md)

[&lt;- Precedente: Verifica stato del Dispatcher](./health-check.md)

In AMS otterrai un archivio GIT precompilato contenente il codice sorgente del dispatcher, pronto per iniziare lo sviluppo e la personalizzazione.

Dopo aver creato il primo `.vhost` file o livello principale `farm.any` file sarà necessario creare un collegamento simbolico dal `available_*` alla directory `enabled_*` directory. L’utilizzo del tipo di collegamento corretto sarà fondamentale per una distribuzione corretta tramite la pipeline di Cloud Manager. Questa pagina ti aiuterà a capire come farlo.

## Archetipo di Dispatcher

Lo sviluppatore dell’AEM avvia il suo progetto solitamente dall’ambiente [Archetipo AEM](https://github.com/adobe/aem-project-archetype)

Ecco un esempio dell’area del codice sorgente in cui puoi vedere i symlink utilizzati:

```
$ tree dispatcher
dispatcher
└── src
   ├── conf.d
.....SNIP.....
    │   └── available_vhosts
    │   │   ├── 000_unhealthy_author.vhost
    │   │   ├── 000_unhealthy_publish.vhost
    │   │   ├── aem_author.vhost
    │   │   ├── aem_flush.vhost
    │   │   ├── aem_health.vhost
    │   │   ├── aem_lc.vhost
    │   │   └── aem_publish.vhost
    └── dispatcher_vhost.conf
    │   └── enabled_vhosts
    │   │   ├── aem_author.vhost -> ../available_vhosts/aem_author.vhost
    │   │   ├── aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
    │   │   ├── aem_health.vhost -> ../available_vhosts/aem_health.vhost
    │   │   └── aem_publish.vhost -> ../available_vhosts/aem_publish.vhost
.....SNIP.....
    └── conf.dispatcher.d
    │   ├── available_farms
    │   │   ├── 000_ams_author_farm.any
    │   │   ├── 001_ams_lc_farm.any
    │   │   └── 002_ams_publish_farm.any
.....SNIP.....
    │   └── enabled_farms
    │   │   ├── 000_ams_author_farm.any -> ../available_farms/000_ams_author_farm.any
    │   │   └── 002_ams_publish_farm.any -> ../available_farms/002_ams_publish_farm.any
.....SNIP.....
17 directories, 60 files
```

A titolo di esempio: `/etc/httpd/conf.d/available_vhosts/` la directory contiene il potenziale `.vhost` i file che possiamo utilizzare nella nostra configurazione in esecuzione.

Il valore abilitato `.vhost` i file verranno visualizzati come percorso relativo `symlinks` all&#39;interno del `/etc/httpd/conf.d/enabled_vhosts/` directory.

## Creazione di un collegamento simbolico

Utilizziamo collegamenti simbolici al file in modo che il server web Apache tratti il file di destinazione come lo stesso file.  Non è necessario duplicare il file in entrambe le directory.  ma solo un collegamento da una directory (collegamento simbolico) all’altra.

Riconosci che le configurazioni implementate verranno indirizzate a un host Linux.  La creazione di un collegamento simbolico non compatibile con il sistema di destinazione causerà errori e risultati indesiderati.

Se la tua workstation non è un computer Linux, probabilmente ti chiederai quali comandi utilizzare per creare correttamente questi collegamenti in modo che possano eseguirne il commit in GIT.

> `TIP:` È importante utilizzare i collegamenti relativi perché se hai installato una copia locale di Apache Webserver e disponessi di una base di installazione diversa, i collegamenti continuerebbero a funzionare.  Se utilizzi un percorso assoluto, la workstation o altri sistemi dovrebbero corrispondere esattamente alla stessa struttura di directory.

### OSX/Linux

I collegamenti simbolici sono nativi di questi sistemi operativi e di seguito sono riportati alcuni esempi di come crearli.  Apri l’applicazione terminale preferita e utilizza i seguenti comandi di esempio per creare il collegamento:

```
$ cd <LOCATION OF CLONED GIT REPO>\src\conf.d\enabled_vhosts
$ ln -s ../available_vhosts/<Destination File Name> <Target File Name>
```

Di seguito è riportato un esempio di comando popolato come riferimento:

```
$ git clone https://github.com/adobe/aem-project-archetype.git
$ cd aem-project-archetype/src/main/archetype/dispatcher.ams/src/conf.d/enabled_vhosts/
$ ln -s ../available_vhosts/aem_flush.vhost aem_flush.vhost
```

Di seguito è riportato un esempio del collegamento, se elenchi il file utilizzando `ls` comando:

```
ls -l
total 0
lrwxrwxrwx. 1 root root 35 Oct 13 21:38 aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
```

### Windows

> `Note:` Si scopre che MS Windows (meglio, NTFS) supporta collegamenti simbolici da... Windows Vista

![Immagine del prompt dei comandi di Windows che mostra l&#39;output della Guida del comando mklink](./assets/git-symlinks/windows-terminal-mklink.png)

> `Warning:` il comando mklink per creare symlink richiede privilegi di amministratore per essere eseguito correttamente. Anche come account amministratore, è necessario eseguire il prompt dei comandi &quot;Come amministratore&quot; a meno che non sia stata abilitata la modalità sviluppatore
> <br/>Autorizzazioni non appropriate:
> ![Immagine del prompt dei comandi di Windows che mostra il comando non riuscito a causa di autorizzazioni](./assets/git-symlinks/windows-mklink-underpriv.png)
> <br/>Autorizzazioni appropriate:
> ![Immagine del prompt dei comandi di Windows eseguito come amministratore](./assets/git-symlinks/windows-mklink-properpriv.png)

Di seguito sono riportati i comandi per creare il collegamento:

```
C:\<PATH TO SRC>\enabled_vhosts> mklink <Target File Name> ..\available_vhosts\<Destination File Name>
```


Di seguito è riportato un esempio di comando popolato come riferimento:

```
C:\> git clone https://github.com/adobe/aem-project-archetype.git
C:\> cd aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts\
C:\aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts> mklink aem_flush.vhost ..\available_vhost\aem_flush.vhost
symbolic link created for aem_flush.vhost <<===>> ..\available_vhosts\aem_flush.vhost
```

#### Modalità sviluppatore ( Windows 10 )

Quando viene inserito in [Modalità sviluppatore](https://docs.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development), Windows 10 consente di testare più facilmente le app che stai sviluppando, utilizzare l’ambiente Ubuntu Bash Shell, modificare una serie di impostazioni incentrate sugli sviluppatori e svolgere altre attività di questo tipo.

Microsoft sembra continuare ad aggiungere funzionalità alla modalità Sviluppatore, o abilitare alcune di queste funzionalità per impostazione predefinita una volta che raggiungono un’adozione più diffusa e sono considerate stabili (ad esempio con Creators Update, l’ambiente Ubuntu Bash Shell non ha più bisogno della modalità Sviluppatore).

E i symlink? Con Modalità sviluppatore ABILITATA, non è necessario eseguire un prompt dei comandi con privilegi elevati per creare symlink. Pertanto, una volta abilitata la modalità Sviluppatore, tutti gli utenti possono creare symlink.

> Dopo aver abilitato la modalità Sviluppatore, gli utenti devono disconnettersi/accedere per rendere effettive le modifiche.

Ora è possibile vedere senza eseguire come amministratore il comando funziona

![Immagine del prompt dei comandi di Windows eseguito come utente normale con la modalità sviluppatore abilitata](./assets/git-symlinks/windows-mklink-devmode.png)

#### Approccio alternativo/programmatico

Esiste una policy specifica che può consentire a determinati utenti di creare collegamenti simbolici → [Creare collegamenti simbolici (Windows 10) - Protezione di Windows | Documentazione di Microsoft](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/create-symbolic-links)

PRO:
- I clienti possono sfruttarlo per consentire a livello di programmazione la creazione di collegamenti simbolici a tutti gli sviluppatori all’interno della propria organizzazione (ad esempio, Active Directory) senza dover abilitare manualmente la modalità Sviluppatore su ciascun dispositivo.
- Inoltre, questo criterio dovrebbe essere disponibile nelle versioni precedenti di MS Windows che non offrono la modalità Sviluppatore.

CON:
- Questo criterio non sembra avere alcun effetto sugli utenti appartenenti al gruppo Administrators. Gli amministratori devono comunque eseguire il prompt dei comandi con privilegi elevati. Strano.

> Per rendere effettive le modifiche ai criteri locali/di gruppo sarà necessario disconnettersi/accedere dall&#39;utente.

Esegui `gpedit.msc`, aggiungi/cambia gli utenti come richiesto. Per impostazione predefinita, gli amministratori sono presenti

![Finestra Editor Criteri di gruppo che mostra le autorizzazioni per la modifica](./assets/git-symlinks/windows-localpolicies-symlinks.png)

#### Abilita collegamenti simbolici in GIT

Git gestisce i symlink in base all’opzione core.symlinks

Origine: [Git - Documentazione di git-config](https://git-scm.com/docs/git-config#Documentation/git-config.txt-coresymlinks)

*Se core.symlinks è false, i collegamenti simbolici vengono estratti come piccoli file semplici che contengono il testo del collegamento. `git-update-index[1]` e `git-add[1]` non modifica il tipo registrato in file normale. Utile su file system come FAT che non supportano collegamenti simbolici.
Il valore predefinito è true, ad eccezione `git-clone[1]` o `git-init[1] will probe and set core.symlinks false if appropriate when the repository is created.` Nella maggior parte dei casi, Git presuppone che Windows non sia adatto per i collegamenti simbolici e lo imposta su false.*

Il comportamento di Git su Windows è ben spiegato qui: Collegamenti simbolici · Git-for-windows/git Wiki · GitHub

> `Info`: i presupposti elencati nella documentazione qui sopra sembrano essere corretti con una possibile configurazione di AEM Developer su Windows, in particolare NTFS e il fatto che abbiamo solo file symlink vs. Directory symlink

Ecco la buona notizia, visto che [Git per Windows versione 2.10.2](https://github.com/git-for-windows/git/releases/tag/v2.10.2.windows.1) il programma di installazione dispone di [opzione esplicita per abilitare il supporto di collegamenti simbolici.](https://github.com/git-for-windows/git/issues/921)

> `Warning`: l’opzione core.symlink può essere fornita in fase di runtime durante la clonazione dell’archivio oppure può essere memorizzata come configurazione globale.

![Visualizzazione del programma di installazione GIT con supporto per symlink](./assets/git-symlinks/windows-git-install-symlink.png)

Git for Windows memorizzerà le preferenze globali in `"C:\Program Files\Git\etc\gitconfig"` . Queste impostazioni potrebbero non essere considerate da altre app client desktop Git.
Questo è il problema, non tutti gli sviluppatori utilizzeranno il client nativo Git (ad esempio Git Cmd, Git Bash) e alcune delle app desktop Git (ad esempio GitHub Desktop, Atlassian Source) potrebbero avere impostazioni/impostazioni predefinite diverse per utilizzare System o un Git incorporato

Ecco un esempio di cosa c&#39;è dentro `gitconfig` file

```
[diff "astextplain"]
    textconv = astextplain
[filter "lfs"]
    clean = git-lfs clean -- %f
    smudge = git-lfs smudge -- %f
    process = git-lfs filter-process
    required = true
[http]
    sslBackend = openssl
    sslCAInfo = C:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
[core]
    autocrlf = true
    fscache = true
    symlinks = true
[pull]
    rebase = false
[credential]
    helper = manager-core
[credential "https://dev.azure.com"]
    useHttpPath = true
[init]
    defaultBranch = master
```

#### Suggerimenti della riga di comando Git

In alcuni casi potrebbe essere necessario creare nuovi collegamenti simbolici, ad esempio aggiungendo un nuovo host o una nuova farm.

Nella documentazione di cui sopra è stato illustrato che Windows offre un comando &quot;mklink&quot; per la creazione di collegamenti simbolici.

Se si lavora in un ambiente Git Bash, è possibile utilizzare il comando Bash standard `ln -s` ma dovrà essere preceduto da un&#39;istruzione speciale come l&#39;esempio qui:

```
MSYS=winsymlinks:nativestrict ln -s test_vhost_symlink ../dispatcher/src/conf.d/available_vhosts/default.vhost
```

#### Riepilogo

Per poter gestire correttamente i symlink Git (almeno per l’ambito della linea di base di configurazione del dispatcher AEM corrente) su un sistema operativo Microsoft Windows, è necessario:

| Elemento | Versione/Configurazione minima | Versione/configurazione consigliata |
|------|---------------------------------|-------------------------------------|
| Sistema operativo | Windows Vista o versione successiva | Windows 10 Creator Update o versione successiva |
| File system | NTFS | NTFS |
| Possibilità di gestire i symlink per l&#39;utente Windows | `"Create symbolic links"` Criteri di gruppo/locali `under "Group Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment"` | Modalità sviluppatore Windows 10 abilitata |
| GIT | Client nativo versione 1.5.3 | Client nativo versione 2.10.2 o successiva |
| Configurazione Git | `--core.symlinks=true` quando si esegue un clone Git dalla riga di comando | Configurazione globale Git<br/>`[core]`<br/>    symlinks = true <br/> Percorso configurazione client Git nativo: `C:\Program Files\Git\etc\gitconfig` <br/>Percorso standard per client Git Desktop: `%HOMEPATH%\.gitconfig` |

> `Note:` Se disponi già di un archivio locale, dovrai clonare nuovamente dall’origine. Puoi clonare in una nuova posizione e unire manualmente le modifiche locali non salvate/non gestite nell’archivio appena clonato.
