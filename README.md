
## Proposal: useStore, useMiddleware, setMiddleware
#### Redux-like app's store management with more user friendly api and integrated within React core.


The goal is to be able to have a completely comfortable way to have a state that can be shared between components like when using Redux but without having to write so much code while having the same features and even more. For example `useMiddleware ` would also accept a Promise as body so it can be used for asynchronous actions. Unlike with Redux, where you have to specify an action `type` which is then displayed in the redux-dev-tool, this library should be more intelligent, building automatically the action name and also still be able to set an action name.

For example, for asynchronous middlewares, if the name of the middleware is `'loadData'`, it would automatically show as "dispatched" actions `loadData//INIT` and if resolved or rejected then `loadData//SUCCESS` and `loadData//ERROR` respectively (I would not use `RESOLVE and REJECT` since they are visually similar therefore slower to detect)


```Javascript
import * as React from 'react';

React.setMiddleware = (name: string, body: (Promise | Function), alwaysExecuted: boolean) => {

};

React.useStore = (path: string, defaults?: object, middlewareTriggers?: Array) => {
    if (!window['__react_store__']) {
        window['__react_store__'] = {};
    }
};

React.setMiddleware('loadData', (path, value, next) => (reject, resolve) => {
    // Whatever
    // bla bla bla and then
    fetch('something on other planet')
        .then(resolve)
        .catch(reject);
}, false);

const Component = () => {
    const [subModule, setSubModule] = React.useStore({ data1: 'Module.subModule', data2: 'Module2.subModule2' }, null, {
        data1: { attribute: ['loadData'] }, // middleware triggers. When attribute changes, loadData middleware is executed.
        attribute2: ['loadData2', 'anotherMiddleware']
    });

    setSubModule('attribute.subAttribute', 'value for attribute');

    // For debugging purposes:
    // 1. Adapt redux-dev-tools to work for this one. Or create custom one.
    // 2. There are no types, instead there will be displayed the component name and then the 'attribute' with its passed value.
    //  2.1 That will allow to show more clearly how the data is flowing between the application in specific moments in time.
    // Allow attach middlewares easily. Like useMiddleware.
};

export default Component;
```
