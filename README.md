# SearchAlgorithmsRL
Search algorithms assignments - deterministic, stochastic and UCT-based Agents

## Search Algorithm 1: Deterministic Search in Taxi Fleet Management

### Introduction
In the first assignment, I explored deterministic search algorithms by taking on the role of a taxi business owner. The primary objective is to efficiently deliver passengers to their designated destinations within the shortest possible time frame.

### Environment
The challenge is set in a simulated environment represented as a rectangular grid, which consists of passable and impassable areas. Specific grid cells are designated as gas stations where taxis can refuel.

### Goals
The task is to model the problem effectively and implement a solution that uses search algorithms to navigate taxis around the grid, pick up passengers, and drop them off at their desired locations in the most efficient manner.

### Actions
1. **Movement**: Taxis can move one tile vertically or horizontally unless the tile is impassable or the taxi is out of fuel.
2. **Pick Up Passengers**: Taxis can pick up passengers if they occupy the same tile, given the taxi's capacity limits.
3. **Drop Off Passengers**: Passengers are dropped off at their destination tiles, refusing to exit the taxi otherwise.
4. **Refuel**: Taxis can only refuel at designated gas station tiles, restoring their fuel to full capacity.
5. **Wait**: Taxis may also wait, which doesn't affect their state.

### Example Scenario
An example input may include three taxis with valid atomic actions:
```
(("move", "taxi 1", (1, 2)), ("wait", "taxi 2"), ("pick up", "very_fancy_taxi", "Yossi"))
```

### Implementation Details
- **Input Format**: The initial environment is described using a dictionary with specific keys for the map layout, taxi details, and passenger information.
- **Class Implementation**: I implemented the TaxiProblem class with methods to define possible actions, transition between states, and check goal states.
- **Heuristic Functions**: Two heuristic functions were required. `h_1` simplifies the estimation based on passenger status, while `h_2` calculates potential distances based on Manhattan distance metrics.
- **Search Algorithm**: I utilized the A* search algorithm to ensure the solution was optimal based on the provided heuristics.

## Search Algorithm 2: Stochastic Task in Taxi Fleet Management

### Introduction
In this second assignment, I continued managing the taxi business from Homework 1, but with an added complexity of non-deterministic behavior from passengers. Unlike the previous exercise, passengers now change their goals dynamically throughout the simulation.

### Environment
The environment remains a grid as established in Homework 1, but the tasks now involve managing unpredictable changes in passenger destinations. 

### Key Differences from Homework 1
- **Passenger Goals**: Goals are chosen from a set of possible destinations and may change during the run.
- **Limited Turns**: The execution of the simulation is constrained to a limited number of turns.
- **Scoring System**: The primary objective is to maximize points, which are awarded or deducted based on specific actions taken during the simulation.

### Tasks and Actions
- **Reset**: Resets the positions of passengers and taxis to their original state without affecting the accumulated points or the number of turns left. Useful for maximizing points by starting fresh when all passengers are at their goals.
- **Terminate**: Ends the execution before all turns are used, potentially to prevent negative scores.
- **Modified Pickup**: It is illegal to pick up a passenger who is already at their goal location.

### Example Scenario
Here is an example of a passenger setup in this stochastic environment:
```python
passenger = {
    "name": "Ron",
    "location": (0,0),
    "destination": (5,5),
    "possible_goals": ((5,5), (4,5), (3,3)),
    "prob_change_goal": 0.05
}
```
- Ron starts at (0,0) and aims for (5,5), but each turn there's a 5% chance his goal will change to one of the other listed possible goals.

### Implementation Details
- **Input Format**: Similar to Homework 1 with additional parameters for passengers indicating their probability of changing goals and the list of potential new goals.
- **Agents to Implement**:
  - **TaxiAgent**: This agent handles the tasks but does not need to act optimally.
  - **OptimalTaxiAgent**: This agent is required to solve the tasks optimally using the algorithms discussed in class.

### Points and Scoring
- **Delivery of a Passenger**: +100 points
- **Resetting Environment**: -50 points
- **Refueling**: -10 points

## Search Algorithm 3: Games in Taxi Fleet Management

### Introduction
In this third assignment, I participated in a competitive taxi dispatch scenario, serving as the head of one of two taxi agencies. The goal was to outmaneuver a rival agency in efficiently transporting passengers to their destinations to maximize points.

### Environment
- **Grid Layout**: The simulation takes place in the same grid world as in the first homework, but without fuel constraints as taxis are upgraded to nuclear fusion engines.
- **Passenger Dynamics**: New passengers may appear on the map with some probability as turns progress.

### Key Differences from Previous Exercises
- **Two Fleets**: There are two separate fleets controlled by different players.
- **Turn-Based Actions**: Players take turns deciding their taxis' actions.
- **Passenger Scoring**: Each passenger now has a point value assigned, which contributes to the player's score upon successful delivery.

### Goals
The objective is to collect the maximum points by strategically transporting passengers before the rival does, within a fixed number of turns.

### Actions
- **Standard Moves**: Similar to previous exercises (move, pick up, drop off, wait), excluding refueling.
- **Game-Specific Dynamics**: Players switch fleets after the first round to ensure fairness, and total scores from both rounds determine the winner.

### Task Implementation
I developed two types of agents for this game:
- **UCT-based Agent**: This agent implements the Upper Confidence Bounds for Trees (UCT) algorithm, focusing on optimizing decision-making through exploration and exploitation of action spaces.
- **General Agent**: For this agent, I explored various strategies to enhance performance, optionally mirroring the UCT approach or innovating beyond it.

### Agent Functions
- **Initialization**: Each agent initializes with the given state and player number.
- **Action Decision**: `act(self, state)` determines the next move based on the current game state.
- **UCT-Specific Functions**: For the UCT agent, additional methods for node selection, expansion, simulation, and backpropagation were crucial for managing the decision tree.

### Code Structure
- **ex3.py**: This file contains the implementations of both my agents.
- **Supporting Files**: Include main.py for gameplay, utils.py for utility functions, simulator.py for running simulations, and a sample agent for testing.
