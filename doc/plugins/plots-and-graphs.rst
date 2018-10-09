..
    This file is part of m.css.

    Copyright © 2017, 2018 Vladimír Vondruš <mosra@centrum.cz>

    Permission is hereby granted, free of charge, to any person obtaining a
    copy of this software and associated documentation files (the "Software"),
    to deal in the Software without restriction, including without limitation
    the rights to use, copy, modify, merge, publish, distribute, sublicense,
    and/or sell copies of the Software, and to permit persons to whom the
    Software is furnished to do so, subject to the following conditions:

    The above copyright notice and this permission notice shall be included
    in all copies or substantial portions of the Software.

    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
    IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
    FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL
    THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
    LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
    FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
    DEALINGS IN THE SOFTWARE.
..

Plots and graphs
################

:breadcrumb: {filename}/plugins.rst Pelican plugins
:footer:
    .. note-dim::
        :class: m-text-center

        `« Math and code <{filename}/plugins/math-and-code.rst>`_ | `Pelican plugins <{filename}/plugins.rst>`_ | `Links and other » <{filename}/plugins/math-and-code.rst>`_

.. role:: dot(code)
    :language: dot

These plugin allow you to render plots and graphs directly from data specified
inline in the page source. Similarly to `math rendering <{filename}/admire/math.rst>`_,
the graphics is rendered to a SVG that's embedded directly in the HTML markup.

.. note-danger:: Experimental features

    Please note that these plugins are highly experimental and at the moment
    created to fulfill a particular immediate need of the author. They might
    not work reliably on general input.

.. contents::
    :class: m-block m-default

`Plots`_
========

Download the `m/plots.py <{filename}/plugins.rst>`_ file, put it including the
``m/`` directory into one of your :py:`PLUGIN_PATHS` and add ``m.plots``
package to your :py:`PLUGINS` in ``pelicanconf.py``.

.. code:: python

    PLUGINS += ['m.plots']
    M_PLOTS_FONT = 'Source Sans Pro'

Set :py:`M_PLOTS_FONT` to a font that matches your CSS theme (it's Source Sans
Pro for `builtin m.css themes <{filename}/css/themes.rst>`_), note that you
*need to have the font installed* on your system, otherwise it will fall back
to whatever system font it finds instead (for example DejaVu Sans) and the
output won't look as expected. In addition you need the
`Matplotlib <https://matplotlib.org/>`_ library installed. Get it via ``pip``
or your distribution package manager:

.. code:: sh

    pip3 install matplotlib

The plugin produces SVG plots that make use of the
`CSS plot styling <{filename}/css/components.rst#plots>`_.

`Bar charts`_
-------------

Currently the only supported plot type is a horizontal bar chart, denoted by
:rst:`.. plot::` directive with :rst:`:type: hbar`:

.. code-figure::

    .. code:: rst

        .. plot:: Fastest animals
            :type: barh
            :labels:
                Cheetah
                Pronghorn
                Springbok
                Wildebeest
            :units: km/h
            :values: 109.4 88.5 88 80.5

    .. plot:: Fastest animals
        :type: barh
        :labels:
            Cheetah
            Pronghorn
            Springbok
            Wildebeest
        :units: km/h
        :values: 109.4 88.5 88 80.5

The multi-line :rst:`:labels:` option contain value labels, one per line. You
can specify unit label using :rst:`:units:`, particular values go into
:rst:`:values:` separated by whitespace, there should me as many values as
labels. Hovering over the bars will show the concrete value in a title.

It's also optionally possible to add error bars using :rst:`:error:` and
configure bar colors using :rst:`:colors:`. The colors correspond to m.css
`color classes <{filename}/css/components.rst#colors>`_ and you can either
use one color for all or one for each value, separated by whitespace. Bar chart
height is calculated automatically based on amount of values, you can adjust
the bar height using :rst:`:bar_height:`. Default value is :py:`0.4`.

It's possible to add an extra line of labels using :rst:`:labels_extra:`.
Again, there should be as many entries as primary labels and values. To omit an extra label for a value, specify it as the :abbr:`reST <reStructuredText>`
comment :rst:`..`.

