# Multiple Feature Branches Kaisay Banate Hain — Practical Guide

Jab aik hi project mein 4-5 alag features par kaam ho raha ho (ya 4-5 developers alag alag cheezon par), to har feature ki apni **alag branch** banti hai `main` ya `develop` se. Chaliye step-by-step dekhtay hain.

## 1. Basic Concept

Har feature branch **ek specific kaam** ke liye hoti hai — mix nahi karte:

```
develop (ya main)
   │
   ├── feature/user-login
   ├── feature/payment-gateway
   ├── feature/product-search
   ├── feature/admin-dashboard
   └── feature/email-notifications
```

Har branch **independent** hoti hai — ek branch ka kaam doosri branch ko affect nahi karta jab tak merge na ho.

---

## 2. Step-by-Step: 5 Feature Branches Banana

### Step 1: Pehle base branch par aayein aur update karein
```bash
git checkout develop
git pull origin develop
```

### Step 2: Har feature ke liye naya branch banayein

```bash
# Feature 1
git checkout -b feature/user-login
git push -u origin feature/user-login

# Wapas develop par aayein
git checkout develop

# Feature 2
git checkout -b feature/payment-gateway
git push -u origin feature/payment-gateway

git checkout develop

# Feature 3
git checkout -b feature/product-search
git push -u origin feature/product-search

git checkout develop

# Feature 4
git checkout -b feature/admin-dashboard
git push -u origin feature/admin-dashboard

git checkout develop

# Feature 5
git checkout -b feature/email-notifications
git push -u origin feature/email-notifications
```

> **Note:** `-u origin feature/xyz` sirf pehli baar chahiye hota hai — yeh local branch ko remote ke sath "link" kar deta hai taake future mein sirf `git push` likhna kaafi ho.

---

## 3. Ek Developer Ek Branch Par Kaam Karta Hai

Agar team mein 5 developers hain:

```bash
Developer A → feature/user-login        par kaam karta hai
Developer B → feature/payment-gateway   par kaam karta hai
Developer C → feature/product-search    par kaam karta hai
Developer D → feature/admin-dashboard   par kaam karta hai
Developer E → feature/email-notifications par kaam karta hai
```

Har banda apni branch par `checkout` karta hai:
```bash
git checkout feature/payment-gateway
```

Kaam karta hai, commits karta hai:
```bash
git add .
git commit -m "Add payment gateway integration"
git push
```

---

## 4. Sab Branches Dekhna

```bash
# Local branches dekhna
git branch

# Remote branches bhi dekhna
git branch -a
```

Output kuch aisa dikhega:
```
  develop
* feature/payment-gateway    ← * matlab abhi is par hain
  feature/user-login
  feature/product-search
  feature/admin-dashboard
  feature/email-notifications
  main
```

---

## 5. Branches Ko Updated Rakhna (Zaroori!)

Jab aap apni feature branch par kaam kar rahay hon, aur is dauran doosray developers `develop` mein apna kaam merge kar dein — to aapki branch **purani** ho jati hai. Roz apni branch ko update karein:

```bash
git checkout feature/payment-gateway
git fetch origin
git rebase origin/develop
```

Yeh conflicts ka chance kam kar deta hai jab final merge ka waqt aaye.

---

## 6. Jab Feature Complete Ho Jaye

```bash
git push
```
Phir GitHub par jaa kar **Pull Request** banayein: `feature/payment-gateway` → `develop`

Team review karegi, approve karegi, phir merge hoga.

---

## 7. Kaam Khatam Hone Ke Baad Branch Delete Karna

Merge hone ke baad purani branches ko delete kar dena chahiye (clutter na ho):

```bash
# Local branch delete
git branch -d feature/payment-gateway

# Remote branch delete
git push origin --delete feature/payment-gateway
```

---

## Quick Cheat Sheet

