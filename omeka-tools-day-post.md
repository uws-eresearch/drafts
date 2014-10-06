
---
title: Is Omeka aDORAable?

author:
- name: Peter Sefton
  affiliation: University of Western Sydney
- name: Andrew Leahy
  affiliation: University of Western Sydney
- name: Gerry Devine
  affiliation: University of Western Sydney
- name: Jake Farrell
  affiliation: Intersect, Australia
-


So, we have been asking this 'Is it aDORAble?' question about a few different software packages, and putting them through their paces at a series of Tuesday 'tools days' hosted by UWS eResearch. What we're asking is, "Is this software going to be one of our supported Working Data Repositories for researcher cohorts?" That is, how does it rate as a [DORA], a Digital Object Repository for Academe?

This week we had our biggest ever tools-day event, with external people joining the usual eResearch suspects. Thanks to Jacqueline Spedding from the Dictionary of Sydney, Michael Lynch & Sharyn Wise from UTS and Cindy Wong and Jake Farrell from Intersect for coming along.

Omeka is a lightweight digital repository / website building solution, originally targeting the Galleries, Archives & Museums space.

# TL;DR

So what were we wanting to know about Omeka? The external folks came along for a variety of reasons, including just finding out more about it, but at UWS we wanted to know the following (with short answers, so you don't have to read on).

*  Is this something we can recommend for researchers with the kinds of research collections Omeka is known for?

    Answer: almost certainly yes, unless we turn up any major problems in further testing, this is a good, solid, basic repository for Digital Humanities projects. So, for image and document based collections with limited budgets this looks like an obvious choice.

*  Can Omeka be used to build a semantically-rich website in a research/publishing project like the [Dictionary of Sydney][DoS]?

   (The reason we're asking this, is that UWS has a couple of projects with some similarities to the Dictionary, and we at UWS are interested in exploring what options there are for building and maintaining a big database like this. The Dictionary uses an open source code-based called [Heurist]. Anyway, we have some data from Hart Cohen's [Journey to Horseshoe Bend] project which was exported from an unfinished attempt to build a website using Heurist, and despite Omeka not being an obvious choice of platform for some, Peter Sefton saw something worth pursuing.)

   The verdict? Still working on it, but reasonably promising so far.

* In summary, beyond its obvious purpose, is this a potential generic Digital Object Repository of Academe ([DORA])?

   Maybe. Of all the repository software we've tried at tools-days and looked at behind the scenes, this seems to be the most flexible and easily approachable.


# Good

Omeka has a lot to recommend it:

* It's easy to get up and running.

* It's easy to hack, and easy to hack well, since it has plugins and themes that let you customise it without touching the core code. These are easy enough to work with that we had people getting (small) results on the day. More on that below.

* It uses the Digital Object Pattern ([DOP]) - ie at the heart of Omeka are digital objects called Items with metadata, and attached files. 

* It has an API which just works, and can add items etc, although there are some complexities, more on which below.

* It has lots of built-in ways to ingest data, including (buggy) CSV import and OAI-PMH harvesting.

# Bad

There are some annoyances:

* The documentation, which at first glance seems fairly comprehensive is actually quite lacking. Examples of the plugin API are incorrect, and the description of the external API are pretty terse and very short on examples (eg they don't actually give an example of how to use your API key, or the pagination)/

* The API while complete is quite painful to use if you want to add anything - to add an item with metadata it's not as simple as saying {"title": "My title"} or even {"dc:title": "My Title"} - you have to do an API call to find elements called Title, from the different element sets, then pick one and use *that*. And copy-pasting someone else's example is hard: their metadata element 50 may not be the same as yours. That's nothing a decent API library wouldn't take care of, and I'm looking for a student who'd like to take the Python API on as a project.

* No access control to speak of.

* By default the MYSQL search is set up to only search for 4 letter words or greater, so you can't search for CO2 or PTA (Parramatta) both of which are in our test data; totally fixable with some tweaking.

* Measured against our [principles], there's one clear gap. We want to encourage all use of metadata to embrace linked-data principles and use URIs to identify things, in preference to strings. So while Omeka scores points for shipping with Dublin Core metadata, it loses out for not supporting *linked* data. If only it let you have a URI as well as a string value for any metadata field!

Since the hack day we have some more news on Omeka's linked data support 



# Can it do Linked Data
There are a few different ways that the current version of Omeka may support Linked Data. THe best way forward is probably to use the ItemRelations plugin.
  
* The Item Relations plugin desperately needs a new UI element to do lookups as at the moment you need to know the integer ID of the item you want to link to. Michael Lynch and Lloyd Harischandra both looked at various aspects of this problem on the day.
	
* Item Relations don't show up in the API. But the API is extensible, so that should be doable, should be simple enough to add a resource for item_realations and allow thevocab lookups etc needed to relate things to each other as (essentially) Subject Predicate Object. PT's been [working on this as a spare-time project][API].
	
* Item Relations doesn't allow for a text label on the relation or the endpoint, so while you might want to say someone is the dc:creator of a resource, you only see the "Creator" label and the title of the item you link to. What if you wanted to say "Dr Sefton" or "Petiepie" rather than "Peter Sefton" but still link to the same item?



# What we did

![Slightly doctored photo, either that or Cindy attended twice!](http://eresearch.uws.edu.au/public/om-pano.jpg)

*Gerry Devine showed off his "PageMaker" Semantic CMS*: Gerry says:

> The SemanticPageMaker (temporary name) is an application that allows for the creation of 'Linked Data'-populated web pages to describe any chosen entity. Web forms are constructed from a pre-defined set of re-usable semantic tags which, when completed, automatically produce RDFa-enabled HTML and a corresponding JSON-LD document. The application thus allows semantically-rich information to be collected and exposed by users with little or no knowledge of semantic web terms.
>
> I have attached some screenshots from my local dev instance as well as an RDFa/html page and a JSON-LD doc that describes the FACE facility (just dummy info at this stage) - note the JSON-LD doesn’t expose all fields (due to duplicated keys) 
>
> A test instance is deployed on Heroku (feel free to register and start creating stuff – might need some pointers though in how to do that until I create some help pages):
>
> https://desolate-falls-4138.herokuapp.com/
>
> Github:
>
> https://github.com/gdevine/SemanticPageMaker


This might be the long-lost the missing link: a simple semantic CMS which doesn't try to be a complete semantic stack with ontologies etc, it just allows you to define entities realtions and give each type of entity a URI, and let them relate to each other and to be a good Linked Data citizen providing RDF and JSON data. Perfect for describing research context.





And during the afternoon, Gerry worked on making his CMS able to be used for lookups, so for example if we wanted to link an Omeka item to a facility at [HIE] we'd be able to do that via a lookup. We're looking at building on work, the [Fill My List][FML] (FML) project started by a team from Open Repositories 2014 on a universal URI lookup service with a consitent API for different sources of truth. Since the tools-day Lloyd has installed a UWS copy of FML so we can start experimenting with it with our family of repositories and research contexts.


 Lloyd and Michael both worked on metadata lookups

Michael got a proof-of-concept UI going so that a user can use auto-complete to find Items rather than having to copy IDs.

*PT and Jacqueline chatted about rich semantically-linked  data-sets* like the Dictionary of Sydney. In preparation for the workshop, PT tried taking the data from the [Journey to Horseshoe Bend][JTHB] project, which is in a similar format to the Dictionary, putting it in a spreadsheet with multiple worksheets and importing it via a very dodgy [Python Script][xslt2omeka]. 

Peter Bugeia investigated how environmental-science data would look in Omeka, by playing with the API to 


*Sharyn and Andrew tried to hack together a simple plugin*. Challenge: see if we can write a plugin which will detect YouTube links in metadata and embed a YouTube player (as a test case for a more general type of plugin that can show web previews of lots of different kinds of data). They got their hack to the "Hello World, I managed to get something on the screen" stage in 45 minutes, which is encouraging.

*Jake looked at map-embedding*: we had some sample data from UWS of KMZ (compressed Google-map-layers for UWS campuses), we wondered if it would be possible to show map data inline in an item page. Jake made some progress on this - the blocker isn't Omeka it was finding a good way to do the map embedding.

*Cindy* continued the work she's been doing with Jake on the Intersect press-button Omeka deployment. They're using something called [Snap Deploy] and [Ansible].

Jake says:
> Through our Snapdeploy service Intersect are planning to offer researchers the ability to deploy their own instance of OMEKA with just a click of a button, with no IT knowledge required. All you need is an AAF log in and Snapdeploy will handle the creation of your NeCTAR Cloud VM and the deployment of OMEKA to that VM for you. We are currently in the beginning stages of adapting the Snapdeploy service to facilitate an Omeka setup and hope to offer it soon. We would also like feedback from you as researchers to let us know if there are any Omeka plug-ins that you think we could include as part of our standard deployment process that would be universally useful to the research community, so that we can ensure our Omeka product offers the functionality that researchers actually need.



*David* explored the API using an obscure long forgotten programming language, "Java" we think he called it.

# What would an Omeka service look like?

If we wanted to offer this at UWS or beyond as well as use it for projects beyond the DH sphere, what would a supported service look like?

If we were to take Omeka out of it's core comfort zone, like say being the working data repository in an engineering lab there are a number of things we'd want to do:

* Create some user-facing forms for data uploads these would need to be simpler than the full admin UI with (you guessed it) lookups for almost everything, People, Subject codes, research context such as facilities.

* Create (at least) group-level access control probably per-collection.

* Build a generic framework for previewing or viewing files of various types. In some cases this is very simple, via the addition of a few lines of HTML, in others we'd want to have some kind of workflow system that can generate derived files.

* Fix the things noted above: better API library, Linked Data Support, 

To make a sustainable service, we'd want to:

* Work out how to provide robust hosting with an optimal number of small Omeka servers per host (is it one? is it ten?).

* Come up with a generic data management plan: "We'll host this for you for 12 months. After which if we don't come to a new arrangement your site will be archived and given a DOI and the web site turned off". Or something.

[DOP]: http://ptsefton.com/2014/09/22/digital-object-pattern-dop-vs-chucking-files-in-a-database-approaches-to-repository-design.htm

[xslt2omeka]: https://github.com/uws-eresearch/omekadd/blob/master/xlsx2omeka.md

[HIE]: http://uws.edu.au/hie

[FML]: https://github.com/gobfrey/OR2014-chalege

[Journey to Horseshoe Bend]: http://pubsites.uws.edu.au/coa/soca/jthb/

[Snap Deploy]: http://www.acronis.com/en-au/business/enterprise-solutions/image-deployment/

[Ansible]: http://www.ansible.com/home

[do]: http://www.dlib.org/dlib/january10/reilly/01reilly.html

[DoS]: http://home.dictionaryofsydney.org/


[DORA]: http://eresearch.uws.edu.au/blog/2014/09/11/who-is-dora/

[principles]: http://eresearch.uws.edu.au/blog/2014/08/08/principles-for-eresearch-systems-development-and-selection/

[DOP-Basic]: http://ptsefton.com/wp-content/uploads/2014/09/dop-basic.png

[file-pattern]: http://ptsefton.com/wp-content/uploads/2014/09/file-pattern.png

[file-object-pattern]: http://ptsefton.com/wp-content/uploads/2014/09/file-object-pattern.png

[JTHB]: http://pubsites.uws.edu.au/coa/soca/jthb/

[Heurist]: https://code.google.com/p/heurist/

[API]: https://github.com/uws-eresearch/plugin-ItemRelations
