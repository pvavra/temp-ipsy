# Plan your data management

Planning a research project can be at times overwhelming especially if it is your first time approaching such a task. 
While designing a project you need to consider many aspects concerning the data you would like to produce such as: which data are the most suitable for your research question, which amount of data you need etc. These are only a few essential aspects that you might consider, there are many more, but one fundamental aspect that we tend to underestimate is **how to manage efficiently our data**. Thinking ahead about the best way to manage our data can save us plenty of time in the future and drastically reduce our mistakes.

A good starting point to start thinking about data management is to use a **Data Management Plan.**

## What is a Data Management Plan (DMP)?

A data management plan is a research document, taylored on your project, that, as the name suggests, helps you to organize your current and future data. It simply consists of a questionnaire that guides you through some of the aspects of the data life cycle.

Despite its name, a DMP should be considered as a living document that you can update throughout your project. Keeping your DMP up to date allows you to record essential information relative to your data, such as sparse information and metadata that you might need in a later stage of the project (e.g. when writing a paper or a report for a funding agency). 

A DMP is increasingly required by funding agencies, since 2021 it is indeed mandatory to provide a DMP for all the [European grants](https://ec.europa.eu/info/funding-tenders/opportunities/docs/2021-2027/common/agr-contr/general-mga_horizon-euratom_en.pdf), here you can find a DMP template by the [ERC](https://erc.europa.eu/sites/default/files/document/file/ERC_info_document-Open_Research_Data_and_Data_Management_Plans.pdf).

## How to create a DMP

There are many options to create a DMP, you could write your DMP by following templates from funding agencies, or you could use ad hoc online tools such as:

- [Argos](https://argos.openaire.eu/home)
- [dmponline](https://dmponline.dcc.ac.uk/)
- [Data stewardship wizard](https://ds-wizard.org/)
- [rdmo project](https://rdmo.aip.de/)
  
At Ipsy we are developing a simple, interactive prompt tool, called **Quest**, that will help you to create and customize your own DMP.

## Why should I use a DMP?

As previously mentioned, a DMP is incresingly becoming mandatory to acquire fundings from German and European agencies, but besides this incentive it provides some other real advantages:

- It helps you to assess critically different aspects of your data that you might easily overlook.
- It helps you to think of your data organically.
- It forces you to create documentation and proper metadata at an early stage of your project.
- If properly updated during the project life, you can publish the DMP together with your data.

!!! note "Start writing documentation right now"
    Documenting your data only at the end of your project is not advisable. It forces you to rely on your fallible memory, on sparse information and it is extremely error prone and time consuming


## Planning your project at Ipsy

In order to help you manage your data, we have implemented a basic **research project workflow** that requires very little effort from your side. 

To adopt the project workflow we suggest you to use the **cluster** for data analysis and data storage. The project workflow consists of three simple steps:

1. Fill in a brief **questionnaire** that helps us to create some intial metadata for your project. Further information about the questionnaire can be found [here] 
[here]: ../../../cluster/cecile/data/#how-to-create-a-project-on-cecile

1. Once the questionnaire is properly filled in, we create a dedicated **project** which comes with a predefined folder structure (to have detailed information about the structure, please refer to the specific [project structure section].
[project structure section]: ../../../cluster/cecile/data/#project-structure

1. Once your raw data are moved into the predefined structure, your data need to be converted into the **BIDS** format (see [BIDS]) by using the tools available in the cluster.
[BIDS]: ../research-data-management/bids.md

## Data Version Control

One of the most important aspects of data management is to keep track of the evolution of your data. This task might be particularly error prone if done manually, it is, thus, advisable to use a systematic and automated approach.</b>  
As for software development also data can be tracked via **version control**. There are not many version control tools dedicated to data, the main one is [Datalad](https://www.datalad.org/), which in the last years has been heavily adopted in cognitive neuroscience research.</b>   
Datalad does not simply track the changes of your data, but also records their relationships. Beyond that, the datalad ecosystem includes other tools that can help you to make your science reproducible and collaborative.


The best way to start with Datalad is to go through the [Datala-handbook](https://handbook.datalad.org/en/latest/).
Dalalad is natively used through the command line, like [git](https://git-scm.com/), but for those who are less comfortable with the terminal there is also a [**GUI version**](http://docs.datalad.org/projects/gooey/en/latest/). 