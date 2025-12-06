# 🔐 Password Strength Checker with Optimization  
A rule-based system to evaluate and automatically improve weak passwords with minimal edits.

---

## 📘 Overview

Weak, predictable passwords are a major cause of security breaches.  
Users often choose passwords that are easy to remember but vulnerable to brute force, dictionary attacks, and pattern-based guessing.

This project provides:

- Accurate password strength evaluation  
- Automatic optimization of weak passwords  
- Minimal and meaningful edits  
- High-entropy, strong suggestions (score ≥ 70)  
- 100% rule-based — no ML required  
- Fast and deterministic output  
- Full frontend + backend implementation  

The method is fully based on the mathematical model and optimization algorithm described in the project document.  
:contentReference[oaicite:0]{index=0}

---

# 🧩 Problem Statement

Users commonly create weak passwords due to convenience and memorability.  
Even when strength checks exist, they rarely provide *actionable, minimal-change suggestions*.

The goal is to design a system that:

1. *Accurately evaluates password strength*  
2. *Automatically generates improved versions of weak passwords*  
3. *Makes as few edits as possible (minimal edit distance)*  
4. *Ensures the improved password meets the strength threshold*  
5. *Avoids ML or brute-force search*  

The optimization is defined as:

\[
\text{Minimize } D(s, s') \quad \text{subject to} \quad Str(s') \ge 70
\]

Where *D(s, s′)* is the edit distance.  


---

# 🚀 Approach Used

This project uses a *Greedy Rule-Based Optimization Algorithm*.  
It sequentially applies the most impactful rule to reach the required strength with minimal edits.  
(Algorithm: Slides 14–16) :contentReference[oaicite:2]{index=2}

---

## 1️⃣ Strength Evaluation Model

Password strength is calculated as:

\[
Str(s') = w_L\,flen(L) + w_c\,f_{classes}(s') + w_e\,Entropy(s') - w_p\,f_{penalty}(s')
\]

### Components (as per Slides 5–12):

✔ *Length Score (flen)*  
✔ *Character Class Score*  
✔ *Entropy Calculation (Shannon)*  
✔ *Common-Word Penalty*  
✔ *Weighted scoring system*  

A password is considered strong if:

\[
Str(s') \ge 70
\]



---

## 2️⃣ Greedy Optimization Steps

The optimizer applies the following minimal-edit rules *in priority order*:

### ✔ Step 1 — If already strong, stop  
(Slide 14)

### ✔ Step 2 — Ensure minimum length  
If length < 8 → append safe characters (Xy9!).  
(Slide 15)

### ✔ Step 3 — Add missing character classes  
Add exactly one character from each missing class:  
- lowercase → a  
- uppercase → A  
- digit → 7  
- special → !  


### ✔ Step 4 — Remove dictionary/common words  
Transform weak patterns:  
- password → P@ssw0rd  
- 1234 → 1#3$  
- qwerty → Qw3rTy  


### ✔ Step 5 — Increase entropy  
Replace repeated characters with random strong characters.  


### ✔ Step 6 — Recompute score  
Stop when score ≥ 70.  

### ✔ Step 7 — Generate final suggestions  
Produces 2–3 strong optimized variations.  
Example (Slides 16):  
- P@ssw0rd123!  
- Passw0rd#Xy  
- P9G5Xy12025

---




# Password Strength Optimizer — Run locally

This project contains a small backend API and a frontend SPA. The backend now serves the frontend static files, so you can run everything from one process.

Prerequisites
- Node.js (16+ recommended) and npm installed

Quick start (recommended)
1. From the project root run:

powershell
npm run install-all
npm start


2. Open http://localhost:3000 in your browser. The frontend and API are served on the same origin.

Alternative — run backend directly
1. Install dependencies and start backend:

powershell
cd backend
npm install
npm start


Advanced — run frontend separately (not required)
If you'd rather run the frontend using a static server (for example during development):

powershell
cd frontend
npx serve .


Notes
- The backend start command runs node server.js (see backend/package.json). The server listens on port 3000 and serves both API endpoints (under /api) and the frontend files.
- Frontend fetches the API via a relative path (/api/checkPassword) so both running on the same origin works correctly.
"# Password-Strength-Optimiser" 
"# Password-Strength-Optimiser"
