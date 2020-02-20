Basics of RiotKit's structure
=============================

**Servers have a single role, for example:**
- S1: Productivity tools, mailboxes etc.
- S2: Primary backup server in USA, Los Angeles
- S3: Secondary backup server (replica) in Europe, France
- S4: Web pages and custom applications hosting

**The reasons behind single role per server is:**
- Easy to manage and to split accesses between people
- Technical things that can grow in time does not have impact on other services
- Different servers can be sub-managed or payed by other people or organizations
- In cases like files storage it's worth to have a second server that is a mirror copy of the first, but in other location
- Storage consuming applications can be running on a host with cheaper storage, cpu consuming applications on a hosting with cheaper cpu plans

### Everything in the repository (Infrastructure as a Code)

Applications configurations, encrypted credentials, user accounts,
firewall configuration, everything can be kept in a git version control
repository.

**Advantages:**
- Each people from the administrative collective see what others do
- There is a possibility to get back in time to a moment when something
  was working
- Server and application can be set up in 10 minutes from a template on
  a new server

### Technical split of repositories

Depending on the level of automation in your organization you may use
automatic deployment to the server, or not. We assume there you have it
\- it's better to explain more than less.

**GIT repositories:**
- project
- deployment

**Project** contains a configuration of the environment. The environment
contains applications eg. mail server, databases, some websites, various
applications written in various languages and technologies.

**Deployment** is a set of instructions, an additional automation how to
configure your server eg. create user accounts, configure firewall, what
is important - download, unpack and update the **Project**.
