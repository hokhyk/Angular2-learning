 
node.js installation on opensuse:

1、安装 npm
$ curl http://npmjs.org/install.sh | sh
$  npm --version
2.15.1ng
2、安装 TypeScript npm 包：
$ npm install -g typescript
安装完成后我们就可以使用 TypeScript 编译器，名称叫 tsc，可将编译结果生成 js 文件。
要编译 TypeScript 文件，可使用如下命令：
tsc filename.ts



#macOS:
1. brew install node
2. npm install -g cnpm --registry=https://registry.npm.taobao.org
3. cnpm i -g npm  //升级npm
4. cnpm install -g typescript
5. cnpm install -g --save-dev @angular/cli@latest
6.  brew install watchman
7.  brew install tree


# ng  app 
 ng --version
 ng --help
 ng set --global packageManager=cnpm
  ng new angular-hello-world
  tree -F -L 1
  cd angular-hello-world
  ng serve --port 4201
  
# ng component
1. ng generate component hello-world
  installing component
    create src/app/hello-world/hello-world.component.css
    create src/app/hello-world/hello-world.component.html
    create src/app/hello-world/hello-world.component.spec.ts
    create src/app/hello-world/hello-world.component.ts
    update src/app/app.module.ts

create src/app/hello-world/hello-world.component.ts:
     A basic Component has two parts:
    1. AComponentdecorator
    2. A component definition class
   
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-hello-world',
  templateUrl: './hello-world.component.html',
  styleUrls: ['./hello-world.component.css']
})
export class HelloWorldComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}

#ng build   for app deplayment
ng build --target=production --base-href 'the/path/under/the/wwwroot'

$brew install now
$cd dist
$now
now is an online deployment service.

#  way to develop angular2 apps:
But for now, bask in our success! Much of writing Angular apps is just as we did above:
1. Split your app into components 
2. Create the views
3. Define your models
4. Display your models
5. Add interaction

npm install -g tsun
tsun is a TypeScript Upgraded Node.

#  Emitting Custom Events
To create a custom output event we do three things:
1. Specify outputs in the @Component configuration
2. Attach an EventEmitter to the output property
3. Emit an event from the EventEmitter, at the right time

@Component({
   selector: 'single-component',
   template: `<button (click)=liked() >Like it?</button>`
})

class SingleComponent {
   @Output() putRingOnIt: EventEmitter<string>;
   
   constructor() {
     this.putRingOnIt = new EventEmitter();
   }
   
   liked(): void {
     this.putRingOnIt.emit("Event emitted.");
   }
}


@Component(
    selector: 'club',
    template: `
    <div>
    <single-component
       (putRingOnIt) = "ringWasPlaced($event)"
    >
    </single-component>
    </div>`
)
class ClubComponent {
    ringWasPlaced(event: string) {
        console.log(`Put your hand up: ${event}`);
    }
}


# modify css class in handy
The line [class.selected]="isSelected(myProduct)" is a fun one: Angular allows us to set classes conditionally on an element using this syntax. This syntax says “add the CSS class selected if isSelected(myProduct) returns true.” This is a really handy way for us to mark the currently selected product.

#Hostbinding()
export class ProductRowComponent {
  @Input() product: Product;
  @HostBinding('attr.class') cssClass = 'item';
}
The HostBinding decoration is new - it lets us set attributes on the host element. The host is the
element this component is attached to.
In this case, we’re using the Semantic UI item class42. Here when we say @HostBinding('attr.class')
cssClass = 'item'; we’re saying that we want to attach the CSS class item to the host element.

Using host is nice because it means we can configure our host element from within the component. This is great because otherwise we’d require the host element to specify the CSS tag and that is bad because we would then make assigning a CSS class part of the requirement to using the Component.

# tag input 
<img class="product-image" [src]="product.imageUrl">
or  <img src="{{ product.imageUrl }}">

# one way data binding : RxJS   Flux-based architecture
The recommended way in Angular, and in many modern web frameworks (such as React), is to adopt a pattern of one-way data binding. That is, your data flows only down through components. If you need to make changes, you emit events that cause changes to happen “at the top” which then trickle down.
One-way data binding can seem like it adds some overhead in the beginning but it saves a lot of complication around change detection and it makes your systems easier to reason about.
Thankfully there are two major contenders for managing your data architecture:
1. Use an Observables-based architecture like RxJS 
2. Use a Flux-based architecture

#internal directives
## ngIf
<div *ngIf="str == 'yes'"></div>

## ngSwitch
<div class="container" [ngSwitch]="myVar">
<div *ngSwitchCase="'A'">Var is A</div>
<div *ngSwitchCase="'B'">Var is B</div>
<div *ngSwitchDefault>Var is something else</div>
</div>

## ngStyle
<div [style.background-color]="'yellow'">
<div [ngStyle]="{color: 'white', 'background-color': 'blue'}">

##ngClass
<div [ngClass]="{bordered: false}">This is never bordered</div>
<div [ngClass]="{bordered: true}">This is always bordered</div>

