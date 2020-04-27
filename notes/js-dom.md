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
