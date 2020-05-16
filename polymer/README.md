# Polymer

Es un proyecto empezado por un equipo de desarrolladores front-end de Chrome. Los proyectos en los que trabajan suelen estar orientados a hacer uso de las APIs del nevagador, son conocidos por el proyecto de Web Components.

El proyecto de web components en su comienza se componía de:

* [polymer library](https://polymer-library.polymer-project.org/2.0/docs/about_20) un proyecto que acelera el desarrollo de componentes web proporcionando utilidades y abstracciones de las diferentes implementaciones en navegadores.
* [webcomponents.org](https://www.webcomponents.org/) un proyecto con el pretenden hacer que los web components se puedan descubrir fácilmente. Podríamos decir que sería algo parecido al npm de los web components.
* [webcomponentsjs](https://github.com/webcomponents/polyfills/tree/master/packages/webcomponentsjs) una librería de polyfills para poder utilizar las especificación de web components en navegadores que no la soportan.

Aunque parezca que no ha sido una opción muy popular en la comunidad, en realidad, se podría decir que ha sido uno de los proyectos web más exitosos porque ha cumplido su objetivo, desaparecer y adoptarse como estándar.



## LitElement

Es una librería [muy ligera](https://bundlephobia.com/result?p=lit-element@2.0.1) (6.6kB gzip) que facilita el desarrollo de componentes web sobre la especificación V1. Contiene además un sistema de templating [lit-html](https://lit-html.polymer-project.org/) que facilita el trabajo con elementos HTML dentro de JS.

Esta librería se compone de templates, estilos, propiedades, eventos y un ciclo de vida.


### Templates

Al extender de LitElement la librería expone el método `render` para declarar templates que se actualizarán dinámicamente cuando las propiedades del componente cambian. LitElement se encarga de actualizar solo las partes del DOM del componente que cambian, para mantener este alto rendimiento la definición del template:

* no debería cambiar el estado del componente
* no debería provocar efectos secundarios
* debería depender solo de propiedades del componente
* debería devolver siempre el mismo resultado pasandole los mimos parámetros

Igual de importante es evitar actualizar el DOM del componente desde otros métodos que no sean `render`.

```javascript
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {

  render(){
    return html`
      <div>
        <p>Hola LitElement!</p>
      </div>
    `;
  }

}

customElements.define('my-element', MyElement);
```



### Templates Compuestos

Con LitElement podemos componer templates a partir de templates más pequeños. Esto se hace utilizando la función de templating `html`, podemos retornar su valor desde un método dentro de nuestra clase.

```javascript
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {

  render(){
    return html`
      ${this.header}
      <main></main>
    `;
  }

  get header() {
    return html`<header>My header</header>`
  }

}

customElements.define('my-element', MyElement);
```


### Data Binding

En LitElement, a diferencia de Angular, el data binding solo puede ser unidireccional. Es decir, no tiene una sintaxis para reflejar los cambios producidos en el template en las propiedades.

También tenemos que diferenciar los casos de data bindign que se presentan durante el desarrollo:

```javascript
// Atributo
html`<p id="${...}">`;

// Atributo true/false
html`<input ?disabled="${...}">`;

// Propiedad
html`<input .value="${...}">`;

// Event handler
html`<button @click="${this.doStuff}"></button>`;
```



### Estilos

Los estilos en LitElement quedan encapsulados gracias al uso del shadow DOM. Lo más recomendable es definir los estilos utilizando la función `css` para retornar el valor de la propiedad estática `styles` del componente.

```javascript
import { LitElement, html, css } from 'lit-element';

class MyElement extends LitElement {

  static get styles() {
    return css`
      :host {
        display: block;
        box-shadow: 0px 2px 5px #e2e2e2;
      }
    `;
  }

  render(){
    return html`
      <div>
        <p>Hola LitElement!</p>
      </div>
    `;
  }

}

customElements.define('my-element', MyElement);
```



### Estilos Compuestos

Los estilos del componente se pueden componern al igual que los templates. Además podemos juntar varios bloques de estilos dentro de `styles` devolviendo un `Array`.

```javascript
// my-element.styles.js
import { LitElement, html, css } from 'lit-element';

export const variables = css`
  :host {
    --primary-color: royalblue;
  }
`;
```

```javascript
import { LitElement, html, css } from 'lit-element';
import { variables } from './my-element.styles';

class MyElement extends LitElement {

  static get styles() {
    return [
      variables,
      css`
        :host {
          display: block;
          color: --primary-color;
          font-size: ${this.fontSize}
        }
      `
    ];
  }

  render(){
    return html`
      <div>
        <p>Hola LitElement!</p>
      </div>
    `;
  }

  get fontSize() {
    return css`1.5em`;
  }

}

customElements.define('my-element', MyElement);
```



### Propiedades

Las propiedades se utilizan para almacenar el estado del componente. Se definen como propiedades de un `Object` retornado en la propiedad estática `properties`. LitElement reconoce una serie de propiedades para los objetos de atributos:

* **`type`** acepta los objetos de tipo de dato de JS `Object`, `String`, etc. LitElement aplicará un coversor por defecto según el tipo seleccionado a los atributos relacionados.
* **`converter`** acepta una `Function` conversora, sirve para aplicar nuestro propio transforamdor atributo-propiedad. 
* **`attribute`**  acepta un `String`, este será el nombre que LitElement utilizará para el atributo relacionado a la propiedad. Por defecto utlizará el nombre de la propiedad convertida a lowercase como atributo.
* **`reflect`** acepta los valores `true`, `null` y `undefined`. Se utiliza para configurar si queremos que los cambios en la propiedad se vean reflejados en el atributo relacionado.
* **`noAccessor`** acepta los valores `true` y `false`. Se utiliza para configurar si queremos que LitElement utilice el getter y setter por defecto de la propiedad. Si utilizamos nuestro propio accesor es imporante llamar a `requestUpdate` en el setter.
* **`hasChanged`** acepta una `Function` que recibirá como argumentos el valor antiguo y el nuevo valor de la propiedad. Retornando `true` le indicamos a LitElement que se ha realizado un cambio en la propiedad y debe ejecutar su ciclo de vida.

```javascript
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {

  static get properties() {
    return {
      name: {
        type: String
      }
    };
  }

  render(){
    return html`
      <div>
        <p>Hola ${this.name}!</p>
      </div>
    `;
  }

}

customElements.define('my-element', MyElement);
```



### Eventos

Como hemos visto en la sección de data binding, LitElement nos permite hacer el binding de métodos a eventos utilizando `@` seguido del nombre del evento en camel case.

```javascript
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {

  render(){
    return html`
      <form @submit=${this.handleSubmit}>
        <input name="nombre">
        <button>Enviar</button>
      </form>
    `;
  }

  handleSubmit() {
    // el objeto this sigue haciendo referencia al componente
  }

}

customElements.define('my-element', MyElement);
```

Para emitir eventos se utilizan los [Custom Events](../web-components/).



### Ciclo de vida

LitElement extiende el [ciclo de vida](../web-components) de los custom elements con los siguientes "hooks" en orden de ejecución:

1. **`someProperty.hasChanged`** el primer método que se dispara el definido en `hasChanged` para determinar si la propiedad ha cambiado.
2. **`requestUpdate`** método con el que podemos iniciar un renderizado indicando la propiedad que ha cambiado.
3. **`performUpdate`** LitElement utiliza este método para determinar cuando debería empezar a procesar los cambios en las propiedades. Por defecto, espera a `requestAnimationFrame`
4. **`shouldUpdate`** método con el que podemos interrumpir el ciclo de vida. Devolviendo `false` detendremos el ciclo de vida, recibe un `Map`con las propiedades que han cambiado como argumento.
5. **`update`** LitElement utiliza este método para actualizar los atributos si procede y llamar a `render`. 
6. **`render`** método que devuelve el template `html` del componente. Siempre debe ser implementado.
7. **`firstUpdated`** método ejecutado solo la primera vez que el componente ha pintado contenido.
8. **`updated`** método ejecutado cada vez que se producen cambios en las propiedades del componente y este termina de pintar.
9. **`updateComplete`** método ejecutado cuando el componente termina su ciclo de vida. Devuelve una `Promise` que podemos utilizar para ejecutar código cuando el componente es estable.