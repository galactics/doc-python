:tocdepth: 3

####################
Documentation Python
####################

:Author: Jules DAVID
:Generated: |today|
:Version: |release|


Cette documentation couvre tous les petits trucs et astuces vus ici et là.
L'initialisation de cette page est tirée des notes personnelle prises durant
le cours **Python Avancé** reçu en décembre 2014 et donné par
`Jean-Philippe Camguilhem <https://github.com/jpcw>`_ de
`Makina-Corpus <http://makina-corpus.com/>`_.

:abbr:`PEP (Python Enhancement Proposals)` 8
============================================

La :pep:`8` définit tout un ensemble de rêgles non-contraignantes de codages,
notamment les conventions de nommages et est un guide de mise en forme.

Les commandes ``pep8`` et ``flake8`` permettent de vérifier si la mise en
forme du code source est conforme à cette PEP, et sont installables via
:ref:`my pip`.

On peut également installe `pep8-naming <https://pypi.python.org/pypi/pep8-naming>`_
qui va permettre de mettre le code en conformité vis à vis des conventions
de nommage.

Une méthode ou variable préfixée d'un underscore ``_`` n'a, par convention,
pas vocation à être utilisée à l'extérieur de la classe. Cependant, comme il
n'y a pas de notion de visibilité d'attributs et de méthodes, on n'empèche
personne de le faire.

De même, les méthodes encadrées par des double-underscore (par exemple
``__init__``) sont des méthodes spéciales.

Selon la :pep:`8`:

===================== ========================== ============================
Type                  Règles                     Exemples
===================== ========================== ============================
Modules               minuscule                  ``scoop``
Classes               Majuscule À Chaque Mot     ``LectureFichier``
Exceptions            \*Error                    ``LectureFichierError``
Fonctions et Méthodes minuscules\_et\_underscore ``appel_fonction()``
Constantes            MAJUSCULES                 ``FICHIER_DEFAUT``
Variables d'instances minuscules\_et\_underscore ``fichier_alpha = Classe()``
===================== ========================== ============================

sys.path
========

La liste :py:obj:`sys.path` a pour rôle de définir, pour python, les
emplacements où il doit chercher les librairies à charger.

Bien que déconseillé, il est possible de modifier :py:obj:`sys.path` pour
permettre de charger dynamiquement des librairies.

Par défaut, celle-ci prend, dans l'ordre, les valeurs suivantes:

    1. le dossier contenant le script lancé
    2. les emplacements de la variable d'environnement :envvar:`PYTHONPATH`
    3. l'emplacement de la stdlib
    4. le site-packages

Mémoire
=======

En python les variables n'ont pas le même fonctionnement qu'en C. Toutes sont
des pointeurs vers des emplacements mémoires.

Le mot clé :keyword:`is` permet une comparaison des emplacements mémoires. Il
ne faut pas le confondre avec ``==`` qui lui compare les valeurs.

Le mot-clé :keyword:`is` fait en fait appel à la fonction :py:func:`id`, qui
retourne l'emplacement mémoire pointé par la variable::

    >>> a = 25614
    >>> b = 25614
    >>> a == b
    True
    >>> a is b
    False
    >>> id(a)
    32271488
    >>> id(b)
    32271296
    >>> b = a
    >>> a is b
    True

.. warning:: Les nombres de 0 à 255 et les caractères ASCII sont mises en
    mémoire par l'intérpréteur python avant le démarage de toute application.
    Ainsi, deux variables ayant la même valeur, contenue dans ces domaines,
    pointerons vers le même emplacement mémoire.

    De même ``None`` n'a qu'un emplacement mémoire.

.. code-block:: python

    >>> a = 192
    >>> b = 192
    >>> a is b
    True

Conditions
==========

En python tout est vrai, sauf ``0``, ``False``, ``None`` et tous les conteneurs
vides (``""``, ``()``, ``[]``, ``{}``, etc.).

