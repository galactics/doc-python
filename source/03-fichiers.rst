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