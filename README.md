# Sequence Listener

Keyboard sequence listener (like barcode readers) in javascript. This lightweight library (**3.2 kB** when compressed) has **no dependencies**.

## Installation

By using bower (http://bower.io/)
```sh
bower install sequence-listener --save
```
### Loading the script

This AMD library can be loaded with requireJs or any other AMD Javascript toolkit

```js
var SequenceListener = require('sequence-listener'); //singleton instance
new SequenceListener();
```

Or, if you are oldschool :
```html
<script src="sequenceListener.js"></script>
<script>var SequenceListener = new SequenceListener();</script>
```

### Listening sequence

An event of type `keyboardSequence` is dispatched on the element who received the input. It could be a form input (you have to enabled it) or the document body itself.

#### Javascript
```js
document.body.addEventListener('keyboardSequence', function (e) {
	alert(e.detail.sequence);
})
```

#### JQuery
```js
$(document.body).on('keyboardSequence', function(e){
    alert(e.originalEvent.detail.sequence);
});
```

## How this is working ?
A lot of devices like **barcode readers**, **magnetic card readers**, **Yubikey** (https://www.yubico.com/products/yubikey-hardware/) are detected as a keyboard when plugged in and send input the exact same way a humain would when using a standard keyboard.

This library will listen the keyboard entries and if the sequence provided is entered very fast, contains only certain chars and the length of the input match your criterias, an event will be triggered.

### What about users typing text very fast ?
Even if it is almost impossible to type as fast as an hardware, there is multiple ways to differenciate the user's input and the devices input. Here are the options provided to helps you :

 - You can calibrate the speed required to be a valid input
 - You can specify a min length of the input or a specific length required
 - You can specify the chars allowed on that inputs
 - You can disabled the listener when the focus is inside a form input

## Configurations

The configurations need to be send in the constructor like this :
```js
new SequenceListener({
	debug: true
});
```

| **Configuration name** | **Default value** | **Description**                                                                                                              |
|--------------------|---------------|--------------------------------------------------------------------------------------------------------------------------|
| debug              | false         | That will write debug output in the console to help you to calibrate the library                                         |
| maxKeyboardDelay   | 75            | When the delay between 2 strokes is greater than this number, the sequence is completed                                  |
| minLength          | 5             | If this is 1, any keystroke will be detected as an input                                                                 |
| exactLength        | null          | If you use this, this is gonna override the minLength value. You need to set one or other or an exception will be thrown |
| allowedChars       | [a-zA-Z0-9]   | Regex use to validate chars individually, more information below                                                         |
| ignoreInputs       | true          | If this is true, the library will not listen when the focus is on a form input                                           |

### allowedChars
This regex will be used to validate the chars individually. **You can't add length valiation** in this regex or this will fail. Also, it is important to note that invalid chars will not stop the recording of the input.

The reason why we decided to not stop the recording is because a lot of those devices add a "return" or other chars at the end of the string. We might add more options later to make it more strict.

#### Exemple : 
If you want to catch only numbers : `[0-9]`, the following string will be catch : **ORDER-12345**. The recorded input would be : **12345**.

## Contribution

Feel free to comments, to send suggestions or to contribute by sending pull requests.
