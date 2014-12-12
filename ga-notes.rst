What Needs to be done?
----------------------

_setDomainName('foo') must be set to properly track sub-domains.

A session from one domain can only be transferred to another domain via
_getLinkerURL, however there is only really one "domain" and a few sub-domains.
So I belive that _getLinkerURL is not required.

Custom filter to show domain
++++++++++++++++++++++++++++

To show the domain in the page reports, set a filter to include all components
of the page URL, as follows:

- In the Profile Settings page, click the Add Filter link.
- Choose Add New Filter and provide the filter a name.
- Choose Custom Filter and select Advanced on the Filter type settings.

Under Advanced settings:

- FieldA should be set to Hostname
- FieldB should be set to Request URI
- Set the values for both Field A and Field B to (.*), which is an expression
  that captures all characters.
- Set the Output To --> Constructor option to Request URI and provide $A1$B1 as
  the value for that choice.

Your reports will now show:

- www.example.com/index.php
- sub.example.com/more.php

This will capture the www.example.com portion of your URL and include that at
the beginning of your page URL in the content reports section.

Adding a filter will not effect custom data [5]_::

    filter only effects data after it is implemented and it does not effect
    your old data at all. 

Exclude work IP from Analytics
++++++++++++++++++++++++++++++

- edit button on main profile page
- profile settings -> filters applied to profile -> add filter -> create new
  filter
- add your IP address so that it is excluded from Google Analyticsq

addIgnoredRef
+++++++++++++

Adding this attribute will reduce refferals between domains which is good
because going from blog.energysage.com to www.energysage.com could result in
the existing referral information to be replaced by the internal-referral from
the blog [6]_.

The solution to this problem would be

.. code:: javascript

    _gaq.push(['_addIgnoredRef', 'energysage.com']);


Why are there multiple cookies on the sub-domains?
--------------------------------------------------

::

    A session from one domain can only be transferred to another domain via a
    properly configured click or form submission, or via _getLinkerURL.

    The passing of the cookie data via these three methods links two page views
    under a single session. If a user independently visits two sites that are
    tracking in the same profile, such as through a bookmark, these visits will
    still be counted under separate sessions. In this scenario, the linking
    methods are not invoked, and thus there is no way to determine the
    initiating session for a given user. 


    

My assumption is by "domain" Google considers "www.foo.com" and "blog.foo.com"
as two seperate domains. Therefore without the getLinkerURL sub-domains end up
with multiple GA traker cookies [1]_.

[2]_::

    The _setDomainName() method sets the Google cookies to the domain name
    string parameter that it is given, otherwise it automatically resolves the
    domain name from the location object in the DOM if no parameter is given.
    This means that on sub-domains, the cookies will be set to separate
    sub-domains.

So the reason why blogs.energysage.com has .blog.energysage.com and
.energysage.com cookies is because there is no setdomain parameter.


How Analytics Data Will be Effected
-----------------------------------

Domain to Sub-Domain will show up in Google as a referral if not properly
configured.

::

    In addition, if you have two sub-domains that have the same page name (i.e.
    index.html), then Google Analytics will combine these two into one which
    will artificially inflate your page statistics.

I believe after properly configuring _setDomainName() certain page statistics
will drop significantly (if both pages of the same name get similar traffic).
So for example blog.energysage.com and energysage.com's total visits will both
show up under /. 

"Google Analytics may consolidate the visits from identically named pages of
different sub-domains and show it as one in its reports [7]_." 




tl;dr
    The statistics for the home page will drop significantly after configuring
    GA tracking with _setDomainName()

.. code:: html

    <script type="text/javascript">

      var _gaq = _gaq || [];
      _gaq.push(['_setAccount', 'UA-19699642-2']);
      _gaq.push(['_setDomainName', 'energysage.com']);
      _gaq.push(['_addIgnoredRef', 'energysage.com']);
      _gaq.push(['_trackPageview']);

      (function() {
        var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
        ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
        var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
      })();

    </script>

In Google event tracking changes made do not effect history, only future
analytics [3]_::

    If you decide to change the category name of an object that has already
    been tracked under a different name, the historical data for the original
    category will not be re-processed, so you will have metrics for the same
    web page element listed under two categories in the reporting interface.

My thinking is that this is true for all of GA, and that making changes to how
we track sub-domains will not our history, but will reflect a slight drop in
impressions for select domains i.e. "/"


Should not use the leading dot in the url for setDomainName because we are not
using lower level subdomains i.e. foo.bar.baz.com::

    the hash code generated when you don't do any subdomain tracking on your
    main site is actually the same as the hash code generated when you use
    subdomain tracking without the leading period.

Adding the leading dot will cause may result in lost of data as the hash code
will be different and so new cookies will be created [4]_.

