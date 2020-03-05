## Варианты импорта

    import * as name1 from './name1';
    import { default as justFunction, someFunction as someF, anotherFunction } from './name2';
    import name3 from './name3'; // импорт значения по умолчанию`

    name1.someFunction(); // вызов из модуля с именем name1
    someF(); // вызов из модуля без имени
    name1.default(); // вызов значения по умолчанию
    name3(); // вызов значения по умолчанию

 `*` - импорт всего из указанного модуля
 
`{someFunction, anotherFunction}` - импорт только перечисленных объектов

`{default as justFunction}` - импорт значения по умолчанию в значение justFunction

`{someFunction as someF}` - импорт с именованием объекта (алиас)

`as name1` - именование модуля, создает область видимости для модуля; 
           если не указать, то импорт в глобальную область видимости


## Варианты экспорта
    export const getFirstWeekday = () => 'Monday';
    export function getThirdWeekday () {return 'Wednesday';}
    
    const getSecondWeekday = () => 'Tuesday';
    const fourthWeekday = 'Thursday';
    export { getSecondWeekday, fourthWeekday };
    
    const getFifthWeekday = () => 'Friday';
    export default getFifthWeekday // экспорт по умолчанию
