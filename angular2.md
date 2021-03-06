 
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
3. sudo ln -s /usr/local/bin/cnpm /usr/bin/cnpm
4. cnpm i -g npm  //升级npm
5. cnpm install -g typescript
6. cnpm install -g --save-dev @angular/cli@latest
7.  brew install watchman
8.  brew install tree


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



#http
##  http related DI
import { HttpModule } from '@angular/http';

import {
  Injectable,
  Inject
} from '@angular/core';
import { Http, Response } from '@angular/http';
import { Observable } from 'rxjs/Observable';
import { SearchResult } from './search-result.model';
export const YOUTUBE_API_KEY = 'AIzaSyDOfT_BO81aEZScosfTYMruJobmpjqNeEk';
export const YOUTUBE_API_URL = 'https://www.googleapis.com/youtube/v3/search';

/**
 * YouTubeService connects to the YouTube API
 * See: * https://developers.google.com/youtube/v3/docs/search/list
 */
@Injectable()
export class YouTubeSearchService {
  constructor(private http: Http,
    @Inject(YOUTUBE_API_KEY) private apiKey: string,
    @Inject(YOUTUBE_API_URL) private apiUrl: string) {
    }

    search(query: string): Observable<SearchResult[]> {
      const params: string = [
        `q=${query}`,
        `key=${this.apiKey}`,
        `part=snippet`,
        `type=video`,
        `maxResults=10`
      ].join('&');
      const queryUrl = `${this.apiUrl}?${params}`;
      return this.http.get(queryUrl)
      .map((response: Response) => {
        return (<any>response.json()).items.map(item => {
          // console.log("raw item", item); // uncomment if you want to debug
          return new SearchResult({
            id: item.id.videoId,
            title: item.snippet.title,
            description: item.snippet.description,
            thumbnailUrl: item.snippet.thumbnails.high.url
          });
        });
      });
    }
  }

