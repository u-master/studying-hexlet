# window, document, DOM, BOM

*Window* — это глобальный объект в браузере, то есть доступный из любого места всегда. Он не является частью JavaScript и предоставляется браузерами. Как правило, к нему обращаются по имени *window*, так как this из-за позднего связывания указывает на него не всегда.
Функция alert находится в объекте window, т.е. когда мы вызываем функцию alert, то в действительности происходит вызов window.alert. Более того, все глобальные **объекты JavaScript тоже принадлежат window** (Math.abs(x) - по сути window.Math.abs(x))

*BOM, Browser Object Model, Объектная модель браузера* - набор глобальных объектов, управляющих поведением браузера. Все они также находятся внутри window.
* объект Navigator - предоставляет информацию о браузере, такую как версия, название, используемую локаль, доступные права, подключенные плагины.
* объект Location - позволяет управлять адресной строкой. Например, вот так можно инициировать загрузку другой страницы: location.href = "https://hexlet.io";
* объект History - с помощью этого объекта можно перемещаться по истории переходов, а также формировать её в тех ситуациях, когда не происходит реального перехода по страницам. Это особенно актуально для SPA (single page application).
* объект Fetch - современный метод для выполнения AJAX запросов. Именно с помощью fetch происходит общение с сервером и другими сайтами.

*DOM, Document Object Model, Объектная модель документа* - это не зависящий от платформы и языка программный интерфейс, позволяющий программам и скриптам получить доступ к содержимому HTML-, XHTML- и XML-документов, а также изменять содержимое, структуру и оформление таких документов.

На практике, во фронтенд разработке это сводится к тому, что браузер предоставляет специальный объект *document*, который содержит внутри себя DOM дерево и который наполнен большим количеством методов (согласно спецификации DOM) для манипулирования этим деревом. Любые изменения, которые производятся с ним, сразу же отображаются браузером на странице.

# Манипулирование DOM деревом

* innerHTML

      elem.innerHTML = '\<b\>inner\</b\> text';

* textContent

      elem.textContent = '\<b\>inner\</b\> text';
      // "&lt;b&gt;inner&lt;/b&gt; text"

* createTextNode

      textElem = document.createTextNode('life is life');
      elem.append(textElem);

* createElement

      newElem = document.createElement('p');
      parentElem.append(newElem);
      
* cloneNode

      const newElem = elem.cloneNode(true); // true => глубокое клонирование

* вставка / удаление

      parentElem.append(newElem); // добавление в конец родительского
      parentElem.prepend(newElem); // добавление в начало родительского   
      parentElem.appendChild(newElem); // (depr) тоже добавляет в конец родительского
      parentElem.insertBefore(newElem, nextElSibling) // (depr) добавляет el в родительский перед nextElSibling
      parentElem.removeChild(elem); // (depr) удаляет elem из родительского
      parentElem.replaceChild(newElem, oldElem); // (depr) заменяет oldElem на newElem
      node.after(...nodes); // вставляет nodes после узла node
      node.before(...nodes); // вставляет nodes перед узлом node
      node.replaceWith(...nodes); // вставляет nodes вместо node
      node.remove(); // удаляет node
      
* поиск

      const subElem = elem.getElementById('content'); // поиск по id (должен быть уникальным в документе)
      const collection = elem.getElementsByClassName('row'); // возвращена коллекция! поиск по классу всех элементов
      elem.getElementsByTagName('span'); // поиск элементов по тэгу
      document.getElementsByTagName('\*'); // все элементы во всем документе
      const ul = document.querySelector('#menu'); // поиск по css-селектору, возвращает первый найденный
      const spans = ul.querySelectorAll('.odd > span'); // возвращает все найденные по селектору
      el.matches(css); // предикат, проверяет, удовлетворяет ли el селектору css
      el.closest(css); // ищет ближайший элемент выше по иерархии, удовлетворяющий селектору. Сам элемент тоже анализируется. Если такой элемент найден, то он возвращается.

* навигация по дереву

      document.documentElement -- доступ к корневому элементу html документа
      document.head -- элемент Head документа
      document.body -- элемент Body документа
      elem.childNodes; // коллекция! всех детей элемента, только для чтения [head, text, body]
      elem.firstChild; // первый ребенок элемента
      elem.lastChild; // последний ребенок элемента 
      elem.childNodes[1]; // ребенок с номером 1 (нумерация с 0)
      elem.parentNode; // родитель элемента
      
  Коллекция, это не массив! В нём отсутствуют привычные методы, такие как map, filter и другие. Для перебора элементов можно воспользоваться итератором forEach или превратить в массив, например, так:

      const elements = [...document.documentElement.childNodes];

  Указанные выше функции оперируют с объектами Nodes, который является базовым, в частности, к Element, Text, Comment. Бывает, что нужно оперировать только элементами (пропускать текстовые узлы и комментарии)
  
      elem.children; // дети-элементы элемента, коллекция!
      elem.firstElementChild; // первый ребенок элемента
      elem.lastElementChild; // последний ребенок элемента 
      elem.parentElement; // родитель элемента
      elem.previousElementSibling; // предыдущий соседний элемент
      elem.nextElementSibling; // следующий соседний элемент
      
