# Vue 3 Dialog Box

A simple customisable dialog box plugin for vue 3

## Installation

```bash
npm install vue3-dialog
```

## Add plugin

```js
import { createApp } from 'vue';
import Dialog from 'vue3-dialog';

const app = createApp({});
app.use(Dialog);
app.mount('#app');
```

## Available options

The API methods accepts these options:

| Attribute        | Type                | Default              | Description      |
| :---             | :---:               | :---:                | :---             |
|  message         | String              | `Hello World!`       |  The message prompted to the user   |
|  buttons         | String Array        | [`cancel`, `confirm`]|  List of buttons for the user to use as a reply  |
|  callbacks       | Object              | {}                   |  Set each key as a button name and the value as a function. The function will be run when the corresponding button is clicked  |
|  wrapperClass    | String              | `__dialog-wrapper`   |  Class for wrapper div that darkens the screen when the dialog box is shown    |
|  boxClass        | String              | `__dialog-box`       |  Class for main dialog box div    |
|  messageClass    | String              | `__dialog-message`   |  Class for message div     |
|  buttonsClass    | String              | `__dialog-buttons`   |  Class for div that holds the buttons    |
|  css             | Object             |  { default: true, wrapper: true, darken: 0.6 }      |  This option is onyl read when first installed. css.default determines if default styles should be applied. wrapper.default determines if the wrapper class styles should be applied. css.darken determines what percentage to darken behind the dialog box    |

### Buttons

The buttons options can optionally be passed as an object in this format:

```js
{
    cancel: {
        text: 'Cancel',
        class: '__dialog-button-cancel __dialog-button'
    },
    confirm: {
        text: 'Confirm',
        class: '__dialog-button-confirm __dialog-button'
    }
}
```
The text attribute is the text that will appear in the button<br>
The class attribute is a class name that will be applied to the button<br>
The values shown are are the default values when none is provided


Passing in the array ['cancel', 'confirm'] will yield the same results as the object above


### Callbacks

Any options passed in that is not list above will be treated as a callback<br>
It will be added to the callbacks options if its value is a function with no arguments

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

is the same as :
```js
{
    buttons: ['delete', 'cancel'],
    delete: () => {
        //some function
    }
}
```

## Usage

It is recommended to set default options in your main.js and only pass in messages, callbacks or button names when passing in options else where

These options could be used when the dialog box is mainly for deleting some contents:

main.js:
```js
app.use(Dialog, {
    message: 'Do you want to delete?',
    buttons: {
        remove: {
            text: 'Yes I want to delete',
            class: 'my-delete-button-class'
        },
        cancel: {}
    },
    boxClass: 'my-box-class',
    css: {
        darken: 0.9
    }
})
```

Details for the empty cancel object will be automatically filled in

mycomponent.vue:
```html
<button v-dialog="{ remove: myDeleteFunction, cancel: myCancelFunction }">DELETE</button>
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


### Provide / Inject

The $dialog method is provided for use with the composition API

```js
import { inject } from 'vue';

const dialog = inject('$dialog');

dialog({
    message: 'Are you sure?'
    buttons: ['yes', 'no'],
    yes: () => {/*some function*/},
    no: () => {/*some function*/}
});
```

### Global Property

The $dialog method is available in the template to activate a dialog box

```html
<button @click="$dialog({
    message: 'Are you sure?'
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
                message: 'Are you sure?'
                buttons: ['yes', 'no'],
                yes: () => {/*some function*/},
                no: () => {/*some function*/}
            })
        }
    }
}
```
