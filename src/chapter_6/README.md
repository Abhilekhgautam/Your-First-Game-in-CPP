# Shooting the Bullets
We have a player and enemies, whats next? Lets shoot them.

We will need multiple bullets which we will store as a vector,
and we will also create a struct `Bullet` to represent the bullets
in our game

```cpp
struct Bullet{
  float x;
  float y;
  bool dead;
};

```
Here x and y represents the position of the bullet and dead represents the 
current status of the bullet.

Let us then create another variable in our private field to represent the 
collection of Bullets. I'll name it `vBullet`, you can name it anything
you want.

```cpp
private:
  //Same as before
  std::vector<Bullet> vBullet;
  // Same as before
```

We should first decide the key that should be pressed so the player 
will shoot.

I choose the `space` key, you can choose any key of your choice.
Let us update the `OnUserUpdate` method so that it listens to the
pressing of the space key.

```cpp
if (GetKey(olc::Key::SPACE).bPressed)
{
  float ftempX = fPlayerPositionX;
  float ftempY = fPlayerPositionY;
  vBullet.emplace_back(Bullet{ftempX + float(sprPlayer->width) / 2, ftempY, false});
}
```
The main concern for the `Bullets` should be their shooting position, I set it to be
from the center of the player.

Whenever `space` key is pressed we add a new Bullet to the vector with values set to
```
x = fPlayerPostionX + sprPlayer->width / 2
y = fPlayerPositionY
dead = false
```

here `fPlayerPositionX` is the top left corner of the player's sprite, to move to the
center of image we should add up half the width of the image to its top left position.

## Drawing the bullets

We will simply use a circle with the radius set to 1, as a bullet.
Similar to drawing enemies, we will use loop here, as we have a collection
of bullets.

```cpp
for (auto &elm: vBullet) {
  if (elm.y > -1 && !elm.dead) {
    FillCircle(elm.x, elm.y, 1, olc::RED);
  }
}
```

## Moving the Bullets

We need to move the bullets with some speed, so let us declare yet another private member
variable that represents the bullet velocity, I will name it fBulletVel, and set it as 

```cpp
float fBulletVel = 180.0f;
```
Moving the bullet means translating its position, since bullets will be fired upward we will be 
subtracting the y coordinate of the bullet with its speed, i.e.

```cpp
for (auto &elm: vBullet) {
  // only take care of bullets which are visible on the screen
   if (elm.y > -1 && !elm.dead) {
     FillCircle(int(elm.x), int(elm.y), 1, olc::RED);
     elm.y = elm.y - fBulletVel * fElapsedTime;
 }
}
```

We don't need to draw bullets that are gone away from the screen display area, so used the if condition to ensure it.
Remember that, we just multiply `fElapsedTime` with the velocity of the moving object.

As of now, your code should look like this
```cpp
#define OLC_PGE_APPLICATION
#include "olcPixelGameEngine.h"

struct Enemy{
    float x;
    float y;
    bool alive;
};

struct Bullet{
  float x;
  float y;
  bool dead;
};

class Game : public olc::PixelGameEngine
{
public:
    Game(){
        sAppName = "Space Warrior";
    }

    void produceEnemy() {
        for (int i = 0; i < 70; ++i) {
            if (i < 18)
                vEnemy.emplace_back(Enemy{float(ScreenWidth()) / 2 + (float) i * 10 - 100, 40.0f, true});
            else if (i < 36)
                vEnemy.emplace_back(Enemy{float(ScreenWidth()) / 2 + 10.0f * (float) i - 280, 55.0f, true});
            else if (i < 54)
                vEnemy.emplace_back(Enemy{float(ScreenWidth()) / 2 + 10.0f * (float) i - 460, 75.0f, true});
            else
                vEnemy.emplace_back(Enemy{float(ScreenWidth()) / 2 + 10.0f * (float) i - 640, 95.0f, true});
        }
    }


    bool OnUserCreate() override
    {
        produceEnemy();
        sprPlayer = std::make_unique<olc::Sprite>("/home/abhilekh/Downloads/player.png");
        sprEnemy = std::make_unique<olc::Sprite>("/home/abhilekh/Downloads/enemy.png");
        return true;
    }

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
        return true;
    }
private:
    float fPlayerPositionX = 185.0f;
    float fPlayerPositionY = 250.0f;
    float fPlayerVel = 90.0f;
    float fBulletVel = 180.0f;

    std::unique_ptr<olc::Sprite> sprPlayer;
    std::unique_ptr<olc::Sprite> sprEnemy;

    std::vector<Enemy> vEnemy;
    std::vector<Bullet> vBullet;

};
int main()
{
    Game game;
    if (game.Construct(450, 340, 4, 4))
        game.Start();

    return 0;
}
```

If that's the case, you should now see bullets being shot. But that bullet doesn't
kill any enemy.

Next up we will implement collision detection for the Bullet and the enemy. Until then Have Fun!
