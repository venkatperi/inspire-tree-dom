# Inspire Tree DOM

[![Join the chat at https://gitter.im/helion3/inspire-tree](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/helion3/inspire-tree?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

[![Build Status](https://travis-ci.org/helion3/inspire-tree-dom.svg?branch=master)](https://travis-ci.org/helion3/inspire-tree-dom)

Inspire Tree DOM is the DOM rendering engine for [Inspire Tree](https://github.com/helion3/inspire-tree).

InspireTree is required.

- [Website & Demos](http://www.inspire-tree.com/)
- [Changelog](https://github.com/helion3/inspire-tree-dom/CHANGELOG.md)

### Features:

- Virtual DOM (powered by Inferno) for blazing fast change rendering.
- Valid HTML structure.
- Clean and easy-to-override CSS.
- Modular SCSS for custom compilation.
- Keyboard navigation.
- Drag and Drop between tree instances (optional).

### Installation

- Yarn: `yarn install --save-dev inspire-tree-dom` or
- NPM `npm install --save-dev inspire-tree-dom`

## Usage

First, you need a valid instance of InspireTree, then you pass it and a target DOM element to InspireTreeDOM:

```js
var tree = new InspireTree({
    data: [{
        text: 'A node'
    }]
});

new InspireTreeDOM(tree, {
    target: '.tree'
});
```

## DOM Configuration

- **autoLoadMore** - Automatically triggers "Load More" links on scroll. Used with deferrals.
- **deferredRendering** - Only render nodes as the user clicks to display more. (See "Deferrals" section below.)
- **dragTargets** - Array of other tree elements which accept drag/drop.
- **nodeHeight** - Height (in pixels) of your nodes. Used with deferrals, if `pagination.limit` not provided.
- **showCheckboxes** - Show checkbox inputs.
- **tabindex** - Define a tab index for the tree container (used for key nav).
- **target** - An Element, selector, or jQuery object.

#### Event List

- **node.click** - `(Event event, TreeNode node)` - User clicked node.
- **node.contextmenu** - `(Event event, TreeNode node)` - User right-clicked node.
- **node.dblclick** - `(Event event, TreeNode node)` - User double-clicked node.
- **node.dropin** - `(TreeNode node)` - Tree has received a new node via drop.
- **node.dropout** - `(TreeNode node), (Element elem)` - Node dropped into a valid target.

#### Overriding DOM Events

In rare cases, you may need to override our default DOM event handlers. To assist with this, those events provide a `preventTreeDefault` method.

```js
tree.on('node.click', function(event, node) {
    event.preventTreeDefault(); // Cancels default listener
});
```

In these cases, it will be up to you to ensure any further logic has been implemented.

However, the original handler is passed as an argument, which still allows you to execute it when you're ready.

```js
tree.on('node.click', function(event, node, handler) {
    event.preventTreeDefault(); // Cancels default listener
    // do some custom logic
    handler(); // call the original tree logic
});
```

Only DOM-based events support this: *node.click, node.dblclick, node.contextmenu*

### Deferred Rendering

Deferred Rendering progressively renders loaded nodes as the user scrolls or clicks a Load More link.

To work properly, you need to enable `deferredRendering` in the configuration.

A "Load More" link will show at the bottom of each section which has more nodes than are initially allowed.
