# FAQ - Vanliga Problem och Lösningar

Detta dokument innehåller felsökning och utförliga instruktioner för vanliga problem som uppstår under övningarna.

---

## Problem med Pull Requests (Steg 7 & 8)

### PR dyker inte upp efter push

**Symptom:** Du har pushat din branch men ser ingen PR-banner eller kan inte hitta din PR.

**Lösning 1: Använd terminal-länken**

Efter `git push` får du en länk i terminalen:
```
remote: Create a pull request for 'person-a/styling' on GitHub by visiting:
remote:   https://github.com/ANVÄNDARNAMN/min-portfolio/pull/new/person-a/styling
```

Kopiera länken och öppna i din browser.

**Lösning 2: Skapa PR manuellt på GitHub**

1. Gå till `https://github.com/ANVÄNDARNAMN/min-portfolio`
2. Klicka på **Pull requests** (flik högst upp)
3. Klicka **New pull request** (grön knapp till höger)
4. Under "Compare changes":
   - **base:** `main` (vänster dropdown)
   - **compare:** `person-a/styling` (höger dropdown)
5. Klicka **Create pull request**
6. Fyll i titel och beskrivning
7. Assigna reviewer (klicka kugghjul under "Reviewers" till höger)
8. Klicka **Create pull request**

---

### Fel 1: Branch inte pushad till GitHub

**Symptom:** Din branch finns inte i dropdown-listan när du ska skapa PR.

**Kontrollera:**
```cmd
:: Lista alla remote branches
git branch -r
```

**Om din branch INTE finns i listan:**
```cmd
:: Kolla vilken branch du är på
git branch

:: Pusha branchen
git push -u origin DIN-BRANCH-NAMN

:: Exempel:
git push -u origin person-a/styling
```

**Försök skapa PR igen.**

---

### Fel 2: "There isn't anything to compare"

**Symptom:** GitHub säger att det inte finns något att jämföra.

**Orsak:** Du har inga nya commits jämfört med main.

**Kontrollera:**
```cmd
:: Se skillnaden mellan din branch och main
git log origin/main..DIN-BRANCH-NAMN

:: Om inget visas - du har inga nya commits!
```

**Lösning:**
```cmd
:: Gör en ändring och commit
echo /* Test */ >> style.css
git add style.css
git commit -m "Test commit"
git push
```

**Försök skapa PR igen.**

---

### Fel 3: Fel branch vald i PR

**Symptom:** Du ser "Comparing changes" men det ser konstigt ut.

**Kontrollera:**
- **base:** Ska vara `main` (dit du vill merga)
- **compare:** Ska vara DIN branch (ex: `person-a/styling`)

**Om det är omvänt:**
- Klicka på dropdown-menyn
- Byt plats på base och compare
- Det ska stå "Able to merge" (grönt)

---

### Fel 4: PR inte synlig i listan

**Symptom:** Du har skapat PR men ser den inte i Pull requests-listan.

**Kontrollera filter:**
1. Gå till **Pull requests**
2. Se till att filtret är "is:open" (standard)
3. INTE "is:closed" eller "is:merged"

**Sök efter din PR:**
1. I sökfältet skriv: `is:pr author:@me`
2. Detta visar alla dina PRs

**Verifiera att PR skapades:**
1. Klicka på din PR
2. Du ska se:
   - Commits-lista
   - Files changed
   - Status "Open"

---

## Problem med Merge Conflicts (Steg 8)

### Konflikt syns inte i PR

**Symptom:** Person B's PR visar inte "This branch has conflicts".

**Lösning 1: Vänta och uppdatera**
```
1. Vänta 10-20 sekunder (GitHub behöver processera)
2. Tryck F5 för att uppdatera sidan
3. Konflikten ska nu synas
```

**Lösning 2: Kontrollera att rätt PR mergades först**
```cmd
:: Gå till Person B's terminal
git switch main
git pull

:: Kolla loggen - Person A's commit ska finnas
git log --oneline -3

:: Du ska se Person A's "Add footer" commit
```

**Lösning 3: Manuellt skapa konflikten**

Om båda PR:s inte ändrade samma del av filen:

**Person A - Ändra exakt samma rader:**
```cmd
git switch person-a/footer
```

Öppna `index.html`, hitta `</body>` och lägg till DIREKT innan:
```html
<footer>
    <p>© 2024 Person A Portfolio</p>
</footer>
</body>
```

```cmd
git add index.html
git commit --amend --no-edit
git push -f
```

**Person B - Ändra SAMMA rader:**
```cmd
git switch person-b/footer
```

Öppna `index.html`, hitta `</body>` och lägg till DIREKT innan:
```html
<footer>
    <p>Kontakt: person-b@email.com</p>
</footer>
</body>
```

```cmd
git add index.html
git commit --amend --no-edit
git push -f
```

Nu ska konflikten uppstå när första PR mergas.

---

### "Resolve conflicts" knapp saknas

**Symptom:** PR visar konflikt men ingen "Resolve conflicts"-knapp.

