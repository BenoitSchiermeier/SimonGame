# SimonGame

# Overview
SimonWorld is a Java implementation of the classic memory game "Simon Says". Built using Java libraries like javalib.funworld.World and javalib.worldimages, this interactive game challenges players to remember and replicate a sequence of colors. It progressively increases in difficulty as the player successfully replicates longer sequences.

Features
Interactive Gameplay: Players interact with the game through mouse clicks, replicating the sequence of colored buttons shown by the game.
Increasing Difficulty: Each level adds a new button to the sequence, challenging the player's memory and attention.
Graphical Interface: The game features a simple and intuitive graphical interface with four colored buttons - Red, Green, Blue, and Yellow.
Game State Management: The game maintains its state, including the sequence of buttons, level, and game over status.
Random Sequence Generation: Each new sequence is randomly generated, providing a unique challenge each time.
Gameplay
Start: The game begins with a sequence of one randomly chosen colored button.
Player Turn: During the player's turn, they need to click the buttons in the same order as displayed by the game.
Progression: Upon successful replication of the sequence, a new button is added, making the sequence longer.
End Game: The game ends when the player fails to correctly replicate the sequence.
Classes
Main Classes
SimonWorld: Extends World and represents the main game world.
Manages the game state, including the sequence of buttons, player actions, and rendering the game scene.
ILoButton: An interface representing a list of buttons.
MtLoButton: Implements ILoButton, representing an empty list of buttons.
ConsLoButton: Implements ILoButton, representing a non-empty list of buttons.
Button: Represents an individual button with a specific color and position.
Gameplay Mechanics
makeScene(): Renders the current state of the game.
onTick(): Handles the logic for each tick of the game clock.
onMousePressed(Posn pos): Processes mouse clicks and updates the game state.
Example and Test Class
ExamplesSimon: Contains examples and tests for the SimonWorld game, including tests for button interaction, game logic, and sequence generation.
