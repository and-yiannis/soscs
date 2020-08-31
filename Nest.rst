.. Roles

.. role:: bash(code)
  :language: bash

.. role:: ts(code)
  :language: typescript

.. Role usage
.. :bash:`ls -l`

#######
Nest
#######

Building Blocks of Nest js
##########################

The main building blocks of a Nest.js application are 

* Controllers

* Providers

* Modules

Controllers
***********

Controllers are responsible for handling incoming requests and returning 
responses. An example of a controller is

.. code-block:: typescript

   import { Controller, Get } from '@nestjs/common';
   
   @Controller('users')
   export class UsersController {
    @Get()
    findAll() {
      return 'This will return all the users';
    }
   }

The controller must be added to the module definition, so that it's recognised.

Providers
*********

Providers (a.k.a. services) are designed to abstract any form of complexity and
logic. An example

.. code-block:: typescript

   import { Injectable } from '@nestjs/common';
   import { User } from './interfaces/user.interface';
   
   @Injectable()
   export class UsersService {
     private readonly users: User[] = [];
   
     create(user: User) {
       this.users.push(user);   }
   
     findAll(): User[] {
       return this.users;
     }
   }


Modules
*******

Modules let you group related files. Each application has a **root module**. 
This is an example of a module incorporating the controller and the service.

.. code-block:: typescript

   import { Module } from '@nestjs/common';
   import { UsersController } from './users.controller.ts';
   import { UsersService } from './users.service.ts';
   
   @Module({
     controllers: [UsersController],
     providers: [UsersService]
   })
   
   export class UsersModule {}

And this is an example of importing the module to the root module of the application

.. code-block:: typescript

   ...
   import { UsersModule } from './users/users.module';
   
   @Module({
     imports: [UsersModule], 
     ...
   })

   export class AppModule { }

A few important concepts:

DTO
   A data transfer object defines how data will be sent over the network
Interfaces
   Interfaces are used for type-checking and defining the types of data that 
   can be passed to a controller or a service

Installation
############

**Installation**

.. code-block:: bash

   npm install -g @nestjs/cli

**Create new app**

.. code-block:: bash

   nest new project-name


**Run the app**

.. code-block:: bash

   npm run start

Scaffolding a new project with :code:`nest new`, creates the following files.


.. code-block:: typescript
   :caption: main.js

   import { NestFactory } from '@nestjs/core';
   import { AppModule } from './app.module';
   
   async function bootstrap() {
     const app = await NestFactory.create(AppModule);
     await app.listen(3000);
   }
   bootstrap();


.. code-block:: typescript
   :caption: app.module.ts

   import { Module } from '@nestjs/common';
   import { AppController } from './app.controller';
   import { AppService } from './app.service';
   
   @Module({
     imports: [], //This is where new modules will be added. 
     controllers: [AppController],
     providers: [AppService],
   })
   export class AppModule {}


.. code-block:: typescript
   :caption: app.controller.ts


   import { Controller, Get } from '@nestjs/common';
   import { AppService } from './app.service';
   
   @Controller()
   export class AppController {
     constructor(private readonly appService: AppService) {}
   
     @Get()
     getHello(): string {
       return this.appService.getHello();
     }
   }


.. code-block:: typescript
   :caption: app.service.ts

   import { Injectable } from '@nestjs/common';
   
   @Injectable()
   export class AppService {
     getHello(): string {
       return 'Hello World!';
     }
   }


Modules
#######

Modules allow grouping related files.
A new module can be generated through the :code:`cli` using

.. code-block:: typescript

   nest generate module <module_name>

This will create a new folder :code:`<module_name>`, if it does not already
exist, and the file :code:`<module_name>.module.ts` in it. 
It will also add it to the :code:`imports` section of the root
:code:`app.module.ts`.

* Nest encourages keeping each module in its own dedicated directory.
* In Nest, modules are singletons by default, and thus you can share the same instance of any provider between multiple modules effortlessly.

Module decorator properties
***************************

.. code-block:: typescript
   
   @Module({
     imports: [],
     controllers: [],
     providers: [],
     exports: [],
   })

providers
   the providers (services) that will be instantiated by the Nest injector and that may be shared at least across this module
controllers
   the set of controllers defined in this module which have to be instantiated
imports
   the list of imported modules that export the providers which are required in this module
exports
   the subset of providers (services) that are provided by this module and should be available in other modules which import this module

Dynamic modules
***************

Dynamic Modules are modules that are customisable during their initialisation. A typical example is a database module, which needs to be initialised with the database connections. Initialisation arguments can be passed to Dynamic Modules with the :code:`forRoot` method.


Controllers
###########