* работа с классами

      elem.className; // строка с классами, то, что в атрибуте class
      elem.classList.add('newClass'); // добавить класс элементу
      elem.classList.remove('oldClass'); // удалить класс у элемента
      elem.classList.replace('oldClass', 'newClass'); // удалить класс у элемента
      elem.classList.toggle('class'); // если указанного класса нет, то добавить, иначе - удалить
      elem.classList.contains("class"); // предикат, true - если указанный класс есть, иначе - false
      
* работа с атрибутами

      elem.hasAttribute(name) – проверяет наличие атрибута
      elem.getAttribute(name) – получает значение атрибута
      elem.setAttribute(name, value) – устанавливает атрибут
      elem.removeAttribute(name) – удаляет атрибут
      elem.attributes – список html атрибутов
      
  Обратите внимание на то, что они работают именно с атрибутами (их именами), а не свойствами. И позволяют не только их извлекать, но и менять. Возникает закономерный вопрос: поменяется ли атрибут, если поменять свойство и наоборот? В основном синхронизация осуществляется только в сторону свойств. То есть меняется атрибут и автоматически обновляется свойство. Но существуют и исключения. Из этих тезисов не следует делать вывод, что нужно стараться работать через атрибуты. Наоборот, по возможности, **всегда работайте со свойствами дом дерева, а атрибуты используйте только для чтения**, чтобы получить то состояние, которое было в доме на момент инициализации (парсинга html).
  
* работа с data- атрибутами

  К кастомному атрибуту, например, data-special-content можно обратиться так elem.dataset.specialContent
  
# События

* В атрибутах тэга:

      <button onclick="alert('Бум!')"></button>

* В свойствах объекта элемента (замещает предыдущую же реализацию события):

      const button = document.getElementById('myButton');
      button.onclick = () => alert('Boom!');

* Добавить обработчик события (на элемент можно навесить много):

      const button = document.getElementById('myButton');
      const handler = (event) => alert('Boom 1!');
      button.addEventListener('click', handler); // добавить обработчик на событие click
      button.removeEventListener('click', handler); // удалить обработчик
      
События как правило складываются в очередь и будут выполнены строго последовательно. Но некоторые события все же берутся в обработку сразу. Это касается тех событий, которые генерируются программно, например onfocus.

* Действие по умолчанию

  Некоторые элементы в DOM обладают действиями по умолчанию. Например, если повесить обработчик на клик по ссылке, то выполнив этот клик, мы внезапно перейдем на другую страницу, ту которая была указана в href. Здесь мы видим пример того самого действия по умолчанию, на которое никак не влияет наличие обработчиков. Чтобы отменить это действие, а такое бывает нужно часто, необходимо вызвать метод *event.preventDefault()* внутри обработчика.

      <a href="#" id="myElement">Бум!</a>

      const button = document.getElementById('myElement');
      button.addEventListener('click', (e) => {
            e.preventDefault();
            alert(e.target.textContent);
      });

  Действиями по умолчанию обладают следующие элементы:
  * Клик по ссылке приводит к переходу на страницу указанную в href атрибуте.
  * Клик на кнопку с типом submit начинает отправку формы на сервер.
  * Вращение колесом мышки в textarea передвигает текст если он не помещается
  * Вызов контекстного меню с помощью правого клика мышки
  
* Стадии перехвата событий

  Предположим что у нас есть два вложенных элемента, на каждом из которых висит обработчик одинакового события. Если выполнить действие по внутреннему элементу, оно же автоматически выполнится и по внешнему, а значит, отработают оба события. В общем случае, событие проходит сквозь дерево начиная от корня до самого глубокого элемента, на котором событие сработало, и затем, в обратном направлении. Путешествия события туда и обратно, называется его стадиями.
  
  * Стадия погружения (Capture phase)
  
    На стадии погружения выполняются не все обработчики, а только те которые были привязаны к этой стадии. Привязка регулируется третьим параметром функции addEventListener
    
        button.addEventListener('click', () => alert('Boom 1!'), true);
        div.addEventListener('click', () => alert('Boom 2!'), true);
        
    Получится такой вывод: Boom 2! Boom 1! Т.е. сначала сработает на внешнем элементе, потом на внутреннем (он же - целевой)
    
  * Стадия всплытия (Bubbling phase)
  
    После остановки погружения на целевом элементе, начинается всплытие. Но на ней выполнение обработчиков происходит изнутри наружу: Boom 1! Boom 2!
  
  Согласно стандарту, большинство событий проходят обе стадии, сначала погружаясь в глубину дерева и затем поднимаясь до самого верха. Стадия погружения, при этом, используется крайне редко, большая часть обработчиков вешается на стадию всплытия.

  Объект *event.target* - это самый глубокий элемент (целевой) до которого идет погружение, он не меняется в процессе всплытия - благодаря нему всегда можно узнать где конкретно произошло событие. Объект *event.currentTarget* - это элемент, к которому прикреплен данный обработчик. В зависимости от ситуации используется тот или иной.

  В обычной ситуации событие должно всплывать до конца, но иногда могут возникать ситуации, когда всплытие нежелательно.
