/*
Simon Says by: Benoit Schiermeier and Arinjay Singh
 */

import javalib.funworld.World;
import javalib.funworld.WorldScene;
import javalib.worldimages.*;
import tester.Tester;

import java.awt.*;
import java.util.Random;

// Represents a game of Simon Says
class SimonWorld extends World {
  //add fields needed to keep track of the state of the world
  // scene size
  static int SCENE_SIZE = 500;
  // number of ticks per second
  static double TICK_RATE = 1.5;
  // four color buttons
  Button buttonRed;
  Button buttonGreen;
  Button buttonBlue;
  Button buttonYellow;
  // whether the buttons are lit
  boolean buttonRedLight;
  boolean buttonGreenLight;
  boolean buttonBlueLight;
  boolean buttonYellowLight;
  // current empty sequence of buttons
  ILoButton sequence = new MtLoButton();
  // currently displayed sequence of colors to user?
  boolean displayingSequence;
  // current button in sequence when displaying sequence
  int sequenceIndex;
  // current button in guess when user is guessing sequence
  int guessIndex;
  // is the game over?
  boolean gameOver;
  // current level of the game (number of buttons in sequence)
  int level;
  // random number generator
  Random rand;

  // CONSTRUCTORS
  // constructor given nothing
  SimonWorld() {
    this(new Random());
  }

  // constructor given a random number generator
  SimonWorld(Random rand) {
    this.buttonRed = new Button(Color.RED, 200, 200);
    this.buttonGreen = new Button(Color.GREEN, 300, 200);
    this.buttonBlue = new Button(Color.BLUE, 200, 300);
    this.buttonYellow = new Button(Color.YELLOW, 300, 300);
    this.buttonRedLight = false;
    this.buttonGreenLight = false;
    this.buttonBlueLight = false;
    this.buttonYellowLight = false;
    this.displayingSequence = true;
    this.sequenceIndex = 0;
    this.guessIndex = 0;
    this.gameOver = false;
    this.level = 1;
    this.rand = rand;

    this.sequence = this.sequence.addRandomButton(rand);
  }

  // constructor given updated attribute values
  SimonWorld(boolean buttonRedLight, boolean buttonGreenLight, boolean buttonBlueLight,
             boolean buttonYellowLight, boolean displayingSequence, ILoButton sequence,
             int sequenceIndex, int guessIndex, boolean gameOver, int level, Random rand) {
    // initialize buttons
    this.buttonRed = new Button(Color.RED, 200, 200);
    this.buttonGreen = new Button(Color.GREEN, 300, 200);
    this.buttonBlue = new Button(Color.BLUE, 200, 300);
    this.buttonYellow = new Button(Color.YELLOW, 300, 300);
    // initialize button lights
    this.buttonRedLight = buttonRedLight;
    this.buttonGreenLight = buttonGreenLight;
    this.buttonBlueLight = buttonBlueLight;
    this.buttonYellowLight = buttonYellowLight;
    // initialize other attributes
    this.displayingSequence = displayingSequence;
    this.sequence = sequence;
    this.sequenceIndex = sequenceIndex;
    this.guessIndex = guessIndex;
    this.gameOver = gameOver;
    this.level = level;
    this.rand = rand;
  }

  /*
   * TEMPLATE
   * FIELDS:
   * ... this.SCENE_SIZE ... - int
   * ... this.TICK_RATE ... - double
   * ... this.buttonRed ... - Button
   * ... this.buttonGreen ... - Button
   * ... this.buttonBlue ... - Button
   * ... this.buttonYellow ... - Button
   * ... this.buttonRedLight ... - boolean
   * ... this.buttonGreenLight ... - boolean
   * ... this.buttonBlueLight ... - boolean
   * ... this.buttonYellowLight ... - boolean
   * ... this.sequence ... - ILoButton
   * ... this.displayingSequence ... - boolean
   * ... this.sequenceIndex ... - int
   * ... this.guessIndex ... - int
   * ... this.gameOver ... - boolean
   * ... this.level ... - int
   * ... this.rand ... - Random
   * METHODS:
   * ... this.makeScene() ... - WorldScene
   * ... this.lastScene(String) ... - WorldScene
   * ... this.onTick() ... - SimonWorld
   * ... this.onMousePressed(Posn) ... - SimonWorld
   * Methods for Fields:
   * ... this.buttonRed.drawDark() ... - WorldImage
   * ... this.buttonRed.drawLit() ... - WorldImage
   * ... this.buttonRed.drawButton(Color) ... - WorldImage
   * ... this.buttonRed.isClicked(Posn) ... - boolean
   * ... this.buttonRed.highlight(boolean) ... - WorldImage
   * ... this.buttonGreen.drawDark() ... - WorldImage
   * ... this.buttonGreen.drawLit() ... - WorldImage
   * ... this.buttonGreen.drawButton(Color) ... - WorldImage
   * ... this.buttonGreen.isClicked(Posn) ... - boolean
   * ... this.buttonGreen.highlight(boolean) ... - WorldImage
   * ... this.buttonBlue.drawDark() ... - WorldImage
   * ... this.buttonBlue.drawLit() ... - WorldImage
   * ... this.buttonBlue.drawButton(Color) ... - WorldImage
   * ... this.buttonBlue.isClicked(Posn) ... - boolean
   * ... this.buttonBlue.highlight(boolean) ... - WorldImage
   * ... this.buttonYellow.drawDark() ... - WorldImage
   * ... this.buttonYellow.drawLit() ... - WorldImage
   * ... this.buttonYellow.drawButton(Color) ... - WorldImage
   * ... this.buttonYellow.isClicked(Posn) ... - boolean
   * ... this.buttonYellow.highlight(boolean) ... - WorldImage
   * ... this.sequence.getButton(int) ... - Button
   * ... this.sequence.length() ... - int
   * ... this.sequence.addRandomButton(Random) ... - ILoButton
   * ... this.sequence.reverse() ... - ILoButton
   * ... this.sequence.reverseHelper(ILoButton) ... - ILoButton
   * ... this.rand.nextInt(int) ... - int
   */

