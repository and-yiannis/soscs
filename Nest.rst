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
| :code:`@Request()`                |    :code:`req`                             |
+-----------------------------------+--------------------------------------------+
| :code:`@Response(), @Res()`       |    :code:`res`                             |
+-----------------------------------+--------------------------------------------+
| :code:`@Next()`                   |    :code:`next`                            |
+-----------------------------------+--------------------------------------------+
| :code:`@Session()`                |    :code:`req.session`                     |
+-----------------------------------+--------------------------------------------+
| :code:`@Param(key?: string)`      |    :code:`req.params/ req.params[key]`     |
+-----------------------------------+--------------------------------------------+
| :code:`@Body(key?: string)`       |    :code:`req.body / req.body[key]`        |
+-----------------------------------+--------------------------------------------+
| :code:`@Query(key?: string)`      |    :code:`req.query / req.query[key]`      |
+-----------------------------------+--------------------------------------------+
| :code:`@Headers(name?: string)`   |    :code:`req.headers / req.headers[name]` |
+-----------------------------------+--------------------------------------------+
| :code:`@Ip()`                     |    :code:`req.ip`                          |
+-----------------------------------+--------------------------------------------+

Example
*******

An example of a basic CRUD controller is given below

.. code-block:: typescript

    import { Controller, Get, Param, Delete, Post, Body } from '@nestjs/common';
    
    import { User } from './user.entity';
    import { UserService } from './user.service';
    import { CreateUserDto } from './dto/create-user.dto';
    
    @Controller('user')
    export class UserController {
        constructor(
            private readonly userService: UserService
        ) {}
    
        @Get()
        findAll(): Promise<User[]> {
            return this.userService.findAll();
        }
    
        @Get(':id')
        findOne(@Param('id') id): Promise<User> {
            return this.userService.findOne(id);
        }
    
        @Post()
        createUser(@Body() createUserDto: CreateUserDto): Promise<User> {
            return this.userService.create(createUserDto);
        }
    
        @Delete(':id')
        remove(@Param('id') id): Promise<void> {
            return this.userService.remove(id);
        }
    
    }


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

An example of a service that implements a basic REST functionality is shown below. A similar example can be found in nestjs' github, :code:`sample/05-sql-typeorm`.

.. code-block:: typescript

    import { Injectable, HttpException, HttpStatus } from '@nestjs/common';
    import { InjectRepository } from '@nestjs/typeorm';
    import { Repository } from 'typeorm';

    import { User } from './user.entity';
    import { CreateUserDto } from './dto/create-user.dto';
    
    @Injectable()
    export class UserService {
      constructor(
        @InjectRepository(User)
        private readonly userRepository: Repository<User>,
      ){}
    
      create(createUserDto: CreateUserDto): Promise<User> {
        const user = new User();
    
        if (!createUserDto.name) { 
          throw new HttpException(
            "Body should contain a 'name' field",
            HttpStatus.BAD_REQUEST
          );
        }
        user.name = createUserDto.user;
        return this.userRepository.save(user);
      }
    
      async findAll(): Promise<User[]> {
        return this.userRepository.find();
      }
    
      findOne(id: string): Promise<User> {
        return this.userRepository.findOne(id);
      }
    
      async remove(id: string): Promise<void> {
        const user = await this.userRepository.findOne(id);
        if (!user) {
            throw new HttpException(
                "Not found",
                HttpStatus.NOT_FOUND
            )
        }

        await this.userRepository.delete(id);
      }
    }


Scoping
*******

Services are scoped to a module. They are not available to other modules. Unlike Angular where they are available everywhere. 

DTOs
####

Data Transfer Objects (DTOs) define how the data will be sent of the the network. DTOs can be stored in a :code:`dto` folder inside the relevant module.

.. code-block:: typescript
   :caption: dto/create-cat.dto.ts

    export class CreateCatDto {
      name: string;
      age: number;
      breed: string;
    }




