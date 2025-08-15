# Animation in Flutter

## 📌 Description
This repository is a summarized reference for everything I learn during the **Flutter Animation** course.  
It contains organized notes, explanations, and practical code examples covering both **implicit** and **explicit** animations. 
The aim is to create a clear and concise resource that I can revisit anytime, and also serve as a helpful reference for others who want to learn Flutter animations.

# 🎯 Types of Animation Widgets in Flutter

In Flutter, animation widgets are divided into **two main types**:

## 1️⃣ Without Controller (Implicit Animations)
These widgets handle the animation automatically without manually controlling frames.

# Subtypes:
## AnimatedFoo Widgets
  - The "Foo" part can be:
    - The **name of an existing widget** you already know.  
      Example: `AnimatedContainer` → the non-animated version is `Container`.  
    - A **functionality** that the widget provides (even if there’s no widget with that name).  
      Example: `AnimatedScale` → there is no widget called `Scale`, but this represents the scaling functionality.  
   - **Important:** All AnimatedFoo widgets **must have a `duration` parameter** to define how long the animation will take.
   - **Optional:** You can also provide a [`curve`](https://api.flutter.dev/flutter/animation/Curves-class.html) to control the speed pattern of the animation.7
    
## ⏳ Duration & 🎯 Curves
- **`duration`** → Defines the total time the animation will take to complete. It is **required** for all implicit animations.  
- **`curve`** → Defines the pacing of the animation (how it speeds up or slows down).  
  Examples from [`Curves`](https://api.flutter.dev/flutter/animation/Curves-class.html):
  - `Curves.linear` → constant speed.  
  - `Curves.easeIn` → starts slow, then speeds up.  
  - `Curves.easeOut` → starts fast, then slows down.  
  - `Curves.bounceOut` → ends with a bounce effect.  
     **Example:**
```dart
import 'package:flutter/material.dart';

class AnimatedContainerExample extends StatefulWidget {
  const AnimatedContainerExample({Key? key}) : super(key: key);

  @override
  State<AnimatedContainerExample> createState() => _AnimatedContainerExampleState();
}

class _AnimatedContainerExampleState extends State<AnimatedContainerExample> {
  bool isExpanded = false;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('AnimatedContainer Example')),
      body: Center(
        child: GestureDetector(
          onTap: () {
            setState(() {
              isExpanded = !isExpanded;
            });
          },
          child: AnimatedContainer(
            width: isExpanded ? 200 : 100,
            height: 100,
            color: isExpanded ? Colors.blue : Colors.red,
            duration: const Duration(seconds: 1),
            curve: Curves.easeInOut,
            child: const Center(
              child: Text(
                'Tap Me!',
                style: TextStyle(color: Colors.white, fontSize: 16),
              ),
            ),
          ),
        ),
      ),
    );
  }
}
 ```
## TweenAnimationWidget

- **Example:** `TweenAnimationBuilder` → uses a Tween to animate between values.
- `TweenAnimationBuilder` requires **three parameters**:
  1. **`builder`** → A function that defines how the widget should look based on the animation value.
  2. **`duration`** → How long the animation will take.
  3. **`tween`** → Defines the start and end values.

### About `tween`:
- `tween` is a **generic class** in Flutter.
- When creating it, you must **specify the data type** it will animate between.
- Example:  
  - For integers: use `IntTween`  
  - For doubles: use `Tween<double>`  
  - For colors: use `ColorTween`  
- The tween defines the initial value (**begin**) and the final value (**end**) for the animation.
  **Example:**
```dart
TweenAnimationBuilder<int>(
  tween: IntTween(begin: 0, end: 100),
  duration: const Duration(seconds: 2),
  builder: (context, value, child) {
    return Text(
      '$value',
      style: const TextStyle(fontSize: 24),
    );
  },
);
 ```
### About the `builder` function:
- The `builder` function in `TweenAnimationBuilder` receives **three parameters**:
  1. **`context`** → The `BuildContext` of the widget.
  2. **`value`** → The current animation value (its type matches the type of the tween).
  3. **`child`** → An optional widget that remains constant during the animation (used for optimization).

**Example:**
```dart
TweenAnimationBuilder<double>(
  tween: Tween<double>(begin: 0, end: 1),
  duration: const Duration(seconds: 2),
  builder: (context, value, child) {
    return Opacity(
      opacity: value,
      child: child,
    );
  },
  child: const Text(
    'Hello Animation!',
    style: TextStyle(fontSize: 24),
  ),
);
```
### About the `child` attribute in `TweenAnimationBuilder`:
- The `child` attribute is used for **static parts of the widget tree** that do not need to be rebuilt on every animation frame.
- This improves performance because Flutter won't rebuild that part repeatedly.
- You pass the static widget to the `child` parameter, and then **use it inside the `builder`** function.
- Commonly used for widgets like `Text`, `Icon`, or any element that does not change during the animation.

**Example:**
```dart
TweenAnimationBuilder<double>(
  tween: Tween<double>(begin: 0, end: 1),
  duration: const Duration(seconds: 2),
  child: const Text(
    'Static Text',
    style: TextStyle(fontSize: 24),
  ),
  builder: (context, value, child) {
    return Opacity(
      opacity: value,
      child: child, // Reuses the static child without rebuilding
    );
  },
);
```


---

## 2️⃣ With Controller (Explicit Animations)

These require manual control using an `AnimationController`.
## 🔧 When to Use AnimationController

Use an **AnimationController** when you need:  

- To **track the animation's value** continuously (e.g., read `controller.value` in real time).  
- To have **full control** over the animation lifecycle, including:  
  - Starting and stopping the animation (`forward()`, `stop()`)  
  - Reversing the animation (`reverse()`)  
  - Repeating the animation indefinitely (`repeat()`)  
- To build **complex or interactive animations** that need manual control.  

---


# Subtypes:
- **Foo Transition Widget** → Controls opacity using an animation.
- **AnimatedBuilder** → Dynamically rebuilds widgets based on the animation value.

## 🎯 First Type: Foo Transition (e.g., ScaleTransition)
- A **Foo Transition** widget performs an operation on its `child` widget.
- It **requires**:
  1. `child` → The widget to be animated.
  2. A property related to its name (e.g., `scale` in `ScaleTransition`).

Example:
- `ScaleTransition` needs:
  - `child` → widget to scale.
  - `scale` → an **Animation** object.

---

## ⚠️ Important:
- The `Animation` parameter (like `scale`) must be of type **`Animation<T>`**.
- You **cannot** create an `Animation` directly because `Animation` is an **abstract class**.
- Instead:
  1. Create a **Tween** with the desired range.
  2. Call `.animate()` on it, passing the `AnimationController`.
---

## The `.animate()` Function
When calling `.animate()` from the tween, it requires a **parent**  this parent is your `AnimationController`.

---
The `AnimationController` requires a property called `vsync` and optionally a `duration`.  
- **vsync** stands for *vertical synchronization*, and it is usually provided by a `TickerProvider`.  
- The `Ticker` class is a special class that calls a function every time a new frame is rendered.  
- `vsync` helps Flutter avoid rendering animations when the screen is not visible, which saves resources.  
- **duration** defines how long the animation will take. It’s not marked as `required`, but if you use `AnimationController` without providing a duration, you will get an exception.  
- There’s also an optional property called **reverseDuration**, which defines how long the reverse animation takes.  

> Note: You can’t set fixed frame-based values for animations because frame rates vary across devices — a device with a lower frame rate will take longer to render each frame, while a device with a higher frame rate will render more smoothly.
# AnimationController and TickerProvider in Flutter

When creating an `AnimationController` in Flutter, you need to pass a `vsync` parameter.  
The `vsync` ensures that the animation is synchronized with the device’s screen refresh rate,  
which improves performance and prevents unnecessary frame rendering.

---

## Why do we need `TickerProvider`?

The `vsync` parameter requires a **TickerProvider**.  
A `Ticker` is responsible for triggering frames at the right time,  
and the `TickerProvider` creates and manages this `Ticker` for you.

However, `TickerProvider` is an **abstract class**, which means you cannot create  
an object of it directly. Instead, you make your class implement it.

---

## How to provide `TickerProvider` in a StatefulWidget?

To make your `State` class a `TickerProvider`, you can use the  
`SingleTickerProviderStateMixin` or `TickerProviderStateMixin` (if you have more than one animation controller).

## Example Code

```dart
class MyAnimatedWidgetState extends State<MyAnimatedWidget>
    with SingleTickerProviderStateMixin {

  late AnimationController _controller;

  @override
  void initState() {
    super.initState();

    _controller = AnimationController(
      duration: const Duration(seconds: 2),
      vsync: this, // 'this' refers to the current State, which is now a TickerProvider
    );
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }
}

```
## Example Code
```dart
import 'package:flutter/material.dart';

class ScaleAnimationExample extends StatefulWidget {
  const ScaleAnimationExample({super.key});

  @override
  State<ScaleAnimationExample> createState() => _ScaleAnimationExampleState();
}

class _ScaleAnimationExampleState extends State<ScaleAnimationExample>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _scaleAnimation;

  @override
  void initState() {
    super.initState();

    // Step 1: Create AnimationController
    _controller = AnimationController(
      duration: const Duration(seconds: 2),
      vsync: this,
    );

    // Step 2: Create Tween and call .animate() with the controller as parent
    _scaleAnimation = Tween<double>(begin: 0.5, end: 1.5).animate(_controller);

    // Step 3: Start the animation
    _controller.repeat(reverse: true);
  }

  @override
  Widget build(BuildContext context) {
    return Center(
      child: ScaleTransition(
        scale: _scaleAnimation, // Animation value
        child: Container(
          width: 100,
          height: 100,
          color: Colors.blue,
        ),
      ),
    );
  }

  @override
  void dispose() {
    _controller.dispose(); // Always dispose controller
    super.dispose();
  }
}
```
⚡ **Tip:**  
- Use **Implicit Animations** for simple and quick effects.  
- Use **Explicit Animations** for more complex and customizable animations.