On peut obtenir le même comportement sur un objet en utilisant les méthodes
:py:meth:`__nonzero__() <object.__nonzero__>` ou
:py:meth:`__len__() <object.__len__>` (si
:py:meth:`__nonzero__() <object.__nonzero__>` n'est pas défini).

En python3 :py:meth:`__nonzero__` est remplacé par
:py:meth:`__bool__() <object.__bool__>`

Muable/Immuable
===============

Les objets pythons sont soit muable, soit immuable (*mutable*/*immutable* en
anglais).

Un objet **immuable** n'accèpte pas de modification *in-place*, mais créera un
nouvel emplacement mémoire si on tente de le modifier. C'est le cas des types
*simples* comme les :py:obj:`tuple`, :py:obj:`str`, :py:obj:`int`,
:py:obj:`float`, etc.::

    >>> a = 658942
    >>> id(a)
    32271488
    >>> a += 614
    >>> id(a)
    33800192

Un objet **muable** garde son emplacement mémoire lorsqu'il est modifié. C'est
le cas notemment des séquences (:py:obj:`list`, :py:obj:`dict`, :py:obj:`set`,
etc., sauf :py:obj:`tuple` et :py:obj:`frozenset`)::

    >>> a = [1, 2, 3, 4]
    >>> id(a)
    38120480
    >>> a.append(5)
    >>> id(a)
    38120480

C'est aussi le cas des objets créés par le développeur.

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
commande :py:meth:`str.decode` et l'inverse via :py:meth:`str.encode`.

La bonne méthode est :
    #. Récupération (fichiers, args, user input, etc.),
    #. convertir vers unicode avec ``decode()``,
    #. faire les opérations en unicode,
    #. puis faire ``encode()`` au dernier moment (avant :py:func:`print` ou
       :py:meth:`file.write`)

.. warning:: DANGER !!

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

Voir :ref:`typeiter`.

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

Fichiers
========

Path
----

Il ne faut jamais concatener soi-même les path, car :py:mod:`os.path` c'est la
vie !

Dans la stdlib de python 3.4 (et PyPy) :py:mod:`path` est super cool et permet
de faire un objet Path, sur lequel on peut faire un join(), rename(), move(),
chown(), etc.

Lecture/Écriture
----------------

Pour la lecture de fichiers, préférer :py:meth:`str.splitlines` à
:py:meth:`file.readlines`.

.. code-block:: python

    with open('text.txt') as f:
        for lines in f.read().splitlines():
            # Action !

Pour la lecture de fichier avec des encodages autres que ASCII utiliser
:py:func:`codecs.open` pour directement spécifier l'encodage du fichier à lire
et éviter d'avoir à faire de decode.

.. note::

    En python 3,  la fonction open se comporte comme :py:func:`codecs.open`
    avec l'encoding 'utf-8' par défaut.


Fichiers temporaires
--------------------

Pour la création de fichiers temporaires :py:mod:`tempfile`. Supprime le
fichier dès l'appel de ``file.close()``.

Algorithmique
=============

Scope
-----

Une variable est accessible depuis n'importe quel sous-scope en lecture, mais
pas en écriture.
Pour pouvoir la modifier dans un sous-scope, il faut la décraler comme
:keyword:`global`, mais c'est :ref:`mal <mal>` !

.. code-block:: python

    variable = 40

    def modifier(value):
        variable += value
        # Renvoie une UnboundLocalError
        return variable

    def modifier(value):
        # Fonctionne
        return variable + value

    def modifier(value):
        global variable
        variable += value
        # Fonctionne mais à éviter
        # parce que global CAYMAL
        return variable

En python 3 on peut utiliser :keyword:`nonlocal` qui permet d'accéder au scope
directement au-dessus.

Fonctions
---------

La valeur par défaut d'un argument d'une fonction n'est évalué qu'une fois
lors de la déclaration. Ainsi si elle fait référence à un objet qui n'existe
pas encore, il y aura erreur.

Décorateurs
-----------

On peut créer ses propres décorateurs, de manière à ajouter une
fonctionnalitée particulière. Par exemple, le décorateur suivant permet de
mettre en cache les sorties d'une fonction.

.. code-block:: python

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    from functools import wraps

    def memorize(func):
        memo = {}
        @wraps(func)
        def memorized_func(x):
            if x not in memo:
                memo[x] = func(x)
            return memo[x]

        return memorized_func

    calls = 0

    @memorize
    def fib(n):
        global calls
        calls += 1

        if n == 0:
            return 0
        elif n == 1:
            return 1
        else:
            return fib(n-1) + fib(n-2)

    print "fib :", fib(40)
    print "calls :", calls

Le décorateur :py:func:`wraps <functools.wraps>` permet de faire passer le
:py:attr:`__doc__ <func.__doc__>`, :py:attr:`__module__ <class.__module__>` et
le :py:attr:`__name__ <class.__name__>` de la fonction décorée (``fib``) à la
fonction décoratrice (``_memorize``).

Des version sympa de décorateurs sont disponibles sur ce
`wiki <https://wiki.python.org/moin/PythonDecoratorLibrary>`_:

    * deprecated
    * timing
    * retry

Boucles
-------

En plus de la syntaxe classique ``for x in ...`` peut utiliser la méthode
:keyword:`for-else <for>`. Le code contenu dans ``else`` ne sera exécuté que
dans le cas où ``for`` n'est pas interrompu ou breaké.

Le même principe est applicable à :keyword:`while-else <while>`.

Exceptions
----------

.. code-block:: python

    >>> try:
    ...     x = 5/0
    ... except:
    ...     print("Hello, il y a une erreur")
    ...     raise
    ... else:
    ...     print("Je passe ici si aucune exception n'est levée")
    ... finally:
    ...     print("Je passe ici quoiqu'il arrive")
    ...
    Hello, il y a une erreur
    Je passe ici quoiqu'il arrive
    Traceback (most recent call last):
      File "<input>", line 2, in <module>
    ZeroDivisionError: integer division or modulo by zero

Context Manager
---------------

:py:func:`contextlib.contextmanager`. Une utilisation régulière est

.. code-block:: python

    with open('file.txt') as f:
        # on fait des trucs ici

qui s'occupe de refermer le fichier automatiquement en fin d'utilisation.
C'est un mix de décorateur et générateur.
C'est très intéressant dans le cas de socket, connexions à des BDD, ouvertures
de fichierts, etc.
Voir l'article de `Sam\&Max <http://sametmax.com/les-context-managers-et-le-mot-cle-with-en-python/>`__.

Annotations
-----------

Depuis python 3.0 il est possible d'ajouter des annotations à une fonction ou
méthode. Ces annotations n'ont aucun effet sur le code et son exécution.

.. code-block:: python

    def fonction(x: str, y: object) -> int:
        # faire des trucs avec
        return

Ce n'est qu'à partir de python 3.5 qu'elles sont utilisées pour définir le
type de variable.

Ce typage statique de certaines portions permet, en plus de donner une
indication directe des types attendus, de faire des optimisation mémoire par
l'interpréteur. Quelques infos `ici <http://sametmax.com/point-rapide-sur-les-types-hints/>`_.

Ce n'est pas incompatible avec des valeurs par défaut. Ça alourdit un peu la
syntaxe.

.. code-block:: python

    from typing import Iterable

    def fonction(x="hello": str, y=(1, 2): Iterable) -> int:
        # des trucs...
        return 4


POO
===

MRO
---

Quoi qu'il arrive, en python2 il faut hériter de :py:obj:`object`. On
bénéficie alors du :abbr:`MRO (Method Resolution Order)`, qui permet de se
débrouiller avec l'héritage multiple. Cf. le `tuto de Makina Corpus`_.

En python3 tous les objets héritent de :py:obj:`object` par défaut.

.. _tuto de Makina Corpus: http://makina-corpus.com/blog/metier/2014/python-tutorial-understanding-python-mro-class-search-path

Setters/Getters
---------------

via des méthodes génériques
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Les setters et getters sont implicites en python, on peut cependant les créer
pour permettre une validation des entrées/sorties.

Les méthodes :py:meth:`object.__getattr__`, :py:meth:`object.__setattr__` et
:py:obj:`object.__delattr__` sont là pour intéragir avec des attributs
existants ou non.

Pour les objets héritants de :py:obj:`object`, on a également accès à la
méthode :py:meth:`object.__getattribute__`.

via @property
^^^^^^^^^^^^^

On peut également passer par les décorateurs :py:class:`property`.

Transforme une méthode en attribut (read-only)::

    >>> class Parrot(object):
    ...     def __init__(self):
    ...         self._voltage = 100000
    ...
    ...     @property
    ...     def voltage(self):
    ...         """Get the current voltage."""
    ...         return self._voltage
    >>> parrot = Parrot()
    >>> parrot.voltage
    100000
    >>> parrot.voltage = 50
    Traceback (most recent call last):
      File "<input>", line 1, in <module>
    AttributeError: can't set attribute
    >>> parrot._voltage = 40
    >>> parrot.voltage
    40

Si on veut mettre en place des setter et deleter, la classe
``Parrot`` devient::

    class Parrot(object):

        def __init__(self):
            self._voltage = 10000

        @property
        def voltage(self):
            return self._voltage

        @voltage.setter
        def voltage(self, value):
            self._voltage = value

        @voltage.deleter
        def voltage(self):
            raise Exception("Impossible de supprimer cet élément")

Attributs spéciaux
------------------

+-------------------+---------------------------------------------------------------------------------------------------+
| Attribut          | Description                                                                                       |
+===================+===================================================================================================+
| ``__call__``      | Rend l'objet appellable                                                                           |
+-------------------+---------------------------------------------------------------------------------------------------+
| ``__dict__``      | Dictionnaire contenant tous les constantes, attributs et méthodes de l'objet/la classe            |
+-------------------+---------------------------------------------------------------------------------------------------+
| ``__slots__``     | Pour la linéarisation d'objets, on sélectionne les attributs qui seront conservés en mémoire      |
|                   | (à la manière de __all__ pour les modules)                                                        |
+-------------------+---------------------------------------------------------------------------------------------------+
| ``__[a-Z0-9]+_?`` | Les attributs préfixés de 2 « _ » et d'un « _ » au plus en suffixe sont des attributs spéciaux.   |
|                   | Ils n'est pas possible de les overrider dans les classes filles.                                  |
+-------------------+---------------------------------------------------------------------------------------------------+

Métaclasses
-----------

Fabriquer des classes à la volée, équivalent des :keyword:`lambda` mais pour
les classes.

Le constructeur d'une classe se fait en deux étapes.

    #. Le __new__ s'occupe de créer la classe
    #. le __init__ s'occupe de créer de l'instance.

En définissant le :py:meth:`__new__() <object.__new__>` on peut donc créer une
classe en lui ajoutant des attributs et méthodes.

.. note:: Pour créer une métaclasse, il faut la faire hériter de :py:obj:`type`.

    .. code-block:: python

        class MyClass(type):
            def __new__(cls, name):
                # ...

On peut également créer des métaclasse grâce à l'outils :py:mod:`abc`.

Singleton
---------

Cet objet ne peut être instancié qu'une seule fois.
C'est dans la méthode :py:meth:`__new__() <object.__new__>` que cela doit être fait.

.. code-block:: python

    class MySingleton():

        _instance = None

        def __new__(cls, *args, **kwargs):

            if cls._instance is None:
                cls._instance = super(MySingleton, cls).__new__(cls, *args, **kwargs)

            return cls._instance

.. warning:: Le désavantage de cette méthode est qu'en cas d'héritage multiple
    une classe fille peut surcharger __new__.

    Pour contrer cet effet il faut passer par une métaclasse.

.. code-block:: python

    class MySingleton(type):

        _instance = None

        def __call__(cls, *args, **kwargs):

            if cls._instances is None:
                cls._instances = super(Singleton, cls).__call__(*args, **kwargs)

            return cls._instances

    #Python2
    class MyClass(BaseClass):
        __metaclass__ = MySingleton

    #Python3
    class MyClass(BaseClass, metaclass=MySingleton):
        pass

Il existe un pattern de Singleton alternatif : le `Borg`_. Il permet le partage
des états entre objets et non de l'instance.

.. _Borg: http://code.activestate.com/recipes/66531-singleton-we-dont-need-no-stinkin-singleton-the-bo/

Modules
=======

À chaque niveau d'arborescence, il faut mettre un fichier ``__init__.py``. Il
doit contenir au moins 1 caractère pour d'obscures raisons de suppression de
fichiers vides par windows lors des zip/unzip.

Si on souhaite créer un module vide, qui n'a vocation qu'à contenir d'autres
modules, il faut créer un fichier ``__init__.py`` contenant::

    __import__("pkg_resources").declare_namespace(__name__)

Outils
======

Développement
-------------

virtualenv
^^^^^^^^^^

Isolation de l'environnement python. On a cloné le binaire python, donc on ne
suit pas les mises à jours faites par le système. La librairie standard est
liée dynamiquement (symlink). On peut l'activer en faisant
``virtualenv <dossier>`` puis source ``<dossier>/bin/activate``.

pew
^^^

`pew <https://github.com/berdario/pew>`_ permet de créer un shell complet avec
l'environnement de virtualenv.

Déploiement
-----------

setuptools
^^^^^^^^^^

.. code-block:: shell

    python setup.py develop

permet de faire un lien symbolique vers la librairie en cours de développement.

.. _my pip:

pip
^^^

Utilitaire officiel de gestion des packets via le site PyPi_.

Attention à la gestion des versions des dépendances, qui peuvent rentrer en
conflit les unes par rapport aux autres.

.. _Pypi: https://pypi.python.org/pypi

buildout
^^^^^^^^

Gestionnaire d'installation et de dépendences, qui permet apparement d'isoler
de gérer assez finement les impacts que ça peut avoir sur le système
(site-packages, versions concurentes). Il y de gros tutos et guides
sur le `site officiel <http://www.buildout.org/en/latest/>`_.

À installer depuis `bootstrap <http://downloads.buildout.org/2/bootstrap.py>`_,

Fonctionne sur le modèle des recipes

.. todo:: à compléter

Lu ici-et-là qu'il est quand même assez lourd et difficilement configurable.

Debug
-----

.. code-block:: python

    import pdb; pdb.set_trace()

* ``l`` affiche le contexte
* ``a`` affiche les variables
* ``c`` continue
* ``n`` ligne suivante

Tests unitaires
---------------

doctest
^^^^^^^

.. code-block:: python

    def compute(nba, nbb):
        """Doc here

        >>> compute(2,3)
        5

        >>> compute(2, '3')
        Traceback (most recent call last):
        ...
        TypeError: unsupported operand type(s) for +: 'int' and 'str'

        >>> compute(5,5,2)
        Traceback (most recent call last):
          File "<input>", line 1, in <module>
        TypeError: compute() takes exactly 2 arguments (3 given)

        """
        return nba + nbb

.. code-block:: python

    python -m doctest -v <fichier.py>

On peut déporter les test dans un fichier \*.txt pour ne pas trop surcharger
la docstring.

Cf. `Sam\&Max <http://sametmax.com/un-gros-guide-bien-gras-sur-les-tests-unitaires-en-python-partie-4/>`__

unitttest
^^^^^^^^^

.. code-block:: python

    import unittest

    class TestTools(unittest.TestCase):

        def testCompute(self):
            from cs.formation import compute

            self.assertEquals(compute(2,5), 7)
            self.assertRaises(TypeError, compute, 2,'3')
            self.assertRaises(TypeError, compute, 2, 3, 5)

    if __name__ == '__main__':
        unittest.main()

Cf. `Sam\&Max <http://sametmax.com/un-gros-guide-bien-gras-sur-les-tests-unitaires-en-python-partie-2/>`__

nosetest
^^^^^^^^

.. code-block:: shell

    pip install nose

Permet de lancer des tests de tous types (unittest, doctest, etc) et d'avoir
la couverture de ceux-ci.

.. code-block:: shell

    nosetests --with-doctest --with-coverage -v myProject/

py.test
^^^^^^^

Très puissant outil de tests, mais fait un peu trop de trucs ésotériques au
niveau des imports. Comme nosetest, il permet de lancer des tests issus
d'autres suites (doctests, unittest, etc.).

Lire l'article de `Sam\&Max <http://sametmax.com/un-gros-guide-bien-gras-sur-les-tests-unitaires-en-python-partie-3/>`__
vachement complet, notamment la partie *Outils* qui liste les extensions
existantes.

On peut citer : 

    * capsys : permet de capturer les stdout/stderr
    * monkeypatch : Modification d'objets à la volée
    * tmpdir : Dossier temporaires

Il y a aussi une foule d'options sympa:

    * ne lancer que les tests dont le nom contient une expression
    * ignorer un path
    * tester aussi les doctest, unittest et nose

tox
^^^

Si j'ai bien compris, c'est un outil d'automatisation des tests, mais il faut
creuser/vérifier `ici <https://testrun.org/tox/latest/>`_.

Documentation
^^^^^^^^^^^^^

`Sphinx <http://sphinx-doc.org/>`_ est la clé !

    * Language extensible
    * Génére la liste des todo automatiquement.
    * L'idée c'est de piloter la structuration de la documentation.
    * ``litteralinclude`` pour mettre des morceaux de codes dans le corps de
      page
    * ``automodule`` permet d'aller chercher les docstring d'un module.

Profiling
^^^^^^^^^

.. code-block:: shell

    python -m cProfile -o profile.pstats fibo.py

pour avoir le nombre d'appels sur chaque fonction.

.. code-block:: shell

    pip install gprof2dot
    gprof2dot -f pstats profile.pstats | dot -Tpng -o output.png

.. image:: _static/profiling.png

On peut également utiliser

.. code-block:: shell

    pip install memory_profiler

qui fait du profiling ligne par ligne et fournit également le décorateur
``@profile``.
Par contre ce n'est pas super précis, parce que python n'a que des références.
Ça ne correspond donc pas vraiment à ce qui est fait par python en mémoire.

.. note:: ça ne remplacera pas gdb pour la détection de fuites.

Librairies sympas
=================

+----------------------------------+-----------------------------------------------------------------------------------+
| Nom                              | Description                                                                       |
+==================================+===================================================================================+
| :py:mod:`__future__`             | Permet d'avoir, en python2, des comportements apparus en python3                  |
|                                  | (unicode partout, print, etc.)                                                    |
+----------------------------------+-----------------------------------------------------------------------------------+
| `Asyncio`_                       | Multi-threading (python3.4, mais existe en non-garanti sous python2,              |
|                                  | sous le nom de trollus).                                                          |
+----------------------------------+-----------------------------------------------------------------------------------+
| `BeautifulSoup`_                 | html et xml, même très mal formatté                                               |
+----------------------------------+-----------------------------------------------------------------------------------+
| :py:mod:`csv`                    | Parsing de fichiers CSV                                                           |
+----------------------------------+-----------------------------------------------------------------------------------+
| `docopt`_                        | Alternative à :py:mod:`argparse`, fait tout à partir d'une chaine d'usage         |
+----------------------------------+-----------------------------------------------------------------------------------+
| `Fabric`_                        | Ssh, pour faire du déploiement par exemple, basé sur paramiko                     |
+----------------------------------+-----------------------------------------------------------------------------------+
| `Hachoir`_                       | Lecture de fichiers, métadonnées, réparations de binaires dégradés                |
+----------------------------------+-----------------------------------------------------------------------------------+
| :py:mod:`logging`                | Module de gestion des niveaux de log                                              |
+----------------------------------+-----------------------------------------------------------------------------------+
| `lxml`_                          | Parsing html et xml                                                               |
+----------------------------------+----------------------------------------------------------+------------------------+
| :py:mod:`multiprocessing`        | Faire des forks comme un fou                             | Utilisent la même API, |
+----------------------------------+----------------------------------------------------------+ ils sont donc          +
| :py:mod:`threading`              | À préférer à :py:mod:`thread`, mais peut être            | facilement             |
|                                  | limité par le :term:`GIL <Global Interpreter Lock>`.     | interchangeable        |
|                                  | Reste quand même super s'il y a beaucoup d'IO (fichiers, |                        |
|                                  | RAM, etc.).                                              |                        |
+----------------------------------+----------------------------------------------------------+------------------------+
| `Paramiko`_                      | ssh                                                                               |
+----------------------------------+-----------------------------------------------------------------------------------+
| `peewee`_                        | client ORM très léger pour sqlite, MySQL & PostgreSQL                             |
+----------------------------------+-----------------------------------------------------------------------------------+
| :py:mod:`pickle`                 | Sérialisation d'objets                                                            |
+----------------------------------+-----------------------------------------------------------------------------------+
| `PIL`_                           | Python Imaging Library (``pip install pillow`` ou ``pilotk``)                     |
+----------------------------------+-----------------------------------------------------------------------------------+
| :py:mod:`queue`                  | Gestion de queues (FIFO, LIFO, etc...). voir :py:mod:`Queue` en python 2.         |
+----------------------------------+-----------------------------------------------------------------------------------+
| `requests`_                      | Alternative plus haut niveau à :py:mod:`urllib`                                   |
+----------------------------------+-----------------------------------------------------------------------------------+
| `Scapy`_                         | Manipulation de paquets réseaux                                                   |
+----------------------------------+-----------------------------------------------------------------------------------+
| `SQLAlchemy`_                    | Connection à une BdD SQL                                                          |
+----------------------------------+-----------------------------------------------------------------------------------+
| `zodb`_                          | Bdd historisée et transactionnelle                                                |
|                                  | (très rapide en lecture, mais moins en écriture).                                 |
+----------------------------------+-----------------------------------------------------------------------------------+

.. Liste des liens vers les différentes docs en ligne
.. _Asyncio: https://www.python.org/dev/peps/pep-3156/
.. _BeautifulSoup: http://www.crummy.com/software/BeautifulSoup/bs4/doc/
.. _docopt: http://docopt.readthedocs.org/en/latest/
.. _Fabric: http://docs.fabfile.org/en/1.10/
.. _Hachoir: https://bitbucket.org/haypo/hachoir/wiki/Home
.. _lxml: http://lxml.de/
.. _Paramiko: https://github.com/paramiko/paramiko/
.. _peewee: http://peewee.readthedocs.org/
.. _PIL: http://pillow.readthedocs.org/
.. _requests: docs.python-requests.org/en/latest/
.. _Scapy: http://secdev.org/projects/scapy/
.. _SQLAlchemy: http://www.sqlalchemy.org/
.. _zodb: http://www.zodb.org/en/latest/

Sinon il y a la super liste de
`Sam\&Max <http://sametmax.com/tres-grand-listing-des-libs-tierce-partie-les-plus-utiles-en-python/>`__.
Ils essayent de la mettre à jour régulièrement.

.. _mal:

Le mal !
========

``import *``, :py:func:`eval` et :keyword:`global` : c'est **mal** !

Références
==========

* `Doc python officielle <https://docs.python.org/>`_ (attention à choisir la bonne version)
* La `Librairie Standard <https://docs.python.org/2/library/index.html>`_
* `Les PEPs <https://www.python.org/dev/peps/>`_
* `Python Module Of The Week <http://pymotw.com/2/>`_ présentation des modules de la stdlib. Très complets
* `Sam\&Max <http://sametmax.com>`_
* `Intermediate Python <http://book.pythontips.com/en/latest/>`_

TODO
====

.. todolist::

Indices et tables
==================

* :ref:`genindex`
* :ref:`search`