Controllers are responsible for handling incoming *requests* and returning *responses* to the client.  
A new controller can be generated using 

.. code-block:: typescript

   nest generate controller <controller_name>

This will create a new folder :code:`<controller_name>`, if it does not already
exist, and the file :code:`<controller_name>.controller.ts` in it. 
It will also add the controller's name to the :code:`controllers` section
of the respective module. 

Dependency Injection
********************

A service can be injected in a controller in its constructor

.. code-block:: typescript

   import { UsersService } from './users.service';

   ...

   @Controller('user')
   export class UserController {
       constructor(private usersService: UsersService) { }
   ...
   }
   
REST examples
*************

**GET**

.. code-block:: typescript

   @Get()
   getUser() {
   ...
   }

**GET with parameter**

.. code-block:: typescript

   @Get(:userID)
   getUser(@Param('userID') userID) {
   ...
   }

**POST with body**

.. code-block:: typescript

   @Post(:userID)
   getUser(@Body() userDTO: UserDTO) {
   ...
   }

**DELETE with Query**

.. code-block:: typescript

   @Delete()
   deleteUser(@Query() query) {
      deleteUser(query.userID)
   ...
   }

   // http://example.com/?userID=3

Similarly, :code:`Put` (replacing existing items) and :code:`Patch` (modifying existing items) are supported.

Return Objects
**************

Custom headers can be returned as

.. code-block:: typescript

   @Header('Content-Type', 'text/html')

Nest.js returns references to objects, which means that their contents can be modified. To avoid this, either return  a slice or create a copy with the spread operator

.. code-block:: typescript

   // Return a slice
   return this.object.slice()

   // Return a copy with the spread operator
   return {...this.object}

Errors can be returned using :code:`HttpException`

.. code-block:: typescript

   throw new HttpException('File not found!', 404);

Request objects
***************
+-----------------------------------+--------------------------------------------+
|:code:`@Request()`                 |    :code:`req`                             |
+-----------------------------------+--------------------------------------------+
|:code:`@Response(), @Res()`        |    :code:`res`                             |
+-----------------------------------+--------------------------------------------+
|:code:`@Next()`                    |    :code:`next`                            |
+-----------------------------------+--------------------------------------------+
|:code:`@Session()`                 |    :code:`req.session`                     |
+-----------------------------------+--------------------------------------------+
|:code:`@Param(key?: string)`       |    :code:`req.params/ req.params[key]`     |
+-----------------------------------+--------------------------------------------+
|:code:`@Body(key?: string)`        |    :code:`req.body / req.body[key]`        |
+-----------------------------------+--------------------------------------------+
|:code:`@Query(key?: string)`       |    :code:`req.query / req.query[key]`      |
+-----------------------------------+--------------------------------------------+
|:code:`@Headers(name?: string)`    |    :code:`req.headers / req.headers[name]` |
+-----------------------------------+--------------------------------------------+
|:code:`@Ip()`                      |    :code:`req.ip`                          |
+-----------------------------------+--------------------------------------------+



Services
########

Services are designed to abstract any form of complexity and logic. A new service
can be generated using

.. code-block:: typescript

   nest generate service <service_name>

This will create a new folder :code:`<service_name>`, if it does not already
exist, and the file :code:`<service_name>.service.ts` in it. 
It will also add the service's name to the :code:`providers` section
of the respective module. 

Scoping
*******

Services are scoped to a module. They are not available to other modules. Unlike Angular where they are available everywhere. 


Swagger support
###############

First install the packages

.. code-block:: typescript

   npm install @nestjs/swagger
   npm install swagger-ui-express


Then add in main.ts

.. code-block:: typescript


   import { NestFactory } from '@nestjs/core';
   import { AppModule } from './app.module';

   // Import the necessary modules
   import { SwaggerModule, DocumentBuilder } from '@nestjs/swagger';
   
   async function bootstrap() {
     const app = await NestFactory.create(AppModule);
   
     // Set up options
     const options = new DocumentBuilder()
       .setTitle('Title')
       .setDescription('Description')
       .setVersion('1.0')
       .addTag('tag')
       .build();
   
     // Create the document
     const document = SwaggerModule.createDocument(app, options);
     SwaggerModule.setup('apidoc', app, document);
     // swagger will be available in http://..../apidoc
   
   
     await app.listen(3000);
   }
   bootstrap();

Type ORM
########

Install 
********

.. code-block:: bash

   npm install @nestjs/typeorm typeorm mysql

Initialise
**********

