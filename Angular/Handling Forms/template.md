la# Forms

## template-driven
angular infers the form object from the DOM

## reactive
form is created programmatically and synchronized with the DOM

## setting up
import {formsmodule} from angular/forms

## Template Driven Example
in html to tell angular what control is using ngModel and name=""
    <input
    ngModel
    name="username">

in ts new method
    onSubmit(form: NgForm){

    }

best place to submit in html: tells angular please give me access to this form you created automatically
    <form (ngSubmit)="onSubmit(f)" #f="ngForm">    


## NgForm object
has a lot of properties

### @Viewchild 
alternative

    @ViewChild('f') signupForm: NgForm;

    onSubmit() {

    }


### Validation
    <input
    required>

built-in Validator directives list somewhere
like for email addresses

creating a disabled submit button
"f" referring to the #f="ngForm"

    <button
      class="btn btn-primary"
      type="submit"
      [disabled]="!f.valid">Submit</button>

and adding red borders to invalid things on .css
    input.ng-invalid.ng-touched, select.ng-invalid, etc {
      border: 1px solid red;
    }

and adding help messages for user
ngmodel 
    <input
      #email="ngModel">
    <span class="help-block" *ngIf="!email.valid && email.touched">Please enter a valid email!</span>    

warning line appears under email input if touched and nothing added or invalid email entered

## ngModelGroup

    <div id="user-data" ngModelGroup="userData">

### Radio Buttons

Creates a databound radio button input
    <div class="radio" *ngFor="let gender of genders">
      <label>
        <input
          type="radio'
          name="gender"
          ngModel
          [value]="gender">
          {{ gender }} </label>

###  .setValue()  to fill forms and override values

### 
    @ViewChild('f') signupForm: NgForm;

    suggestUserName(){
      this.signupForm.form.patchValue({
        userData: {
          username: suggestedName
        }
      });
    }

## Using Form Data
    user = {
      username:'',
      email: '',

    }
    submitted = false;

    onSubmit(){
      this.submitted = true;
      this.user.username = this.signupForm.value.userData.username;
      etc.
    }

    referring to name="" in html div 

    and in shown response in html
      <div *ngIf="submitted">
        <h3>Your Data</h3>
        <p>Username: {{ user.username }}</p>

## Reset Form
    onSubmit(){
      ...(all those other values)
      this.signupForm.reset();
    }
  
### creating a form

    <form>
      <div class="form-group">   CSS class for looks (not angular)
        <label for="email">Mail</label>
        <input type="email" id="email" ngModel name="email" required email class="form-control" #email="ngModel">
        <span class="help-block" *ngIf="!email.valid && email.touched">Please enter a valid email!</span>

      <div class="form-group">
        <label for="subscription">Choose a Subscription</label>
        <select name="subscription" id="subscription" [ngModel]="selectedSubscription" class="form-control">
        <option *ngFor="let subscription of subscriptions"
        [value]="subscription">{{ subscription }}</option>

        in .ts
        subscriptions = ['Basic', 'Advanced', 'Pro'];
        selectedSubscription = 'Advanced';
        @ViewChild('signupForm') sgnForm: NgForm;

        OnSubmit(){
          console.log(this.sgnForm.value);
        }
