# Drawing Basic Shapes
Before we start writing our Game, let us see some useful functions provided by olc pixel game engine that help us in drawing shape.

Note: I won't be providing full working code, just add these codes to the `OnUserUpdate` function and that would work.

## Drawing a Line

To draw a line use the `DrawLine` Method
```cpp
DrawLine(5,5, 30, 30);
```

`DrawLine` has a signature of:
```cpp
DrawLine(int32_t x1, int32_t y1, int32_t x2, int32_t y2, Pixel p = olc::WHITE, uint32_t pattern = 0xFFFFFFFF)
```
It simply draws a line from (x1,y1) to (x2,y2)

## Drawing a Circle

To draw a circle use the `DrawCircle` Method
```cpp
DrawCircle(50, 50, 5);
```
DrawCircle has a signature of 
```cpp
DrawCircle(int32_t x, int32_t y, int32_t radius, Pixel p = olc::WHITE, uint8_t mask = 0xFF)
```
It simply draws a cirle centered at (x, y) with a radius.

## Drawing a Filled Circle

As we might have guessed just call the `FillCircle` Method
```cpp
FillCircle(50, 50, 5, olc::RED);
```

We passed an additional parameter, the color of the pixel to fill with.

## Drawing a Rectangle

We use `DrawRect` method.
```cpp
DrawRect(5,5, 30, 40);
```

It draws a rectangle at (5,5) with a width of 30 and height of 40.

## Drawing a Triangle

We use DrawTriangle method.

```cpp
DrawTriangle(5,5,20,5, 12,10);
```
`DrawTriangle` has following signature

```cpp
DrawTriangle(int32_t x1, int32_t y1, int32_t x2, int32_t y2, int32_t x3, int32_t y3, Pixel p = olc::WHITE)
```

It simply draws a triangle between three points (x1, y1), (x2, y2) and (x3, y3);

