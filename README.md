# Unity Timer

Run actions after a delay in Unity3D.

This library has been battle-tested and hardened throughout numerous projects, including the award-winning [Pitfall Planet](http://pitfallplanet.com/).

Written by Alexander Biggs + Adam Robinson-Yu.

### Installation

Download the Timer.unitypackage file in this repository. Open the package file to install the package into your currently opened project in Unity.

### Basic Example

The Unity Timer package provides the following method for creating timers:

```c#
/// <summary>
/// Register a new timer that should fire an event after a certain amount of time
/// has elapsed.
/// </summary>
/// <param name="duration">The time to wait before the timer should fire, in seconds.</param>
/// <param name="onComplete">An action to fire when the timer completes.</param>
public static Timer Register(float duration, Action onComplete);
```

The method is called like this:

```c#
// Log "Hello World" after five seconds.

Timer.Register(5f, () => Debug.Log("Hello World"));
```

## Features

**Make a timer repeat by setting `isLooped` to true.**

```c#
// Call the player's jump method every two seconds.

Timer.Register(2f, player.Jump, isLooped: true);
```

**Cancel a timer after calling it.**

```c#
Timer timer;

void Start() {
   timer = Timer.Register(2f, () => Debug.Log("You won't see this text if you press X."));
}

void Update() {
   if (Input.GetKeyDown(KeyCode.X)) {
      Timer.Cancel(timer);
   }
}
```

**Measure time by [realtimeSinceStartup](http://docs.unity3d.com/ScriptReference/Time-realtimeSinceStartup.html) instead of scaled game time by setting `useRealTime` to true.**

```c#
// Let's say you pause your game by setting the timescale to 0.
Time.timeScale = 0f;

// ...Then set useRealTime so this timer will still fire even though the game time isn't progressing.
Timer.Register(1f, this.HandlePausedGameState, useRealTime: true);
```
**Attach the timer to a MonoBehaviour so that the timer is destroyed when the MonoBehaviour is.**

Very often, a timer called from a MonoBehaviour will manipulate that behaviour's state. Thus, it is common practice to cancel the timer in the OnDestroy method of the MonoBehaviour. We've added a convenient extension method that attaches a Timer to a MonoBehaviour such that it will automatically cancel the timer when the MonoBehaviour is detected as null.

```c#
public class CoolMonoBehaviour : MonoBehaviour {

   void Start() {
   
      // Use the AttachTimer extension method to create a timer that is destroyed when this
      // object is destroyed.
      this.AttachTimer(5f, () => {
      
         // If this code runs after the object is destroyed, a null reference will be thrown,
         // which could corrupt game state.
         this.gameObject.transform.position = Vector3.zero;
      });
   }
   
   void Update() {
      // This code could destroy the object at any time!
      if (Input.GetKeyDown(KeyCode.X)) {
         GameObject.Destroy(this.gameObject);
      }
   }
}
```

**A number of other useful features are included...**
- timer.Pause()
- timer.Resume()
- timer.GetTimeRemaining()
- timer.GetRatioComplete()
- timer.isDone

## Motivation

Out of the box, there are two main ways of handling timers in Unity:

1. Use a coroutine with the WaitForSeconds method.
2. Store the time that your timer started in a private variable (e.g. `startTime = Time.time`), then check in an Update call if `Time.time - startTime >= timerDuration`.

The first method is verbose, forcing you to refactor your code to use IEnumerator functions. Furthermore, it necessitates having access to a MonoBehaviour instance to start the coroutine, meaning that solution will not work in non-MonoBehaviour classes.

The second method is error-prone, and hides away the actual game logic that you are trying to express.

This library alleviates both of these concerns, making it easy to add an easy-to-read, expressive timer to any class in your Unity project.

## Usage Notes / Caveats

1. This library does not currently seem to work on the Hololens. We are looking into a solution for this.
2. All timers are destroyed when changing scenes. This behaviour is typically desired, and it happens because timers are updated by a TimerController that is also destroyed when the scene changes.
