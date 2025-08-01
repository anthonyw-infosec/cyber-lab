## 🔓 TryHackMe: Offensive Security Intro  
*First hands-on hacking lab — completed August 3, 2025*

---

### 🧠 Goal  
Hack your first website in a legal, controlled lab. Learn what ethical hackers do and how websites are tested for vulnerabilities.

---

### 🖥️ Tools Used

- TryHackMe’s in-browser terminal and target website

---

### 🔍 Attack Process

| Step | Action                          | Purpose                                |
|------|----------------------------------|----------------------------------------|
| 1    | Used `dirb` to find hidden URLs  | Discovered a hidden bank deposit page  |
| 2    | Navigated to the deposit URL     | Attempted to add funds to bank account |
| 3    | Submitted request with payload   | Illegally added funds (simulated)      |
| 4    | Challenge completed              | Demonstrated web exploitation process  |

---

### 🧠 In My Own Words

I learned that websites can contain hidden URLs not meant for public access. If discovered, these endpoints can be abused by attackers to manipulate systems — like adding funds to an account. This exercise showed me how **insufficient access controls** and **unprotected routes** can lead to major security risks.

---

### 🧩 Key Terms Learned

- **Ethical Hacking**: Legally testing systems for vulnerabilities  
- **dirb**: A tool used to brute-force hidden directories and pages on websites

---

### ✅ Next Steps

- Keep practicing with TryHackMe
- Document the next lesson "Defensive Security Intro"

---

*Last updated: August 1, 2025*
