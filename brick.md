%title:
%author: J. Daugherty
%date: 2016-10-25





-> Painless Console Programming <-
-> with *brick* <-



-> for intermediate Haskell programmers <-

-------------------------------------------------




-> Programming terminals is hard! <-


^
-> High-level abstraction is needed. <-

-------------------------------------------------

~~~

  ui = str "Some text"

~~~

~~~

  Some text

~~~

-------------------------------------------------

~~~

  ui = hCenter $ str "Some text"

~~~

~~~

                           Some text

~~~

-------------------------------------------------

~~~

  ui = (hCenter $ str "Some text")
       <=> hBorder

~~~

~~~


                           Some text
   ──────────────────────────────────────────────────────


~~~

-------------------------------------------------

~~~

  ui = (hCenter $ str "Some text")
       <=> hBorder
       <=> (str "Some more text")

~~~

~~~


                           Some text
   ──────────────────────────────────────────────────────
   Down below (long line) 


~~~

-------------------------------------------------

~~~

  ui = border $
       (hCenter $ str "Some text")
       <=> hBorder
       <=> (str "Some more text")

~~~

~~~

  ┌───────────────────────────────────────────────────────┐
  │                        Some text                      │
  │───────────────────────────────────────────────────────│
  │Down below (long line)                                 │
  └───────────────────────────────────────────────────────┘

~~~

-------------------------------------------------

~~~

  ui = withBorderStyle ascii $
       border $
       (hCenter $ str "Some text")
       <=> hBorder
       <=> (str "Some more text")

~~~

~~~

  +-------------------------------------------------------+
  |                        Some text                      |
  |-------------------------------------------------------|
  |Down below (long line)                                 |
  +-------------------------------------------------------+

~~~

-------------------------------------------------

~~~

  ui = withBorderStyle unicodeRounded $
       border $
       (hCenter $ str "Some text")
       <=> hBorder
       <=> (str "Some more text")

~~~

~~~

  ╭───────────────────────────────────────────────────────╮  
  │                        Some text                      │
  │───────────────────────────────────────────────────────│
  │Down below (long line)                                 │
  ╰───────────────────────────────────────────────────────╯

~~~

-------------------------------------------------

-> Brick Workflow <-
====================

~~~

   Draw          Await                    Handle
  Initial ─────> Event ─────────────────> Event ─────> Quit
   State           ^                      (I/O)
                   │                        │
                   │                        │
                   │                        │
                   └── Redraw <── Change <──┘
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
