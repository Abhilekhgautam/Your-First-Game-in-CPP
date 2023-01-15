# Hello, OLC!

It is time to write your first program using OLC game engine. How can we break the tradition, we will start by writing a simple Hello, World program.

## Creating a New Directory
Begin by making a directory to store your Code. I'll name it 'Space Warrior' thats the name of the game we are going to create.

## For Linux
   ```shell 
   $ mkdir ~/Space-Warrior
   $ cd ~/Space-Warrior
   ```

## Adding the required Header
We will then add the olcpixelgameengine.h header in our directory which you can download form the olcpixelgameengine github page.

## Create a new cpp file
We will now create a new cpp file I will name it main.cpp.
```shell
$ touch ~/Space-Warrior/main.cpp
```

## Hello, World!
We begin by adding the required header
```cpp
#define OLC_PGE_APPLICATION
#include "olcPixelGameEngine.h"
```

Now let us create a new class for our Game, `Game` it will inherit `olc::PixelGameEngine`

```cpp
  class Game : public olc::PixelGameEngine
  {
    public:
	Game()
	{
		sAppName = "Space Warrior";
	}

	bool OnUserCreate() override
	{
		// Called once at the start, so create things here
		return true;
	}

	bool OnUserUpdate(float fElapsedTime) override
	{
	        Clear(olc::BLACK);
		DrawString(5,5, "Hello, World");	
		return true;
	}
};
``` 

Any class that inherits from `olc::PixeGameEngine` should override two functions, `OnUserCreate` and `OnUserUpdate`. Don't worry we will
get into details later.

Now lets write our main function, main function would be very neat

```cpp
int main()
{
	Game game;
	if (game.Construct(256, 240, 4, 4))
		game.Start();

	return 0;
}
```

That's it now if you run this program, you should see a window with string "Hello, World" being displayed.

## Getting into Details
 As mentioned earlier we inherited our class from `olc::PixelGameEngine` and then we override two functions,

 ### OnUserCreate()
   This function is called only once by the engine at the start so we will use this function to initialize our stuff, for now we have nothing
   to initialize, so we just return `true` from the function.

 ### OnUserUpdate()
   This function is called repeatedly by the engine per frame. For smooth transition of the game, we need to keep updating the frame and whenever the frame is updated this function is called. We will talk about this in more details later.
