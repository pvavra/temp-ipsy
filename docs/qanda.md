# Welcome to the Questions & Answers (Q&A)

This page contains a loose collection of questions and the subsequent discussion from our mattermost `Cecile` channel.

The purpose of this is to keep track of already discussed issues which have not yet made it into dedicated pages/sections in the main documentation.

The topics include both (1) how do things on our cluster, as well as (2) advice on how to do things in accordance with open science principles more generally.

## Where should I store manuals/guides/etc related to the hardware/software used to acquire the sourcedata?
Let's say that you want to store (for archival reasons) a manual which contains info why and especially how you needed to write a custom conversion script for your sourcedata-to-bids and where the non-standard data-format is described. Where should this be located inside the `project_folder`?

## Where do I place logs (e.g. from SLURM or from tools)

## Do I create a `results` folder or should everything go into `derivatives`?

## How should I track (e.g. with git) my code?
Options include: 
* have one git repo at the root of `code`
* have on git repo per subfolder of `code`, so one for `source2bids`, another one for `preprocessing` and a third one for `analysis_first_level`, etc.
* have one datalad repo at the root of the `project`.
