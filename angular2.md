 
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
 
 