@Component({
  selector: 'app-ng-class-example',
  templateUrl: './ng-class-example.component.html'
})
export class NgClassExampleComponent implements OnInit {
isBordered: boolean; classesObj: Object; classList: string[];
  constructor() {
  }
ngOnInit() {
this.isBordered = true; this.classList = ['blue', 'round']; this.toggleBorder();
}
toggleBorder(): void { this.isBordered = !this.isBordered; this.classesObj = {
bordered: this.isBordered };
}

<div [ngClass]="classesObj">
Using object var. Border {{ classesObj.bordered ? "ON" : "OFF" }}
</div>

be careful when you have class names that contains dashes, like bordered-box. JavaScript requires that object-literal keys with dashes be quoted like a string, as in:
1 <div[ngClass]="{'bordered-box':false}">...</div>

We can also use a list of class names to specify which class names should be added to the element.
 For that, we can either pass in an array literal:
<div class="base" [ngClass]="['blue', 'round']">
 This will always have a blue background and round corners
 This will always have a blue background and round corners
</div>
### [class.xxx_property]
<div class="field"
[class.error]="!sku.valid && sku.touched">


## ngFor
The syntax is *ngFor="let item of items".
<div class="ui list" *ngFor="let c of cities; let num = index">

## nonbindable
<span class="pre" ngNonBindable>
&larr; This is what {{ content }} rendered 
</span>



# Ng2 forms' directives in <form></form>
## ngForm
There are two important pieces of functionality that NgForm gives us:
1. AFormGroupnamedngForm
2. A(ngSubmit)output
e.g.
<form #f="ngForm" (ngSubmit)="onSubmit(f.value)"
First we have #f="ngForm". 
The #v=thing syntax says that we want to create a local variable for this view.

## FormGroup and Formcontrols
1.To create a new FormGroup and FormControls implicitly use:
• ngForm and
• ngModel
2.But to bind to an existing FormGroup and FormControls use: 
• formGroup and
• formControl
And we have to import 
import {
  FormBuilder,
  FormGroup
} from '@angular/forms';

export class DemoFormSkuWithBuilderComponent implements OnInit {
  myForm: FormGroup;
constructor(fb: FormBuilder) { this.myForm = fb.group({
      'sku': ['ABC123']
    });

• control - creates a new FormControl
• group - creates a new FormGroup

<form [formGroup]="myForm"  >
<label <input
for="skuInput">SKU</label>
type="text"
id="skuInput"
placeholder="SKU" [formControl]="myForm.controls['sku']">
</form>

myForm.valid  myForm.value  myForm.controls  myForm.touched 

# Validators
To use validators we need to do two things:
1. Assign a validator to the FormControl object
2. Check the status of the validator in the view and take action accordingly.

let control = new FormControl('sku', Validators.required);

<div *ngIf="!myForm.valid"

[formControl]="sku">
<div *ngIf="!sku.valid" 
class="ui error message">SKU is invalid</div>
<div *ngIf="sku.hasError('required')"


We used bracket-notation,
 e.g. myForm.controls['sku']. 
 We could also use the dot- notation,
  e.g myForm.controls.sku. 
  In general, be aware that TypeScript may give a warning if you use the dot-notation and the object is not properly typed (but that is not a problem here).

<label <input
for="skuInput">SKU</label>
type="text"
id="skuInput"
placeholder="SKU" [formControl]="myForm.controls['sku']">
<div *ngIf="!myForm.controls['sku'].valid" class="ui error message">
SKU is invalid</div>
<div *ngIf="myForm.controls['sku'].hasError('required')"

## custom validations
function skuValidator(control: FormControl): { [s: string]: boolean }
 { if (!control.value.match(/^123/)) {
 return {invalidSku: true}; }
}
This validator will return an error code invalidSku if the input (the control.value) does not begin
with 123.

### adding multiple validators to a single field using Validators.compose()
constructor(fb: FormBuilder) { this.myForm = fb.group({
      'sku':  ['', Validators.compose([
        Validators.required, skuValidator])]
});
<div *ngIf="sku.hasError('invalidSku')"
class="ui error message">SKU must begin with <span>123</span></div>

# watch for changes
To watch for changes on a control we:
1. get access to the EventEmitter by calling control.valueChanges.  
2. add an observer using the .subscribe method

constructor(fb: FormBuilder) { this.myForm = fb.group({
      'sku':  ['', Validators.required]
    });
this.sku = this.myForm.controls['sku'];
this.sku.valueChanges.subscribe( (value: string) => {
        console.log('sku changed to:', value);
      }
);
this.myForm.valueChanges.subscribe( (form: any) => {
        console.log('form changed to:', form);
      }
);
}

## ngModel
NgModel is a special directive: it binds a model to a form.
 ngModel is special in that it implements two-way data binding.
  Two-way data binding is almost always more complicated and difficult to reason about vs. one-way data binding.
   Angular is built to generally have data flow one-way: top- down.
    However, when it comes to forms, there are times where it is easier to opt-in to a two-way bind.
 <label <input
for="productNameInput">Product Name</label> type="text"
id="productNameInput"
placeholder="Product Name"
 [formControl]="myForm.get('productName')" 
 [(ngModel)]="productName">

# Dependeny Injection DI
## Within Angular’s DI system,
 instead of directly importing and creating a new instance of a class,
instead we will:
1. Create the dependency (e.g. the service class)
2. Configure the injection (i.e. register the injection with Angular in our NgModule)
3. Declare the dependencies on the receiving component

The first thing we do is create the service class, that is, the class that exposes some behavior we want
to use. This will be called the injectable because it is the thing that our components will receive via
the injection.
Reminder on terminology: a provider provides (creates, instantiates, etc.) the injectable (the thing
you want). In Angular when you want to access aninjectable youinject a dependency into a function
(often a constructor) and Angular’s dependency injection framework will locate it and provide it
to you.


## Provider Injector and Dependency
Dependency injection in Angular has three pieces:
• the Provider (also often referred to as a binding) maps a token (that can be a string or a class)
to a list of dependencies. It tells Angular how to create an object, given a token.
• the Injector that holds a set of bindings and is responsible for resolving dependencies and
injecting them when creating objects
• the Dependency that is what’s being injected

##defining the services or service providers.
1 import { Injectable } from '@angular/core';
2 @Injectable()
3 export class UserService {
4 }

## Providing Dependencies with NgModule
what we’d normally do is
• use NgModule to register what we’ll inject – these are called providers and
• use decorators (generally on a constructor) to specify what we’re injecting

### injecting a class
1 providers: [ UserService ]
This tells Angular that we want to provide a singleton instance of UserService whenever UserService is injected. Because this pattern is so common, the class by itself is actually shorthand notation
for the following, equivalent configuration:
@NgModule ...

1 providers: [
2 { provide: UserService, useClass: UserService }
3 ]

@Component ...
16  constructor(private userService: UserService) {
17 // empty because we don't have to do anything else!
18 }
Here we’re mapping the UserService class to the UserService token.
As we’ve seen above, in this case the injector will create a singleton behind the scenes and return
the same instance every time we inject it .

### injecting a value
Another way we can use DI is to provide a value, much like we might use a global constant. For
instance, we might configure an API Endpoint URL depending on the environment.
1 providers: [
2 { provide: 'API_URL', useValue: 'http://my.api.com/v1' }
3 ]

1 import { Inject } from '@angular/core';
2 3
export class AnalyticsDemoComponent {
4 constructor(@Inject('API_URL') apiUrl: string) {
5 // works! do something w/ apiUrl
6 }
7 }

### Using a factory   Configurable Services
9
@NgModule({
10 imports: [
11 CommonModule
12 ],
13 providers: [
14 {
15 // `AnalyticsService` is the _token_ we use to inject
16 // note, the token is the class, but it's just used as an identifier!
17 provide: AnalyticsService,
19 // useFactory is a function - whatever is returned from this function
20 // will be injected
21 useFactory() {
22
23 // create an implementation that will log the event
24 const loggingImplementation: AnalyticsImplementation = {
25 recordEvent: (metric: Metric): void => {
26 console.log('The metric is:', metric);
27 }
28 };
29
30 // create our new `AnalyticsService` with the implementation
31 return new AnalyticsService(loggingImplementation);
32 }
33 }
34 ],
35 declarations: [ ]
36 })
37 export class AnalyticsDemoModule { }

Here in providers we’re using the syntax:
1 providers: [
2 { provide: AnalyticsService, useFactory: () => ... }
3 ]
useFactory takes a function and whatever this function returns will be injected.
In useFactory we’re creating an AnalyticsImplementation object that has one function: recordEvent. recordEvent is where we could, in theory, configure what happens when an event is recorded.
Again, in a real app this would probably send an event to Google Analytics or a custom event logging
software.

### Factory Dependencies
Using a factory is the most powerful way to create injectables, because we can do whatever we want
within the factory function. Sometimes our factory function will have dependencies of it’s own。
@NgModule({
  imports: [
    CommonModule,
    HttpModule, // <-- added
  ],
  providers: [
    // add our API_URL provider
    { provide: 'API_URL', useValue: 'http://devserver.com' },
    {
      provide: AnalyticsService,

      // add our `deps` to specify the factory depencies
      deps: [ Http, 'API_URL' ],

      // notice we've added arguments here
      // the order matches the deps order
      useFactory(http: Http, apiUrl: string) {

        // create an implementation that will log the event
        const loggingImplementation: AnalyticsImplementation = {
          recordEvent: (metric: Metric): void => {
            console.log('The metric is:', metric);
            console.log('Sending to: ', apiUrl);
            // ... You'd send the metric using http here ...
          }
        };

        // create our new `AnalyticsService` with the implementation
        return new AnalyticsService(loggingImplementation);
      }
    },
  ],
  declarations: [ ]
})

# Dependency Injection in Apps
