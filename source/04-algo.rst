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
