
---
title: Is Omeka aDORAable?

---

So, we have been asking this 'Is it aDORAble?' question about a few different software packages, and putting them through their paces at a series tuesday 'tools days' hosted by UWS eResearch. What we're asking is, "Is this software going to be one of our supported Working Data Repostories for researcher cohorts?" That is, how does it rate as a [DORA], a Digital Object Repository for Academe?

This week we had our biggest ever event, with external people joining the usual eResearch suspects. Thanks to Jacqeuline Spedding from the Dictionary of Sydney, Michael Lynch and  Sharyn Jones from UTS and Cindy Wong and Jake Farell from Intersect for coming along.

Omeka is a lightweight repository solution, originally targeting the Galleries, Archives & Museums space.

Omeka has a lot to recommend it:

*  It's easy to get up and running.

*  It's easy to hack, and easy to hack well, since it has plugins and themes that let you customise it without touching the core code. These are easy enough to work with that we had people getting (small) results on the day. More on that below.

* It uses the [Digital Object Pattern] (DOP) - ie at the heart of Omeka are digital object called Items with metadata, and and attached files. 

* It has an API which just works, and can add items etc, although there are some complexities, more on which below.

* It has lots of built-in ways to ingest data,

There are some annoyances:

* The documentation, which at first glance seems fairly comprehensive is actually quite lacking. Examples of the plugin API are incorrect, and the description of the external API are pretty terse and very short on examples (eg they don't actually give an example of how to use your API key, or the pagination)/

* The API while complete is a quite painful to use if you want to add anything - to add an item with metdata it's not as simple as saying {"title": "My title"} or even {"dc:title": "My Title"} - you have to do an API call to find elements called title, from the different element sets, then pick one and use *that*. And copy-pasting someone else's example is hard: their metadata element 50 may not be the same as ours. That's nothing a decent API library wouldn't take care of, and I'm looking for a student who'd like to take the Python API on as a project.

* Measured against our principles, there is a clear gap. We want to encourage all use of metadata to embrace linked-data principles and use URIs to identify things, in preference to strings. So while Omeka scores points for shipping with Dublin Core metadata, it loses out for not supporting *linked* data. If only it let you have a URI as well as a string value for any metadata field! We're not the only ones who want this, we know from sporadic forum posts that people want their linked data and the Omeka team seem to support the idea. So, we'll see what we can do to restart the conversation.

So what were we wanting to know about Omeka? The external folks came along for a variety of reasons, including just finding out more about it, but at UWS we wanted to know. I woun't make you read to the end:

*  Is this something we can recommend for researchers with the kinds of research collections Omeka is known for?
   Answer, almost certainaly yes - I'm happy\
   ]|

[do]: http://www.dlib.org/dlib/january10/reilly/01reilly.html

[DOP]: http://ptsefton.com/2014/09/22/digital-object-pattern-dop-vs-chucking-files-in-a-database-approaches-to-repository-design.htm

[DORA]: http://eresearch.uws.edu.au/blog/2014/09/11/who-is-dora/

[principled]: http://eresearch.uws.edu.au/blog/2014/08/08/principles-for-eresearch-systems-development-and-selection/

[DOP-Basic]: http://ptsefton.com/wp-content/uploads/2014/09/dop-basic.png

[file-pattern]: http://ptsefton.com/wp-content/uploads/2014/09/file-pattern.png

[file-object-pattern]: http://ptsefton.com/wp-content/uploads/2014/09/file-object-pattern.png

[NETCDF]: http://www.unidata.ucar.edu/software/netcdf/
