# Angular Cheatsheet
Set of basic functionalities from Angular in one place

# AngularCLI
Command line inferface for Angular - set of commands that will help us during development.

**1. Setup**

| Command  | Description |
| ------------- | ------------- |
| npm install -g @angular/cli  | Install Angular CLI globally |

**2. New application**

| Command  | Description |
| ------------- | ------------- |
| ng new best-practises --dry-run  | just simulate ng new  |
| ng new best-practises --skip-install  | skip install means don't run npm install  |
| ng new best-practises --prefix best  | set prefix to best  |
| ng new --help	| check available command list  |


**3. Lint - check and make sure that our code if free of code smells/ bad formatting**

| Command  | Description |
| ------------- | ------------- |
| ng lint my-app --help  | check available command list  |
| ng lint my-app --format stylish  | format code  |
| ng lint my-app --fix   | fix code smells  |
| ng lint my-app  | show warnings  |


**4. Blueprints**

| Command  | Description |
| ------------- | ------------- |
| ng g c my-component --flat true  | don't create new folder for this component  |
| --inline-template (-t)  | will the template be in .ts file?  |
| --inline-style (-s) 	  | will the style be in .ts file?  |
| --spec		  | generate spec?  |
| --prefix  | assign own prefix  |
| ng g d directive-name	  | create directive  |
| ng g s service-name	  | create service |
| ng g cl models/customer	  | create customer class in models folder |
| ng g i models/person  | create create interface in models folder |
| ng g e models/gender  | create  create ENUM gender in models folder |
| ng g p init-caps	  | create create pipe  |

**5. Building&Serving**

| Command  | Description |
| ------------- | ------------- |
| ng build   | build app to /dist folder  |
| ng build --aot  | build app without code that we don't need (optimatization)  |
| ng build --prod	  | build for production  |
| ng serve -o   | serve with opening a browser  |
| ng serve --live-reload	  | reload when changes occur  |
| ng serve -ssl   | serving using SSL  |

**6. Add new capabilities**

| Command  | Description |
| ------------- | ------------- |
| ng add @angular/material 	  | add angular material to project  |
| ng g @angular/material:material-nav --name nav  | create material navigation component  |

# Components & Templates
Components are the most basic UI building block of an Angular app. An Angular app contains a tree of Angular components.

## Sample component ts file
```ts
import { Component } from '@angular/core';

@Component({
	// component attributes
	selector: 'app-root',
	templateUrl: './app.component.html',
	styleUrls: ['./app.component.less']
})

export class AppComponent {
	title = 'my-dogs-training';
}
```
** Component attributes**