| Command | Kaam |
|---|---|
| `git checkout -b feature/xyz` | Nayi branch banana aur usay switch karna |
| `git push -u origin feature/xyz` | Branch ko remote par pehli baar push karna |
| `git branch` | Sab local branches dekhna |
| `git branch -a` | Local + remote sab branches dekhna |
| `git checkout feature/xyz` | Kisi existing branch par switch karna |
| `git rebase origin/develop` | Apni branch ko latest develop se update karna |
| `git branch -d feature/xyz` | Local branch delete karna |
| `git push origin --delete feature/xyz` | Remote branch delete karna |

---
# DevOps Engineer Kis Branch Mein Kaam Karta Hai?

Yeh ek acha sawal hai kyunke DevOps ka kaam **developers se thora different** hota hai — wo feature code nahi likhta, balkay **infrastructure, pipelines, aur deployment** manage karta hai.

## 1. DevOps Ka Kaam Feature Branches Mein Nahi Hota

Developer:
```
feature/user-login → naya code likhta hai
```

DevOps Engineer:
```
Infrastructure, CI/CD, deployment configs par kaam karta hai
```

DevOps ka focus **application logic** nahi, balkay **application kaisay build, test, aur deploy hoti hai** — yeh hai.

---

## 2. DevOps Ke Liye Alag Branches / Files

DevOps engineer aksar in cheezon par kaam karta hai:

```
main / develop
   │
   ├── feature/user-login          ← Developer A
   ├── feature/payment-gateway     ← Developer B
   ├── infra/setup-ci-pipeline     ← DevOps Engineer
   ├── infra/docker-config         ← DevOps Engineer
   └── infra/kubernetes-deployment ← DevOps Engineer
```

Naming convention aksar `infra/`, `devops/`, ya `chore/` se start hoti hai:

```bash
git checkout -b infra/setup-github-actions
git checkout -b infra/add-dockerfile
git checkout -b devops/update-nginx-config
git checkout -b chore/upgrade-node-version
```

---

## 3. DevOps Kin Files/Folders Par Kaam Karta Hai

| File/Folder | Kaam |
|---|---|
| `.github/workflows/*.yml` | CI/CD pipelines (GitHub Actions) |
| `Dockerfile` | Container image banane ke liye |
| `docker-compose.yml` | Multiple services ko local mein chalane ke liye |
| `k8s/` ya `kubernetes/` | Kubernetes deployment configs |
| `terraform/` | Infrastructure as Code (cloud resources) |
| `.env.example` | Environment variables ka template |
| `nginx.conf` | Server configuration |
| `scripts/deploy.sh` | Deployment automation scripts |

Yani DevOps ka **code repo ke andar hi ek separate area** hota hai jo application logic se nahi, **build/deploy process** se related hota hai.

---

## 4. Real Example — Repo Structure

```
my-project/
│
├── src/                    ← Developers yahan kaam karte hain (app code)
│   ├── components/
│   ├── controllers/
│   └── models/
│
├── .github/workflows/      ← DevOps yahan kaam karta hai (CI/CD)
│   ├── ci.yml
│   └── deploy.yml
│
├── docker/                 ← DevOps yahan kaam karta hai
│   └── Dockerfile
│
├── k8s/                    ← DevOps yahan kaam karta hai
│   └── deployment.yaml
│
└── terraform/               ← DevOps yahan kaam karta hai
    └── main.tf
```

---

## 5. DevOps Ka Workflow Bhi Waisa Hi Hai (Branch → PR → Merge)

DevOps bhi normal developer jaisa hi process follow karta hai:

```bash
git checkout develop
git pull origin develop

git checkout -b infra/add-ci-pipeline

# .github/workflows/ci.yml banata hai ya edit karta hai

git add .
git commit -m "Add CI pipeline for automated testing"
git push -u origin infra/add-ci-pipeline
```

Phir PR banata hai: `infra/add-ci-pipeline → develop`

Team review karti hai (kyunke CI/CD sab ke kaam ko affect karta hai), approve hoti hai, merge hoti hai.

---

## 6. Farq — Developer vs DevOps Branch Focus

