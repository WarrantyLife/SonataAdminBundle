The Show action
===============

This document will cover the Show action and related configuration options.

Basic configuration
-------------------

Routes
------

You can disable showing entities by removing the corresponding routes in your Admin.
For more detailed information about routes, see :doc:`routing`::

    // src/Admin/PersonAdmin.php

    final class PersonAdmin extends AbstractAdmin
    {
        protected function configureRoutes(RouteCollectionInterface $collection): void
        {
            // Removing the show route will disable showing entities.
            $collection->remove('show');
        }
    }

Group options
-------------

When adding a group to your show page, you may specify some options for the group itself.

- ``collapsed``: unused at the moment
- ``class``: the class for your group in the admin; by default, the value
  is set to ``col-md-12``.
- ``fields``: the fields in your group (you should NOT override this unless
  you know what you're doing).
- ``box_class``: the class for your group box in the admin; by default,
  the value is set to ``box box-primary``.
- ``description``: A text shown at the top of the show group.
- ``translation_domain``: The translation domain for the show group title
  (the Admin translation domain is used by default).

To specify options, do as follow::

    // src/Admin/PersonAdmin.php

    use Sonata\AdminBundle\Admin\AbstractAdmin;
    use Sonata\AdminBundle\Show\ShowMapper;

    final class PersonAdmin extends AbstractAdmin
    {
        protected function configureShowFields(ShowMapper $show): void
        {
            $show
                ->tab('General') // the tab call is optional
                    ->with('Addresses', [
                        'class'       => 'col-md-8',
                        'box_class'   => 'box box-solid box-danger',
                        'description' => 'Lorem ipsum',
                    ])
                        ->add('title')
                        // ...
                    ->end()
                ->end()
            ;
        }
    }

When extending an existing Admin, you may want to remove some fields, groups or tabs.
Here is an example of how to achieve this::

    // src/Admin/PersonAdmin.php

    use Sonata\AdminBundle\Show\ShowMapper;

    final class PersonAdmin extends ParentAdmin
    {
        protected function configureShowFields(ShowMapper $show): void
        {
            parent::configureShowFields($show);

            // remove one field
            $show->remove('field_to_remove');

            // remove a group from the "default" tab
            $show->removeGroup('GroupToRemove1');

            // remove a group from a specific tab
            $show->removeGroup('GroupToRemove2', 'Tab2');

            // remove a group from a specific tab and also remove the tab if it ends up being empty
            $show->removeGroup('GroupToRemove3', 'Tab3', true);
        }
    }

.. note::

    ``ParentAdmin`` is not allowed to be ``final`` then!

Customising the query used to show the object from within your Admin class
--------------------------------------------------------------------------

Setting up a showAction is pretty much the same as a form, which we did
in the initial setup. It is actually a bit easier, because we are only
concerned with displaying information. Smile, the hard part is already done.
The following is a working example of a ShowAction::

    // src/Admin/PostAdmin.php

    use Sonata\AdminBundle\Show\ShowMapper;

    final class ClientAdmin extends AbstractAdmin
    {
        protected function configureShowFields(ShowMapper $show): void
        {
            // here we set the fields of the ShowMapper variable,
            // $show (but this can be called anything)
            $show

                 // The default option is to display the value
                 // as text (for boolean this will be 1 or 0)
                ->add('name')
                ->add('phone')
                ->add('email')

                 // The boolean option is actually very cool
                 // true   shows a check mark and the 'yes' label
                 // false  shows a check mark and the 'no' label
                ->add('dateCafe', FieldDescriptionInterface::TYPE_BOOLEAN)
                ->add('datePub', FieldDescriptionInterface::TYPE_BOOLEAN)
                ->add('dateClub', FieldDescriptionInterface::TYPE_BOOLEAN)
            ;

        }
    }

.. tip::

    To customize the displayed label of a show field you can use the ``label`` option::

        $show->add('name', null, ['label' => 'UserName']);

    Setting this option to ``false`` will make the label empty.

Setting up a custom show template (very useful)
-----------------------------------------------

The first thing you need to do is define it in app/config/config/yml:

.. configuration-block::

    .. code-block:: yaml

        # config/packages/sonata_admin.yaml

        sonata_admin:
            title:      Acme
            title_logo: img/logo_small.png
            templates:
                show:   '@App/Admin/display_client.html.twig'

Once you have defined this, Sonata Admin looks for it in the following location:

``templates/Admin/display_client.html.twig``

Now that you have told Sonata Admin where to find the template, it is time to put one in there.

The recommended way to start is to copy the default template, and paste it into its new home.

This ensures that you can update Sonata Admin and keep all of your hard work.

The original template can be found in the following location:

``vendor/sonata-project/admin-bundle/src/Resources/views/CRUD/base_show.html.twig``

Now that you have a copy of the default template, check to make sure it works.
