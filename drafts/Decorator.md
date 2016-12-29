Decorator
===
----------------------
### O problema da pizza (The pizza problem)
##### Contexto

Imagine que você quer fazer um software de uma pizzaria e certamente, em algum momento, você terá uma classe chamada... Pizza!

Uma entidade Pizza certamente teria um nome, preço, sabores, no mínimo. Deveria se parecer com algo do tipo:
```
class PizzaMargherita{
   constructor(){
     this._name = "Margherita";
     this._price = 22.50;
     this._ingredients = ["basil", "tomato", "cheese"];
   }
   this.getIngredients(){
    return [].concat(this._ingredients);
   }
}
```

Até aqui, tudo bem. É possível representar várias pizzas em várias classes diferentes.

O problema acontece quando se quer fazer uma extensão do código sem alterar as características originais da entidade.

Como proceder,  por exemplo, se o usuário deseja adicionar o dobro de queijo na pizza? Afim de manter alta coesão, seria algo assim?

```
class PizzaMargheritaWithDoubleCheese{
   constructor(){
     this._name = "Margherita";
     this._price = 22.50;
     this._ingredients = ["basil", "tomato", "doubleCheese"];
   }
   this,getIngredients(){
    return [].concat(this._ingredients);
   }
}
```

Numa pizzaria com poucas pizzas, isto poderia resolver.

No entanto, seria extremamente improdutivo para o programador (e para o dono da pizzaria) ter várias classes que representam variações de uma pizza e é este o problema que o **Decorator** visa resolver.

#### Vantagens do decorator
 - Evita a criação de várias classes representando objetos com propriedades similares
 - Promove a criação de funcionalidades em tempo de execução
 - Preserva o princípio 'Open-closed' das classes (as torna abertas para extensão e fechadas para modificação)

#### Desvantagens do decorator
 - Pode haver excesso de subclasses
 
#### Definição de Decorator

Vejamos a definição de *Decorator* de acordo com a *Wikipedia*:
>Decorator ou wrapper, é um padrão de projeto de software que permite adicionar um comportamento a um objeto já existente em tempo de execução, ou seja, agrega dinamicamente responsabilidades adicionais a um objeto. Decorators oferecem uma alternativa flexível ao uso de herança para estender uma funcionalidade, com isso adiciona-se uma responsabilidade ao objeto e não à classe.

Tendo essa definição como guia, podemos fazer assim:

```
class PizzaBase {
  constructor(price){
    this._price = price;
  }
}

class PizzaTopping {
  constructor(name, value){
    this._name = name;
    this._value = value;
  }
}

class PizzaDecorator extends PizzaBase{
  constructor(name, price){
    super(price);
     this._name = name;
     this._price = price;
     this._toppings = [];
  }

  decorate(pizzaTopping){
     this._toppings.push(pizzaTopping);
     this._price += pizzaTopping._value;
  }
  
  listToppings(){
     this._toppings.map(function(topping){
       return topping._name;
     });
  }  
}

//E em seguida, a implementação da solução
var margherita = new PizzaDecorator("Margherita", 14.5);
margherita.decorate(new PizzaTopping("Tomato", 2));
margherita.decorate(new PizzaTopping("Basil", 5));
margherita.decorate(new PizzaTopping("Cheese", 14));
```

TODO:
 - Implementação real da pizza (UX + UI)

### Outros exemplos
 - Listar decorator de Addy Osmani
 - Listar outras implementações
 - Links externos


