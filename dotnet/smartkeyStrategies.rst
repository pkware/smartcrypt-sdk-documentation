Smartkey Strategies
===================

Determining your strategy for structuring your Smartkeys is important for security and ease of management. Below are some typical scenarios and strategies for structuring keys in each scenario. Before continuing, it is important to have a good understanding of :doc:`Smartkeys <faq>`

The ``Kind`` property of Smartkeys is used to sort Smartkeys into logical groups. It is used in conjunction with the Kind filter on Applications to create a whitelist of which groups of keys an application may have access to. This is useful if displaying Smartkeys in a UI, when having to make statements about which machines may have a Smartkey, and for applying internal policies to Smartkey distribution. Let's demonstrate with an example.

Recall that _permission_ to use the key is tied to a User identity, the whitelist only servers to determine if that user can use the Smartkey with a specific Application.

General use
-----------

In most cases, an application only needs one Kind associated with it. For example, you might indicate that the Application for your customer management system (CMS) should receive ``acme_cms`` keys, but that your custom instant messenger (IM) should receive ``acme_im`` keys.

In some cases though, you may want to enforce additional rules.

Kinds for security groups
-------------------------

You may have a set of Smartkeys that are used for extremely sensitive data, regardless of the application, with a Kind of ``acme_sensitive``. In this case, your CMS and IM applications should both be allowed to use these key Kinds, in additional to their application-specific Kinds.

Kinds for key purpose
---------------------

You may have a set of Smartkeys that are used for a specific type of data. For example, you may define the Kind ``acme_database`` for all keys that are used to encrypt database columns. In this case, you would want both your CMS and your IM applications to have access to these Kinds so they can use the corresponding Smartkeys to encrypt data before storing them.
