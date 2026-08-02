---
title: "Toward Building a Secure Competitive Learning Platform for Computer Science Technical Challenges"
publishedAt: "2023-06"
summary: "My final bachelor's thesis in Software Engineering — a full-stack web platform designed for cybersecurity training and technical evaluations. Inspired by TryHackMe and HackerRank, it empowers instructors to create gamified CTF-style challenges, recruiters to evaluate candidates, and learners to build skills through real-world scenarios."
images:
  - "https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/Competitive-Learning-Platform/frontend/vue-home.png"
  - "https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/Competitive-Learning-Platform/django/main.png"
  - "https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/Competitive-Learning-Platform/uml/use-case.png"
  - "https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/Competitive-Learning-Platform/frontend/vue-challenge.png"

team:
  - name: "Guebli Ayoub Abdessami"
    role: "AI Engineer"
    avatar: "/images/avatar.jpeg"
    linkedIn: "https://www.linkedin.com/in/ayoub-guebli-0615342b8"
  - name: "Touati Mohamed Amine"
    role: "Frontend Developer"
  - name: "Aymen Ababsa"
    role: "Backend Developer"

supervisors:
  - name: "Dr. Naila Marir"
    role: "Supervisor"

institution: "Abdelhamid Mehri – Constantine 2 University, Faculty of NTIC"
---

# Toward Building a Secure Competitive Learning Platform for Computer Science Technical Challenges

This was our **final-year graduation project** for a Bachelor’s degree in **Software Engineering**. The system is a secure, gamified learning and recruitment platform tailored for **cybersecurity and technical training**. It supports **challenge creation**, **real-time competitions**, **job offers**, and **performance analytics** — all within a clean, scalable architecture built using **Django** and **Vue.js**.

[Backend repo for presentation](https://github.com/ab2gbl/atelier_django)

[Frontend repo for presentation](https://github.com/ab2gbl/atelier)

### 👥 Team

| Member | Role | Link |
| --- | --- | --- |
| Guebli Ayoub Abdessami | Web Full Stack Developer | [LinkedIn Profile](https://www.linkedin.com/in/ayoub-guebli-0615342b8) |
| Aymen Ababsa | Backend & mobile Developer | [LinkedIn Profile](https://www.linkedin.com/in/aymen-ababsa-50b19722a/) |
| Touati Mohamed Amine | Frontend Developer | [LinkedIn Profile](https://www.linkedin.com/in/mohamed-amine-touati-725980295) |
| Dr. Naila Marir | Supervisor | [LinkedIn Profile](https://www.linkedin.com/in/naila-marir-7250139b/) |

[Download / preview the PDF](https://drive.google.com/file/d/1LF2UDy5oP1A6PBH0PK_0lOOEr1NHzZrS/preview)

---

## 🎯 Project Goals

- Help learners develop **real-world cybersecurity skills**
- Allow instructors to create **custom CTF-style challenges**
- Give recruiters tools to **evaluate developer talent**
- Provide a secure, **gamified, multi-role** learning experience

---

## 🔍 Key Features

- 👤 **Multi-user roles**: Developer, Instructor, Recruiter, Admin, Analyst
- 🧩 **Challenge creation**: Step-by-step tasks, multimedia content, hints, scoring
- 📅 **Scheduling (Planify)**: Admins assign start/end dates to challenges
- 💼 **Job posting & interviews**: Recruiters assign challenges and notify winners
- 🛡️ **Secure login**: 2FA, role-based access, account lockout protection
- 🏅 **Gamification**: Badges, leaderboards, XP, ranks, progress tracking
- 📊 **Analytics dashboard**: Progress insights for users and analysts

---

## 🧠 System Architecture

| Layer | Technology |
| --- | --- |
| Frontend | Vue.js |
| Backend | Django, Django REST |
| Database | MySQL |
| Mobile Support | Flutter (prototype) |
| Auth & Security | JWT, 2FA, OWASP ESAPI |
| UI Frameworks | Bootstrap, Vuetify |
| Dev Tools | VS Code, Postman, GitHub |
| Diagramming | [draw.io](http://draw.io), Figma |

Pattern: **MVVM architecture** with separation of logic and UI.

![use-case](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/uml/use-case.png)

---

## 🧱 Domain Model & Class Overview

The system models include:

- `User` (abstract base) → `Developer`, `Admin`, `Recruiter`, `Instructor`, `Analyst`
- `Challenge`, `Task`, `GamifiedCourse`, `Path`, `ParticipationResult`
- `Job`, `Interview`, `Statics`, `Badge`, `Recommendation`
- Support for `solo` and `team` registrations

![classes-diagram](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/uml/classes-diagram.png)

---

## 🕹️ Gamification System

- Earn badges by completing challenges and paths
- Points system for solving challenges, bonus for top performers
- Leaderboards per domain and skill track
- Custom paths like “Linux Fundamentals” with interactive learning tasks
- Rich content: text, images, videos, downloadable challenge files

---

## 📲 UI Snapshots

- Login interface with 2FA

![login pic](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/Competitive-Learning-Platform/login.png)

- Instructor challenge builder

![create1 pic](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/Competitive-Learning-Platform/create1.png)

![create2 pic](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/Competitive-Learning-Platform/create2.png)

![create3 pic](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/Competitive-Learning-Platform/create3.png)

![create4 pic](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/Competitive-Learning-Platform/create4.png)

- Admin challenge scheduling

![calendar pic](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/Competitive-Learning-Platform/calendar.png)

- Challenge participation interface

![vue-challenge pic](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/frontend/vue-challenge.png)

---

## 🛠️ Implementation Notes

- Implemented login/signup with JWT and 2FA
- Developed challenge scenario builder with task editor
- Challenge files (.rar) used to simulate real-world pentesting setups
- Support for multi-platform development (web, mobile via Flutter)

---

## 🧪 Sample Challenge Scenario

> Linux Fundamentals — Privilege Escalation

- Title: `shellFlag`
- Task: Use `find` and `grep` to locate hidden `.txt` flag files in a simulated file structure
- Scoring: 15 points + bonus for first submissions
- Hints and file downloads provided

---

## ✅ General Conclusion

This project was a **complete development cycle** — from idea to deployment — including:

- Market research (TryHackMe, HackerRank, Tianchi)
- Requirement gathering & use-case modeling
- Domain design, implementation, and testing
- UI prototyping, security integration, and gamification

---

## 🚀 Future Enhancements

- 🔁 AI for adaptive challenge difficulty and personalized recommendations
- 🎯 Sentiment analysis from challenge feedback
- 🌍 Public challenge marketplace
- 🤖 Cheat detection via behavior analysis
- 💬 Chat forums & real-time collaboration
- 🥽 VR-based CTF environments

---

**This project was built with passion, teamwork, and a love for cybersecurity.**
