---
layout: post
title: User Guide
---

{{ User Guide }}
================

<p class="meta">23 July 2019</p>

Load your text thread of choice into LAST. Accepted file types: .xml, .csv

It will take a moment for your text thread to fully load as the program parses
through the text and makes its initial categorizations. It will do its best to
identify as many different named entities as it can. Those entities will be
names of individuals used in the text thread, as well as locations,
organizations, products, events, and even works of art. The program will
highlight those entities in the conversation navigator in the bottom half of the
screen, as well as create labels representing them in the role selector found in
the top half. The program will only place entities in either the "Other People"
or "Other Entities" category. It is up to the user to recategorize the names of
themselves and their partner. Do so by either dragging and dropping labels or
clicking the highlighted words to shift their categorizations. Right click
labels and select 'delete' to quickly remove misidentified words. Scroll through
all of the pages of the conversation navigator to ensure that nothing got missed
or miscategorized. In case of duplicate words that belong in different contexts,
right click the label in the role selector and select 'split' to break it into
unique instances. You may also select 'join' to rejoin labels if you change your
mind about splitting. Using the provided unique indices as reference, categorize as
finely as you like. Once satisfied, select save from the menu. The program will
process your file and produce a .csv file with all instances of identified,
categorized words replaced with placeholders indicating category. Additionally,
it will build a changelog that maps all manual changes made by the user. This
does not sort by word or index, but rather by character offset, an internal
mechanism maintained by spaCy, the nlp library. This may be used to track
overall effectiveness of our internal models (fewer changes means a better
adapted model) without deanonymizing the output file and thus invalidating the
program's work.

Role Selector:
Occupying the top half of the program, the role selector consists of four colored
boxes indicating each of the major four categories (Participant, Partner, Other
Person, and Other Entities). When files are loaded into
the program, LAST parses through the file categorizes as many named
entities as it sees. Those entities are placed in either of the two "other"
categories based off of our natural language processing library's estimates.
From there, the user may drag and drop these labels back and forth between the
four categories to recategorize them. They may also right click labels and
select 'delete' to quickly and efficiently delete labels associated with words
mistakenly identified as named entities. In cases where context may change
the categorization of a word, right click labels and click 'split' to split them
unique labels for each instance. Each label will now only be associated with the
into one instance of the word whose index matches. Dragging and dropping a split
label will only update one word at a time. Words with only one instance cannot
be split. If you change your mind about splitting tokens, select join to regroup them.
Any changes made up top in the role selector are reflected down below
in the conversation navigator.

Conversation Navigator:
Set in the bottom half of the program, the conversation navigator is a visual
representation of the conversation that has been loaded in. It is oriented
according to convention, with the participant's text boxes justified right, and
their conversational partner's text boxed justified left. When files are loaded
into the program, LAST does a first pass to categorize as many named
entities as it sees. Those entities are placed in either of the two "other"
categories based off of our natural language processing library's estimates.
Words in the conversation navigator are highlighted in colors corresponding to
those categories. Clicking a word shifts its categorization by one category. The
order of categories is as follows: Participant, Partner, Other Person, Other
Entity, None. Clicking a word categorized as Participant shifts it to the
Partner category, and so on and so forth. Clicking a word identified as an Other
Entity decategorizes it, and clicking a word not placed in one of the four major
categories places it in the Participant category. If the word is grouped with
others (all sharing the same label), then clicking one updates the categorization
of all. If instances of words are split into unique labels by the user in the
role selector, then clicking a label only updates that unique instance. Any
changes made in the conversation navigator are reflected above in the role
selector.