.. code-block:: typescript

   import { Module } from '@nestjs/common';
   import { TypeOrmModule } from '@nestjs/typeorm';
   
   @Module({
     imports: [
       TypeOrmModule.forRoot({
         type: 'mysql',
         host: 'localhost',
         port: 3306,
         username: 'root',
         password: 'root',
         database: 'test',
         entities: [], // Fill in the entities here...
         synchronize: true,
       }),
     ],
   })
   export class AppModule {}

:code:`entities`, needs to include all the entities of the database.


Entities
********

.. code-block:: typescript

    import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm';
    
    @Entity()
    export class User {
      @PrimaryGeneratedColumn()
      id: number;
    
      @Column()
      firstName: string;
    
      @Column()
      lastName: string;
    
      @Column({ default: true })
      isActive: boolean;
    }

Entities should be included in the :code:`entities` part of the 
:code:`TypeOrmModule.forRoot` declaration.

Each module should import the entities defined in current scope using the
:code:`forFeature()` method.

.. code-block:: typescript
    :emphasize-lines: 5,8

    import { Module } from '@nestjs/common';
    import { TypeOrmModule } from '@nestjs/typeorm';
    import { UsersService } from './users.service';
    import { UsersController } from './users.controller';
    import { User } from './user.entity';
    
    @Module({
      imports: [TypeOrmModule.forFeature([User])],
      providers: [UsersService],
      controllers: [UsersController],
    })
    export class UsersModule {}

The repository can then be injected in the service...

.. code-block:: typescript
    :emphasize-lines: 2,3,4,9,10,14,18,22

    import { Injectable } from '@nestjs/common';
    import { InjectRepository } from '@nestjs/typeorm';
    import { Repository } from 'typeorm';
    import { User } from './user.entity';
    
    @Injectable()
    export class UsersService {
      constructor(
        @InjectRepository(User)
        private usersRepository: Repository<User>,
      ) {}
    
      findAll(): Promise<User[]> {
        return this.usersRepository.find();
      }
    
      findOne(id: string): Promise<User> {
        return this.usersRepository.findOne(id);
      }
    
      async remove(id: string): Promise<void> {
        await this.usersRepository.delete(id);
      }
    }

If you want to use the repository outside of the module which imports
:code:`TypeOrmModule.forFeature`, you'll need to re-export the providers
generated by it. You can do this by exporting the whole module, like this:

.. code-block:: typescript
    :emphasize-lines: 7

    import { Module } from '@nestjs/common';
    import { TypeOrmModule } from '@nestjs/typeorm';
    import { User } from './user.entity';
    
    @Module({
      imports: [TypeOrmModule.forFeature([User])],
      exports: [TypeOrmModule]
    })
    export class UsersModule {}


Auto-load entities

Manually adding entities to the entities array of the connection options can be
tedious. To address this issue, an alternative solution is provided. To
automatically load entities, set the :code:`autoLoadEntities` property
of the configuration object (passed into the :code:`forRoot()` method) to true.

Relations
*********

https://typeorm.io/#/

One to one
----------

.. code-block:: typescript
    :emphasize-lines: 16,17

    import {Entity, PrimaryGeneratedColumn, Column, OneToOne} from "typeorm";
    import {User} from "./User";
    
    @Entity()
    export class Profile {
    
        @PrimaryGeneratedColumn()
        id: number;
    
        @Column()
        gender: string;
    
        @Column()
        photo: string;

        // specify inverse side as a second parameter
        @OneToOne(type => User, user => user.profile) 
        user: User;
    
    }

.. code-block:: typescript
    :emphasize-lines: 13,14,15

    import {Entity, PrimaryGeneratedColumn, Column, OneToOne, JoinColumn} from "typeorm";
    import {Profile} from "./Profile";
    
    @Entity()
    export class User {
    
        @PrimaryGeneratedColumn()
        id: number;
    
        @Column()
        name: string;
    
        // specify inverse side as a second parameter
        @OneToOne(type => Profile, profile => profile.user)
        @JoinColumn()
        profile: Profile;

    }

.. code-block:: bash

    +-------------+--------------+----------------------------+
    |                        profile                          |
    +-------------+--------------+----------------------------+
    | id          | int(11)      | PRIMARY KEY AUTO_INCREMENT |
    | gender      | varchar(255) |                            |
    | photo       | varchar(255) |                            |
    +-------------+--------------+----------------------------+
    
    +-------------+--------------+----------------------------+
    |                          user                           |
    +-------------+--------------+----------------------------+
    | id          | int(11)      | PRIMARY KEY AUTO_INCREMENT |
    | name        | varchar(255) |                            |
    | profileId   | int(11)      | FOREIGN KEY                |
    +-------------+--------------+----------------------------+

Many to one / one to many
-------------------------

