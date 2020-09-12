# tiled

[![godoc reference](https://img.shields.io/badge/godoc-reference-5272B4)](https://pkg.go.dev/github.com/lafriks/go-tiled?tab=doc)
[![Build Status](https://cloud.drone.io/api/badges/lafriks/go-tiled/status.svg)](https://cloud.drone.io/lafriks/go-tiled)

Go library to parse Tiled map editor file format (TMX) and render map to image. Currently supports only orthogonal rendering out-of-the-box.

## Installing

    $ go get github.com/lafriks/go-tiled

You can use `go get -u` to update the package. You can also just import and start using the package directly if you're using Go modules, and Go will then download the package on first compilation.

## Basic Usage:

```go
package main

import (
	"fmt"
	"os"

	"github.com/lafriks/go-tiled"
	"github.com/lafriks/go-tiled/render"
)

const mapPath = "maps/map.tmx" // Path to your Tiled Map.

func main() {
    // Parse .tmx file.
	gameMap, err := tiled.LoadFromFile(mapPath)
	if err != nil {
		fmt.Printf("error parsing map: %s", err.Error()
		os.Exit(2)
	}

	fmt.Println(gameMap)

	// You can also render the map to an in-memory image for direct
	// use with the default Renderer, or by making your own.
	renderer, err := render.NewRenderer(gameMap)
	if err != nil {
		fmt.Printf("map unsupported for rendering: %s", err.Error()
		os.Exit(2)
	}

	// Render just layer 0 to the Renderer.
	err = renderer.RenderLayer(0)
	if err != nil {
		fmt.Printf("layer unsupported for rendering: %s", err.Error()
		os.Exit(2)
	}

	// Get a reference to the Renderer's output, an image.NRGBA struct.
	img := renderer.Result

	// Clear the render result after copying the output if separation of 
	// layers is desired.
	renderer.Clear()

	// And so on. You can also export the image to a file by using the
	// Renderer's Save functions.

}

```

## Documentation

For further documentation, see https://pkg.go.dev/github.com/lafriks/go-tiled or run:

    $ godoc github.com/lafriks/go-tiled

