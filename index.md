---
title: "Kevin C. Owens — Tech, Code, AI"
author: "Kevin C. Owens"
---

<!-- Navigation Bar -->
<nav>
  <div class="nav-left">
    <a href="/">kevincowens.dev</a>
  </div>

  <div class="nav-links">
    <a href="/about/">About</a>
    <a href="/blog/">Blog</a>

    <div class="dropdown">
      <span class="dropdown-btn">GitHub Repos ▾</span>
      <div class="dropdown-content" id="repo-dropdown">
        <!-- Filled dynamically by JS -->
      </div>
    </div>
  </div>
</nav>

<main>

# 👋 Hi, I'm Kevin  
**Cybersecurity | AI | Homelab Automation | USAF First Sergeant**

I build secure, modular systems that blend **AI**, **automation**, and **real‑world operations**.  
This site documents my learning path, projects, and experiments as I transition from a 20‑year Air Force career into full‑time tech.

---

## 🚀 What I'm Working On

### **AI‑Driven Homelab Architecture**
Building a modular, agentic system using:
- Ollama  
- n8n  
- GitHub Copilot  
- Local LLM workflows  
- Zero‑Trust principles  

### **Cybersecurity Technology Degree**
Currently completing my **B.S. in Cybersecurity Technology**, focusing on:
- Network defense  
- Secure automation  
- AI‑augmented SOC workflows  

### **Learning Path**
My current focus areas:
- Multi‑agent architectures  
- Local inference optimization  
- Secure container orchestration  
- Markdown‑first documentation workflows  

---

## 📝 Latest Blog Posts
A few highlights from my writing:

- **[Building My Local AI Workflow](/blog/posts/)**  
- **[Zero‑Trust for the Homelab](/blog/posts/)**  
- **[Deploying n8n with Secure Boundaries](/blog/posts/)**  

*(This list will auto‑populate once you add posts.)*

---

## 🧰 Featured Projects

### **Agentic Homelab**
A fully modular, self‑maintaining system with strict tool boundaries and local LLMs.

### **System Status Page**
A Markdown‑driven, always‑current snapshot of my homelab for agent handoff.

### **AI‑Enhanced Research Workflow**
A multi‑agent pipeline that gathers sources, summarizes them, and formats them into Markdown with citations.

---

## 📫 Connect With Me
- **GitHub:** [github.com/kevincowens](https://github.com/kevincowens)  
- **LinkedIn:** *(add link when ready)*  
- **Email:** *(optional)*  

</main>

<footer>
  © 2026 Kevin C. Owens — Built with Markdown + Monokai Pro
</footer>

<!-- GitHub Repo Dropdown Script -->
<script>
fetch("https://api.github.com/users/kevincowens/repos")
  .then(res => res.json())
  .then(data => {
    const menu = document.getElementById("repo-dropdown");
    data
      .sort((a, b) => a.name.localeCompare(b.name))
      .forEach(repo => {
        const item = document.createElement("a");
        item.href = repo.html_url;
        item.textContent = repo.name;
        menu.appendChild(item);
      });
  });
</script>
