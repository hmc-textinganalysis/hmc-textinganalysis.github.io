---
layout: post
title: Developer Guide
---

{{ Developer Guide }}
================

<p class="meta">23 July 2019</p>

LAST is a visual interface built using tkinter that uses the open source
python3 library spaCy to parse through documents and categorize named entities.
It is built out of a hierarchal system of classes which handles both backend
categorization as well as front end user display/functionality. The hierarchy is
as follows:

                                          top
                                           |
                                          LAST
                                      /          \
                 ConversationNavigator            RoleSelector
                   |                  \          /             \
               UtteranceRow       VerticalScrolledFrame       CanvasControl
             /     |        \                   |                   |        \
    Message  IphoneUtterance  AndroidUtterance  |           CanvasWrapper    Icon*
         |                                      |                             |
    MessageRow                          Scrolling_Area                    DndHandler
         |                                      |
    TokenLabel*                         Mousewheel_Support
          
                                                                          *connected

By file:

    last.py:      [top, LAST, ConversationNavigator, RoleSelector, UtteranceRow,
                   IphoneUtterance, AndroidUtterance, Message, MessageRow,
                   TokenLabel]
                   
    scrolling.py: [VerticalScrolledFrame, Scrolling_Area, Mousewheel_Support]
    
    dnd.py:       [CanvasControl, CanvasWrapper, Icon, DndHandler]
    

The general outline of each of those classes is as follows:
  - top:  Our call to initialize tkinter in our program. The root of everything.
  - LAST: The main tkinter frame which serves as the parent to and maintains all
          the different internal pieces. It loads and saves files, builds the
          conversation navigator and role selector, and initializes the change
          log.
  - RoleSelector: One of our two interactive mechanisms for quickly
          recategorizing named entities. All named entities as identified by
          spaCy are placed in the Other Entities category unless they are
          marked as a person, in which case they are placed in the Other People
          category. Each of these entities gets a label to represent it, placed
          in the appropriate box. Using drag and drop functionality found in
          dnd.py, icons may be passed between categories and snapped into place
          in the new list. This class interfaces most directly with the
          functionality of dnd.py.
  - ConversationNavigator: One of our two interactive mechanisms for quickly
          recategorizing named entities. All named entities as identified by
          spaCy are processed and added to the counter dictionary and marked
          with indices. Tokens are automatically grouped by our program, so all
          instances of "Charlie" will hold the same index unless split. This
          portion is modeled to resemble actual text conversations, and holds
          the different utterances as text boxes justified according to
          convention. It is broken up into several scrollable pages to make it
          a bit easier on our program to load, and has functionality to change
          pages.
  - UtteranceRow: The class built to manage positioning and alignment of
          messages. Mostly just a frame to hold onto things for the
          ConversationNavigator to iterate through. The utterances it holds are
          either Android or Apple depending on the file type.
  - IphoneUtterance: Instance of the utterance class to hold text metadata,
          built specifically to parse iphone files (.csv).
  - AndroidUtterance: Instance of the utterance class to hold text metadata,
          built specifically to parse android files (.xml).
  - Message: Frames to hold onto individual text messages.
  - MessageRow: Frames within the general Message frame which primarily exist
          to make sure that all of the text fits nicely.
  - TokenLabel: Class to recognize named entities as categorized labels. Every
          single word in the text conversation is used to construct one of these,
          such that any word could be categorized or recategorized by either
          spaCy or the user. It manages its own clustering, splitting, and
          (re)categorization.
  - VerticalScrolledFrame: The class which manages and facilitates scrolling
          in tkinter. Builds the scrolling area.
  - Scrolling_Area: The interior canvas of the vertical scrolled frame.
  - Mousewheel_Support: Helper class that ensures that the scrollbar has
          functionality.
  - CanvasControl: Class managed by the Role Selector, used to keep track of
          all of the different canvasWrapper objects and allow them to interface
          with one another so that icons can be passed between them.
  - CanvasWrapper: Helper class that maintains canvases to use in and interface
          with drag and drop. Handles both motion and attaching icons to the
          canvas. One per box in the role selector.
  - Icon: The label which may be dragged and dropped by the user. A simple box
          with text inside, which holds onto the corresponding tokenLabel.
  - DndHandler: The helper class that does the bulk of the work for the actual
          process of dragging and dropping. Detects motion, release, and
          communicates that information upwards back to the Icon.

There are two data structures passed between most, if not all, of the different
objects present in this program. The first is the canvas control object, found
in dnd.py. This is used both to allow for drag and drop functionality in the
role selector as well as keep track of those changes and communicate them to
the different moving parts in this program. This functionality is best
demonstrated in MessageRow.updateLabels(), which is a method constantly called
to check if any changes have been made. If there have (indicated by a change
made to the changeLabel touple as the result of dragging and dropping an icon),
then the required categorical changes are made and the CanvasControl.changeLabel
touple is reset to (none, none, none).

The other important structure to be aware of is the counter dictionary,
initialized by LAST. The counter dictionary holds two key components: a list of
the total number of tokens stored under the key "numElements", and lists of
tokens with identical text. The numElements count is used to generate unique
indices for all of the various tokens that are identified. The lists of tokens
are mapped to by keys with their display text on them, and those lists are used
to ensure synchrony in groupings.

Next steps for future developers:
  - Integrate the featureExtractor script to give users the option to
    automatically process their newly anonymized text threads and generate
    statistical data.
  - Expand available file types to handle Messenger, Line, WeChat, etc.
  - Figure out packaging (Professor Medero got it to work on her laptop with
    py2app, but we were unable to replicate). Also py2app only works to generate
    ios app bundles, so it would be worth it to search for other ways to make
    the program fully standalone (not .exe, that's not remotely standalone) on
    other operating systems.
