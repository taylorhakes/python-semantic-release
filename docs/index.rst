.. include:: ../README.rst

Documentation Contents
======================

.. toctree::
   :maxdepth: 1

   commands
   Strict Mode <strict_mode>
   configuration
   commit-parsing
   Changelog Templates <changelog_templates>
   Multibranch Releases <multibranch_releases>
   automatic-releases/index
   Python Semantic Release GitHub Action <github-action>
   troubleshooting
   contributing
   contributors
   Migrating from Python Semantic Release v7 <migrating_from_v7>
   Internal API <api/modules>
   Algorithm <algorithm>
   View on GitHub <https://github.com/python-semantic-release/python-semantic-release>

Getting Started
===============

If you haven't done so already, install Python Semantic Release following the
instructions above.

There is no strict requirement to have it installed locally if you intend on
:ref:`using a CI service <automatic>`, however running with :ref:`cmd-main-option-noop` can be
useful to test your configuration.

Generating your configuration
-----------------------------

Python Semantic Release ships with a command-line interface, ``semantic-release``. You can
inspect the default configuration in your terminal by running

``semantic-release generate-config``

You can also use the :ref:`-f/--format <cmd-generate-config-option-format>` option to specify what foramt you would like this configuration
to be. The default is TOML, but JSON can also be used.

You can append the configuration to your existing ``pyproject.toml`` file using a standard redirect,
for example:

``semantic-release generate-config --pyproject >> pyproject.toml``

and then editing to your project's requirements.

.. seealso::
   - :ref:`cmd-generate-config`
   - :ref:`configuration`


Setting up version numbering
----------------------------

Create a variable set to the current version number.  This could be anywhere in
your project, for example ``setup.py``::

   from setuptools import setup

   __version__ = "0.0.0"

   setup(
      name="my-package",
      version=__version__,
      # And so on...
   )

Python Semantic Release can be configured using a TOML or JSON file; the default configuration file is
``pyproject.toml``, if you wish to use another file you will need to use the ``-c/--config`` option to
specify the file.

Set :ref:`version_variables <config-version-variables>` to a list, the only element of which should be the location of your
version variable inside any Python file, specified in standard ``module:attribute`` syntax:

``pyproject.toml``::

   [tool.semantic_release]
   version_variables = ["setup.py:__version__"]

.. seealso::
   - :ref:`configuration` - tailor Python Semantic Release to your project

Setting up commit parsing
-------------------------

We rely on commit messages to detect when a version bump is needed.
By default, Python Semantic Release uses the
`Angular style <https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#commits>`_.
You can find out more about this in :ref:`commit-parsing`.

.. seealso::
   - :ref:`config-branches` - Adding configuration for releases from multiple branches.
   - :ref:`commit_parser <config-commit-parser>` - use a different parser for commit messages.
     For example, Python Semantic Release also ships with emoji and scipy-style parsers.
   - :ref:`remote.type <config-remote-type>` - specify the type of your remote VCS.

Setting up the changelog
------------------------

.. seealso::
   - :ref:`Changelog <config-changelog>` - Customize the changelog generated by Python Semantic Release.
   - :ref:`changelog-templates-migrating-existing-changelog`

.. _index-creating-vcs-releases:

Creating VCS Releases
---------------------

You can set up Python Semantic Release to create Releases in your remote version
control system, so you can publish assets and release notes for your project.

In order to do so, you will need to place an authentication token in the appropriate
environment variable so that Python Semantic Release can authenticate with the remote
VCS to push tags, create releases, or upload files.
You should use the appropriate steps below to set this up.

GitHub (``GH_TOKEN``)
"""""""""""""""""""""

Use a personal access token from GitHub. See :ref:`automatic-github` for
usage. This token should be stored in the ``GH_TOKEN`` environment variable

To generate a token go to https://github.com/settings/tokens
and click on *Personal access token*.

GitLab (``GL_TOKEN``)
"""""""""""""""""""""

A personal access token from GitLab. This is used for authenticating when pushing
tags, publishing releases etc. This token should be stored in the ``GL_TOKEN``
environment variable.

Gitea (``GITEA_TOKEN``)
"""""""""""""""""""""""

A personal access token from Gitea. This token should be stored in the ``GITEA_TOKEN``
environment variable.

.. seealso::
   - :ref:`Changelog <config-changelog>` - customize your project's changelog.
   - :ref:`upload_to_vcs_release <config-publish-upload-to-vcs-release>` -
     enable/disable uploading artefacts to VCS releases
   - :ref:`version --vcs-release/--no-vcs-release <cmd-version-option-vcs-release>` - enable/disable VCS release
     creation.
   - `upload-to-gh-release`_, a GitHub Action for running ``semantic-release publish``

.. _upload-to-gh-release: https://github.com/python-semantic-release/upload-to-gh-release

.. _running-from-setuppy:

Running from setup.py
---------------------

Add the following hook to your ``setup.py`` and you will be able to run
``python setup.py <command>`` as you would ``semantic-release <command>``::

   try:
       from semantic_release import setup_hook
       setup_hook(sys.argv)
   except ImportError:
       pass

.. note::
   Only the :ref:`version <cmd-version>`, :ref:`publish <cmd-publish>`, and
   :ref:`changelog <cmd-changelog>` commands may be invoked from setup.py in this way.

Running on CI
-------------

Getting a fully automated setup with releases from CI can be helpful for some
projects.  See :ref:`automatic`.
