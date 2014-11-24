# What is this?

# What did we do?

# Lloyd: Name-authority lookups

My contributions to the Omeka day

Omeka had the capability to lookup Library of Congress related name authorities via LcSuggest plugin. I [modified the plugin][modded-lc-suggest] so that the user can customize form elements to be able to lookup from DAAO.

How it works:

1. Login to omeka admin
2. Click on LCSuggest menu item on the left menu
3. Select the element you want to add autocomplete lookup feature
4. Select the name authority you want to use. In this case, it's DAAO names

So now we have the knowledge to add as many name authorities as we want when needed.
# Peter & Gerry: Omeka as a catalogue for lab data
Our team (Gerry Devine, Peter Bugeia) looked at a gap which UWS Hawkesbury Institute for the Environment has with cataloging their laboratory data. We looked at using Omeka to catalog the data.

First some background. UWS HIE do climate change research. They use Intersect's ruby-on-rails DIVER app to capture their field facility time series sensor network data. In addition to this data, technical staff collect  a lot of field samples, mostly soil and plant samples. The samples are managed in a home-grown Ruby-on-rails app which Gerry Devine wrote called SampleTracker. Researchers use a variety of analytical and diagnostic testing equipment to analyse the samples. Once completed, the output from these tests are placed on a share drive with very little metadata. The researchers currently hand the samples and a Job Request form with the details of the analysis to be run to lab technicians who schedule and execute the jobs. This is all manual but in the future there will be a job request/scheduling system built which manages these requests. (As an aside, we looked at doing the job request system in Omeka too, but felt it was probably better to have this workflow segregated) Once a job is complete, Individual researchers know where their lab data is placed but no-one else does, so this is a problem for future discovery and re-use of the data. having a catalog solves that problem.

So the design was to have the (future) job request system send completed LabRun details (job request metadata plus log files from the diagnostic equipment) to Omeka via the Omeka API and to e-mail the researcher that the job was completed. The Labdata files themselves are very large so the current plan was to not upload them to Omeka but to leave them in the file share.

So we created a new Omeka instance on the Nectar SA node using Intersect's Snap Deploy. This took about 15 minutes to provision.  After trying to deploy an instance on "any Nectar node" failed, presumably because most Nectar nodes are maxed out, we were able to deploy to the Nectar SA node.  This worked fine but ran into an immediate problem. We found that we couldn't use the Omeka API interface due to not having command line access to the machine due to availability of public/private Intersect keys (details are a little obsure!). We found that the sample Omeka API client software uses urlLib and we needed to change this to use urlLib2 so that we could send data via the API unsecured (not ideal but ok in test mode). We ran out of time trying to get UrlLib2 running. An alternative we did't try was to simply use curl with the --insecure option which we felt would have worked. This is further work to be done. Other hackers ran into the same problem (David C). I've asked my Intersect colleagues if we could sort out the private key/public access key problem.

In the meantime we started to model the LabRun and run-results as a new ItemType in Omeka. It was easy enough to add new Omeka elements to the new LabRun item Type, but we found there was no typing or validation we could apply to these elements. When creating a new LabRun item, we set the new element up as linked open data where we thought relevant but this was painful through the UI. If we do this through the API their is less of a problem, but to make this robust it appears we'll need to configure the Item UI. It's not clear whether this can be done through the Omeka object model and for it to be doable on a per Item basis (different itemtypes will need to have different validation)  => Further work is required.

Screen shots:
![Context diagram](http://eresearch.uws.edu.au/blog/wp-content/uploads/2014/11/LabData_Omeka_Plan.jpg),
![Interaction diagram](http://eresearch.uws.edu.au/blog/wp-content/uploads/2014/11/20141120_161455.jpg),
![Omeka LabRun Item Type](http://eresearch.uws.edu.au/blog/wp-content/uploads/2014/11/Omeka-LabRun-Item-Type.png),
![Omeka LabRun Item](http://eresearch.uws.edu.au/blog/wp-content/uploads/2014/11/Omeka-Item.png)


[modded-lc-suggest]: https://github.com/uws-eresearch/plugin-LcSuggest

