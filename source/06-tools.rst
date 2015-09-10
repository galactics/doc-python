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

* ``h`` affiche l'aide
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
