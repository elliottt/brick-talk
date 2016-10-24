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

-> Writing terminal applications *is* painful! <-

^
-> We have to worry about ... <-
^

\ \ \ \ \ \ \ \ \ \ \ \ ... *interface layout* and *terminal size*
^
\ \ \ \ \ \ \ \ \ \ \ \ ... how to set *terminal attributes*
^
\ \ \ \ \ \ \ \ \ \ \ \ ... the *terminal's feature set*

-------------------------------------------------

-> *vty* provides: <-
-> primitives for *drawing*, processing *input events*, <-
-> and managing *terminal capabilities* <-

^

-> *brick* provides: <-
-> an *application abstraction*, a *drawing and layout language*, <-
-> a simple *text editor*, and *attribute theme* support <-

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
-> Express your UI in a *purely-functional, declarative style*: <-

~~~

  ui = withAttr "header" $
       str "This is the header"

~~~

-------------------------------------------------

-> Why brick? <-
================

-> Use *high-level combinators* to create complex layouts: <-

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

-> Use *high-level combinators* to create complex layouts: <-

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

-> Use *high-level combinators* to create complex layouts: <-

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

-> Use *high-level combinators* to create complex layouts: <-

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

-> Avoid dealing with *terminal size math*: <-

~~~

  ui = fill ' ' <+> str "Right-justified"

~~~

~~~

                                                Right-justified

~~~

-------------------------------------------------

-> Why brick? <-
================

-> Use dynamic *attribute themes*: <-

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
- Provide initial *application state*
^
- Provide *drawing function* to *draw your state*
^
- Provide *event handling function* to *transform your state*
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