  // draw the current state of the game
  public WorldScene makeScene() {
    // draw the game over message if needed and return
    if (this.gameOver) {
      return this.lastScene("Game Over");
    }

    // draw the background given the scene size
    WorldScene ws = new WorldScene(SCENE_SIZE, SCENE_SIZE);

    // draw the unlit buttons as a default image of the buttons
    WorldImage buttonsImage = new BesideAlignImage(AlignModeY.TOP,
        new AboveImage(
            this.buttonRed.highlight(this.buttonRedLight),
            this.buttonBlue.highlight(this.buttonBlueLight)),
        new AboveImage(
            this.buttonGreen.highlight(this.buttonGreenLight),
            this.buttonYellow.highlight(this.buttonYellowLight)));

    // draw the level in the world scene
    ws = ws.placeImageXY(new TextImage("Level: " + this.level, 20, Color.BLACK),
        SCENE_SIZE * 1 / 2, SCENE_SIZE * 1 / 8);

    // depending on whether computer displaying sequence,
    // draw the appropriate message in the world scene
    if (this.displayingSequence) {
      ws = ws.placeImageXY(new TextImage("Watch the sequence", 20, Color.BLACK),
          SCENE_SIZE * 1 / 2, SCENE_SIZE * 7 / 8);
    } else {
      ws = ws.placeImageXY(new TextImage("Repeat the sequence", 20, Color.BLACK),
          SCENE_SIZE * 1 / 2, SCENE_SIZE * 7 / 8);
    }

    // draw the buttons in the world scene and return new version of world scene
    return ws.placeImageXY(buttonsImage, SCENE_SIZE / 2, SCENE_SIZE / 2);
  }

  // Returns the final scene with the given message displayed
  public WorldScene lastScene(String msg) {
    // create a new world scene
    WorldScene ws = new WorldScene(SCENE_SIZE, SCENE_SIZE);
    // draw final scene with the given messages displayed in the center of the screen
    ws = ws.placeImageXY(new TextImage(msg, 20, Color.BLACK),
        SCENE_SIZE * 1 / 2, SCENE_SIZE * 1 / 2);
    // return the world scene
    return ws;
  }

  // handles ticking of the clock and updating the world if needed
  public SimonWorld onTick() {
    // if the game is over, return this world
    if (this.gameOver) {
      return this;
    }

    // if button is highlighted, turn it off
    if ((this.buttonRedLight || this.buttonGreenLight
        || this.buttonBlueLight || this.buttonYellowLight)) {
      return new SimonWorld(false, false, false, false,
          this.displayingSequence, this.sequence, this.sequenceIndex,
          this.guessIndex, this.gameOver, this.level, this.rand);
    }

    // if sequence index is equal to the length of the sequence, then the sequence is done
    if (this.sequenceIndex == this.sequence.length()) {
      return new SimonWorld(this.buttonRedLight, this.buttonGreenLight,
          this.buttonBlueLight, this.buttonYellowLight,
          false, this.sequence, 0,
          this.guessIndex, this.gameOver, this.level, this.rand);
    }

    // if the computer is displaying the sequence
    if (this.displayingSequence) {
      // get the next color in the sequence
      Color nextColor = this.sequence.getButton(this.sequenceIndex).color;
      if (nextColor.equals(Color.RED) && !this.buttonRedLight) {
        return new SimonWorld(true, this.buttonGreenLight,
            this.buttonBlueLight, this.buttonYellowLight,
            this.displayingSequence, this.sequence,
            this.sequenceIndex + 1, this.guessIndex,
            this.gameOver, this.level, this.rand);
      } else if (nextColor.equals(Color.GREEN) && !this.buttonGreenLight) {
        return new SimonWorld(this.buttonRedLight, true,
            this.buttonBlueLight, this.buttonYellowLight,
            this.displayingSequence, this.sequence,
            this.sequenceIndex + 1, this.guessIndex,
            this.gameOver, this.level, this.rand);
      } else if (nextColor.equals(Color.BLUE) && !this.buttonBlueLight) {
        return new SimonWorld(this.buttonRedLight, this.buttonGreenLight,
            true, this.buttonYellowLight,
            this.displayingSequence, this.sequence,
            this.sequenceIndex + 1, this.guessIndex,
            this.gameOver, this.level, this.rand);
      } else if (nextColor.equals(Color.YELLOW) && !this.buttonYellowLight) {
        return new SimonWorld(this.buttonRedLight, this.buttonGreenLight,
            this.buttonBlueLight, true,
            this.displayingSequence, this.sequence,
            this.sequenceIndex + 1, this.guessIndex,
            this.gameOver, this.level, this.rand);
      }
    }
    // return this world
    return this;
  }

