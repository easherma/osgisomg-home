---
title: "Crowd Sourcing For Planners"
date: 2018-01-18T11:08:26-06:00
draft: true
---

# Map-based Survey Tool

### Executive Summary

#### Purpose

* A survey tool was developed that consists of the following:

    * A client facing map-based survey tool

    * A backend to store submissions from the survey tool

    * Processing, cleaning, and editing of data was then done locally using GIS software

    * This processed data was then loaded into a local relational database, where it was combined with other datasets and aggregated to generate scores used in our Prioritization process

#### User Feedback

* RPCs were very happy to have direct input in the planning process

* They described it as empowering to see submissions on a map to know they were being considered

* We had a very high response rate and a keen interest in accuracy of submissions (we had lots of follow-ups from RPCs on the details of their submissions)

#### Future Recommendations & Other Considerations

* Model outputs will depend on data quality; improved coding for submissions (for example to allow 1:1 match with FWHA requirements as opposed to an open text field) and allowing for simplification/editing of features by the users would increase data quality.

* In general, the hypothesis is that longer segments will be more likely to have worse congestion scores, especially highway segments (data coverage for congestion is not absolute and greater congestion would tend to occur as measured on higher functional class roads). There are likely ways to weight this by total length or other statistical factors. Future analysis could involve running greater coverage of travel time data using the NPMRDS; additionally we could look for seasonal peaks in travel time data associated with harvest cycles.

* Incorporate scoring functions into the backend directly so that reports can be re-run on demand

* A visualization of the submitted data that is similar to the frontend web map that allows a superuser to view and edit all submissions in context would facilitate the process.

* Setting dynamic thresholds/budgets for mileage could influence submissions from RPC (vs an open-ended budget)

### Purpose

An intuitive, map-based tool was developed to collect input from planners and truck users to identify CRFC segments eligible for FAST ACT formula funding.

It’s functionally was developed with simple, easy to use features regardless of an individual's GIS domain knowledge or familiarity with spatial tools, which are becoming increasingly integrated into the planning and development process for any planning agencies.

The goal was to create an interface that could consistently and effectively collect input from a wide variety of stakeholders of varying degrees of complexity in terms of their requirements and technical familitary. This also served a larger purpose to create a process that could be replicated easily and iterated on for future improvement. The technical implementation is reflective of this goal.

This was based on preliminary research examining other open-source tools used for similar purposes. The focus on open-source tools allowed us to use a foundation to work from, allowing us to achieve greater functionality within the same time and budget. This also reduces uncertainty, as we not only can base our project on past projects with demonstrated success but also examine the flaws and shortcomings of other projects and identify pitfalls to avoid.

With that in mind, we split the project into two separate processes that correspond to the identification and then prioritization tasks of this project.

### Benefits over traditional Surveys

For the purposes of this study, there are several advantages to creating this style of survey input vs other, more traditional methods.

#### Instant Context

This approach provides an enriched information environment to show freight nodes, congestion and relationship to highway networks. This helps the user identify certain cause and effect conditions which might lead to congestion or safety issues. Being able to show relationships between congestion and terminal locations, or grain elevators on rural roads, for instance, can help the user more quickly make intuitive choices about which road segments should be prioritized.

#### Consistency

While we allowed for a large range of input in terms of descriptive text of a submission, this map-based survey tool allowed us to reach out to a wide variety of stakeholders at various times and have them all submit entries that generally conform to the same structure. This facilitates analysis and makes sure that stakeholder’s submissions are on as equal footing as possible.

#### Legibility

Similarly, allowing users to directly input entries into a digital, geospatial system insures that their ideas and intentions are communicated in an easy to understand way. Using hand drawn maps or other descriptors is time consuming, and leaves much to interpretation.

#### Ease of Analysis & Feedback

Using common geospatial data standards, we were able to quickly create representations of submissions that were quickly ready to be subjected to further analysis, and results are easy to share with all stakeholders and edit as needed. Storing results in a relational database also set us up easily for the ranking and prioritization phase.

#### High Response Rate & Engagement