.. code-block:: typescript
    :emphasize-lines: 13,14

    import {Entity, PrimaryGeneratedColumn, Column, ManyToOne} from "typeorm";
    import {User} from "./User";
    
    @Entity()
    export class Photo {
    
        @PrimaryGeneratedColumn()
        id: number;
    
        @Column()
        url: string;
    
        @ManyToOne(type => User, user => user.photos)
        user: User;
    
    }

.. code-block:: typescript
    :emphasize-lines: 13,14

    import {Entity, PrimaryGeneratedColumn, Column, OneToMany} from "typeorm";
    import {Photo} from "./Photo";
    
    @Entity()
    export class User {
    
        @PrimaryGeneratedColumn()
        id: number;
    
        @Column()
        name: string;
    
        @OneToMany(type => Photo, photo => photo.user)
        photos: Photo[];
    
    }

.. code-block:: 

    +-------------+--------------+----------------------------+
    |                         photo                           |
    +-------------+--------------+----------------------------+
    | id          | int(11)      | PRIMARY KEY AUTO_INCREMENT |
    | url         | varchar(255) |                            |
    | userId      | int(11)      | FOREIGN KEY                |
    +-------------+--------------+----------------------------+
    
    +-------------+--------------+----------------------------+
    |                          user                           |
    +-------------+--------------+----------------------------+
    | id          | int(11)      | PRIMARY KEY AUTO_INCREMENT |
    | name        | varchar(255) |                            |
    +-------------+--------------+----------------------------+

Many to many
------------

.. code-block:: typescript
    :emphasize-lines: 13,14

    import {Entity, PrimaryGeneratedColumn, Column, ManyToMany} from "typeorm";
    import {Question} from "./Question";
    
    @Entity()
    export class Category {
    
        @PrimaryGeneratedColumn()
        id: number;
    
        @Column()
        name: string;
    
        @ManyToMany(type => Question, question => question.categories)
        questions: Question[];
    
    }

.. code-block:: typescript
    :emphasize-lines: 16,17,18

    import {Entity, PrimaryGeneratedColumn, Column, ManyToMany, JoinTable} from "typeorm";
    import {Category} from "./Category";
    
    @Entity()
    export class Question {
    
        @PrimaryGeneratedColumn()
        id: number;
    
        @Column()
        title: string;
    
        @Column()
        text: string;
    
        @ManyToMany(type => Category, category => category.questions)
        @JoinTable()
        categories: Category[];
    
    }

.. code-block:: 

    +-------------+--------------+----------------------------+
    |                        category                         |
    +-------------+--------------+----------------------------+
    | id          | int(11)      | PRIMARY KEY AUTO_INCREMENT |
    | name        | varchar(255) |                            |
    +-------------+--------------+----------------------------+
    
    +-------------+--------------+----------------------------+
    |                        question                         |
    +-------------+--------------+----------------------------+
    | id          | int(11)      | PRIMARY KEY AUTO_INCREMENT |
    | title       | varchar(255) |                            |
    | text        | varchar(255) |                            |
    +-------------+--------------+----------------------------+
    
    +-------------+--------------+----------------------------+
    |              question_categories_category               |
    +-------------+--------------+----------------------------+
    | questionId  | int(11)      | PRIMARY KEY FOREIGN KEY    |
    | categoryId  | int(11)      | PRIMARY KEY FOREIGN KEY    |
    +-------------+--------------+----------------------------+

A many to many relation with custom properties requires the manual creation
of the intermediate entity, that contains two many to one relations pointing 
to both *category* and *question*.



Nestjsx Crud
************

https://github.com/nestjsx/crud

The service

.. code-block:: typescript
    :emphasize-lines: 3,8,9,10

    import { Injectable } from "@nestjs/common";
    import { InjectRepository } from "@nestjs/typeorm";
    import { TypeOrmCrudService } from "@nestjsx/crud-typeorm";
    
    import { Company } from "./company.entity";
    
    @Injectable()
    export class CompaniesService extends TypeOrmCrudService<Company> {
      constructor(@InjectRepository(Company) repo) {
        super(repo);
      }
    }

The controller

.. code-block:: typescript
    :emphasize-lines: 2,7,8,9,10,11,13

    import { Controller } from "@nestjs/common";
    import { Crud, CrudController } from "@nestjsx/crud";
    
    import { Company } from "./company.entity";
    import { CompaniesService } from "./companies.service";
    
    @Crud({
      model: {
        type: Company,
      },
    })
    @Controller("companies")
    export class CompaniesController implements CrudController<Company> {
      constructor(public service: CompaniesService) {}
    }




The End
##############




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



