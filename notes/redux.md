# Redux

Redux — это такая база данных в программе. Она хранит внутри себя состояние (то есть данные) приложения. 
Redux отвечает только за состояние и никак не связан с браузером, DOM и фронтендом в целом. 
Его можно использовать, самого по себе, даже на бекенде в Node.js.
Redux, с точки зрения кода — это объект, внутри которого лежат данные. 
Он используется остальными частями приложения для их хранения, изменения и извлечения. 
В терминологии Redux он называется **контейнер**, так как данные хранятся внутри.

### Три принципа

Что главное в Redux:

  1. Single source of truth — используя Redux, мы работаем только с одним контейнером на приложение. 
  Это одно из ключевых отличий от Flux-архитектуры. Всё состояние в одном месте.
  1. State is read-only — данные меняются только косвенно, используя функциональный стиль.
  1. Changes are made with pure functions — внутри хранилища можно использовать только чистые функции. 
  Тут правила даже строже чем во Flux, так как не позволяется использовать даже Date.now() и ему подобные функции, 
  которые хотя и не обладают побочными эффектами, но все же являются недетерминированными. 
  Все подобные вызовы должны делаться до вызова dispatch.

Ниже полный пример использования Redux:

    import { createStore } from 'redux';

    // Редьюсер – функция, которая описывает то, как изменяются данные внутри контейнера
    // Она принимает на вход текущее состояние приложения и должна вернуть новое
    // Именно так работает функция reduce, отсюда и название
    // Второй параметр описывает действие, с его помощью мы узнаем
    // как конкретно надо обновить данные для конкретного вызова
    // action — это объект, в котором обязательно есть поле type, содержащее имя действия
    // в этом же объекте прокидываются дополнительные данные. Для данных, которые передаются
    // как параметры, лучше использовать вложеный единый объект (например, action.payload).
    const reducer = (state = 0, action) => {
      switch (action.type) {
        case 'INCREMENT':
          return state + 1;
        case 'DECREMENT':
          return state - 1;
        default: // действие по умолчанию – возврат текущего состояния
          return state;
      }
    };

    // Создание контейнера на основе редьюсера
    // Именно в этом контейнере хранится состояние, которое возвращает редьюсер
    const store = createStore(reducer);

    // Состояние можно извлечь с помощью функции getState()
    store.getState(); // 0 – так как это начальное значение состояния

    // Функция subscribe позволяет подписываться на изменение состояния контейнера
    // Она очень похожа на addEventListener, но без указания события
    // Как только меняется любая часть состояния, контейнер по очереди вызывает все что было добавлено
    // Здесь мы просто извлекаем из контейнера состояние и печатаем его на экран
    store.subscribe(() => console.log(store.getState()));

    // Для избежания дублирования и повышения уровня абстракции, вынесем действия в функции
    const increment = () => ({ type: 'INCREMENT' });
    const decrement = () => ({ type: 'DECREMENT' });
    
    // dispatch – функция, которая вызывает редьюсер
    // передает туда текущее состояние
    // и действие (напомню что это объект со свойством type)
    store.dispatch(increment()); // => 1
    store.dispatch(increment()); // => 2
    store.dispatch(decrement()); // => 1
    
    store.getState(); // 1





## Состояние

Все, что хранится в контейнере, мы называем состоянием, но не все состояния одинаково полезны. Вот какую классификацию вводит документация Redux:

  * Domain data — данные приложения, которые нужно отображать, использовать и модифицировать. Например, список пользователей, загруженный с сервера.
  * App state — данные, определяющие поведение приложения. Например, текущий открытый URL.
  * UI state — данные, определяющие то, как выглядит UI. Например, вывод списка в плиточном виде.

Структура состояния должна напоминать базу данных. Всё максимально плоско и нормализованно.


## Начальное состояние

Начальное состояние задаётся в определении редьюсера:

    const reducer = (state = 0, action) { /* ... */ }

Но часто этого недостаточно. Данные могут прийти из бэкенда и их нужно прогрузить в контейнер перед началом работы. Для этого случая в Redux есть особый путь:

    const store = createStore(reducer, initState);

Redux посылает специальное действие (*@@redux/INIT*), которое нельзя перехватывать. 
Если редьюсер реализован правильно и **содержит секцию default в switch**, то контейнер заполнится данными из `initState`. 
В коде выше, функция `createStore` вызовет редьюсер так: `reducer(initState, '@@redux/INIT')`. Затем выполнится ветка `default` и состоянием контейнера станет число `initState`.

## Подписка

