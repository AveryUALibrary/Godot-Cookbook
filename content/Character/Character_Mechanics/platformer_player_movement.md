# Platformer Player Movement
```
Godot Version: 4.2.1
Tested on: February 8, 2024
Created by Sebastian Shirk
```
* This recipe will show you how to create a simple platformer character that can move left and right, jump, and fall. We will also include some custom mechanics such as wall-jumping/sliding, double jumping, and dashing. This recipe will also show you how to create a simple platformer level to test your character in.

## Creating the Character
* Creat a new scene and make it a `CharacterBody2D`
* Add a `Sprite` as a child of the `CharacterBody2D` and set the `Texture` to a character sprite.
* Add a `CollisionShape2D` or a `CollisionPloygon2D` as a child of the `CharacterBody2D` and set the `Shape`. If you are using a `CollisionPloygon2D` you will need to set the `Polygon` points to match the shape of the character sprite.
* With our character created, we can now add a script to the `CharacterBody2D` to control its movement.
* Click on your `CharacterBody2D` and click the `Attach Script` button or you can right-click on your `CharacterBody2D` and select `Attach Script`.