.. code-figure::

    .. code:: rst

        .. plot:: Runtime cost
            :type: barh
            :labels:
                Ours minimal
                Ours default
                3rd party
                Full setup
            :labels_extra:
                15 modules
                60 modules
                200 modules
                ..
            :units: µs
            :values: 15.09 84.98 197.13 934.27
            :errors: 0.74 3.65 9.45 25.66
            :colors: success info danger dim
            :bar_height: 0.6

    .. plot:: Runtime cost
        :type: barh
        :labels:
            Ours minimal
            Ours default
            3rd party
            Full setup
        :labels_extra:
            15 modules
            60 modules
            200 modules
            ..
        :units: µs
        :values: 15.09 84.98 197.13 934.27
        :errors: 0.74 3.65 9.45 25.66
        :colors: success info danger dim
        :bar_height: 0.6

`Graphs`_
=========

Download the `m/dot.py <{filename}/plugins.rst>`_ file, put it including the
``m/`` directory into one of your :py:`PLUGIN_PATHS` and add ``m.dot``
package to your :py:`PLUGINS` in ``pelicanconf.py``.

.. note-danger::

    Note that this plugin, unlike most of the others, requires at least Python
    3.5 to run properly.

.. code:: python

    PLUGINS += ['m.dot']
    M_DOT_FONT = 'Source Sans Pro'
    M_DOT_FONT_SIZE = 16.0

Set :py:`M_DOT_FONT` and :py:`M_DOT_FONT_SIZE` to a font that matches your CSS
theme (it's Source Sans Pro at :css:`16px` for
`builtin m.css themes <{filename}/css/themes.rst>`_), note that you *need to
have the font installed* on your system, otherwise it will fall back to
whatever system font it finds instead (for example DejaVu Sans) and the output
won't look as expected. In addition you need the
`Graphviz <https://graphviz.org/>`_ library installed. Get it via your
distribution package manager, for example on Ubuntu:

.. code:: sh

    sudo apt install graphviz

The plugin produces SVG graphcs that make use of the
`CSS graph styling <{filename}/css/components.rst#graphs>`_.

`Directed graphs`_
--------------------

The :rst:`.. digraph::` directive uses the ``dot`` tool to produce directed
graphs. The required directive argument is graph title, contents is whatever
you would put inside the :dot:`digraph` block. Use the :rst:`:class:` to
specify a `CSS color class <{filename}/css/components.rst#colors>`_ for the
whole graph, it's also possible to color particular nodes and edges using the
(currently undocumented) ``class`` attribute.

.. code-figure::

    .. code:: rst

        .. digraph:: Finite state machine

            rankdir=LR

            S₁ [shape=circle, class="m-primary", peripheries=2]
            S₂ [shape=circle]
            _  [style=invis]

            _  -> S₁ [class="m-warning"]
            S₁ -> S₂ [label="0"]
            S₂ -> S₁ [label="0"]
            S₁ -> S₁ [label="1"]
            S₂ -> S₂ [label="1"]

    .. digraph:: Finite state machine

        rankdir=LR

        S₁ [shape=circle, class="m-primary", peripheries=2]
        S₂ [shape=circle]
        _  [style=invis]
        b  [style=invis]

        _  -> S₁ [class="m-warning"]
        S₂ -> b  [style=invis]
        S₁ -> S₂ [label="0"]
        S₂ -> S₁ [label="0"]
        S₁ -> S₁ [label="1"]
        S₂ -> S₂ [label="1"]

For more information check the official
`GraphViz Reference <http://www.graphviz.org/doc/info/>`_, in particular the
extensive `attribute documentation <http://www.graphviz.org/doc/info/attrs.html>`_.

.. note-warning::

    Note that currently all styling is discarded and only the
    ``class`` and ``fontsize`` attributes are taken into account.

.. note-warning::

    The ``class`` attribute is new in Graphviz 2.40.1. If you have an older
    version on your system, this attribute will get ignored.

`Undirected graphs`_
--------------------

The :rst:`.. graph::` and :rst:`.. strict-graph::` directives are similar to
:rst:`.. digraph::`, but allow undirected graphs only. Again these are
equivalent to :dot:`graph` and :dot:`strict graph` in the DOT language:

.. code-figure::

    .. code:: rst

        .. graph:: A house
            :class: m-success

            { rank=same 0 1 }
            { rank=same 2 4 }

            0 -- 1 -- 2 -- 3 -- 4 -- 0 -- 2 -- 4 --1
            3 [style=solid]

    .. graph:: A house
        :class: m-success

        rankdir=BT

        { rank=same 0 1 }
        { rank=same 2 4 }

        0 -- 1 -- 2 -- 3 -- 4 -- 0 -- 2 -- 4 --1
        3 [style=filled]
