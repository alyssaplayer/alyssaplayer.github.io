---
layout: page
title: Colour Palette Extractor Tool
description: A Python tool that uses K-Means clustering to extract dominant colours from an image and outputs hex codes with accessibility recommendations. 
img: assets/img/pinterest_card.png
importance: 4
category: machine learning
---

Given any image, this tool identifies its most dominant colours using K-Means clustering and returns hex codes alongside recommended font colours for accessibility.

This tool identifies the most dominant colours of any image using K-Means clustering and returns hex codes alongside recommended font colours for accessibility, using the Rec. 709 luminance formula. 

The purpose of this tool is to give designers a data-driven starting point for building a brand/design identity by extracting the dominant colour palette of any inspiration image. 

## How It Works

The script loads an image and reshapes it into a flat list of pixels. A K-means clustering model is applied to this data, defining groups of dominant colours. It then converts the RGB values to hex codes and calculates a contrast ratio to indicate whether black or white text is more suitable for each colour. 

## Example Output
The program will produce a figure showing the most dominant colours and output a table with the hex code, swatch, and recommended font colour. 

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/pinterest_card.png" title="Input image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/palette_output.png" title="Extracted palette" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: the original input image. Right: the 5 dominant colours extracted by K-Means, visualised as swatches.
</div>

and produces a table like this:

Color (Hex)     | Recommended Font Color
---------------------------------------------
#e2dce6         | Black (#000000)
#889d76         | Black (#000000)
#656336         | White (#FFFFFF)
#ecc89d         | Black (#000000)
#dcad68         | Black (#000000)

## Tools Used

- **scikit-learn** — K-Means clustering
- **matplotlib** — Used for image loading and palette visualisation  
- **numpy** — Used for pixel matrix reshaping
