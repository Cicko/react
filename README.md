
## Proposal: useStore, useMiddleware, setMiddleware
#### Redux-like app's store management with more user friendly api and integrated within React core.

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