# 🎯 Object Pool Plugin for Unreal Engine

![Object Pool Plugin](https://d3kjluh73b9h9o.cloudfront.net/original/4X/0/e/5/0e58561e2a95cf843fe8896e644f203c324f29d5.png)

Boost runtime performance by recycling Actors, Pawns, Characters, and Projectiles with this highly optimized Object Pool system for Unreal Engine.

---

## 📦 Installation

Make sure the plugin is downloaded and enabled through the Epic Games Launcher.

---

## 🧱 Pooled Classes

You must **reparent** your Blueprint classes to one of the specialized base classes provided by the plugin:

- `APooledActor`
- `APooledPawn`
- `APooledCharacter`

These inherit Unreal’s default types and are fully compatible with the pooling system.

![Pooled Classes](https://d3kjluh73b9h9o.cloudfront.net/original/4X/e/4/3/e4358d5fdf20b9565f031fb9f449af254fd97e4e.png)

---

## 🔁 Pool Lifecycle Events

Use these custom Blueprint events instead of the default `BeginPlay` and `EndPlay`:

- `OnPoolBeginPlay` – triggered when the instance is activated.
- `OnPoolEndPlay` – triggered when the instance is returned to the pool.

These allow clean resets without destroying the object.

![Pool Events](https://d3kjluh73b9h9o.cloudfront.net/original/4X/3/a/3/3a3f8c7089c49cfb39498e653660724ad88d1f43.png)

---

## 🧩 Pool Components

Four component types are included:

| Component             | Description                                  |
|-----------------------|----------------------------------------------|
| `UObjectPool`         | Manages pooled `APooledActor` instances      |
| `UPawnPool`           | Manages pooled `APooledPawn` instances       |
| `UCharacterPool`      | Manages pooled `APooledCharacter` instances  |
| `USharedObjectPool`   | Manages multiple classes at once             |

![Component Types](https://d3kjluh73b9h9o.cloudfront.net/original/4X/c/c/c/ccca417bbb7a7167fca5fcb9d07147b0f7ff590d.png)

> ⚠️ **Do NOT add a Pool Component to a class it spawns.** This causes infinite recursion!

---

## ⚙️ Component Properties

Each Pool Component offers the following properties:

- **Template Class**: The Actor class to pool.
- **Pool Size**: Number of instances to preallocate.
- **Auto Initialize**: Automatically build pool on `BeginPlay`.

Advanced flags:
- `ReinitializeInstances`
- `InstantiateOnDemand`
- `NeverFailDeferredSpawn`
- `KeepOrphanActorsAlive`

![Component Details](https://d3kjluh73b9h9o.cloudfront.net/optimized/4X/9/8/2/9826cbd298fcd6621625d3496a922748189d3ad1_2_424x750.png)

---

## 🧠 Spawn Nodes (Blueprint)

Use the following Blueprint nodes to spawn actors from pool:

- `Spawn Actor from Pool`
- `Spawn Pawn from Pool`
- `Spawn Character from Pool`

> ⚠️ As of **v1.9.0**, you must select the same class on **both** the Pool Component and the Spawn node.

![Spawn 1](https://d3kjluh73b9h9o.cloudfront.net/optimized/4X/f/6/5/f65fc53c4c1bc3b2fbbcbdee65e8c3c9366ac0a0_2_1035x637.png)
![Spawn 2](https://d3kjluh73b9h9o.cloudfront.net/optimized/4X/9/6/4/964c97733f75dffa84079ae7490a32c33eebcb3e_2_1035x690.png)

---

## 🔫 Projectile Support

### 🚫 Default UE5 ProjectileMovement is NOT compatible with pooling.

Use the plugin's `PooledProjectileComponent` for pooled movement. For visual projectiles without simulation, use the `PooledSplineProjectileComponent`.

![Projectile Setup](https://d3kjluh73b9h9o.cloudfront.net/optimized/4X/b/b/2/bb25f82196e9236233ecac99ecb88d714df9c288_2_342x750.png)

---

## 🌀 Spline Projectiles

The `PooledSplineProjectileComponent` uses splines and sphere traces to simulate bullet travel without physics.

- Disable physics simulation
- Provide a spline to define the path
- Use CPU-based sphere traces for collisions

![Spline Setup 1](https://d3kjluh73b9h9o.cloudfront.net/optimized/4X/f/5/9/f596074c2a1a36662ee08cd24331e4b37bbb729d_2_376x750.png)
![Spline Setup 2](https://d3kjluh73b9h9o.cloudfront.net/optimized/4X/f/7/c/f7c5e566e8457695509598f9e8ff6523a3772d3b_2_1035x466.jpeg)
![Spline Setup 3](https://d3kjluh73b9h9o.cloudfront.net/optimized/4X/d/d/3/dd3d2d9f838a5c3dda0867bdedb936b512313808_2_1035x466.jpeg)

To apply spline logic to your pooled projectiles:

1. Add a Spline Component to your level.
2. Assign it to the `PooledSplineProjectileComponent` instance.

![Spline Assignment](https://d3kjluh73b9h9o.cloudfront.net/optimized/4X/b/6/f/b6f8df67b0fed0f46382c9e91cc96a6a247f5f25_2_1035x363.png)

Once active, the bullet will automatically follow the assigned spline path.

![Bullet Follow](https://d3kjluh73b9h9o.cloudfront.net/optimized/4X/f/5/8/f58fb404c931140f601d667f25fbd96641b14885_2_1035x429.png)

---

## ✨ Niagara Systems

To use Niagara effects with pooled actors:

- Set the **Niagara Component → Force Solo** to **true**
- This ensures GPU pooling is bypassed in favor of CPU-based pooling

![Niagara](https://d3kjluh73b9h9o.cloudfront.net/optimized/4X/f/c/4/fc49bfd816698e9b9cb57ab2eefbed186d66c31b_2_765x748.png)

---

## 🧪 C++ Developer API

### Core Functions:

```cpp
UFUNCTION(BlueprintCallable)
void InitializeObjectPool();

UFUNCTION(BlueprintCallable)
void EmptyObjectPool();

UFUNCTION(BlueprintCallable)
void GetObjectsFromPool(TArray<APooledActor*>& Spawned, TArray<APooledActor*>& Inactive);

UFUNCTION(BlueprintCallable)
APooledActor* GetInactiveObject();

UFUNCTION(BlueprintCallable)
static APooledActor* BeginDeferredSpawnFromPool(...);

UFUNCTION(BlueprintCallable)
static APooledActor* FinishDeferredSpawnFromPool(...);
````

Also includes templated utility methods:

```cpp
template <typename T>
TArray<T*> GetObjectsFromPool() const;
```

> All pool components follow the same API interface (`UObjectPool`, `UPawnPool`, `UCharacterPool`, `USharedObjectPool`).

---

## 🧠 Best Practices

* Use pooling for high-frequency spawning (bullets, enemies, FX).
* Avoid pooling for unique or persistent actors.
* Always return pooled actors using `ReturnActor()` instead of `DestroyActor()`.

---

---
layout: default
title: "[SAVIOR] Create Once : SGUID"
---

{% include blueprint.html
   id="c4rjre_z"
   title="Create Once : SGUID"
   description="Creates a new GUID only when Object's 'SGUID' is invalid. If existing SGUID is valid then no new values are generated for this Object. This is important for Runtime-Generated Objects that require a 'SGUID' to respawn from Save Data."
%}
