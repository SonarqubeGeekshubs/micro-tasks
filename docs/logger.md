<a name="module_logger"></a>

## logger
Registers the hooks and methods of the module **logger** in microTasks.


* [logger](#module_logger)
    * [~hooks](#module_logger..hooks)
    * [~logger.error([arguments])](#module_logger..logger.error)
    * [~logger.log([arguments])](#module_logger..logger.log)
    * [~logger.warn([arguments])](#module_logger..logger.warn)

<a name="module_logger..hooks"></a>

### logger~hooks
Hook list registered in microTask.


| Name | Type | Description |
| --- | --- | --- |
| logger.error | <code>method</code> | Executes `logger.error` method |
| logger.log | <code>method</code> | Executes `logger.log` method |
| logger.warn | <code>method</code> | Executes `logger.warn` method |
| microTasks.onActionRegisterError | <code>method</code> | Executes `logger.error` method |
| microTasks.onActionEnd | <code>method</code> | Executes `logger.log` method |
| microTasks.onHookRegisterError | <code>method</code> | Executes `logger.error` method |
| microTasks.onGlobalError | <code>method</code> | Executes `logger.error` method |
| microTasks.onMethodRegisterError | <code>method</code> | Executes `logger.error` method |
| microTasks.onMethodRun | <code>method</code> | Executes `logger.log` method |
| microTasks.onTaskEnd | <code>method</code> | Executes `logger.log` method |
| microTasks.onTaskError | <code>method</code> | Executes `logger.error` method |
| microTasks.onTaskRegisterError | <code>method</code> | Executes `logger.error` method |
| microTasks.onTaskRunError | <code>method</code> | Executes `logger.error` method |

<a name="module_logger..logger.error"></a>

### logger~logger.error([arguments])
Prints in console an error.


| Name | Type | Description |
| --- | --- | --- |
| [arguments] | <code>\*</code> | Arguments to log |

**Example**  
```js
microTasks.methodRun('logger.error', 'this is', 'an error')
```
<a name="module_logger..logger.log"></a>

### logger~logger.log([arguments])
Prints in console a log.


| Name | Type | Description |
| --- | --- | --- |
| [arguments] | <code>\*</code> | Arguments to log |

**Example**  
```js
microTasks.methodRun('logger.log', 'this is', 'an log')
```
<a name="module_logger..logger.warn"></a>

### logger~logger.warn([arguments])
Prints in console a warning.


| Name | Type | Description |
| --- | --- | --- |
| [arguments] | <code>\*</code> | Arguments to log |

**Example**  
```js
microTasks.methodRun('logger.warn', 'this is', 'a warning')
```