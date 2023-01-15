# Adding Motion to Player
Now we are going to add life to our player, we will begin by adding motions to the player. But before we start writing some code let us look at the concept behing moving (translating) the player.

## Translating a Point
 
 Moving a player means we want to translate the player's position in the 2D plane.
 Consider a point in the 2D plane,`(x1, y1)`, now what will you do if you want the point to move by 5 units to the right?

 That's simple add 5, to the current position in the X axis, so our new postion will be `(x1 + 5, y1)`.

 Similarly to move the player to left we will substract 5, so our new postion will be `(x1 - 5, y1)`.

 Notice that we don't change the y coordinate at all because we are dealing with the horizontal motion, so y must be constant here.

 Similarly to move the point by 5 units upward, simply subtract 5 to the current position to obtain the new coordinate `(x1, y1 - 5)`.
 
 To move the point 5 units downward, simply add 5 to the current position to obtain the new coordinate `(x1, y1 + 5)`

 To Summarize,

 
| Current Position | To Left | To Right  | Upwards| Downwards|
| -------------    |:-------:| -----:    | ----:  | ----:    |
| (x1, y1)         | x1 - 5  | x1 + 5    | y1 - 5 | y1 + 5   |

If you are confused why we subtracted in case of upward motion, just remember the top left corner is `(0,0)`, so moving downwards requires addtion and moving upwards require subtraction.

## Handling User Input

We will only move the player when certain keys are pressed, so we will need to check when certain keys are pressed. That is preety easy,
We will use the `GetKey` function and pass it in the key we are looking for,
lets say the left arrow.
```cpp
if (GetKey(olc::Key::LEFT).bPressed)
{
  std::cout << "Left Arrow was pressed\n";
}
```

## Adding Motion
We need to move the player with some speed, so first we need to create a member
variable representing the speed of player.

```cpp
class Game: public olc::PixelGameEngine{
// same as before
private:
  //same as befor
  float fPlayerVel = 0.5;
};
```

Since we can handle user input, let us update our `OnUserUpdate` function to:
```cpp
bool OnUserUpdate(float fElapsedTime) override
{
  Clear(olc::BLACK);
  DrawSprite(fPlayerPositionX, fPlayerPositionY, sprPlayer.get());
  if (GetKey(olc::Key::LEFT).bHeld)
  {
     fPlayerPositionX = fPlayerPositionX - fPlayerVel;
  }
  if (GetKey(olc::Key::RIGHT).bHeld)
  {
     fPlayerPositionX = fPlayerPositionX + fPlayerVel;
  }
  if (GetKey(olc::Key::UP).bHeld)
  {
     fPlayerPositionY = fPlayerPositionY - fPlayerVel;
  }
  if (GetKey(olc::Key::DOWN).bHeld)
  {
     fPlayerPositionY = fPlayerPositionY + fPlayerVel;
  }
 return true;
}
```

I don't think we need any explanation here, we just applied the same translation technique we learnt earlier.
Once you compile and run the code, we should be able to move the player now.
