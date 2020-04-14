.. Roles

.. role:: bash(code)
  :language: bash

.. role:: ts(code)
  :language: typescript

.. Role usage
.. :bash:`ls -l`

#######
Angular
#######

Prerequisites:
##############

First, install node.js and npm. Then install angular using 

.. code-block:: bash

  npm install -g @angular/cli

It's best to install angular command line interface globally, so that ng works from everywhere. 

Any additional packages can be installed using :bash:`npm install`.


The Application Shell
#####################

**Create a new workspace**

.. code-block:: bash

  ng new angular-tour-of-heroes

**Install the necessary packages**

.. code-block:: bash

  npm install

**Serve the application**

.. code-block:: bash

  ng serve

**Global style sheet**

The file `src/styles.css` is the global style sheet that is applied to the entire application. 

**Interpolation binding**

:ts:`{{var_name}}`

The interpolation binding presents the component's property `var_name` to the HTML for displaying.


The Hero Editor
###############

**Generate a component**

`ng generate component heroes`

The CLI creates a new folder with the comonent `heroes`. Additionally, it adds an `import` statement in the `app.module.ts`, importing the new component and adds a line declaring the component in the `declarations` section of  the `NgModule` Decoration of the `AppModule` class in the same file. 

If `ng generate component` is called in a subdirectory of `src/app`, the new component will be created inside that subdirectory. 

**Two-way binding**

:ts:`[(ngModel)]="var_name"` is Angular's two-way data binding syntax. It binds the property `var_name` to a textbox, so that data can flow in both directions, from the property to the textbox and vice versa. 

In order to work, the module needs to import 

:ts:`import { FormsModule } from '@angular/forms';`


Every new model has to be imported in the `NgModule` decorator of its main class, in the imports section. 


**Declarations**

Every component and every module has to be declared in exactly one `NgModule`. 

Displaying a list
#################

**Displaying a list**

A list can be displayed using `*ngFor`. E.g.

`<li *ngFor="let hero of heroes">` will create a list for each `hero` of `heroes`.


**Event handler**

The click events can be handled with `(click)`, e.g.

`<li *ngFor="let hero of heroes" (click)="onSelect(hero)">`

The function `onselect` with the argument `hero` will be called everytime an item of the list is clicked. 


**Selective display**

Using `*ngIf` we can chose which parts of the html to display. E.g.

`<div *ngIf="selectedHero">`

will show the `div` only if the `selectedHero` property evaluates to `True`, e.g. if it is defined. 

**Selectively apply classes**

`<li *ngFor="let hero of heroes" [class.selected]="hero === selectedHero" (click)="onSelect(hero)">`

will apply the `class` `selected` to the `li` component only if `hero === selectedHero` evaluates to `True`.


Master/Detail Components
########################

**The input property**

Suppose that a compoenent needs to accept an input. Let's say that this input will be `hero`, which will be of type `Hero`. First import the `Input` symbol from `@angular/core` adding it in the relevant import statement as 

`import { Component, OnInit, Input } from '@angular/core';`

and then declare the input in the class as 

`@Input() hero: Hero;`

The component `app-hero-detail` can then be called as

`<app-hero-detail [hero]="selectedHero"></app-hero-detail>`

where the property 'selectedHero' of the calling component is passed as the input `hero` to the `app-hero-detail` component.

**One way binding**

`<app-hero-detail [hero]="selectedHero"></app-hero-detail>`

is an example of one way binding, where the value of `selectedHero` is passed to `hero`, but not vice versa. 


Services
########


**Generate Service**

`ng generate service hero`

will create a new service called `hero`.

**of**
The `of` symbol of RxJS creates an Observable from a regular property. 


Routing
#######

**Adding the routing module**

.. code-block:: bash

  ng generate module app-routing --flat --module=app

* `--flat` puts the file in `src/app` instead of its own folder
* `--module=app` tells the CLI to register it in the `imports` array of the `AppModule`.


Adding the following to the main `.html` page redirects to the router:

.. code-block:: html

  <router-outlet></router-outlet>

The router has to be added in the main module as: 

.. code-block:: typescript

  import { NgModule } from '@angular/core';
  import { RouterModule, Routes } from '@angular/router';
  import { HeroesComponent } from './heroes/heroes.component';
  
  const routes: Routes = [
    { path: 'heroes', component: HeroesComponent }
  ];
  
  
  @NgModule({
    imports: [RouterModule.forRoot(routes)],
    exports: [RouterModule]
  })
  export class AppRoutingModule { }

And finally, import the routing module in the main module's imports. 

To navigate to a route use the `nav` tag as follows:

.. code-block:: html

  <nav>
    <a routerLink="/heroes">Heroes</a>
  </nav>

**Template literals**
Backticks in javascript define a `template literal`.

Template literals are string literals allowing embedded expressions. You can use multi-line strings and string interpolation features with them. 


HTTP
####

**InMemoryDbService**

It can be used to create fake backends, by returning data to the angular application, as if they were coming from an http service. 

Declare it in `app.module.ts` as

.. code-block:: typescript

  import { HttpClientInMemoryWebApiModule } from 'angular-in-memory-web-api';

and in the imports of the same file as

.. code-block:: typescript

    HttpClientInMemoryWebApiModule.forRoot(
      InMemoryDataService, { dataEncapsulation: false }
    )
  
The `InMemoryDataService` in the above is the service that provides the fake backend. Within there, import 

.. code-block:: typescript

  import { InMemoryDbService } from 'angular-in-memory-web-api';

and create a class that implements `InMemoryDbService`.




Dictionary
##########

**Property**
A property is a variable inside a component class.

**Method**
A method is a function inside a component class.


NPM stuff
#########

**List all the globally installed packages**
`npm list -g --depth=0`

**Uninstall a globally installed package. (needs root)**
`npm -g uninstall <package_name>`

**--save flag**
`npm install --save` is no longer necessary after npm 5.0.0. It is used by default. Its functionality was to add the package to the package.json file, which it now does anyway. 



