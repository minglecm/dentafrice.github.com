---
layout: post
title: "CoffeeScript Mixins"
description: "Introducing a new way to include shared functionality in CoffeeScript"
category: programming
tags: [coffeescript, mixins, multiple inheritance]
---
{% include JB/setup %}

Lately I've been working on an interesting project bringing mixin functionality to CoffeeScript.

Right now it's just a module that you can install as a Node package or you can copy over the source from the repo for usage on the web.

It behaves similar to the way that Ruby handles including a module. You use `include` and then an optional `included` callback is fired.

---

## Usage

### Initializing

    npm install coffeescript-mixins

    mixins = require 'coffeescript-mixins'
    mixins.bootstrap() # mixes include into Function.prototype

or for the **Web** ([GitHub](https://github.com/dentafrice/coffeescript-mixins)):

    https://github.com/dentafrice/coffeescript-mixins

    window.CoffeeScriptMixins.bootstrap() # mixes include into Function.prototype

### Using Mixins

It works by adding a function to Function.prototype called `include` that allows you to include another class (or object) into a CoffeeScript class.

You define a mixin just like a normal class:

    class Mixin
      sharedFunction: ->
        console.log 'heya'

You can then include it like so:

    class A
      @include Mixin

    a = new A()
    a.sharedFunction() # heya

You can even do some pretty cool stuff with CoffeeScript's `super`. Overriding a mixed in function and then calling `super` will call back up to the mixin (injects itself between the class and superclass):

    class B
      @include Mixin

      sharedFunction: ->
        console.log 'before..'
        super # actually calls Mixin#sharedFunction via __super__.

    b = new B()
    b.sharedFunction() # before.. > heya

---

## In Depth

At the most simple level all the `include` function does is copy functions over from a mixin to a class with a little bit of super magic.

CoffeeScript gives subclasses a magic `__super__` property that just points directly to your superclass and allows the usage of `super`.

Regardless of whether or not you actually have a superclass, when you use `@include` we make a copy of the super object to place the mixed in functions on.

The copy is so that we don't start shoving and overriding functions on your actual superclass (eg. inheriting from Backbone.View).

It then puts a copy on the extended super object as well as your prototype.

Putting it on the super object allows for you to override your copy and then use CoffeeScript's `super` to invoke the original mixed in function.

### Conflict Resolution

The last one always wins. So if you include two mixins that contain the same function name and get into the dreaded diamond of doom then the last one will be the one that will be copied over regardless of overrides or anything else.

### Prototypal Inheritance

Functions are simply copied over to the including class when using mixins.  You can't dynamically add functions to the mixin and then have those functions appear in those classes that have included it.

When inheriting from a class that has a function with the same name as an included function, the mixin will take precedence.  Example:

    class Parent
      foo: ->

    class Mixin
      foo: ->

    class Child extends Parent
      @include Mixin

    c = new Child()
    c.foo() 

`Mixin#foo` will get called as it's being included on the `Child` regardless of whether or not foo is a property somewhere up the prototype chain.

---

## Future

I'm looking to expand it in the future as well to allow you to specify which functions you want to import into the current class.

I'm also looking for a better way to do it on the Node side. Mixing functions into Function.prototype kind of defeats the purpose of exporting into modules.

Any ideas? Drop me a line: [me@caleb.io](mailto:me@caleb.io).