Сделать это можно двумя способами:
    * event.stopPropagation() -- останавливает всплытие, но дает возможность доработать всем обработчикам, которые висят на текущем элементе
    * event.stopImmediatePropagation() -- не дает выполнится больше ни одному обработчику
    
* События документа
  * *DOMContentLoaded* возникает в тот момент, когда DOM дерево полностью построено и готово к работе, но при этом стили, скрипты и картинки могут находиться в процессе загрузки. Это событие происходит на document.
  
    Скорость построения DOM дерева, очень зависима от тегов <script>. По стандарту, любой <script>, встреченный в HTML, будет выполняться ДО полного построения дерева. Следовательно скорость отработки кода в этом блоке <script> будет сильно влиять на то, когда пользователь увидит сам сайт, и то, когда сработает событие DOMContentLoaded.
      
    Поскольку DOM полностью готов только тогда, когда срабатывает событие DOMContentLoaded, многие механизмы активизируются именно на нем, например, браузерная подстановка значений в поля форм. По этой же причине, большинство сайтов, инициализируют интерфейсы также на этом событии.

  * *load* – когда все ресурсы загружены (картинки, стили, скрипты, ...)
  
  * *beforeunload* – когда надо уйти со страницы

# Формы

Отправку формы нужно перехватывать на событии submit самой формы.

    form.addEventListener('submit', (e) => {
      // Если хотим работать с формой через JS, то нужно остановить отправку:
      e.preventDefault();
      // что-то делаем
    });
    
Работа с данными формы:

  * Объект [FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData).
  
        form.addEventListener('submit', (e) => {
          e.preventDefault();
          // Данные формы извлекаются из DOM автоматически 
          const formData = new FormData(e.target); // элемент формы взят из события
          // Теперь с ними можно работать
          formData.get('email'); // example@example.com
          // values() возвращает итератор, поэтому преобразуем в массив
          [...formData.values()]; // ['example@example.com', 'supersecret']
          // Тоже итератор
          [...formData.entries()]; // [['email', 'example@example.com'], ['password', 'supersecret']]
          // Преобразование в обычный объект
          Object.fromEntries(formData); // { email: 'example@example.com', password: 'supersecret' }
        });
    
  * Свойство формы elements
  
        form.elements.email // <input name="email" ...
        form.elements.password // <input name="password" ...

        // Обработка событий элемента формы (например, для валидации)
        form.elements.email.addEventListener('change', () => {
            // Обработка
        });
        
# AJAX (Asynchronous JavaScript and XML)
  До появления HTML5, браузеры предоставляли (и сейчас предоставляют) специальный объект *XMLHttpRequest*. Работать с ним крайне неудобно и, по сути, все использовали обертку созданную в jQuery.
  
    const request = new XMLHttpRequest();
    request.onreadystatechange = () => {
      if (this.readyState == 4 && this.status == 200) {
        document.getElementById('demo').innerHTML = this.responseText;
      }
    };
    request.open('GET', '/api/v1/articles/152.json', true);
    request.setRequestHeader('X-Requested-With', 'XMLHttpRequest');
    request.send();

  С появлением стандарта HTML5, появился новый механизм для http запросов. [Полифилл](https://github.com/github/fetch)
  
    // const promise = fetch(url[, options]);
    fetch('/api/v1/articles/152.json')
      .then((response) => {
        console.log(response.status); // => 200
        console.log(response.headers.get('Content-Type'));
        return response.json();
       })
      .then((article) => {
        console.log(article.title); // => 'Как использовать fetch?'
      })
      .catch(console.error);

  Обратите внимание на то что response.json тоже возвращает промис. Кроме json данные можно получать, используя функции blob, text, formData и arrayBuffer.
  
  Отправка формы POST запросом:

    const form = document.querySelector('form');
    fetch('/users', {
      method: 'POST',
      body: new FormData(form),
    });

  Отправка формы как json:

    fetch('/users', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        name: 'Hubot',
        login: 'hubot',
      })
    })

# URL

  Распарсить или собрать url легко и просто не получалось. С одной стороны, можно всегда воспользоваться сторонними библиотеками, которых достаточно много, но с другой, в браузерах уже есть встроенный для этого механизм (обычно добавляется полифиллами).
  
    const url = new URL('../cats', 'http://www.example.com/dogs');
    console.log(url.hostname); // => www.example.com
    console.log(url.pathname); // => /cats

    url.hash = 'tabby';
    console.log(url.href); // => http://www.example.com/cats#tabby

    url.pathname = 'démonstration.html';
    console.log(url.href); // => http://www.example.com/demonstration.html

  А вот как можно работать с query параметрами:

    // https://some.site/?id=123
    const parsedUrl = new URL(window.location.href);
    console.log(parsedUrl.searchParams.get('id')); // => 123
    parsedUrl.searchParams.append('key', 'value')
    console.log(parsedUrl); // => https://some.site/?id=123&key=value

  *fetch* умеет работать с объектом *URL* напрямую:

    const response = await fetch(new URL('http://www.example.com/démonstration.html'));


