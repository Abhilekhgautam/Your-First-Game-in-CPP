# Working with Time

Time is an important concept in a game, In a game we give players the illusion of movement, But in fact we are just displaying them the static images, We keep repositioning image throughout the screen which creates the illusion of movement.

We know that,

Speed = Distance travelled / Time Taken

which implies that,

Distance Travelled = Speed * Time Taken

So to calculate the distance travelled by any object we should know about its speed and the time that has passed.

A parameter `fElapsedTime` is passed as a  parameter to `OnUserUpdate` function which is the previous frame duration in seconds.

So to handle time properly in our game we have to multiply the speed of moving object by `fElapsedTime`.
Let us Quickly bring changes to our code,
```cpp
bool OnUserUpdate(float fElapsedTime) override
{
  DrawSprite(fPlayerPositionX, fPlayerPositionY, sprPlayer.get());
  if (GetKey(olc::Key::LEFT).bHeld)
  {
     fPlayerPositionX = fPlayerPositionX - fPlayerVel * fElapsedTime;
  }
  if (GetKey(olc::Key::RIGHT).bHeld)
  {
     fPlayerPositionX = fPlayerPositionX + fPlayerVel * fElapsedTime;
  }
  if (GetKey(olc::Key::UP).bHeld) 
  {
     fPlayerPositionY = fPlayerPositionY - fPlayerVel * fElapsedTime;
  }
  if (GetKey(olc::Key::DOWN).bHeld)
  {
     fPlayerPositionY = fPlayerPositionY + fPlayerVel * fElapsedTime;
  }
  return true;
}
```

You should just remember that, if there is any motion just multiply the speed by `fElapsedTime` and you should be fine.

## The Player Doesn't Move Now

No worries, we have set `fPlayerVel = 0.5` and the value of `fElapsedTime` is very small, so their product would be even smaller.

To solve this simply increase the player's velocity.
You just need to update the value of `fPlayerVel`.

```cpp
float fPlayerVel = 90.0f;
```
A value of `90` should work fine, if that didn't worked just try some other values.

Next up we will add enemies to our game. Stay Tuned.
