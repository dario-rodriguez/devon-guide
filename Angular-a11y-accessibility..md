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

## Common Accessibility Patterns

1. Text alternatives: 
Add alternate text content to make visual information accessible using these W3C guidelines. The appropriate technique depends on the specific markup but can be accomplished using offscreen spans, aria-label or label elements, image alt attributes, figure/figcaption elements and more.
