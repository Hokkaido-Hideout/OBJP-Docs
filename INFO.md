# 🛍️ Object Pool Plugin for Unreal Engine

### 🚀 Boost Performance. Slash Spawns. Game On.

Say goodbye to the overhead of constant Actor spawning and destruction! The **Object Pool Plugin** is a high-performance, production-ready solution for Unreal Engine developers who need **fast, efficient, and scalable Actor reuse** in gameplay.

---

### 🧠 What It Does:

The Object Pool Plugin introduces a drop-in system that recycles Actors, Pawns, Characters, and Projectiles instead of spawning new ones every time. This drastically improves runtime performance — reducing memory churn, frame spikes, and garbage collection stalls.

---

### 💡 Key Features:

* **Blueprint & C++ Ready** — Easy to use in Blueprints with fully exposed functions, or plug into C++ with granular control.
* **Multi-Type Pools** — Includes separate components for Actors, Pawns, Characters, and a powerful Shared Pool for mixed types.
* **Custom Events** — Supports `OnPoolBeginPlay` and `OnPoolEndPlay` for seamless behavior resetting.
* **Auto Initialization & Deferred Spawning** — Spawn on demand, auto-load on game start, or use advanced manual control.
* **Projectile Optimization** — Includes custom projectile movement & spline support, bypassing Unreal’s default limitations.
* **Niagara Compatible** — Built-in support for CPU-based pooling with Niagara FX via Force Solo mode.

---

### 🎮 Ideal Use Cases:

* Bullet hell shooters
* RPGs with frequent NPC spawning
* Tower defense games
* Action games with repeated VFX or destructibles
* VR & mobile games needing tight performance budgets

---

### ⚠️ Pro Tips:

* Never add a Pool Component to a class it spawns. That causes infinite loops!
* Combine with Niagara’s “Force Solo” for FX pooling.

---

### 🧩 Fully Modular. Production-Ready.

Designed with real-world performance in mind, this plugin is already battle-tested in commercial projects. It’s plug-and-play, version-compatible, and easy to drop into any Unreal Engine project using 5.x.

---

### 🎯 Why Choose This Over DIY Solutions?

* Saves hundreds of hours of manual pooling setup.
* Provides fully maintained C++ backend with exposed Blueprint logic.
* Includes full support for multiple actor types, custom events, and advanced memory handling.

---

### 🛒 Ready to scale your gameplay without sacrificing performance?

**Grab the Object Pool Plugin now** and supercharge your spawning pipeline — one pooled Actor at a time.

---