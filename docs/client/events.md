<h3 class="doc-title">Client events</h3>

- [Introduction](#introduction)
- [Internal workflow](#internal-workflow)

<h4 name="introduction">Introduction</h4>

Components are useless without the usage of event listeners reacting to client events.
However, since React does only allow one event listener per event dealing with multiple
event handlers becomes tricky. It becomes even more tricky because of how the Impulse
framework works and its components are designed. 

Luckily, the framework provides an easy but yet very flexible and powerful approach for
registering event listeners.

<h4 name="internal-workflow">Internal workflow</h4>