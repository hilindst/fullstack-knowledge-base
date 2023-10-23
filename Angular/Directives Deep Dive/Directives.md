# Directives

## Attribute
element properties

[ngClass]
[ngStyle]

## Structural
whole DOM and view container
* means structural directive
only 1 structural directive allowed on single element

like *ngFor    can be called on that element or child elements
  *ngIf

### Making your Own!

### Attribute Directive-dIY

folder-->file-->directive.ts
    import {Directive, OnInit} from '@angular/core';

    @Directive({
      selector: '[appBasicHighlight]'  //[] tells angular to select as an attribute on an element 
    })
    
    export class BasicHighlightDirective implements OnInit {
      constructor(private elementRef: ElementRef){
    }
    ngOnInit(){
      this.elementRef.nativeElement.style.backgroundColor = 'green';
    }
    }

directive goes under declarations on app module

## Renderer2 
  Better method for highlight directive
  import {Directive, OnInit, ElementRef, Renderer2 } from '@angular/core';

    @Directive({
      selector: '[appBetterHighlight]'  //[] tells angular to select as an attribute on an element 
    })
    
    export class BetterHighlightDirective implements OnInit {
      constructor(private elRef, ElementRef, renderer: Renderer2){
    }
    ngOnInit(){
      this.renderer.setStyle(this.elRef.nativeElement, 'background-color', 'blue', false, false); //fourth value optional
    }
    }

## HostListner Version
all events available to event listener (click, mouseenter, etc)

  import {Directive, OnInit, ElementRef, Renderer2, HostListener } from '@angular/core';

    @Directive({
      selector: '[appBetterHighlight]'  //[] tells angular to select as an attribute on an element 
    })
    
    export class BetterHighlightDirective implements OnInit {
      constructor(private elRef, ElementRef, renderer: Renderer2){
    }
    ngOnInit(){
      
    }
    @HostListener('mouseenter') mouseover(eventDate: Event){
      this.renderer.setStyle(this.elRef.nativeElement, 'background-color', 'blue', false, false);

      @HostListener('mouseleave') mouseleave (eventDate: Event){
        this.renderer.setStyle(this.elRef.nativeElement, 'background-color', 'transparent', false, false);
    }
    }

## Hostbinding
    import {Directive, OnInit, ElementRef, Renderer2, HostListener, Hostbinding } from '@angular/core';

    @Directive({
      selector: '[appBetterHighlight]'  //[] tells angular to select as an attribute on an element 
    })
    
    export class BetterHighlightDirective implements OnInit {
      @HostBinding('style.backgroundColor') backgroundColor: string = 'transparent';

      constructor(private elRef, ElementRef, renderer: Renderer2){
      }
      ngOnInit(){  
      }
      @HostListener('mouseenter') mouseover(eventDate: Event){
        this.backgroundColor = 'blue';
      }
      @HostListener('mouseleave') mouseleave (eventDate: Event){
        this.backgroundColor = 'transparent';
      }
    }
### Custom Property Binding
    import {Directive, OnInit, ElementRef, Renderer2, HostListener, Hostbinding } from '@angular/core';

    @Directive({
      selector: '[appBetterHighlight]'  //[] tells angular to select as an attribute on an element 
    })
    
    export class BetterHighlightDirective implements OnInit {
      @Input() defaultColor: string = 'transparent';
      @Input() highlightColor: string = 'blue';
      @HostBinding('style.backgroundColor') backgroundColor: string;

      constructor(private elRef, ElementRef, renderer: Renderer2){
      }
      ngOnInit(){  
        this.backgroundColor = this.defaultColor; //prevents initializing bug
      }
      @HostListener('mouseenter') mouseover(eventDate: Event){
        this.backgroundColor = this.highlightColor;
      }
      @HostListener('mouseleave') mouseleave (eventDate: Event){
        this.backgroundColor = this.defaultColor;
      }
    }

    in html
      <p appBetterHighlight [defaultColor]="'yellow'" [highlightColor]="'red'">yes</p>

### Structural Directive DIY

unless.directive.ts

import {Directive, Input, TemplateRef, ViewContainerRef} from '@angular/core';

    @Directive({
      selector: '[appUnless]'  //[] tells angular to select as an attribute on an element 
    })
    
    export class UnlessDirective {
      @Input() set appUnless(condition/value: boolean){  
        if(!condition) {
          this.vcRef.createEmbeddedView(this.templateRef);
        } else {
          this.vcRef.clear();
        }       
    }
    constructor(private templateRef: TemplateRef<any>, private vcRef: ViewContainerRef){}
    }

in html
    <div *appUnless="onlyOdd">
      <li
      class="list-group-item"
      [ngClass]="{odd: even % 2 !==0}"
      [ngStyle]="{backgroundColor: even % 2 !==0 ? 'yellow' : 'transparent'}"
      *ngFor="let even of even Numbers">
      {{even}}
      </li>
    </div>


## Structural Directive *ngSwitch
  property bind [ngSwitch]="value"
  can use to show specific elements
    <div [ngSwitch]="value">
      <p *ngSwitchCase="5"> Value is 5 </p>
      <p *ngSwitchCase="10"> Value is 10 </p>
      <p *ngSwitchDefault> Value is default </p>
    
changing value in component.ts changes what is shown
can be a better solution than *ngIf

#Questions
    [] property element binding?
    Element Ref not working....why use both elementref and renderer2?

