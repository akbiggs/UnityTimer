# Unity Timer

Run actions after a delay in Unity3D.

This library has been battle-tested and hardened throughout numerous projects, including the award-winning [Pitfall Planet](http://pitfallplanet.com/).

Written by Alexander Biggs + Adam Robinson-Yu.

### Installation

Download the Timer.unitypackage file in this repository. Open the package file to install the package into your currently opened project in Unity.

### Basic Example

```c#
// Log "Hello World" after five seconds.

Timer.Register(5f, () => Debug.Log("Hello World"));
```

### Features

Make a timer repeat by setting `isLooped` to true.

```c#
// Call the player's jump method every two seconds.

Timer.Register(2f, player.Jump, isLooped: true);
```

Cancel a timer after calling it.

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

Measure time by [realtimeSinceStartup](http://docs.unity3d.com/ScriptReference/Time-realtimeSinceStartup.html) instead of scaled game time by setting `useRealTime` to true.

```c#
// Pause your game by setting the timescale to 0.
Time.timeScale = 0f;

// Use real time so this timer will still fire even though the game time isn't progressing.
Timer.Register(1f, character.SayHello, useRealTime: true);
```

### Motivation

Out of the box, there are two main ways of handling timers in Unity:

1. Use a coroutine with the WaitForSeconds method.
2. Store the time that your timer started in a private variable (e.g. `startTime = Time.time`), then check in an Update call if `Time.time - startTime >= timerDuration`.

The first method is verbose, forcing you to refactor your code to use IEnumerator functions. Furthermore, it necessitates having access to a MonoBehaviour instance to start the coroutine, meaning that solution will not work in non-MonoBehaviour classes.

The second method is error-prone, and hides away the actual game logic that you are trying to express.

This library alleviates both of these concerns, making it easy to add an easy-to-read, expressive timer to any class in your Unity project.

### Usage Notes / Caveats

1. This library does not currently seem to work on the Hololens. We are looking into a solution for this.
2. Timers are destroyed when changing scenes.