Можно подписаться на изменение состояния функцией `store.subscribe`. Ее параметром является колбэк функция, которая вызывается при изменении стейта. Например, нам нужно в реакте отрисовывать компонент при изменении redux-состояния.

    // Элемент для подключения React
    const containerElement = document.getElementById('container');
    
    // Подписываемся на изменения состояния внутри контейнера
    // На каждое изменение отрисовываем наш компонент заново
    store.subscribe(() => {
      const state = store.getState();
      ReactDOM.render(
        <Increment dispatch={store.dispatch} count={state} increment={increment} />,
        containerElement,
      );
    });
    
    // Первый раз нужно отрисовать руками
    ReactDOM.render(
      <Increment dispatch={store.dispatch} increment={increment} />,
      containerElement,
    );

## Комбинирование редьюсеров

С ростом количества сущностей (состояний), редьюсер становится очень тяжёлым. Огромный кусок кода, который делает всё.
Для решения этой проблемы, Redux имеет встроенный механизм, позволяющий создавать множественные редьюсеры и комбинировать их друг с другом.
Работает это так: для каждого свойства верхнего уровня пишется свой собственный редьюсер, а затем они с помощью функции `combineReducers` объединяются в корневой редьюсер, который уже используется для создания контейнера.

    {
      todos: [
        { id: 1, name: 'why?' },
        { id: 3, name: 'who?' },
      ],
      comments: [
        { id: 23, todoId: 3, text: 'great!' },
      ],
    }
    
    import { combineReducers, createStore } from 'redux';
    
    const todosReducer = (state = [], action) => {
      // сюда попадут данные из todos
    };
    
    const commentsReducer = (state = [], action) => {
      // сюда попадут данные из comments
    };
    
    const rootReducer = combineReducers({
      todos: todosReducer,
      comments: commentsReducer,
    });
    const store = createStore(rootReducer);
    
В каждый редьюсер приходит state, но это не всё состояние контейнера, а только та часть, которая лежит в соответствующем свойстве.

Редьюсеры могут быть даже вложенными и для этого не нужны никакие специальные средства, обычные функции, принимающие на вход данные и возвращающие новые данные.
С таким подходом появляется одна особенность, которая вначале может испугать. Так как каждый редьюсер имеет доступ только к своей части состояния, действия, порождающие изменения сразу в нескольких местах, будут повторяться в разных редьюсерах.

    const todos = (state = {}, action) => {
      switch (action.type) {
        case 'TODO_REMOVE':
          // ...
      }
    };
    
    const comments = (state = {}, action) => {
      switch (action.type) {
        // При удалении ToDo также нужно удалить все его комментарии
        case 'TODO_REMOVE':
          // ...
      }
    };

## Мидлвары (middlewares)

Мидлвары — функции, которые последовательно вызываются в процессе обновления контейнера.

Общий принцип работы таков:
  * Мидлвары встраиваются в хранилище при его создании.
  * Во время диспатчинга (отправки действий) данные проходят через них и только затем попадают в редьюсер.

Создаются мидлвары во время инициализации контейнера. Если нет параметра начального состояния, 
можно мидлвары указывать вторым параметром - redux умеет их различать.
Однако, перед тем как передать мидлвар в функцию `createStore`, нужно применить к нему функцию `applyMiddleware`

    import { createStore, applyMiddleware } from 'redux';
    
    const store = createStore(
      reducer,
      /* preloadedState, */
      applyMiddleware(thunk)
    )

В случае если мидлваров несколько, придётся воспользоваться функцией `compose`:

    import { createStore, applyMiddleware, compose } from 'redux';
    import thunk from 'redux-thunk';
    import logger from 'redux-logger';
    
    const store = createStore(
      reducer,
      /* preloadedState, */
      compose(
        applyMiddleware(thunk),
        applyMiddleware(logger)
      ),
    )

### Расширение Redux DevTools

Для Redux написано специальное браузерное расширение Redux DevTools. Установите его в свой браузер.
Ниже код подключения этого расширения к хранилищу:

    const reduxDevtools = window.__REDUX_DEVTOOLS_EXTENSION__;
    const store = createStore(
       reducer,
       /* preloadedState, */
        reduxDevtools && reduxDevtools(),
     );

Обратите внимание на то, что он не требует использования функции `applyMiddleware`.

## React Redux

Команда Redux создала библиотеку react-redux, которая значительно упрощает привязку Redux к React. Далее мы пройдём все этапы по её подключению к React-проекту.

### Провайдер

*Провайдер* (Provider) — корневой React-компонент, который делает Redux-контейнер доступным для всего приложения. **Он находится на верхнем уровне JSX** и "оборачивает" в себя все остальные компоненты. Провайдер кроме прочего, **выполняет подписку** через `store.subscribe`. Это значит, что больше не придётся беспокоиться об обновлении приложения при изменении данных внутри контейнера.

    import React from 'react';
    import { createStore } from 'redux';
    import { render } from 'react-dom';
    import { Provider } from 'react-redux'; // импорт компонента!
    import TasksBox from './components/TasksBox.jsx';
    import reducers from './reducers.jsx'

    // Контейнер передаётся в провайдер
    const store = createStore(reducers);

    render(
      <Provider store={store}>
        <TasksBox />
      </Provider>,
      document.getElementById('container'),
    );

