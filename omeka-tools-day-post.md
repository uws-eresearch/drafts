
---
title: Is Omeka aDORAable?

author:
- name: Peter Sefton
  affiliation: University of Western Sydney
- name: Author Two #ADD YOUR NAME!!!
  affiliation: University of Nowhere

---

So, we have been asking this 'Is it aDORAble?' question about a few different software packages, and putting them through their paces at a series tuesday 'tools days' hosted by UWS eResearch. What we're asking is, "Is this software going to be one of our supported Working Data Repositories for researcher cohorts?" That is, how does it rate as a [DORA], a Digital Object Repository for Academe?

This week we had our biggest ever tools-day event, with external people joining the usual eResearch suspects. Thanks to Jacqueline Spedding from the Dictionary of Sydney, Michael Lynch & Sharyn Wise from UTS and Cindy Wong and Jake Farrell from Intersect for coming along.

Omeka is a lightweight digital repository / website building solution, originally targeting the Galleries, Archives & Museums space.

# TL;DR

So what were we wanting to know about Omeka? The external folks came along for a variety of reasons, including just finding out more about it, but at UWS we wanted to know the following (with short answers, so you don't have to read on).

*  Is this something we can recommend for researchers with the kinds of research collections Omeka is known for?

    Answer: almost certainly yes, unless we turn up any major problems in further testing, this is a good, solid, basic repository for Digital Humanities projects. So, for image and document based collections with limited budgets.

*  Can Omeka be used to build a semantically-rich website in a research/publishing project like the [DIctionary of Sydney][DoS]?

   (The reason we're asking this, is that UWS has a couple of projects with some similarities to the Dictionary, and we at UWS are interested in exploring what options there are for building and maintaining a big data base like this. The Dictionary uses an open source code-based called [Heurist]. Anyway, we have some data from Hart Cohen's [Journey to Horseshoe Bend] project which was exported from a n unfinished attempt to build a website using Heurist, and despite Omeka not being an obvious choice of platform for some, Peter Sefton saw something worth pursuing.)

   The verdict? Still working on it, but reasonably promising so far.

* In summary, beyond its obvious purpose, is this a potential generic Digital Object Repository of Academe [DORA]?

   Maybe. Of all the repository software we've tried at tools-days and looked at behind the scenes, and in fact  this has seems to be the most flexible and easily approachable of any software ever.


# Good

Omeka has a lot to recommend it:

*  It's easy to get up and running.

* It's easy to hack, and easy to hack well, since it has plugins and themes that let you customise it without touching the core code. These are easy enough to work with that we had people getting (small) results on the day. More on that below.

* It uses the [Digital Object Pattern] (DOP) - ie at the heart of Omeka are digital object called Items with metadata, and and attached files. 

* It has an API which just works, and can add items etc, although there are some complexities, more on which below.

* It has lots of built-in ways to ingest data,

# Bad

There are some annoyances:

* The documentation, which at first glance seems fairly comprehensive is actually quite lacking. Examples of the plugin API are incorrect, and the description of the external API are pretty terse and very short on examples (eg they don't actually give an example of how to use your API key, or the pagination)/

* The API while complete is a quite painful to use if you want to add anything - to add an item with metdata it's not as simple as saying {"title": "My title"} or even {"dc:title": "My Title"} - you have to do an API call to find elements called title, from the different element sets, then pick one and use *that*. And copy-pasting someone else's example is hard: their metadata element 50 may not be the same as ours. That's nothing a decent API library wouldn't take care of, and I'm looking for a student who'd like to take the Python API on as a project.

* No access control to speak of.

* Measured against our [principles], there one clear gap. We want to encourage all use of metadata to embrace linked-data principles and use URIs to identify things, in preference to strings. So while Omeka scores points for shipping with Dublin Core metadata, it loses out for not supporting *linked* data. If only it let you have a URI as well as a string value for any metadata field! We're not the only ones who want this, we know from sporadic forum posts that people want their linked data and the Omeka team seem to support the idea. So, we'll see what we can do to restart the conversation.

  (There are a few different ways that Omeka may support Linked Data. Here are just a few:
  
  * Super hacky - use the HTML feature and put in RDFa \<a href="http://orcid.org/0000-0002-3545-944X" property="http://purl.org/dc/elements/1.1/creator">ptsefton</a> markup into metadata values
  
  * *Super simple but not very usable*: use bare URIs as values and (maybe) hack the Omeka theme to display some other string
  
  * *Also simple but problematic in other ways*: - use a portmanteau value like "Peter Sefton \<http://http://orcid.org/0000-0002-3545-944X>
  
  * *The [DOP] DIY approach*: Simply use the built in metadata (and Item relations) where possible/practical and also store a 'proper' RDF bitstream with richer metadata for each item, then use this to display more Linked-Data-esque item summaries, build clever external indexes.
  
  * *Elegant, but will it scale?* Use the Item Relations plugin. Create a page for Peter Sefton, with a URI, then link another record to it.

  This looks quite good, but with a few (soluble but sobering) problems:
  
    * The Item Relations plugin desperately needs a new UI element to do lookups as at the moment you need to know the integer ID of the item you want to link to. Michael Lynch and Lloyd Harischandra both looked at various aspects of this problem on the day.
	
	* Item Relations don't show up in the API. But the API is extensible, so that should be doable.
	
	* Item Relations doesn't allow for a text label on the relation or the endpoint, so while you might want to say someone is the dc:creator of a resource, you only see the "Creator" label and the title of the item you link to. What if you wanted to say "Dr Sefton" or "Petiepie" rather than "Peter Sefton" but still link to the same item?
  
  * *Change Omeka* (or write a very intrusive plugin): What if all metadata 'elements' in Omeka had an extra slot for the a URI value as well as a string value? And while we're at it, allow metadata elements to have URIs as well as names. Problem with this is it crosses into the territory of the Item Relations plugin.

 * *Bizarre ugly stuff we'd almost certainly never do in real life* One way of hacking together Linked Data support would be to resort to ugly hacks like having "Creator1String" and "Creator1URI" as two separate fields then hiding this behind the UI. Or more fun but much much worse, when someone creates a new 'Creator' allow them to add  URI by minting a new metadata element like Creator_Peter%20Sefton_URI. Pretty sure this would break Omeka and make the DORA-gods very cranky.

  )

# TODO

If we were to take Omeka out of it's core comfort zone there are a number of things we'd want to do:

* Create some user 

# What would an Omeka service look like?



[do]: http://www.dlib.org/dlib/january10/reilly/01reilly.html

[DoS] : http://home.dictionaryofsydney.org/

[DOP]: http://ptsefton.com/2014/09/22/digital-object-pattern-dop-vs-chucking-files-in-a-database-approaches-to-repository-design.htm

[DORA]: http://eresearch.uws.edu.au/blog/2014/09/11/who-is-dora/

[principles]: http://eresearch.uws.edu.au/blog/2014/08/08/principles-for-eresearch-systems-development-and-selection/

[DOP-Basic]: http://ptsefton.com/wp-content/uploads/2014/09/dop-basic.png

[file-pattern]: http://ptsefton.com/wp-content/uploads/2014/09/file-pattern.png

[file-object-pattern]: http://ptsefton.com/wp-content/uploads/2014/09/file-object-pattern.png


[Heurist]: https://code.google.com/p/heurist/