  // handles mouse clicks and updates the world if needed
  public SimonWorld onMousePressed(Posn pos) {
    /*
     * Everything in the template for SimonWorld plus the following:
     * Fields of Parameters:
     * ... pos.x ... - int
     * ... pos.y ... - int
     * Methods on Parameters:
     * ... this.buttonRed.isClicked(pos) ... - boolean
     * ... this.buttonGreen.isClicked(pos) ... - boolean
     * ... this.buttonBlue.isClicked(pos) ... - boolean
     * ... this.buttonYellow.isClicked(pos) ... - boolean
     */

    //if the game is over or the computer is displaying the sequence, do nothing
    if (this.gameOver || this.displayingSequence) {
      return this;
    }

    // if the mouse click is on the red button
    if (this.buttonRed.isClicked(pos)) {
      // if the red button is the next button in the sequence
      if (this.sequence.getButton(this.guessIndex).color.equals(Color.RED)) {
        // if the guess index is equal to the length of the sequence, then the sequence is done
        if (this.guessIndex == this.sequence.length() - 1) {
          return new SimonWorld(true, this.buttonGreenLight,
              this.buttonBlueLight, this.buttonYellowLight,
              true, this.sequence.addRandomButton(this.rand),
              this.sequenceIndex, 0, this.gameOver, this.level + 1, this.rand);
        }
        // else the sequence is not done, still need to guess more buttons
        else {
          return new SimonWorld(true, this.buttonGreenLight, this.buttonBlueLight,
              this.buttonYellowLight, this.displayingSequence, this.sequence, this.sequenceIndex,
              this.guessIndex + 1, this.gameOver, this.level, this.rand);
        }
      }
      // else the guess was wrong, game over
      else {
        return new SimonWorld(true, this.buttonGreenLight, this.buttonBlueLight,
            this.buttonYellowLight, this.displayingSequence, this.sequence, this.sequenceIndex,
            this.guessIndex, true, this.level, this.rand);
      }
    }
    // if the mouse click is on the green button
    else if (this.buttonGreen.isClicked(pos)) {
      // if the red button is the next button in the sequence
      if (this.sequence.getButton(this.guessIndex).color.equals(Color.GREEN)) {
        // if the guess index is equal to the length of the sequence, then the sequence is done
        if (this.guessIndex == this.sequence.length() - 1) {
          return new SimonWorld(this.buttonRedLight, true,
              this.buttonBlueLight, this.buttonYellowLight,
              true, this.sequence.addRandomButton(this.rand),
              this.sequenceIndex, 0, this.gameOver, this.level + 1, this.rand);
        }
        // else the sequence is not done, still need to guess more buttons
        else {
          return new SimonWorld(this.buttonRedLight, true, this.buttonBlueLight,
              this.buttonYellowLight, this.displayingSequence, this.sequence, this.sequenceIndex,
              this.guessIndex + 1, this.gameOver, this.level, this.rand);
        }
      }
      // else the guess was wrong, game over
      else {
        return new SimonWorld(this.buttonRedLight, true, this.buttonBlueLight,
            this.buttonYellowLight, this.displayingSequence, this.sequence, this.sequenceIndex,
            this.guessIndex, true, this.level, this.rand);
      }
    }
    // if the mouse click is on the blue button
    else if (this.buttonBlue.isClicked(pos)) {
      // if the red button is the next button in the sequence
      if (this.sequence.getButton(this.guessIndex).color.equals(Color.BLUE)) {
        // if the guess index is equal to the length of the sequence, then the sequence is done
        if (this.guessIndex == this.sequence.length() - 1) {
          return new SimonWorld(this.buttonRedLight, this.buttonGreenLight,
              true, this.buttonYellowLight, true,
              this.sequence.addRandomButton(this.rand), this.sequenceIndex,
              0, this.gameOver, this.level + 1, this.rand);
        }
        // else the sequence is not done, still need to guess more buttons
        else {
          return new SimonWorld(this.buttonRedLight, this.buttonGreenLight,
              true, this.buttonYellowLight,
              this.displayingSequence, this.sequence, this.sequenceIndex,
              this.guessIndex + 1, this.gameOver, this.level, this.rand);
        }
      }
      // else the guess was wrong, game over
      else {
        return new SimonWorld(this.buttonRedLight, this.buttonGreenLight,
            true, this.buttonYellowLight,
            this.displayingSequence, this.sequence, this.sequenceIndex,
            this.guessIndex, true, this.level, this.rand);
      }
    }
    // if the mouse click is on the yellow button
    else if (this.buttonYellow.isClicked(pos)) {
      // if the red button is the next button in the sequence
      if (this.sequence.getButton(this.guessIndex).color.equals(Color.YELLOW)) {
        // if the guess index is equal to the length of the sequence, then the sequence is done
        if (this.guessIndex == this.sequence.length() - 1) {
          return new SimonWorld(this.buttonRedLight, this.buttonGreenLight,
              this.buttonBlueLight, true,
              true, this.sequence.addRandomButton(this.rand),
              this.sequenceIndex, 0, this.gameOver, this.level + 1, this.rand);
        }
        // else the sequence is not done, still need to guess more buttons
        else {
          return new SimonWorld(this.buttonRedLight, this.buttonGreenLight, this.buttonBlueLight,
              true, this.displayingSequence, this.sequence, this.sequenceIndex,
              this.guessIndex + 1, this.gameOver, this.level, this.rand);
        }
      }
      // else the guess was wrong, game over
      else {
        return new SimonWorld(this.buttonRedLight, this.buttonGreenLight, this.buttonBlueLight,
            true, this.displayingSequence, this.sequence, this.sequenceIndex,
            this.guessIndex, true, this.level, this.rand);
      }
    }
    // return the world as is
    return this;
  }
}

// Represents a list of buttons
interface ILoButton {
  // get the button at the given index
  Button getButton(int index);

  // get the length of the list
  int length();

  // add a random button to the end of the list
  ILoButton addRandomButton(Random rand);

  // reverse the list of buttons
  ILoButton reverse();

  // helper method for reverse
  ILoButton reverseHelper(ILoButton acc);
}

// Represents an empty list of buttons
class MtLoButton implements ILoButton {
  /*
   * TEMPLATE
   * Fields:
   * ... none ...
   * Methods:
   * ... this.getButton(int index) ... -- Button
   * ... this.length() ... -- int
   * ... this.addRandomButton(Random rand) ... -- ILoButton
   * ... this.reverse() ... -- ILoButton
   * ... this.reverseHelper(ILoButton acc) ... -- ILoButton
   */

  // get the button at the given index
  public Button getButton(int index) {
    return null;
  }

  // get the length of the list
  public int length() {
    return 0;
  }

  // add a random button to the end of the list
  public ILoButton addRandomButton(Random rand) {
    /*
     * Everything in the MtLoButton class plus the following:
     * Methods of Parameters:
     * ... rand.nextInt(int bound) ... -- int
     */

    int randInt = rand.nextInt(4);
    if (randInt == 0) {
      return new ConsLoButton(new Button(Color.RED, 200, 200), this);
    } else if (randInt == 1) {
      return new ConsLoButton(new Button(Color.GREEN, 300, 200), this);
    } else if (randInt == 2) {
      return new ConsLoButton(new Button(Color.BLUE, 200, 300), this);
    } else {
      return new ConsLoButton(new Button(Color.YELLOW, 300, 300), this);
    }
  }

  // reverse the list of buttons
  public ILoButton reverse() {
    return this;
  }

  // reverse helper
  public ILoButton reverseHelper(ILoButton acc) {
    /*
     * Everything in the ConsLoButton class plus the following:
     * Methods of Parameters:
     * ... acc.getButton(int index) ... -- Button
     * ... acc.length() ... -- int
     * ... acc.addRandomButton(Random rand) ... -- ILoButton
     * ... acc.reverse() ... -- ILoButton
     * ... acc.reverseHelper(ILoButton acc) ... -- ILoButton
     */

    return acc;
  }
}

// Represents a non-empty list of buttons
class ConsLoButton implements ILoButton {
  // the first button in the list
  Button first;
  // the rest of the buttons in the list
  ILoButton rest;

  // constructor
  ConsLoButton(Button first, ILoButton rest) {
    this.first = first;
    this.rest = rest;
  }

