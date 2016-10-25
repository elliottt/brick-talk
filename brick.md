%title:
%author: J. Daugherty
%date: 2016-10-25









-> Building terminal applications <-
-> one *brick* at a time <-

-------------------------------------------------

"Life is miserable and full of pain."

George R. R. Martin,
author of *A Song of Ice and Fire* (Game of Thrones, etc.)

^

Buttercup:
  "You mock my pain."
Man in Black:
  "Life is pain, Highness. Anyone who says differently is selling
   something."

from *The Princess Bride*

-------------------------------------------------

-> Writing terminal applications _is_ painful! <-

^
-> We have to worry about ... <-
^

\ \ \ \ \ \ \ \ \ \ \ \ ... _interface layout_ and _terminal size_
^
\ \ \ \ \ \ \ \ \ \ \ \ ... when to set and unset _terminal attributes_
^
\ \ \ \ \ \ \ \ \ \ \ \ ... the _terminal's feature set_

-------------------------------------------------

-> Options in Haskell <-
========================

^
-> vty <-
---------

- Curses-like library
- Provides "image" drawing interface
- Exposes input event interface
- Uses Terminfo to configure and interact with the terminal

^

-> brick <-
-----------

- Builds on _vty_
- Provides application abstraction
- Provides a drawing and layout language
- Includes a simple text editor
- Provides attribute themes

-------------------------------------------------

-> A simple program <-
======================

~~~~

  main = simpleMain $ str "Hello, world!"  

~~~~

-------------------------------------------------

-> Why brick? <-
================

^
-> Express your UI in a _purely-functional, declarative style_: <-

~~~

  ui = withAttr "header" $
       str "This is the header"

~~~

-------------------------------------------------

-> Why brick? <-
================

-> Use _high-level combinators_ to create complex layouts: <-

~~~


  ui = border $
       vBox [ hCenter $ str "Up above"
            , hBorder
            , str "Down below (long line)"
            ]

~~~

~~~

 ┌──────────────────────┐
 │       Up above       │
 │──────────────────────│
 │Down below (long line)│
 └──────────────────────┘

~~~

-------------------------------------------------

-> Why brick? <-
================

-> Use _high-level combinators_ to create complex layouts: <-

~~~

  ui = withBorderStyle ascii $
       border $
       vBox [ hCenter $ str "Up above"
            , hBorder
            , str "Down below (long line)"
            ]

~~~

~~~

 +----------------------+
 |       Up above       |
 |----------------------|
 |Down below (long line)|
 +----------------------+

~~~

-------------------------------------------------

-> Why brick? <-
================

-> Use _high-level combinators_ to create complex layouts: <-

~~~

  ui = withBorderStyle unicodeRounded $
       border $
       vBox [ hCenter $ str "Up above"
            , hBorder
            , str "Down below (long line)"
            ]

~~~

~~~

 ╭──────────────────────╮
 │       Up above       │
 │──────────────────────│
 │Down below (long line)│
 ╰──────────────────────╯

~~~

-------------------------------------------------

-> Why brick? <-
================

-> Use _high-level combinators_ to create complex layouts: <-

~~~

  ui = withBorderStyle unicodeRounded $
       borderWithLabel (str "Border!") $
       vBox [ hCenter $ str "Up above"
            , hBorder
            , str "Down below (long line)"
            ]

~~~

~~~

 ╭────────Border!───────╮
 │       Up above       │
 │──────────────────────│
 │Down below (long line)│
 ╰──────────────────────╯

~~~

-------------------------------------------------

-> Why brick? <-
================

-> Avoid dealing with _terminal size math_: <-

~~~

  ui = fill ' ' <+> str "Right-justified"

~~~

~~~

                                                Right-justified

~~~

-------------------------------------------------

-> Why brick? <-
================

-> Use dynamic _attribute themes_: <-

~~~

  ui = withDefAttr "someAttr" $
       str "Right-justified"

  applicationTheme :: AttrMap
  applicationTheme = attrMap defAttr
    [ ("someAttr", white `on` blue)
    ]

~~~
-------------------------------------------------

-> Brick Workflow <-
====================

At a high level:

^
- Provide initial _application state_
^
- Provide _drawing function_ to _draw your state_
^
- Provide _event handling function_ to _transform your state_
^

~~~

   Draw       Await          Redraw
  Initial --> Event <------- State
   State        |              ^
                |              |
                v              |
              Handle ------> Change
              Event          State
                |
                |
                v
               Quit

~~~

-------------------------------------------------

-> Example: increment/show a counter <-
=======================================

~~~

  initialState = 0

  drawMyApp c = [str $ "Counter: " <> show c]

  handleEvent c (EvKey (KChar '+') []) = continue $ c + 1  
  handleEvent c (EvKey (KChar 'q') []) = halt       c
  handleEvent c _                      = continue   c

~~~

-------------------------------------------------

-> fin! <-

-> https://github.com/jtdaugherty/brick <-
-> http://hackage.haskell.org/package/brick <-