An easy to use interface that immediately shows results on a map is a powerful indicator that feedback is valued and important. Accordingly, we enjoyed a very high response rate for all outreach efforts and had great engagement throughout our sessions. Many stakeholders also followed up with additional submissions and feedback.

## Specifications

After determining and testing basic functionality, the survey tool was developed and then loaded with relevant data layers that can be selected to provide additional context (see ‘Data Sources below).

### Map-based Survey Tool (Frontend)

**Tools used:** Leaflet, Javascript, jQuery HTML5, CSS

#### Features

Interactive ‘slippy’ map interface (a term referring to modern web maps that allows for zooming and panning).

![interface](images/blog/images/image_0.png)

On visit, user is greeted with a pop-up window that explains the context/purpose of the project and offers a brief tutorial.

##### ![image alt text](images/blog/images/image_1.png)Layers

A wide variety of relevant data sources were identified, processed, formatted, and stylized for use in this web map. Each layer can be turned on or off via the layer control, allowing for vital information and context to be displayed while the user is creating a submission.

##### ![image alt text](images/blog/images/image_2.png)Icons/Tools

The **Pencil Tool** highlights sections of roadway by dropping points and uses a routing engine to create lines along the roadway.

The **Marker Tool** allows user to drop down points to identify places of interests or other items.

User can click the ‘i’ icon to have this tutorial repeated.

##### ![image alt text](images/blog/images/image_3.png)Criteria Dropdown

If the user clicks on the ‘**CRFC Criteria**’ dropdown, they see some helpful information from the FHWA regarding the acceptable criteria for designating a CRFC.

##### ![image alt text](images/blog/images/image_4.png)Address Search

A basic address search was implemented using an ESRI geocoder. If a user types an address or place and selects a result, the map will drop a pin and zoom to that location.

##### ![image alt text](images/blog/images/image_5.png)Submittal Process

User must self-identify name, organization, and a contact email address in order to submit points

![image alt text](images/blog/images/image_6.png)

After drawing a route or placing a marker, the user can add a detailed text description to provide further detail and context.

User can submit as many points and lines in a single session as they’d like, but cannot edit or delete existing submissions.

![image alt text](images/blog/images/image_7.png)

User can click, drag, zoom around map and turn layers on and off. Existing submissions can also be shown using the layer control. Most layers, including the ‘Submissions’ layer, have additional context available as pop-ups if the user clicks on the geometry.

### Geospatial Submittal Database (Backend)

**Tools used:** Python, Django, Postgres, Postgis, Leaflet, Javascript, HTML5

The backend is where the data from the survey tool gets sent.

Unlike the Survey Tool, accessing the backend requires a user account and login in order to view, edit, or submit data.

The backend is where we receive the data, and can be used to edit submissions and manually create new ones that don’t follow existing roadways.

Additionally, there is the capacity for creating multiple user accounts with various levels of permission. For example, a super user might be able to see, edit, and delete all submissions, but a user with less permissions may only be able to edit their own submissions.

#### Submission Data Structure

![image alt text](images/blog/images/image_8.png)A submission object consists of a **name**,** org**, **email**,**description**, and **geometry**(either Point or LineString).

Each submission can be viewed individually as a detail. A list view is also available.

![image alt text](images/blog/images/image_9.png)

### Prioritization Process and Analysis

**Tools used:** Postgres, Postgis, QGIS, LibreOffice/Excel

The following diagram provides an overview of the workflow of the collection, processing, analysis, prioritization, and final output.

![image alt text](images/blog/images/image_10.png)

In order to save processing costs and avoid additional development time, some of the final analysis and editing was done locally. An advantage of using Open Source tools for this process is that the methods are identical; processes developed locally could be ported to the backend to allow for cloud-based functionality.

Using Open Source desktop software such as QGIS also allows for handling of more complex issues. For instance, we had at least one submission where the project submitted did not follow any existing roadway alignments. However, the road geometry was complex enough that the RPC needed to send plan drawings. Using software, we were able to geo-reference that image in order to enable easy digitizing and incorporation of this project into our prioritization ranking.