Type ORM
########

https://typeorm.io/#/

Install 
********

.. code-block:: bash

   npm install @nestjs/typeorm typeorm mysql

Initialise
**********

.. code-block:: typescript
   :caption: app.module.ts

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
   :caption: user.entity.ts

    import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm';
    
    @Entity({name: "user"}) // This should match the entity name in the database.
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

The :code:`@Entity` and :code:`Column` decorators can take various arguments which can be found in 
https://github.com/typeorm/typeorm/blob/master/docs/decorator-reference.md

Entities should be included in the :code:`entities` part of the 
:code:`TypeOrmModule.forRoot` declaration.

Each module should import the entities defined in current scope using the
:code:`forFeature()` method.

.. code-block:: typescript
   :caption: user.module.ts
   :emphasize-lines: 5,8

   import { Module } from '@nestjs/common';
   import { TypeOrmModule } from '@nestjs/typeorm';
   import { UserService } from './user.service';
   import { UserController } from './user.controller';
   import { User } from './user.entity';
   
   @Module({
     imports: [TypeOrmModule.forFeature([User])],
     providers: [UserService],
     controllers: [UserController],
   })
   export class UserModule {}

The repository can then be injected in the service...

.. code-block:: typescript
   :caption: user.service.ts
   :emphasize-lines: 2,3,4,9,10,14,18,22

   import { Injectable } from '@nestjs/common';
   import { InjectRepository } from '@nestjs/typeorm';
   import { Repository } from 'typeorm';
   import { User } from './user.entity';
   
   @Injectable()
   export class UserService {
     constructor(
       @InjectRepository(User)
       private userRepository: Repository<User>,
     ) {}
   
     findAll(): Promise<User[]> {
       return this.userRepository.find();
     }
   
     findOne(id: string): Promise<User> {
       return this.userRepository.findOne(id);
     }
   
     async remove(id: string): Promise<void> {
       await this.userRepository.delete(id);
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
    export class UserModule {}


Auto-load entities

Manually adding entities to the entities array of the connection options can be
tedious. To address this issue, an alternative solution is provided. To
automatically load entities, set the :code:`autoLoadEntities` property
of the configuration object (passed into the :code:`forRoot()` method) to true.


Relations
*********

https://typeorm.io/#/relations

One to one
----------

This will create a one to one relation between profile and user

.. code-block:: typescript
    :emphasize-lines: 7,8

    import {Entity, OneToOne} from "typeorm";
    import {User} from "./User";
    
    @Entity({name: 'profile'})
    export class Profile {
        ...
        @OneToOne(type => User, x => x.profile) 
        user: User;
    
    }

.. code-block:: typescript
    :emphasize-lines: 7,8,9

    import {Entity, OneToOne, JoinColumn} from "typeorm";
    import {Profile} from "./Profile";
    
    @Entity({name: 'user'})
    export class User {
        ...
        @OneToOne(type => Profile, x => x.user)
        @JoinColumn()
        profile: Profile;

    }

this will add the following column to the :code:`user` table.

.. code-block:: bash

    +-------------+--------------+----------------------------+
    |                          user                           |
    +-------------+--------------+----------------------------+
    |    ...      |    ...       |                            |
    | profileId   | int(11)      | FOREIGN KEY                |
    +-------------+--------------+----------------------------+

Many to one / one to many
-------------------------

The following will create a many to one connection between photos a user. I.e. a User has many photos, but a photo belongs to one user. 

.. code-block:: typescript
    :emphasize-lines: 7,8,9

    import {Entity, ManyToOne} from "typeorm";
    import {User} from "./User";
    
    @Entity({name: "photo"})
    export class Photo {
        ...
        @ManyToOne(type => User, x => x.photos)
        @JoinColumn({name: 'User_id'})
        user: User;
    
    }

.. code-block:: typescript
    :emphasize-lines: 7,8

    import {Entity, OneToMany} from "typeorm";
    import {Photo} from "./Photo";
    
    @Entity({name: "user"})
    export class User {
        ...
        @OneToMany(type => Photo, x => x.user)
        photos: Photo[];
    
    }

This will generate the following column in the :code:`photo` table. The :code:`user` table will remain unchanged.

.. code-block:: 

    +-------------+--------------+----------------------------+
    |                         photo                           |
    +-------------+--------------+----------------------------+
    |   ...       |   ...        |                            |
    | User_id     | int(11)      | FOREIGN KEY                |
    +-------------+--------------+----------------------------+
    


Many to many
------------

The following will create a many to many relation between categories and questions. I.e. a question can belong in many categories, and a category can have many questions.

.. code-block:: typescript
    :emphasize-lines: 7,8

    import {Entity, ManyToMany} from "typeorm";
    import {Question} from "./Question";
    
    @Entity()
    export class Category {
        ...
        @ManyToMany(type => Question, question => question.categories)
        questions: Question[];
    
    }

.. code-block:: typescript
    :emphasize-lines: 7,8,9

    import {Entity, ManyToMany, JoinTable} from "typeorm";
    import {Category} from "./Category";
    
    @Entity()
    export class Question {
        ...
        @ManyToMany(type => Category, category => category.questions)
        @JoinTable({name: "quest_categ")
        categories: Category[];
    
    }

This code will create the following junction table, leaving the :code:`category` and :code:`question` tables untouched.

.. code-block:: 

    
    +-------------+--------------+----------------------------+
    |              quest_categ               |
    +-------------+--------------+----------------------------+
    | questionId  | int(11)      | PRIMARY KEY FOREIGN KEY    |
    | categoryId  | int(11)      | PRIMARY KEY FOREIGN KEY    |
    +-------------+--------------+----------------------------+



A many to many relation with custom properties requires the manual creation
of the intermediate entity (junction table), that contains two many to one
relations pointing to both *category* and *question*.


TypeORM migrations
******************


.. code-block:: bash 

    #Create migration
    npm run typeorm migration:create -- -n firstMigration

    #Show migrations
    npm run typeorm migration:show
    
    #Run migrations
    npm run typeorm migration:run
    
    #Revert last migration
    npm run typeorm migration:revert



Nestjsx Crud
############

A basic set of CRUD services can be automatically generated with nestjsx crud:

https://github.com/nestjsx/crud

First install

.. code-block:: bash

    npm install @nestjsx/crud
    npm install @nestjsx/crud-typeorm
    npm install class-transformer class-validator

The service

.. code-block:: typescript
    :emphasize-lines: 3,8,9,10

    import { Injectable } from '@nestjs/common';
    import { InjectRepository } from '@nestjs/typeorm';
    import { TypeOrmCrudService } from '@nestjsx/crud-typeorm';
    import { Repository } from 'typeorm';
    
    import { User } from './user.entity';
    
    @Injectable()
    export class UserService extends TypeOrmCrudService<User> {
      constructor(
        @InjectRepository(User)
        userRepository: Repository<User>
      ) {
        super(userRepository);
      }
    }

The controller

.. code-block:: typescript
    :emphasize-lines: 2,7,8,9,10,11,13

    import { Controller } from '@nestjs/common';
    import { Crud, CrudController } from '@nestjsx/crud';
    
    import { User } from './user.entity';
    import { UserService } from './user.service';
    
    @Crud({
      model: {
        type: User,
      },
    })
    @Controller('user')
    export class UserController implements CrudController<User> {
      constructor(public service: UserService) {}
    }

Overriding the default routes and other options can be found in
https://github.com/nestjsx/crud/wiki/Controllers




Swagger support
###############

Setup
*****

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

Controllers
***********

Parameter support in swagger can be added by modifying the controller as follows:

.. code-block:: typescript
    :emphasize-lines: 3,13

    import { Controller, Get, Param, Delete } from '@nestjs/common';
    ...
    import { ApiParam } from '@nestjs/swagger';

    @Controller('book')
    export class BookController {
        constructor(
            private readonly bookService: BookService
        ){}
        ... 
        @ApiParam({type: "string", name: 'id'})
        @Get(':id')
        findOne(@Param('id') id) {
            return this.bookService.findOne(id);
        }
        ...
        }
    }

DTOs / Entities
***************

To see the expected fields of a request body, decorate the respective DTO
and the entity with the :code:`@ApiProperty()`. E.g.

.. code-block:: typescript
    :emphasize-lines: 1,4,7
    :caption: create-book.dto.ts

    import { ApiProperty } from '@nestjs/swagger';
    
    export class CreateBookDto {
        @ApiProperty()
        title: string;
    
        @ApiProperty()
        year: number;
    }

If one of the properties is an array of other DTOs, this can be done as follows

.. code-block:: typescript

    import { ApiProperty } from '@nestjs/swagger';
    import { CreateAuthorDto } from '../where/the/dto/is';
    
    export class CreateBookDto {
        ...
        @ApiProperty({
            type: [CreateAuthorDto]
        })
        authors: CreateAuthorDto[]
    }


Authentication
**************

Authentication in swagger can be enabled via :code:`@ApiBearerAuth()`

.. code-block:: typescript
    :emphasize-lines: 3,5

    import { Controller, Get, Param, Delete } from '@nestjs/common';
    ...
    import { ApiBearerAuth } from '@nestjs/swagger';

    @ApiBearerAuth()
    @Controller('book')
    export class BookController {
        constructor(
            private readonly bookService: BookService
        ){}
        ... 
    }



Steps
#####

Follow the steps below to create a basic CRUD back end.

* Create the module

    * :code:`nest generate module <name>`
    * adds an entry in the imports section of the app.module.ts

* Create the <name>.entity.ts

* Create any DTOs in the :code:`./dto` folder.

    * Decorate with the :code:`@ApiProperty()` for swagger support.

* Create the service

    * :code:`nest generate service <name>`
    * Register the service in the :code:`providers` section of the module
    * Follow the template in the Services section.

* Create the Controller

    * :code:`nest generate controller <name>`
    * Register the service in the :code:`controllers` section of the module
    * Follow the template in the Controllers section.
    * Add swagger support with :code:`@ApiParam()`

* Add the repository in the Module's imports

    * :code:`[TypeOrmModule.forFeature([<name>])]`

* Add the repository in the top level module's entities

    * :code:`TypeOrmModule.forRoot({ ..., entities[<name>], ...})`


Callbacks
#########

Callbacks are functions that take another function :code:`f(err, arg)` as an argument. If the regular function completes ok, it calls the argument as :code:`f(false, arg)`. If an error pops up in the regular function, it calls it argument as :code:`f(error)`. An example is shown below.


.. code-block:: javascript
    :caption: Callbacks

    callbackFun = function(allOk, f) {
        if (allOk) {
            f(false, 1);
        } else {
            f(true);
        }
    }
    
    let state = true     // callback will return in ok state
    // let state = false // callback will return in error state
    
    callbackFun(state, (err, arg) => {
        if (err) {
            console.log("in error state");
        } else {
            console.log("all good! " + arg);
        }
    })



Promises
########

Some of the following material is taken from:
https://itnext.io/javascript-promises-vs-rxjs-observables-de5309583ca2

Construction phase:
*******************

A promise is an object that is constructed with a single argument, which is a function that we call the :code:`executor`.  The executor takes two further functions as arguments, which we call :code:`resolve` and :code:`reject`. The executor function ends by calling either of the :code:`resolve` or :code:`reject` functions. The arguments of the latter functions are called :code:`resolveArg` and :code:`rejectArg` respectively.

.. code-block:: javascript

     myPromise = new Promise((resolve, reject) => {
       if (successful) {
         resolve(resolveArg);
       } else {
         reject(rejectArg)
       }
     })

Usage phase
***********

A promise object is resolved (i.e. used) by invoking its :code:`then` member
function.
The :code:`then` function takes two functions as arguments.
The first function is called if the :code:`resolve` function of the promise was called.
The second function is called if the :code:`reject` function of the promise was called.
The arguments for the :code:`resolve` and :code:`reject` are passed to the respective function
arguments of the :code:`then` function.

.. code-block:: javascript

     myPromise.then(
       // This will be called if 'resolve' was called in the executor
       (argRes) => { // argRes is the same as the resolveArg in the executor
        console.log(argRes);
       },
       // This will be called if 'reject' was called in the executor
       (argRej) => { // argRej is the same as the rejectArg in the executor
        console.log(argRej);
       }
     )

The promises are useful because the allow the delayed execution of a
function (i.e. the resolve function) after some other operations have
taken place in the executor function. It also offers the flexibility to
call a second function (the reject function) when things have not gone as
expected.

Chaining promises:
******************

The result of the :code:`then` function is another promise, which is resolved with whatever argument was returned from the original promise. E.g.

.. code-block:: javascript

    // Original promise 
    let pr = new Promise(res => { res('hello'); })

    // pr1 is another promise, whose resolution argument will be 'hello world!'
    let pr1 = pr.then(resArg => { return resArg + ' world!'; } );

    // The resolution of pr1. Output will be 'hello world!'
    pr1.then(resArg => { console.log(resArg); });

This property allows chaining up promises as follows

.. code-block:: javascript

    let pr = new Promise(res => { res('hello'); })
    
    // Chained up promises. Output will still be 'hello world!'
    pr.then(resArg => { return resArg + ' world!'; } ).then(resArg => { console.log(resArg); });

Check also the following code: 

.. code-block:: javascript

    let pr = new Promise(res => { res('hello'); })

    let pr1 = pr.then(resArg => {
        return new Promise(                                         // The pending Promise
            (res, rej) => {                                         //
                res(resArg);   // Result will be 'Succeeded: hello' //
                //rej(resArg); // Result will be 'Failed: hello'    //
            }                                                       //
        )                                                           //
    })
    
    pr1.then(
        (resArg) => { console.log('Succeeded: ' + resArg); },
        (rejArg) => { console.log('Failed: ' + rejArg); }
    );

The resolution of :code:`pr` returns a pending promise. In this case, the resolution of the promise returned by :code:`pr.then`, i.e. the :code:`pr1`, will be resolved or rejected depending on the resolution of the pending promise.  

More on this point in https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then

Another example that will lead us to the :code:`async/await syntax`

.. code-block:: javascript

    // asyncTask(i) is a promise resolved with value i+1
    function asyncTask(i) {
        return new Promise(resolve => resolve(i + 1));
    }
    
    function runAsyncTasks() {
        return asyncTask(0) // promise resolved with value = 1
            .then(res1 => { return asyncTask(res1); })  // promise resolved with value = 2
            .then(res2 => { return asyncTask(res2); })  // promise resolved with value = 3
    }

    runAsyncTasks().then(result => console.log(result));

Looking in :code:`runAsyncTasks`,
:code:`asyncTask(0)` is a promise resolved with value 1. 
:code:`.then(res1 => { return asyncTask(res1); })` is a promise resolved with value = 2
and :code:`.then(res2 => { return asyncTask(res2); })` is a promise resolved with value = 3. 
Therefore, the :code:`runAsyncTasks` function, returns a promise that is resolved with value = 3. 

When we resolve it with 
:code:`runAsyncTasks().then(result => console.log(result));` the result we get is 3.

Async/Await
***********

Using the :code:`Async/Await`, the above example can be written as 

.. code-block:: javascript

    // asyncTask(n) : Promise that's resolved with argument n + 1.
    function asyncTask(i) {
        return new Promise(resolve => resolve(i + 1));
    }
    
    async function runAsyncTasks() {
        const res1 = await asyncTask(0);
        const res2 = await asyncTask(res1);
        return await asyncTask(res2);
    }
    
    runAsyncTasks().then(result => console.log(result));

The :code:`await` keyword pauses the execution of the asynchronous function and waits for the result of the promise's resolution. The :code:`await` keyword can only be used within a function that is declared as :code:`async`. The async function returns a promise that is resolved with whatever value the function returns. 

If a promise is rejected, the :code:`await` keyword throws an error, with the rejection's argument. This has to be handled within a :code:`try/catch` loop.


Examples 
*********

An example of a function returning a promise

.. code-block:: javascript

    function promiseReturningFunction(argIn) {
      return new Promise((resolve, reject) => {
        if (argIn) {
          resolve("It's successful!");
        } else {
          reject("Try again");
        }
      })
    }
    
    promiseReturningFunction(1).then(
      (successMessage) => {
        console.log("Yay! " + successMessage)
      },
      (failMessage) => {
        console.log("Nope... " + failMessage)
      }
    );

We can also create sleep functions...

.. code-block:: javascript

    function sleep(ms) {
      return new Promise(
        (resolve, reject) => {
          setTimeout(() => {resolve('hello')}, ms)
        }
      )
    }
    
    sleep(2000).then(
      (x) => {
        console.log("result is " + x);
      }
    );









Observables 
############

Definitions
***********

* **Observable**

.. code-block:: javascript

    import { Observable } from 'rxjs';

    observable = new Observable(subscriberFunction)

* **subscriberFunction**

.. code-block:: javascript

    subscriberFunction = (subscriber) => {
        subscriber.next(..),
        subscriber.error(..)
        subscriber.complete()
    }


* **Subscription**

.. code-block:: javascript

    subscription = observable.subscribe(observer);

* **observer**

.. code-block:: javascript

    observer = {
        next: (x) => {...},
        error: (x) => {...},
        complete: () => {...},
    }



The following material is taken from 
https://itnext.io/javascript-promises-vs-rxjs-observables-de5309583ca2

Simple example 

.. code-block:: javascript

    const  { Observable } = require('rxjs');
    
    let ob = new Observable(subscriberFunc);
    
    function subscriberFunc(observer) {
        observer.next(4);
    }
    
    ob.subscribe(x => console.log(x));

More complete example

.. code-block:: javascript

    // Creation
    const observable = new Observable(subscriberFunc);

    function subscriberFunc(observer) {
        const value = Math.random();
        if (value <= 1/3.0)
            observer.next(value);
        else if (value <= 2/3.0)
            observer.error("Value <= 2/3 (error)");
        else
            throw "Value > 2/3 (throw)"
        observer.complete();
    }

    // Usage
    observable.subscribe(nextFunc, errorFunc, completeFunc);
    function nextFunc(value) {
        console.log("Got value: " + value);
    }
    function errorFunc(error) {
        console.log("Caught error: " + error);
    }
    function completeFunc() {
        console.log("Completed");
    }

    // Or more compactly
    observable.subscribe({
        next(value) { console.log("Got value: " + value) },
        error(err) { console.log("Caught error: " + err) },
        complete() { console.log("Completed"); }
    });

The same with promises for comparison

.. code-block:: javascript

    // Creation
    const promise = new Promise((resolve, reject) => {
        const value = Math.random();
        if (value <= 1/3.0)
            resolve(value);
        else if (value <= 2/3.0)
            reject("Value <= 2/3 (reject)");
        else
            throw "Value > 2/3 (throw)"
    });

    // Usage
    promise
        .then(value => console.log("Got value: " + value))
        .catch(error => console.log("Caught error: " + error));


The construction of a promise takes as an argument the :code:`executor` function, that has as arguments two further functions, the :code:`resolve` and :code:`reject`. The :code:`executor` eventually calls one of the :code:`resolve` and :code:`reject` functions with arguments :code:`resolveArg` and :code:`rejectArg`.

A promise's resolution occurs with the :code:`then` function, that takes as arguments two functions, which will be called depending on whether the :code:`executor` called the :code:`resolve` or the :code:`reject` function, with the respective arguments.

The construction of observables, takes a function with a single argument, the :code:`observer`. The observer is an object with 3 methods, the :code:`next`, :code:`error` and :code:`complete`.

An observable's 'resolution' happens with the :code:`subscribe` function, which takes 3 functions as arguments, which will be executed depending on which of the :code:`next`, :code:`error` and :code:`complete` were invoked in the :code:`observer`.


Differences with promises
*************************

Single vs Multiple values
-------------------------

.. code-block:: javascript

    const promise = new Promise(resolve => {
        resolve(1);
        resolve(2);
        resolve(3);
    });

    promise.then(result => console.log(result));

    // Output 
    // 1

.. code-block:: javascript

    const observable = new Observable(observer => {
        observer.next(1);
        observer.next(2);
        observer.next(3);
    });

    observable.subscribe(result => console.log(result));

    // Output 
    // 1
    // 2
    // 3


Multicast vs Unicast
--------------------

Promises are Unicast, Observables are mulitcast

.. code-block:: javascript

    const promise = new Promise(resolve => {
        console.log("Executing...");
        resolve(Math.random());
    });
    promise.then(result => console.log(result));
    promise.then(result => console.log(result));

    //  Output
    //  Executing...
    //  0.1951561731912439
    //  0.1951561731912439

.. code-block:: javascript

    const observable = new Observable(observer => {
        console.log("Executing...");
        observer.next(Math.random());
    });
    observable.subscribe(result => console.log(result));
    observable.subscribe(result => console.log(result));

    //  Output
    //  Executing...
    //  0.5884515904517829
    //  Executing...
    //  0.7974144930327094

Eager vs. Lazy
--------------

* Promises are eager. The executor function is called as soon as the promise is created. 
* Observables are lazy. The subscriber function is only called when a client subscribes to the observable.

.. code-block:: javascript

    const promise = new Promise(resolve => {
        console.log("- Executing");
        resolve();
    });

    console.log("- Subscribing");
    promise.then(() => console.log("- Handling result"));

    //  Output
    //  - Executing
    //  - Subscribing
    //  - Handling result

.. code-block:: javascript

    const observable = new Observable(observer => {
        console.log("- Executing");
        observer.next();
    });

    console.log("- Subscribing");
    observable.subscribe(() => console.log("- Handling result"));

    //  Output
    // - Subscribing
    // - Executing
    // - Handling result

Asynchronous vs Synchronous Handlers
------------------------------------

* The handler functions of promises are executed asynchronously. That is, they are executed after all the code in the main program or the current function has been executed. 
* The handler functions of observables are executed synchronously. That is, they are executed within the flow of the current function or the main program. 

.. code-block:: javascript

    console.log("- Creating promise");
    const promise = new Promise(resolve => {
        console.log("- Promise running");
        resolve(1);
    });
    console.log("- Registering handler");
    promise.then(result => console.log("- Handling result: " + result));
    console.log("- Exiting main");
    
    //  Output
    //  - Creating promise
    //  - Promise running
    //  - Registering handler
    //  - Exiting main
    //  - Handling result: 1


.. code-block:: javascript

    console.log("- Creating observable");
    const observable = new Observable(observer => {
        console.log("- Observable running");
        observer.next(1);
    });
    console.log("- Registering handler");
    observable.subscribe(v => console.log("- Handling result: " + v));
    console.log("- Exiting main");
    
    //  Output
    //  - Creating observable
    //  - Registering handler
    //  - Observable running
    //  - Handling result: 1
    //  - Exiting main