| Attribute  | Description |
| ------------- | ------------- |
| changeDetection	  | The change-detection strategy to use for this component.  |
| viewProviders  | 	Defines the set of injectable objects that are visible to its view DOM children  |
| moduleId  | The module ID of the module that contains the component  |
| encapsulation  | 	An encapsulation policy for the template and CSS styles  |
| interpolation  | 	Overrides the default encapsulation start and end delimiters ({{ and }}  |
| entryComponents  | 	A set of components that should be compiled along with this component.  |
| preserveWhitespaces  | 	True to preserve or false to remove potentially superfluous whitespace characters from the compiled template.   |

## Component life cycles

| Life cyccle  | Description |
| ------------- | ------------- |
| ngOnInit  | Called once, after the first ngOnChanges()   |
| ngOnChanges  | Called before ngOnInit() and whenever one of input properties change.   |
| ngOnDestroy  | Called just before Angular destroys the directive/component  |
| ngDoCheck  | Called during every change detection run  |
| ngAfterContentChecked  | Called after the ngAfterContentInit() and every subsequent ngDoCheck()  |
| ngAfterViewChecked  | Called after the ngAfterViewInit() and every subsequent ngAfterContentChecked().  |
| ngAfterContentInit  | Called once after the first ngDoCheck().  |
| ngAfterViewInit  | Called once after the first ngAfterContentChecked().   |

**Template syntax**

| Syntax  | Description |
| ------------- | ------------- |
| {{user.name}}	| Interpolation - just generate user name here  |
| <img [src] = "user.imageUrl">	| property binding - bind image url for user to src attribute  |
| <button (click)="do()" ... />	| Event - assign function to click event  |
| <button *ngIf="user.showSth" ... />	| Show button when user.showSth is true  |
| *ngFor="let item of items"	| Iterate through items list |
| <div [ngClass]="{green: isTrue(), bold: itTrue()}"/> | Angular ngClass attribute  |
| <div [ngStyle]="{'color': isTrue() ? '#bbb' : '#ccc'}"/>	| Angular ngStyle attribute  |


## Content projection
Content projection is injection inner html into child component

Example:

**Parent component template**
```html
<component>
	<div>
		(some html here)
	</div>
</component>
```

**Child component template**
```html 
<ng-content></ng-content>
```
(some html here) will be injection into <ng-content></ng-content>

**Two differents htmls**
```html
<component>
	<div well-title>
		(some html here)
	</div>
	<div well-body>
		(some html here)
	</div>
</component>
```

```html
<ng-content select="title"></ng-content> 
<ng-content select="body"></ng-content>
```

# Routing
The Angular Router enables navigation from one view to the next as users perform application tasks.

**Sample routing ts file**
```ts
const appRoutes: Routes = [
  { path: 'crisis-center', component: CrisisListComponent },
  { path: 'hero/:id',      component: HeroDetailComponent },
  {
    path: 'heroes',
    component: HeroListComponent,
    data: { title: 'Heroes List' }
  },
  { path: '',
    redirectTo: '/heroes',
    pathMatch: 'full'
  },
  { path: '**', component: PageNotFoundComponent }
];
```

Then this should be added inside Angular.module imports
```ts
RouterModule.forRoot(appRoutes)
```

**Usage**
```html
  <a routerLink="/crisis-center" routerLinkActive="active">Crisis Center</a>	
```
routerLinkActive="active" will add active class to element when the link's route becomes active

```ts
//Navigate from code
this.router.navigate(['/heroes']);

// with parameters													
this.router.navigate(['/heroes', { id: heroId, foo: 'foo' }]);

// Receive parameters without Observable
let id = this.route.snapshot.paramMap.get('id');										
 ```
 
## CanActivate/CanDeactivate
Interface that a class can implement to be a guard deciding if a route can be activated. If all guards return true, navigation will continue. 

```ts

class AlwaysAuthGuard implements CanActivate {	
	canActivate() {
		return true;
	}
}
```
and assing it in routing module:

```ts
 {
    path: 'artist/:artistId',
    component: ArtistComponent,
    canActivate: [AlwaysAuthGuard], 
    children: [
      {path: '', redirectTo: 'tracks'},
      {path: 'tracks', component: ArtistTrackListComponent},
      {path: 'albums', component: ArtistAlbumListComponent},
    ]
  }
```

# Modules
Angular apps are modular and Angular has its own modularity system called NgModules. NgModules are containers for a cohesive block of code dedicated to an application domain, a workflow, or a closely related set of capabilities. 

## Sample module with comments

```ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';

@NgModule({
	declarations: [AppComponent], // components, pipes, directives
	imports: [BrowserModule, AppRoutingModule], // other modules
	providers: [], // services
	bootstrap: [AppComponent] // top component
})
export class AppModule { }
```

# Services
Components shouldn't fetch or save data directly and they certainly shouldn't knowingly present fake data. They should focus on presenting data and delegate data access to a service.

**Sample service with one function**
```ts
@Injectable()
export class MyService {
	public items: Item[];
	constructor() { }
	
	getSth() {
		// some implementation
	}
}
```
**Usage**
It should be injected before usage
```ts 
constructor(private dogListService: MyService) 
```

and add in module:

```ts
providers: [MyService]
```
