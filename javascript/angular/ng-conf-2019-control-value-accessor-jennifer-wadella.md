# The Control Value Accessor

#### Jennifer Wadella | ng-conf | 2019 | [YouTube](https://youtu.be/kVbLSN0AW-Y)

> Defines an interface that acts as a bridge between the angular forms API and a native element in the DOM
>
> — cvs source code

## Reactive Form Crash Course

* **Reactive Form** - a model-driven approach to handling form inputs whose values change over time

* **Form Control** - basic form building blocks to create input/radio/select/etc.. *Tracks the value and validation status*

* **Form Group** - a group of form controls

* **FormControlName** - directive that ties form element to a formControl



```javascript
this.galaxyForm = new FormGroup({
	galaxy: new FormControl({value: null, disabled: false}, [Validators.required]),
  rating: new FormControl({value: null, disabled: true}, [Validators.required]),
  name: new FormControl({value: null, disabled: false}, [Validators.required])
});
```

The CVS interface:

```javascript
interface ControlValueAccessor {
  writeValue(obj: any): void
  registerOnChange(fn: any): void
  registerOnTouched(fn: any): void
  setDisabledState(isDisabled: boolean)?: void
}
```

Remember that in Typescript, you will get errors if you do not implement all of the method of the interface that is being implemented.

Breaking down the interface

```javascript
writeValue(obj: any): void
```

* writes a **new value** to the element
* will be called 
  * when `FormControl` is **instantiated** AND
  * when `patchValue` or `setValue` is called

```javascript
registerOnChange(fn: any): void
```

* registers a callback function
* callback function is called when the control's value in the UI has changed

```javascript
registerOnTouched(fn: any): void
```

* registers a callback function
* callback function is called by the forms API on initialization to update the form model on blur

```javascript
setDisabledState(isDisabled: boolean)?: void
```

* called by the forms API when the control status changes to or from **DISABLED**. 
* this will be called when the `FormControl` is instantiated *IF*
  * the disabled key is present **AND**
  * when `.enable()` or `.disable()` is called

## How to implement

The `NG_VALUE_ACCESSOR` is used to register the component as a provider for the `controlValueAccessor`. Because it is being registered early, use of `forwardRef` is needed to refer to it

```javascript
import { Component, forwardRef } from '@angular/core';
import { NG_VALUE_ACCESSOR } from '@angular/forms';

@Component({
	selector: 'cvs-example',
	template: `<input (change)="exampleChange($event)" type="text">`,
	providers: [{
		provide: NG_VALUE_ACCESSOR,
		useExisting: forwardRef(() => CVSExampleComponent),
		multi: true
	}]
})
export class CVAEXampleComponent {}

```

*(This is similar to the example she used, I only modified to get it to sync with my thought process)*

In order to make use of the typescript interface, we need to implement it:

```javascript
export class CVAExampleComponent implements ControlValueAccessor {}
```

After this, the methods from the interface need to be implemented:

```javascript
export class CVAExampleComponent implements ControlValueAccessor {
	writeValue() {}
	registerOnChange() {}
	registerOnTouched() {}
	setDisabledState() {}
}
```

Of course, there will be more to the component that what is seen here, but the important thing to take note to is that the interface is completely implemented. Otherwise, you will get errors thrown your way from TS.

### Key Concepts

* CVA is great for granular control of displaying to the UI & communication with the forms API
* Keep your wrapper components dumb
* Just input and output form values!
* Leave validation logic to the parent form component
* CVA can be used with any form API
  * including template driven

---

### Extra

* [Slides](https://tehfedaykin.github.io/WormholesandCVAs)

* [Galaxy Rating App](https://github.com/tehfedaykin/galaxy-rating-app)

  

