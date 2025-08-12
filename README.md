# Animation in Flutter

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
   - **Optional:** You can also provide a [`curve`](https://api.flutter.dev/flutter/animation/Curves-class.html) to control the speed pattern of the animation.
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
