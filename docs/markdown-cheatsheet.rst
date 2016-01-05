Qi docs here!

Markdown cheatsheet: # H1 ## H2 ### H3 #### H4 ##### H5 ###### H6

Emphasis, aka italics, with *asterisks* or *underscores*.

Strong emphasis, aka bold, with **asterisks** or **underscores**.

Combined emphasis with **asterisks and *underscores***.

Strikethrough uses two tildes. [STRIKEOUT:Scratch this.]

1. First ordered list item
2. Another item ⋅⋅\* Unordered sub-list.
3. Actual numbers don't matter, just that it's a number ⋅⋅1. Ordered
   sub-list
4. And another item.

⋅⋅⋅You can have properly indented paragraphs within list items. Notice
the blank line above, and the leading spaces (at least one, but we'll
use three here to also align the raw Markdown).

⋅⋅⋅To have a line break without a paragraph, you will need to use two
trailing spaces.⋅⋅ ⋅⋅⋅Note that this line is separate, but within the
same paragraph.⋅⋅ ⋅⋅⋅(This is contrary to the typical GFM line break
behaviour, where trailing spaces are not required.)

-  Unordered list can use asterisks
-  Or minuses
-  Or pluses
-  

`I'm an inline-style link <https://www.osisoft.com>`__

`I'm an inline-style link with title <https://www.osisoft.com>`__

`I'm a reference-style link <https://www.osisoft.com>`__

`I'm a relative reference to a repository
file <../blob/master/LICENSE>`__

`You can use numbers for reference-style link
definitions <http://osisoft.com>`__

Or leave it empty and use the `link text
itself <http://www.osisoft.com>`__.

URLs and URLs in angle brackets will automatically get turned into
links. http://www.osisoft.com or http://www.osisoft.com and sometimes
osisoft.com (but not on Github, for example).

Some text to show that the reference links can follow later.

Here's our logo (hover to see the title text):

Inline-style: |alt text|

Reference-style: |alt text|

Inline ``code`` has ``back-ticks around`` it.

.. code:: javascript

    var s = "JavaScript syntax highlighting";
    alert(s);

.. code:: c#

    String s = new String();

.. code:: python

    s = "Python syntax highlighting"
    print s

::

    No language indicated, so no syntax highlighting. 
    But let's throw in a <b>tag</b>.

var s = "JavaScript syntax highlighting"; alert(s); s = "Python syntax
highlighting" print s No language indicated, so no syntax highlighting
in Markdown Here (varies on Github). But let's throw in a tag.

Colons can be used to align columns.

+-----------------+-----------------+---------+
| Tables          | Are             | Cool    |
+=================+=================+=========+
| col 3 is        | right-aligned   | $1600   |
+-----------------+-----------------+---------+
| col 2 is        | centered        | $12     |
+-----------------+-----------------+---------+
| zebra stripes   | are neat        | $1      |
+-----------------+-----------------+---------+

There must be at least 3 dashes separating each header cell. The outer
pipes (\|) are optional, and you don't need to make the raw Markdown
line up prettily. You can also use inline Markdown.

+------------+---------------+--------------+
| Markdown   | Less          | Pretty       |
+============+===============+==============+
| *Still*    | ``renders``   | **nicely**   |
+------------+---------------+--------------+
| 1          | 2             | 3            |
+------------+---------------+--------------+

    Blockquotes are very handy in email to emulate reply text. This line
    is part of the same quote.

Quote break.

    This is a very long line that will still be quoted properly when it
    wraps. Oh boy let's keep writing to make sure this is long enough to
    actually wrap for everyone. Oh, you can *put* **Markdown** into a
    blockquote.

.. raw:: html

   <dl>

.. raw:: html

   <dt>

Definition list

.. raw:: html

   </dt>

.. raw:: html

   <dd>

Is something people use sometimes.

.. raw:: html

   </dd>

.. raw:: html

   <dt>

Markdown in HTML

.. raw:: html

   </dt>

.. raw:: html

   <dd>

Does *not* work **very** well. Use HTML tags.

.. raw:: html

   </dd>

.. raw:: html

   </dl>

Three or more...

--------------

Hyphens

--------------

Asterisks

--------------

Underscores

|IMAGE ALT TEXT HERE|

.. |alt text| image:: https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png
.. |alt text| image:: https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png
.. |IMAGE ALT TEXT HERE| image:: http://img.youtube.com/vi/2HtS3QCc3Oo/0.jpg
   :target: http://www.youtube.com/watch?v=2HtS3QCc3Oo
