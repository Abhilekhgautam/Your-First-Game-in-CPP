# Scoreboard and the Concept of Life
We are nearly at the end of the book now, we can move the player, shoot the threatful enemies, Let us now work out
for the Scoreboard and implement the concept of life.

The Rule of the game is simple, you kill a enemy you get 5 points, you collide with the enemy your life reduces by 1,
since collision kills the enemy, you will also gain those points.

To store the score and life count, add two new variables to the private member field of the class,
```cpp
private:
  int score = 0;
  int life_count = 3;
```

## Displaying the Score:
We will use the `DrawString` function to display the score, Update `OnUserUpdate` function to,

```cpp
DrawString(0, 5, "SCORE:" + std::to_string(score));
```
We also need to update the score, when the enemy is killed,
A enemy is killed when
* The bullet strikes the enemy
* The enemy collides with the player.

We now just need to add the following line where the above conditions are satisfied.
```cpp
score = score + 5;
```
We also need to reduce the `life_count` by 1 when
* The enemy collides with the player

So just add the following line where the above condition matches in the `OnUserUpdate` function.
```cpp
life_count = life_count - 1;
```