![image alt text](images/blog/images/image_11.png)

The submissions data was also cleaned and edited via generally the following two methods:

1. Corrections to geometry/dissolving line segments that appeared to represent the same area (i.e. total or mostly overlapping segments with identical descriptions.

2. Removal of duplicate entries (i.e. sometimes a user submitted the same section multiple times, in some cases I made submissions for users that then later went ahead and created their own new submissions anyways).

Future iterations would allow for a fleshed out backend that would allow a user to edit their own submissions. That, along with extra time for a follow up survey with each agency, would resolve most data quality issues.

## Deployment

The frontend operates as a static site. This means that all dynamic code is run client-side. Mainly for the sake of convenience, this was prototyped and then deployed using Github Pages.

For future iterations, it could be deployed on any static server (Amazon s3, etc) or even integrated into the backend to run as a single process.

The Backend is a dynamic server running a Django (a Python web development framework) project, deployed on an Amazon EC2 instance. For ease of testing and deployment, the project was developed using Docker.

## Maintenance/Future Deployment Considerations

As is, the frontend does not require authentication which theoretically allows anyone to submit data. This is a pretty low risk vector for the project since it isn’t published widely and the worst case scenario is that someone could spam the tool, but they can’t alter or delete existing submissions.

In subsequent iterations, the backend and frontend could be integrated more tightly, which would allow more stakeholders with varying degrees of technical skill to see and modify their submissions, and allow for varying levels of permission to view, edit, or create submissions.

Optionally, the frontend tool can pipe data to a turn-key services such as Carto or Mapbox. Submissions were, in fact, sent to Carto (which is a cloud-based geospatial database hosting service) as a redundant backup during development. However, the more complex analysis needed for ranking and prioritization would still require running a more complex series of SQL queries.

The code for all aspects was developed using version control, specifically Git. Code repositories are stored on GitHub.

A dump of the data from user submissions as well as the local Postgres database that they were imported into can also be made available.


# The Politics of Planning

# 26 is a Crowd

# Narrative

Federal funding has been made available for critical corridors in urban and rural areas. DOTs just need to select miles that meet criteria and submit to FHWA.

But where do you even begin to start identifying these miles? Illinois is a big state, and there is a limited budget of mileage assigned. How do you know what to choose if you are at a broad level of planning for the state? (IDOT historically has been an organization far more focused on the implementation and maintenance of specific roadway projects than overall planning).

My colleague [Libby Ogard](https://www.linkedin.com/in/libby-ogard-7657664/) approached me with this project. Her idea was that IDOT should engage directly with the 23 or so regional planning agencies that had more ground zero knowledge of their jurisdictions. This would treat into delicate ground politically, but we both agreed it would ultimately result in the best process and results for getting submissions.

The project is going to be looking at freight supply chain connectivity in rural areas of the state. The state DOT can identify specific corridors that would qualify for additional funding if they have deficiencies in connectivity, poor routing for freight trucks, etc. Mainly agriculture related, single mode of transport (truck).

### Challenges
* Robust and comprehensive participation from rural planning orgs and other institutions in identifying corridors
* Data deficiencies that prevent efficient analysis of the supply chain network
* Past efforts to survey planning groups resulted in poor engagement

### The Pitch

My pitch was to create a map-based survey tool; an interface that could consistently and effectively collect input from a wide variety of stakeholders of varying degrees of complexity using the intuitive and familiar interface of a web map.

Using this map as an interface allowed users to feel directly included in the decision-making process as they could literally see their submissions being accepted and included as we went through the process. Allowing users to see contextual information on a map and then create submission through the same interface helped ensure a high quality of submissions.

Users got very engaged in the process, and felt included.

This also served a larger purpose to create a process that could be replicated easily and iterated on for future improvement.




# Implementation

# Lessons Learned

Clients:
* [Illinois Soybean Associaton](http://www.ilsoy.org/)
* [Illinois Department of Transportation](http://www.idot.illinois.gov/)

Partners:
* [Prime Focus](http://www.primefocusllc.com/)
