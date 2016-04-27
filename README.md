# Unity Timer

This library makes it easy to run actions after a delay in Unity3D.

Written by: Alexander Biggs + Adam Robinson-Yu

# Examples

Log "Hello World" after five seconds.

```c#
Timer.Register(5f, () => Debug.Log("Hello World"))
```

Make the player jump every two seconds.

```c#
Timer.Register(2f, player.Jump, isLooped: true);
```

Have a character say hello after five seconds of in-game time have passed.

```c#
Timer.Register(5f, character.SayHello, useRealTime: false);

// Pause the game for three seconds.
Time.timeScale = 0f;
Timer.Register(3f, () => Time.timeScale = 1f);

// Because the game was paused for three seconds, the character will say hello after
// eight seconds have passed
```

Kill the player unless they hit the X key in three seconds.

```c#
public class AwardWinningGame : MonoBehaviour {
    
    public Player player;
    private Timer _killPlayerTimer;
    
    private void Start() {
        _killPlayerTimer = Timer.Register(3f, player.Die);
    }
    
    private void Update() {
        if (Input.GetKeyDown(KeyCode.X)) {
            Timer.Cancel(_killPlayerTimer);
            Debug.Log("You won the game!");
        }
    }
    
}
```
