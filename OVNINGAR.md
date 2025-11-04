# Git Fördjupning - Övningar

## Översikt
**Total tid:** ~4 timmar  
**Del 1:** Individuell (1.5 timme) - Lokalt arbete och grundläggande Git  
**Del 2:** Par-programmering (2.5 timmar) - Samarbete via GitHub med Pull Requests

---

# DEL 1: INDIVIDUELL ÖVNING (1.5 timme)

## Mål
- Bygga commit-historik
- Öva branching och merging lokalt
- Hantera merge conflicts själv
- Använda recovery-verktyg

---

## Steg 1: Skapa ditt projekt (10 min)

### 1.1 Initiera repository
```cmd
:: Skapa projektmapp
cd %USERPROFILE%\DEV
mkdir min-portfolio
cd min-portfolio

:: Initiera Git
git init

:: Kolla status
git status
```

### 1.2 Skapa grundstruktur

**Öppna VS Code i projektmappen:**
```cmd
code .
```

**Skapa filer i VS Code:**

1. Skapa `index.html` med följande innehåll:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Min Portfolio</title>
</head>
<body>
    <h1>Välkommen</h1>
</body>
</html>
```

2. Skapa `style.css`:
```css
body { margin: 0; padding: 20px; }
h1 { color: #333; }
```

3. Skapa `app.js`:
```javascript
console.log('Portfolio loaded');
```

4. Skapa `.gitignore`:
```
node_modules/
.DS_Store
*.log
```

### 1.3 Första commit
```cmd
:: Lägg till alla filer
git add .

:: Kolla vad som är staged
git status

:: Commit
git commit -m "Initial commit: Add project structure"

:: Verifiera
git log --oneline
```

**CHECKPOINT:** Du ska se 1 commit i loggen.

---

## Steg 2: Bygg funktionalitet med branches (20 min)

### 2.1 Skapa "About"-sektion
```cmd
:: Skapa ny branch
git switch -c feature/about
```

**Öppna `index.html` i VS Code och lägg till (under `<h1>`-taggen):**
```html
<section id="about">
    <h2>Om mig</h2>
    <p>Jag är en utvecklare som lär mig Git!</p>
</section>
```

**Commit ändringen:**
```cmd
git add index.html
git commit -m "Add about section"

:: Kolla loggen
git log --oneline --graph --all
```

### 2.2 Skapa "Projects"-sektion (parallell branch)
```cmd
:: Byt tillbaka till main
git switch main

:: Skapa annan branch från main
git switch -c feature/projects
```

**Öppna `index.html` i VS Code och lägg till (under `<h1>`-taggen):**
```html
<section id="projects">
    <h2>Mina Projekt</h2>
    <ul>
        <li>Git-övning</li>
    </ul>
</section>
```

**Commit:**
```cmd
git add index.html
git commit -m "Add projects section"

:: Visa graf - du ska se två branches från main
git log --oneline --graph --all
```

**CHECKPOINT:** Du ska se 3 commits och 2 branches i grafen.

---

## Steg 3: Merge branches (15 min)

### 3.1 Merge första branchen
```cmd
:: Byt till main
git switch main

:: Merge about-branchen
git merge feature/about

:: Kolla resultatet
git log --oneline --graph --all

:: Öppna index.html - about-sektionen ska finnas
```

### 3.2 Merge andra branchen (konflikt!)
```cmd
:: Försök merge projects
git merge feature/projects

:: ❌ KONFLIKT! Båda branches ändrade samma del av index.html
```

**Du kommer se:**
```
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

### 3.3 Lös konflikten

**Öppna `index.html` i VS Code.**

Du kommer se konfliktmarkörer:
```html
<<<<<<< HEAD
<section id="about">
    <h2>Om mig</h2>
    <p>Jag är en utvecklare som lär mig Git!</p>
</section>
=======
<section id="projects">
    <h2>Mina Projekt</h2>
    <ul>
        <li>Git-övning</li>
    </ul>
</section>
>>>>>>> feature/projects
```

**Redigera filen:** Ta bort markörer och behåll BÅDA sektionerna:
```html
<h1>Välkommen</h1>
<section id="about">
    <h2>Om mig</h2>
    <p>Jag är en utvecklare som lär mig Git!</p>
</section>
<section id="projects">
    <h2>Mina Projekt</h2>
    <ul>
        <li>Git-övning</li>
    </ul>
</section>
```

**Spara filen och markera konflikten som löst:**
```cmd
:: Markera som löst
git add index.html

:: Kolla status
git status

:: Avsluta merge
git commit -m "Merge feature/projects - resolve conflict"

:: Visa slutresultat
git log --oneline --graph --all
```

**CHECKPOINT:** Konflikten löst, båda sektionerna finns i index.html.

---

## Steg 4: Experimentera och ångra (20 min)

### 4.1 Gör en "dålig" ändring
```cmd
:: Skapa ny branch för experiment
git switch -c experiment/styling
```

**Öppna `style.css` i VS Code och lägg till:**
```css
body { background-color: lime; }
h1 { font-family: Comic Sans MS; }
```

**Commit:**
```cmd
git add style.css
git commit -m "Add experimental styling"
```

**Lägg till mer CSS i `style.css`:**
```css
h2 { color: red; }
```

**Commit:**
```cmd
git add style.css
git commit -m "Change h2 color"
```

**Lägg till ännu mer i `style.css`:**
```css
p { font-size: 8px; }
```

**Commit:**
```cmd
git add style.css
git commit -m "Make text tiny"
```

### 4.2 Inse att det var dålig idé
```cmd
:: Visa vad vi gjort
git log --oneline

:: Kopiera hash för "Add experimental styling"
:: Exempel: abc1234

:: Byt tillbaka till main
git switch main

:: Experimentet finns inte här!
type style.css
```

### 4.3 Men vi vill ha EN av ändringarna!
```cmd
:: Cherry-pick bara color-committen
:: (använd hashen för "Change h2 color")
git cherry-pick <hash-för-h2-color>

:: Kolla filen
type style.css

:: Bara h2-färgen finns här!
```

### 4.4 Reflog-räddning
```cmd
:: Simulera att vi "tappade bort" något
git switch experiment/styling

:: Kopiera senaste commit-hash
git log --oneline

:: "Förstör" genom att gå tillbaka
git reset --hard HEAD~2

:: Oh nej! 2 commits borta!
git log --oneline

:: Reflog till undsättning
git reflog

:: Hitta "Make text tiny" i listan
:: Kopiera hashen

:: Återställ
git reset --hard <hash-för-make-text-tiny>

:: Allt tillbaka!
git log --oneline
```

**CHECKPOINT:** Du har övat cherry-pick och reflog.

---

## Steg 5: Städa upp (10 min)

```cmd
:: Byt till main
git switch main

:: Lista alla branches
git branch

:: Ta bort experiment-branch (vi vill inte ha den)
git branch -D experiment/styling

:: Ta bort feature-branches (redan mergade)
git branch -d feature/about
git branch -d feature/projects

:: Kolla slutresultat
git branch
git log --oneline --graph --all
```

**CHECKPOINT:** Bara main-branch kvar, ren historik.

---

## Sammanfattning Del 1

**Du har nu:**
- Skapat ett projekt från scratch
- Arbetat med flera parallella branches
- Löst merge conflicts
- Använt cherry-pick för att plocka specifika commits
- Räddats av reflog efter "misstag"
- Städat upp branches

**Nästa:** Samarbeta med en kompis via GitHub!

---
---

# DEL 2: PAR-ÖVNING (2.5 timmar)

## Mål
- Samarbeta via GitHub
- Använda Pull Requests
- Code review
- Hantera konflikter i team
- Branching-strategi

**OBS:** Ni är 2 personer. Person A och Person B.

---

## Steg 1: Skapa delat GitHub-repo (15 min)

### Person A: Skapa repository

```cmd
:: Gå till ditt portfolio-projekt
cd %USERPROFILE%\DEV\min-portfolio

:: Skapa GitHub-repo
gh repo create min-portfolio --public --source=. --remote=origin --push

```

**I GitHub-webben (Person A):**
1. Gå till repot på github.com
2. Klicka **Settings**
3. Klicka **Collaborators** (vänster meny)
4. Klicka **Add people**
5. Lägg till Person B's GitHub-användarnamn

### Person B: Acceptera och clona

**I GitHub:**
1. Kolla din email eller GitHub-notifikationer
2. Acceptera inbjudan

**I terminalen:**
```cmd
:: Gå till DEV-mappen
cd %USERPROFILE%\DEV

:: Clona repot
git clone https://github.com/ANVÄNDARNAMN/min-portfolio.git

:: OBS: Byt ANVÄNDARNAMN mot Person A's namn

:: Gå in i mappen
cd min-portfolio

:: Kolla innehåll
dir

:: Kolla remote
git remote -v

:: Kolla branches
git branch -a
```

**CHECKPOINT:** Båda har samma kod lokalt.

---

## Steg 2: Branching-strategi (10 min)

**Diskutera tillsammans:**

### Main-branch = Produktionskod
- Alltid fungerande
- Inget mergeas direkt till main
- Bara via Pull Requests

### Feature-branches
- En branch per funktion
- Format: `feature/beskrivning` eller `ditt-namn/beskrivning`
- Korta liv-längder

### Workflow
1. Skapa branch från main
2. Gör commits
3. Push till GitHub
4. Öppna Pull Request
5. Code review
6. Merge via GitHub
7. Pull ner till lokal main

**Skriv ner er strategi:** Vem gör vad först?

**CHECKPOINT:** Ni är överens om workflow.

---

## Steg 3: Första Pull Request - Person A (25 min)

### 3.1 Person A: Skapa feature

```cmd
:: Kolla att du är på main
git branch

:: Hämta senaste (god vana!)
git pull

:: Skapa branch
git switch -c feature/contact-form
```

**Skapa `contact.html` i VS Code:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>Kontakt</title>
</head>
<body>
    <h1>Kontakta mig</h1>
    <form>
        <input type="text" placeholder="Namn">
        <input type="email" placeholder="Email">
        <button>Skicka</button>
    </form>
</body>
</html>
```

**Commit:**
```cmd
git add contact.html
git commit -m "Add contact form page"
```

**Öppna `index.html` i VS Code och lägg till i `<body>`:**
```html
<nav>
    <a href="contact.html">Kontakt</a>
</nav>
```

**Commit:**
```cmd
git add index.html
git commit -m "Add navigation to contact page"

:: Kolla historik
git log --oneline
```

### 3.2 Person A: Push till GitHub

```cmd
:: Push branchen
git push -u origin feature/contact-form

:: Du får en länk i terminalen!
```

### 3.3 Person A: Öppna Pull Request

**I GitHub-webben:**
1. Gå till repot
2. Du ser en banner: "feature/contact-form had recent pushes"
3. Klicka **Compare & pull request**
4. Fyll i:
   - **Titel:** "Add contact form"
   - **Beskrivning:**
     ```
     ## Ändringar
     - Skapad contact.html med formulär
     - Lagt till navigering i index.html
     
     ## Testning
     - [x] Öppnat contact.html i browser
     - [x] Testat länkar
     
     @PERSON-B - Kan du granska?
     ```
5. Klicka **Create pull request**

**CHECKPOINT:** PR skapad och synlig på GitHub.

---

## Steg 4: Code Review - Person B (20 min)

### 4.1 Person B: Granska koden

**I GitHub-webben:**
1. Gå till **Pull requests**
2. Klicka på "Add contact form"
3. Klicka fliken **Files changed**
4. Granska koden:
   - Klicka på rad-nummer för att kommentera
   - Exempel: "Bra! Kanske lägga till labels för input-fält?"
5. Klicka **Review changes** (grön knapp uppe till höger)
6. Välj:
   - ⭐ **Comment** - Bara kommentar
   - ✅ **Approve** - Godkänn
   - ❌ **Request changes** - Behöver ändringar
7. För övningens skull: Välj **Request changes**
8. Skriv: "Snyggt! Men lägg till labels för bättre accessibility."
9. Klicka **Submit review**

### 4.2 Person A: Åtgärda feedback

```cmd
:: Kolla att du är på rätt branch
git branch
```

**Öppna `contact.html` i VS Code och uppdatera:**
```html
<form>
    <label>Namn: <input type="text" placeholder="Ditt namn"></label>
    <label>Email: <input type="email" placeholder="din@email.com"></label>
    <button>Skicka</button>
</form>
```

**Commit:**
```cmd
git add contact.html
git commit -m "Add labels to form inputs for accessibility"

:: Push
git push

:: Commiten dyker automatiskt upp i PR!
```

### 4.3 Person B: Godkänn och merge

**I GitHub:**
1. Gå tillbaka till PR
2. Se den nya commiten
3. Klicka **Review changes** igen
4. Välj **Approve**
5. Skriv: "Perfekt!"
6. Klicka **Submit review**
7. Klicka **Merge pull request**
8. Klicka **Confirm merge**
9. Klicka **Delete branch** (städa upp remote-branch)

**CHECKPOINT:** Första PR mergad!

---

## Steg 5: Synka lokalt - Båda (10 min)

### Person A:
```cmd
:: Byt till main
git switch main

:: Pull ner den mergade koden
git pull

:: Ta bort lokal feature-branch
git branch -d feature/contact-form

:: Kolla att allt finns
dir
git log --oneline
```

### Person B:
```cmd
:: Byt till main (om du inte redan är där)
git switch main

:: Pull ner den mergade koden
git pull

:: Kolla innehåll
dir
type contact.html

:: Loggen ska visa Person A's commits
git log --oneline
```

**CHECKPOINT:** Båda har samma kod.

---

## Steg 6: Parallellt arbete - Båda samtidigt (30 min)

### Person A: Jobbar med styling
```cmd
git switch main
git pull
git switch -c person-a/styling
```

**Öppna `style.css` i VS Code och lägg till:**
```css
nav { background: #333; padding: 10px; }
nav a { color: white; text-decoration: none; }
```

**Commit:**
```cmd
git add style.css
git commit -m "Add navigation styling"
```

**Lägg till mer i `style.css`:**
```css
form { max-width: 400px; margin: 20px 0; }
input, button { display: block; margin: 10px 0; padding: 8px; }
```

**Commit:**
```cmd
git add style.css
git commit -m "Add form styling"

:: Push
git push -u origin person-a/styling
```

### Person B: Jobbar med JavaScript (SAMTIDIGT!)
```cmd
git switch main
git pull
git switch -c person-b/validation
```

**Öppna `app.js` i VS Code och lägg till:**
```javascript
// Form validation
document.addEventListener('DOMContentLoaded', function() {
    console.log('Validation loaded');
});
```

**Commit:**
```cmd
git add app.js
git commit -m "Add form validation setup"
```

**Lägg till mer i `app.js`:**
```javascript
// TODO: Add email validation
```

**Commit:**
```cmd
git add app.js
git commit -m "Add validation placeholder"

:: Push
git push -u origin person-b/validation
```

**CHECKPOINT:** Två PRs aktiva samtidigt.

---

## Steg 7: Två Pull Requests (20 min)

### Person A: Skapa PR
1. Gå till GitHub
2. Öppna PR för `person-a/styling`
3. Titel: "Add CSS styling for navigation and form"
4. Assigna Person B som reviewer
5. Create PR

### Person B: Skapa PR
1. Gå till GitHub
2. Öppna PR för `person-b/validation`
3. Titel: "Add form validation"
4. Assigna Person A som reviewer
5. Create PR

### Båda: Granska varandras kod
- Person A granskar Person B's PR
- Person B granskar Person A's PR
- Godkänn om koden ser bra ut

### Merge båda
1. Merge Person A's PR först
2. Merge Person B's PR
   - Kan behöva uppdateras först (klicka "Update branch")
3. Delete båda remote-branches

**CHECKPOINT:** Båda PRs mergade.

---

## Steg 8: Konflikt-scenario (30 min)

### Setup: Båda ändrar samma fil

**Person A:**
```cmd
git switch main
git pull
git switch -c person-a/footer
```

**Öppna `index.html` i VS Code och lägg till (längst ner i `<body>`):**
```html
<footer>
    <p>© 2024 Person A Portfolio</p>
</footer>
```

**Commit:**
```cmd
git add index.html
git commit -m "Add footer"
git push -u origin person-a/footer
```

**Person B: (INNAN Person A's PR mergas!)**
```cmd
git switch main
git pull  
:: (Du har inte Person A's ändring än!)

git switch -c person-b/footer
```

**Öppna `index.html` i VS Code och lägg till (längst ner i `<body>`):**
```html
<footer>
    <p>Kontakt: person-b@email.com</p>
</footer>
```

**Commit:**
```cmd
git add index.html
git commit -m "Add contact footer"
git push -u origin person-b/footer
```

### Skapa PRs
- Båda öppnar PRs på GitHub

### Merge första PR
- Person A's PR mergas först
- Person B godkänner och mergar

### Konflikt uppstår!
**Person B's PR visar nu:**
```
This branch has conflicts that must be resolved
```

### Person B: Lös konflikten

**Metod 1: Via GitHub-webben (enklast)**
1. Klicka **Resolve conflicts** i PR
2. Redigera filen direkt i GitHub
3. Ta bort markörer, behåll BÅDA raderna:
   ```html
   <footer>
       <p>© 2024 Person A Portfolio</p>
       <p>Kontakt: person-b@email.com</p>
   </footer>
   ```
4. Klicka **Mark as resolved**
5. Klicka **Commit merge**
6. Nu kan PR mergas!

**Metod 2: Lokalt (mer kontroll)**
```cmd
:: Hämta senaste main
git switch main
git pull

:: Byt till din branch
git switch person-b/footer

:: Merge main in i din branch
git merge main
```

**Konflikt uppstår! Öppna `index.html` i VS Code.**

Hitta markörer och lös (behåll båda raderna). Spara filen.

**Fortsätt:**
```cmd
git add index.html
git commit -m "Resolve footer conflict"
git push

:: PR uppdateras och kan mergas!
```

**CHECKPOINT:** Konflikt löst, PR mergad.

---

## Steg 9: Protected Branch och Rules (15 min)

### Person A: Sätt upp branch protection

**I GitHub:**
1. Gå till **Settings**
2. Klicka **Branches** (vänster meny)
3. Under "Branch protection rules", klicka **Add rule**
4. Branch name pattern: `main`
5. Aktivera:
   - ✅ Require a pull request before merging
   - ✅ Require approvals (1)
   - ✅ Dismiss stale pull request approvals when new commits are pushed
6. Klicka **Create**

### Test: Försök pusha direkt till main

**Person B:**
```cmd
git switch main
git pull

:: Gör en ändring
echo /* More CSS */ >> style.css

git add style.css
git commit -m "Quick fix"

:: Försök pusha
git push
```

**Resultat:**
```
remote: error: GH006: Protected branch update failed
```

**Rätt sätt:**
```cmd
:: Ångra commit (behåll ändringen)
git reset HEAD~1

:: Skapa branch
git switch -c quick-fix

git add style.css
git commit -m "Quick CSS fix"
git push -u origin quick-fix

:: Öppna PR och vänta på godkännande!
```

**CHECKPOINT:** Branch protection fungerar.

---

## Steg 10: Avancerat - Rebase workflow (20 min)

### Person A: Flera små commits

```cmd
git switch main
git pull
git switch -c feature/gallery
```

**Öppna `index.html` i VS Code och lägg till:**
```html
<section id="gallery">
```

**Commit 1:**
```cmd
git add index.html
git commit -m "Add gallery section"
```

**Lägg till i `index.html`:**
```html
<h2>Galleri</h2>
```

**Commit 2:**
```cmd
git add index.html
git commit -m "Add gallery heading"
```

**Lägg till i `index.html`:**
```html
<div class="image-grid"></div>
```

**Commit 3:**
```cmd
git add index.html
git commit -m "Add image grid"
```

**Öppna `style.css` i VS Code och lägg till:**
```css
.image-grid { display: grid; }
```

**Commit 4:**
```cmd
git add style.css
git commit -m "Add grid styling"

:: Många små commits!
git log --oneline
```

### Person A: Squash commits innan PR

```cmd
:: Interaktiv rebase - kombinera 4 commits
git rebase -i HEAD~4

:: Editor öppnas med:
:: pick abc1234 Add gallery section
:: pick def5678 Add gallery heading
:: pick ghi9012 Add image grid
:: pick jkl3456 Add grid styling

:: Ändra till:
:: pick abc1234 Add gallery section
:: squash def5678 Add gallery heading
:: squash ghi9012 Add image grid
:: squash jkl3456 Add grid styling

:: Spara och stäng

:: Ny editor öppnas för commit-meddelande
:: Skriv:
:: Add complete gallery section with grid layout
:: 
:: - Gallery HTML structure
:: - Grid CSS styling

:: Spara och stäng

:: Kolla loggen - bara 1 commit!
git log --online

:: Force push (eftersom vi ändrat historik)
git push -f origin feature/gallery
```

**OBS:** Bara force-push på dina egna branches, aldrig på delade!

### Person B: Review och merge

1. Granska PR
2. Fin och ren historik - bara 1 commit!
3. Approve och merge

**CHECKPOINT:** Lärt er squash commits.

---

## Steg 11: Avslutande samarbete (15 min)

### Tillsammans: Reflektion och dokumentation

**Person A: Skapa README**

```cmd
git switch main
git pull
git switch -c docs/readme
```

**Skapa `README.md` i VS Code:**
```markdown
# Min Portfolio

Ett samarbetsprojekt för att lära Git och GitHub.

## Features
- Hem-sida
- Om mig-sektion
- Projekt-lista
- Kontaktformulär
- Galleri

## Utvecklare
- Person A
- Person B

## Workflow
Vi använder:
- Feature branches
- Pull Requests
- Code review
```

**Commit:**
```cmd
git add README.md
git commit -m "Add project documentation"
git push -u origin docs/readme
```

**Person B: Lägg till installation-guide**

1. Gå till PR för docs/readme
2. Klicka **Files changed**
3. Klicka på `README.md`
4. Klicka **Edit file** (penna-ikon)
5. Lägg till:
   ```markdown
   ## Installation
   ```bash
   git clone https://github.com/ANVÄNDARNAMN/min-portfolio.git
   cd min-portfolio
   ```
   
   Öppna `index.html` i din browser.
   ```
6. Klicka **Commit changes**
7. Välj "Commit directly to the docs/readme branch"
8. Klicka **Commit changes**

**Person A: Merge**

1. Se att Person B lagt till kod i din PR!
2. Review och merge

**CHECKPOINT:** Ni har samarbetat på samma branch!

---

## Sammanfattning Del 2

**Ni har nu:**
- Skapat och delat GitHub-repository
- Använt Pull Requests
- Gjort code reviews
- Löst merge conflicts (både lokalt och på GitHub)
- Jobbat parallellt på olika features
- Satt upp branch protection
- Lärt er rebase och squash
- Samarbetat på samma branch
- Dokumenterat ert projekt

---

## Reflektionsfrågor (diskutera tillsammans)

1. **Vad var svårast med samarbetet?**
2. **Hur undviker man merge conflicts?**
3. **När ska man squasha commits vs behålla dem?**
4. **Varför är code review viktigt?**
5. **Vad händer om någon force-pushar till main?**
6. **Hur skulle ni strukturera branches i ett större team?**


---

## Checklista - Är ni klara?

**Del 1 (Individuellt):**
- [ ] Skapat projekt med commits
- [ ] Arbetat med branches
- [ ] Löst merge conflict
- [ ] Använt cherry-pick
- [ ] Använt reflog för recovery

**Del 2 (Tillsammans):**
- [ ] Skapat delat GitHub-repo
- [ ] Gjort minst 3 Pull Requests var
- [ ] Granskat varandras kod
- [ ] Löst merge conflict (lokalt eller på GitHub)
- [ ] Satt upp branch protection
- [ ] Testat rebase/squash
- [ ] Skapat dokumentation tillsammans

**Om alla är checkade - Bra jobbat!**

Ni behärskar nu Git och GitHub-samarbete!
