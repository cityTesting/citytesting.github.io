---
layout: post
title: What is software testing?
categories: [testing]
---

**Software testing** is the process to check that the software matches the requirements. It can be divided into two categories:

* **Functional**: Ensure that the software does what is defined in the functional requirements. We can divide it in three layers:

  - **Unit**: Every component is tested isolated. It used to test methods/functions.
  
  - **Integration**: Ensure that the "communication" between two components works as expected.
  For example, if our product uses a database, the integration between them is checked with the integration tests.
  
  - **End-to-End**: The complete workflow of our application is tested here. These tests are the closest to the customer's use. 

* **Non-functional**: the tests of this category check different aspects of the product, performance, security, usability....



