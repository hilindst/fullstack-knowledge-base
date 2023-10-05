# Components & Databinding

Splitting up components will allow you to keep things organized and make pieces easier to use elsewhere

# Property & Event Binding
 HTML elelments: native properties & events
 Directives: custom properties & events
 Components: custom properties & events

 ## Decorators 

 ### Binding to custom properties
    export class Component implements OnInit {
     element: {type: string, name: string, content: string};
     ngOnInit() {}
    }

--> app.component.ts and .html access that new component
  [element]

Allowing parent components to access child properties with DECORATORS
on Child Component:

    import { Component, OnInit, Input }from '@angular/core';

    @Component({
     unchanged
    })

    export class ServerElementComponent implements OnInit {
     @Input() element: {type: string, name: string, content: string};
     constructor(){}
     ngOnInt(){}
    }
in the app.component.html:
    <app-server-element *ngFor="let serverElement of serverElemennts"
    [element]="serverElement"></app-server-element>

 ## Alias
when you dont want to bind the same property/word inside the component and outside:
  
    @Input('srvElement) element: {type: string, name: string, content: string};

in the app.component.html this becomes:
 
    <app-server-element *ngFor="let serverElement of serverElemennts"
    [srvElement]="serverElement"></app-server-element>

## Binding to custom events

### Event Emitter---need more context
ts file
    export class...
      @Output() serverCreated = new EventEmitter<{serverName:string, serverContent: string}>(); 

      onAddServer(){
        this.serverCreated.emit({serverName: this.newServerName, serverContent: this.newServerContent});
      }

to make it listenable from outside @Output() and add "Output" to import from '@angular/core'

so in app component html it has and again Alias' can be applied to change property outside of child element
     <app-cockpit
      (serverCreated)="onServerAdded($event)"></app-cockpit>

## View Encapsulation

### Local References
 can be placed on any html element, #serverNameInput---referencing element with all its properties
 ONLY useable in that html template, but can be passed thru that TS
    <input type='text' class='form-control' #serverNameInput>

    onAddServr(nameInput: HTMLInputElement){
      this.serverCreated.emit({
        serverName: nameInput.value,
        serverContent: this.newServerContnent
      });
    }

## @ViewChild

If accessing inside of ngOnInit() it should look like in template:
    @ViewChild('serverContentInput', {static: true}) serverContentInput: ElementRef; 

so far...Component, OnInit, EventEmitter, Output, ViewChild, ElementRef from '@angular/core'


## ng-content  Directive
    <ng-content></ng-content>

re-usable widgets


## Lifecycle Hooks
ngOnChanges(changes: Simple Changes): called after a bound input property changes (@input)
ngOnInit: called once the component is initialized
ngDoCheck: called during every change detection run
ngAfterContentInit: called after content(ng-content) has been projected into view
ngAfterContentChecked: called every time the projected content has been checked
ngAfterViewInit: called after the component's view (and child view) has been initialized
ngAfterViewChecked: called every tiem the view (and child views) have been checked
ngOnDestroy: called once the component is about to be destroyed

Implement and import interfaces 
