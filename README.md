[![Stories in Ready](https://badge.waffle.io/undefined-NaN/codename-iggy.png?label=ready&title=Ready)](https://waffle.io/undefined-NaN/codename-iggy)
# codename-iggy

#mind:seal

BackEnd: MongoDB
model- /server/models/userData.js
db test and setup- test-data.js
Refer to the above test file to look at sample data being sent from the client side to the backend and play with the native driver queries to the mongo database.





#The Mithril Parts

##index.html
The logic flow starts at index.html. In mithril, the browser
just loads a large collection of environment variables to start with.
None of these variables in these scripts are drawn on the screen until 
they are mounted by m.routes, in routes.js, which is the last file 
loaded.

Other than bootstrap and framework dependencies, the focus for a 
developer should be on the script tags in the body.

##storageGetSet.js
This file sets some basic functions that allow us to interact with
localStorage. It's also a kind of 'helper' file. 

##devMode.js
This file is for developers only! It sets some basic data into local 
storage for you to manipulate (dummy decks, some sane default values, 
etc.). It prints instructions on its use to the console.log, and will 
alert you before it runs. You can turn it off by setting 'var devMode = 
false' on line 41 in index.html.

NOTE: you cannot run inline scripts in chrome extensions, so you will 
have to put this variable into the storageGetSet or devMode file if 
you want these set while working on a chrome extension and haven't 
loaded them into your local storage already in a way that is accessible 
to the extension.

##app.js
This is the first Mithril module. Mithril modules are objects that
contain a view property and an optional controller property, which are 
both loaded by Mithril's m.route at the appropriate time.

##Deck.js
This is the Deck model. These models are a little messy. Essentially,
they contain object templates, and server calls. This is also true for 
the next file,

##Card.js
Notice in particular Card.vm. When a new card is generated elsewhere, 
it is generated by calling this generator. It allows any values to be 
passed in to allow flexible expansion, but in practice only expects an
object with a "front" and "back" property.

##Components
All the rest, other than router.js (and then bootstrap's files), are
components--view and controllers and variables for each view.

##newDeck.js 
This is the component for the "New Deck" page, which you get routed to
by clicked the "New Deck" button on the "home" (default) page.

##deckDash.js
This is the component that is loaded when you click any deck's button  
on the 'home' screen (the default landing page). Notice that the 
controller gets passed an object that contains variables that are
assigned and then utilized by the view. These variables are passed
from the routes.js m.route() call. 

##addCards.js
This is the page that is reached by clicking on 'add cards' from any
deck's dash. It also is passed in variables that let it know which deck
you want it to be adding to.

##settings.js
This is the module for the settings page. It has one settings set up
as a proof of concept. It modifies a variable in the model by calling
a model function designed for that purpose. That model function also
writes that changed settings to the App.mindSeal() object, and then
uses the mindSealSet() function to set it to local storage. If more
settings are added, they should probably follow this model.

##about.js
Not much to this page. 

##viewDeck.js
This is the most complicated page, and it has problems. Ideally, it 
would be rewritten, as the bugs it experiences (see below) are likely a 
result of it not being written in a way mithril expects.

It has params passed in by the router, as is common. And then notice 
that the controller is passed in as a parameter/argument to the view 
when the view is called (Mithril always calls the controller before it
calls the view). 

The chain of functions is a little complex to show and hide pieces of
the page. When the "show" button is pressed, a function is run that
sets a variable to rendered DOM elements, and toggles a boolean. That 
same function will be called again when one of the newly displayed 
buttons is pressed, but will hide, instead of show, the back of the 
next card, based on that boolean--and then toggle it again.

There is a 'next card' function that gets run every other time, 
and it also handles deciding if there are any more cards to display.
If there aren't any more cards, it will show assign a variable in the 
view to DOM elements for 'completed'. A critical feature would be
very simple here: add in a condition that checks if that card's 
'to be seen' property indicates a date that has not yet passed, 
and respond accordingly. First, though, the 'rate' function should
be tweaked to set those dates correctly.

known bugs:

moment.js stuff used to work, but stopped working after updating the 
card viewmodel and using it. I never got around to restoring that 
functionality. That means the view will always show every

also, for some reason, when viewing a deck, then going back to home,
selecting a new deck, and then viewing it--but only sometimes--you
will have to refresh to get the card to show.

This same effect is produced after adding new cards. You will see an
empty card; if that happens, refreshing the page then shows the card.

##home.js
The default component. Renders a DOM element button for every deck, puts 
them in an array, and then sets that array to a variable that is loaded
in the view.

##routes.js
this is where components get mounted. Take some time to do some reading
here to understand the flow of information. Notice that Mithril will
load a view and controller, will load the controller first, and will
allow the output of the controller being called/constructed to be passed
in to the view being called here as an argument.

-----

Other future development:
The advanced algorithm would adjust the cScale on a particular card to be
more or less agressive based on their responses, essentially making the
scale self correcting. This shouldn't be too hard to implement, and would 
go in viewDeck's rate function.
