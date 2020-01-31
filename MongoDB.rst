#######
MongoDB
#######

MongoDB
*******

General
=======

**Start server**

.. code-block:: bash

   sudo systemctl start mongod

**Stop server**

.. code-block:: bash

   sudo systemctl stop mongod

**Restart server**

.. code-block:: bash

   sudo systemctl restart mongod

**Server status**

.. code-block:: bash

   sudo systemctl status mongod

You can also verify mongod is working by searching the :code:`/var/log/mongodb/mongod.log` file for the line 

:code:`[initandlisten] waiting for connections on port 27017`


**Data directories**

The db data directories are 

:code:`/val/log/mongodb`

:code:`/var/lib/mongodb`


**Debian 9**

In Debian 9, the start/stop/restart commands were:

.. code-block:: bash

   sudo service mongod [start/stop/restart]

:code:`service` resides in :code:`/usr/sbin` and :code:`systemctl` in :code:`/bin`.



Commands
========
**Start interactive**

.. code-block:: bash

  mongo

**Get help**

.. code-block:: bash

  help

**Show databases**

.. code-block:: bash

  show dbs

**Use a database**

.. code-block:: bash

   use

This creates a new or loads an existing database.

**See collections in a database**

.. code-block:: bash

  show collections

CRUD
====

**C(reate)**

.. code-block:: bash

  db.dogs.insert({..})

**R(etrieve)**

.. code-block:: bash

  db.dogs.find()

Finds all dogs.

.. code-block:: bash

  db.dogs.find({breed: "Mutt"})

Find all dogs with breed "Mutt"

**U(pdate)**

.. code-block:: bash

  db.dogs.update({name: "Lulu"}, {breed: "Labradoodle"})

Will update Lulu's breed but remove all other fields apart from _id.

.. code-block:: bash

  db.dogs.update({name: "Rusty"}, {$set: {name: "Tater", isCute: true}})

Will update Rusty's name to Tater, will add a new field "isCute" but will leave all other fields intact.

**R(emove)**

.. code-block:: bash

  db.dogs.remove({...}).limit(n)

Remove n entries that match the criteria

.. code-block:: bash

  db.dogs.drop()

Drops all the entries from the collection!!!


Mongoose
********

General
=======

**Require**

.. code-block:: javascript

  var mongoose = require("mongoose");

**Connect**

.. code-block:: javascript

  mongoose.connect("mongodb://localhost:27017/db_name", {useNewUrlParser: true});

**Create Schema**

.. code-block:: javascript

  var userSchema = new mongoose.Schema({
      name: String,
      surname: String
  });

**Create model**

.. code-block:: javascript

  var User = mongoose.model("User", userSchema);

If the Schema and Model exist in the database, the code will use them, otherwise it will create them. 

Write
=====

**Write directly in the database**

.. code-block:: javascript

  User.create({
      name: "Charlie",
      surmane: "Brown"
  });

**Create object and save**

.. code-block:: javascript

  var newUser = new User({
      name: "Charlie",
      surname: "Brown"
  });

  newUser.save(function(err, user){
      if(err){
          console.log(err);
      } else {
          console.log(user);
      }
  });

Find
====

**Find one entry**

.. code-block:: javascript

  newUser.findOne({name: "Charlie"}, function(err, foundUser){
      if(err){
          console.log(err);
      } else {
          console.log(foundUser);
      }
  });

**Find all entries**

.. code-block:: javascript

  User.find({}, function(err, users){
      if(err){
          console.log("Error!");
      } else {
          console.log(users);
      }
  });

**Find By Id**

.. code-block:: javascript

  User.findById(id, function(err, foundUser){
      if (err){
          console.log(err);
      } else {
          console.log(foundUser);
      }
  });

**Find By Id And Update**

.. code-block:: javascript

  User.findByIdAndUpdate(id, newUser, function(err, updatedUser){
      if(err){
          console.log(err);
      } else {
          console.log(updatedUser)
      }
  });

newUser should be an object that conforms to the database's model.


**Find By Id And Remove**

.. code-block:: javascript

  User.findByIdAndRemove(id, function(err){
      if(err){
          console.log(err);
      } else {
          console.log("User deleted");
      }
  });



Remove
======

**Remove all entries from the database**

.. code-block:: javascript

  User.remove({}, function(err){
      if(err){
          console.log(err);
      }
  });

mongoose.set("userFindAndModify", false);








