###
Vim
###

:code:`\`^` is a mark where the insert mode was last on.

:code:`\`.` is a mark to the place where the last change was made.

:code:`:b#` returns to the last edited buffer.

Pathogen 
*********

.. code-block:: bash

    mkdir ~/.vim/autoload

.. code-block:: bash

    mkdir ~/.vim/bundle

.. code-block:: bash

    curl -LSso ~/.vim/autoload/pathogen.vim https://tpo.pe/pathogen.vim

add in .vimrc 

.. code-block:: bash

    execute pathogen#infect()

Now any plugins that we want to install can be extracted to a subdirectory under :code:`~/.vim/bundle` and they will be added to :code:`'runtimepath'`

Instructions from:

https://github.com/tpope/vim-pathogen

NERDTree
********

Run

.. code-block:: bash

    git clone https://github.com/scrooloose/nerdtree.git ~/.vim/bundle/nerdtree

Then reload vim and run 

.. code-block:: bash

    :helptags ~/.vim/bundle/nerdtree/doc 

and checkout 

.. code-block:: bash

    :help NERDTree.txt

Instructions from:

https://github.com/scrooloose/nerdtree

Typescript support
******************

To enable typescript support for vim run the following


.. code-block:: bash

    git clone https://github.com/leafgarland/typescript-vim.git ~/.vim/pack/typescript/start/typescript-vim