  /*
   * TEMPLATE
   * Fields:
   * ... this.first ... -- Button
   * ... this.rest ... -- ILoButton
   * Methods:
   * ... this.getButton(int index) ... -- Button
   * ... this.length() ... -- int
   * ... this.addRandomButton(Random rand) ... -- ILoButton
   * ... this.reverse() ... -- ILoButton
   * ... this.reverseHelper(ILoButton acc) ... -- ILoButton
   * Methods for Fields:
   * ... this.first.drawDark() ... -- WorldImage
   * ... this.first.drawLit() ... -- WorldImage
   * ... this.first.drawButton(Color) ... -- WorldImage
   * ... this.first.isClicked(Posn) ... -- boolean
   * ... this.first.highlight() ... -- WorldImage
   * ... this.rest.getButton(int index) ... -- Button
   * ... this.rest.length() ... -- int
   * ... this.rest.addRandomButton(Random rand) ... -- ILoButton
   * ... this.rest.reverse() ... -- ILoButton
   * ... this.rest.reverseHelper(ILoButton acc) ... -- ILoButton
   */

  // get button at given index
  public Button getButton(int index) {
    if (index == 0) {
      return this.first;
    } else {
      return this.rest.getButton(index - 1);
    }
  }

  // get the length of the list
  public int length() {
    return 1 + this.rest.length();
  }

  // add a random button to the end of the list
  public ILoButton addRandomButton(Random rand) {
    /*
     * Everything in the ConsLoButton class plus the following:
     * Methods of Parameters:
     * ... rand.nextInt(int bound) ... -- int
     */

    int randInt = rand.nextInt(4);
    if (randInt == 0) {
      return new ConsLoButton(new Button(Color.RED, 200, 200), this.reverse()).reverse();
    } else if (randInt == 1) {
      return new ConsLoButton(new Button(Color.GREEN, 300, 200), this.reverse()).reverse();
    } else if (randInt == 2) {
      return new ConsLoButton(new Button(Color.BLUE, 200, 300), this.reverse()).reverse();
    } else {
      return new ConsLoButton(new Button(Color.YELLOW, 300, 300), this.reverse()).reverse();
    }
  }

  // reverse the list of buttons
  public ILoButton reverse() {
    return this.reverseHelper(new MtLoButton());
  }

  // reverse helper
  public ILoButton reverseHelper(ILoButton acc) {
    /*
     * Everything in the ConsLoButton class plus the following:
     * Methods of Parameters:
     * ... acc.getButton(int index) ... -- Button
     * ... acc.length() ... -- int
     * ... acc.addRandomButton(Random rand) ... -- ILoButton
     * ... acc.reverse() ... -- ILoButton
     * ... acc.reverseHelper(ILoButton acc) ... -- ILoButton
     */

    return this.rest.reverseHelper(new ConsLoButton(this.first, acc));
  }
}

// Represents one of the four buttons you can click
class Button {
  // the color of the button
  Color color;
  // the x position of the button
  int x;
  // the y position of the button
  int y;

  // constructor
  Button(Color color, int x, int y) {
    this.color = color;
    this.x = x;
    this.y = y;
  }

  /*
   * TEMPLATE
   * Fields:
   * ... this.color ... -- Color
   * ... this.x ... -- int
   * ... this.y ... -- int
   * Methods:
   * ... this.drawDark() ... -- WorldImage
   * ... this.drawLit() ... -- WorldImage
   * ... this.drawButton(Color) ... -- WorldImage
   * ... this.isClicked(Posn) ... -- boolean
   * ... this.highlight() ... -- WorldImage
   * Methods for Fields:
   * ... this.color.darker() ... -- Color
   * ... this.color.brighter() ... -- Color
   */

  // Draw this button dark
  WorldImage drawDark() {
    return this.drawButton(this.color.darker().darker().darker());
  }

  // Draw this button lit
  WorldImage drawLit() {
    return this.drawButton(this.color.brighter().brighter().brighter());
  }

  // Draw this button with the given color
  WorldImage drawButton(Color color) {
    /*
     * Everything in the Button class plus the following:
     * Methods of Parameters:
     * ... color.darker() ... -- Color
     * ... color.brighter() ... -- Color
     */

    return new RectangleImage(100, 100, OutlineMode.SOLID, color);
  }

  // Is the given position within this button?
  boolean isClicked(Posn pos) {
    /*
     * Everything in the Button class plus the following:
     * Fields of Parameters:
     * ... pos.x ... -- int
     * ... pos.y ... -- int
     */

    return pos.x >= this.x - 50 && pos.x <= this.x + 50
        && pos.y >= this.y - 50 && pos.y <= this.y + 50;
  }

  // Draw this button lit or dark depending on the given boolean
  WorldImage highlight(boolean isLit) {
    if (isLit) {
      return this.drawLit();
    } else {
      return this.drawDark();
    }
  }
}

// Examples
class ExamplesSimon {
  //put all of your examples and tests here
  // random number generator for testing
  Random randTest = new Random(10);

  //runs the game by creating a world and calling bigBang
  boolean testSimonSays(Tester t) {
    SimonWorld starterWorld = new SimonWorld(randTest);
    int sceneSize = SimonWorld.SCENE_SIZE;
    return starterWorld.bigBang(sceneSize, sceneSize, 1 / starterWorld.TICK_RATE);
  }

