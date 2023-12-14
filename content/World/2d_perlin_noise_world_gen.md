# 2D Worlds with Perlin Noise and Tile Maps

This recipe was created and tested on Godot 4.2. It covers how to create a 2D world using Perlin noise and tile maps.

## Create the World Scene

First, let's go ahead and begin to create the world scene. Let's create a new `2D Scene`. Name the root node `World` and save the scene as `world.tscn`. 

## Create the Tile Map

Next, let's create a `TileMap` node (it should be a child of `World` node). We will use this node to create the world.

### Adding the Tile Set

Go ahead and click on the `TileMap` node. In the `Inspector`, click on the `Tile Set` property. This will open the `Tile Set` editor. Click on the `New Tile Set` button. This will create a new tile set.

Now, while the `TileMap` node is still selected, at the bottom of the screen select the `TileSet` tab. This will open the `TileSet` editor. You can either add tiles by dragging and dropping images into the editor, or by clicking on the `Import` button and selecting the images you want to use. For this recipe, we will use the images [textures.zip]()

