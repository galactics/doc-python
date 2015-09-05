Spécificité du langage
######################

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

Un peu plus d'infos sur `cette présentation <http://nedbatchelder.com/text/names1.html>`_
au PyCon 2015.

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

On peut reproduire l'immutabilité sur des objets en utilisant :py:attr:`object.__slots__`.

.. code-block:: python

    class Immutable(object):
        """An immutable class
        """
        __slots__ = ["one", "two", "three"]

        def __init__(self, one, two, three):
            """Constructor"""
            super(Immutable, self).__setattr__("one", one)
            super(Immutable, self).__setattr__("two", two)
            super(Immutable, self).__setattr__("three", three)

        def __setattr__(self, name, value):
            msg = "'{0}' has no attribute {1}".format(self.__class__, name)
            raise AttributeError(msg)