## Overview
Multiple studies suggest that around 15-20% of the population are living with a disability of some kind. In comparison, that number is higher than any single browser demographic currently, other than Chrome2. Not considering those users when developing an application means excluding a large number of people from being able to use it comfortable or at all.
   
Some people are unable to use the mouse, view a screen, see low contrast text, Hear dialogue or music and some people having difficulty to understanding the complex language.This kind of people needed the support like Keyboard support, screen reader support, high contrast text, captions and transcripts and Plain language support. This disability may change the from permanent to the situation. 

## Key Concerns of Accessible Web Applications
*  **Semantic Markup** - Allows the application to be understood on a more general level rather than just details of whats being rendered
* **Keyboard Accessibility** - Applications must still be usable when using only a keyboard
* **Visual Assistance** - color contrast, focus of elements and text representations of audio and events

### Semantic Markup

### Keyboard Accessibility
Keyboard accessibility is the ability of your application to be interacted with using just a keyboard. The more streamlined the site can be used this way, the more keyboard accessible it is. Keyboard accessibility is one of the largest aspects of web accessibility since it targets:

* those with motor disabilities who can't use a mouse
* users who rely on screen readers and other assistive technology, which require keyboard navigation
* those who prefer not to use a mouse

#### Focus 
Keyboard interaction is driven by something called focus. In web applications, only one element on a document has focus at a time, and keypresses will activate whatever function is bound to that element.
Focus element border can be styled with CSS using the  outline  property, but it should not be removed. Elements can also be styled using the  :focus  psuedo-selector.

#### Tabbing
The most common way of moving focus along the page is through the  tab  key. Elements will be traversed in the order they appear in the document outline - so that order must be carefully considered during development. 
There is way change the default behaviour or tab order. This can be done through the  tabindex  attribute. The  tabindex  can be given the values:
* less than zero - to let readers know that an element should be focusable but not keyboard accessible
* 0 - to let readers know that that element should be accessible by keyboard
* greater than zero - to let readers know the order in which the focusable element should be reached using the keyboard. Order is calculated from lowest to highest.

#### Transitions
The majority of transitions that happen in an Angular application will not involve a page reload. This means that developers will need to carefully manage what happens to focus in these cases.
#### Example 
````Javascript
@Component({
  selector: 'ngc2-modal',
  template: `
    <div
      role="dialog"
      aria-labelledby="modal-title"
      aria-describedby="modal-description">
      <div id="modal-title">{{title}}</div>
      <p id="modal-description">{{description}}</p>
      <button (click)="close.emit()">OK</button>
    </div>
  `,
})
export class ModalComponent {
  constructor(private modal: ModalService, private element: ElementRef) { }

  ngOnInit() {
    this.modal.visible$.subscribe(visible => {
      if(visible) {
        setTimeout(() => {
          this.element.nativeElement.querySelector('button').focus();
        }, 0);
      }
    })
  }
}

````



## Accessibility with Material angular
The a11y package provides a number of tools to improve accessibility. 
API reference for Angular CDK a11y
````Javascript
import {A11yModule} from '@angular/cdk/a11y';
````


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
#### Types of ListKeyManager
 * FocusKeyManager 
 * ActiveDescendantKeyManager

### FocusKeyManager
Used when options will directly receive browser focus. Each item managed must implement the FocusableOption interface:
````Javascript
interface FocusableOption extends ListKeyManagerOption {
  focus(): void;
}
````
### ActiveDescendantKeyManager
Used when options will be marked as active via aria-activedescendant. Each item managed must implement the Highlightable interface:

````Javascript
interface Highlightable extends ListKeyManagerOption {
  setActiveStyles(): void;
  setInactiveStyles(): void;
}
````
Each item must also have an ID bound to the listbox's or menu's aria-activedescendant.

### FocusTrap
The cdkTrapFocus directive traps Tab key focus within an element. This is intended to be used to create accessible experience for components like modal dialogs, where focus must be constrained. This directive is declared in A11yModule.
This directive will not prevent focus from moving out of the trapped region due to mouse interaction.
#### Example
````HTML
<div class="my-inner-dialog-content" cdkTrapFocus>
  <!-- Tab and Shift + Tab will not leave this element. -->
</div>
````
### Regions
Regions can be declared explicitly with an initial focus element by using the cdkFocusRegionStart, cdkFocusRegionEnd and cdkFocusInitial DOM attributes. When using the tab key, focus will move through this region and wrap around on either end.

#### example:
````HTML
<a mat-list-item routerLink cdkFocusRegionStart>Focus region start</a>
<a mat-list-item routerLink>Link</a>
<a mat-list-item routerLink cdkFocusInitial>Initially focused</a>
<a mat-list-item routerLink cdkFocusRegionEnd>Focus region end</a>
````

### InteractivityChecker
InteractivityChecker is used to check the interactivity of an element, capturing disabled, visible, tabbable, and focusable states for accessibility purposes.

### LiveAnnouncer
LiveAnnouncer is used to announce messages for screen-reader users using an aria-live region.
Example 
````HTML
@Component({...})
export class MyComponent {

 constructor(liveAnnouncer: LiveAnnouncer) {
   liveAnnouncer.announce("Hey Google");
 }
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

 
