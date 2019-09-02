---
layout: post
title:  Creating visualisation for KADlytics 
date:   2018-09-12 
categories: ["Web Development"]
image: "/assets/images/2018-09-12-KADlytics/kadlytics-logo.svg"
---

---
"In engineering, designing complex systems is a huge task made up of large teams of design engineers managing hundreds of components, thousands of files and hundreds of thousands of dependencies. 

One of the main challenges within these projects is tracking the dependencies between files and monitoring the effect of change in the system. Design Structure Matrices (DSMs) have been used in systems engineering and project management to model the structure of complex systems as a dependency network; perform system analysis; and aid project planning and organisational design since the 1960s, but this is traditionally a laborious task resulting in outdated analysis. 

KADlytics creates a solution to automate DSM creation, enabling real time dependency tracking and analysis to alleviate these issues within engineering and beyond."

*Written by Miminal*

--- 

##### Benefit of dependency tracking:
---

![](/assets/images/2018-09-12-KADlytics/cadmodel.jpg)

Dependency tracking allows us to gain further insight into what files should be changed together, which would not have been obvious if two files were part of a different subsystem. For example, lets assume the parts highlighted in this CAD model are dependent on each other, but each has their own subsystem. Through observing only the subsystem we would have no any idea that parts of the different subsystems would need to be changed together.

---


##### Design Structure Matrix:
![](/assets/images/2018-09-12-KADlytics/A_sample_Design_Structure_Matrix_(DSM).png)

[https://en.wikipedia.org/wiki/Design_structure_matrix](https://en.wikipedia.org/wiki/Design_structure_matrix)

The next two figures uses the concept of this design structure matrix to look at the dependencies between files.

---

##### Design System Structure coloured by subsystem:

---
![](/assets/images/2018-09-12-KADlytics/subsystem.jpg)

The different colours indicate different subsystems in the project. ...

---
Before the AI is applied to project files, showing there is no obvious patterns in the matrix stucture.

##### After AI is applied:

---
![](/assets/images/2018-09-12-KADlytics/module.jpg)

Patterns appear.

---

The limitation with the matrix structure is that it is too hard to understand unless you spend a lot of time learning how to interpret the information. It was decided that KADlytics would primarily offer value to project managers, with this in mind, simplicity and easy interpretation was key to the next development phase.

My development time was used to create a ranking table for showing a rise or drop in importance ranking for every file in a project. This ranking calculation was not been developed during my time at Minimal, so I had to create artificial data and design a schema for a mock-up version of the table. With the help of Ant Design (A bootstrap-like library designed for React.js) I was able  quickly design a prototype that was showcased to potential customers of KADytics.


##### Ranking table:
![](/assets/images/2018-09-12-KADlytics/table.jpg)


Over time more mock-up features were added to the ranking table which included importance ranking over time for an individual file and improved visuals for displaying a file's most connected neighbours. 

##### Expanded table gives more insight into a file. 

![](/assets/images/2018-09-12-KADlytics/expanded1.jpg)

Ranking over time for an individual file (in this case, INTAKE PLENUM UPPER).

---


![](/assets/images/2018-09-12-KADlytics/expanded2.jpg)

File's most connected neighbours.

---

Near the end of my development I worked on backend development; Creating schemas, building a pipeline for inserting data into the MongoDB web app database, and passing the inserted data into the visualisations.

Developing KADlytics gave me commercial experience in developing for clients and experience with adjusting library patches / updates, React & Ant Design, and MongoDB (a noSQL database). 