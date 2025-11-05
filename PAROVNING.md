# Pull Request Parövning (4 timmar)

## Översikt
**Tid:** ca 4 timmar  
**Format:** 2 personer per grupp  
**Syfte:** Öva PR-workflow, code review och konflikthantering

---

## Del 1: Grundläggande PR-workflow (60 min)

### Setup: Person A skapar projekt (10 min)

**Person A:**
```cmd
cd %USERPROFILE%\DEV
mkdir team-portfolio
cd team-portfolio
git init
code .
```

**Skapa `index.html` i VS Code:**
```html
<!DOCTYPE html>
<html lang="sv">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Team Portfolio</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>Team Portfolio</h1>
    </header>
    <main>
        <p>Under konstruktion</p>
    </main>
</body>
</html>
```

**Skapa `style.css`:**
```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    line-height: 1.6;
    color: #333;
}
```

**Skapa `.gitignore`:**
```
node_modules/
.DS_Store
*.log
.vscode/
```

```cmd
git add .
git commit -m "Initial commit: Project structure"
gh repo create team-portfolio --public --source=. --remote=origin --push
```

**Lägg till Person B som collaborator:**
- GitHub → Settings → Collaborators → Add people
- Lägg till Person B's användarnamn

**Skapa GitHub Project:**
1. GitHub → Projects → New project
2. Välj "Board" template
3. Namn: "Team Portfolio Development"
4. Create project
5. Kolumner finns redan: Todo, In Progress, Done

**Person B: Acceptera inbjudan och klona**

```cmd
cd %USERPROFILE%\DEV
git clone https://github.com/PERSON-A-USERNAME/team-portfolio.git
cd team-portfolio
dir
```

**CHECKPOINT:** Båda har identiskt projekt lokalt.

**Skapa första issue:**

