# 2D Worlds with Perlin Noise and Tile Maps

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

Now, you have a `TileMap` with a `TileSet` that contains the tiles you want to use.

## Create the World Generation Script

Now, let's create the script that will generate the world. Right click on the `World` node and select `New Script`. Name the script `world.gd`.