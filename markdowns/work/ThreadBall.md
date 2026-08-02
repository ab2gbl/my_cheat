---
title: "ThreadBall: Multi-threaded Socket Communication"
publishedAt: "2023-01-15"
summary: "ThreadBall is a multi-threaded Python-based server-client application designed to demonstrate socket programming and concurrent connections. It simulates a message relay system where clients take turns sending and receiving data, offering a practical introduction to threading and socket communication in networked systems."
images:
  - "https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/threadball/threadball1.png"
  - "https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/threadball/threadball2.png"

team:
  - name: "Ayoub Guebli"
    role: "Software Developer"
    avatar: "/images/avatar.jpeg"
    linkedIn: "https://www.linkedin.com/in/ayoub-guebli-0615342b8"
link: "https://github.com/ab2gbl/ThreadBall"
---

# 🎮 ThreadBall

A small Java-based football game simulation demonstrating **concurrent programming** with **threads** and **semaphores**.

[Projects repo](https://github.com/ab2gbl/ThreadBall/)

## ⚙️ About

**ThreadBall** is a lightweight game simulation where two football teams compete to score goals. The players are implemented as **threads**, and ball movement is synchronized using **semaphores** to simulate real-time, thread-safe coordination.

<video width="100%" controls>
  <source src="https://github.com/user-attachments/assets/4d8dac05-1ff7-4ffb-a123-8eea61948b35" type="video/mp4" />
  Your browser does not support the video tag.
</video>

## 🧵 Key Concepts

- 🧠 Each player is a thread running concurrently
- ⛓️ Ball control is managed using semaphores
- 🔄 The ball moves forward and backward between players
- ⚽ Teams shoot and score—first to 3 goals wins!
- ❗ There’s a chance the ball is dropped, forcing a restart

## 🖥️ GUI

The game includes a simple Java Swing interface that:

- Displays players, goals, and ball movement
- Updates team scores live
- Shows fall events and winning messages

## 📁 Project Structure

```bash
/src
  ├── Team.java                # game on console
  ├── TeamAnim/                # 2 Teams with GUI
  │   ├── TeamMain.java        # Main game logic and thread control
  │   ├── MyTeamPanel.java     # Drawing the game GUI
  │   ├── MyTeamFrame.java     # JFrame wrapper
  │   └── ...                  # pics
  └── anim/                    # 1 Team with GUI
      └── ...                  # same like TeamAnim
```

## 🚀 Running the Game

```txt
src/Team                 execution console
src/anim/Main            GUI with 1 team
src/TeamAnim/TeamMain    app final
```

1. **Clone the repository**:

```bash
git clone https://github.com/ab2gbl/ThreadBall.git
```

2. **Compile the code**:

```bash
cd ThreadBall/src
javac TeamAnim/*.java
```

3. **Run the game**:

```bash
java TeamAnim.TeamMain
```

## 📚 What I Learned

This project helped me explore the development of **concurrent applications**, especially:

- Synchronization with semaphores
- Multithreaded programming in Java
- Coordinating game logic with a GUI
