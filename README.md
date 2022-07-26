# Vue 3 Dialog Box

A simple and customisable dialog box plugin for vue 3

## Installation

```bash
npm install vue3-dialog
```

## Add plugin

```js
import { createApp } from 'vue';
import App from './App.vue';
import Dialog from 'vue3-dialog';

createApp(App)
.use(Dialog, {
    //...options (not required)
})
.mount('#app');
```

## Available options

An object with these attributes can be passed to the plugin on install in your `main.js` file or when calling the plugin methods in a component

| Attribute        | Type                | Default              | Description      |
| :---             | :---:               | :---:                | :---             |
|  message         | String              | `Hello World!`       |  The message prompted to the user   |
|  buttons         | String Array        | `['cancel', 'confirm']`|  List of buttons for the user to use as a reply  |
|  presets         | Object              | `{}`                 | Preset dialog boxes with their own buttons and message |
|  preset          | String              | --                   | The preset to use, defined in the presets option |
|  callbacks       | Object              | `{}`                  |  Set each key as a button name and the value as a function. The function will be run when the corresponding button is clicked  |
|  wrapperClass    | String              | `__dialog-wrapper`   |  Class for wrapper div that darkens the screen when the dialog box is shown    |
|  boxClass        | String              | `__dialog-box`       |  Class for main dialog box div    |
|  messageClass    | String              | `__dialog-message`   |  Class for message div     |
|  buttonsClass    | String              | `__dialog-buttons`   |  Class for div that holds the buttons    |
|  css             | Object             |  `{ default: true, wrapper: true, darken: 0.6 }`      |  **This option is only read when first installed**<br> css.default determines if default styles should be applied. wrapper.default determines if the wrapper class styles should be applied. css.darken determines what percentage to darken behind the dialog box    |

Options set in `main.js` are saved as a global property that can be editted later

This object can be accessed by injection:
```js
import { inject } from 'vue';

const options = inject('$dialogOptions');

options.presets.delete = {
    message: 'Delete?',
    buttons: ['cancel', 'delete']
}
```
Directly in the template:
```html
<button @click="$dialogOptions.message = 'Changed!'">Click Me!</button>
```

Or directly in the script with the options API:
```js
export default {
    mounted() {
        this.$dialogOptions.presets.delete = {
            message: 'Delete?',
            buttons: ['cancel', 'delete']
        }
    }
}
```


### Buttons

The buttons options can optionally be passed as an object in this format:

```js
{
    buttons: {
        cancel: {
            text: 'Cancel',
            class: '__dialog-button-cancel __dialog-button'
        },
        confirm: {
            text: 'Confirm',
            class: '__dialog-button-confirm __dialog-button'
        }
    }
}
```
The text attribute is the text that will appear in the button<br>
The class attribute is a class name that will be applied to the button<br>
The values shown are are the default values when none is provided


This object will create the same results as the object above:
```js
{
    buttons: ['cancel', 'confirm']
}
```


### Callbacks

Any options passed in that is not listed above will be treated as a callback<br>
It will be added to the callbacks option if its value is a function with no arguments

For example,
```js
{
    buttons: ['delete', 'cancel'],
    callbacks: {
        delete: () => {
            //some function
        }
    }
}
```

Is the same as :
```js
{
    buttons: ['delete', 'cancel'],
    delete: () => {
        //some function
    }
}
```

## Usage

In your main.js you should set the default or fallback options for the dialog box along with your presets<br>
In your components you then only need to specify the preset name along with the callback functions

For example:

main.js:
```js
app.use(Dialog, {
    message: 'Are you sure?',
    buttons: {
        yes: {
            text: 'Yes',
            class: 'my-yes-button-class'
        },
        no: {}
    },
    presets: {
        remove: {
            message: 'Do you want to delete this?',
            buttons: ['cancel', 'remove']
        }
    },
    boxClass: 'my-box-class',
    css: {
        darken: 0.9
    }
})
```

The default 'no' button's details will automatically be filled in as:
```js
{
    no: {
        text: 'No',
        class: '__dialog-button-no __dialog-button'
    }
}
```

'cancel' and 'remove' buttons are filled in similarly

In a component file:
```html
<button v-dialog="{ yes: myYesFunction }">Button One</button>
<button v-dialog:remove="{ cancel: myCancelFunction, remove: myRemoveFunction }">Button Two</button>
```

### Directive

The v-dialog directive can be applied to an element to replace @click

```html
<button v-dialog="{
    message: 'Are you sure?'
    buttons: ['yes', 'no'],
    yes: () => {/*some function*/},
    no: () => {/*some function*/}
}">
Click Me!
</button>
```

When this button is clicked a dialog box will appear with the message `Are you sure?` and a yes and no button. The dialog box will disapear after these are pressed

When using the directive the preset name can be passed in as the directive argument<br>
This:
```html
<button v-dialog:confirm>Click Me!</button>
```
Is equivalent to:
```html
<button v-dialog="{ preset: 'confirm' }">Click Me!</button>
```

### Provide / Inject

The $dialog method is provided for use with the composition API

```js
import { inject } from 'vue';

const dialog = inject('$dialog');

dialog({
    message: 'Are you sure?',
    buttons: ['yes', 'no'],
    yes: () => {/*some function*/},
    no: () => {/*some function*/}
});
```

### Global Property

The $dialog method is available in the template to activate a dialog box

```html
<button @click="$dialog({
    message: 'Are you sure?',
    buttons: ['yes', 'no'],
    yes: () => {/*some function*/},
    no: () => {/*some function*/}
})">
Click Me!
</button>
```

Or in the script when using the options API

```js
export default {
    methods: {
        activateDialog() {
            this.$dialog({
                message: 'Are you sure?',
                buttons: ['yes', 'no'],
                yes: () => {/*some function*/},
                no: () => {/*some function*/}
            })
        }
    }
}
```
