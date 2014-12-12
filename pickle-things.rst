Reproducing Problems
====================

Get quote from production in test environment
---------------------------------------------

On Server, where test is a MarketQuoteSolarPv1::

    python manage.py shell

.. code-block:: python
    
    >> import cPickle as pickle
    >> out_s = open('marketquote_dump.pkl', 'wb')
    >> pickle.dump(test, out_s)
    >> out_s.close()

On client::

    scp eslive@energysage.com:~/marketquote_dump.pkl /home/tom/energysage/es-site/es/marketquote_dump.pkl
    python manage.py shell

.. code-block:: python

    >> import cPickle as pickle
    >> in_s = open('marketquote_dump.pkl', 'rb')
    >> test = pickle.load(in_s)
    >> old = MarketQuote.objects.get_subclass(pk=1)
    >> test.supplier = old.supplier
    >> test.submitter = old.submitter
    >> test.property_object_id = old.property_object_id
    >> test.market_property = old.market_property
    >> test.marketquote_ptr_id = old.marketquote_ptr_id
    >> test.save()

now that the correct property and supplier is associated with the quote it can
be "touched up" in the django admin.
