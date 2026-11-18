# BST Galaxy Transport

**A C++ console-based space-trading game powered by a binary search tree.**

> **CS 255 — Computer Science II | Project X**  
> Project documentation based on the assignment specification. Feature descriptions below represent the intended game design; implementation and testing status should be verified against the repository source code.

## Overview

**BST Galaxy Transport** combines an interactive space-trading scenario with core computer science concepts: binary search trees (BSTs), recursive traversal, dynamic memory allocation, pointers, classes, and operator overloading.

You are a transport captain operating from the **Inter-Galactic Trade Station (IGTS)**, located **250 light-years from Terra Prima**. Discover planets, navigate a growing galactic map, trade for credits, and decide when to retire. Every move carries risk: a wrong jump can leave your ship lost in space, low fuel can strand the crew, and declining morale can trigger mutiny.

## Game Features

- **BST-driven galaxy map:** Planet nodes are ordered by their distance from Terra Prima.
- **Strategic navigation:** Jump inward (left child), outward (right child), or return directly to IGTS (the root).
- **Recursive map exploration:** View the full map using preorder traversal or the current planet's subtree using inorder traversal.
- **Planet discovery:** Acquire information on up to three additional planets from each planet, once.
- **Trading system:** Earn or lose credits according to each planet's trade value.
- **Resource management:** Monitor crew morale, fuel credits, and your trade account.
- **Multiple endings:** Retire or face a failed jump, fuel depletion, or mutiny.

## Gameplay

Each turn, the ship's computer reports your current location and resources:

```text
SHIP STATUS
You are orbiting: [Current Planet]
Trade Account: [Credits]
Crew Morale: [Value]/100
Fuel Credits Remaining: [Value]/100
```

### Command Menu

| Option | Command | Action |
|:---:|---|---|
| `1` | Hyperjump Inner | Move to the current node's left child; costs 10–15 morale. |
| `2` | Hyperjump Outer | Move to the current node's right child; costs 10–15 fuel credits. |
| `3` | Return to IGTS | Jump directly to the root node at no resource cost. |
| `4` | Print All Known Planets | Recursively traverse the entire BST in preorder. |
| `5` | Print Local Neighborhood | Recursively traverse the current planet's subtree in inorder. |
| `6` | Trade | Add the current planet's trade value to your account. |
| `7` | Acquire Trade Partner Knowledge | Discover and insert previously unknown planets into the BST. |
| `9` | Retire | End the game with your final trading balance. |

**Navigation warning:** The map uses directional child links, not arbitrary bidirectional routes. After leaving a planet, you cannot move straight back to its parent; return to IGTS and follow the appropriate BST path. Attempting to jump in a direction with no known child planet ends the game.

### Resources and Failure Conditions

- **Crew morale:** Starts at `100`. Inner jumps decrease morale by a random `10–15` points. Morale at `0` or below results in mutiny.
- **Fuel credits:** Starts at `100`. Outer jumps consume a random `10–15` fuel credits. Fuel at `0` or below ends the journey.
- **Trade account:** Can be positive or negative. Trading applies the current planet's `tradeVal` (between `-50` and `50` credits).
- **Retirement:** You can end your career voluntarily. Your ending depends on your final account balance.

## Binary Search Tree Design

Each discovered planet appears in a BST node. The tree is ordered by distance from Terra Prima:

- Smaller distances are inserted into the **left** subtree (inner planets).
- Larger distances are inserted into the **right** subtree (outer planets).
- The **IGTS**, at `250 LY`, is the root of the initial map.
- Two additional randomly selected planets are included when the game starts.

```text
                  IGTS (250 LY)
                   /         \
        Inner planets      Outer planets
         (< 250 LY)         (> 250 LY)
```

The game defines a fixed catalog of **100 planets** loaded from `planets.txt`. Only discovered planets are added to the player's map. Planets are dynamically allocated separately from the BST nodes, which store `Planet*` pointers.

### Traversals

| Algorithm | Order | Used for |
|---|---|---|
| **Preorder** | Root → Left → Right | Listing all known planets starting at IGTS. |
| **Inorder** | Left → Root → Right | Listing planets in the current subtree by distance. |

Both operations use **recursion**.

## Project Architecture

The assignment divides development into three parts:

| Component | Responsibility |
|---|---|
| `Planet.h` / `Planet.cpp` | Planet data, accessors, discovery state, trade partner knowledge, and overloaded operators. |
| `binNode.h` | BST node structure holding a pointer to a `Planet`. |
| `binarySearchTree.h` / `binarySearchTree.cpp` | Node insertion, recursive traversals, and node cleanup. |
| `ShipsComputer.h` / `ShipsComputerPART.cpp` | Game state, navigation, trading, discovery, failure handling, and planet allocation. |
| `main.cpp` | Main game loop and command-menu interface. |
| `planets.txt` | Catalog of planets in `name,distance,tradeValue` format. |
| `part1Testing.cpp` / `part2Testing.cpp` | Instructor-provided component tests, if included in the repository. |

**Note:** `ShipsComputerPART.cpp` is the partially completed starter filename referenced by the assignment. Check the actual repository for the final implementation filename before building.

### C++ Concepts Demonstrated

- Binary search tree insertion and ordering
- Recursive preorder and inorder traversals
- Pointer-based structures and dynamic allocation
- Class encapsulation and constructors/destructors
- Operator overloading (`<` and `<<`)
- File input and structured data parsing
- State-driven game loops and input handling

### Memory Ownership

The BST manages and releases its dynamically allocated **tree nodes**. The `ShipsComputer` manages and releases its dynamically allocated **Planet objects**. This separation prevents the BST from assuming ownership of planets it only references.

## Build and Run

**Requirements:** A C++ compiler such as `g++`, and the project's source files and `planets.txt`.

From the project directory, a typical build command is:

```bash
g++ -std=c++17 -Wall -Wextra -pedantic \
    main.cpp Planet.cpp binarySearchTree.cpp ShipsComputerPART.cpp \
    -o bst-galaxy-transport
```

Run it from a directory containing `planets.txt`:

```bash
./bst-galaxy-transport
```

On Windows with MinGW, run `bst-galaxy-transport.exe` instead.

> **Build note:** This command assumes the final `ShipsComputer` implementation remains in `ShipsComputerPART.cpp` and that the listed `.cpp` files are present. If you renamed or reorganized files, update the command accordingly. The assignment supplies a partially implemented game, so a successful build alone does not confirm that all menu features are complete.

## Testing

The assignment provides staged test drivers:

1. **Part 1 — Planet:** Compile `part1Testing.cpp` with `Planet.cpp` and the provided header; compare output with the assignment's reference.
2. **Part 2 — BST:** Compile `part2Testing.cpp` with `Planet.cpp` and `binarySearchTree.cpp`; verify insertion, preorder and inorder traversal, and recursive node cleanup.
3. **Part 3 — Full game:** Check gameplay and each menu option, resource updates, planet discovery, trading, retirement, and all documented failure conditions.

Because discovery and travel costs incorporate randomness, gameplay results may differ between runs.

## Project Status

This README documents the **required design and intended features**, not a verified release. Update this section after reviewing the final source code and running the tests.

**Implementation status:** Not verified from source code.  
**Test status:** Not verified from test results.

## Academic Context

Developed as part of **CS 255 — Computer Science II**, *Project X: BST Galaxy Transport*. The assignment identifies this as an **individual project** and specifies **no outside assistance**. Follow your course's academic integrity and repository publication requirements when using or sharing this documentation.

## License

No license has been specified. Add a `LICENSE` file only if you have the rights and permission to publish the code under that license.