| | Developer | DevOps Engineer |
|---|---|---|
| **Branch naam** | `feature/`, `bugfix/` | `infra/`, `devops/`, `chore/` |
| **Kaam karta hai** | Application code (frontend/backend logic) | Pipelines, deployment, infra config |
| **Files touch karta hai** | `src/`, `components/`, `controllers/` | `.github/workflows/`, `Dockerfile`, `k8s/` |
| **Merge karta hai kis mein** | `develop` | `develop` ya seedha `main` (deployment configs ke liye) |
| **Extra responsibility** | Feature complete karna | Branch protection rules, environments (staging/prod) setup karna |

---

## 7. Bonus: DevOps Special Branches Bhi Manage Karta Hai

Yeh **deployment-related branches** bhi aksar DevOps hi control karta hai:

```
staging     ← QA/testing environment ke liye deploy hota hai
production  ← Live environment (main ka mirror)
```

Jab code `main` mein merge hota hai, DevOps ka CI/CD pipeline automatically usay `staging` ya `production` par deploy kar deta hai — yeh sab DevOps hi setup karta hai.

---
# DevOps Engineer ka Practical Workflow — `infra/setup-ci-pipeline` Branch Par

Chaliye bilkul step-by-step, shuru se dekhtay hain — kaisay DevOps Engineer branch banata hai, kaam karta hai, authenticate hota hai, aur pull/push karta hai.

## 1. Sabse Pehle — Repo Clone Karna (Authentication Step 1)

Agar DevOps Engineer **naya** hai ya nayi machine par kaam kar raha hai, sabse pehle usay GitHub se authenticate hona parhta hai.

### Do Tareeqay Hain Authentication Ke:

**A) SSH Key (Recommended, Professional)**
```bash
# SSH key generate karna (agar pehle se nahi hai)
ssh-keygen -t ed25519 -C "devops@company.com"

# Public key copy karna
cat ~/.ssh/id_ed25519.pub
```
Yeh public key GitHub par add ki jati hai: **Settings → SSH and GPG Keys → New SSH Key**

Acha sawal hai — confusion clear kar deta hoon. Yahan concept samajhna zaroori hai:

## Key Kis Machine Par Generate Hogi?

**Jis server/machine se aap Git push/pull karenge, wahi SSH key generate hogi.** Matlab:

- Agar aap apne **local laptop/PC** se GitHub use kar rahe hain → wahan key banegi
- Agar aap kisi **remote/production server** (jahan CI/CD deploy hota hai) se GitHub access karna chahte hain → **usi server** par login karke key generate karni hogi

```bash
# Us machine par jo GitHub se connect hogi:
ssh-keygen -t ed25519 -C "devops@company.com"
```

Ye command chalane se do files banti hain:

| File | Kya Hai | Kahan Rehti Hai |
|---|---|---|
| `~/.ssh/id_ed25519` | **Private Key** | Sirf isi machine par, kabhi kahin share nahi hoti |
| `~/.ssh/id_ed25519.pub` | **Public Key** | Ye GitHub par add hoti hai |

## Private Key Kahan Add Hoti Hai?

**Private key kahin bhi "add" nahi hoti — wo apni jagah (us server ke `~/.ssh/` folder) mein hi rehti hai.** Ye kabhi GitHub ya kisi aur jagah upload/paste nahi ki jati.

## Public Key Kahan Add Hoti Hai?

Ye GitHub par add hoti hai:

1. `cat ~/.ssh/id_ed25519.pub` se copy karo
2. GitHub → Settings → SSH and GPG Keys → New SSH Key → Paste karo

## Real DevOps Scenario Mein

Agar aapka **CI/CD server** (jaise Jenkins, GitLab Runner, ya production server) GitHub se code pull karega:

- Us server par hi `ssh-keygen` chalega
- Us server ki **public key** GitHub repo mein **Deploy Key** ke tor par add hogi (Settings → Deploy Keys — ye individual account key se better hai kyunki isay specific repo tak limit kar sakte hain)
- Private key hamesha usi server par secure rahegi, file permission `600` ke sath:

