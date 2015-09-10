Séquences
=========

Désigne les :py:obj:`str`, :py:obj:`list`, :py:obj:`dict`, :py:obj:`set`,
:py:obj:`tuple`, :py:obj:`bytearray`, etc.

Voir :ref:`cette documentation <typesseq>` pour plus de détails.

Fonctions et lib utiles
-----------------------

Dans la stdlib:

    * :py:func:`enumerate`
    * :py:func:`any`, :py:func:`filter` et :py:func:`map`
    * :py:func:`len`
    * :py:func:`max` et :py:func:`min`
    * :py:func:`range` et :py:func:`xrange`
    * :py:func:`reversed` et :py:func:`sorted`
    * :py:func:`zip`

et aussi :py:mod:`itertools` qui recueille quelques fonctions parfois bien
utiles:

    * :py:func:`ifilter() <itertools.ifilter>` et
      :py:func:`imap() <itertools.imap>`
    * :py:func:`permutations() <itertools.permutations>`
    * :py:func:`combinations() <itertools.combinations>`

:py:obj:`tuple`
---------------

Le tuple est immuable.

.. warning:: ``(1)`` n'est pas un tuple, à la différence de ``(1,)``

    .. code-block:: python

        >>> a = (1)    # raté !
        >>> type(a)
        <type 'int'>

        >>> a = (1, )  # gagné !
        >>> type(a)
        <type 'tuple'>


:py:obj:`dict`
--------------

Tableau associatif, dont la clé peut être n'importe quelle valeur immuable
(str, int, tuple, etc.).

La méthode :py:meth:`dict.items` retourne la liste complète des couples
clé-valeur sous forme de tuple.

:py:meth:`dict.iteritems` fait la même chose en renvoyant un
:ref:`itérateur <iterateurs>`.
En python3 :py:meth:`dict.items` a le comportement de :py:meth:`dict.iteritems` en python2.

Les fonctions :py:meth:`dict.setdefault` et :py:meth:`dict.get` sont à
utiliser lorsqu'on veut avoir une valeur par défaut dans un tableau associatif
si la clé n'existe pas.

:py:obj:`list`
--------------

.. warning:: La suppression d'un élément d'une liste lors d'une itération va
   réorganiser la liste. On peut donc manquer des éléments.

.. code-block:: python

    >>> fruits = ['bananes', 'cerises', 'pommes', 'mangues']
    >>> for fruit in fruits:
    ...     fruits.remove(fruit)
    >>> fruits
    ['cerises', 'mangues']

:py:obj:`str`
-------------

Méthodes utiles
^^^^^^^^^^^^^^^

    * :py:meth:`str.replace` et :py:meth:`str.translate`
    * :py:meth:`str.split` et :py:meth:`str.partition`
    * :py:meth:`str.strip`, :py:meth:`str.rstrip` et :py:meth:`str.lstrip`
    * :py:meth:`str.startswith` et :py:meth:`str.endswith`

Formatage
^^^^^^^^^

.. code-block:: python

    >>> # MAAAAAAL, on crée 6 objets string différents
    >>> text = 'text ' + str(1) + ' another text ' + str(2) + ' fini'

    >>> # Bien !
    >>> text = 'text %d another text %d fini' % (1, 2)
    >>> text = 'text {0} another text {1} fini'.format(1, 2)
    >>> text = 'text {premier} another text {second} fini'.format(premier=1, second=2)

La concatenation de chaines de caractères est beaucoup plus rapide en passant
par string.join() que par concaténation directe (+). Il faut donc le préférer
pour de grands ensembles de données.

Les méthodes de formatage :py:meth:`str.upper`, :py:meth:`str.lower`,
:py:meth:`str.title` et :py:meth:`str.capitalize` permettent de gérer la case.

Les remplacements sont plus efficaces avec :py:meth:`str.translate` que par
:py:meth:`str.replace` pour les caractères.

Encoding
^^^^^^^^

Par défaut python2 est en ASCII et python3 en unicode. Par contre dans un
termial, python détecte l'encoding du tty et accèpte donc son encodage
(ex : utf-8).

.. note:: Il y a une différence entre la représentation **unicode** et
   l'encoding **utf-8**.

Python peut convertir de charset/codepage/encoding vers unicode grâce à la
commande :py:meth:`bytes.decode` et l'inverse via :py:meth:`str.encode`.

La bonne méthode est :
    #. Récupération (fichiers, args, user input, etc.),
    #. convertir vers unicode avec ``decode()``,
    #. faire les opérations en unicode,
    #. puis faire ``encode()`` au dernier moment (avant :py:func:`print` ou
       :py:meth:`file.write`)

.. warning:: En python 2

    .. code-block:: python

        >>> 'héhé'.isalpha()
        False
        >>> u'héhé'.isalpha()
        True

En python 2 on peut forcer les objets :py:class:`str` a adopter le même
comportement qu'en python 3 avec

.. code-block:: python

    from __future__ import unicode_literals

List comprehension
------------------

Aussi appelé list-inextension, c'est la création de séquences directement. Par
exemple

.. code-block:: python

    >>> fruits = ['banane', 'mangue', 'fraise', 'cerise', 'abricot', 'pomme']
    >>> fruits_i = [fruit for fruit in fruits if 'i' in fruit]
    >>> fruits_i
    ['fraise', 'cerise', 'abricot']

Ce type d'opération fonctionne avec toutes les séquences (list, tuple, dict,
etc.) et est très efficace d'un point de vue CPU.

Attention cependant à ne pas utiliser les parenthèses ``()`` à la place des
crochets. Celles-ci servent à la création des :ref:`générateurs <generateurs>`.
Il convient d'utiliser le constructeur classique :py:class:`tuple`.

unpacking
---------

