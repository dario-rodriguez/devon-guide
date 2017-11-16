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

 