```bash
chmod 600 ~/.ssh/id_ed25519
```

**Golden Rule:** Public key share hoti hai, private key kabhi nahi. Har machine/server ki apni alag key pair honi chahiye.

Kya aap ye setup **apne local machine** ke liye kar rahe hain ya **CI/CD server (jaise Jenkins/GitHub Actions runner)** ke liye? Us hisaab se main next steps detail mein bata sakta hoon.

Phir clone karte waqt SSH URL use hota hai:
```bash
git clone git@github.com:company/my-project.git
```
Achha sawal! Ye URL kahin se "milta" nahi automatically — aapko khud GitHub se copy karna parhta hai. Chaliye poora process dekhte hain:

## SSH URL Kaha Se Milta Hai?

1. GitHub par apne repository (`company/my-project`) par jayein
2. Green button **"Code"** par click karein (repo page ke upar right side)
3. Ek dropdown khulega jismein 3 options honge: **HTTPS**, **SSH**, **GitHub CLI**
4. **SSH** tab select karein
5. Wahan se URL copy karein — kuch aisa dikhega:
   ```
   git@github.com:company/my-project.git
   ```

Ye URL repo-specific hota hai — har repository ka apna alag URL hota hai (organization/username + repo name ke hisaab se).

## Ab Clone Kaise Karein

Terminal khol kar jis folder mein aap project rakhna chahte hain wahan jayein, phir:

```bash
cd ~/projects
git clone git@github.com:company/my-project.git
```

Agar SSH key sahi se GitHub par add hai (jo aapne pehle step mein ki thi), to ye direct clone kar dega — koi password nahi maangega.

## Verify Karna (Agar Pehli Baar SSH Use Kar Rahe Hain)

Clone se pehle test kar sakte hain ke SSH connection GitHub se ho raha hai ya nahi:

```bash
ssh -T git@github.com
```

Agar sahi se setup hai to ye message aayega:
```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

## Clone Ke Baad — Branch Banana

Clone hone ke baad, folder mein jayein aur naya branch banayein:

```bash
cd my-project
git checkout -b infra/setup-ci-pipeline
```

---

Bilkul, chaliye clone step ko complete karte hain aur aage ka workflow bhi dekh lete hain:

## Clone Karna (SSH Key Setup Ke Baad)

### Step 1: Pehle Test Karein SSH Connection Kaam Kar Raha Hai

Clone karne se pehle ye verify kar lein ke GitHub aapki SSH key pehchan raha hai:

```bash
ssh -T git@github.com
```

Agar sahi se connect hua to ye message aayega:
```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

### Step 2: Repo Clone Karna

```bash
git clone git@github.com:company/my-project.git
```

Ye command:
- Repo ko current directory mein download kar degi
- `my-project` naam ka folder ban jayega
- Aap us folder mein `cd my-project` karke ja sakte hain

### Step 3: Naye Branch Par Kaam Shuru Karna

Ab jo aapne mention kiya `infra/setup-ci-pipeline` branch, wo yahan create hoga:

```bash
cd my-project
git checkout -b infra/setup-ci-pipeline
```

Ya newer Git syntax:
```bash
git switch -c infra/setup-ci-pipeline
```

### Step 4: Kaam Karna Aur Changes Add Karna

Files edit karne ke baad:
```bash
git add .
git commit -m "Add CI pipeline configuration"
```

### Step 5: Branch Ko Push Karna

Pehli baar push karte waqt upstream set karna padta hai:
```bash
git push -u origin infra/setup-ci-pipeline
```

Uske baad sirf `git push` kaafi hoga.

### Step 6: Pull Karna (Latest Changes Lena)

Agar team ke doosre members ne changes kiye hain aur aap unhe apne branch mein lana chahte hain:
```bash
git pull origin main
```

