# Animation in Flutter

# 🎯 Types of Animation Widgets in Flutter

In Flutter, animation widgets are divided into **two main types**:

## 1️⃣ Without Controller (Implicit Animations)
These widgets handle the animation automatically without manually controlling frames.

**Subtypes:**
- **AnimatedFoo Widgets**  
  - The "Foo" part can be:
    - The **name of an existing widget** you already know.  
      Example: `AnimatedContainer` → the non-animated version is `Container`.  
    - A **functionality** that the widget provides (even if there’s no widget with that name).  
      Example: `AnimatedScale` → there is no widget called `Scale`, but this represents the scaling functionality.  
   - **Important:** All AnimatedFoo widgets **must have a `duration` parameter** to define how long the animation will take.
   - **Optional:** You can also provide a [`curve`](https://api.flutter.dev/flutter/animation/Curves-class.html) to control the speed pattern of the animation.

- **TweenAnimationWidget** → Example: `TweenAnimationBuilder` (uses a Tween to animate between values).

---

## 2️⃣ With Controller (Explicit Animations)
These require manual control using an `AnimationController`.

**Subtypes:**
- **FadeTransition Widget** → Controls opacity using an animation.
- **AnimatedBuilder** → Dynamically rebuilds widgets based on the animation value.

---

## ⏳ Duration & 🎯 Curves
- **`duration`** → Defines the total time the animation will take to complete. It is **required** for all implicit animations.  
- **`curve`** → Defines the pacing of the animation (how it speeds up or slows down).  
  Examples from [`Curves`](https://api.flutter.dev/flutter/animation/Curves-class.html):
  - `Curves.linear` → constant speed.  
  - `Curves.easeIn` → starts slow, then speeds up.  
  - `Curves.easeOut` → starts fast, then slows down.  
  - `Curves.bounceOut` → ends with a bounce effect.  

---

⚡ **Tip:**  
- Use **Implicit Animations** for simple and quick effects.  
- Use **Explicit Animations** for more complex and customizable animations.
