## Overview
Multiple studies suggest that around 15-20% of the population are living with a disability of some kind. In comparison, that number is higher than any single browser demographic currently, other than Chrome2. Not considering those users when developing an application means excluding a large number of people from being able to use it comfortable or at all.
   
Some people are unable to use the mouse, view a screen, see low contrast text, Hear dialogue or music and some people having difficulty to understanding the complex language.This kind of people needed the support like Keyboard support, screen reader support, high contrast text, captions and transcripts and Plain language support. This disability may change the from permanent to the situation. 
 
To make the internet more accessible to this kind of people A11y in Angular is introduced. 
  * Write meaningful HTML
  * Enable the keyboard
  * Handle focus
  * Alert the user

##  Meaningful HTML

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

## Accessibility with ngAria

The goal of ngAria is to improve AngularJS's default accessibility by enabling common ARIA attributes that convey state or semantic information for assistive technologies used by persons with disabilities. 

### ngAria module
 ngAria hooks into standard AngularJS directives and quietly injects accessibility support into your application at runtime. Most of what ngAria does is only visible "under the hood". To see the module in action, once you've added it as a dependency, you can test a few things:
* Using your favorite element inspector, look for attributes added by ngAria in your own code.
* Test using your keyboard to ensure tabindex is used correctly.
* Fire up a screen reader such as VoiceOver or NVDA to check for ARIA support.

````Javascript
angular.module('myApp', ['ngAria'])...
````

### ngAria supported directives
* ngModel
* ngDisabled
* ngRequired
* ngReadonly
* ngChecked
* ngValue
* ngShow
* ngHide
* ngClick
* ngDblClick
* ngMessages

#### ngModel
Much of ngAria's heavy lifting happens in the ngModel directive. For elements using ngModel, special attention is paid by ngAria if that element also has a role or type of checkbox, radio, range or textbox .For those elements using ngModel, ngAria will dynamically bind and update the following ARIA attributes (if they have not been explicitly specified by the developer):
* aria-checked
* aria-valuemin
* aria-valuemax
* aria-valuenow
* aria-invalid
* aria-required
* aria-readonly
* aria-disabled
````Html
<form>
  <custom-checkbox role="checkbox" ng-model="checked" required
      aria-label="Custom checkbox" show-attrs>
    Custom checkbox
  </custom-checkbox>
</form>
<hr />
<b>Is checked:</b> {{ !!checked }}
````
#### ngValue and ngChecked
To ease the transition between native inputs and custom controls, ngAria now supports ngValue and ngChecked. The original directives were created for native inputs only, so ngAria extends support to custom elements by managing aria-checked for accessibility.

 Example
````Html
<custom-checkbox ng-checked="val"></custom-checkbox>
<custom-radio-button ng-value="val"></custom-radio-button>
````
Becomes:
````Html
<custom-checkbox ng-checked="val" aria-checked="true"></custom-checkbox>
<custom-radio-button ng-value="val" aria-checked="true"></custom-radio-button>
````
#### ngDisabled
The disabled attribute is only valid for certain elements such as button, input and textarea. To properly disable custom element directives such as <md-checkbox> or <taco-tab>, using ngAria with ngDisabled will also add aria-disabled. This tells assistive technologies when a non-native input is disabled, helping custom controls to be more accessible.

##### Example
````Html
<md-checkbox ng-disabled="disabled"></md-checkbox>
````
##### Becomes:
````Html
<md-checkbox disabled aria-disabled="true"></md-checkbox>
````

#### ngReadonly
The boolean readonly attribute is only valid for native form controls such as input and textarea. To properly indicate custom element directives such as <md-checkbox> or <custom-input> as required, using ngAria with ngReadonly will also add aria-readonly. This tells accessibility APIs when a custom control is read-only.

##### Example
````HTML
<md-checkbox ng-readonly="val"></md-checkbox>
````

#### ngRequired
The boolean required attribute is only valid for native form controls such as input and textarea. To properly indicate custom element directives such as <md-checkbox> or <custom-input> as required, using ngAria with ngRequired will also add aria-required. This tells accessibility APIs when a custom control is required.
Example
````HTML
<md-checkbox ng-required="val"></md-checkbox>
````
Becomes:
````HTML
<md-checkbox ng-required="val" aria-required="true"></md-checkbox>
````
#### ngShow
The ngShow directive shows or hides the given HTML element based on the expression provided to the ngShow attribute. The element is shown or hidden by removing or adding the .ng-hide CSS class onto the element.

Example
````HTML
.ng-hide {
  display: block;
  opacity: 0;
}
````
````HTML
<div ng-show="false" class="ng-hide" aria-hidden="true"></div>
````
````HTML
<div ng-show="false" class="ng-hide" aria-hidden="true"></div>
````
Becomes:
<div ng-show="true" aria-hidden="false"></div>

#### ngClick and ngDblclick
If ng-click or ng-dblclick is encountered, ngAria will add tabindex="0" to any element not in a node blacklist: Button Anchor Input Textarea Select Details/Summary To fix widespread accessibility problems with ng-click on div elements, ngAria will dynamically bind a keypress event by default as long as the element isn't in the node blacklist. You can turn this functionality on or off with the bindKeypress configuration option. ngAria will also add the button role to communicate to users of assistive technologies. This can be disabled with the bindRoleForClick configuration option. For ng-dblclick, you must still manually add ng-keypress and a role to non-interactive elements such as div or taco-button to enable keyboard access.
Example
````HTML
html <div ng-click="toggleMenu()"></div> Becomes: html <div ng-click="toggleMenu()" tabindex="0"></div>
````
#### ngMessages
The ngMessages module makes it easy to display form validation or other messages with priority sequencing and animation. To expose these visual messages to screen readers, ngAria injects aria-live="assertive", causing them to be read aloud any time a message is shown, regardless of the user's focus location.
Example
````HTML
<div ng-messages="myForm.myName.$error">
  <div ng-message="required">You did not enter a field</div>
  <div ng-message="maxlength">Your field is too long</div>
</div>
````
Becomes:
````HTML
<div ng-messages="myForm.myName.$error" aria-live="assertive">
  <div ng-message="required">You did not enter a field</div>
  <div ng-message="maxlength">Your field is too long</div>
</div>
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

 
