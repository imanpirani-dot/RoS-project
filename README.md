# Traveling Salesman Problem - Nearest Neighbor Algorithm

A Bash implementation of the Traveling Salesman Problem using the Nearest Neighbor algorithm, featuring Iranian cities as examples.

## Features

- Implements Nearest Neighbor heuristic for TSP
- Uses major Iranian cities as example locations
- Pure Bash implementation with no external dependencies
- Simple and easy to understand

## Iranian Cities Used

- Tehran (Capital)
- Mashhad
- Isfahan
- Shiraz
- Tabriz

## Installation

Clone the repository:

```bash
git clone https://github.com/imanpirani-dot/RoS-project.git
cd RoS-project
```
## Usage

Make the script executable and run:

```bash
chmod +x src/tsp_nn.sh
./src/tsp_nn.sh
```

Or use the example runner:

```bash
chmod +x examples/run_example.sh
./examples/run_example.sh
```

## Algorithm

The Nearest Neighbor algorithm:

1. Starts from a specified city (Tehran)
2. Repeatedly visits the nearest unvisited city
3. Returns the complete path and total distance

Coordinates

Cities use simplified coordinates (approximate relative positions):

· Tehran: 35, 51
· Mashhad: 36, 59
· Isfahan: 32, 51
· Shiraz: 29, 52
· Tabriz: 38, 46
