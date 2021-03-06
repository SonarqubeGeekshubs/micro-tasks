# Action documentation

## Configuration

An action is a plain javascript object that supports the following options:

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| action | <code>object</code> |  | Action configuration. |
| action.method | <code>string</code> |  | Method that is executed when running the action. **IMPORTANT:** if `action.method` is asynchronous it has to return a promise. |
| [action.params] | <code>\*</code> | <code>[]</code> | List of parameters for the `action.method`. If it is not an array, it is wrapped in an array. Params are parsed before run `action.method`. See [action parser](#parser). |
| [action.if] | <code>object</code> |  | If the `if` property exists, the `action.method` is only executed if the condition pass. See an [example](../examples/conditional-action.js). |
| [action.if.method] | <code>string</code> |  | This method validates if the `taks.method` must be executed. |
| [action.if.params] | <code>\*</code> |  | List of parameters for the `action.if.method`. |
| [action.if.equalTo] | <code>\*</code> |  | The result of `action.if.method` has to be equal than `action.if.equalTo` to pass the condition. |
| [action.resultPath] | <code>string</code> |  | If it exists, the return value of the `action.method` is set on the `payload.resultPath`. See and [example](../examples/result-path.js). |
| [action.catch] | <code>boolean</code> | <code>false</code> | Specifies that this action captures errors from previous rejected action. See and [example](../examples/rejected-action.js). |
| [action.actions] | <code>array</code> |  | If exists microTasks executed this subactions before resolving or rejecting this action. See and [example](../examples/serial-actions.js). |
| [action.parallel] | <code>boolean</code> | <code>false</code> | If `action.actions` exists will be executed in parallel. This action will be resolved when all subactions have been resolved. See [MDN Promise All](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all). See and [example](../examples/parallel-actions.js). |
| [action.race] | <code>boolean</code> | <code>false</code> | If `action.actions` exists will be executed like a race. This action will be resolved when one subaction have been resolved or rejected. [MDN Promise Race](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/race). See and [example](../examples/race-actions.js). |

```javascript
// api login example: send request to https://api.example.com/user/login with post data
// and set the result in payload.userModel
{
  if: { // check if payload.email is a valid email
    method: 'validate.isEmail',
    params: '{payload.email}',
    equalTo: true
  },
  method: 'request.send', // send a request
  params: {
    body: { // request post data: https://api.example.com/user/login
      email: '{{payload.email}}',
      password: '{{payload.password}}'
    },
    hostname: 'api.example.com', // request url: https://api.example.com/user/login
    path: 'user/login',
    protocol: 'https',
    method: 'POST'
  },
  resultPath: 'userModel' // set the response in payload.userModel
}
```

```javascript
// race actions example: request user list to mirror01 and mirror02
// this action will be resolved when the first server responds
{
  race: true,
  actions: [
    {
      method: 'request.send', // send a request
      params: {
        hostname: 'mirror01.example.com', // request url: https://mirror01.example.com/users
        path: 'users',
        method: 'GET'
      }
    },
    {
      method: 'request.send', // send a request
      params: {
        hostname: 'mirror02.example.com', // request url: https://mirror02.example.com/users
        path: 'users',
        method: 'GET'
      }
    }
  ]
}
```

## Parser

Before executing each action, microTask **parse the parameters** and replace the values between braces `{{...}}` `{...}` with `context` and `payload` values.

- To replace a string use double braces: `'I am {{payload.userAge}} years old'` => `'I am 18 years old' // as string`.
- To replace a value or object use single braces: `'{payload.userAge}'` => `18 // as number`.
- You can use as source the payload: `'{payload.userAge}'`. See and [example](../examples/payload.js).
- You can use as source the context: `'{context.apiDbConnection}'`. See and [example](../examples/context.js).
- You can use dot notation if the value you want to use is a deep property of the context or payload, e.g.: `'{context.api.db.connection}'` or `'{payload.users[0].name}'`.
- Payload is used as context of current task **shared among all actions** of a task.
- Context is used as context of application **shared among all tasks**.

```javascript
microTasks.contextSet('shop.db.conection', {
  host: '123.45.678.90',
  user: 'root',
  password: 'a1b2c3d4'
})

// run task with array of actions
microTasks.taskRun([
  {
    method: 'mysql.query',
    params: {
      query: 'SELECT * FROM shop.users WHERE email='{{payload.email}}' AND password={{payload.password}}',
           // SELECT * FROM shop.users WHERE email='info@migueldelmazo.com' AND password='12345678'
      connection: '{context.shop.db.conection}'
           // { host: '123.45.678.90', user: 'root', password: 'a1b2c3d4' }
    }
  }
], {
  email: 'info@migueldelmazo.com',
  password: '12345678'
})
```
