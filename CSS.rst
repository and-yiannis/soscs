#######
CSS
#######

Block Content
#############

The following div will dissapear for screen sizes less than :code:`sm`

.. code-block:: html

  <div class="d-none d-sm-block"> 
  ...
  </div>


Content Justification
#####################

https://getbootstrap.com/docs/4.6/utilities/flex/#justify-content

.. code-block:: html

    <div class="d-flex justify-content-start">...</div>
    <div class="d-flex justify-content-end">...</div>
    <div class="d-flex justify-content-center">...</div>
    <div class="d-flex justify-content-between">...</div>
    <div class="d-flex justify-content-around">...</div>

Size dependent CSS
##################

.. code-block:: css

    @media (max-width: 576px) {
      /* CSS that should be displayed if width is
         equal to or less than 576 goes here */
      .myclass{
          min-width: 160px;
      }
    }

Spacing
#######

https://getbootstrap.com/docs/4.6/utilities/spacing/#notation

* :code:`m` - margin
* :code:`p` - padding

Where sides is one of:

* :code:`t` - top 
* :code:`b` - bottom
* :code:`l` - left
* :code:`r` - right
* :code:`x` - left and right
* :code:`y` - top and bottom
* blank - all

Where size is one of:

* :code:`0` - 0
* :code:`1` - $spacer * .25
* :code:`2` - $spacer * .5
* :code:`3` - $spacer
* :code:`4` - $spacer * 1.5
* :code:`5` - $spacer * 3
* :code:`auto` - for classes that set the margin to auto

Validation
##########

* Dirty (When its content has been changed)
* Pristine 
* Touched (When it has lost focus)
* Untouched
* Valid
* Invalid

Recipes
#######

Placing a column at the bottom of the enclosing div
***************************************************
Item 3 will appear at the bottom of the enclosing div.

.. code-block:: html

   <div class="d-flex flex-column" style="min-height: 100px;">
     <div>Item 1</div>
     <div>Item 2</div>
     <div class="mt-auto">Item 3</div>
   </div>

