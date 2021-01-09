---
layout: post
title: Start of Documentation
published: true
---
<h1 class="doc-title">Introduction</h1>

- [Impulse](#impulse)
- [Why Impulse](#why-impulse)

<a name="impulse"><</a>

<a name="why-impulse"></a>

Views in Impulse are almost as simple as HTML but very powerful. A view encapsulates a set of components which will be later converted by the Impulse js engine to HTML. The following example is a minimal example with just a simple text output.

<div>
  <div class="code-header">
    <div class="container-fluid">
        <div class="row">
            <div class="button red" />
          	<div class="button yellow" />
          	<div class="button green" />
        </div>
    </div>
  </div>
  <pre class="code-white line-numbers language-markup">
      <code class="language-markup">&lt;impulse&gt;
          &lt;window&gt;
              Hello world
          &lt;/window&gt;
      &lt;/impulse&gt;</code>
  </pre>
</div>
 
A view must start with an opening impulse tag and must be closed with a closing impulse tag.