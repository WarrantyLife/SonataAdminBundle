Bootlint
========

The admin comes with `Bootlint`_ integration.
Bootlint is an HTML linter for Bootstrap projects.

You should use it when you want add some contributions on Sonata UI to check
the eventual Twitter Bootstrap conventions' mistakes.

Enable Bootlint
---------------

To use Bootlint in your admin, you can enable it in your config file:

.. configuration-block::

    .. code-block:: yaml

        # config/packages/sonata_admin.yaml

        sonata_admin:
            options:
                use_bootlint:    true # enable Bootlint

Then open your browser debugger to look after some Bootlint warnings on console.

No warning? Congrats! Your page is fully Bootstrap compliant! ;)

.. _`Bootlint`: https://github.com/twbs/bootlint
