# A Simple Rules Engine

For one of my project, I needed a simple rules engine. 

I like the [DecisionDag](https://github.com/mandarjog/decisionDag) that [Mandar Jog](https://github.com/mandarjog) built. 
It uses [Commons JEXL](https://commons.apache.org/proper/commons-jexl/reference/syntax.html) and allows rules to be defined in plain english. 


In my use case, I had following goals.
* keep it simple (KISS)
* rules engine as micro-service (deployable to cloud).
* a rules catalog of sorts that allowed new versions of the rules to be added and older ones removed.
* ability to calculate within the rule and return results.

This is a simple rules engine built with spring-boot in java. 

The rules are in plain javascript. 

Nashorn sccript engine allows runtime loading and evaluation of rules. 


### Instructions

* Install the app by cloning the repository [rulesengine](https://github.com/akoranne/rulesengine.git)

* Build and run the app
  ```
     $ cd rulesengine
     $ gradlew bootRun
  ```

* Call rest end-points.
  ```
     $ curl -v 'http://localhost:8080/api/rules/WhatToDo?family_visiting=yes'
     
     $ curl 'http://localhost:8080/api/rules/WhatToDo?family_visiting=no&money=poor&weather=good'
     
     $ curl 'http://localhost:8080/api/rules/WhatToDo?family_visiting=no&money=poor&weather=cold'
     
     $ curl 'http://localhost:8080/api/rules/WhatToDo?family_visiting=no&money=rich&weather=cold'

  ```

### Cloud

[Meet PCF Dev](https://blog.pivotal.io/pivotal-cloud-foundry/products/meet-pcf-dev-your-ticket-to-running-cloud-foundry-locally). It is a simplifed, and minimized version of the Pivotal Cloud Foundry intended for your local machine. And [Getting started](https://pivotal.io/platform/pcf-tutorials/getting-started-with-pivotal-cloud-foundry-dev/introduction) is simple.


#####Deploy to cloud

* Target the cloud instance
  ```
     $ cf login -a api.local.pcfdev.io --skip-ssl-validation

     API endpoint:  api.local.pcfdev.io   
     Email>     admin
     Password>  admin

  ```

* Build and deploy to cloud
  ```
     $ cd rulesengine
	 $ ./gradlew assemble
     $ cf push -f manifest.xml
  ```

* Test the cloud service
  ```
     $ curl -v 'http://simplerules.local.pcfdev.io/api/rules/WhatToDo?family_visiting=yes'
     
     $ curl 'http://simplerules.local.pcfdev.io/api/rules/WhatToDo?family_visiting=no&money=poor&weather=good'
     
     $ curl 'http://simplerules.local.pcfdev.io/api/rules/WhatToDo?family_visiting=no&money=poor&weather=cold'
     
     $ curl 'http://simplerules.local.pcfdev.io/api/rules/WhatToDo?family_visiting=no&money=rich&weather=cold'

  ```
