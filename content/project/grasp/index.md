---
title: GRASP
summary: GRASP performs ancestral sequence reconstruction.<br><br>[Use GRASP now](http://grasp.scmb.uq.edu.au)<br><br>[Link to repository](http://github.com/bodenlab/grasp)


tags:
- Inference
date: "2016-04-27T00:00:00Z"

# Optional external URL for project (replaces project detail page).
external_link: http://grasp.scmb.uq.edu.au

image:
  caption: Photo by Toa Heftiba on Unsplash
  focal_point: Smart
---
## GRASP web service

GRASP web service was developed to facilitate the steps of performing a reconstruction of ancestor sequences (represented by partial-order graphs) and the exploration, archival and sharing of the output. The service consists of three major parts: an inference engine [bnkit](https://github.com/bodenlab/bnkit) written in Java, a web service backend written in Java using the Spring framework and Postgres, and web client functionality written in Javascript. The latter two are contained in the open source project [GRASP](https://github.com/bodenlab/GRASP). GRASP depends on the open source project [bnkit](https://github.com/bodenlab/bnkit).

### GRASP: What can it do?

The [GRASP web service](http://grasp.scmb.uq.edu.au) has a User's Guide that we recommend. You will find it in the menu at the top of the GRASP screen at all times.

### GRASP: How do I make it work on my computer?

Use any standard web browser and enter the URL [http://grasp.scmb.uq.edu.au](http://grasp.scmb.uq.edu.au). We recommend that you sign up for an account; with an account you will be able to use a lot of features that otherwise are unavailable.

### GraspCmd: How do I run it? 

Follow the User's Guide.

### What else?

There is a [command-line version]() to run reconstructions on your local hardware. This version does _not_ have all the features; for instance there is no interactive mode and currently no way of transferring your reconstruction to the web service.