  // tests for ILoButton interface
  // test getButton
  boolean testGetButton(Tester t) {
    return t.checkExpect(new MtLoButton().getButton(0), null)
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
            new MtLoButton()).getButton(0),
        new Button(Color.RED, 200, 200))
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
            new MtLoButton()).getButton(1),
        null)
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new MtLoButton())).getButton(0),
        new Button(Color.RED, 200, 200))
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new MtLoButton())).getButton(1),
        new Button(Color.GREEN, 300, 200))
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new MtLoButton())).getButton(2),
        null)
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new ConsLoButton(new Button(Color.BLUE, 200, 300),
                    new MtLoButton()))).getButton(0),
        new Button(Color.RED, 200, 200))
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new ConsLoButton(new Button(Color.BLUE, 200, 300),
                    new MtLoButton()))).getButton(1),
        new Button(Color.GREEN, 300, 200))
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new ConsLoButton(new Button(Color.BLUE, 200, 300),
                    new MtLoButton()))).getButton(2),
        new Button(Color.BLUE, 200, 300))
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new ConsLoButton(new Button(Color.BLUE, 200, 300),
                    new MtLoButton()))).getButton(3),
        null)
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new ConsLoButton(new Button(Color.BLUE, 200, 300),
                    new ConsLoButton(new Button(Color.YELLOW, 300, 300),
                        new MtLoButton())))).getButton(0),
        new Button(Color.RED, 200, 200))
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new ConsLoButton(new Button(Color.BLUE, 200, 300),
                    new ConsLoButton(new Button(Color.YELLOW, 300, 300),
                        new MtLoButton())))).getButton(1),
        new Button(Color.GREEN, 300, 200))
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new ConsLoButton(new Button(Color.BLUE, 200, 300),
                    new ConsLoButton(new Button(Color.YELLOW, 300, 300),
                        new MtLoButton())))).getButton(2),
        new Button(Color.BLUE, 200, 300))
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new ConsLoButton(new Button(Color.BLUE, 200, 300),
                    new ConsLoButton(new Button(Color.YELLOW, 300, 300),
                        new MtLoButton())))).getButton(3),
        new Button(Color.YELLOW, 300, 300))
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new ConsLoButton(new Button(Color.BLUE, 200, 300),
                    new ConsLoButton(new Button(Color.YELLOW, 300, 300),
                        new MtLoButton())))).getButton(4),
        null);
  }

  // test length method
  boolean testLength(Tester t) {
    return t.checkExpect(new MtLoButton().length(), 0)
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
        new MtLoButton()).length(), 1)
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
        new ConsLoButton(new Button(Color.GREEN, 300, 200),
            new MtLoButton())).length(), 2)
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
        new ConsLoButton(new Button(Color.GREEN, 300, 200),
            new ConsLoButton(new Button(Color.BLUE, 200, 300),
                new MtLoButton()))).length(), 3)
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
        new ConsLoButton(new Button(Color.GREEN, 300, 200),
            new ConsLoButton(new Button(Color.BLUE, 200, 300),
                new ConsLoButton(new Button(Color.YELLOW, 300, 300),
                    new MtLoButton())))).length(), 4);
  }


  // given random number generator, test addRandomButton method
  boolean testAddRandomButton(Tester t) {
    return t.checkExpect(new MtLoButton().addRandomButton(randTest),
        new ConsLoButton(new Button(Color.BLUE, 200, 300), new MtLoButton()))
        && t.checkExpect(new ConsLoButton(new Button(Color.BLUE, 200, 300),
            new MtLoButton()).addRandomButton(randTest),
        new ConsLoButton(new Button(Color.BLUE, 200, 300),
            new ConsLoButton(new Button(Color.GREEN, 300, 200), new MtLoButton())))
        && t.checkExpect(new ConsLoButton(new Button(Color.BLUE, 200, 300),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new MtLoButton())).addRandomButton(randTest),
        new ConsLoButton(new Button(Color.BLUE, 200, 300),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new ConsLoButton(new Button(Color.GREEN, 300, 200), new MtLoButton()))))
        && t.checkExpect(new ConsLoButton(new Button(Color.BLUE, 200, 300),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new ConsLoButton(new Button(Color.GREEN, 300, 200),
                    new MtLoButton()))).addRandomButton(randTest),
        new ConsLoButton(new Button(Color.BLUE, 200, 300),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new ConsLoButton(new Button(Color.GREEN, 300, 200),
                    new ConsLoButton(new Button(Color.GREEN, 300, 200), new MtLoButton())))));
  }

  // test reverse method
  boolean testReverse(Tester t) {
    return t.checkExpect(new MtLoButton().reverse(), new MtLoButton())
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
            new MtLoButton()).reverse(),
        new ConsLoButton(new Button(Color.RED, 200, 200), new MtLoButton()))
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new MtLoButton())).reverse(),
        new ConsLoButton(new Button(Color.GREEN, 300, 200),
            new ConsLoButton(new Button(Color.RED, 200, 200), new MtLoButton())))
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new ConsLoButton(new Button(Color.BLUE, 200, 300),
                    new MtLoButton()))).reverse(),
        new ConsLoButton(new Button(Color.BLUE, 200, 300),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new ConsLoButton(new Button(Color.RED, 200, 200),
                    new MtLoButton()))))
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new ConsLoButton(new Button(Color.BLUE, 200, 300),
                    new ConsLoButton(new Button(Color.YELLOW, 300, 300),
                        new MtLoButton())))).reverse(),
        new ConsLoButton(new Button(Color.YELLOW, 300, 300),
            new ConsLoButton(new Button(Color.BLUE, 200, 300),
                new ConsLoButton(new Button(Color.GREEN, 300, 200),
                    new ConsLoButton(new Button(Color.RED, 200, 200),
                        new MtLoButton())))));
  }

  // test reverseHelper method
  boolean testReverseHelper(Tester t) {
    return t.checkExpect(new MtLoButton().reverseHelper(new MtLoButton()), new MtLoButton())
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
            new MtLoButton()).reverseHelper(new MtLoButton()),
        new ConsLoButton(new Button(Color.RED, 200, 200), new MtLoButton()))
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new MtLoButton())).reverseHelper(new MtLoButton()),
        new ConsLoButton(new Button(Color.GREEN, 300, 200),
            new ConsLoButton(new Button(Color.RED, 200, 200), new MtLoButton())))
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new ConsLoButton(new Button(Color.BLUE, 200, 300),
                    new MtLoButton()))).reverseHelper(new MtLoButton()),
        new ConsLoButton(new Button(Color.BLUE, 200, 300),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new ConsLoButton(new Button(Color.RED, 200, 200), new MtLoButton()))))
        && t.checkExpect(new ConsLoButton(new Button(Color.RED, 200, 200),
            new ConsLoButton(new Button(Color.GREEN, 300, 200),
                new ConsLoButton(new Button(Color.BLUE, 200, 300),
                    new ConsLoButton(new Button(Color.YELLOW, 300, 300),
                        new MtLoButton())))).reverseHelper(new MtLoButton()),
        new ConsLoButton(new Button(Color.YELLOW, 300, 300),
            new ConsLoButton(new Button(Color.BLUE, 200, 300),
                new ConsLoButton(new Button(Color.GREEN, 300, 200),
                    new ConsLoButton(new Button(Color.RED, 200, 200),
                        new MtLoButton())))));
  }

  // tests for Button class
  // test drawDark
  boolean testDrawDark(Tester t) {
    return t.checkExpect(new Button(Color.RED, 200, 200).drawDark(),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.RED.darker().darker().darker()))
        && t.checkExpect(new Button(Color.GREEN, 300, 200).drawDark(),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.GREEN.darker().darker().darker()))
        && t.checkExpect(new Button(Color.BLUE, 200, 300).drawDark(),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.BLUE.darker().darker().darker()))
        && t.checkExpect(new Button(Color.YELLOW, 300, 300).drawDark(),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.YELLOW.darker().darker().darker()));
  }

  // test drawLit method
  boolean testDrawLit(Tester t) {
    return t.checkExpect(new Button(Color.RED, 200, 200).drawLit(),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.RED.brighter().brighter().brighter()))
        && t.checkExpect(new Button(Color.GREEN, 300, 200).drawLit(),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.GREEN.brighter().brighter().brighter()))
        && t.checkExpect(new Button(Color.BLUE, 200, 300).drawLit(),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.BLUE.brighter().brighter().brighter()))
        && t.checkExpect(new Button(Color.YELLOW, 300, 300).drawLit(),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.YELLOW.brighter().brighter().brighter()));
  }

  //test drawButton method
  boolean testDrawButton(Tester t) {
    return t.checkExpect(new Button(Color.RED, 200, 200)
            .drawButton(Color.RED.darker().darker().darker()),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.RED.darker().darker().darker()))
        && t.checkExpect(new Button(Color.GREEN, 300, 200)
            .drawButton(Color.GREEN.darker().darker().darker()),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.GREEN.darker().darker().darker()))
        && t.checkExpect(new Button(Color.BLUE, 200, 300)
            .drawButton(Color.BLUE.darker().darker().darker()),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.BLUE.darker().darker().darker()))
        && t.checkExpect(new Button(Color.YELLOW, 300, 300)
            .drawButton(Color.YELLOW.darker().darker().darker()),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.YELLOW.darker().darker().darker()))
        && t.checkExpect(new Button(Color.RED, 200, 200)
            .drawButton(Color.RED.brighter().brighter().brighter()),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.RED.brighter().brighter().brighter()))
        && t.checkExpect(new Button(Color.GREEN, 300, 200)
            .drawButton(Color.GREEN.brighter().brighter().brighter()),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.GREEN.brighter().brighter().brighter()))
        && t.checkExpect(new Button(Color.BLUE, 200, 300)
            .drawButton(Color.BLUE.brighter().brighter().brighter()),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.BLUE.brighter().brighter().brighter()))
        && t.checkExpect(new Button(Color.YELLOW, 300, 300)
            .drawButton(Color.YELLOW.brighter().brighter().brighter()),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.YELLOW.brighter().brighter().brighter()));
  }

  // test isClicked method
  boolean testIsClicked(Tester t) {
    return t.checkExpect(new Button(Color.RED, 200, 200).isClicked(new Posn(200, 200)), true)
        && t.checkExpect(new Button(Color.GREEN, 300, 200).isClicked(new Posn(300, 200)), true)
        && t.checkExpect(new Button(Color.BLUE, 200, 300).isClicked(new Posn(200, 300)), true)
        && t.checkExpect(new Button(Color.YELLOW, 300, 300).isClicked(new Posn(300, 300)), true)
        && t.checkExpect(new Button(Color.RED, 200, 200).isClicked(new Posn(300, 200)), false)
        && t.checkExpect(new Button(Color.GREEN, 300, 200).isClicked(new Posn(200, 200)), false)
        && t.checkExpect(new Button(Color.BLUE, 200, 300).isClicked(new Posn(300, 300)), false)
        && t.checkExpect(new Button(Color.YELLOW, 300, 300).isClicked(new Posn(200, 300)), false);
  }

  // test highlight method
  boolean testHighlight(Tester t) {
    return t.checkExpect(new Button(Color.RED, 200, 200).highlight(true),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.RED.brighter().brighter().brighter()))
        && t.checkExpect(new Button(Color.GREEN, 300, 200).highlight(true),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.GREEN.brighter().brighter().brighter()))
        && t.checkExpect(new Button(Color.BLUE, 200, 300).highlight(true),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.BLUE.brighter().brighter().brighter()))
        && t.checkExpect(new Button(Color.YELLOW, 300, 300).highlight(true),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.YELLOW.brighter().brighter().brighter()))
        && t.checkExpect(new Button(Color.RED, 200, 200).highlight(false),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.RED.darker().darker().darker()))
        && t.checkExpect(new Button(Color.GREEN, 300, 200).highlight(false),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.GREEN.darker().darker().darker()))
        && t.checkExpect(new Button(Color.BLUE, 200, 300).highlight(false),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.BLUE.darker().darker().darker()))
        && t.checkExpect(new Button(Color.YELLOW, 300, 300).highlight(false),
        new RectangleImage(100, 100, OutlineMode.SOLID,
            Color.YELLOW.darker().darker().darker()));
  }

  // tests for SimonWorld class

  // SimonWorld examples
  SimonWorld sw1 = new SimonWorld(new Random(1));
  WorldScene ws = new WorldScene(sw1.SCENE_SIZE, sw1.SCENE_SIZE);
  WorldImage buttonsImage = new BesideAlignImage(AlignModeY.TOP,
      new AboveImage(
          sw1.buttonRed.highlight(false),
          sw1.buttonBlue.highlight(false)),
      new AboveImage(
          sw1.buttonGreen.highlight(false),
          sw1.buttonYellow.highlight(false)));

  WorldScene result1 = ws
      .placeImageXY(new TextImage("Level: " + sw1.level, 20, Color.BLACK),
          sw1.SCENE_SIZE * 1 / 2, sw1.SCENE_SIZE * 1 / 8)
      .placeImageXY(new TextImage("Watch the sequence", 20, Color.BLACK),
          sw1.SCENE_SIZE * 1 / 2, sw1.SCENE_SIZE * 7 / 8)
      .placeImageXY(buttonsImage, sw1.SCENE_SIZE / 2, sw1.SCENE_SIZE / 2);

  SimonWorld sw2 = new SimonWorld(false, false, false, false, true,
      new ConsLoButton(new Button(Color.RED, 200, 200), new MtLoButton()),
      0, 0, false, 1, new Random());
  WorldImage buttonsImage2 = new BesideAlignImage(AlignModeY.TOP,
      new AboveImage(
          sw2.buttonRed.highlight(false),
          sw2.buttonBlue.highlight(false)),
      new AboveImage(
          sw2.buttonGreen.highlight(false),
          sw2.buttonYellow.highlight(false)));
  WorldScene result2 = ws
      .placeImageXY(new TextImage("Level: " + sw2.level, 20, Color.BLACK),
          sw2.SCENE_SIZE * 1 / 2, sw2.SCENE_SIZE * 1 / 8)
      .placeImageXY(new TextImage("Watch the sequence", 20, Color.BLACK),
          sw2.SCENE_SIZE * 1 / 2, sw2.SCENE_SIZE * 7 / 8)
      .placeImageXY(buttonsImage2, sw2.SCENE_SIZE / 2, sw2.SCENE_SIZE / 2);

  SimonWorld sw3 = new SimonWorld(true, false, false, false, false,
      new ConsLoButton(new Button(Color.RED, 200, 200), new
          ConsLoButton(new Button(Color.BLUE, 200, 300), new MtLoButton())),
      0, 1, false, 2, new Random());
  WorldImage buttonsImage3 = new BesideAlignImage(AlignModeY.TOP,
      new AboveImage(
          sw3.buttonRed.highlight(true),
          sw3.buttonBlue.highlight(false)),
      new AboveImage(
          sw3.buttonGreen.highlight(false),
          sw3.buttonYellow.highlight(false)));
  WorldScene result3 = ws
      .placeImageXY(new TextImage("Level: " + sw3.level, 20, Color.BLACK),
          sw3.SCENE_SIZE * 1 / 2, sw3.SCENE_SIZE * 1 / 8)
      .placeImageXY(new TextImage("Repeat the sequence", 20, Color.BLACK),
          sw3.SCENE_SIZE * 1 / 2, sw3.SCENE_SIZE * 7 / 8)
      .placeImageXY(buttonsImage3, sw3.SCENE_SIZE / 2, sw3.SCENE_SIZE / 2);
  SimonWorld sw4 = new SimonWorld(false, false, true, false, false,
      new ConsLoButton(new Button(Color.RED, 200, 200),
          new ConsLoButton(new Button(Color.BLUE, 200, 300), new MtLoButton())),
      0, 1, false, 2, new Random());
  WorldImage buttonsImage4 = new BesideAlignImage(AlignModeY.TOP,
      new AboveImage(
          sw4.buttonRed.highlight(false),
          sw4.buttonBlue.highlight(true)),
      new AboveImage(
          sw4.buttonGreen.highlight(false),
          sw4.buttonYellow.highlight(false)));
  WorldScene result4 = ws
      .placeImageXY(new TextImage("Level: " + sw4.level, 20, Color.BLACK),
          sw4.SCENE_SIZE * 1 / 2, sw4.SCENE_SIZE * 1 / 8)
      .placeImageXY(new TextImage("Repeat the sequence", 20, Color.BLACK),
          sw4.SCENE_SIZE * 1 / 2, sw4.SCENE_SIZE * 7 / 8)
      .placeImageXY(buttonsImage4, sw4.SCENE_SIZE / 2, sw4.SCENE_SIZE / 2);
  SimonWorld sw5 = new SimonWorld(false, true, false, false, false,
      new ConsLoButton(new Button(Color.RED, 200, 200),
          new ConsLoButton(new Button(Color.BLUE, 200, 300),
              new ConsLoButton(new Button(Color.GREEN, 300, 200), new MtLoButton()))),
      0, 2, false, 2, new Random());

  WorldImage buttonsImage5 = new BesideAlignImage(AlignModeY.TOP,
      new AboveImage(
          sw5.buttonRed.highlight(false),
          sw5.buttonBlue.highlight(false)),
      new AboveImage(
          sw5.buttonGreen.highlight(true),
          sw5.buttonYellow.highlight(false)));
  WorldScene result5 = ws
      .placeImageXY(new TextImage("Level: " + sw5.level, 20, Color.BLACK),
          sw5.SCENE_SIZE * 1 / 2, sw5.SCENE_SIZE * 1 / 8)
      .placeImageXY(new TextImage("Repeat the sequence", 20, Color.BLACK),
          sw5.SCENE_SIZE * 1 / 2, sw5.SCENE_SIZE * 7 / 8)
      .placeImageXY(buttonsImage5, sw5.SCENE_SIZE / 2, sw5.SCENE_SIZE / 2);

  SimonWorld sw6 = new SimonWorld(false, false, false, false, false,
      new ConsLoButton(new Button(Color.RED, 200, 200),
          new ConsLoButton(new Button(Color.BLUE, 200, 300),
              new ConsLoButton(new Button(Color.GREEN, 300, 200), new MtLoButton()))),
      0, 0, true, 2, new Random());
  WorldScene result6 = sw6.lastScene("Game Over");

  SimonWorld sw7 = new SimonWorld(false, false, false, true, false,
      new ConsLoButton(new Button(Color.RED, 200, 200),
          new ConsLoButton(new Button(Color.BLUE, 200, 300),
              new ConsLoButton(new Button(Color.GREEN, 300, 200),
                  new ConsLoButton(new Button(Color.YELLOW, 300, 300), new MtLoButton())))),
      0, 3, false, 2, new Random());
  WorldImage buttonsImage7 = new BesideAlignImage(AlignModeY.TOP,
      new AboveImage(
          sw7.buttonRed.highlight(false),
          sw7.buttonBlue.highlight(false)),
      new AboveImage(
          sw7.buttonGreen.highlight(false),
          sw7.buttonYellow.highlight(true)));
  WorldScene result7 = ws
      .placeImageXY(new TextImage("Level: " + sw7.level, 20, Color.BLACK),
          sw7.SCENE_SIZE * 1 / 2, sw7.SCENE_SIZE * 1 / 8)
      .placeImageXY(new TextImage("Repeat the sequence", 20, Color.BLACK),
          sw7.SCENE_SIZE * 1 / 2, sw7.SCENE_SIZE * 7 / 8)
      .placeImageXY(buttonsImage7, sw7.SCENE_SIZE / 2, sw7.SCENE_SIZE / 2);


  // test makeScene method
  boolean testMakeScene(Tester t) {
    return t.checkExpect(sw1.makeScene(), result1)
        && t.checkExpect(sw2.makeScene(), result2)
        && t.checkExpect(sw3.makeScene(), result3)
        && t.checkExpect(sw4.makeScene(), result4)
        && t.checkExpect(sw5.makeScene(), result5)
        && t.checkExpect(sw6.makeScene(), result6)
        && t.checkExpect(sw7.makeScene(), result7);
  }


  // test lastScene method
  boolean testLastScene(Tester t) {
    return t.checkExpect(new SimonWorld().lastScene("Game Over"),
        new WorldScene(500, 500).placeImageXY(new TextImage("Game Over", 20, Color.BLACK),
            250, 250))
        && t.checkExpect(new SimonWorld().lastScene("You Win!"),
        new WorldScene(500, 500).placeImageXY(new TextImage("You Win!", 20, Color.BLACK),
            250, 250));
  }

  // after onTick SimonWorld examples
  SimonWorld sw1AfterTick = new SimonWorld(false, false, true, false, true,
      new ConsLoButton(new Button(Color.BLUE, 200, 300), new MtLoButton()),
      1, 0, false, 1, new Random());
  SimonWorld sw2AfterTick = new SimonWorld(true, false, false, false, true,
      new ConsLoButton(new Button(Color.RED, 200, 200), new MtLoButton()),
      1, 0, false, 1, new Random());
  SimonWorld sw3AfterTick = new SimonWorld(false, false, false, false, false,
      new ConsLoButton(new Button(Color.RED, 200, 200), new
          ConsLoButton(new Button(Color.BLUE, 200, 300), new MtLoButton())),
      0, 1, false, 2, new Random());
  SimonWorld sw4AfterTick = new SimonWorld(false, false, false, false, false,
      new ConsLoButton(new Button(Color.RED, 200, 200),
          new ConsLoButton(new Button(Color.BLUE, 200, 300), new MtLoButton())),
      0, 1, false, 2, new Random());
  SimonWorld sw5AfterTick = new SimonWorld(false, false, false, false, false,
      new ConsLoButton(new Button(Color.RED, 200, 200),
          new ConsLoButton(new Button(Color.BLUE, 200, 300),
              new ConsLoButton(new Button(Color.GREEN, 300, 200), new MtLoButton()))),
      0, 2, false, 2, new Random());
  SimonWorld sw7AfterTick = new SimonWorld(false, false, false, false, false,
      new ConsLoButton(new Button(Color.RED, 200, 200),
          new ConsLoButton(new Button(Color.BLUE, 200, 300),
              new ConsLoButton(new Button(Color.GREEN, 300, 200),
                  new ConsLoButton(new Button(Color.YELLOW, 300, 300), new MtLoButton())))),
      0, 3, false, 2, new Random());


  // test onTick method
  boolean testOnTick(Tester t) {
    return t.checkExpect(sw1.onTick(), sw1AfterTick)
        && t.checkExpect(sw2.onTick(), sw2AfterTick)
        && t.checkExpect(sw3.onTick(), sw3AfterTick)
        && t.checkExpect(sw4.onTick(), sw4AfterTick)
        && t.checkExpect(sw5.onTick(), sw5AfterTick)
        && t.checkExpect(sw6.onTick(), sw6)
        && t.checkExpect(sw7.onTick(), sw7AfterTick);
  }


  // test onMousePressed method
  boolean testOnMousePressed(Tester t) {
    return t.checkExpect(sw1.onMousePressed(new Posn(200, 200)), sw1)
        && t.checkExpect(sw1.onMousePressed(new Posn(200, 300)), sw1)
        && t.checkExpect(sw2.onMousePressed(new Posn(300, 200)), sw2)
        && t.checkExpect(sw2.onMousePressed(new Posn(300, 300)), sw2)
        && t.checkExpect(sw3.onMousePressed(new Posn(200, 200)), //red
        new SimonWorld(true, false, false, false, false,
            new ConsLoButton(new Button(Color.RED, 200, 200), new
                ConsLoButton(new Button(Color.BLUE, 200, 300), new MtLoButton())),
            0, 1, true, 2, new Random()))
        && t.checkExpect(sw3.onMousePressed(new Posn(300, 200)), //green
        new SimonWorld(true, true, false, false, false,
            new ConsLoButton(new Button(Color.RED, 200, 200), new
                ConsLoButton(new Button(Color.BLUE, 200, 300), new MtLoButton())),
            0, 1, true, 2, new Random()))
        && t.checkExpect(sw4.onMousePressed(new Posn(200, 200)),
        new SimonWorld(true, false, true, false, false,
            new ConsLoButton(new Button(Color.RED, 200, 200),
                new ConsLoButton(new Button(Color.BLUE, 200, 300), new MtLoButton())),
            0, 1, true, 2, new Random()))
        && t.checkExpect(sw4.onMousePressed(new Posn(300, 300)),
        new SimonWorld(false, false, true, true, false,
            new ConsLoButton(new Button(Color.RED, 200, 200),
                new ConsLoButton(new Button(Color.BLUE, 200, 300), new MtLoButton())),
            0, 1, true, 2, new Random()))
        && t.checkExpect(sw5.onMousePressed(new Posn(200, 200)),
        new SimonWorld(true, true, false, false, false,
            new ConsLoButton(new Button(Color.RED, 200, 200),
                new ConsLoButton(new Button(Color.BLUE, 200, 300),
                    new ConsLoButton(new Button(Color.GREEN, 300, 200), new MtLoButton()))),
            0, 2, true, 2, new Random()))
        && t.checkExpect(sw5.onMousePressed(new Posn(300, 300)),
        new SimonWorld(false, true, false, true, false,
            new ConsLoButton(new Button(Color.RED, 200, 200),
                new ConsLoButton(new Button(Color.BLUE, 200, 300),
                    new ConsLoButton(new Button(Color.GREEN, 300, 200), new MtLoButton()))),
            0, 2, true, 2, new Random()))
        && t.checkExpect(sw6.onMousePressed(new Posn(200, 200)), sw6)
        && t.checkExpect(sw6.onMousePressed(new Posn(200, 300)), sw6)
        && t.checkExpect(sw6.onMousePressed(new Posn(300, 200)), sw6)
        && t.checkExpect(sw6.onMousePressed(new Posn(300, 300)), sw6)
        && t.checkExpect(sw7.onMousePressed(new Posn(200, 200)),
        new SimonWorld(true, false, false, true, false,
            new ConsLoButton(new Button(Color.RED, 200, 200),
                new ConsLoButton(new Button(Color.BLUE, 200, 300),
                    new ConsLoButton(new Button(Color.GREEN, 300, 200),
                        new ConsLoButton(new Button(Color.YELLOW, 300, 300), new MtLoButton())))),
            0, 3, true, 2, new Random()))
        && t.checkExpect(sw7.onMousePressed(new Posn(200, 300)),
        new SimonWorld(false, false, true, true, false,
            new ConsLoButton(new Button(Color.RED, 200, 200),
                new ConsLoButton(new Button(Color.BLUE, 200, 300),
                    new ConsLoButton(new Button(Color.GREEN, 300, 200),
                        new ConsLoButton(new Button(Color.YELLOW, 300, 300), new MtLoButton())))),
            0, 3, true, 2, new Random()))
        && t.checkExpect(sw3.onMousePressed(new Posn(100, 100)), sw3)
        && t.checkExpect(sw4.onMousePressed(new Posn(100, 300)), sw4)
        && t.checkExpect(sw5.onMousePressed(new Posn(400, 200)), sw5)
        && t.checkExpect(sw7.onMousePressed(new Posn(400, 300)), sw7);
  }
}

