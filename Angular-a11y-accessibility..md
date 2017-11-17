## Overview
Multiple studies suggest that around 15-20% of the population are living with a disability of some kind. In comparison, that number is higher than any single browser demographic currently, other than Chrome2. Not considering those users when developing an application means excluding a large number of people from being able to use it comfortable or at all.
   
Some people are unable to use the mouse, view a screen, see low contrast text, Hear dialogue or music and some people having difficulty to understanding the complex language.This kind of people needed the support like Keyboard support, screen reader support, high contrast text, captions and transcripts and Plain language support. This disability may change the from permanent to the situation. 
 
To make the internet more accessible to this kind of people. A11y is introduced in Angular. 
  * Write meaningful HTML
  * Enable the keyboard
  * Handle focus
  * Alert the user

## Meaningful HTML

HTML Radio Input
````Html
<label for="roundTrip">
   <input type="radio" id="roundTrip">
   Round Trip
 </label>
````

HTML checkbox Input
````Html
<label for="roundTrip">
   <input type="check" id="roundTrip">
   Round Trip
 </label>
````

HTML Button
````Html
<button>Button</button>
````

## Accessibility with Material angular
The a11y package provides a number of tools to improve accessibility. 
API reference for Angular CDK a11y
````Javascript
import {A11yModule} from '@angular/cdk/a11y';
````

## Types of key managers

* ListKeyManager
* FocusKeyManager 
* ActiveDescendantKeyManager

### ListKeyManager
ListKeyManager manages the active option in a list of items based on keyboard interaction. Intended to be used with components that correspond to a role="menu" or role="listbox" pattern . Any component that uses a ListKeyManager will generally do three things:
* Create a @ViewChildren query for the options being managed.
* Initialize the ListKeyManager, passing in the options.
* Forward keyboard events from the managed component to the ListKeyManager.

Each option should implement the ListKeyManagerOption interface:
````Javascript
interface ListKeyManagerOption {
  disabled?: boolean;
  getLabel?(): string;
}
````


## Common Accessibility Patterns

#### 1. Text alternatives: 
Add alternate text content to make visual information accessible using these W3C guidelines. The appropriate technique depends on the specific markup but can be accomplished using offscreen spans, aria-label or label elements, image alt attributes, figure/figcaption elements and more.

#### 2. HTML Semantics: 
 If you're creating custom element directives, Web Components or HTML in general, use native elements wherever possible to utilize built-in events and properties. Alternatively, use ARIA to communicate semantic meaning .

#### 3. Focus management:
 Guide the user around the app as views are appended/removed. Focus should never be lost, as this causes unexpected behavior and much confusion (referred to as "freak-out mode").

#### 4. Announcing changes:
When filtering or other UI messaging happens away from the user's focus, notify with ARIA Live Regions.

#### 5. Color contrast and scale:
Make sure content is legible and interactive controls are usable at all screen sizes. Consider configurable UI themes for people with color blindness, low vision or other visual impairments.

#### 6. Progressive enhancement:
Some users do not browse with JavaScript enabled or do not have the latest browser. An accessible message about site requirements can inform users and improve the experience

 