Ya rebase ke sath (cleaner history ke liye):
```bash
git pull --rebase origin main
```

---

Chahen to main next step bata sakta hoon — jaise **Pull Request kaise banate hain GitHub par**, ya **CI pipeline (GitHub Actions) ka basic YAML file** kaisa hota hai `infra/setup-ci-pipeline` ke context mein. Kya aap wo dekhna chahenge?

**B) HTTPS + Personal Access Token (PAT)**
```bash
git clone https://github.com/company/my-project.git
```
Jab push/pull karega, username aur password ki jaga **token** maangega:
```
Username: devops-username
Password: ghp_xxxxxxxxxxxxxxxxxxxx   ← yeh PAT hai, normal password nahi
```
Token banta hai: **GitHub Settings → Developer Settings → Personal Access Tokens**

> **Company mein aksar SSH key use hoti hai** kyunke baar baar token daalna nahi parhta, aur zyada secure hai.

---

## 2. Repo Access Milna (Permission Zaroori Hai)

Sirf authenticate hona kaafi nahi — DevOps Engineer ko us **repository ka collaborator/member** bhi hona zaroori hai:

- Repo Owner usay **Settings → Collaborators and Teams** mein add karta hai
- Ya wo **Organization** ka member hota hai jisay `write` ya `admin` access diya gaya hota hai

```
Roles example:
- Read     → sirf dekh sakta hai
- Write    → push/pull kar sakta hai, branch bana sakta hai
- Admin    → settings bhi change kar sakta hai (branch protection, secrets)
```

DevOps Engineer ko aksar **Admin ya Maintainer** access diya jata hai kyunke usay CI/CD settings aur secrets bhi manage karne hotay hain.

---

## 3. Ab Clone Karna Aur Branch Par Kaam Karna

```bash
# Step 1: Repo clone karna (agar pehli baar hai)
git clone git@github.com:company/my-project.git
cd my-project

# Step 2: Latest develop branch le kar aana
git checkout develop
git pull origin develop

# Step 3: Naya branch banana apne kaam ke liye
git checkout -b infra/setup-ci-pipeline
```

---

## 4. Ab Actual Kaam Karna — CI/CD File Banana

DevOps Engineer is branch par folder/file banata hai:

```bash
mkdir -p .github/workflows
```

Phir file create karta hai:
```bash
touch .github/workflows/ci.yml
```

Aur usmein pipeline likhta hai (jaisa humne pehle dekha tha).

---

## 5. Commit Aur Push Karna

```bash
git add .github/workflows/ci.yml
git commit -m "Add CI pipeline for automated testing"
git push -u origin infra/setup-ci-pipeline
```

Yahan push karte waqt **authentication check hoti hai**:
- Agar SSH set hai → automatically authenticate ho jata hai (koi password nahi maangta)
- Agar HTTPS + Token hai → pehli baar token maangega, phir Git usay cache/store kar leta hai

---

## 6. Doosra Sawal: "Kya `infra/setup-ci-pipeline` Ko Pull Karengay?"

Yeh depend karta hai **kaun, kab, aur kyun** pull kar raha hai:

### Scenario A: Aap khud doosri machine par kaam continue karna chahtay hain
```bash
git fetch origin
git checkout infra/setup-ci-pipeline
git pull origin infra/setup-ci-pipeline
```

### Scenario B: Doosra team member is branch ko review/test karna chahta hai
```bash
git fetch origin
git checkout infra/setup-ci-pipeline
```
(Agar local mein branch nahi hai to `checkout` khud fetch kar ke bana deta hai)

### Scenario C: PR ban chuki hai, koi review kar raha hai
Wo GitHub par PR ke "Files Changed" tab mein dekh sakta hai bina pull kiye bhi — ya locally test karne ke liye pull kar sakta hai.

---

## 7. Ab CI/CD Secrets Ka Authentication — Yeh Alag Cheez Hai!

Yeh **bohat important point** hai — **do alag authentication** hoti hain:

