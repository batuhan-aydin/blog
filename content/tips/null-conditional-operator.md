+++ 
date = "2020-03-05"
title = "Null Conditional Operator"
slug = "null-conditional-operator" 
tags = ["csharp"]
+++

We can consider null conditinal operators as navigators.

Usually, we check data if it is null like this
```c#
if(game.Status is null) {
    // throw exception
}
```

After null conditional operators it's like this

```c#
if(game?.Status is null) {
    // throw exception
}
```

The difference is, the first code part asks, is game.Status null? 
The second one asks, is game or game.Status null? If game is null, it doesn't checks Status.

Let's write a better example. Those two codes are equivalent.

```c#
int? player = player?.Id;
int? player = (player != null) ? player.Id : null;
```