# 2D Worlds with Perlin Noise and Tile Maps

```
Godot Version: 4.2.1
Tested on: Janurary 22, 2024
Created by Avery Fernandez
```

This recipe was created and tested on Godot 4.2. It covers how to create a 2D world using Perlin noise and tile maps.

## Create the World Scene

First, let's go ahead and begin to create the world scene. Let's create a new `2D Scene`. Name the root node `World` and save the scene as `world.tscn`. 

## Create the Tile Map

Next, let's create a `TileMap` node (it should be a child of `World` node). We will use this node to create the world.

### Adding the Tile Set

Go ahead and click on the `TileMap` node. In the `Inspector`, click on the `Tile Set` property. This will open the `Tile Set` editor. Click on the `New Tile Set` button. This will create a new tile set.

Currently, the `TileMap` node will be using a tile size of `16 px by 16 px`. You will want to change that to the resolution of your tiles. For this recipe, we will use `64 px by 64 px`. You can change this by clicking on the `TileMap` node and click on `TileSet` in the `Inspector`. Then, change the `Tile Size` property to `64 px by 64 px`.

Now, while the `TileMap` node is still selected, at the bottom of the screen select the `TileSet` tab. This will open the `TileSet` editor. You can either add tiles by dragging and dropping images into the editor, or by clicking on the `Import` button and selecting the images you want to use. For this recipe, we will use the images [textures.zip](https://github.com/AveryUALibrary/Godot-Cookbook/blob/f858305335a2277218363dc860feddd978a3e13c/content/World/perlin_noise_tiles.zip)

> **Note:** When the message "The atlas's texture was modified. Would you like to automatically create tiles in the atlas." appears, go ahead and click yes.

We recommend going through the individual tiles and setting the `ID` property to a number that makes sense to you. For example, you may want to set the `ID` of the grass tile to `1` and the `ID` of the water tile to `0`.

The values of the `ID` for this recipe are as follows:
```
0 - Water
1 - Sand
2 - Grass
3 - Rock
```

Now, you have a `TileMap` with a `TileSet` that contains the tiles you want to use.

## Create the World Generation Script

Now, let's create the script that will generate the world. Right click on the `World` node and select `Attach Script`. Name the script `world.gd`.

The way we will generate the world is by using Perlin noise. Perlin noise is a type of noise that is used to create smooth random values. It is often used to create terrain in games. We will use a combination of Perlin noise and tile maps to create our world.

### Import the Tile Map

Click and drag the `TileMap` node into the script, before releasing the click hold down `Ctrl`. This will create a variable that references the `TileMap` node. We will use this variable to access the `TileMap` node from the script. You should see something like this:

```gdscript
@onready var tile_map = $TileMap
```

### Create our Perlin Noise Generator

For our world generation, we want to simulate the real world. We will accomplish this by using Perlin noise to generate both the world altitude, but also the world temperature and humidity. We will use the 3 parameters in combination to determine what type of tile to use.

First, let's create the variables we will use to generate the world. Add the following variables to the script:

```gdscript
var moisture = FastNoiseLite.new()
var temperature = FastNoiseLite.new()
var altitude = FastNoiseLite.new()
```

Next, let's set the seed for each of the noise generators. We will use the seed to generate the same world each time. Add the following code to the `_ready` function:

```gdscript
func _ready():
	moisture.seed = randi()
	temperature.seed = randi()
	altitude.seed = randi()
```

With the seed set, we want a function that takes the tile coordinates and returns the altitude, temperature, and moisture. Add the following function to the script:

```gdscript
func get_world_data(tile_location: Vector2):
	var altitude_value = altitude.get_noise_2d(tile_location.x, tile_location.y)
	var temperature_value = temperature.get_noise_2d(tile_location.x, tile_location.y)
	var moisture_value = moisture.get_noise_2d(tile_location.x, tile_location.y)
	return [altitude_value, temperature_value, moisture_value]
```

### Create the World Generation Function

Now, let's create the function that will generate the world. Add the following function to the script:

```gdscript
func select_tile(tile_location: Vector2):
	var world_data = get_world_data(tile_location)
	var altitude_value = world_data[0]
	var temperature_value = world_data[1]
	var moisture_value = world_data[2]
	var tile_id = 0
	if altitude_value < 0.2:
		tile_id = 0
	elif altitude_value < 0.4:
		tile_id = 1
	elif altitude_value < 0.6:
		tile_id = 2
	else:
		tile_id = 3
	tile_map.set_cell(0, Vector2i(tile_location), tile_id, Vector2i(0, 0))
```

This function takes the tile location and uses the `get_world_data` function to get the altitude, temperature, and moisture. It then uses the altitude to determine what type of tile to use. It then sets the tile at the tile location to the tile id.

### Generate the World

Now, let's generate the world. Add the following code to the `_ready` function:

```gdscript
func _ready():
    # ... Previous code ...
    for x in range(-100, 100):
		for y in range(-100, 100):
			select_tile(Vector2(x, y))