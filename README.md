DevTown WordPress Automation Scripts
====================================

What is it?
-----------
These are a set of scripts intended to make life easier when dealing with WordPress applications
in a continuous integration environment.  Tasks such as migrating data (including hostname munging)
and configuring for different environments are managed here.

How do I use it?
----------------

### Initial Setup
1. If you haven't started your WordPress app yet, download the latest WordPress release, and, in your local environment, configure things normally, including your local database environment.
2. Incorporate dt-wordpress-automation by running the following:

    `curl -L https://raw.github.com/developertown/dt-wordpress-automation/master/init | bash -s`
    This will initialize a local git repo for the project (if needed), setup some environment files and add this project as a submodule for future use

3. Configure the environments.  You'll have a newly created subdirectory in your project called _environments.  For each contained environment-specific config file, setup the appropriate database and hostname attributes.

### Keeping up-to-date
As this set of scripts evolves, you'll want to get the latest and greatest.  To do so, from within your project, run:

    `dt/update`

### Dumping the development database
From your development environment:

    `dt/dbdump`

### Dumping a database, and transforming the hostnames
A very common task is to move from the development environment to the ci environment, and need to adjust the hostnames in the database dump for embedded links in pages, etc.  Assuming you configured the _environments files correctly, you can do this:

    `dt/dbmigrate development ci`

That command will dump the database, and translate all instances of the development hostname to the ci hostname.  The db dump will be printed to the screen.  (Note that this task is most often performed automatically at DT on the build server)

### Importing a database
Tying everything together here -- you can import a database from stdin with the import script.  Again, assuming you configured the _environments files correctly, you can perform an operation like this:

    `dt/dbmigrate development ci | dt/dbimport ci`

That command will run the migrate script as above, and then import the result into the ci database instance.  (Note that this task is most often performed automatically at DT on the build server)
