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
:py:meth:`object.__delattr__` sont là pour intéragir avec des attributs
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

Attributs privés
----------------

En python l'attribut privé est plus une convention qu'une règle. L'attribut
privé d'instance se préfixe d'un underscore (``_spam``)

On peut également créer des attributs privés de classe préfixés de deux
underscore et suffixé d'un underscore ou moins (``__spam`` ou ``__spam_``).
Ces attributs sont immédiatements renommés (``_ClassName__spam`` ou
``_ClassName__spam_``).
C'est ce qu'on appèle du :ref:`name mangling <tut-private>`

.. code-block:: python

    >>> class A(object):
    ...     __bonjour = "Hello"
    ...
    >>> class B(A):
    ...     __bonjour = "World"
    ...
    >>> print(B._A__bonjour)
    Hello
    >>> print(B._B__bonjour)
    World
    >>> print(B.__yo)  # L'attribut de base disparait
    Traceback (most recent call last):
        ...
        B.__bonjour
    AttributeError: type object 'B' has no attribute '__bonjour'

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

D'autres attributs sont disponibles :ref:`ici <customization>`.

Métaclasses
-----------

Fabriquer/Modifier des classes à la volée, équivalent des :keyword:`lambda` mais pour
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

On va également pouvoir modifier le comportement de la classe (et non plus de l'instance).
On peut par exemple faire des surcharges de méthodes spéciales::

    # Classe n'ayant pas de métaclasse particulière
    class A:
        pass

    print(A)  # affiche "<class '__main__.A'>"

    # *** Exemple avec une métaclasse ***
    # déclaration de la métaclasse
    class MyMeta(type):
        def __str__(self):
            return "<my super class '{}'>".format(self.__name__)

    # déclaration de la classe utilisant la métaclasse
    class A(metaclass=MyMeta):
        pass

    print(A)  # affiche "<my super class 'A'>"

On modifie donc bien le comportement de la classe et non plus de l'objet.

Des articles explicatifs `ici <http://eli.thegreenplace.net/2011/08/14/python-metaclasses-by-example/>`_ et
`là <http://sametmax.com/le-guide-ultime-et-definitif-sur-la-programmation-orientee-objet-en-python-a-lusage-des-debutants-qui-sont-rassures-par-les-textes-detailles-qui-prennent-le-temps-de-tout-expliquer-partie-8/>`_

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

Les patterns
------------

Brandon Rhodes fait un site détaillant les `Designs Patterns <http://python-patterns.guide/>`__
appliqués à Python
