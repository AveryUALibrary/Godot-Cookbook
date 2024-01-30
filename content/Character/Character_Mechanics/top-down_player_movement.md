# Top-down Player Movement
```
Godot Version: 4.2.1
Tested on: January 30, 2024
Created by Sebastian Shirk
```
This recipe will cover how to create a top-down player movement system. This system will allow the player to move in 8 directions (up, down, left, right, and diagnols). This recipe will also cover player movement with a camera system that will follow the player around the world as well as screen wrap without a camera.

## Setting up the Character
* Create a new scene and make the root node a `CharacterBody2D` node. Rename and save the scene as `Player.tscn`. Add a `Sprite2D`, a `Camera2D` (if you are going to be using screen wrap, ignore adding this camera), and a `CollisionShape2D` or `CollisionPloygon2D` as children of the `Player` node. Add in the texture you want to use for your player and create the collision shape or polygon you want to use as your players collision. 
* We will go ahead and allocate the keys we want to use for the player movement. In the `Input Map` tab found in the project settings, add the following inputs:
    * `move_up` - `W`
    * `move_down` - `S`
    * `move_left` - `A`
    * `move_right` - `D`

## Setting up the Player Script
* We want to create a script that will handle movement in all 8 directions. The full finished scripts can be found in the `Final Scripts` section. 
* Create a new script and attach it to the `Player` node. Name the script `Player.gd`.
* Lets add the one key variable we need for player movement. Move speed. Add the following variable to the top of the script:
```gdscript
var move_speed = 500
```
* Now we want to create a function that will handle the player movement. Add the following code to the physics process function:
```gdscript
	var move_direction = Vector2(0,0)
	if Input.is_action_pressed("move_left"):
		move_direction.x -= move_speed
	if Input.is_action_pressed("move_right"):
		move_direction.x += move_speed
	if Input.is_action_pressed("move_up"):
		move_direction.y -= move_speed
	if Input.is_action_pressed("move_down"):
		move_direction.y += move_speed
	
	if move_direction.x != 0  && move_direction.y != 0:
		move_direction = move_direction.normalized() * move_speed
	
	position += move_direction * delta
	move_and_slide()
```
* Lets break down what is happening here. First we create a variable called `move_direction` and set it to a `Vector2` with a value of `0,0`. This will be the direction we want to move the player. Next we check to see if the `move_left` action is pressed. If it is, we subtract the `move_speed` from the `x` value of the `move_direction` variable. We do this for all 4 directions. If the player is moving in both the `x` and `y` directions, we normalize the `move_direction` variable and multiply it by the `move_speed`. This will make sure that the player moves at the same speed in all directions. Finally, we add the `move_direction` to the players position. The `move_and_slide()` function will handle any collisions for us by attempting to find a path to slide your character on whenever colliding with something.

* Lets test this code by running the scene. If you have a `Camera2D` you will have to toggle off its `Enable` property in the `Inspector` to see the affects of your script. If you are placing your `Player` in a seperate scene and want to keep the `Camera2D` on, make sure to add other nodes to the scene so that the player can be seen moving as the other nodes remain stationary. Otherwise, you will not see the player move.


## Creating a Screen Wrap

* Lets create a screen wrap for our player. This will allow the player to move off the screen and appear on the opposite side of the screen. This will give us the ability to create a large playing field without having to worry about the player moving out of the view of the camera.