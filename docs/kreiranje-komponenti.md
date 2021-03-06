# Različiti načini kreiranja React komponenti

Postoje različiti načini kreiranja React komponenti, a glavni su:

- `React.createClass` metoda (zastarelo)
- ES6 klasa
- ES6 funkcija

Glavna razlika između ES6 klase i funkcije je što klasa može imati stanje, dok ga čista funkcija ne može imati.

## Komponenta kao klasa

Komponenta kao klasa se takođe naziva komponenta sa stanjem (*stateful component*).

```jsx
import React, {Component} from 'react'

export default class Developer extends Component {
  render() {
    return (
      <div className="dev-wrapper">
        <h3 className="dev-name">{this.props.name}</h3>
        <img alt="developer" src={this.props.image} />
        <p>Moje glavne veštine su {this.props.skills}.</p>
      </div>
    )
  }
}
```

## Komponenta kao funkcija

Komponenta kao čista funkcija, ili komponenta sa bez stanja (*stateless component*), se koristi za jednostavne prezentacione komponente koje ne sadrže poslovnu logiku.

Komponenta kao funkcija prima `props` objekat kao ulaz, a vraća pripremljen `jsx` (nalik HTML-u) kao rezultat.

### Funkcionalna komponenta sa `return` naredbom

Na primer gornja klasa, pošto ne sadrži stanje, može biti refaktorisana u čistu funkciju. Komponenta kao funkcija je zapravo samo render metoda gornje klase:

```jsx
import React from 'react'

const Developer = props => {
  return (
    <div className="dev-wrapper">
      <h3 className="dev-name">{props.name}</h3>
      <img alt="developer" src={props.image} />
      <p>Moje glavne veštine su {props.skills}.</p>
    </div>
  )
}

export default Developer
```

Primetite da unutar funkcije ne koristimo `this`, već prosleđene atribute hvatamo direktno iz `props` objekta. Takođe, nije nam potrebno da uvozimo `Component` iz React-a, jer nemamo klasu koja je nasleđuje. Sam uvoz `React` biblioteke je i dalje neophodan. Na kraju, primetite da smo podrazumevani izvoz (`export default`) morali da premestimo na dno fajla, jer ga nije moguće ostaviti na početku, kao kod klase.

Ukoliko je potrebno da unutar funkcije obavimo neka računanja, to činimo pre `return` naredbe.

### Funkcionalna komponenta bez `return` naredbe

Ako funkcija u prvoj liniji odmah vraća vrednost, onda ključna reč `return` nije neophodna. Funkcija implicitno vraća ono nakon strelice:

```jsx
const Developer = props => (
  <div className="dev-wrapper">
    <h3 className="dev-name">{props.name}</h3>
    <img alt="developer" src={props.image} />
    <p>Moje glavne veštine su {props.skills}.</p>
  </div>
)
```

### Funkcionalna komponenta bez zagrada

Moguće je čak izbaciti zagrade, čime dobijamo najkraći mogući zapis komponente:

```jsx
const Developer = props =>
  <div className="dev-wrapper">
    <h3 className="dev-name">{props.name}</h3>
    <img alt="developer" src={props.image} />
    <p>Moje glavne veštine su {props.skills}.</p>
  </div>
```

Inače, zagrade se uglavnom ostavljaju radi preglednosti.

### Funkcionalna komponenta sa razlaganjem `props` parametra

U React zajednici ćete se susresti sa razlaganjem (destruktuiranjem) `props` objekta koji je prosleđen funkciji. Nakon razlaganja, vrednosti koristimo kao varijable, a ne kao atribute `props` objekta:

```jsx
const Developer = ({name, image, skills}) =>
  <div className="dev-wrapper">
    <h3 className="dev-name">{name}</h3>
    <img alt="developer" src={image} />
    <p>Moje glavne veštine su {skills}.</p>
  </div>
```

Sintaksa [razlaganja objekta](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Object_destructuring) je sledeća:

```js
const {name, image, skills} = props
```

Ovim se iz `props` objekta izvlače sledeći atributi (`name`, `image`, `skills`) i dodeljuju istoimenim varijablama.

Sličnu sintaksu koristimo kod `import` naredbe.
