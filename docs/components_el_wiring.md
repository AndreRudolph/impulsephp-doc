Creating more complex components should be as easy as possible without breaking any conventions of the Framework. Whenever you face the issue of wiring components from the same component tree, you can do this easily via the **_Wire_** annotation. 

You can define the component(s) that will be wired by using symfonys awesome expression language. Impulse extends the expression language by adding four different functions to evaluate candidates for the expression match:

- instanceOf
- id()
- uid()
- component(attribute)