**Lösning 1: Refresh och navigera**
1. Tryck F5
2. Klicka på **Commits**
3. Klicka på **Files changed**
4. Klicka tillbaka till **Conversation**
5. "Resolve conflicts" ska nu synas

**Lösning 2: Lös lokalt istället**

Detta är mer tillförlitligt:

```cmd
:: Hämta senaste main (med Person A's footer)
git switch main
git pull

:: Kolla - Person A's footer finns nu
type index.html

:: Byt till din branch
git switch person-b/footer

:: Merge main in i din branch
git merge main
```

**Terminal visar:**
```
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

**Öppna `index.html` i VS Code.**

Du ser konfliktmarkörer:
```html
<<<<<<< HEAD
<footer>
    <p>Kontakt: person-b@email.com</p>
</footer>
=======
<footer>
    <p>© 2024 Person A Portfolio</p>
</footer>
>>>>>>> main
```

**Förklaring av markörer:**
- `<<<<<<< HEAD` = Din kod (på nuvarande branch)
- `=======` = Separator
- `>>>>>>> main` = Deras kod (från main)

**Lös konflikten:**
1. Ta bort ALLA markörer (`<<<<<<<`, `=======`, `>>>>>>>`)
2. Behåll BÅDA footer-raderna:
```html
<footer>
    <p>© 2025 Person A Portfolio</p>
    <p>Kontakt: person-b@email.com</p>
</footer>
```
3. Spara filen

**Fortsätt:**
```cmd
:: Markera som löst
git add index.html

:: Kolla status (ska stå "all conflicts fixed")
git status

:: Avsluta merge
git commit -m "Resolve footer conflict"

:: Push till GitHub
git push

:: Gå till GitHub - PR uppdaterad och kan mergas!
```

---

### Konflikt löst men PR visar fortfarande fel

**Symptom:** Du har löst konflikten men PR säger fortfarande "has conflicts".

**Kontrollera:**
```cmd
git status
```

**Om det står:**
```
You have unmerged paths.
```

**Du har missat att göra `git add`:**
```cmd
git add .
git commit -m "Resolve conflicts"
git push
```

**Om det står:**
```
nothing to commit, working tree clean
```

**Men PR fortfarande visar konflikt:**
1. Vänta 30 sekunder
2. Refresh GitHub-sidan (F5)
3. Om fortfarande fel - kontrollera att du pushade till rätt branch

---

## Problem med Branch Protection (Steg 9)

### Push till main lyckas trots protection

**Symptom:** `git push` till main fungerar fortfarande.

**Kontrollera:**
1. Gå till GitHub → Settings → Branches
2. Kolla att regeln finns
3. Branch name pattern: `main` (exakt)
4. "Require a pull request before merging" är AKTIVERAD

**Om regeln saknas:**
- Skapa regeln igen enligt Steg 9
- Vänta 1 minut (GitHub behöver synka)
- Testa igen

**Om du är repo-owner/admin:**
- Branch protection gäller INTE alltid admins
- Under regeln, aktivera: "Do not allow bypassing the above settings"

---

## Problem med Rebase/Squash (Steg 10)

### Vim öppnas istället för VS Code

**Symptom:** `git rebase -i` öppnar Vim (svårt att använda).

**Lösning:**
```cmd
:: Sätt VS Code som editor för rebase
git config --global sequence.editor "code --wait"

:: Avbryt nuvarande rebase
git rebase --abort

:: Försök igen
git rebase -i HEAD~4
```

**Nu ska VS Code öppnas.**

Se även [GIT-SETUP.md](GIT-SETUP.md) för komplett editor-konfiguration.

---

### "Cannot force update because remote contains work"

**Symptom:** `git push -f` efter rebase ger felmeddelande.

**Förklaring:** Någon annan har pushat till din branch.

**Lösning (säker):**
```cmd
:: Hämta senaste
git fetch origin

:: Se skillnader
git log origin/DIN-BRANCH..HEAD

:: Om du är säker på att du vill skriva över:
git push -f origin DIN-BRANCH
```

**VARNING:** 
- Bara force-push på DINA egna branches
- ALDRIG på main eller delade branches
- Dubbelkolla att det är rätt branch!

---

## Allmänna Git-problem

### "fatal: not a git repository"

**Symptom:** Git-kommandon ger fel.

**Lösning:**
```cmd
:: Kontrollera att du är i rätt mapp
cd %USERPROFILE%\DEV\min-portfolio

:: Kolla om .git finns
dir /a

:: Om .git saknas - initiera
git init
```

---

### "Your branch is ahead of origin/main by X commits"

**Symptom:** `git status` visar detta meddelande.

**Förklaring:** Du har commits lokalt som inte är pushade.

**Lösning:**
```cmd
:: Pusha dina commits
git push

