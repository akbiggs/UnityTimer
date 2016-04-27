# Unity Timer

This library makes it easy to run callbacks after a delay in Unity3D.

# Examples

Log "Hello World" after five seconds.

```c#
Timer.Register(5f, () => Debug.Log("Hello World"))
```
