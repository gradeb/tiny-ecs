# TinyECS

[![Build Status](https://travis-ci.org/bvalosek/tiny-ecs.png?branch=master)](https://travis-ci.org/bvalosek/tiny-ecs)
[![NPM version](https://badge.fury.io/js/tiny-ecs.png)](http://badge.fury.io/js/tiny-ecs)

A mean lean Entity-Component-System library.

[![browser support](https://ci.testling.com/bvalosek/tiny-ecs.png)](https://ci.testling.com/bvalosek/tiny-ecs)

## Installation

Works on the server or the browser (via [Browserify](http://browserify.org)):

```
npm install tiny-ecs
```

## Usage

Manage entities via an `EntityManager` instance:

```javascript
var EntityManager = require('tiny-ecs').EntityManager;

var entities = new EntityManager();
```

## Creating Entities

Create an entity:

```javascript
var hero = entities.createEntity();
```

### Working with Entities

Components are added via providing any generic constructor function:

```javascript
function Position()
{
  this.x = 0;
  this.y = 0;
}
```

```javascript
function Sprite()
{
  this.image = 'hero.png';
}
```

Add the components:

```javascript
hero.addComponent(Position).addComponent(Sprite);
```

We now have new data members on our entity for the components. TinyECS will add
an instance member that is the name of the component constructor, lowercased.

```javascript
hero.position.x = 10;
hero.sprite.image === 'hero.png'; // true
```

Add arbitrary text tags to an entity:

```javascript
hero.addTag('player');
```

You can also remove components and tags in much the same way:

```javascript
hero.removeComponent(Sprite);
hero.removeTag('player');
```

To determine if an entity has a specific component:

```javascript
if (hero.hasComponent(Position)) { ... }
```

And to check if an entity has ALL of a set of components:

```javascript
if (hero.hasComponents([Position, Sprite])) { ... }
```

### Querying Entities

The entity manager is setup with indexed queries, allowing extremely fast
querying of the current entities. Querying entities returns an array of
entities.

Get all entities that have a specific set of components:

```javascript
var toDraw = entities.queryComponents([Position, Sprite]);
```

Get all entities with a certain tag:

```javascript
var enemies = entities.queryTag('enemy');
```

### Removing Entities

Directly:

```javascript
hero.remove();
```

Via the manager:

```javascript
entities.remove(hero);
```

### Using a Factory Function

You can pass in an optional factory function to the `EntityManager` with the
signature `Function(T: TypeToCreate): Object`. This function will be used to create
all components and entities, allowing you to wire in some sort of dependency
injector if needed (such as [Sack](http://github.com/bvalosek/sack)), or object
pooling.

```javascript
var entities = new EntityManager(function(T) {
  return container.make(T);
});
```

The factory will be called with `T === Entity`:

```javascript
var hero = entities.createEntity();
```

The factory will be called with `T === Position`:

```javascript
hero.addComponent(Position);
```

## Tern Support

The source files are all decorated with [JSDoc3](http://usejsdoc.org/)-style
annotations that work great with the [Tern](http://ternjs.net/) code inference
system. Combined with the Node plugin (see this project's `.tern-project`
file), you can have intelligent autocomplete for methods in this library.

## Testing

Testing is done with [Tape](http://github.com/substack/tape) and can be run
with the command `npm test`.

Automated CI cross-browser testing is provided by
[Testling](http://ci.testling.com/bvalosek/tiny-ecs).


## License
Copyright 2014 Brandon Valosek

**TinyECS** is released under the MIT license.