:: Om du INTE vill pusha:
git reset --hard origin/main
```

---

### "Your branch and origin/main have diverged"

**Symptom:** Lokal och remote historik skiljer sig.

**Lösning 1: Merge (säkrare):**
```cmd
git pull
```

**Lösning 2: Rebase (renare historik):**
```cmd
git pull --rebase
```

**Lösning 3: Kasta bort lokala ändringar:**
```cmd
git reset --hard origin/main
```

---

### Fel användarnamn/email i commits

**Symptom:** Commits visar fel namn i GitHub.

**Kontrollera nuvarande inställning:**
```cmd
git config user.name
git config user.email
```

**Ändra:**
```cmd
git config --global user.name "Ditt Riktiga Namn"
git config --global user.email "din@email.com"
```

**För framtida commits:** Ovanstående räcker.

**För redan gjorda commits (avancerat):**
```cmd
git rebase -i HEAD~3
:: Markera commits du vill ändra med "edit"
git commit --amend --author="Namn <email@example.com>" --no-edit
git rebase --continue
git push -f
```

---

## GitHub-specifika problem

### "Permission denied (publickey)"

**Symptom:** Kan inte pusha/pulla från GitHub.

**Lösning med GitHub CLI (enklast):**
```cmd
gh auth login
:: Följ instruktionerna
```

**Lösning med HTTPS (alternativ):**
```cmd
:: Använd HTTPS istället för SSH
git remote set-url origin https://github.com/ANVÄNDARNAMN/min-portfolio.git

:: Nästa push/pull kommer be om lösenord
:: Använd Personal Access Token istället för lösenord
```

---

### Collaborator kan inte se repot

**Symptom:** Person B ser inte repot efter inbjudan.

**Kontrollera:**
1. Person B: Kolla email för inbjudan
2. Person B: Kolla https://github.com/notifications
3. Acceptera inbjudan

**Om inbjudan inte kommit:**
1. Person A: GitHub → Repo → Settings → Collaborators
2. Kolla att inbjudan är "Pending"
3. Klicka "Copy invite link"
4. Skicka länken direkt till Person B

---

### Kan inte merga PR - "Review required"

**Symptom:** Merge-knappen är grå trots godkänd review.

**Orsak:** Branch protection kräver godkännande.

**Lösning:**
1. Se till att någon ANNAN än PR-skaparen har godkänt
2. Klicka på **Reviews** - ska visa checkmark
3. Om du är ensam i repot: Settings → Branches → Redigera regel
4. Avaktivera "Require approvals" temporärt för övning

---

## VS Code-problem

### VS Code öppnar inte från terminalen

**Symptom:** `code .` ger "command not found" eller liknande.

**Lösning:**

1. Öppna VS Code
2. Tryck `Ctrl+Shift+P`
3. Skriv: "Shell Command: Install 'code' command in PATH"
4. Klicka på alternativet
5. Starta om terminalen
6. Testa: `code --version`

Se även [GIT-SETUP.md](GIT-SETUP.md) för mer detaljer.

---

### Git commit öppnar fortfarande Vim

**Symptom:** Trots konfiguration öppnas Vim vid `git commit`.

**Lösning:**
```cmd
:: Sätt editor igen
git config --global core.editor "code --wait"

:: Verifiera
git config --global --get core.editor

:: Ska visa: code --wait
```

**Om fortfarande Vim:**
```cmd
:: Kolla om lokal config överskrider global
cd min-portfolio
git config --local --get core.editor

:: Om det visar något - ta bort det
git config --local --unset core.editor
```

---

## Checklista Felsökning

När något inte fungerar, gå igenom denna lista:

**Grundläggande:**
- [ ] Är jag i rätt mapp? (`cd min-portfolio`)
- [ ] Har jag internet? (testa https://github.com)
- [ ] Är Git installerat? (`git --version`)

**För branch-problem:**
- [ ] Vilken branch är jag på? (`git branch`)
- [ ] Är branchen pushad? (`git branch -r`)
- [ ] Har jag commits? (`git log --oneline`)

**För PR-problem:**
- [ ] Har jag pushat? (`git push`)
- [ ] Kopierade jag rätt URL?
- [ ] Är jag inloggad på GitHub?
- [ ] Valde jag rätt base/compare?

**För konflikt-problem:**
- [ ] Mergade rätt PR först?
- [ ] Har jag väntat 20 sekunder och refreshat?
- [ ] Ändrade vi samma rader i samma fil?

**För merge-problem:**
- [ ] Har jag gjort `git add` efter konfliktlösning?
- [ ] Har jag committat? (`git status`)
- [ ] Har jag pushat? (`git push`)

---

## Användbara kommandon för felsökning

```cmd
:: Se var du är
cd

:: Se vilken branch
git branch

:: Se status
git status

:: Se remote-branches
git branch -r

:: Se senaste commits
git log --oneline -5

:: Se vad som är ändrat
git diff

:: Se remote URL
git remote -v

:: Se vem du är (för commits)
git config user.name
git config user.email

:: Se all config
git config --list

:: Starta om från senaste commit
git reset --hard HEAD

:: Starta om från remote
git reset --hard origin/main
```
