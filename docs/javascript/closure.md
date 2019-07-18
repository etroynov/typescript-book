## Замыкания

Лучшая вещь, которую когда-либо получал JavaScript - это замыкания. Функции в JavaScript имеют доступ ко всем переменным объявленным объявленным во внешней области видимости. 
Замыкания лучше всего объяснить на примерах:
```ts
function outerFunction(arg) {
    var variableInOuterFunction = arg;

    function bar() {
        console.log(variableInOuterFunction); // Доступ к переменной из внешней области видимости
    }   
    // Вызов локальной функции демонстрирует, что она имеет доступ к arg 
    bar();
}

outerFunction("hello closure"); // выведет hello closure!
```

Вы можете видеть что внутренняя функция имеет доступ к переменной (variableInOuterFunction) из вышестоящей области видимости.
Переменные во внешней функции были замкнуты (или связаны) с внутренней функцией. Отсюда и термин  **замыкание**.
Сама концепция достаточно проста и довольно интуитивна.

Теперь самая крутая часть: Внутренняя функция имеет доступ к переменным из вышестоящей области видимости *даже после того когда вышестоящая функция ее вернула*.
Все это потому, что переменные все еще связаны во внутренней функции и не зависят от внешней функции. Еще раз взглянем на пример:
```ts
function outerFunction(arg) {
    var variableInOuterFunction = arg;
    return function() {
        console.log(variableInOuterFunction);
    }
}

var innerFunction = outerFunction("hello closure!");

// Обратите внимание, что externalFunction возвращена
innerFunction(); // выведет hello closure!
```

### Почему это круто
Это позволяет нам объявлять объекты, в паттерне Revealing Module (паттерн выявления модулей, он же — паттерн открытый модуль, он же — паттерн раскрывающийся модуль)

```ts
function createCounter() {
    let val = 0;
    return {
        increment() { val++ },
        getVal() { return val }
    }
}

let counter = createCounter();
counter.increment();
console.log(counter.getVal()); // 1
counter.increment();
console.log(counter.getVal()); // 2
```

На высоком уровне это позволяет делать Node.js что-то вроде этого (Не беспокойтесь, если у вас в мозгу пока не щелкнуло. Это только 🌹):

```ts
// Псевдокод для объяснения концепции
server.on(function handler(req, res) {
    loadData(req.id).then(function(data) {
        // `res` был замкнут и доступен
        res.send(data);
    })
});
```
