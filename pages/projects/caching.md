---
permalink: /projects/caching.html
layout: project
title: Caching Analysis Data
shortname: caching
pagetype: project
image: logos/xcache.png
blurb: Cached-based placement of analysis datasets.
maturity: Deployed
maturity-note:
focus-area:
 - doma
 - osglhc
team:
 - bbockelm
 - sfiligoi
 - fkw888
 - osschar
 - ddavila0
---

### LHC

Significant portions of LHC analysis use the same datasets, running
over each dataset several times.  Hence, we can utilize cache-based approaches
as an opportunity to efficiency of CPU use (via reduced latency) and network
(reduce WAN traffic). We are investigating the use of regional caches to
store, on-demand, certain datasets.

In Southern California the UCSD CMS Tier-2 and Caltech CMS Tier-2
joined forces to create and maintain a regional cache, commonly referred as the
"CMS SoCal cache", that benefits all southern California CMS
researchers.

Later on ESnet approached the SoCal CMS group to integrate a caching server into
the SoCal Cache. The server is deployed on the ESnet PoP at Sunnyvale, CA. but it is
managed by UCSD via the PRP kubernetes cluster.

A recent study, led by ESnet, on the network savings of the SoCal cache, was carried
out by analyzing the XRootD monitoring records from the XCache servers. The results
showed a factor 3 reduction of network bandwidth over the analyzed period.

<br>
<div class="card" style="width: 40rem; margin: auto">
  <img class="card-img-top" style="object-fit: contain"  src="/assets/images/caching-esnet-reduction-ratio.png" alt="Network utilization savings">
  <div class="card-body">
   <h5 class="card-title">Network utilization savings</h5>
   <p class="card-text">Network utilization reduction ratio in terms of (a)number of accesses and (b) volume transferred.
   </p>
  </div>
</div>
<br>

The aforementioned study also demonstrated how the accesses to the cache are
evenly distributed among the different servers that conform the SoCal cache.

<br>
<div class="card" style="width: 40rem; margin: auto">
  <img class="card-img-top" style="object-fit: contain"  src="/assets/images/caching-esnet-hit_and_miss.png" alt="SoCal hits and misses">
  <div class="card-body">
   <h5 class="card-title">Misses(a) and Hits(b) distribution in SoCal cache</h5>
   <p class="card-text">The above shows the distribution of hits and misses among the servers that conform the SoCal cache.
   </p>
  </div>
</div>
<br>

We also engaged with CMS to have a monitoring page that shows the popularity of
the analyzed data, this helps us to consider changes in the namespace definition
for what we cache.

<br>
<div class="card" style="width: 40rem; margin: auto">
  <img class="card-img-top" style="object-fit: contain"  src="/assets/images/caching-data-popularity.png" alt="CMS data popularity">
  <div class="card-body">
   <h5 class="card-title">CMS data popularity</h5>
   <p class="card-text">The above shows the distribution of accesses in terms of volume of the CMS analysis tasks by data campaign.
   </p>
  </div>
</div>
<br>

### Open Science Data Federation (OSDF)

Similarly to the LHC experiments the OSG has deployed a set of caches and origins that serve both public and authenticated data from diverse experiments and individual
researchers. In the following image we can see the location of the different origins and caches conforming the federation


<br>
<div class="card" style="width: 40rem; margin: auto">
  <img class="card-img-top" style="object-fit: contain"  src="/assets/images/caching-osdf-map.png" alt="OSDF map">
  <div class="card-body">
   <h5 class="card-title">Open Science Data Federation</h5>
   <p class="card-text">Location of the different caches and origins within the OSDF.
   </p>
  </div>
</div>
<br>

<br>
For more information on how to joing the OSDF please visit the following [link](https://opensciencegrid.org/docs/data/stashcache/overview/)


### Monitoring improvements

<br>

During the past couple of years a significant amount of effor was dedicated to understand and improve the issues affecting the collection
of the XRootD monitoring data. A [first study: XrootD Monitoring Validation](https://zenodo.org/record/3981359#.YnWgRtPMK3c) done in order to understand the data loss,
found that the cause was a common UDP issue known as "UDP packet fragmentation".
The [second study: XRootD Monitoring Scale Validation](https://zenodo.org/record/4688624#.YnWhRNPMK3e) was carried out to find the limitations of the monitoring collector
when used at a a higher scale.
<br>

As a result of the first study mentioned above a new component called [The shoveler](https://github.com/opensciencegrid/xrootd-monitoring-shoveler) was introduced in the
monitoring infrastructure to prevent the data loss due to UDP packet fragmentation. As depicted in the next figure, this lightweight component uses a secure and reliable channel to communicate
the monitoring data from the XRootD servers to the central monitoring collector operated by OSG.
<br>

<br>
<div class="card" style="width: 40rem; margin: auto">
  <img class="card-img-top" style="object-fit: contain"  src="/assets/images/caching-shoveler.png" alt="Shoveler diagram">
  <div class="card-body">
   <h5 class="card-title">The shoveler</h5>
   <p class="card-text">The shoveler is deployed in between the XRootD server(s) and the XRootD Collector to ensure a reliable channel.
   </p>
  </div>
</div>
<br>


Finally, software improvements to the OSG collector have enabled us to start collecting and anlazying g-stream data, which is the XRootD monitoring stream
that includes cache specific events. In the figure below we can observe an example of the g-stream data being colleted form the caches in the OSDF.


<br>
<div class="card" style="width: 40rem; margin: auto">
  <img class="card-img-top" style="object-fit: contain"  src="/assets/images/caching-gstream.png" alt="GRACC g-stream">
  <div class="card-body">
   <h5 class="card-title">XRootD g-stream monitoring data</h5>
   <p class="card-text">An example of the g-stream data collected from the caches in the OSDF.
   </p>
  </div>
</div>
<br>

## Repositories

Currently XCache is distributed by the OSG both in the form of RPM and docker images.
The following are the corresponding repositories where the base code can be found:

 * [github.com/opensciencegrid/xcache](https://github.com/opensciencegrid/xcache)
 * [github.com/opensciencegrid/docker-xcache](https://github.com/opensciencegrid/docker-xcache)


## Reports

 * [Report on cache usage on the WLCG and potential use cases and deployment scenarios for the US LHC facilities](https://github.com/iris-hep/iris-hep.github.io-source/blob/master/assets/pdf/Cache_Usage_on_the_WLCG.pdf)
 * [Report on LHC data access patterns, data uses, and intelligent caching approaches for the HL-LHC (draft)](https://github.com/iris-hep/iris-hep.github.io-source/blob/master/assets/pdf/LHC_Data_Access_Patterns_draft.pdf)
 * [XrootD Monitoring Validation](https://zenodo.org/record/3981359#.YnWgRtPMK3c)
 * [XRootD Monitoring Scale Validation](https://zenodo.org/record/4688624#.YnWhRNPMK3e)


## Documentation

 * [How to join the Open Science Data Federation](https://opensciencegrid.org/docs/data/stashcache/overview/)
