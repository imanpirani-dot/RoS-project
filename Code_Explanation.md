TSP Nearest Neighbor with Iranian Cities
File Structure

```
tsp_nn.sh
├── Main Script Entry
├── Cities Data Array
├── calculate_distance() Function
└── solve_tsp() Recursive Function
```

1. Script Header & Cities Definition

```bash
#!/bin/bash
echo "TRAVELING SALESMAN - NEAREST NEIGHBOR"
echo "Iranian Cities Example"
echo "====================================="

# Iranian cities with coordinates
cities=("Tehran:35:51" "Mashhad:36:59" "Isfahan:32:51" "Shiraz:29:52" "Tabriz:38:46")
```

What it does:

· #!/bin/bash: Shebang line specifying Bash as the interpreter
· Prints title and description to console
· Defines an array of Iranian cities with simplified coordinates:
  · Format: "CityName:X:Y"
  · Coordinates are approximate relative positions, not real geographic coordinates

Cities used:

· Tehran (Capital) - 35, 51
· Mashhad - 36, 59
· Isfahan - 32, 51
· Shiraz - 29, 52
· Tabriz - 38, 46

2. Distance Calculation Function

```bash
calculate_distance() {
    local x1=$1 y1=$2 x2=$3 y2=$4
    local dx=$((x2-x1))
    local dy=$((y2-y1))
    echo $((dx*dx + dy*dy))
}
```

How it works:

1. Takes four parameters: x1, y1, x2, y2
2. Calculates differences in x and y coordinates
3. Returns squared Euclidean distance (dx² + dy²)
4. Note: Uses squared distance to avoid floating-point math in Bash

Example:

· Tehran (35,51) to Mashhad (36,59)
· dx = 1, dy = 8
· Distance = 1² + 8² = 1 + 64 = 65

3. Main TSP Solving Function

```bash
solve_tsp() {
    local visited=($1)
    local current=$2
    local total_distance=$3
```

Parameters:

· visited: Array of cities visited so far
· current: Index of current city in the cities array
· total_distance: Cumulative distance traveled so far

3.1 Base Case - All Cities Visited

```bash
if [[ ${#visited[@]} -eq ${#cities[@]} ]]; then
    echo "Path: ${visited[@]} Distance: $total_distance"
    return
fi
```

Logic:

· If number of visited cities equals total cities, solution is complete
· Prints the path and total distance
· Returns from recursion

3.2 Finding Nearest Unvisited City

```bash
local nearest=""
local min_distance=9999

for ((i=0; i<${#cities[@]}; i++)); do
    if [[ ! " ${visited[@]} " =~ " ${cities[i]%:*} " ]]; then
```

Initialization:

· nearest: Stores index of nearest unvisited city
· min_distance: Tracks minimum distance found (initialized to large value)

Loop:

· Iterates through all cities (0 to 4)
· Checks if city is not in visited list using pattern matching
· cities[i]%:* extracts city name (removes everything after first colon)

3.3 Extracting City Coordinates

```bash
local city1=${cities[current]}
local name1=${city1%:*}; city1=${city1#*:}
local x1=${city1%:*}; local y1=${city1#*:}

local city2=${cities[i]}
local name2=${city2%:*}; city2=${city2#*:}
local x2=${city2%:*}; local y2=${city2#*:}
```

String manipulation:

· ${variable%:*}: Removes shortest match of :* from end (gets city name)
· ${variable#*:}: Removes shortest match of *: from start (gets coordinates)
· Example: "Tehran:35:51"
  · city1%:* = "Tehran"
  · city1#*: = "35:51"
  · Then split "35:51" into x1=35, y1=51

3.4 Distance Calculation and Comparison

```bash
local dist=$(calculate_distance $x1 $y1 $x2 $y2)

if [[ $dist -lt $min_distance ]]; then
    min_distance=$dist
    nearest=$i
fi
```

Process:

1. Calculates distance from current city to candidate city
2. If distance is less than current minimum, updates minimum and nearest city

3.5 Recursive Call

```bash
if [[ -n $nearest ]]; then
    solve_tsp "$(echo "${visited[@]} ${cities[nearest]%:*}")" $nearest $((total_distance + min_distance))
fi
```

Recursion:

· If a nearest city was found (not empty string)
· Makes recursive call with:
  · Updated visited list (adds new city name)
  · New current city index
  · Updated total distance (adds distance traveled)

4. Initial Function Call

```bash
solve_tsp "Tehran" 0 0
```

Starting point:

· Begins with Tehran as visited city
· Current city index is 0 (Tehran)
· Initial distance is 0

Algorithm Flow

1. Start at Tehran (index 0)
2. Find nearest unvisited city from current position
3. Move to that city, add to visited list
4. Repeat until all cities visited
5. Output complete path and total distance

Example Execution

```
TRAVELING SALESMAN - NEAREST NEIGHBOR
Iranian Cities Example
=====================================
Path: Tehran Isfahan Shiraz Mashhad Tabriz Distance: [total]
```

Key Bash Concepts Used

1. Arrays: cities=() and array manipulation
2. String Manipulation: ${var%:*} and ${var#*:} for parsing
3. Arithmetic Expansion: $((expression)) for calculations
4. Pattern Matching: =~ operator for checking array membership
5. Recursion: Function calling itself
6. Local Variables: local keyword for function-scoped variables
7. Parameter Expansion: ${#array[@]} for array length

Limitations & Notes

1. Squared Distance: Uses squared Euclidean distance (avoids sqrt in Bash)
2. No Return Trip: Doesn't return to starting city (open TSP)
3. Greedy Algorithm: May not find optimal solution
4. Fixed Coordinates: Coordinates are simplified, not real geographic
5. Bash Integer Math: All calculations are integer-based

Time Complexity

· O(n²) where n is number of cities
· Each step finds minimum among remaining cities
· 5 cities = 5+4+3+2+1 = 15 comparisons

This implementation demonstrates a classic greedy algorithm for TSP using pure Bash scripting with Iranian cities as a practical example.
