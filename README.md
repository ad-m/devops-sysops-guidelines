# DevOps/SysOps Guidelines

## Follow standards

There are a set of standards to keep in mind:
    
* [The Twelve-Factor App]
    
    _Why:_ It ensures the functioning of the application as a service, which favors the abstraction of 
    their functioning and the distribution of responsibility, eg between different teams. 
    It also provides rules that eliminate the problem of differences between the production 
    and development environment.

* [Project Guidelines]

    _Why:_ 
    
* [Semantic Versiong]
* [Security 101 for SaaS startups]

## Issue Tracking

## Everything is code

* Store as many settings in the version control system as possible.

    _Why:_ Changing the configuration affects the operation of services in the same way 
    as changing its source code. Thus, there must remain a trace and the possibility of
    reversing all changes. This will also allow limiting people with access to the 
    production environment, as changes can be reported outside of it.
    
    _How:_ In the first step, familiarize with the configuration management systems eg. 
    [Ansible], [Salt], [Puppet], [Chef]. In the next step you will need to write tools 
    for exporting and applying settings.

* All custom tools that are present in the production environment should be versioned.

    _Why:_ Requirements are changing. Unnecessary scripts do not exist on production. 
    Therefore, if the script is needed, there will be changes in it, so there must be 
    a procedure for its installation, configuration and update, and the ability to track
    changes in all of them.

## Testing the code

Code testing means testing both the source code of the application, but also changes in the configuration 
of the environment described by the code.

## Monitoring

* Make sure the monitoring of backup process of critical data continuosly works.

    _Why:_ The status of the backup copy is a metric like the rest. Jump to the next point.

* Verify that the service metrics are specified, collected, and incorrect values trigger an alert.

    _Why:_ Services fail. We want to know about it as soon as possible, not just from users. We also do not want to lose any data.
    
    _How:_ You can collect data through an additional agent and your own scripts. [Zabbix] is an example of such a solution.
    You can alternative use the right time-series platform eg. [InfluxData TICK]. 

* Provide information about exceptions in the operation of the application.

    _Why:_ Errors happen. Users will only say about them when it is very bad, and  minor errors are also 
    onerous and should not occur.

    _How:_ You can use [Sentry], which allows you to monitor exceptions in a huge number of applications.

## Deploying the code

* The deployment of the application on the server should be possible under the 1st command.

    _Why:_ Too many commands, clicking in the graphical interface is prone to errors. Graphic or multi-command 
    instructions are a terrible waste of time and no automatic tests. No automatic tests means no tests, because 
    no one has time when everything works.
      
    _How:_ If the source code is available in the repository and application settings are also a code, it is
    enough to automate installation & apply settings. All you need to do is use the right configuration 
    management system.
    
* Developers should be able to create an environment identical to the production one at a time, at least locally.

    _Why:_ Debugging, debugging, debugging. The less people have access to production, the better.

    _How:_ Provide the appropriate virtual machines to the developers eg. by [Vagrant + provision script](https://www.vagrantup.com/docs/cli/provision.html).

* Limit debugging on production.

    _Why:_ Production environment is not suitable for testing.

    _How:_ Analyze the use of the SSH console and share the relevant data in a different way, eg metrics 
    of the monitoring system, entries in the log collecting system.    

## Disaster recovery

* Make sure that all processes are used to create the environment are documented.

  _Why:_ People are on one day, they are not on another day when [the bus hits them](https://en.wikipedia.org/wiki/Bus_factor). People may also want to switch off the cell 
  during holidays.

* Make sure all critical external services are exchangeable. 

  _Why:_ The companies are on one day and disappear on another day. The risk is excessive dependence on one supplier. If there is no competition for the service, 
  maybe you should keep it yourself out of the blocks? 

  _How:_ Identify external services that affect the production environment. Identify the competition. Make an attempt to integrate.
  
## Backup

* Make sure the organisation's critical data is backed-up in following of [Security 101 for SaaS startups] recommendation.

* Enter the mechanisms for automatic verification of backup.

    _Why:_ Data during transmission may change. The backup storage media may change. This also requires testing for degustation.
    
    _How:_ Use the implemented mechanisms of the backup system. If they are not available - you can always start 
    the virtual machine automatically, upload everything there and check (count checksums, etc.).

* Periodically manually verify the correctness of backup copies by creating a separate environment. 

    _Why:_ The implementation of an identical environment is a great opportunity to verify the completeness of 
    procedures, operation of installation scripts, availability of backup copies and measure the duration of 
    operations, etc.
    
    _How:_ Apply the organization's procedures. In times of virtualized infrastructure, you can buy everything for hours.

## Data protection

* Organizatively, define the narrowest scope of information that is protected (customer data, narrow implementation details).

    _Why:_ People are social beings. Administrators and developers also. They will say, for example, with mates over beer.
    It is necessary to realistically determine what to keep silent, in addition to access passwords.  In another case, the protection of technical 
    information is illusory.
    
    _How:_ Try to answer honestly "What information is really important and disclosing it will bring losses to the company?". 
    For example, salaries are not such information, as market rates are generally available.

## Security 
[Filesystem Hierarchy Standard]: http://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html
[The Twelve-Factor App]: https://12factor.net/
[Project Guidelines]: https://github.com/wearehive/project-guidelines
[Semantic Versiong]: https://semver.org/
[Security 101 for SaaS startups]: https://github.com/forter/security-101-for-saas-startups
[Sentry]: https://sentry.io/welcome/
[Chef]: https://www.chef.io/chef/
[Puppet]: https://puppet.com/
[Ansible]: https://www.ansible.com/
[SaltStack]: https://saltstack.com/
[Zabbix]: https://www.zabbix.com/
[InfluxData TICK]: https://www.influxdata.com/time-series-platform/