| Type | Kis Liye | Kaisay Set Hoti Hai |
|---|---|---|
| **Git Authentication** | Code push/pull karne ke liye | SSH key / Personal Access Token |
| **CI/CD Authentication** | Pipeline ko server/cloud par deploy karne ke liye | GitHub Secrets |

Jab pipeline chalti hai aur **server par deploy** karti hai, tab GitHub Actions ko bhi authenticate hona parhta hai us server/service se. Yeh **GitHub Secrets** mein store hota hai:

```
Settings → Secrets and variables → Actions → New repository secret
```

Example secrets:
```
SERVER_SSH_KEY       ← Production server ka private key
SERVER_HOST          ← Server ka IP address
DOCKER_USERNAME       
DOCKER_PASSWORD
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
```

Pipeline file mein aisay use hota hai (kabhi bhi plain text mein nahi likha jata):
```yaml
- name: Deploy to Server
  run: |
    echo "${{ secrets.SERVER_SSH_KEY }}" > private_key.pem
    chmod 600 private_key.pem
    ssh -i private_key.pem user@${{ secrets.SERVER_HOST }} "docker restart myapp"
```

---

## 8. Poora Flow — Ek Nazar Mein

```
1. DevOps Engineer authenticate hota hai GitHub par (SSH/PAT)
        ↓
2. Repo clone karta hai (agar access hai)
        ↓
3. develop se naya branch: infra/setup-ci-pipeline
        ↓
4. CI/CD file likhta hai (.github/workflows/ci.yml)
        ↓
5. Secrets add karta hai GitHub Settings mein
   (server credentials, docker login, etc.)
        ↓
6. Commit + Push karta hai apni branch par
        ↓
7. PR banata hai: infra/setup-ci-pipeline → develop
        ↓
8. Team review karti hai (kyunke CI sab ko affect karti hai)
        ↓
9. Approve + Merge
        ↓
10. Ab CI/CD pipeline live ho gayi — 
    har future PR par yeh automatically chalegi
```

---

## Zaroori Baat: Secrets Kabhi Code Mein Nahi Hoti

```yaml
# ❌ GALAT - kabhi na karein
run: ssh -i "mypassword123" user@server

# ✅ SAHI - Secrets ka use
run: ssh -i "${{ secrets.SERVER_SSH_KEY }}" user@server
```

Agar password ya key seedha code mein likh di jaye aur push kar di jaye, to wo **GitHub history mein hamesha ke liye reh jati hai** — chahay baad mein delete bhi kar dein. Is liye **GitHub Secrets** ya **environment variables** hi use karni chahiye.

---

Chahen to main aapko dikha doon ke **GitHub Secrets add karna** actual mein kaisa dikhta hai (step by step screenshots ki tarah explain kar doon), ya phir **SSH key setup** ka poora process detail mein bata doon?

# GitHub Actions CI/CD Pipeline — Practical Example

Chaliye ek real-world jaisa CI/CD pipeline banate hain jo DevOps Engineer actual mein likhta hai. Main aapko **step-by-step** samjhaunga ke har line kya kar rahi hai.

## File Location
```
.github/workflows/ci-cd.yml
```

## Complete Pipeline (Node.js Project Example)

```yaml
name: CI/CD Pipeline

# Yeh pipeline kab chalegi
on:
  pull_request:
    branches: [develop, main]
  push:
    branches: [main]

jobs:
  # ---------- JOB 1: Testing ----------
  test:
    name: Run Tests
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install dependencies
        run: npm install

      - name: Run Linter
        run: npm run lint

      - name: Run Tests
        run: npm test

      - name: Build project
        run: npm run build

  # ---------- JOB 2: Deploy to Staging ----------
  deploy-staging:
    name: Deploy to Staging
    needs: test
    if: github.ref == 'refs/heads/develop'
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Deploy to Staging Server
        run: |
          echo "Deploying to staging environment..."
          # yahan actual deployment commands aayenge
          # e.g., rsync, ssh, docker push, etc.

  # ---------- JOB 3: Deploy to Production ----------
  deploy-production:
    name: Deploy to Production
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Deploy to Production Server
        run: |
          echo "Deploying to production environment..."
          # actual production deployment commands
```

