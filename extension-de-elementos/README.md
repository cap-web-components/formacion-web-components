# Extensión de elementos

Por definición los componentes son extensibles. Ahora bien, a la hora de extender un componente, tenemos que diferenciar entre si quremos diferenciar un componente HTML nativo o un custom element.

## Extensión de elmentos HTML nativos

Utilizando las APIs del navegador podemos extender los componentes bases que nos proporciona. De hecho, esta es considerada un buena práctica para el desarrollo de [Progresive Web Apps](https://web.dev/progressive-web-apps/) ya que partimos de una base que extendemos a medida que el navegador carga nuestros elementos.

```javascript
class MyCustomInput extends HTMLInputElement {

  constructor() {
    super();
    // Añadidmos lógica adicional al input nativo
  }

}

customElements.define('my-custom-input', MyCustomInput, { extends: 'input' });
```

A la hora de definir nuestra etiqueta, a diferencia de los custom elements, tenemos que utilizar el tercer parámetro para indicar al navegador que elemento en concreto queremos extender ya que hay elementos que comparten la misma clase. Por ejemplo, `<q>` y `<blockquote>` comparten la clase `HTMLQuoteElement`.

Para utilizar el elemento extendido tenemos que hacer uso de la etiqueta nativa acompañada del atributo `is`.

```html
  <input is="my-custom-input">
```

Se puede ver un caso práctico en [numbers-only-input.html](../demos/numbers-only-input.html)



## Extensión de custom elements

Las APIs del navegador nos permiten extender custom elements de una forma más directa que componentes HTML nativos. Al igual al crear un nuevo custom element creamos una clase que extiende de `HTMLElement` podemos crear una clase que extienda directamente de un custom element.

```javascript
  class MyExtendedCustomElement extends MyCustomElement {

    constructor() {
      super();
      // Añadimos lógica adicional al custom element
    }

  }

  customElements.define('my-extended-custom-element', MyExtendedCustomElement);
```

Es habitual que en el `constructor` del primer elemento que extiende de `HTMLElement` adjuntemos en shadow DOM. Si nos encontramos en esta situación, volver a adjuntar el shadow DOM con el método `attachShadow` lanzaría una excepción porque ya se ha adjuntado al ejecutar el constructor del elemento extendido llamando a `super`.

Se puede ver un caso práctico en [app-card.html](../demos/app-card.html)