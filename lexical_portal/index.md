# User guide to ANEE lexical portal of Akkadian

## The main view

![Main portal view](full_view.png)

The main view of the lexical portal is a a big blur of colored dots. Each dot represents a word, and words are connected with each other via *arcs*. Arcs can vary in strength, and they are what cause some words to appear close together.

### Selected and highlighted words

If you zoom into the view, you will see distinct words with text hovering over the dots. Hovering on a word causes it to be highlighted, making it and its linked words stand out. Clicking on a word causes it to become *selected*; it becomes highlighted, and words that are not connected to it are hidden. A sidebar with additional information and links will also appear. If you click on an empty part of the graph, the word is deselected, and you see all the words again.

![Zoomed portal view](zoomed.png)

The highlighted word has its translation in parentheses; in this Akkadian-English version, it's the English translation.

### Hide edges

When all words are active and you're in a populated part of the graph, it can look very cluttered and render slowly. You can hide the edges by toggling the "hide edges" button, as shown here:

![Showing edges](show_edges.png)

![Hiding edges](hide_edges.png)

### Clusters

When a group of words cluster together, a program assigns a color to them. This color has absolutely no predetermined linguistic significance, although in many cases clusters are suggestive of shared typical usage. For example, in the current (as of December 2021) rendering, yellow dots to the right of the graph have many nouns related to activity, occupation and social position (*mayor*, *rider*, *brewer*, *lady#house*), some proper nouns, and some verbs. Other cluster may have predominately proper nouns, or predominately verbs.

![A cluster of words](yellow.png)

### Filtering on clusters

If you'd like to get a view of just one cluster (in this case, indicated by colour), you can do that selecting a word in that cluster (whereupon only words connected to that word, but from any cluster, remain), then deselecting it by clicking on an empty spot, and finally clicking on "Toggle filter on this colour" in the sidepanel.

<video width="900" controls>
  <source src="filter.mp4" type="video/mp4">
</video>

## Searching

While the graph may be zoomed in using either the mouse wheel or the slider on the bottom left of the page, the main way to navigate is via the autocompleting search bar at the top. The autocompletion generates a pop-up menu, with two listings: those words ("Nodes") which contain the search term as a substring, and those words whose *translations* contain the search term as a substring. Thus, if you are browsing the Akkadian-English portal, and type *horse*, you will find Akkadian words whose translations contain "horse", such as *sisû* and *tarbaṣu* under the *Translations* heading. The autocomplete normalises to the Latin alphabet as used in English, so you can write "tarbasu" and find find *tarbaṣu* that way.

<video width="900" controls>
  <source src="search.mp4" type="video/mp4">
</video>

## The sidebar

The sidebar shows information for the most recently active word, or if a word hasn't been activated yet, it's hidden. You can also hide or show it by clicking on the orange arrows poking out from it.

### Linked words

Under the headings "Linked words" and "Linked proper nouns" you will find anchor links. Clicking these activates a new word and centers the view on that word.

### Browsing history

As you navigate along the linked words (or otherwise), you may want to get back to where you were before. This can be done using the forward and backward arrows at the top of the side panel. If there is browseable history in either direction, the arrow is blue, otherwise it is black.

### Concordances

Each word is linked to a corpus search on The Language Bank of Finland's Korp search engine. Click on "Search in Korp", and a concordance view for the word will appear (allow some time for the search to be completed). This search view contains a great deal of additional information about the matching texts.

### Ego graphs

When a word is active, its linked words may be all over the main graph, making it hard to get an overview. A condensed view may be accessed by clicking on "Go to this word's ego graph". This will cause the browser to navigate to a new page, so you will have to click the browser's back button to return to the previous view.


## Downloading the data

[TODO]

## About the data pipeline

[TODO]