What this means is that if you weren't doing subdomain tracking previously,
using the leading period will cause your new Google Analytics Tracking Code to
destroy your old cookies because the hash codes don't match. This is similar to
what happens when you turn hashing off::

    Simply not including the leading period, if you don't have to, means you'll
    have less cookie reset, which will ease the transition to subdomain
    tracking.

By not adding setDomainName [5]_::

    your own site could appear as a referrer if a user accesses more than one subdomain in one visit.

Cross-Domains
-------------

::

    Modify your cross-domain profile with a filter to show the full domain in
    your content reports.  Once you have domain linking established, you will
    see only the request URI in the content reports, and not the top-level or
    sub-domain for a given page. 
    
    So for example, for visits to the following pages:
    - http://www.example.com/index.php and another page on
    - http://sub.example.com/more.php
    
    The reports will show:
    - index.php
    - more.php
      
    You will not be able to distinguish which page is from which domain.

Pages for **www.foobar.com** and **abc.foobar.com** will be indistinguishable
in Google analytics reporting when you configure the linking parameter.

SEO
---

Getting an organic ranking of > 10 does not necessarily mean that a result will
show up on the first page of Google.


Terminology
-----------

Referrals: untagged links on other web page
Direct: people who type your URL into a browser
Organic: organic search engine traffic

- Direct traffic is always overwritten by referrals, organic and tagged
  campaigns
- New campaign, referral or organic link that brings a visitor to the site
  always overrides the existing campaign cookie

"bounce" is described as a single-page visit to your site, adding event
tracking to a page will lower the amount of bounces because an additional GIF 
request from tracked events [6]_.

Top Level Domain:        baz.com
Primary Domain:          www.baz.com
Sub Domain:              foo.baz.com
Lower Level Sub Domains: bar.foo.baz.com

What is a cookie
++++++++++++++++

A cookie is just an arbitrary string sent by the server to the client as part
of the HTTP response. The client will then return this cookie back to the
server in subsequent requests.

Google Analytics Cookies
~~~~~~~~~~~~~~~~~~~~~~~~

::

    __utma  long term cookie containing the main details of the user

            171161141.906029690.1372713225.1375100444.1375109939.9

            Domain hash:
                Unique number identifying the user. Useful for keeping track of
                return visits and as a primary key for storing locally.
            UID:
                ----
            Time of initial visit:
                Unix timestamp of this visitors first ever visit to the website.
            Beginning of previous session:
                Unix timestamp of this visitors previous visit to the website.
            Beginning of current session:
                Unix timestamp of this visitors current visit to the website
                (the start of the current session).
            Session Counter:
                Total number of sessions.

    __utmb  current session cookie containing details about the current website
            visit (its a hash of the domain)

            Segment 1: Pages viewed in the current session
            Segment 2: Unknown (My notes say Responses…)

    __utmc  no expiration cookie, used to determine if the user has closed their
            browser

    __utmz  current session containing referral details about the current visit

            Segment 2: Referral Count
            Segment 3: Source Count
            Segment 4: This segment contains all the information about where
                       the visitor came from. It is seperated by pipes and then
                       the label/value is seperated again by an equals sign. An
                       example for this segment:

                       “utmcsr=google|utmccn=(organic)|utmcmd=organic|utmctr=keywords”.

                       The information is as follows:
                       utmcsr:   Source
                       utmcmd:   Medium (e.g. for google it can be organic or adwords)
                       utmctr:   The keywords used
                       utmcct:   Campaign content. Adwords information.
                       utmccn:   Campaign name. Adwords information.
                       utmgclid: Click ID from Adwords.

    __utmv  Used to store visitor-level custom variable data. This cookie is
            created when a developer uses the _setCustomVar method with a
            visitor level custom variable. This cookie was also used for the
            deprecated _setVar method. The cookie is updated every time data is
            sent to Google Analytics.


.. [1] https://developers.google.com/analytics/devguides/collection/gajs/gaTrackingSite?csw=1#cookies
.. [2] http://www.dannytalk.com/how-to-track-sub-domains-cross-domains-in-google-analytics/
.. [3] https://developers.google.com/analytics/devguides/collection/gajs/eventTrackerGuide#Categories
.. [4] https://developers.google.com/analytics/devguides/collection/gajs/eventTrackerGuide#implementationConsiderations
.. [5] http://www.ericmobley.net/guide-to-tracking-multiple-subdomains-in-google-analytics/
.. [6] http://www.roirevolution.com/blog/2011/01/google_analytics_subdomain_tracking.php
.. [7] http://www.seotakeaways.com/cross-domain-tracking-google-analytics-works/#ixzz2b7Yl4HQg