**Person A:**
1. GitHub → Issues → New issue
2. Titel: "Add navigation menu"
3. Beskrivning: "Skapa navigeringsmeny med länkar till olika sektioner"
4. Assigna till Person A
5. Create issue
6. Notera issue-numret (t.ex. #1)
7. Dra issue till "In Progress" i Project board

---

### Iteration 1: Person A - Navigation (15 min)

**Person A:**
```cmd
git switch main
git pull
git switch -c feature/navigation
```

**Öppna `index.html`, lägg till efter `<header>`:**
```html
<nav>
    <ul>
        <li><a href="#home">Hem</a></li>
        <li><a href="#about">Om oss</a></li>
        <li><a href="#projects">Projekt</a></li>
        <li><a href="#contact">Kontakt</a></li>
    </ul>
</nav>
```

**Öppna `style.css`, lägg till:**
```css
header {
    background: #2c3e50;
    color: white;
    padding: 1.5rem;
    text-align: center;
}

nav ul {
    list-style: none;
    display: flex;
    justify-content: center;
    gap: 2rem;
    padding: 1rem;
    background: #34495e;
}

nav a {
    color: white;
    text-decoration: none;
    font-weight: 500;
}

nav a:hover {
    color: #3498db;
}
```

```cmd
git add .
git commit -m "Add navigation menu with styling

Fixes #1"
git push -u origin feature/navigation
```

**Öppna PR på GitHub:**
- Titel: "Add navigation menu"
- Beskrivning: "Lägger till navigeringsmeny med fyra länkar. Closes #1"
- Assigna Person B som reviewer
- Länka till issue #1 (GitHub länkar automatiskt via "Closes #1")
- Create pull request

**Person B: Code review**
- Gå till Pull requests → Klicka på PR
- Files changed → Granska koden
- Lägg minst en kommentar på en rad
- Review changes → Request changes
- Kommentar: "Lägg till active state för navigation länkar"

**Person A: Implementera feedback**

**I `style.css`, lägg till:**
```css
nav a.active {
    color: #3498db;
    border-bottom: 2px solid #3498db;
}
```

```cmd
git add style.css
git commit -m "Add active state styling for navigation"
git push
```

**Person B: Godkänn och merge**
- Se uppdateringen i PR
- Review changes → Approve → Submit review
- Merge pull request → Confirm merge
- Delete branch
- Se att issue #1 stängdes automatiskt
- Dra kortet till "Done" i Project board

**Båda:**
```cmd
git switch main
git pull
git branch -d feature/navigation
```

**CHECKPOINT:** Navigation mergad till main.

**Skapa nästa issue:**

**Person B:**
1. GitHub → Issues → New issue
2. Titel: "Add footer with social links"
3. Beskrivning: "Skapa footer med kontaktinformation och social media länkar"
4. Assigna till Person B
5. Create issue (t.ex. #2)
6. Dra till "In Progress" i Project

---

### Iteration 2: Person B - Footer (15 min)

**Person B:**
```cmd
git switch main
git pull
git switch -c feature/footer
```

**Öppna `index.html`, lägg till före `</body>`:**
```html
<footer>
    <p>&copy; 2025 Team Portfolio</p>
    <p>Kontakt: team@example.com</p>
</footer>
```

**Öppna `style.css`, lägg till:**
```css
main {
    min-height: 60vh;
    padding: 2rem;
}

footer {
    background: #2c3e50;
    color: white;
    padding: 2rem;
    text-align: center;
}

footer p {
    margin: 0.5rem 0;
}
```

```cmd
git add .
git commit -m "Add footer with contact information

References #2"
git push -u origin feature/footer
```

**Öppna PR:**
- Titel: "Add footer section"
- Beskrivning: "Closes #2"
- Assigna Person A som reviewer

**Person A: Review**
- Request changes: "Lägg till social media länkar"
- Kommentera i issue #2: "Behöver social media länkar enligt feedback"

**Person B: Uppdatera**

**I `index.html`, lägg till i footer före copyright:**
```html
<div class="social-links">
    <a href="#">GitHub</a>
    <a href="#">LinkedIn</a>
    <a href="#">Twitter</a>
</div>
```

**I `style.css`, lägg till:**
```css
.social-links {
    margin-bottom: 1rem;
}

.social-links a {
    color: white;
    margin: 0 1rem;
    text-decoration: none;
}

.social-links a:hover {
    color: #3498db;
}
```

```cmd
git add .
git commit -m "Add social media links to footer

Addresses feedback in #2"
git push
```

**Person A: Approve och merge**

**Båda:**
```cmd
git switch main
git pull
git branch -d feature/footer
```

**CHECKPOINT:** Footer mergad.

**Person A: Skapa issue för about section**
1. GitHub → Issues → New issue
2. Titel: "Add about section with team members"
3. Assigna till Person A
4. Create issue (#3)
5. Dra till "In Progress"

---

### Iteration 3: Person A - About Section (20 min)

**Person A:**
```cmd
git switch main
git pull
git switch -c feature/about-section
```

**Öppna `index.html`, ersätt `<main>` innehåll:**
```html
<main>
    <section id="about">
        <h2>Om Teamet</h2>
        <p>Vi är två utvecklare som lär oss Git workflow tillsammans.</p>
        <div class="team-members">
            <div class="member">
                <h3>Person A</h3>
                <p>Frontend utvecklare</p>
                <p>Fokus på HTML och CSS</p>
            </div>
            <div class="member">
                <h3>Person B</h3>
                <p>Backend utvecklare</p>
                <p>Fokus på logic och struktur</p>
            </div>
        </div>
    </section>
</main>
```

**I `style.css`, lägg till:**
```css
#about {
    max-width: 1000px;
    margin: 0 auto;
    text-align: center;
}

#about h2 {
    color: #2c3e50;
    margin-bottom: 1rem;
    font-size: 2rem;
}

#about > p {
    color: #7f8c8d;
    margin-bottom: 2rem;
}

.team-members {
    display: flex;
    gap: 2rem;
    justify-content: center;
    margin-top: 2rem;
}

.member {
    background: #ecf0f1;
    padding: 2rem;
    border-radius: 8px;
    min-width: 250px;
}

.member h3 {
    color: #2c3e50;
    margin-bottom: 0.5rem;
}

.member p {
    color: #7f8c8d;
    margin: 0.25rem 0;
}
```

```cmd
git add .
git commit -m "Add about section with team members

Closes #3"
git push -u origin feature/about-section
```

**Öppna PR, Person B granskar direkt och mergar (approve utan changes)**

**Person B: Dra #3 till "Done" i Project**

**Båda:**
```cmd
git switch main
git pull
git branch -d feature/about-section
```

**CHECKPOINT:** About section klar. Del 1 slutförd.

**Kontrollera Project board:**
- Alla 3 issues (#1, #2, #3) ska vara i "Done"
- GitHub → Projects → Team Portfolio Development

---

## Del 2: Parallellt arbete och konflikter (90 min)

**Skapa issues för nästa features:**

**Person A:**
1. GitHub → Issues → New issue
2. Titel: "Create projects page"
3. Assigna till Person A
4. Create issue (#4)

**Person B:**
1. GitHub → Issues → New issue
2. Titel: "Create contact form page"
3. Assigna till Person B
4. Create issue (#5)

**Båda: Dra era issues till "In Progress"**

---

### Scenario 1: Olika filer - Inga konflikter (20 min)

**Person A: (Startar först)**
```cmd
git switch main
git pull
git switch -c feature/projects-section
```

**Skapa ny fil `projects.html`:**
```html
<!DOCTYPE html>
<html lang="sv">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Projekt - Team Portfolio</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>Våra Projekt</h1>
    </header>
    <nav>
        <ul>
            <li><a href="index.html">Hem</a></li>
            <li><a href="#about">Om oss</a></li>
            <li><a href="projects.html" class="active">Projekt</a></li>
            <li><a href="#contact">Kontakt</a></li>
        </ul>
    </nav>
    <main>
        <section class="projects">
            <div class="project-card">
                <h3>Team Portfolio</h3>
                <p>En responsiv portfolio-sida</p>
                <p>Tekniker: HTML, CSS, Git</p>
            </div>
        </section>
    </main>
</body>
</html>
```

**I `style.css`, lägg till:**
```css
.projects {
    max-width: 1000px;
    margin: 0 auto;
}

.project-card {
    background: white;
    border: 1px solid #ddd;
    padding: 2rem;
    margin: 1rem 0;
    border-radius: 8px;
}

.project-card h3 {
    color: #2c3e50;
    margin-bottom: 1rem;
}

.project-card p {
    color: #7f8c8d;
    margin: 0.5rem 0;
}
```

```cmd
git add .
git commit -m "Add projects page

Closes #4"
git push -u origin feature/projects-section
```

**Öppna PR (VÄNTA med merge)**

---

**Person B: (Samtidigt, INNAN Person A mergar)**
```cmd
git switch main
git pull
git switch -c feature/contact-form
```

**Skapa ny fil `contact.html`:**
```html
<!DOCTYPE html>
<html lang="sv">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kontakt - Team Portfolio</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>Kontakta Oss</h1>
    </header>
    <nav>
        <ul>
            <li><a href="index.html">Hem</a></li>
            <li><a href="#about">Om oss</a></li>
            <li><a href="projects.html">Projekt</a></li>
            <li><a href="contact.html" class="active">Kontakt</a></li>
        </ul>
    </nav>
    <main>
        <form class="contact-form">
            <label>
                Namn:
                <input type="text" name="name" required>
            </label>
            <label>
                Email:
                <input type="email" name="email" required>
            </label>
            <label>
                Meddelande:
                <textarea name="message" rows="5" required></textarea>
            </label>
            <button type="submit">Skicka</button>
        </form>
    </main>
</body>
</html>
```

**I `style.css`, lägg till:**
```css
.contact-form {
    max-width: 600px;
    margin: 2rem auto;
}

.contact-form label {
    display: block;
    margin: 1.5rem 0;
    color: #2c3e50;
}

.contact-form input,
.contact-form textarea {
    width: 100%;
    padding: 0.75rem;
    margin-top: 0.5rem;
    border: 1px solid #ddd;
    border-radius: 4px;
    font-family: Arial, sans-serif;
}

.contact-form button {
    background: #2c3e50;
    color: white;
    padding: 0.75rem 2rem;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 1rem;
}

.contact-form button:hover {
    background: #34495e;
}
```

```cmd
git add .
git commit -m "Add contact form page

Closes #5"
git push -u origin feature/contact-form
```

**Öppna PR**

---

**Nu mergas båda PRs i ordning:**
1. Person A's PR mergas först
2. Person B's PR mergas direkt efter (ingen konflikt)

**Båda: Dra #4 och #5 till "Done" i Project**

**Båda:**
```cmd
git switch main
git pull
git branch -d feature/projects-section
git branch -d feature/contact-form
```

**CHECKPOINT:** Båda PRs mergade utan konflikter.

**Skapa issues för nästa features (kommer skapa konflikt):**

**Person A:**
- Titel: "Add hero section to homepage"
- Create issue (#6), dra till "In Progress"

**Person B:**
- Titel: "Add statistics section to homepage"
- Create issue (#7), dra till "In Progress"

---

### Scenario 2: Samma fil - GitHub konfliktlösning (35 min)

**Person A:**
```cmd
git switch main
git pull
git switch -c feature/hero-section
```

**Öppna `index.html`, lägg till direkt efter `<nav>`:**
```html
<section class="hero">
    <h2>Välkommen till vår Portfolio</h2>
    <p>Vi bygger moderna webbapplikationer</p>
    <a href="#about" class="cta-button">Läs mer om oss</a>
</section>
```

**I `style.css`, lägg till efter nav-styling:**
```css
.hero {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    padding: 4rem 2rem;
    text-align: center;
}

.hero h2 {
    font-size: 2.5rem;
    margin-bottom: 1rem;
}

.hero p {
    font-size: 1.2rem;
    margin-bottom: 2rem;
}

.cta-button {
    background: white;
    color: #667eea;
    padding: 1rem 2rem;
    text-decoration: none;
    border-radius: 4px;
    font-weight: bold;
}
```

```cmd
git add .
git commit -m "Add hero section to homepage

Closes #6"
git push -u origin feature/hero-section
```

**Öppna PR (VÄNTA med merge)**

---

**Person B: (SAMTIDIGT)**
```cmd
git switch main
git pull
git switch -c feature/stats-section
```

**Öppna `index.html`, lägg till direkt efter `<nav>` (SAMMA PLATS):**
```html
<section class="stats">
    <div class="stat">
        <h3>10+</h3>
        <p>Projekt</p>
    </div>
    <div class="stat">
        <h3>2</h3>
        <p>Utvecklare</p>
    </div>
    <div class="stat">
        <h3>100%</h3>
        <p>Engagemang</p>
    </div>
</section>
```

**I `style.css`, lägg till efter nav-styling:**
```css
.stats {
    display: flex;
    justify-content: space-around;
    padding: 3rem 2rem;
    background: #ecf0f1;
}

.stat {
    text-align: center;
}

.stat h3 {
    font-size: 2rem;
    color: #2c3e50;
    margin-bottom: 0.5rem;
}

.stat p {
    color: #7f8c8d;
    font-size: 1rem;
}
```

```cmd
git add .
git commit -m "Add statistics section to homepage

Closes #7"
git push -u origin feature/stats-section
```

**Öppna PR**

---

**Merge Person A's PR först**

**Person B's PR visar nu konflikt**

**Person B: Lös konflikt via GitHub**
1. Klicka "Resolve conflicts" i PR
2. Se konfliktmarkörer i `index.html`:
```html
<<<<<<< feature/stats-section
<section class="stats">
    ...
</section>
=======
<section class="hero">
    ...
</section>
>>>>>>> main
```

3. Redigera till att ha BÅDA sektionerna:
```html
<section class="hero">
    <h2>Välkommen till vår Portfolio</h2>
    <p>Vi bygger moderna webbapplikationer</p>
    <a href="#about" class="cta-button">Läs mer om oss</a>
</section>
<section class="stats">
    <div class="stat">
        <h3>10+</h3>
        <p>Projekt</p>
    </div>
    <div class="stat">
        <h3>2</h3>
        <p>Utvecklare</p>
    </div>
    <div class="stat">
        <h3>100%</h3>
        <p>Engagemang</p>
    </div>
</section>
```

4. Mark as resolved
5. Commit merge
6. Merge pull request
7. Dra #6 och #7 till "Done" i Project

**Båda:**
```cmd
git switch main
git pull
git branch -d feature/hero-section
git branch -d feature/stats-section
```

**CHECKPOINT:** Konflikt löst via GitHub.

**Skapa issues för footer-uppdateringar:**

**Person A:**
- Titel: "Update footer with real links and phone"
- Create (#8), dra till "In Progress"

**Person B:**
- Titel: "Add newsletter to footer"
- Create (#9), dra till "In Progress"

---

### Scenario 3: Samma fil - Lokal konfliktlösning (35 min)

**Person A:**
```cmd
git switch main
git pull
git switch -c feature/update-footer
```

**Öppna `index.html`, uppdatera footer:**
```html
<footer>
    <div class="social-links">
        <a href="https://github.com">GitHub</a>
        <a href="https://linkedin.com">LinkedIn</a>
        <a href="https://twitter.com">Twitter</a>
    </div>
    <p>&copy; 2025 Team Portfolio. Alla rättigheter förbehållna.</p>
    <p>Kontakt: team@example.com | Tel: 123-456-789</p>
</footer>
```

```cmd
git add index.html
git commit -m "Update footer with real links and phone number

Closes #8"
git push -u origin feature/update-footer
```

**Öppna PR (VÄNTA med merge)**

---

**Person B:**
```cmd
git switch main
git pull
git switch -c feature/footer-newsletter
```

**Öppna `index.html`, uppdatera footer:**
```html
<footer>
    <div class="newsletter">
        <h3>Prenumerera på vårt nyhetsbrev</h3>
        <form>
            <input type="email" placeholder="Din email">
            <button type="submit">Prenumerera</button>
        </form>
    </div>
    <div class="social-links">
        <a href="#">GitHub</a>
        <a href="#">LinkedIn</a>
        <a href="#">Twitter</a>
    </div>
    <p>&copy; 2025 Team Portfolio</p>
    <p>Kontakt: team@example.com</p>
</footer>
```

**I `style.css`, lägg till:**
```css
.newsletter {
    margin-bottom: 1.5rem;
}

.newsletter h3 {
    margin-bottom: 1rem;
}

.newsletter form {
    display: flex;
    gap: 1rem;
    justify-content: center;
}

.newsletter input {
    padding: 0.5rem;
    border: 1px solid #ddd;
    border-radius: 4px;
}

.newsletter button {
    background: #3498db;
    color: white;
    padding: 0.5rem 1.5rem;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}
```

```cmd
git add .
git commit -m "Add newsletter subscription to footer

Closes #9"
git push -u origin feature/footer-newsletter
```

**Öppna PR**

---

**Merge Person A's PR först**

**Person B: Lös konflikt lokalt**

```cmd
git switch main
git pull
git switch feature/footer-newsletter
git merge main
```

**Konflikt uppstår. Öppna `index.html` i VS Code.**

**Se konfliktmarkörer:**
```html
<<<<<<< HEAD
<div class="newsletter">
    ...
</div>
<div class="social-links">
    <a href="#">GitHub</a>
    ...
</div>
<p>&copy; 2025 Team Portfolio</p>
<p>Kontakt: team@example.com</p>
=======
<div class="social-links">
    <a href="https://github.com">GitHub</a>
    ...
</div>
<p>&copy; 2025 Team Portfolio. Alla rättigheter förbehållna.</p>
<p>Kontakt: team@example.com | Tel: 123-456-789</p>
>>>>>>> main
```

**Redigera till:**
```html
<footer>
    <div class="newsletter">
        <h3>Prenumerera på vårt nyhetsbrev</h3>
        <form>
            <input type="email" placeholder="Din email">
            <button type="submit">Prenumerera</button>
        </form>
    </div>
    <div class="social-links">
        <a href="https://github.com">GitHub</a>
        <a href="https://linkedin.com">LinkedIn</a>
        <a href="https://twitter.com">Twitter</a>
    </div>
    <p>&copy; 2025 Team Portfolio. Alla rättigheter förbehållna.</p>
    <p>Kontakt: team@example.com | Tel: 123-456-789</p>
</footer>
```

```cmd
git add index.html
git commit -m "Merge main and resolve footer conflict"
git push
```

**Person A: Approve och merge PR**

**Båda: Dra #8 och #9 till "Done"**

**Båda:**
```cmd
git switch main
git pull
git branch -d feature/update-footer
git branch -d feature/footer-newsletter
```

**CHECKPOINT:** Lokal konflikt löst. Del 2 slutförd.

**Kontrollera Project:**
- Issues #4-9 ska vara i "Done"

---

## Del 3: Code review och iteration (60 min)

**Person A: Skapa issue med medvetet dålig kod**
1. Titel: "Add skills section"
2. Label: "enhancement"
3. Beskrivning: "Lägga till skills section på startsidan"
4. Assigna till Person A
5. Create (#10)
6. Dra till "In Progress"

---

### Scenario: Förbättra kod-kvalitet (60 min)

**Person A:**
```cmd
git switch main
git pull
git switch -c feature/skills-section
```

**Öppna `index.html`, lägg till efter about section:**
```html
<section id="skills">
    <h2>Kompetenser</h2>
    <div style="display: flex; gap: 20px;">
        <div style="background: gray; padding: 20px;">
            <h3>HTML</h3>
            <p>Expert</p>
        </div>
        <div style="background: gray; padding: 20px;">
            <h3>CSS</h3>
            <p>Expert</p>
        </div>
        <div style="background: gray; padding: 20px;">
            <h3>Git</h3>
            <p>Lär mig</p>
        </div>
    </div>
</section>
```

```cmd
git add index.html
git commit -m "Add skills section

Work in progress for #10"
git push -u origin feature/skills-section
```

**Öppna PR**

---

**Person B: Första review - Request changes**

Kommentarer i PR:
1. På rad med inline styles: "Flytta styling till CSS-fil"
2. På "gray": "Använd färgkod från vår färgpalett"
3. På section: "Lägg till CSS-klass istället för inline styles"

**Request changes med kommentar:**
"Bra start! Men vi behöver:
1. Ta bort inline styles
2. Skapa CSS-klasser
3. Använd projektets färgpalett
4. Lägg till responsiv design"

**Kommentera även i issue #10:** "Code review begär ändringar, se PR"

---

**Person A: Första iterationen**

**Öppna `index.html`, uppdatera:**
```html
<section id="skills">
    <h2>Kompetenser</h2>
    <div class="skills-container">
        <div class="skill-card">
            <h3>HTML</h3>
            <p>Expert</p>
        </div>
        <div class="skill-card">
            <h3>CSS</h3>
            <p>Expert</p>
        </div>
        <div class="skill-card">
            <h3>Git</h3>
            <p>Lär mig</p>
        </div>
    </div>
</section>
```

**I `style.css`, lägg till:**
```css
#skills {
    max-width: 1000px;
    margin: 3rem auto;
    text-align: center;
}

#skills h2 {
    color: #2c3e50;
    margin-bottom: 2rem;
}

.skills-container {
    display: flex;
    gap: 2rem;
}

.skill-card {
    background: #ecf0f1;
    padding: 2rem;
}

.skill-card h3 {
    color: #2c3e50;
}
```

```cmd
git add .
git commit -m "Move skills styling to CSS file

Addresses review feedback in #10"
git push
```

---

**Person B: Andra review - Request changes igen**

Kommentar:
"Bättre! Men:
1. Lägg till border-radius på cards
2. Gör flexbox responsiv med wrap
3. Lägg till hover-effekt
4. Centrera innehållet i cards"

---

**Person A: Andra iterationen**

**Uppdatera `style.css`:**
```css
#skills {
    max-width: 1000px;
    margin: 3rem auto;
    padding: 0 1rem;
    text-align: center;
}

#skills h2 {
    color: #2c3e50;
    margin-bottom: 2rem;
    font-size: 2rem;
}

.skills-container {
    display: flex;
    gap: 2rem;
    flex-wrap: wrap;
    justify-content: center;
}

.skill-card {
    background: #ecf0f1;
    padding: 2rem;
    border-radius: 8px;
    min-width: 200px;
    text-align: center;
    transition: transform 0.3s ease;
}

.skill-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}

.skill-card h3 {
    color: #2c3e50;
    margin-bottom: 0.5rem;
}

.skill-card p {
    color: #7f8c8d;
}
```

```cmd
git add style.css
git commit -m "Add responsive design and hover effects to skills

Final iteration for #10"
git push
```

---

**Person B: Tredje review - Approve**

Kommentar:
"Perfekt! Nu är koden ren och följer projektets struktur. Bra jobbat med responsiviteten."

**Approve och merge**

**Dra #10 till "Done" i Project**

**Båda:**
```cmd
git switch main
git pull
git branch -d feature/skills-section
```

**CHECKPOINT:** Kod förbättrad genom iterativ review. Del 3 slutförd.

**Kontrollera Project:**
- Issue #10 i "Done"
- Totalt 10 stängda issues

---

## Del 4: Avancerad workflow (30 min)

**Person B: Skapa issue för dokumentation**
- Titel: "Add project documentation"
- Create (#11), dra till "In Progress"

---

### Branch protection och professionella regler (30 min)

**Person A: Aktivera branch protection**

1. GitHub → Settings → Branches
2. Add rule för `main`
3. Aktivera:
   - Require pull request before merging
   - Require approvals: 1
4. Save changes

**Testa att bryta regeln:**

**Person A:**
```cmd
git switch main
echo "Test" >> index.html
git add index.html
git commit -m "Test direct push"
git push
```

**Får felmeddelande. Regel fungerar.**

```cmd
git reset --hard HEAD~1
```

---

**Person B: PR med många commits**

```cmd
git switch main
git pull
git switch -c feature/documentation
```

**Skapa `README.md`:**
```markdown
# Team Portfolio

Portfolio-webbplats för vårt team.
```

```cmd
git add README.md
git commit -m "Add README"
```

**Uppdatera README.md:**
```markdown
# Team Portfolio

Portfolio-webbplats för vårt team.

## Tekniker
- HTML5
- CSS3
```

```cmd
git add README.md
git commit -m "Add tech stack"
```

**Uppdatera README.md igen:**
```markdown
# Team Portfolio

Portfolio-webbplats för vårt team.

## Tekniker
- HTML5
- CSS3
- Git/GitHub

## Team
- Person A
- Person B
```

```cmd
git add README.md
git commit -m "Add team members

Part of #11"
git push -u origin feature/documentation
```

**Öppna PR**

**Se att PR har 3 commits**

**Person B: Squash commits vid merge**
- Person A approvar
- Person B: Merge pull request → Välj "Squash and merge"
- Commit message: "Add project documentation (closes #11)"
- Confirm squash and merge

**I main branch finns nu bara 1 commit istället för 3**

**Dra #11 till "Done"**

**Båda:**
```cmd
git switch main
git pull
git branch -d feature/documentation
```

---

**Dokumentera workflow i README**

**Person A: Skapa sista issue**
- Titel: "Document Git workflow conventions"
- Create (#12), dra till "In Progress"

**Person A:**
```cmd
git switch main
git pull
git switch -c docs/workflow
```

**Uppdatera `README.md`:**
```markdown
# Team Portfolio

Portfolio-webbplats för vårt team.

## Tekniker
- HTML5
- CSS3
- Git/GitHub

## Team
- Person A
- Person B

## Git Workflow

### Regler
1. Aldrig pusha direkt till main
2. Alltid skapa feature branch
3. Minst 1 approval krävs för merge
4. Använd squash merge för flera commits

### Branch naming
- `feature/` - Nya features
- `fix/` - Buggfixar
- `docs/` - Dokumentation

### Commit messages
- Tydliga och beskrivande
- Imperativ form: "Add", "Fix", "Update"
- Referera till issues: "Closes #X", "Fixes #X", "Relates to #X"

## GitHub Project
Vi använder GitHub Projects för att spåra arbete:
- Issues för alla features
- Kanban board: Todo → In Progress → Done
- Koppla PRs till issues
```

```cmd
git add README.md
git commit -m "Document Git workflow and conventions

Closes #12"
git push -u origin docs/workflow
```

**Öppna PR, Person B approvar och mergar**

**Dra #12 till "Done"**

**Båda:**
```cmd
git switch main
git pull
git branch -d docs/workflow
```

**CHECKPOINT:** Branch protection aktiv, workflow dokumenterad. Del 4 slutförd.

**Slutkontroll av GitHub Projects:**
1. Gå till Projects → Team Portfolio Development
2. Verifiera att alla 12 issues är i "Done"
3. Se övergripande vy av projektets framsteg

---

## Sammanfattning

**Genomfört:**
- Del 1: 3 grundläggande PRs med review-process (issues #1-3)
- Del 2: 3 scenarios med parallellt arbete och konflikter (issues #4-9)
- Del 3: Iterativ code review med 3 omgångar feedback (issue #10)
- Del 4: Branch protection, squash merge, workflow-dokumentation (issues #11-12)

**Färdigheter tränade:**
- Skapa branches och PRs
- Code review (kommentera, request changes, approve)
- Lösa konflikter (GitHub och lokalt)
- Iterativ utveckling baserat på feedback
- Branch protection rules
- Squash commits
- Dokumentera workflow
- **GitHub Issues: Skapa, assigna, länka till PRs**
- **GitHub Projects: Kanban board, spåra progress**

**Verifiering:**
```cmd
git log --oneline --graph
git branch -a
```

**GitHub verifiering:**
- Issues: Alla 12 issues stängda
- Projects: Alla kort i "Done" kolumnen
- PRs: Alla mergade med koppling till issues

Projektet är nu komplett med professionell Git-workflow, issue tracking och project management.
