# Autonomous Agent Pathfinding in JADE

> An intelligent multi agent system simulation where an autonomous agent navigates unknown 2D environments using heuristic search algorithms.

![Java](https://img.shields.io/badge/Java-21-orange) ![JADE](https://img.shields.io/badge/Framework-JADE-blue)
## Overview

This project implements a **reactive agent with internal state** moving through a 2D grid map whilst navigating around objects. Unlike usual pathfinding implementations (Dijkstra or global A*), this agent has limited perception and discovers the map in real-time.

Developed as part of the *Desarrollo Basado en Agentes* course at the *Universidad de Granada*.

### The Challenge
The agent (represented by a raccoon) must find the shortest path to the target (a trash bin) while:
* Having **zero initial knowledge** of the map.
* Only seeing adjacent cells (local perception).
* Avoiding complex obstacles (walls, U-shapes, convex/concave structures).
* Minimizing energy consumption (number of steps).

## Key Features

* **Real-Time Heuristic Search:** A custom pathfinding algorithm combining Manhattan/Euclidean distances and dynamic weighing.
* **Dynamic Penalty System:** The cost of visited nodes increases which allows the agent to backtrace out of dead ends (U-shaped obstacles) automatically.
* **Exploration:** The agent builds an internal model of the world as it explores.
* **Interactive GUI:** Java Swing-based:
    * different maps (`.txt` files) can be loaded
    * Start and End positions can be set via click
    * the pathfinding is visualized step-by-step.

## Tech Stack & Architecture

* **Language:** Java 21
* **Framework:** **JADE** (Java Agent Development Framework) for agent lifecycle management and behaviors.
* **GUI:** Java Swing (Custom `GridLayoutManager`).
* **Architecture:**
    * **Agent (Scout):** Manages the lifecycle and energy.
    * **Behaviors:** Uses `WalkBehaviour` (state machine) and `StepBehaviour` (atomic actions) to implement the *Sense-Plan-Act* cycle.
    * **Environment:** Matrix-based representation of the physical world.

## Getting Started

### Prerequisites
* Java JDK 21 or higher.
* JADE Framework libraries (included in `lib/` or configured via IDE).

### Installation

1.  **Clone the repository**
    ```bash
    git clone https://github.com/gotoryu/dba-pr2
    ```

2.  **Configuration**
    Ensure the JADE library is added to your classpath. If you are using Intellij IDEA for example: Go to File --> Project Structure --> Libraries --> New Project Library --> add your jade.jar

3.  **Run the Simulation**
    Execute the `pr2mapAgent.Main` class.

4.  **Usage**
    * Select a map file.
    * Click on a grass cell to set the **Raccoon (Start)**.
    * Click on another cell to set the **Target**.
    * Press **Start** to watch the agent solve the maze.

## Algorithm Details

To solve the "U-Shape Obstacle" problem without a global map, the agent uses a **frequency-based penalty function**:

```java
// Simplified logic snippet
int penalty = basePenalty;
if (node.isRecentlyVisited()) {
    penalty = basePenalty * (maxRecentVisits - positionInHistory);
}
node.fCost = gCost + heuristic + penalty;
```
This ensures that if the agent gets stuck in a loop or a dead end, the cost of the current path rises rapidly, forcing the algorithm to explore alternative, previously "expensive" looking paths, effectively guiding it out of the trap.