---

## Har Section Ka Matlab

### 1. `on:` — Trigger Kab Chalega
```yaml
on:
  pull_request:
    branches: [develop, main]
  push:
    branches: [main]
```
- Jab koi **PR banti hai** `develop` ya `main` ki taraf → tests chalte hain
- Jab koi **push hoti hai** seedha `main` par → tests + deployment chalta hai

### 2. `jobs: test` — Quality Check
Yeh ensure karta hai ke:
- Code lint rules follow karta hai
- Sab tests pass ho rahay hain
- Project successfully build ho raha hai

**Agar yeh fail ho jaye → PR merge nahi ho sakti** (agar aapne "Require status checks" enable ki hui hai branch protection mein)

### 3. `needs: test` — Dependency
```yaml
deploy-staging:
  needs: test
```
Iska matlab: **deploy tabhi hoga jab `test` job pass ho jaye**. Agar tests fail hon, deployment bilkul nahi chalega.

### 4. `if: github.ref == 'refs/heads/develop'` — Conditional Deployment
Yeh check karta hai ke code kaunsi branch se aa raha hai:
- `develop` se aaye → **staging** par deploy ho
- `main` se aaye → **production** par deploy ho

---

## Yeh Poora Cycle Kaisay Kaam Karta Hai

```
Developer → feature/payment-gateway par kaam karta hai
                    │
                    ▼
         PR banata hai → develop
                    │
                    ▼
    ✅ CI Pipeline chalti hai (test job)
    - Lint check
    - Tests
    - Build
                    │
                    ▼
         Team review + approve
                    │
                    ▼
            Merge → develop
                    │
                    ▼
    🚀 Automatically Staging par deploy
    (QA team yahan test karti hai)
                    │
                    ▼
      Sab theek → PR banti hai develop → main
                    │
                    ▼
            Merge → main
                    │
                    ▼
    🚀 Automatically Production par deploy
    (Live ho gaya users ke liye)
```

---

## Branch Protection Ke Sath Connection (Aapke Pehle Screenshot Se)

Yaad hai aapne "Require approvals" wali setting dekhi thi? Iske sath ek aur important option hota hai:

> ☑ **Require status checks to pass before merging**

Isko enable karne se aap select kar saktay hain ke **`test` job pass hona zaroori hai** merge se pehle. Yani:

```
PR ban gayi → CI chal rahi hai → Merge button DISABLED (grey)
CI pass ho gayi → Merge button ENABLED (green)
CI fail ho gayi → Merge button DISABLED (red ❌)
```

Is se koi bhi **broken code** `main` ya `develop` mein nahi ja sakta, chahay approval mil bhi jaye.

---

## Docker Ke Sath Example (Zyada Real-World)

Agar deployment Docker use kar rahi ho:

```yaml
  deploy-production:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Build Docker Image
        run: docker build -t myapp:latest .

      - name: Login to Docker Hub
        run: echo "${{ secrets.DOCKER_PASSWORD }}" | docker login -u "${{ secrets.DOCKER_USERNAME }}" --password-stdin

      - name: Push Docker Image
        run: docker push myapp:latest

      - name: Deploy to Server (SSH)
        run: |
          ssh user@server "docker pull myapp:latest && docker restart myapp"
```

> **`secrets.DOCKER_PASSWORD`** — Yeh GitHub repo settings mein "Secrets" section mein store hota hai (Settings → Secrets and variables → Actions), taake password code mein visible na ho.

---

Agar aap batayein ke aapka project **kis language/framework** mein hai (Node.js, Python, PHP, Laravel, etc.) aur **kahan deploy** karna hai (VPS, AWS, Vercel, etc.), to main aapko **exact working pipeline** bana kar de sakta hoon.
