---
layout: post
title: Views
published: true
---

## Views

Views in Impulse are simple to understand but very powerful. A view encapsulates a set of components which will be later converted to HTML in the browser. The following example is a minimal example with just a simple text output.

-- example with only label

A view must start with an opening impulse tag and must be closed with a closing impulse tag.

### Custom attributes
Each component within a view can be extended by custom attributes. These attributes may be used to customize special components for e.g. specific processing with the component.

-- example with custom attributes

### Nesting views

The framework also provides a possibility to nest views:

-- example with view nesting

### Applying controllers to views
In the next documentation section for controllers you will learn how to connect a view with a controller.
