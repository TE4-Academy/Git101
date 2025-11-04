# Git Installation och Konfiguration

Detta dokument beskriver hur du verifierar din Git-installation och konfigurerar VS Code som standard editor istället för Vim.

---

## Kontrollera Git-installation

### Verifiera att Git är installerat

Öppna CMD eller terminalen i VS Code och kör:

```cmd
git --version
```

**Förväntat resultat:**
```
git version 2.43.0 (eller senare)
```

Om Git inte är installerat, ladda ner från: https://git-scm.com/download/win

---

## Konfiguration av Git

### Kontrollera nuvarande konfiguration

```cmd
:: Visa all konfiguration
git config --list

:: Visa specifika inställningar
git config user.name
git config user.email
git config core.editor
```

### Sätt användarnamn och email

**Dessa används i alla dina commits:**

```cmd
:: Globala inställningar (gäller alla projekt)
git config --global user.name "Ditt Namn"
git config --global user.email "din.email@example.com"

:: Verifiera
git config user.name
git config user.email
```

---

## Byta från Vim till VS Code

Git använder som standard Vim för commit-meddelanden och interaktiva kommandon (som `git rebase -i`). Vim kan vara svårt att använda. Byt till VS Code istället.

### Sätt VS Code som standard editor

```cmd
git config --global core.editor "code --wait"
```

**Förklaring:**
- `code` = VS Code-kommandot
- `--wait` = Git väntar tills du stänger filen i VS Code

### Verifiera inställningen

```cmd
git config core.editor
```

**Förväntat resultat:**
```
code --wait
```

### Testa att det fungerar

```cmd
:: Skapa ett test-repo
cd %USERPROFILE%\DEV
mkdir git-test
cd git-test
git init

:: Skapa en fil
echo test > test.txt
git add test.txt

:: Commit utan meddelande - VS Code ska öppnas
git commit
```

VS Code öppnas med en fil för commit-meddelandet. Skriv något, spara och stäng filen. Commiten genomförs.

---

## Extra konfigurationer (rekommenderat)

### Färgstöd i terminalen

```cmd
git config --global color.ui auto
```

### Standardbranch-namn

```cmd
:: Använd "main" istället för "master"
git config --global init.defaultBranch main
```

### Bättre logg-formatering

```cmd
:: Skapa ett alias för snygg logg
git config --global alias.lg "log --oneline --graph --all --decorate"

:: Använd sedan:
git lg
```

### Autostash vid rebase

```cmd
:: Automatiskt spara lokala ändringar vid rebase
git config --global rebase.autoStash true
```

### Pull-strategi

```cmd
:: Använd merge som standard vid pull
git config --global pull.rebase false

:: ELLER använd rebase som standard
git config --global pull.rebase true
```

---

## Konfigurera VS Code för Git

### Installera VS Code (om inte redan gjort)

Ladda ner från: https://code.visualstudio.com/

### Lägg till VS Code i PATH

Under installationen, markera:
- "Add to PATH" (viktigt!)
- "Register Code as an editor for supported file types"

### Verifiera VS Code-kommandot

```cmd
code --version
```

Om kommandot inte fungerar:

1. Öppna VS Code
2. Tryck `Ctrl+Shift+P`
3. Skriv: "Shell Command: Install 'code' command in PATH"
4. Starta om terminalen

---

## Felsökning

### Problem: "code is not recognized as an internal or external command"

**Lösning:**
1. Hitta VS Code-installationen (vanligtvis `C:\Program Files\Microsoft VS Code\bin`)
2. Lägg till i system PATH:
   - Sök efter "Environment Variables" i Windows
   - Redigera "Path" under "System variables"
   - Lägg till: `C:\Program Files\Microsoft VS Code\bin`
3. Starta om terminalen

### Problem: Git använder fortfarande Vim

**Kontrollera editor-inställningen:**
```cmd
git config --global --get core.editor
```

**Om den visar "vim" eller är tom, sätt den igen:**
```cmd
git config --global core.editor "code --wait"
```

### Problem: VS Code öppnas men Git säger "Aborting commit"

**Orsak:** Du stängde VS Code utan att spara commit-meddelandet.

**Lösning:** 
1. Öppna commit-filen i VS Code
2. Skriv ditt meddelande
3. Spara (`Ctrl+S`)
4. Stäng filen/fliken (`Ctrl+W`)

### Problem: Git rebase öppnar inte VS Code

**Kontrollera sekvens-editor:**
```cmd
git config --global sequence.editor "code --wait"
```

---

## Användbara Git-alias (valfritt)

```cmd
:: Kortare kommandon
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status

:: Använd sedan:
git st      :: istället för git status
git co main :: istället för git checkout main
```

---

## Komplett konfigurationsmall

Kör alla dessa kommandon för en grundläggande setup:

```cmd
:: Användarinformation
git config --global user.name "Ditt Namn"
git config --global user.email "din.email@example.com"

:: Editor
git config --global core.editor "code --wait"
git config --global sequence.editor "code --wait"

:: Standardbranch
git config --global init.defaultBranch main

:: Färg
git config --global color.ui auto

:: Pull-strategi
git config --global pull.rebase false

:: Användbara alias
git config --global alias.lg "log --oneline --graph --all --decorate"
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
```

---

## Verifiera komplett konfiguration

```cmd
git config --list --global
```

**Förväntad output (exempel):**
```
user.name=Ditt Namn
user.email=din.email@example.com
core.editor=code --wait
sequence.editor=code --wait
init.defaultbranch=main
color.ui=auto
pull.rebase=false
alias.lg=log --oneline --graph --all --decorate
alias.st=status
alias.co=checkout
alias.br=branch
alias.ci=commit
```

---

## Konfigurationsfilen

All global konfiguration sparas i:
```
%USERPROFILE%\.gitconfig
```

Du kan redigera den direkt i VS Code:
```cmd
code %USERPROFILE%\.gitconfig
```

**Exempel på `.gitconfig`:**
```ini
[user]
    name = Ditt Namn
    email = din.email@example.com
[core]
    editor = code --wait
[sequence]
    editor = code --wait
[init]
    defaultBranch = main
[color]
    ui = auto
[pull]
    rebase = false
[alias]
    lg = log --oneline --graph --all --decorate
    st = status
    co = checkout
    br = branch
    ci = commit
```