# Minimap package [![Build Status](https://travis-ci.org/atom-minimap/minimap.svg?branch=master)](https://travis-ci.org/atom-minimap/minimap)

A preview of the full source code.

![Minimap Screenshot](https://github.com/atom-minimap/minimap/blob/master/resources/screenshot.png?raw=true)

### Installation

```
apm install minimap
```

### Features

* Service-based Plugin API: Use the plugin generation command and start developing your plugin right away.
* Decoration API: Use the same API to manage `TextEditor` and `Minimap` decorations.
* Canvas-based Rendering: Simple, fast and flexible.
* Stand-alone Mode: Wants to display a preview of a text editor in your UIs, use a stand-alone version of the Minimap.

### Available Plugins

Below is the list of available plugins so far:

Package|Description
---|---
[Auto-Hide](https://atom.io/packages/minimap-autohide)|Hides the Minimap while editing.
[Bookmarks](https://atom.io/packages/minimap-bookmarks)|Displays Atom bookmarks.
[Code Glance](https://atom.io/packages/minimap-codeglance)|Shows the code that's under the mouse cursor when hovering the Minimap.
[Find And Replace](https://atom.io/packages/minimap-find-and-replace)|Displays the search matches.
[Git Diff](https://atom.io/packages/minimap-git-diff)|Displays the file diff.
[Hide on inactive panes](https://atom.io/packages/minimap-hide)|Hide the Minimap when pane isn't focus.
[Highlight Selected](https://atom.io/packages/minimap-highlight-selected)|A Minimap binding for the highlight-selected package.
[Linter](https://atom.io/packages/minimap-linter)|Displays linter markers.
[Pigments](https://atom.io/packages/minimap-pigments)|Displays the pigments colors.
[Selection](https://atom.io/packages/minimap-selection)|Display the buffer's selections.

### Settings

Setting|Description
---|---
`Auto Toggle`|If checked the Minimap is toggled on at startup. (default=true)
`Display Code Highlights`|If checked the code will be highlighted using the grammar tokens. (default=true)
`Display Minimap On Left`|If checked the Minimap appears on the left side of editors, otherwise it appears on the right side. (default=false)
`Char Height`|The height of a character in the Minimap in pixels. (default=2)
`Char Width`|The width of a character in the Minimap in pixels. (default=1)
`Interline`|The space between lines in the Minimap in pixels. (default=1)
`Text Opacity`|The opacity used to render the line text in the Minimap. (default=0.6)
`Display Plugins Controls`|If checked, the Minimap plugins can be activated/deactivated from the Minimap settings view and a quick settings dropdown will be available on the top right corner of the Minimap. **You need to restart Atom for this setting to be effective.**(default=true)
`Minimap Scroll Indicator`|Toggles the display of a side line showing which part of the buffer is currently displayed by the Minimap. The side line appear only if the Minimap height is bigger than the editor view height. (default=true)
`Plugins *`|When plugins are installed, a setting is created for each to enable/disable them directly from the Minimap settings view.
`Scroll Animation`|Enable animations when scrolling the editor by clicking on the Minimap. (default=false)
`Scroll Animation Duration`|Duration of the scroll animation when clicking on the Minimap. (default=300)
`Use Hardware Acceleration`|If checked the Minimap scroll is done using a `translate3d` transform, otherwise the `translate` transform is used. (default=true)
`Absolute Mode`|When enabled the Minimap uses an absolute positioning, letting the editor's content flow below the Minimap.(default=true) <p>Note that this setting will do nothing if `Display Minimap On Left` is also enabled.</p>

### Key Bindings

The Minimap package doesn't provide any default keybindings. But you can define your own as demonstrated below:

```cson
'atom-workspace':
  'cmd-m': 'minimap:toggle'
  'ctrl-alt-cmd-j': 'minimap:generate-javascript-plugin'
  'ctrl-alt-cmd-b': 'minimap:generate-babel-plugin'
  'ctrl-alt-cmd-c': 'minimap:generate-coffee-plugin'
```

### Tweaking The Minimap

#### Hiding scrollbars

If you want to hide the default editor scrollbar, edit your `style.less` (Open Your Stylesheet) and use the following snippet:

```css
atom-text-editor .vertical-scrollbar,
atom-text-editor::shadow .vertical-scrollbar {
  opacity: 0;
  width: 0;
}
```

#### Changing the Minimap's background

![minimap-custom-background](https://github.com/atom-minimap/minimap/blob/master/resources/minimap-custom-background.png?raw=true)

```css
atom-text-editor atom-text-editor-minimap,
atom-text-editor::shadow atom-text-editor-minimap {
  background: green;
}
```

#### Changing the color of the Minimap's `visible-area`

![minimap-custom-background](https://github.com/atom-minimap/minimap/blob/master/resources/minimap-custom-visible-area.png?raw=true)

```css
atom-text-editor atom-text-editor-minimap::shadow .minimap-visible-area,
atom-text-editor::shadow atom-text-editor-minimap::shadow .minimap-visible-area {
  background-color: green;
  opacity: .5;
}
```

#### Changing the color of the Minimap's `scroll-indicator`

![minimap-custom-background](https://github.com/atom-minimap/minimap/blob/master/resources/minimap-custom-scroll-indicator.png?raw=true)

```css
atom-text-editor atom-text-editor-minimap::shadow .minimap-scroll-indicator,
atom-text-editor::shadow atom-text-editor-minimap::shadow .minimap-scroll-indicator {
  background-color: green;
}
```

#### Disabling mouse interactions when in absolute mode

If you want to prevent to catch the mouse pointer when the `absoluteMode` setting is enabled you can use the following snippet to do so:

```css
atom-text-editor atom-text-editor-minimap,
atom-text-editor::shadow atom-text-editor-minimap {
  pointer-events: none;
}

atom-text-editor atom-text-editor-minimap::shadow .minimap-visible-area,
atom-text-editor::shadow atom-text-editor-minimap::shadow .minimap-visible-area {
  pointer-events: auto;
}
```

The visible area will still allow interaction but the Minimap track won't.

----

## Developpers Documentation

### Plugins

#### Plugin Generation Command

Use the `Generate Javascript Plugin`, `Generate Coffee Plugin` or `Generate Babel Plugin` commands, available in the command palette, to generate a new Minimap plugin package.

- `Minimap: Generate Javascript Plugin`: Will generate a vanilla JavaScript package.
- `Minimap: Generate Coffee Plugin`: Will generate a CoffeeScript package.
- `Minimap: Generate Babel Plugin`: Will generate a ES6 package that uses babel-js.

#### Plugins Controls

When the `displayPluginsControls` setting is toggled on, plugins activation can be managed directly from the Minimap package settings or by using the quick settings dropdown available on the Mimimap itself:

![Minimap Screenshot](https://github.com/atom-minimap/minimap/blob/master/resources/plugins-list.gif?raw=true)

### Minimap Decorations

The Minimap package mimic the decoration API available on editors so that you can easily add your own decorations on the Minimap.

While the interface is the same, some details such as the available decorations types change relatively to the editor's decorations API.

#### Scope And Styling

The most important change is that decorations on the Minimap doesn't use a `class`, but rather a `scope`

```coffee
minimapView.decorateMarker(marker, type: 'line', scope: '.scope .to .the.marker.style')
```

It's still possible to pass a class parameter to the decoration:


```coffee
minimapView.decorateMarker(marker, type: 'line', class: 'the marker style')
```

In that case, when rendering the decoration a scope will be build that will look like `.minimap .editor .the.marker.style`.

The reason of using a scope rather than a class is that while editor's decorations are part of the DOM and benefit of the styles cascading, Minimap's decorations, rendered in a canvas, do not. In order to work around that, decoration's styles are defined using a `scope` property containing the selector allowing to retrieve the decoration style.

This allow the Minimap decorations to still be styled using css. For instance, the scope used by the `minimap-selection` package is:

```css
.minimap .editor .selection .region {
  /* ... */
}
```

Note that the scope is prefixed with `.minimap` so that you can override the selection style in the Minimap without impacting the editor's one.

Also note that only the `background` property will be retrieved to style a decoration.

A last option is to pass a css color directly in a `color` option, such as:

```coffee
minimapView.decorateMarker(marker, type: 'line', color: '#ff0000')
```

In that case neither the scope nor the class will be used.

#### Decorations Types

Another non-trivial change is the list of available decoration's type. At the time, the available types on the Minimap are:

- `line`: Same as the editor one, it colors the line background with a color extracted from the decoration scope.
- `highlight-under`: Correspond to an editor `highlight` decoration that is rendered before rendering the line content.
- `highlight-over`, `highlight`: Correspond to an editor `highlight` decoration that is rendered after having rendered the line content.
- `highlight-outline`: Correspond to an editor `highlight` decoration that is rendered only as an outline in the Minimap.

### License

MIT
