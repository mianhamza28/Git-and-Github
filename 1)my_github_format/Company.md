# Practical Hands-On Example — 3 Feature Branches Bana Kar Merge Karna

Chaliye ek **chota sample project** banate hain aur step-by-step practice karte hain. Aap yeh commands apne terminal mein khud chala sakte hain.

## Step 0: Sample Project Setup

```bash
mkdir my-demo-project
cd my-demo-project
git init
```

Ek basic file banayein aur pehla commit karein:

```bash
echo "# My Demo Project" > README.md
git add README.md
git commit -m "Initial commit"
```

Ab `main` branch ko rename karke `develop` bhi bana lete hain (real projects mein aisay hi hota hai):

```bash
git branch develop
git checkout develop
```

---

## Step 1: Feature 1 — Login Page

```bash
git checkout -b feature/login-page
```

Ek nayi file banayein (jaisay yeh koi real feature ho):

```bash
echo "function login() { console.log('User logged in'); }" > login.js
git add login.js
git commit -m "Add login functionality"
```

---

## Step 2: Wapas Develop Par Aayein, Feature 2 Banayein

```bash
git checkout develop
git checkout -b feature/search-bar
```

```bash
echo "function search(query) { console.log('Searching: ' + query); }" > search.js
git add search.js
git commit -m "Add search bar functionality"
```

---

## Step 3: Wapas Develop, Feature 3 Banayein

```bash
git checkout develop
git checkout -b feature/dark-mode
```

```bash
echo "function toggleDarkMode() { console.log('Dark mode toggled'); }" > darkmode.js
git add darkmode.js
git commit -m "Add dark mode toggle"
```

---

## Step 4: Check Karein — Ab Kitni Branches Hain?

```bash
git branch
```

Output:
```
  develop
* feature/dark-mode
  feature/login-page
  feature/search-bar
  main
```

Har branch **alag file** rakhti hai — kyunke har feature independent hai. Agar aap `feature/login-page` par jayein, wahan sirf `login.js` hoga, `search.js` ya `darkmode.js` nahi honge (jab tak merge na ho).

Check kar ke dekhein:
```bash
git checkout feature/login-page
ls
```

---

## Step 5: Features Ko Develop Mein Merge Karna

Ab jaisay real world mein PR approve hone ke baad merge hoti hai, waisay hi hum local mein merge karte hain:

```bash
git checkout develop

# Feature 1 merge
git merge feature/login-page

# Feature 2 merge
git merge feature/search-bar

# Feature 3 merge
git merge feature/dark-mode
```

Ab check karein `develop` branch mein sab files aa gayi:
```bash
ls
```

Output:
```
README.md  login.js  search.js  darkmode.js
```

Sab teeno features ab `develop` mein integrate ho chuki hain! ✅

---

## Step 6: History Dekhein (Visual)

```bash
git log --oneline --graph --all
```

Yeh aapko branches ka **visual tree** dikhayega — kaisay har feature branch out hui aur wapas merge hui.

---

## Step 7: Purani Branches Clean Karna

Kaam complete ho gaya, ab feature branches delete kar dein:

```bash
git branch -d feature/login-page
git branch -d feature/search-bar
git branch -d feature/dark-mode
```

---

## Bonus: Conflict Practice (Real Scenario)

Agar aap **conflict resolve karna** bhi practice karna chahen — do branches mein **same file ki same line** change kar ke merge try karein, to Git conflict dikhayega aur aapko manually resolve karna hoga. Bataiye agar yeh bhi try karna hai, main uska bhi step-by-step example bana deta hoon.

---

### Ab Aap Khud Try Karein:
1. Upar diye commands copy-paste karein terminal mein
2. Har step ke baad `git status` aur `git log --oneline` chala kar dekhein kya ho raha hai
3. Agar koi error ya confusion aaye, mujhe batayein — main us specific step ko explain kar dunga

Kya aap **conflict resolve karne ka practical example** bhi dekhna chahenge?
