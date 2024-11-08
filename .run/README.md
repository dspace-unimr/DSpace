# Run configurations for Intellij IDEA

This directory contains run configurations to build and deploy DSpace
(backend) from within Intellij IDEA.

Most configurations are simply copy-pasted from the official
[Docker-README](../dspace/src/main/docker-compose/README.md).

It was necessary to use the 'shell script' type of run configuration
to enable grouping of related scripts in the IDE possible.
(Grouping run configs of differing kinds is not possible, afaik.)

An exception is the Maven-config for deploying the backend as an (almost)
self-contained JAR: The shell-script version will only work if your
OS' version of maven is high enough.
Since Intellij brings its own Maven which might be updated more frequently as,
e.g., Ubuntu's version, there's a run-config using the IDE's Maven to build
the JAR.

If this works as envisioned, it should be possible to simply clone the
repository and build, run and deploy the backend with only a few clicks and
now manuel intervention otherwise. It worked 'on my machine' ☺. If it
doesn't do on yours, notify me, please.

# Run backend - Docker only

This should start a running backend instance from your local branch:

1. Clone repository
2. Run ["To build DSpace images using code in your branch"](./To%20build%20DSpace%20images%20using%20code%20in%20your%20branch.run.xml)
3. Wait until everything is built and deployed.
4. Visit [localhost:8080/server](localhost:8080/server).
5. Create admin account by
   running ["Create an admin account"](./Create%20an%20admin%20account.run.xml)

# Run backend - Self contained JAR

This should start Postgresql-database and Solr in containers and then build,
deploy and run the backend app as an (otherwise) self-contained JAR.

It will attempt to install the app to `/opt/dspace`.

1. Clone repository
2. Create directory `/opt/dspace` and configure permissions.
3. Run *one* of the following:
    - ["Maven package (IDE's mvn)"](./Maven%20package%20(IDE's%20mvn).run.xml)
    - ["Maven package (Shell)"](./Maven%20package%20(Shell).run.xml)
4. Run ["Ant fresh_install"](./Ant%20fresh_install.run.xml)
5. Optionally, check database connection by running
   ["Initialize database"](./Initialize%20database.run.xml)
6. Visit [localhost:8080/server](localhost:8080/server).
7. Create admin account by
   running ["Create an admin account (JAR)"](./Create%20an%20admin%20account%20(JAR).run.xml)

# Configure remote repositories

I thought it made sense if we all agreed to a common setup and naming convention
for handling remote repositories.
So I added a run configuration that configures these remotes in the following
way:

| Repository        | Link                                   | Name              |
|-------------------|----------------------------------------|-------------------|
| DSpace official   | https://github.com/DSpace/DSpace       | `dspace-official` |
| DSpace UMR GitHub | https://github.com/dspace-unimr/DSpace | `dspace-unimr`    |
| DSpace UMR GitLab | (undisclosed) 😛                       | `origin`          |

## Scripts:

| Name                                                                               | Effect                                                           |
|------------------------------------------------------------------------------------|------------------------------------------------------------------|
| ["Add_remote_DSpace_official"](./Add_remote_DSpace_official.run.xml)               | Configures the official DSpace backend repository as a remote    | 
| ["Add_remote_unimr-dspace"](./Add_remote_unimr-dspace.run.xml)                     | Configures UMR's fork of of DSpace official as remote repository |
| ["Configure_remote_git_repositories"](./Configure_remote_git_repositories.run.xml) | Configures both the official and UMR's repositories as remotes   |
| ["Remove_remote_dspace-official"](Remove_remote_dspace-official.run.xml)           | Removes remote repository `dspace-official`                      |
| ["Remove_remote_unimr-dspace"](Remove_remote_unimr-dspace.run.xml)                 | Removes remote repository `dspace-unimr`                         |
| ["Fetch_all_remotes"](./Fetch_all_remotes.run.xml)                                 | *Fetches* commits from remote repositories                       |