## (<any>response.json()).items.
We’re telling TypeScript that we’re not interested in doing strict type checking.
When working with a JSON API, we don’t generally have typing definitions for the API
responses, and so TypeScript won’t know that the Object returned even has an items key,
so the compiler will complain.
We could call response.json()["items"] and then cast that to an Array etc., but in this
case (and in creating the SearchResult, it’s just cleaner to use an any type, at the expense
of strict type checking.

## http related event,  rxjs,
import {
  Component,
  OnInit,
  Output,
  EventEmitter,
  ElementRef
} from '@angular/core';

// By importing just the rxjs operators we need, We're theoretically able
// to reduce our build size vs. importing all of them.
import { Observable } from 'rxjs/Observable';
import 'rxjs/add/observable/fromEvent';
import 'rxjs/add/operator/map';
import 'rxjs/add/operator/filter';
import 'rxjs/add/operator/debounceTime';
import 'rxjs/add/operator/do';
import 'rxjs/add/operator/switch';

import { YouTubeSearchService } from './you-tube-search.service';
import { SearchResult } from './search-result.model';

@Component({
  selector: 'app-search-box',
  template: `
    <input type="text" class="form-control" placeholder="Search" autofocus>
  `
})
export class SearchBoxComponent implements OnInit {
  @Output() loading: EventEmitter<boolean> = new EventEmitter<boolean>();
  @Output() results: EventEmitter<SearchResult[]> = new EventEmitter<SearchResult[]>();

  constructor(private youtube: YouTubeSearchService,
              private el: ElementRef) {
  }

  ngOnInit(): void {
    // convert the `keyup` event into an observable stream
    Observable.fromEvent(this.el.nativeElement, 'keyup')
      .map((e: any) => e.target.value) // extract the value of the input
      .filter((text: string) => text.length > 1) // filter out if empty
      .debounceTime(250)                         // only once every 250ms
      .do(() => this.loading.next(true))         // enable loading
      // search, discarding old events if new input comes in
      .map((query: string) => this.youtube.search(query))
      .switch()
      // act on the return of the search
      .subscribe(
        (results: SearchResult[]) => { // on sucesss
          this.loading.next(false);
          this.results.next(results);
        },
        (err: any) => { // on error
          console.log(err);
          this.loading.next(false);
        },
        () => { // on completion
          this.loading.next(false);
        }
      );
  }
}
### @Output() loading:
The two @Outputs specify that events will be emitted from this component. That is, we can use the
(output)="callback()" syntax in our view to listen to events on this component.
1 <app-search-box
2 (loading)="loading = $event"
3 (results)="updateResults($event)"
4 ></app-search-box>

### OnInit    ngOnInit（)
his class implements OnInit because we want to use the ngOnInit lifecycle callback. If
a class implements OnInit then the ngOnInit function will be called after the first change detection
check.
ngOnInit is a good place to do initialization (vs. the constructor) because inputs set on a component
are not available in the constructor.
Here we create the EventEmitters for both loading and the results. loading will emit a boolean
when this search is loading and results will emit an array of SearchResults when the search is
finished.

### the element el ElementRef
2. The element el that this component is attached to. el is an object of type ElementRef, which
   is an Angular wrapper around a native element.

#### Angular 4 ElementRef
[https://segmentfault.com/a/1190000008653690]
Angular 的口号是 - "一套框架，多种平台。同时适用手机与桌面 (One framework.Mobile & desktop.)"，即 Angular 是支持开发跨平台的应用，比如：Web 应用、移动 Web 应用、原生移动应用和原生桌面应用等。

为了能够支持跨平台，Angular 通过抽象层封装了不同平台的差异，统一了 API 接口。如定义了抽象类 Renderer 、抽象类 RootRenderer 等。此外还定义了以下引用类型：ElementRef、TemplateRef、ViewRef 、ComponentRef 和 ViewContainerRef 等。下面我们就来分析一下 ElementRef 类：

ElementRef的作用
在应用层直接操作 DOM，就会造成应用层与渲染层之间强耦合，导致我们的应用无法运行在不同环境，如 web worker 中，因为在 web worker 环境中，是不能直接操作 DOM。有兴趣的读者，可以阅读一下 Web Workers 中支持的类和方法 这篇文章。通过 ElementRef 我们就可以封装不同平台下视图层中的 native 元素 (在浏览器环境中，native 元素通常是指 DOM 元素)，最后借助于 Angular 提供的强大的依赖注入特性，我们就可以轻松地访问到 native 元素。

ElementRef的定义
export class ElementRef {
  public nativeElement: any;
  constructor(nativeElement: any) { this.nativeElement = nativeElement; }
}
ElementRef的应用
我们先来介绍一下整体需求，我们想在页面成功渲染后，获取页面中的 div 元素，并改变该 div 元素的背景颜色。接下来我们来一步步，实现这个需求。

### events, observable RxJS
   turn the keyup events into an observable stream.

####  rxjs  Rx.Observable.fromEvent
RxJS provides a way to listen to events on an element using Rx.Observable.fromEvent.
// convert the `keyup` event into an observable stream
  Observable.fromEvent(this.el.nativeElement, 'keyup')
Notice that in fromEvent:
• the first argument is this.el.nativeElement (the native DOM element this component is
attached to)
• the second argument is the string 'keyup', which is the name of the event we want to turn
into a stream

#### chain functions on to steam.
We can now perform some RxJS magic over this stream to turn it into SearchResults. Let’s walk
through step by step.
Given the stream of keyup events we can chain on more methods. In the next few paragraphs we’re
going to chain several functions on to our stream which will transform the stream. Then at the end
we’ll show the whole example together.
First, let’s extract the value of the input tag:
1 .map((e: any) => e.target.value) // extract the value of the input
Above says, map over each keyup event, then find the event target (e.target, that is, our input
element) and extract the value of that element. This means our stream is now a stream of strings.
Next:
HTTP 203
1 .filter((text: string) => text.length > 1)
This filter means the stream will not emit any search strings for which the length is less than one.
You could set this to a higher number if you want to ignore short searches.
1 .debounceTime(250)
debounceTime means we will throttle requests that come in faster than 250ms. That is, we won’t
search on every keystroke, but rather after the user has paused a small amount.
1 .do(() => this.loading.next(true)) // enable loading
Using do on a stream is a way to perform a function mid-stream for each event, but it does not
change anything in the stream. The idea here is that we’ve got our search, it has enough characters,
and we’ve debounced, so now we’re about to search, so we turn on loading.
this.loading is an EventEmitter. We “turn on” loading by emitting true as the next event. We emit
something on an EventEmitter by calling next. Writing this.loading.next(true) means, emit a
true event on the loading EventEmitter. When we listen to the loading event on this component,
the $event value will now be true (we’ll look more closely at using $event below).
1 .map((query: string) => this.youtube.search(query))
2 .switch()
We use .map to call perform a search for each query that is emitted. By using switch we’re,
essentially, saying “ignore all search events but the most recent”. That is, if a new search comes
in, we want to use the most recent and discard the rest.

### events, subscribe
  subscribe takes three arguments: onSuccess, onError, onCompletion.
47 .subscribe(
48 (results: SearchResult[]) => { // on sucesss
49 this.loading.next(false);
50 this.results.next(results);
51 },
52 (err: any) => { // on error
53 console.log(err);
54 this.loading.next(false);
55 },
56 () => { // on completion
57 this.loading.next(false);
58 }
59 );
60 }

## import Http Response RequestOptions Headers
import { Component, OnInit } from '@angular/core';
import {
  Http,
  Response,
  RequestOptions,
  Headers
} from '@angular/http';

## http, making a POST request.
Making POST request with @angular/http is very much like making a GET request except that we
have one additional parameter: a body.
23 makePost(): void {
24 this.loading = true;
25 this.http.post(
26 'http://jsonplaceholder.typicode.com/posts',
27 JSON.stringify({
28 body: 'bar',
29 title: 'foo',
30 userId: 1
31 }))
32 .subscribe((res: Response) => {
33 this.data = res.json();
34 this.loading = false;
35 });
36 }

Notice in the second argument we’re taking an Object and converting it to a JSON string using
JSON.stringify

## http,  PUT/PATCH/DELETE/HEAD
There are a few other fairly common HTTP requests and we call them in much the same way.
• http.put and http.patch map to PUT and PATCH respectively and both take a URL and a body
• http.delete and http.head map to DELETE and HEAD respectively and both take a URL (no body)
38 makeDelete(): void {
39 this.loading = true;
40 this.http.delete('http://jsonplaceholder.typicode.com/posts/1')
41 .subscribe((res: Response) => {
42 this.data = res.json();
43 this.loading = false;
44 });
45 }

## RequestOptions
All of the http methods we’ve covered so far also take an optional last argument: RequestOptions.
The RequestOptions object encapsulates:
• method
• headers
• body
• mode
• credentials
• cache
• url
• search
47 makeHeaders(): void {
48 const headers: Headers = new Headers();
49 headers.append('X-API-TOKEN', 'ng-book');
50
51 const opts: RequestOptions = new RequestOptions();
52 opts.headers = headers;
53
54 this.http.get('http://jsonplaceholder.typicode.com/posts/1', opts)
55 .subscribe((res: Response) => {
56 this.data = res.json();
57 });
58 }

# Routing
In web development, routing means splitting the application into different areas usually based on
rules that are derived from the current URL in the browser.
## the html5 client-side routing
There’s two things you need to be aware of when using HTML5 mode routing, though
1. Not all browsers support HTML5 mode routing, so if you need to support older
browsers you might be stuck with hash-based routing for a while.
2. The server has to support HTML5 based routing.

using the history.pushState method that exposes the browser’s navigational history to JavaScript.
So now, instead of relying on the anchor hack to navigate routes, modern frameworks can rely on
pushState to perform history manipulation without reloads.
In Angular we configure routes by mapping paths to the component that will handle them.

## 3 main components of Angular routing
• Routes describes the routes our application supports
• RouterOutlet is a “placeholder” component that shows Angular where to put the content of each route
• RouterLink directive is used to link to routes

## imports
5 import {
6 RouterModule,
7 Routes
8 } from '@angular/router';

## app.modules.ts:  routes configuration
const routes: Routes = [
  // basic routes
  { path: '', redirectTo: 'home', pathMatch: 'full' },
  { path: 'home', component: HomeComponent },
  { path: 'about', component: AboutComponent },
  { path: 'contact', component: ContactComponent },
  { path: 'contactus', redirectTo: 'contact' },

  // authentication demo
  { path: 'login', component: LoginComponent },
  {
    path: 'protected',
    component: ProtectedComponent,
    canActivate: [ LoggedInGuard ]
  },

  // nested
  {
    path: 'products',
    component: ProductsComponent,
    children: childRoutes
  }
];

• path specifies the URL this route will handle
• component is what ties a given route path to a component that will handle the route
• the optional redirectTo is used to redirect a given path to an existing route
As a summary, the goal of routes is to specify which component will handle a given path.

## installing our routes
 To use the routes in our app we do two things to our NgModule:
1. Import the RouterModule
2. Install the routes using RouterModule.forRoot(routes) in the imports of our NgModule
@NgModule({
  declarations: [
    AppComponent,
    HomeComponent,
    ContactComponent,
    AboutComponent,
    LoginComponent,
    ProtectedComponent,
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    RouterModule.forRoot(routes), // <-- routes

    // added this for our child module
    ProductsModule
  ],
  providers: [
    // uncomment this for "hash-bang" routing
    // { provide: LocationStrategy, useClass: HashLocationStrategy }
    AUTH_PROVIDERS,
    LoggedInGuard
  ],
  bootstrap: [AppComponent]
})

##  RouterOutlet using <router-outlet>
We are are able to use the router-outlet directive in our template because we imported the RouterModule in our NgModule.
1 <div class="page-header">
2 <div class="container">
3 <h1>Router Sample</h1>
4 <div class="navLinks">
5 <a [routerLink]="['/home']">Home</a>
6 <a [routerLink]="['/about']">About Us</a>
7 <a [routerLink]="['/contact']">Contact Us</a>
8 |
9 <a [routerLink]="['/products']">Products</a>
10 <a [routerLink]="['/login']">Login</a>
11 <a [routerLink]="['/protected']">Protected</a>
12 </div>
13 </div>
14 </div>
15
16 <div id="content">
17 <div class="container">
18 <router-outlet></router-outlet>
19 </div>
20 </div>

## RouterLink using [routerLink]
see above routes' link code in the template.
5 <a [routerLink]="['/home']">Home</a>
6 <a [routerLink]="['/about']">About Us</a>
7 <a [routerLink]="['/contact']">Contact Us</a>
the value of routerLink is a string with an array containing a string "['home']", for example.there are more things you can provide when linking to routes.

## --base-href  <base href="/">    provide: APP_BASE_HREF, useValue: '/'
This line declares the base HTML tag. This tag is traditionally used to tell the browser where to look
for images and other resources declared using relative paths.
It turns out Angular Router also relies on this tag to determine how to construct its routing
information.
For instance, if we have a route with a path of /hello and our base element declares href="/app",
the application will use /app/# as the concrete path.

You can declare the application base path
programmatically, when configuring our NgModule by using the APP_BASE_HREF provider:

1 @NgModule({
2 declarations: [ RoutesDemoApp ],
3 imports: [
4 BrowserModule,
5 RouterModule.forRoot(routes) // <-- routes
6 ],
7 bootstrap: [ RoutesDemoApp ],
8 providers: [
9 { provide: LocationStrategy, useClass: HashLocationStrategy },
10 { provide: APP_BASE_HREF, useValue: '/' } // <--- this right here
11 ]
12 })
Putting { provide: APP_BASE_HREF, useValue: '/' } in the providers is the equivalent of using
<base href="/"> on our application HTML header.
When deploying to production we can also set the value of the base-href by using the --base-href command-line option.

## routing strategies  PathLocationStrategy HashLocationStrategy
The way the Angular application parses and creates paths from and to route definitions is called location strategy.
The default strategy is PathLocationStrategy, which is what we call HTML5 routing.
While using this strategy, routes are represented by regular paths, like /home or /contact.
Instead of using the default PathLocationStrategy we can also use the HashLocationStrategy.
The reason we’re using the hash strategy as a default is because if we were using HTML5 routing,
our URLs would end up being regular paths (that is, not using hash/anchor tags).

## route parameters
We can specify that a route takes a parameter by putting a colon : in front of the path segment like this:
/route/:param
###  using route parameters    ActivateRoute
1 import { ActivatedRoute } from '@angular/router';
Next, we inject the ActivatedRoute into the constructor of our component. 
1 const routes: Routes = [
2 { path: 'product/:id', component: ProductComponent }
3 ];
Then when we write the ProductComponent, we add the ActivatedRoute as one of the constructor
arguments:
1 export class ProductComponent {
2 id: string;
3 4
constructor(private route: ActivatedRoute) {
5 route.params.subscribe(params => { this.id = params['id']; });
6 }
7 }
Notice that route.params is an observable. We can extract the value of the param into a hard value
by using .subscribe. In this case, we assign the value of params['id'] to the id instance variable
on the component.

#### service example
1 class SpotifyService {
2 constructor(public http: Http) {
3 }
4 5
searchTrack(query: string) {
6 let params: string = [
7 `q=${query}`,
8 `type=track`
9 ].join("&");
10 let queryURL: string = `https://api.spotify.com/v1/search?${params}`;
11 return this.http.request(queryURL).map(res => res.json());
12 }
13 }

#### input label accepting inputs example
1 <h1>Search</h1>
2 3
<p>
4 <input type="text" #newquery
5 [value]="query"
6 (keydown.enter)="submit(newquery.value)">
7 <button (click)="submit(newquery.value)">Search</button>
8 </p>
Here we have the input field and we’re binding its DOM element value property the query property
of our component.
We also give this element a template variable named #newquery. We can now access the value of
this input within our template code by using newquery.value.

### RouterLink directive with route parameters for a given route
24 <h3>
25 <a [routerLink]="['/artists', t.artists[0].id]">
26 {{ t.artists[0].name }}
27 </a>
28 </h3>

### accepts the input and do the search and render  the search results:
22 export class SearchComponent implements OnInit {
23 query: string;
24 results: Object;
25
26 constructor(private spotify: SpotifyService,
27 private router: Router,
28 private route: ActivatedRoute) {
29 this.route
30 .queryParams
31 .subscribe(params => { this.query = params['query'] || ''; });
32 }
On the constructor we’re injecting the SpotifyService (that we created above), Router, and the
ActivatedRoute and making them properties of our class.
In our constructor we subscribe to the queryParams property - this lets us access query parameters,
such as the search term (params['query']).
In a URL like: http://localhost/#/search?query=cats&order=ascending, queryParams gives us
the parameters in an object. This means we could access the order with params['order'] (in this
case, ascending).

#### the search method
In our SearchComponent we will call out to the SpotifyService and render the results. There are
two cases when we want to run a search:
We want to run a search when the user:
• enters a search query and submits the form
• navigates to this page with a given URL in the query parameters (e.g. someone shared a link
or bookmarked the page)
43 search(): void {
44 console.log('this.query', this.query);
45 if (!this.query) {
46 return;
47 }
48
49 this.spotify
50 .searchTrack(this.query)
51 .subscribe((res: any) => this.renderResults(res));
52 }

54 renderResults(res: any): void {
55 this.results = null;
56 if (res && res.tracks && res.tracks.items) {
57 this.results = res.tracks.items;
58 }
59 }

### searching on page load
34 ngOnInit(): void {
35 this.search();
36 }

### submit the searching
38 submit(query: string): void {
39 this.router.navigate(['search'], { queryParams: { query: query } })
40 .then(_ => this.search() );
41 }

We’re manually telling the router to navigate to the search route, and providing a query parameter,
then performing the actual search.
Doing things this way gives us a great benefit: if we reload the browser, we’re going to see the same
search result rendered. We can say that we’re persisting the search term on the URL.

## track component  （page 251)


# ionic3 lazyload  
https://www.jianshu.com/p/2c95e0fa4cc6 

# ionic3 page hooks page lifecycle events ionic document: navController的API部分
https://www.jianshu.com/p/72b704b5c9ed
ionViewDidLoad 第一次调用 返回void
ionViewWillEnter 每次调用 返回void
ionViewDidEnter 每次调用 返回void
ionViewWillLeave 每次调用 返回void
ionViewDidLeave 每次调用 返回void
ionViewWillUnload 每次调用 返回void
ionViewCanEnter 每次调用 返回boolean
ionViewCanLeave 每次调用 返回boolean

# start a new ionic3 project
install nvm: curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
(https://github.com/creationix/nvm#install-script)
nvm install node
nvm use node
nvm list

//install n: curl -L https://git.io/n-install | bash
//install node: $ n stable
//install npm: $ curl -0 -L https://npmjs.com/install.sh | sudo sh
upgrade npm: npm install -g npm
install cnpm: npm install -g cnpm --registry=https://registry.npm.taobao.org
npm install -g ionic cordova
npm uninstall -g ionic
npm cache clean

ionic start campermate blank --v2
cd campermate
ionic serve -l

ionic g page Location
ionic g page MyDetails
ionic g page CampDetails

ionic g provider Data
ionic g provider GoogleMaps
ionic g provider Connectivity

往App Module里面添加页面与服务:

# pouchdb 
npm install pouchdb --save
我们也要给PouchDB安装“typings”这样TypeScript编译器就不会抱怨了（ 因为他不知道
PouchDB是什么） 。
> 运行如下命令按钮PouchDB的types
npm install @types/pouchdb --save --save-exact

ionic cordova plugin add cordova-plugin-geolocation --variable GEOLOCATION_USAGE_DESCRIPTION="To locate you"
npm install --save @ionic-native/geolocation

# Ionic打包  ionic resources
设计图标 :Ionic资源工具之要求一个 192x192 的图标作为基本，但是由于应用商店需求一个大一些的图

标，因此最好先设计为 1024x1024（ 甚至是 2048x2048） 。在大部分时候，将大图缩小比较

好，特殊情况下你可以特殊设计小号图标。

> 创建一个 192x192 的图标名为 icon.png 将他保存到 resource 文件夹内（ 覆盖已有的图

标）

设计闪屏: 如果你

将闪屏设计为 2208x2208 那么所有的闪屏都可以很好的生成。尽量将重要部分设计为靠近图片的中心部分，靠近边缘的是稍微不那么重要

的东西（ 即，背景图片部分） 。制作自己的 2208x2208 闪屏然后存放到resources文件夹命名为 splash.png.
现在制作好了基本图标和闪屏，可以通过如下命令生成其他的图标和闪屏：

ionic resources

设置Bundler ID和App Name:  在进行构建之前有一个重要的步骤是修改config.xml文件里的App Name和Bundle ID。
<?xml version='1.0' encoding='utf-8'?>

<widget id="io.ionic.starter" version="0.0.1"

xmlns="http://www.w3.org/ns/widgets"

xmlns:cdv="http://cordova.apache.org/ns/1.0">

<name>V2 Test</name>

<description>An Ionic Framework and Cordova project.</description>

<author email="hi@ionicframework" href="http://ionicframework.com/">Ionic Framework Te

am</author>

id也就是现在的io.ionic.starter。这个值应该匹配签名的值，应该被设置为类似：com.yourname.project。

设置Cordova偏好:
<preference name="webviewbounce" value="false" />

<preference name="UIWebViewBounce" value="false" />

<preference name="DisallowOverscroll" value="true" />

<preference name="android-minSdkVersion" value="16" />

<preference name="BackupWebStorage" value="none" />

缩减素材: tinyPNG

生成android keystore:
android Android需要你创建一个‘keystore’来对应
用进行签名。给应用签名需要一个Android SDK自带的工具叫做keytool。如果你的电脑上没有按组航

Android SDK的话，请参考这个指引：http://ionicframework.com/docs/v1/ionic-cli-faq/#android-sdk
设置好Android SDK之后，可以按照如下步骤创建一个keystore文件。

运行如下命令生成一个指定别名alias_name（ 你应该改一下） 的keystore文件

keytool -genkey -v -keystore my-release-key.keystore -alias alias_name -keyalg RSA

-keysize 2048 -validity 10000
输入这个命令的时候会提醒你输入密码 -- 输入个密码存好并记住。为了后面能在应用商店更新你的应用你需要这个keystore文件，以及别名和密码。
如果你忘记了其中一个的话，就更新不了。

生成一个Key Hash:
keytool -exportcert -alias alias_name -keystore my-release-key.keystore | openssl sha1
 -binary | openssl base64
确保用你的keystore别名替换alias_name和用你的keystore文件路径替换my-releasekey.keystore。

一旦完成的话会在终端输出你的Key Hash，然后你就可以简单复制到你的Facebook应用的Android platform setting去。

创建文件 platforms/android/release-signing.properties 文件，添加如下内容：

storeFile=snapaday-release.keystore

keyAlias=snapaday
                        nyhuiredw2q` 1OP]CAYJUTHIGKFR'DSW
                        HJM`
ionic build android --release
这个命令会给你生成.apk文件,位置
在：platforms/android/build/outputs/apk/ .

android本地环境搭建：
1、安装JAVA，OPENJDK1.8
2、安装android studio
3、ionic cordova build
4、 根据提示信息修改 java的HOME_DIRECTORY, path
: sudo vi /etc/profile.d/jdk_home.sh
  #!/bin/sh
  export JAVA_HOME=$(readlink -f /usr/bin/java | sed "s:/bin/java::")
  export PATH=$JAVA_HOME/bin:$PATH
   source /etc/profile.d/jdk_home.sh
 5. vi ~/.bash_profile
    export ANDROID_HOME="/home/vagrant/Android/Sdk/"
    export PATH="$ANDROID_HOME:$PATH"
  source ~/.pro_file
 6. graddle
    export GRADDLE_HOME="/home/vagrant/.gradle/wrapper/dists/gradle-4.4-all/9br9xq1tocpiv8o6njlyu5op1/gradle-4.4/bin"
    export PATH="$GRADDLE_HOME:$PATH"
  source ~/.pro_file
  
# https://oauth.net/2/     oauth2
# cordova-plugin-whitelist 协议白名单配置整理 
  https://blog.csdn.net/u011127019/article/details/56009248 
  http://cordova.apache.org/docs/en/latest/reference/cordova-plugin-whitelist/
  http://cordova.apache.org/docs/en/latest/reference/cordova-plugin-inappbrowser/


