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
      
