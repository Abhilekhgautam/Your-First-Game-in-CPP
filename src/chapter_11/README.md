Congratulations, You have successfully writen your first game.

## What's Next?
 Writing good code was never the aim of this book, but good code is what we all need. Lets see how we can make our code better.

## What can we improve?
The list is pretty long but we will only  discuss what I really hate (very much) about this code. 

Let us start from our struct definition,
### Intoducing a Entity Struct.
We defined two struct one to represent the Enemy
```cpp
struct Enemy{
    float x;
    float y;
    bool alive;
};
```
and other to represent Bullets
```cpp
struct Bullet{
  float x;
  float y;
  bool dead;
};
```
Look at those fields, aren't they same. Can we not define a single struct for both of them. After all they both represent a Entity in our game, then why not have a single struct for them as:

```cpp
struct Entity {
   float x;
   float y;
   bool alive;
};
```
Then how can we distinguish between an enemy and a bullet? Probably you could add a new field in the struct that represents the type of Entity as:

```cpp
struct Entity{
 //snip
 std::string entity_type;
};
```
And then our constructor for Entity takes a string as parameter.
```cpp
Entity::Entity(std::string e_type){
  if (e_type == "Bullet"){
    // some error handling stuff...
    
    // create a bullet
  }
  // similar thing for our enemy.
}
```
This way we will just need to have a single Vector (Vector of Entity) instead of two and we can check for either enemy or bullet as:

```cpp
for(auto &elm : vEntity){
   if (elm.entity_type == "Bullet"){
    // do whatever you want to do with bullet

   }
   // similar things ...
}
```
### Improving the produceEnemy function
At the moment our `produceEnemy` function is:
```cpp
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
```
Do you see all those hard-coded integers there, they are bad, very very bad. Lets see how we can improve this, 
the integer literals we are using in every if statements actually is ensuring that there are exactly 18 enemies per row being displayed.
So what we can do is use create a new variable and use it.
```cpp
int enemies_per_row = 18;

void produceEnemy(){
  for(//...){
  if (i < enemies_per_row){
   // do sth
   }
  else if (i < 2 * enemies_per_row){
    // do sth
   }
  }

}
```
furthermore, statements like this are very unreadable
```cpp
 vEnemy.emplace_back(Enemy{float(ScreenWidth()) / 2 + (float) i * 10 - 100, 40.0f, true});
```
While we create any Entity either an enemy or a bullet, we need to know the Coordinate for those entities.
So why not make our constructor take `positions` and `entity_type` as parameter.`

```cpp
Entity::Entity(float x, float y, std::string e_type){
  // create entity...
}
```
and insert it into the vector as,
```cpp
vEntity.emplace_back(Entity(x_position, y_position, "Enemy"));
```
Somewhat cleaner? I hope you agree.

### Cleaning up OnUserUpdate function
Everything inside a single function is a big NOOO but that is what we have done with our `OnUserUpdate` function.
What we can do is, split the logic inside multiple functions, like we can add a new function that only handles user input as : 

```cpp
void handle_input(){
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
}
```
and then calling it inside our `OnUserUpdate` function as:
```cpp
bool OnUserUpdate(float fElapsedTime) override
{
// same as before
   
   handle_input();

// same as before...
}
```
Similarly you can add a new function that is just dedicated for collision detection and you can just call that inside `OnUserUpdate` function.

### Don't repeat youself
If you didn't follow my earlier suggestion of adding a new function for collision detection then this is a must have for you.
If you look at our `OnUserUpdate` function you can clearly see we are repeating ourself,

This thing is for drawing alive enemy to the screen.
```cpp
for (auto elm: vEnemy) {
   if (elm.alive)
     DrawSprite(elm.x, elm.y, sprEnemy.get());
}
```
and another similar construct for moving our enemy towards the player,
```cpp
for(auto &elm : vEnemy){
    if(elm.alive){
         float tempX = ((fPlayerPositionX + float(sprPlayer->width) / 2
            ) - elm.x + float(sprEnemy->height) + float(sprEnemy->width) / 2
        );
       float tempY = (fPlayerPositionY - elm.y + float(sprEnemy->height));

      // snip
    }
 }
```
Why not merge these two into one?

```cpp
for(auto &elm : vEntity){
  if(elm.alive && elm.entity_type == "Enemy"){
    DrawSprite(elm.x, elm.y, sprEnemy.get());
    float tempX = ((fPlayerPositionX + float(sprPlayer->width) / 2
    ) - elm.x + float(sprEnemy->height) + float(sprEnemy->width) / 2
    );
    float tempY = (fPlayerPositionY - elm.y + float(sprEnemy->height));
  }
}
```
Do you see more of these kind, just merge them.

### Stay Consistent
This might sound simple and easy, but you need to be consistent with your naming convention.
<ul>
<li>Do your variable name follow same pattern?</li>
<li>Do your function name follow same pattern?</li>
</ul>
If your answer to any of these is a No, go and improve that.

### Splitting into multiple files 
Do you feel like this is too much inside a single file, feel free to split the program into multiple files.
Everything inside a single file is also a big no and you should try avoiding that. But it is ok for simple 
projects like this.

## That's It
There might still be lots of things to improve but those were the things I really wanted to talk about.
