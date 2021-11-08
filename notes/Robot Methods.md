---
tags: [Minesweeper Bot General Notes]
title: Robot Methods
created: '2021-10-29T23:17:35.817Z'
modified: '2021-10-30T00:41:05.108Z'
---

# Robot Methods

I'll just run through some example code that utilzes the `Robot` class. At the end of the document I'll have an example program that you can experiment with.

[Documentation](https://docs.oracle.com/en/java/javase/16/docs/api/java.desktop/java/awt/Robot.html)

### Importing

`Robot` is found within the `awt` package. `import java.awt.Robot`.

### Constructor

Constructors create a class object that allows access to the methods. Constructors make use of the `new` keyword to create them.

```java
Robot bot = new Robot();
```

- `Robot` is the type of our variable
- `bot` is the name I chose to use
- `new` tells the JDK that we're creating an object
- `Robot()` is the constructor method call found in the [documentation](https://docs.oracle.com/en/java/javase/16/docs/api/java.desktop/java/awt/Robot.html)

The constructor throws an `AWTException` if the platform configuration does not allow low-level input control. Java requires us to handle the error just in case it is thrown. We'll also need to import it using `import java.awt.AWtException`. 

```java
try{
  Robot bot = new Robot();
}
catch(AWTException e){
  // handle the error however you please
  System.out.println(e);
}
```
This is the correct way to use the `Robot` constructor. 

*Instead of using a try catch, we can create our `Robot` object within a method that throws the `AWTException` instead*

```java
public static void createBot() throws AWTException{
  Robot bot  = new Robot();
}
```

### Mouse Movement

`public void mouseMove​(int x, int y)`

This one is pretty self explanatory. Notice that this is a nonstatic method, so it must be called against a `Robot` object. The x/y coordinates start in the __upper left hand corner__ of the screen.

```java
// moves the mouse to (600, 500)
bot.mousemove(600, 500)
```

### Mouse Clicks

`Robot` gives us two methods to click the mouse: `public void mousePress​(int buttons)` and `public void mouseRelease​(int buttons)`. To simulate a mouse click, you must use both of these methods. We can simulate pretty much every button on a mouse, even mice that have extra buttons can be pressed if the JDK has the proper `Toolkit` settings enabled.

The three mouse buttons:
- InputEvent.BUTTON1_DOWN_MASK
- InputEvent.BUTTON2_DOWN_MASK
- InputEvent.BUTTON3_DOWN_MASK

`InputEvent` needs to be imported with `java.awt.event.InputEvent`.

```java
bot.mousePress(InputEvent.BUTTON1_DOWN_MASK);
bot.mouseRelease(InputEvent.BUTTON1_DOWN_MASK)
```
*Mouse clicks where the mouse currently rests. Use `mouseMove()` to change it's location.*

### Getting screen resolution

Someone asked if we could get the screen resoulation. I almost had it correct during our meeting! The answer is within the `Toolkit` object, I was just missing one method call. The screen height and width are within a `Dimension` object that is returned by using a method on within the `Toolkit`. So both will need to be imported.
```java
import java.awt.Toolkit;
import java.awt.Dimension;

Dimension screensize = Toolkit.getDefaultToolkit().getScreenSize();
double width = screensize.getWidth();
double height = screensize.getHeight();
```

### Getting pixel colors

We'll need another import for this one because the `public Color getPixelColor​(int x, int y)` returns a `Color` object. Import the `Color` class like so: `import java.awt.Color`.

```java
Color pxColor = bot.getPixelColor(500, 600)
```

---

Here's some sample code that moves the mouse around, clicks, and prints some `Color` objects. Feel free to expirement with the `Robot` class. I've made a few bots to automate workflow and some games. Comes in really handy!

```java
import java.awt.Robot;
import java.awt.AWTException;
import java.awt.Dimension;
import java.awt.Toolkit;
import java.awt.event.InputEvent;

public class BotExample {
  public static void main(String[] args) {
    try {
      Robot bot = new Robot();

      // settind delay to half a second
      bot.setAutoDelay(500);

      // get screen dimensions
      Dimension screensize = Toolkit.getDefaultToolkit().getScreenSize();
      double width = screensize.getWidth();
      double height = screensize.getHeight();

      System.out.printf("%d, %d\n", (int) width, (int) height);

      // mouse movement
      bot.mouseMove(0, 0);
      bot.mouseMove(500, 600);
      bot.mouseMove((int) width, (int) height);

      // performing a mouse click
      bot.mouseMove(800, 900);
      bot.mousePress(InputEvent.BUTTON1_DOWN_MASK);
      bot.mouseRelease(InputEvent.BUTTON1_DOWN_MASK);

      // loop through a straight line of pixels in the middle of the screen
      // I'm opting to just print the color object itself
      for (int i = 0; i < width; i++) {
          System.out.println(bot.getPixelColor(i, (int) height / 2));
      }

    } catch (AWTException e) {
      System.out.println(e);
    }
  }
}
```
