*django-mbtiles* serve maps from MBTiles files using Django. 
It mainly relies on `landez <https://github.com/makinacorpus/landez/>`_.

=======
INSTALL
=======

::

    pip install -r requirements.txt
    python setup.py install


=====
USAGE
=====

* Add ``mbtilesmap`` to your ``INSTALLED_APPS``
* Make sure you have ``'django.core.context_processors.static'`` in your `context processors <https://docs.djangoproject.com/en/dev/howto/static-files/#with-a-context-processor>`_

* Include mbtilesmap urls into your project

::

    urlpatterns = patterns('',
        ...
        ...
        url(r'^', include('mbtilesmap.urls', namespace='mb', app_name='mbtilesmap')),
    )

* Add the HTML header and call the template tag

::

    {% load mbtilesmap_tags %}
    ...
    {% block head %}
    {% mbtilesmap_head %}
    {{ block.super }}
    {% endblock head %}
    
    ...
    ...
    {% mbtilesmap filename %}


Example
=======

You can find a working demo project (MBTiles maps browser *livembtiles*) 
in the ``example/`` folder of the source tree (see dedicated ``README.rst`` file).


Cache with nginx
================

* Declare a cache zone in the ``http`` section :

::

    http {
        ...
        proxy_cache_path  /var/cache/nginx levels=1:2 keys_zone=master:10m inactive=7d max_size=1g;
        proxy_temp_path /var/tmp/nginx;
    }

Cache name will be ``master``, index will be ``10m``, will last ``7d`` and have a maximum size of ``1g``.

* Serve from cache for a specific location :

::

    location @proxy {
        ...
        proxy_cache             master;
        proxy_cache_key         $$scheme$$host$$uri$$is_args$$args;
        proxy_cache_valid       200  7d;
        proxy_cache_use_stale   error timeout invalid_header;
    }

See *example* project's buildout for deployment automation.


=======
AUTHORS
=======

    * Mathieu Leplatre <mathieu.leplatre@makina-corpus.com>
    * `Andreas Trawoeger <https://github.com/atrawog>`_ 
    
|makinacom|_

.. |makinacom| image:: http://depot.makina-corpus.org/public/logo.gif
.. _makinacom:  http://www.makina-corpus.com


=======
LICENSE
=======

    * Lesser GNU Public License
