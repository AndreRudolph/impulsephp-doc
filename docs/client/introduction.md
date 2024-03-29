<h3 class="doc-title">Introduction</h3>

- [Introduction](#introduction)
- [Techologies](#technologies)
    - [NPM](#npm)
	- [Typescript](#typescript)
    - [SCSS](#scss)
- [Frameworks](#frameworks)
	- [React](#react)
    - [Bootstrap](#bootstrap)
    - [webpack (encore)](#webpack)
- [Installation](#installation)
	- [Quickstart](#quickstart)

<h4><a id="introduction">Introduction</a></h4>

Managing complex applications becomes tricky when dealing with assets like typescript, sass / scss and static resources like images or media in general. To reduce this complexity, the Impulse PHP Framework takes advantage of using webpack as a module builder to compile, minify and collect all required assets together.

Luckily the Symfony framework already provides the awesome WebpackEncoreBundle that wraps webpack by providing a handy facade for its configuration.

Once the Impulse framework has been initialized, the original webpack.config.js had been replaced and all files in the assets/ directory had been removed.


Though you can create awesome and complex javascript based web applications with the Impulse PHP Framework by using pure PHP, you might face situations that need custom javascript code.

This articles purpose is to give you a brief overview of used client side technologies and frameworks that are used by the Impulse framework.

<h4><a id="technologies">Technologies</a></h4>

<h5><a id="npm"></a>NPM</h5>
NPM is a package manager that keeps track of thousands of different npm packages. The Impulse PHP Framework typescript and scss source files are independent npm packages that can be installed to your project.

<h5><a id="typescript">Typescript</a></h5>
Typescript is a well known and established industry standard when it comes to reliable implementation and maintenance of javascript applications. It can be transpiled to javascript by using a transpiler and offers much more features than javascript which makes the development process more clean, fast and predictable: Types, generics, interfaces, etc.

<h5><a id="scss">SCSS</a></h5>
SCSS is a great tool to organize the work with stylesheets by providing variables, mixins, includes and of course nesting css rules with syntactic sugar.

<h4><a id="frameworks">Frameworks</a></h4>

<h5><a id="react">React</a></h5>

<h4><a id="installation">Installation</a></h4>

<h5><a id="quickstart">Quickstart</a></h5>
If you prefer a simple setup with no need of compiling impulse sources and creating your own components you may proceed with this quickstart section.

Every release of the Impulse framework provides already compiled versions of the client app as well as the different stylesheet themes that you can directly include.