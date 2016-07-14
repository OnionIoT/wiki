# Using Git

[[_TOC_]]


[//]: # (What is Git)

## What is Git

Git is a popular [version control](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control) system that allows coders to track changes to their codebase, easily collaborate with others, and manage their project.

Git is the version control system used by [GitHub](https://github.com/), the super popular online web-based repository hosting service. The team at Onion uses GitHub exclusively for all of our version control needs. 


[//]: # (Using Git on the Omega)

## Using Git on the Omega

The Omega's operating system supports Git, allowing you to use the Omega like you would any other computer.

### Installing Git

Installation of Git just requires a few packages:
```
opkg update
opkg install git git-http
```


### Using Git

If you are completely new to git and GitHub, check out this short [bootcamp series](https://help.github.com/categories/bootcamp/).

There are a variety of [tutorials](http://git-scm.com/docs/gittutorial) available online for more info.

### Proposing a Change / Fix / Link direct on Github (Pull Request)

Navigate to the Document/Tutorial you would like to change. (https://github.com/OnionIoT/wiki)  You have to be loged in on Github. Click on the pencil in the document header. As you are not the owner of the onionIOT wiki you will be informed that it creates a Fork. In my case it means :

```
You’re editing a file in a project you don’t have write access to. Submitting a change to this file will write it to a new branch in your fork username/wiki, so you can send a pull request.
```

Make changes to the document, check for mistakes/tipos etc.

Undernieth of the document you have a "Propose file change" field where you can make some notes for the Onion Team. If they like your request and everithing seams to be ok they just kann fork it back and have in no time changed what you proposed.

**Keep in Mind!**
A good tutorial/howto is short and says in a few words/sentence what has to be done. Pictures help to clarify and dont forget to mention the Credits/Licences of others work.