### `connect`

Для связывания данных из контейнера с пропсами конкретного компонента, react-redux предоставляет функцию `connect`.
Также функция `connect` **пробрасывает в компонент дополнительные пропсы**. Один из важных из них - **функция `dispatch`**. Эта функция работает точь-в-точь как и store.dispatch. Ей нужно передать действие, что в свою очередь запустит цепочку вызовов до перерисовки.

    import React from 'react';
    import { connect } from 'react-redux';
    
    // Эта функция, берет нужные данные из контейнера и отдаёт их компоненту
    const mapStateToProps = state => {
      const props = {
        tasks: state.tasks,
      }
      return props;
    };

    class TasksBox extends React.Component {
      handleAddTask = (e) => {
        e.preventDefault();
        // dispatch!
        const { dispatch, newTaskText } = this.props;
        dispatch(addTask({ text: newTaskText }));
      };
      
      render() {
        // Извлекаем задачи из свойств
        const { tasks } = this.props;
        // Отрисовываем задачи
        <div>{/* logic with this.handleAddTask */}</div>
      }
    }

    // connect соединяет контейнер с текущим компонентом
    // Наружу экспортируется компонент, который используется как обычно (пример выше)
    export default connect(mapStateToProps)(TasksBox);
    
Функция mapStateToProps принимает на вход состояние из контейнера и должна возвратить объект, свойства которого станут props в компоненте. В тривиальном случае мы всегда можем реализовывать эту функцию так `state => state`. Но делать так не стоит. Более того, всю предварительную обработку данных, подготовленных для вывода, стоит делать именно здесь. В идеале в компоненты должны попадать уже готовые к выводу данные.

Однако, функция **`connect` позволяет обходиться без явного вызова `dispatch`**. Общий принцип работы такой: в файл с компонентом импортируются необходимые действия и передаются вторым параметром в функцию connect.

    // Формируем объект с действиями - свойства этого объекта прокинутсяв пропсы компонента!
    const actionCreators = {
      addTask: actions.addTask,
    };
    
    class TasksBox extends React.Component {
      handleAddTask = (e) => {
        e.preventDefault();
        const { addTask, newTaskText } = this.props;
        addTask({ text: newTaskText });  // Вызываем action из пропса и он сам диспатчится
      };
      
      render() {
        // Отрисовываем задачи
      }
    }
    
    // Передаём действия вторым параметром!!!
    export default connect(mapStateToProps, actionCreators)(TasksBox);
    
### Файловая структура

Имея такое количество сущностей, возникает закономерный вопрос: как их раскладывать в файловой системе. Обычно делают так:

    actions/index.js
    components/App.jsx
    reducers/index.js
    index.jsx

## `redux-action`

Библиотека, которая позволяет упорядочить создание функций-действий и реализации редьюсеров.
Она разделяет процесс создания объекта действия на два этапа:

  1. Определение действий:

     Функция **`createAction`** принимает тип действия (свойство type) и возвращает функцию, принимающую `payload`:

         import { createAction } from 'redux-actions';
         export const addUser = createAction('USER_ADD');

  1. Определение редьюсеров:

     Функция **`handleActions`** принимает объект, где ключами являются действия, а значением - обработчики, обрабатывающие свойство `payload`:

         import { handleActions } from 'redux-actions';
         import { addUser } from './actions.jsx';
         
         const defaultState = {};
         const userReducer = handleActions(
           {
             [addUser]: (state, { payload: { user } }) => {
               return { ...state, [user.id]: user };
             },
            },
           defaultState
         );
         
         export default combineReducers({
           users: userReducer,
         });

Далее уже привычным образом в функцию `createStore` из Redux передаются редьюсеры, а функции действий используются при вызовах `store.dispatch`:

    import { addUser } from './actions.jsx'
    import reducers from './reducers.jsx'

    const user = /* get user from somewhere */;
    const store = createStore(reducers);
    store.dispatch(addUser({ user }));

Либо, качестве ключа объекта для `handleActions()` выступает функция, полученная из `createAction()`:

    const actionsHandlers = {
      'ADD_TASK': (state) => (/* handler */)
    };

Такое поведение возможно благодаря внутреннему устройству функции createAction, в которой переопределен метод toString примерно на: `actionCreator.toString = () => type;`

Итак, появились плюшки:

  * Каждый обработчик теперь отдельная функция, а значит их окружения не пересекаются, как в случае switch.
  * Не нужно описывать поведение по умолчанию, когда возвращается сам state.
  * Стало невозможным отправлять неправильное действие, так как каждая функция-обработчик формируется на основании функций, генерирующих действия.

