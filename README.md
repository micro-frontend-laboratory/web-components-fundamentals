# web-components-fundamentals

> **Part 1 of 5** in [Micro-Frontend Laboratory](https://github.com/micro-frontend-laboratory)
> · [polyglot-microfrontends](https://github.com/micro-frontend-laboratory/polyglot-microfrontends) →

**Custom elements with nothing but the browser. A class, a tag, attributes in, events out.**

```js
class UserCard extends HTMLElement {
  static observedAttributes = ['name'];
  attributeChangedCallback() {
    this.#name.textContent = this.getAttribute('name');
  }
}
customElements.define('user-card', UserCard);
```

[`apps/basics/web-components-basics`](apps/basics/web-components-basics) holds three components,
each a single plain JavaScript file with no framework and no build step:

| Component | Shows |
| --- | --- |
| `<user-card name role>` | An attribute in: `observedAttributes` and `attributeChangedCallback` |
| `<counter-button label start>` | An event out: `CustomEvent('count-changed', { detail: { count } })` |
| `<todo-list heading>` | A public method the page calls: `todoList.addItem('Milk')` |

The guide, [docs/guides/web-components-basics.md](docs/guides/web-components-basics.md), walks
through the pattern they share.

## Run it

You only need [Docker Desktop](https://www.docker.com/products/docker-desktop/).

```bash
git clone https://github.com/micro-frontend-laboratory/web-components-fundamentals.git
cd web-components-fundamentals
docker compose -f compose.basics.yaml up --build --detach --wait
```

Open `http://localhost:4100`. On Windows, `tools\scripts\basics\deploy-web-components-basics.bat`
rebuilds and restarts the page.

## Repository layout

```text
apps/
  basics/
    web-components-basics/  the three components and the page, served by nginx
compose.basics.yaml         the shop-basics group, published on 4100
tools/scripts/basics/       Windows shortcut around docker compose
docs/guides/                how the components are built
```

## Next

The same contract, attributes and properties in and `CustomEvent`s out, is all that Angular,
React and Vue need to put their components on one page.
[polyglot-microfrontends](https://github.com/micro-frontend-laboratory/polyglot-microfrontends)
shows it with three micro-frontends and three shells.
