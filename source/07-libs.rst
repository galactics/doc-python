Librairies
##########

Package et Module
=================

Le sujet de la création de package et module est le sujet d'un tuto
`ici <http://www.dabeaz.com/modulepackage/ModulePackage.pdf>`_

Package
-------

À chaque niveau d'arborescence, il faut mettre un fichier ``__init__.py``. Il
doit contenir au moins 1 caractère pour d'obscures raisons de suppression de
fichiers vides par windows lors des zip/unzip.

Si on souhaite créer un package vide, qui n'a vocation qu'à contenir d'autres
packages, il faut créer un fichier ``__init__.py`` contenant::

    __import__("pkg_resources").declare_namespace(__name__)

Pour les packages contenant des modules, on peut faire ça::

	# __init__.py
	from .foo import *
	from .bar import *
	__all__ = (foo.__all__ + bar.__all__)

Dans les cas de gros packages, ils peuvent néanmoins avoir un effet important
sur les performances.

Les Namespace packages sont une idée sympa pour permettre à d'autres développeurs de faire des extensions de sa librairie (CF tuto p. 105).

Module
------

Un module est un fichier `.py`.


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
| `begins`_                        | Autre alternative, permettant de simplement décorer une fonction pour en faire    |
|                                  | la fonction principale du script et automatiquement créer les arguments de la     |
|                                  | commande à partir des arguments de la fonction.                                   |
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
.. _begins: https://pypi.python.org/pypi/begins
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