L'unpacking se fait grâce à l'opérateur ``*`` (splat).

En gros ça permet d'extraire des données d'un itérable. Dans certains cas
c'est même automatique

.. code-block:: python

    >>> super_liste = [1, 2, 3]
    >>> a, b, c = super_liste
    >>> a
    1
    >>> b
    2
    >>> c
    3

En python 3 on peut même faire de l'unpacking partiel

.. code-block:: python

    >>> super_liste = [1, 2, 3, 4]
    >>> a, *b = super_liste
    >>> a
    1
    >>> b
    [2, 3, 4]
    >>> super_liste = [1, 2, 3, 4]
    >>> a, *b, c = super_liste
    >>> a
    1
    >>> b
    [2, 3]
    >>> c
    4

On peut aussi l'utiliser directement dans une boucle

.. code-block:: python

    >>> a = [[1, 'hello'],[2, 'world']]
    >>> for i, word in a:
    ...     print("%d %s" % (i, word))
    ...
    1 hello
    2 world

Mais là où l'unpacking est surtout utile c'est pour passer des arguments à
une fonction

.. code-block:: python

    >>> def add(a, b, c):
    ...     return a + b + c
    ...
    >>> add(1, 2, 3)
    6
    >>> values = [1,2,3]
    >>> add(*values)

Ça marche également avec les :py:obj:`dict` en argument de fonction, mais dans
ce cas il faut utiliser le double ``*``.

.. code-block:: python

    >>> def fonction_bizarre(arg1, arg2):
    ...     print("mon arg1 est {0}".format(arg1))
    ...     print("mon arg2 est {0}".format(arg2))
    ...
    >>> args = {'arg1': 'hello', 'arg2': 'world'}
    >>> fonction_bizarre(**args)
    mon arg1 est hello
    mon arg2 est world

Optimisation
------------

L'utilisation de boucles pour parcourir des tableaux est très coûteuse,
surtout lorsqu'il y a des imbrications. Tous les objets ne sont pas égaux face
à ce problème, les objets "rapides" sont, dans l'ordre:

    #. :py:obj:`dict`
    #. :py:obj:`tuple`
    #. :py:obj:`list`

On peut également utiliser les objets :py:obj:`array.array`, qui permettent de
faire des tableaux d'un seul type d'objet.

Numpy et Scipy font appel à des optimisations en C et permettent donc de gérer
des objets volumineux plus facilement.

L'utilisation de Cython et PyPy permet de faire gagner en vitesse d'exécution.

On peut, quand c'est possible utiliser les :ref:`générateurs <generateurs>`,
comme :py:func:`xrange` à la place de :py:func:`range`.

Les list-comprehension sont plus rapides qu'une boucle for classique.

La fonction :py:func:`map` est également rapide, mais il vaut mieux éviter
d'utiliser les :ref:`lamba-functions <tut-lambda>`, car elles sont
ré-interprétées à chaque élément.

Enfin, les fonctions et méthodes préfixées de ``c*`` sont souvent une
ré-implémentation en C du module, souvent beaucoup plus rapide.

Autres types de séquences
-------------------------

On peut également aller voir sur :py:mod:`collections` et le tuto sur
`PyMOTW <http://pymotw.com/2/collections/index.html>`_ pour avoir de nouveaux
types (:py:obj:`collections.namedtuple`, :py:obj:`collections.OrderedDict`, etc.).

.. _iterateurs:

Itérateurs
----------

Voir :ref:`typeiter` et `Iterable vs. Iterators vs Generators <http://nvie.com/posts/iterators-vs-generators/>`_

.. _generateurs:

Générateurs
-----------

générateurs simples
^^^^^^^^^^^^^^^^^^^

À la place de créer la liste et de la charger complètement en mémoire, on peut
utiliser les générateurs, qui vont ne renvoyer que l'élément nécessaire au
moment opportun.

Par exemple::

    >>> # va charger un tableau de 2000 entrées en mémoire
    >>> a = [sum(range(x)) for x in range(0, 10, 2)]
    >>> for i in a:
    ...     print(i)
    ...
    0
    1
    6
    15
    28
    >>> # On peut réutiliser la liste autant de fois qu'on veut

En remplaçant le ``[]`` par ``()`` on va transformer la liste en générateur.
Celui-ci ne contiendra pas la totalité des éléments, mais *générera* ceux-ci
à chaque itération::

    >>> # va créer un générateur
    >>> b = (sum(range(x)) for x in range(0, 10, 2))
    >>> print(b)
    <generator object <genexpr> at 0x1be03c0>
    >>> for i in b:
    ...     print(i)
    ...
    0
    1
    6
    15
    28
    >>> for i in b:  # ceci n'affiche rien
    ...     print(i)
    ...

:keyword:`yield`
^^^^^^^^^^^^^^^^

Le mot clé :keyword:`yield` est à utiliser à la place de :keyword:`return`.
La fonction est ainsi transformée en générateur et son code n'est **pas
éxécuté** au moment de l'appel.

L'éxécution du code contenu dans le générateur n'est éxécuté que lors d'une
itération. À chaque itération, le générateur va s'arréter au mot-clé
:keyword:`yield` en retourner la valeur. À l'itération suivante, le générateur
va redémarrer à l'endroit où il s'était arrété.

.. code-block:: python

    >>> def creer_generateur():
    ...     for i in range(25):
    ...         yield i*i
    ...
    >>> gene = cree_generateur()  # pas d'éxécution de code
    >>> print(gene)
    <generator object <genexpr> at 0x1be25c0>
    >>> for i in gene:
    ...     print(i)
    ...
    0
    1
    4
    9
    16

Voir :ref:`generator-types` et l'article de `Sam&Max <http://sametmax.com/comment-utiliser-yield-et-les-generateurs-en-python/>`_
