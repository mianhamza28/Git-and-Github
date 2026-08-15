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

Chahen to main aapko ek **actual GitHub Actions CI/CD pipeline file** bana kar dikha doon (`.github/workflows/deploy.yml`) taake aap dekh saken DevOps engineer asal mein kya likhta hai?

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
