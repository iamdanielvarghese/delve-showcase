# Delve: Procedural Terrain Engine Documentation

A technical breakdown and architectural showcase of the **Delve Engine**, a Python-based procedural generation system utilizing Perlin Noise for dynamic environment rendering.

🔗 **[View the Full Interactive Architecture Site Here](https://iamdanielvarghese.github.io/delve-showcase/)**

## Overview

This repository houses the technical documentation for the Delve project. Instead of static, hand-drawn levels, Delve generates infinite, playable terrain dynamically using a custom Python architecture. 

The full documentation covers:
* The mathematics behind continuous gradient generation (Perlin Noise).
* Real-time biome quantization and mapping.
* Predictive grid-mapping for dynamic collision detection.
* Memory management and SQLite chunk caching.

## Tech Stack
* **Language:** Python 3
* **Rendering:** Pygame
* **Data Storage:** SQLite
* **Documentation Generator:** Material for MkDocs

## Local Development (For the Docs)

If you wish to run this documentation site locally:

1. Clone this repository.
2. Install the requirements:
   ```bash
   pip install mkdocs-material
