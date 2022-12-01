---
title: Aggiunta corretta di collegamenti simbolici nel GIT
description: Istruzioni su come e dove aggiungere collegamenti simbolici quando lavori sulle configurazioni del Dispatcher.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: df3afc60f765c18915eca3bb2d3556379383fafc
workflow-type: tm+mt
source-wordcount: '1252'
ht-degree: 0%

---


# Aggiunta di collegamenti simbolici a GIT

[Sommario](./overview.md)

[&lt;- Precedente: Verifica dello stato del dispatcher](./health-check.md)

In AMS otterrai un archivio GIT prepopolato contenente il codice sorgente del dispatcher maturo pronto per iniziare lo sviluppo e la personalizzazione.

Dopo aver creato il tuo primo `.vhost` file o livello superiore `farm.any` file necessario per creare un collegamento simbolico dal `available_*` nella directory `enabled_*` directory. L’utilizzo del tipo di collegamento appropriato sarà fondamentale per una corretta implementazione tramite la pipeline di Cloud Manager. Questa pagina ti aiuterà a sapere come farlo.

## Archetipo di Dispatcher

Lo sviluppatore AEM avvia il progetto tipicamente dal [archetipo AEM](https://github.com/adobe/aem-project-archetype)

Di seguito è riportato un esempio dell’area del codice sorgente in cui puoi vedere i collegamenti simbolici utilizzati:

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

Ad esempio, `/etc/httpd/conf.d/available_vhosts/` la directory contiene il potenziale di staging `.vhost` file che possiamo utilizzare nella nostra configurazione in esecuzione.

Abilitato `.vhost` i file verranno visualizzati come percorso relativo `symlinks` all&#39;interno del `/etc/httpd/conf.d/enabled_vhosts/` directory.

## Creazione di un collegamento simbolico

Utilizziamo i collegamenti simbolici al file in modo che Apache Webserver tratti il file di destinazione come lo stesso file.  Non vogliamo duplicare il file in entrambe le directory.  Invece solo una scorciatoia da una directory (collegamento simbolico) all&#39;altra.

Riconosce che le configurazioni distribuite verranno indirizzate a un host Linux.  La creazione di un symlink non compatibile con il sistema di destinazione causerà errori e risultati indesiderati.

Se la tua workstation non è una macchina Linux, probabilmente ti chiederai quali comandi utilizzare per creare correttamente questi collegamenti in modo che possano inserirli in GIT.

> `TIP:` È importante utilizzare i collegamenti relativi perché se si installasse una copia locale di Apache Webserver e si disponesse di una base di installazione diversa, i collegamenti continuerebbero a funzionare.  Se si utilizza un percorso assoluto, la workstation o altri sistemi devono corrispondere esattamente alla stessa struttura di directory.

### OSX / Linux

I collegamenti simbolici sono nativi di questi sistemi operativi e qui ci sono alcuni esempi di come creare questi collegamenti.  Apri l&#39;applicazione terminale preferita e utilizza i seguenti comandi di esempio per creare il collegamento:

```
$ cd <LOCATION OF CLONED GIT REPO>\src\conf.d\enabled_vhosts
$ ln -s ../available_vhosts/<Destination File Name> <Target File Name>
```

Di seguito è riportato un esempio di comando popolato per riferimento:

```
$ git clone https://github.com/adobe/aem-project-archetype.git
$ cd aem-project-archetype/src/main/archetype/dispatcher.ams/src/conf.d/enabled_vhosts/
$ ln -s ../available_vhosts/aem_flush.vhost aem_flush.vhost
```

Ecco un esempio del collegamento ora, se elencare il file utilizzando il `ls` comando:

```
ls -l
total 0
lrwxrwxrwx. 1 root root 35 Oct 13 21:38 aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
```

### Windows

> `Note:` Si scopre che MS Windows (meglio, NTFS) supporta i collegamenti simbolici da... Windows Vista

![Immagine del prompt dei comandi di Windows che mostra l&#39;output della guida del comando mklink](./assets/git-symlinks/windows-terminal-mklink.png)

> `Warning:` Il comando mklink per creare i collegamenti simbolici richiede i privilegi di amministratore per essere eseguito correttamente. Anche come account amministratore, è necessario eseguire il prompt dei comandi &quot;Come amministratore&quot; a meno che la modalità sviluppatore non sia abilitata
> <br/>Autorizzazioni improprie:
> ![Immagine del prompt dei comandi di Windows che mostra il comando non riuscito a causa di autorizzazioni](./assets/git-symlinks/windows-mklink-underpriv.png)
> <br/>Autorizzazioni appropriate:
> ![Immagine del prompt dei comandi di Windows eseguita come amministratore](./assets/git-symlinks/windows-mklink-properpriv.png)

Di seguito sono elencati i comandi per creare il collegamento:

```
C:\<PATH TO SRC>\enabled_vhosts> mklink <Target File Name> ..\available_vhosts\<Destination File Name>
```


Di seguito è riportato un esempio di comando popolato per riferimento:

```
C:\> git clone https://github.com/adobe/aem-project-archetype.git
C:\> cd aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts\
C:\aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts> mklink aem_flush.vhost ..\available_vhost\aem_flush.vhost
symbolic link created for aem_flush.vhost <<===>> ..\available_vhosts\aem_flush.vhost
```

#### Modalità sviluppatore ( Windows 10 )

Quando viene inserito in [Modalità Sviluppatore](https://docs.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development), Windows 10 consente di testare più facilmente le app che stai sviluppando, utilizzare l’ambiente shell Ubuntu Bash, modificare una serie di impostazioni incentrate sugli sviluppatori e fare altre cose del genere.

Microsoft sembra continuare ad aggiungere funzionalità alla modalità Sviluppatore, o abilitare alcune di queste funzionalità per impostazione predefinita una volta che raggiungono un&#39;adozione più diffusa e sono considerate stabili (ad esempio con Creators Update, Ubuntu Bash Shell l&#39;ambiente non ha più bisogno della modalità Sviluppatore).

E i collegamenti simbolici? Con la modalità Sviluppatore ENABLED, non è necessario eseguire un prompt dei comandi con privilegi elevati per poter creare collegamenti simbolici. Pertanto, una volta abilitata la modalità Sviluppatore , tutti gli utenti possono creare collegamenti simbolici.

> Dopo aver abilitato la modalità Sviluppatore, gli utenti devono disconnettersi/accedere per rendere effettive le modifiche.

Ora è possibile vedere senza eseguire come amministratore il comando funziona

![Immagine del prompt dei comandi di Windows eseguita come utente normale con la modalità sviluppatore abilitata](./assets/git-symlinks/windows-mklink-devmode.png)

#### Approccio alternativo/programmatico

C&#39;è una politica specifica che può consentire a determinati utenti di creare collegamenti simbolici → [Creare collegamenti simbolici (Windows 10) - Protezione di Windows | Documentazione Microsoft](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/create-symbolic-links)

PRO:
- Questo potrebbe essere sfruttato dai clienti per consentire programmaticamente la creazione di collegamenti simbolici a tutti gli sviluppatori all’interno della loro organizzazione (ad esempio, Active Directory) senza dover abilitare manualmente la modalità Sviluppatore su ciascun dispositivo.
- Inoltre, questo criterio deve essere disponibile nelle versioni precedenti di MS Windows che non offrono la modalità Sviluppatore.

CON:
- Questo criterio sembra non avere alcun effetto sugli utenti appartenenti al gruppo Administrators. Gli amministratori dovranno comunque eseguire il prompt dei comandi con privilegi elevati. Strano.

> Per rendere effettive le modifiche al criterio locale/di gruppo sarà necessario disconnettersi/accedere dall&#39;utente.

Esegui `gpedit.msc`, aggiungi/modifica gli utenti in base alle esigenze. Per impostazione predefinita, gli amministratori sono presenti

![Finestra Editor Criteri di gruppo che mostra l&#39;autorizzazione per regolare](./assets/git-symlinks/windows-localpolicies-symlinks.png)

#### Abilitare i collegamenti simbolici in GIT

Git gestisce i collegamenti simbolici in base all&#39;opzione core.symlinks

Origine: [Documentazione Git - git-config](https://git-scm.com/docs/git-config#Documentation/git-config.txt-coresymlinks)

*Se core.symlinks è false, i collegamenti simbolici vengono estratti come piccoli file semplici che contengono il testo di collegamento. `git-update-index[1]` e `git-add[1]` non cambierà il tipo registrato in file regolare. Utile su file system come FAT che non supportano i link simbolici.
Il valore predefinito è true, tranne `git-clone[1]` o `git-init[1] will probe and set core.symlinks false if appropriate when the repository is created.` Nella maggior parte dei casi, Git presuppone che Windows non sia adatto ai collegamenti simbolici e imposta questo valore su false.*

Il comportamento di Git su Windows è spiegato bene qui: Collegamenti simbolici ・ Git-for-windows/git Wiki ・ GitHub

> `Info`: Le ipotesi elencate nella documentazione sopra collegata sembrano essere OK con una possibile configurazione AEM Sviluppatore su Windows, in particolare NTFS e il fatto che abbiamo solo link simbolici di file rispetto a symlink di directory

Ecco la buona notizia, visto che [Git per Windows versione 2.10.2](https://github.com/git-for-windows/git/releases/tag/v2.10.2.windows.1) l&#39;installatore ha un [opzione esplicita per abilitare il supporto del collegamento simbolico.](https://github.com/git-for-windows/git/issues/921)

> `Warning`: L&#39;opzione core.symlink può essere fornita in fase di runtime durante la clonazione dell&#39;archivio, o altrimenti può essere memorizzata come configurazione globale.

![Visualizzazione del programma di installazione GIT che mostra il supporto per i collegamenti simbolici](./assets/git-symlinks/windows-git-install-symlink.png)

Git for Windows memorizzerà le preferenze globali in `"C:\Program Files\Git\etc\gitconfig"` . Queste impostazioni potrebbero non essere prese in considerazione da altre app client desktop Git.
Questo è il problema, non tutti gli sviluppatori utilizzeranno il client nativo Git (ad es. Git Cmd, Git Bash) e alcune delle app desktop Git (ad es. GitHub Desktop, Atlassian Sourcetree) possono avere impostazioni/impostazioni diverse per utilizzare il sistema o un Git integrato

Ecco un esempio di cosa c&#39;è all&#39;interno del `gitconfig` file

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

#### Suggerimenti Della Riga Di Comando Git

In alcuni scenari potrebbe essere necessario creare nuovi collegamenti simbolici (ad esempio, aggiungere un nuovo host o una nuova farm).

Nella documentazione di cui sopra abbiamo visto che Windows offre un comando &quot;mklink&quot; per creare collegamenti simbolici.

Se lavori in un ambiente Git Bash, puoi utilizzare il comando Bash standard `ln -s` ma dovrà essere preceduto da un&#39;istruzione speciale come l&#39;esempio qui:

```
MSYS=winsymlinks:nativestrict ln -s test_vhost_symlink ../dispatcher/src/conf.d/available_vhosts/default.vhost
```

#### Riepilogo

Per far sì che i collegamenti simbolici di gestione Git siano correttamente (almeno per l’ambito della linea di base di configurazione del dispatcher AEM corrente) in un sistema operativo Microsoft Windows, sarà necessario:

| Elemento | Versione minima / Configurazione | Versione/configurazione consigliata |
|------|---------------------------------|-------------------------------------|
| Sistema operativo | Windows Vista o versione successiva | Aggiornamento di Windows 10 Creator o successivo |
| File system | NTFS | NTFS |
| Gestione dei collegamenti simbolici per l&#39;utente Windows | `"Create symbolic links"` Criteri di gruppo/locale `under "Group Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment"` | Modalità sviluppatore Windows 10 abilitata |
| GIT | Client nativo versione 1.5.3 | Client nativo versione 2.10.2 o successiva |
| Configurazione Git | `--core.symlinks=true` opzione quando si esegue un clone git dalla riga di comando | Configurazione globale Git<br/>`[core]`<br/>    symlinks = true <br/> Percorso di configurazione del client Git nativo: `C:\Program Files\Git\etc\gitconfig` <br/>Posizione standard per i client Git Desktop: `%HOMEPATH%\.gitconfig` |

> `Note:` Se disponi già di un archivio locale, dovrai clonarlo nuovamente dall’origine. Puoi clonare in una nuova posizione e unire manualmente le modifiche locali non impegnate/non impostate nell’archivio appena clonato.