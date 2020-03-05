По степени изолированности тестируемых частей кода функциональные тесты принято делить на:
* Модульные (Они проверяют работоспособность конкретных программных модулей, например, функций. 
Такие тесты проще всего писать, но они не способны помочь проверить, что модули работают вместе.)
* Интеграционные (Проверить, что модули работают вместе.)
* Системные (Наиболее сложные и максимально эффективные. В случае сайтов — это тестирование через браузер. 
Эти тесты эмулируют поведение настоящего пользователя, ходят по сайту, кликают на ссылки, заполняют и отправляют формы. )

При написании тестов нужно ориентироваться на разнообразие входных данных. 
У любой функции есть один или несколько основных сценариев использования и достаточно написать ровно одну проверку, 
которая покрывает этот сценарий. Дальше нужно смотреть на *пограничные случаи* - ситуации, в которых код может 
повести себя по-особенному:
* Работа с пустой строкой
* Обработка `null`
* Деление на ноль (в большинстве языков вызывает ошибку)
* Специфические ситуации для конкретных алгоритмов

В большом числе ситуаций пограничные случаи требуют отдельной обработки, наличия условных конструкций. 
Тесты должны быть построены таким образом, чтобы они затрагивали каждую такую конструкцию. 
Но не забывайте, что условные конструкции могут порождать хитрые связи. 
Например, два независимых условных блока порождают 4 возможных сценария.

Комбинация всех возможных вариантов поведения функции называется *цикломатической сложностью*. 
Это число показывает все возможные пути кода внутри функции. 
Цикломатическая сложность — хороший ориентир для понимания того, сколько и какие тесты нужно написать.

## Assert (утверждение)

Каждую проверку, которую мы пишем для функции, в тестировании принято называть *утверждением (assert)*.
Node.js поставляется с модулем assert, в котором есть несколько функций, упрощающих написание утверждений.
Ошибочный вариант выбрасывает исключение с сообщением и выводится стектрейс:

    AssertionError [ERR_ASSERTION]: false == true
    
Использование:

    import assert from 'assert';
    assert(capitalize('hello') === 'Hello');
    assert.equal(capitalize('hello'), 'Hello');
    

    