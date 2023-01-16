# Collision Detection - II
Earlier we talked about Collision Detection, where we detected the collsion 
between bullet and the enemy. This time we have to detect collision between
the enemy and player.

The concepts and techniques are similar as before,
![Collision between Enemy and Player](../image/collision-enemy-player.png)

I won't be explaining too much, If you are still confused head back to chapter-7, where I explained in detail about the collision detection.

## The conditions
* `x + sprEnemy->width >= x1`
* `x <= x1 + sprPlayer->width`
* `y + sprEnemy->height >= y1`

We check for the range of corners of the enemy, if they satisfy all of the above
condition, we can say that they have collided.

## Implementation
Update the `OnUserUpdate` method to contain following snippets,
```cpp
for(auto &elm : vEnemy){
  if(elm.alive)
  {
   // Same as before..
     if (elm.x + sprEnemy->width >= fPlayerPositionX &&
         elm.x <= fPlayerPositionX + sprPlayer->width &&
         elm.y + sprEnemy->height >= fPlayerPositionY)
        {
          elm.alive = false;
        }
      break;
  }
}
```
Above code justifies everything we talked earlier, nothing to explain here.
We set `elm.alive = false` to kill the enemy. So now you should see
enemies being killed on collision with the player.

Till now your `OnUserUpdate` method should look like this:
```cpp
bool OnUserUpdate(float fElapsedTime) override
{
  Clear(olc::BLACK);
  DrawSprite(fPlayerPositionX, fPlayerPositionY, sprPlayer.get());
  if (GetKey(olc::Key::LEFT).bHeld) {
    fPlayerPositionX = fPlayerPositionX - fPlayerVel * fElapsedTime;
   }
   if (GetKey(olc::Key::RIGHT).bHeld) {
    fPlayerPositionX = fPlayerPositionX + fPlayerVel * fElapsedTime;
    }
   if (GetKey(olc::Key::UP).bHeld) {
    fPlayerPositionY = fPlayerPositionY - fPlayerVel * fElapsedTime;
   }
        if (GetKey(olc::Key::DOWN).bHeld) {
                fPlayerPositionY = fPlayerPositionY + fPlayerVel * fElapsedTime;
        }
        if (GetKey(olc::Key::SPACE).bPressed) {
            float ftempX = fPlayerPositionX;
            float ftempY = fPlayerPositionY;
            vBullet.emplace_back(Bullet{ftempX + float(sprPlayer->width) / 2, ftempY, false});
        }

        for (auto elm: vEnemy) {
            if (elm.alive)
                DrawSprite(elm.x, elm.y, sprEnemy.get());
        }

        for (auto &elm: vBullet) {
            // only take care of bullets which are visible on the screen
            if (elm.y > -1 && !elm.dead) {
                FillCircle(int(elm.x), int(elm.y), 1, olc::RED);
                elm.y = elm.y - fBulletVel * fElapsedTime;
            }
        }

   for (auto &elm: vBullet) {
     for (auto &enemy: vEnemy) {
        if (!elm.dead && enemy.alive && elm.y > enemy.y && elm.x + 1 >= enemy.x &&
            elm.x - 1 <= enemy.x + float(sprEnemy->width) &&
            elm.y - 1 <= enemy.y + float(sprEnemy->height)) {
            // kill both bullet and enemy.
            elm.dead = true;
            enemy.alive = false;
         }
       }
     }

   for(auto &elm : vEnemy){
    if(elm.alive){
        float tempX = ((fPlayerPositionX + float(sprPlayer->width) / 2
         ) - elm.x + float(sprEnemy->height) + float(sprEnemy->width) / 2
                               );
        float tempY = (fPlayerPositionY - elm.y + float(sprEnemy->height));

        // simple pythagoras theorem
        float tempHypo = powf(tempX, 2) + powf(tempY, 2);
        float Hypo = sqrtf(tempHypo);

        float sinTheta = (tempY / Hypo);
        float cosTheta = (tempX / Hypo);

        elm.x = elm.x + fEnemyVel * cosTheta * fElapsedTime;
        elm.y = elm.y + fEnemyVel * sinTheta * fElapsedTime;

        if (elm.x + sprEnemy->width >= fPlayerPositionX &&
            elm.x <= fPlayerPositionX + sprPlayer->width &&
            elm.y + sprEnemy->height >= fPlayerPositionY)
        {
          elm.alive = false;
        }
          break;
     }
   }

return true;
}
```

Next up we will add Scoreboard and implement the concept of life
for out Player.
