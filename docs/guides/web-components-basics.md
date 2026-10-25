# Web Components basics

`apps/basics/web-components-basics` shows the whole native API with three small components and
no framework. Each one is a single plain JavaScript file, served as it is: no build step.

| Component | Shows |
| --- | --- |
| `<user-card name role>` | `observedAttributes` and `attributeChangedCallback`: the page sets an attribute, the component re-renders |
| `<counter-button label start>` | A component telling the page something happened: `CustomEvent('count-changed', { detail: { count } })` |
| `<todo-list heading>` | A public method the page calls: `todoList.addItem('Milk')`; emits `item-added` |

`todo-list` takes `heading` rather than `title`, because `title` is a global HTML attribute and
would show as a tooltip over the whole component.

## The pattern

Every component does the same four things:

1. Build its DOM once in the constructor with `createElement`, and attach it to a Shadow Root.
2. Share one `CSSStyleSheet` between all instances through `adoptedStyleSheets`.
3. React to attribute changes in `attributeChangedCallback`.
4. Show values with `textContent`, so typed HTML is displayed, never run.

That is the whole contract of a custom element: attributes and properties go in, `CustomEvent`s
come out. Every framework can produce and consume it, which is what
[polyglot-microfrontends](https://github.com/micro-frontend-laboratory/polyglot-microfrontends)
builds on.

## Run it

```bash
docker compose -f compose.basics.yaml up --build --watch web-components-basics
```

Open `http://localhost:4100`.

## What to try

- Type a new name: `<user-card>` updates through an attribute and `attributeChangedCallback`.
- Click "Likes": the page hears the `count-changed` event and shows the new count.
- Click `Add "Milk" from the page`: the page calls the component's public method.
- Type `<img src=x onerror=alert(1)>` as the name: it shows as text. `textContent` cannot run code.
