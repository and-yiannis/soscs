#######
Angular
#######


**Install**

.. code-block:: bash

  npm install @angular/cli


**Start new project**

.. code-block:: bash

  ng new my-app

ng should point to ``node_modules/\@angular/cli/bin/ng`` if Angular has not been installed globally.


**Interpolation syntax**

.. code-block:: javascript

  {{ title }}

It adds the variable's value as a string


**Two way binding**

.. code-block:: javascript

  [(ngModel)]

Binds an element to a variable both ways, so the element displays the value of the variable and when the element is updated the value of the variable is updated too.

**Repeater directive**

.. code-block:: javascript

  *ngFor

It repeats the host element for each element in a list. E.g.

.. code-block:: javascript

  <li *ngFor="let elem of elems>

Will repeat the ``<li>`` for every element of ``elems``, which is an array of elements.

**Conditionals**

.. code-block:: javascript

  *ngIf

removes or adds the host element to the DOM, depending on a condition being true or false. E.g.

.. code-block:: javascript

  <div *ngIf="x">

will show the ``<div>`` if ``x`` evaluates to ``true`` and will remove it otherwise.





