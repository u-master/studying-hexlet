
## Assert (утверждение)

Каждую проверку, которую мы пишем для функции, в тестировании принято называть *утверждением (assert)*.
Node.js поставляется с модулями `assert` и `power-assert`, в котором есть несколько функций, упрощающих написание утверждений.
Ошибочный вариант выбрасывает исключение с сообщением и выводится стектрейс. `power-assert` еще показывает значение каждого объекта и результат каждой операции, входящей в выражение, переданное в функцию

Подключение модуля:

    import assert from 'assert';
   
Использование:
   
    assert(capitalize('hello') === 'Hello'); // AssertionError [ERR_ASSERTION]: false == true
    
Функция `assert.equal(actual, expected)` проверяет равенство **по ссылке**! 

    assert.equal(capitalize('hello'), 'Hello');
    // => AssertionError [ERR_ASSERTION]: 'hello' == 'Hello'
    //    generatedMessage: true,
    //    code: 'ERR_ASSERTION',
    //    actual: 'hello',
    //    expected: 'Hello',
    //    operator: '=='
    
Два разных объекта, имеющих одинаковое содержание, рассматриваются, как не эквивалентные:

    assert.equal({ key: 'value' }, { key: 'value' });
    // AssertionError [ERR_ASSERTION]: { key: 'value' } == { key: 'value' }

Для сравнения **по значению** используется `assert.deepEqual(actual, expected)`.

    assert.deepEqual({ key: 'value' }, { key: 'value' }); // всё ок

Для тестирования негативных сценариев предназначены функции 
`assert.notEqual(actual, expected)` и `assert.notDeepEqual(actual, expected)`.

Использование `power-assert`:

    import assert from 'power-assert';
    assert.equal(capitalize('hello'), 'Hello');
    
    
