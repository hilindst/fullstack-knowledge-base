# Animations

## InstaLL
You probably need to install the new animations package (running the command never hurts): npm install --save @angular/animations 
Add the BrowserAnimationsModule to your imports[] array in AppModule
This Module needs to be imported from @angular/platform-browser/animations' => import { BrowserAnimationsModule } from '@angular/platform-browser/animations' (in the AppModule!)
You then import trigger , state , style etc from @angular/animations instead of @angular/core 

### Implement
  Animatioins array in @component

      trigger(name, [animation:])

set up property
    state = "normal";  

inhtml 
    [@name]="property"


      @Component({
        animations: [
          trigger('divState', [
            state('normal', style({
              'background-color':'red', 
              transform: 'translateX(0)'f
            })),
            state('highlight', style({
              backgroundColor: 'blue';
              transform: 'translateX(100px)'
            })), 
            transition('normal => highlighted', animate(300)),
            transition('highlighted => normal', animate(800))
          ])
        ]
      })

      export class AppComponent {
        state = "normal";
      }

can apply multiple stages in transition
learn css animations