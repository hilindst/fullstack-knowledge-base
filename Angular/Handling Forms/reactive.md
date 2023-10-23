# Reactive Forms

import ReactiveFormsModule from @angular/forms

    export class AppComponent implements OnInit {
      genders = ['male', 'female']
      signupForm: FormGroup;

    }

    ngOnInit(){
      this.signupForm = new FormGroup({
        'username': new FormControl(null),
        'email': new FormControl(null),
        'gender': new FormControl('male')
      });
    }

    on tmeplate
      <form [formGroup]="signupForm">

      on inputs
      <input
      formControlName="username">

  ### Submit

html template
    <form [formGroup]="signupForm" (ngSubmit)="onSubmit()">
      onSubmit(){
        console.log(this.signupForm);
      }

### Validation
    
    export class AppComponent implements OnInit {
      signupForm: FormGroup;

    }

    ngOnInit(){
      this.signupForm = new FormGroup({
        'username': new FormControl(null, Validators.required),
        'email': new FormControl(null, [Validators.required, Validators.email]),
        'gender': new FormControl('male')
      });
    }

### Creating Custom Validators
    export class AppComponent implements OnInit {
        genders = ['male', 'female']
        signupForm: FormGroup;
        forbiddenUsernames = ['Chris,'Anna'];

    }

    
    ngOnInit(){
      this.signupForm = new FormGroup({
        'userData': new FormGroup({
          'username': new FormControl(null, [Validators.required, this.forbiddenNames.bind(this)]),
          'email': new FormControl(null, [Validators.required, Validators.email])
        }),
        'gender': new FormControl('male')
      });
    }

    forbiddenNames(control: FormControl): {[s: string]: boolean}{
      if (this.forbiddenUsernames.indexof(control.value) !== -1){
        return {'nameIsForbidden': true};
      }
        return null;
    }


### Accessing controls
beneath username input html
    <span *ngIf="!signupForm.get('username').valid && signupForm.get('username').touched" class="help-block">Please enter a valid username!</span>

## Groupin Form Groups
       export class AppComponent implements OnInit {
      signupForm: FormGroup;

    }

    ngOnInit(){
      this.signupForm = new FormGroup({
        'userData': new FormGroup({
          'username': new FormControl(null, Validators.required),
          'email': new FormControl(null, [Validators.required, Validators.email])
        }),
        'gender': new FormControl('male')
      });
    }

 in html  
    <form [formGroup]="signupForm" (ngSubmit)="onSubmit()"> 
      <div formGroupName="userData">
        <div class="form-group">
//path needs to be updated
        <span *ngIf="!signupForm.get('userData.username').valid && signupForm.get('userData.username').touched" class="help-block">Please enter a valid username!</span>

## Arrays of Form Controls
     ngOnInit(){
      this.signupForm = new FormGroup({
        'userData': new FormGroup({
          'username': new FormControl(null, Validators.required),
          'email': new FormControl(null, [Validators.required, Validators.email])
        }),
        'gender': new FormControl('male'),
        'hobbies': new FormArray([])
      });
    }

    onAddHobby(){
      const control = new FormControl(null, Validators.required);
      (<FormArray>this.signupForm.get('hobbies')).push(control);
    }

// in html
    <div formArrayName="hobbies">
      <h4>Your Hobbies</h4>
      <button class="btn btn-default" type="button" (click)="onAddHobby()">Add HObby</button>    
      <div class="form-group" *ngFor="let hobbyControl of signupForm.get('hobbies').controls; let i = index">
      <input type="text" class="form-control" [formControlName]="i">

### Finetuning Errror Codes
    <span *ngIf="!signupForm.get('userData.username').valid && signupForm.get('userData.username').touched" class="help-block">Please enter a valid username!</span>
    <span *ngIf="signupForm.get('userData.username').errors['nameIsForbidden']">This name is invalid</span>
    <span *ngIf="signupForm.get('userData.username').errors['required']">This field is required</span>
    

## Async Validator
  ngOnInit(){
      this.signupForm = new FormGroup({
        'userData': new FormGroup({
          'username': new FormControl(null, Validators.required),
          'email': new FormControl(null, [Validators.required, Validators.email], this.forbiddenEmails)
        }),
        'gender': new FormControl('male'),
        'hobbies': new FormArray([])
      });
    }
 
    forbiddenEmails(control: FormControl); Promise<any> | Observable<any>   {
      const promise = new Promise<any>((resolve, reject)=> {
        setTimeout(() => {
          if (control.value === 'test@test.com') {
            resolve({'emailIsForbidden': true});
          } else{
            resolve(null);
          }
      }, 1500);  //timeout is simulating reaching out to webserver
      });
      return promise;
    } 

## Reacting to Status or Value Changes
    ngOnInit(){
        this.signupForm = new FormGroup({
          'userData': new FormGroup({
            'username': new FormControl(null, Validators.required),
            'email': new FormControl(null, [Validators.required, Validators.email], this.forbiddenEmails)
          }),
          'gender': new FormControl('male'),
          'hobbies': new FormArray([])
        });
        this.signupForm.valueChanges.subscribe((value) => console.log(value) );

        or 

        this.signupForm.statusChanges.subscribe((status) => console.log(status));
    }
 
### Set or Patch Values
    ngOnInit(){
        this.signupForm = new FormGroup({
          'userData': new FormGroup({
            'username': new FormControl(null, Validators.required),
            'email': new FormControl(null, [Validators.required, Validators.email], this.forbiddenEmails)
          }),
          'gender': new FormControl('male'),
          'hobbies': new FormArray([])
        });
       this.signupForm.setValue({
        'userData": {
          'username': 'Max',
          'email': 'max@test.com'
        },
        'gender': 'male',
        'hobbies': []
       });

       // to update just a part
      this.signupForm.patchValue({
        'userData": {
          'username': 'Charlie',
        }});
       
    }
    onSubmit(){
      console.log(this.signupForm);
      this.signupForm.reset();
    }