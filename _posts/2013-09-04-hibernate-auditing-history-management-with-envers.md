---
layout: post
title:  "Hibernate Auditing/ History Management with Envers"
date:   2013-09-04 09:00:13
categories: general
permalink: archivers/hibernate-auditing-history-management-with-envers
---
Recently in our project we had requirement of keeping the history/ audit records of a group of related tables. While I was googling to find a good solution and design for the problem I came across Hibernate Envers.

Hibernate Envers is like a revision management solution for you database entities, it will maintain a snapshot of state of various tables whenever the transaction is committed. But yes you can use it only in case you already have hibernate configured in your application, and you will be able to audit only those tables which you have configured to be persisted by hibernate. Setting up auditing using Envers does not requires any coding effort, just few very simple steps of configuration:

1. Add hibernate-envers jar to your application.
2. Add annotation @Audited to all the domain classes you want to be audited.

Envers will by default generate following schema:

1. Revinfo Table - The table will store the the revision number for the transaction and the timestamp for it.
2. For each table with name TableName configured for auditing envers will generate corresponding table TableNameAUD._  It will have all columns as in the audited table and 2 additional columns:                  
3. rev - this will be revision number in the REVINFO table
4. revtype - this will be a numeric value 0, 1 or 2 for Insert, Update or Delete respectively.

Thus whenever you commit any transaction which changes any table configured to be audited a record will go into REVINFO table for the new revision number and data of table configured to be audited will be captured and stored in the corresponding auditing table.

Much simple is it not. But yes envers does provides a couple of more detailed configurations and customization options for details check : [Hibernate Envers Documentation](http://docs.jboss.org/hibernate/envers/3.6/reference/en-US/html_single/)
