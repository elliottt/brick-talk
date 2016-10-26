%title:
%author: J. Daugherty
%date: 2016-10-25









-> Painless Console Programming <-
-> with *brick* <-

-------------------------------------------------

-> Writing terminal applications is painful! <-
===============================================

^
-> We have to worry about ... <-
^

\ \ \ \ \ \ \ \ \ \ ... _interface layout_ and _terminal resizes_
^

\ \ \ \ \ \ \ \ \ \ ... _terminal attributes_
^

\ \ \ \ \ \ \ \ \ \ ... _terminal feature set_

-------------------------------------------------

-> Haskell to the rescue! <-
============================

^
-> vty <-
---------

- Curses-like library
- Provides "image" drawing interface
- Input event interface
- Supports Terminfo

^

-> brick <-
-----------

- Builds on _vty_
- Provides application abstraction
- Provides a drawing and layout language
- Includes a simple text editor
- Provides attribute themes

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


               
  ui = vBox [ hCenter $ str "Up above"
            , hBorder
            , str "Down below (long line)"
            ]

~~~

~~~

                          
          Up above        
   ────────────────────── 
   Down below (long line) 
                          

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

-> Avoid dealing with _brittle terminal size math_: <-

~~~

  ui = (fill ' ' <+> str "Right-justified") <=>
       (hCenter $ str "Centered")

~~~

~~~

                                             Right-justified  
                         Centered

~~~

-------------------------------------------------

-> Why brick? <-
================

-> Use _attribute themes_: <-

~~~

  ui = withDefAttr "someAttr" $
       str "White on a blue background"

  applicationTheme :: AttrMap
  applicationTheme = attrMap (bg blue)
    [ ("someAttr", fg white)
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

-------------------------------------------------

-> Brick Workflow <-
====================

~~~

   Draw           Await                    Handle
  Initial ------> Event -----------------> Event -----> Quit
   State            ^                        |
                    |                        |
                    |                        |
                    |                        |
                    \-- Redraw <-- Change <--/
                                   State

~~~

-------------------------------------------------

-> Example: increment/show a counter <-
=======================================

~~~

  initialState = 0

  drawMyApp c = [center $ str $ "Counter: " <> show c]

  handleEvent c (EvKey (KChar '+') []) = continue $ c + 1  
  handleEvent c (EvKey (KChar 'q') []) = halt       c
  handleEvent c _                      = continue   c

~~~

-------------------------------------------------

-> fin! <-

-> *Code, demos, documentation* <-
-> https://github.com/jtdaugherty/brick <-

-> *Releases* <-
-> http://hackage.haskell.org/package/brick <-

-> *Contact* <-
-> cygnus@foobox.com <-

-> Thanks! <-
