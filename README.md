# CustomerCentricEngineering
# Sustenance Engineering
# Customer Success
# Continous Product delivery 
Are the various names by which this Sustenance Engineering team goes by and 

This repo contains general guidelines for the sustenance of any software product by experience.


# So , You just landed as a lead Engineer  in customer centric/customer success/Sustenance / Continuous process integration (CPD) team !!!

# Congratulations !!
Now you get to be a lawyer, doctor, Artist( Creative) , Cop, Product Manager/Troubleshooter/ Negotiator and finally an Engineer ( hmm not a regular engineer but a # Reverse engineer) .

Do not believe me , Read on ! I promise ; you will either believe me or you will get something useful like this document serve as checklist/todo list in your job .
Read till end as I have kept climax for last , where in you may think it is straight from "Release It!: Design and Deploy Production-Ready Software" book by Michael T. Nygard
but trust me I was picturizing myself while reading this this book. In one form or the other experienced and resolved most of the issues described in this book. 

So , Let's begin ...
I have these ten commandments for you to get started with your job and sail smooth 😊

# 1)	Onboarding: ( Divide and Conquer is your friend here )
# 2)	Case Handling :  Create or Follow the process ( this is where you get to play  lawyer)
# 3)	Symptoms: ( This is where you get to play doctor) Running book/play book.
# 4)	Communication Channel: 
# 5)	Testing and Monitoring: 
# 6)	Team Engagement and growth
# 7)	Hackathon/ Brown Bag sessions/Dev fests/ Tests fests/Field Ramp up sessions
# 8)	Tools
# 9)	Release Vehicle
# 10)	Documentation / Credentials management/Setup safeguard ( Guard your fort , once you have built it) 


# On-Boarding: 
       Get a big picture of the product and define the scope of delivery with respective identified stake holders.
Identify the set of components and sub-components in your product. Once the components are identified then assign each subset of components to each team member based on their skills. 
Prepare skill matrix of the team members and map them against the skills required for the delivery.
If you are the first one in team then hire team members based on the listed skills and experience levels.
**Arrange frequent brown bag / reverse KT / walk through sessions among team members.**
So that team will have complete knowledge of the existing components and documents all these 
In a confluence page . 
Each component age my follow this template  intro , architecture , skills , main modules , other components to which talks , schema diagram , APIs  , Test cases , code base ( git repo urls) , set up details , debugging details etc. Point of contacts etc.
Some times you have get information from the original developer / team sometimes you won’t especially if it is old code base or if the original dev left , So you may have to figure out from the code and back track to components.  **This is where your reverse engineering skills can be helpful** . If you do not have skill then by working on this project you will become reverse engineering over period of time.

# 2) Case Handling :
Formally there will be a channel like salesforce, company websites for customers to raise support requests and these will be usually first looked by tech support/field  folks. 
Some special cases ( bugs, configuration anomalies, performance issues, security concerns , integration compatibility issues , dependency issues, Solution Architects / sales teams POC/pilot setup assistance Documentation editions , changes to knowledge base , Migration assistance , upgrade issues , migration issues  etc ) will most probably your teams responsibility to handle. 
Even some of the core engineering features has to run through your lenses ex: breaking down monolith to microservices ,  Re-architecting solution / design reviews , performance enhancement solutions , stability/resiliency enhancements like rate limiter/circuit breaker .

Now for all these stake holders to interact and provide solution and be accounted  we would need a channel like Bugzilla  or Jira or Servicenow . So establish one if not already there .

Informally we may  as well create a slack channel / internal social website / share point/ confluence page etc for field and other folks to interact with us.

So by now  we hav a formal and an informal channel to communicate and address issues.

Once the cases start coming we need to assign it to respective engineers. This is called triaging and is usually done during daily standup. One challenge you face during triaging is engineers are in different time zone , so be aware of load of engineers across timezones.
Rotating on-call support during weekend needs to be established ex: Pager duty . SLAs need to be defined/mapped based on what SLA product is sold ex:  Gold (1 day) , silver( 3 days) , bronze (5 days ) .

**Tip**: Post at leat one update per day for cases you own. This will avoid unnecessary calls with stake holders .

Apart from regular cases there will be some very special cases which we call Escalations. Which will be usually incoming hot and those will be typically outages . you will be expected to jump on call as soon as possible . 
YaY !! Do not be intimidated/demotivated by it unless you have more priority personal things to handle. This is an opportunity to shine because typically lot of things will be at stake and many stakeholders will be following the case.

**Tip**:  80% of the escalation/outage issues I have seen had fairly simple solutions. So , take a deep breath and go for it. I would like quote one such case where the solution was fairly simple.
           Ex: it was first noticed in one of the high profile customer that a table (OAuth2RefreshToken) was growing rapidly and 10 million rows were created over span of 15 days, each row contained about 1 KB of data . 10 million x 1KB = 10 GB of disk space. Due to this partition was full and application could perform any write operations and application was down from the end user perspective.
 Quick scan of the table came there was rouge OAuth2Client created by default to issue refresh token with TTL of 8 days and refresh tokens were piling up in the table. 
**Bonus tip**:  As a customer advocate / troubleshooter your first job is to restore service in case of outages so customer can  get back in business. So , the immediate task was to take snapshot/ stop service and clean up the table and create some space.
10 million records deletion will take days if we use Delete * from “OAuth2RefreshToken” where “clientID” = ‘rougeClient’; because this is a full table scan and delete  row by row with lock overhead.
 So , I had to go back to my basics of SQL delete command for any options to speed up , quickly concluded delete is not the way to go by trying to delete 1000 records , which took 5 to 10 minutes approx. “TRUNCATE”  command to rescue because it does by pages and deletes entire table contents
Tada !! We had restored the service with truncate command and within 90 minutes we were out of the woods. 
A KB was created for this https://kb.vmware.com/s/article/87609 , Do not worry I will only mentioning only publicly available data.
For to be providing quick resolution you need to know by heart some of the major commits and amendments to the source code / documentation. This is where you get to play advocate/lawyer , isn’t lawyer  just quotes articles numbers / amendment / previous case examples ?

# 3) Symptoms:
Keep an open eye for the symptoms in the problem description and the solution provided , document/hashtag your symptoms and solutions , causes , preventive actions.
RCACA: root cause analysis and corrective action , of ten you will be doing this exercise for various high severity cases documenting all of these is key because this is where you get to play a doctor because you will be bombarded with vague descriptions of problem symptoms , you would be typically questioning   back with standard set of questions like
a) since how many days this is happening 
b) what wrong did you or what changed
c) Any outages / network glitches 
d) Do you have any screenshots of the issue
e) what was the user doing at this time f) Have you seen this before
g) Did you apply patches / performed upgrade/ installed any new software from different vendor/Any recent solution/hot-fix applied  etc , you would require this information  this issue is the side effect of something .
Does this not look like a patient-Doctor conversation ? You  MUST have a running list of known symptoms and what things to look for in logs and /or customer setup . 
Typically you would create a diagnostic tool to gather this information if logs alone are not sufficient.
**Tip:**  when you have identified complete set of symptoms for a case try elimination approach to narrow down to a major contributing symptom and thereby identifying cause. Organize this data so that some of the AI(Artificial Intelligence) and  ML ( Machine Learning) algorithms can be applied and come out with some sort of QnA / chatbot system.


# 3.A) Running Book / Playbook:   This is a by-product of above three major points . Your team should have a journal of important activities / day to day task happened/ frequently used commands / best practices/ How tos
Ex: 
a)	How to take dbDump of different dbs in different OSes and all supported combinations of DB and OS.

b)	Best practices of creating test setup / automation pipelines etc.

c)	Case hand over plicies to other time zones . Rules and roster for the team .

d)	Known issues / limitations of the product / performance testing / security testing results go here .

e)	Frequent editions to these are required ex: contact information of team , point of contact other integration product team , even best practices some times change over time or turn out to be not a best practice.
# 4)  Communication Channels:  
Formaly field and other stake holders will interact with us using formal channels like Bugzilla , jira or email. If this is the only process for all cases irrespective of the problem’s severity or impact / minor or major/ simple or complex query then it becomes a process overhead. 
So we created a slack channel where the field and other stake holders can post their case in an informal manner and get quick solution or fix over the channel , if it is already known or provide references for field to proceed with for ex: a case with similar issue for other customer which was solved in earlier PR/ticket.
If the query posted is time consuming and requires detailed analysis or takes more time then we can field to raise a formal ticket.
The communication channel served as rate limiter to the engineering resources . Also it will serve as history /log of queries in one place and field/others can search.
In fact this turned out to be a crowd knowledge sourcing and announcement channel which was highly engaging . more than CPD engineers filed members started using it 
# 5) Testing and Monitoring : 
This is another role you have to play. You are expected to do all sorts of testing. Most importantly you will be doing  post release testing which means mostly non-functional tests like security , performance , stability , diagnostic test, combinatorial , integration testing , regression testing , compatibility testing ( ex: it is not possible/ time consuming to test with “every openLDAP flavor”  / “every Load Balancer” / “every cloud vendor” / “every browser”/ every mobile vedor/ very database vendor etc    before release. Only post release when customer tries some of these combinations and ran into issues then we should always be ready with minimal setup and just plug the changing/component like we might have tested with nginx , f5 load balancer prelease . now the customer has nsx-t or AWS LB . in this case nsx-t or AWS LB is our delta.
Cool thing is that you get to fix this as well . you may ask QA and respective core-engineering should be doing testing and fixing respectively instead of a separate. In my opinion QA and core engineering team are mostly lab  rats and they have their own charter and they mostly play in home ground and less exposure to field and they not need be!!
The non-functional testing/development/engineering works like bit of magic . you should be monitoring the product from to boost non-functional aspects.
 For ex: A minor configuration setting change can boost performance like increase thread pool size for an heavy weight component. There are  different layers at which performance tuning can be done a) application (by introducing caching) b) ORM c) DB d) network ( increase bandwidth) e) OS 

Same with security , there are various layers where security concerns can arise or exist . you must have complete diagram of the product handy and detailed architecture of the component under question.
Ex: If you are going to do DB performance tuning then first you should have monitored results in place. Then the Schema diagram / ER diagram / indices list / Expensive query list you must have them handy.
Bottleneck can be in any layer , so gather numbers from each layer and how much percentage each layer is contributing. For ex: UI / app layer is contributing 10% , response time for the api is contributing 50% , Db layer 20% , pull from associated DBs like ealsticsearch/AD/messaging 10%.
Always have at least one setup for each major possible combination of configurations.
 Ex: a) test setup with openldap. B) test setup with cluster with AD and DB as MYSQL + Elasticsearch.
Or have a automation pipeline which can spinup a required setup for you .
This will speed up your reproducing the issue with less efforts.

# 6) Team engagement and growth:
Have a charter in the core engineering sprint review meeting. Where in you provide primarily the numbers related to cases incoming vs resolved . rate of resolution (MTC( mean time to close, MTR mean time to respond) etc.
Number of RCACAs done and improvemnts done. Number of high profile cases /escalations etc.
This serves mainly as your deliverable and this helps you to contribute upcoming features. 
I would like to quote once such activity from my experience. It was decided to create new feature/API to pull the netbios information of a logged-in. The solution proposed get the netbios 
Information from DB and render it as api response . So it was three apis 1) existing public api  which can get main user information    2) a new private api which can get netbios information from DB 
3) a new public API which is wrapper of these two apis to get the complete required information.
The estimate was two sprint cycles ( 4 weeks) including dev / test/integration / release effrots.
When I saw  this in sprint review meeting immediately quoted that there is already a AD ldap attribute “MSDS-PrincipalName” which brings in this information , we just have to make documentation changes to include this user attribute definition in the UI . After that the original public api (SCIM/me) can itself used no need to write new code. This is just turned out to be one day effort and same was incorporated  as you can see in this link https://docs.vmware.com/en/VMware-Horizon-Cloud-Service/services/hzncloudmsazure.admin15/GUID-DEC9A12A-D3BA-497A-A930-E1174F9E26D2.html 

So the point is if we did not have kept tab on the product new features and not engaging then this would have become overengineering!

# 7) Hackathons/Brown Bag sessions/Dev Fests / Test fests/ Field Engagements/Upskilling
Honestly this job over the time becomes routine and less motivating and enginees with core “design and dev” mindset or more specifically with “Startup” mindset will be tempted to switch to other teams / get de-motivated etc.
So , it is necessary to have frequent hackathons/dev fests/ test fests/ knowledge sharing sessions.
Frequent interaction with other teams like creating POCs for ehancements etc. 
From my experience the outcome of these exercises are performance enhancement / process improvements / more stable product / more secure .
For ex: make faster synchronization of AD/openldap , improve security for DB connection , tools for managing and delivering patches / hotfixes . 
 So , if we did not have these  exercises frequently then this would have not been possible and engineering will be less motivated. Some of the inhouse monitoring tolls / log analysis tolls/chatbots / patch mgmt. tools would not have seen light.

# 8) TOOLS: 
Know your required tools for the job very well. Knowledge and application of tools is what differentiates you from other engineering teams. Your toolkit typically would typicaly consist of Thread dump collection and analysis tool ( https://fastthread.io  ) , heap dump analysis ( Eclipse MAT) , DB dump analysis tools , Garbage collection tools ( https://gceasy.io ) 
  **Tip**:  Enable gc , heapdump on crash for the jvm process . Enabling   GC will not have any overhead whereas enabling heapdump would require free diskspace of heap size or more . DBdump will be helpful to run large queries offline and perform some analysis on the query or query optimization techniques without impacting prod/pre-prd setups. We typically used DBdump to reproduce issues faster and in an isolated fashion.
TCPDUMP : This is a vital tool and a must learn/know in this field. Gathering tcpdump and analyzing it with wire shark provide better insights of the end to end communication. By analyzing several tcpdumps you will identify a pattern what are your usual suspects. This is where you play cop/detective in some fashion. 
From my experience for most network problems the pattern or the most culprit turned out to be the some configuration change / misconfiguration in load balancer. 
Speaking about load balancer I would like a major major incident because of which our team was grounded in client location ( Mumbai city)  for an extra 4 days . The plan was initially we will be in and out of woods in 3 days and return flight tickets were booked accordingly and hotel reservation was only for 3 days .
We had our in-house representative at the client he had installed all the required software and our job was to configure / tie up these installed software based on recommended steps and perform basic testing , have the system table for a day and get client’s signoff and packup. First day for configuring and testing and troubleshooting . 2) Dry run and monitor for stability. “Troubleshoot if any” we were not expecting any . then gather stability/performance numbers , address any queries customer has and get sign off . 3) buffer day for any follow up if required.
But like they say “man proposes god disposes” there was one problem identified on the second day during testing . Sporadically app launch used to fail , out of 10 launches of app 6 or more  used to fail.
This problem kept us grounded at client’s place for 5 more days. We were trying to troubleshoot from various angles , application side , db , messaging , code , load , network etc everything looked well at very high level. When we asked client any recent change in network / load balancer the answer as usual was NO.
The only unknown area or in other words which was not configured by us was  Load-balancer. So we gathered tcpdump and we could see that request packets were being dropped . we checked various standard parameters of the load balancer and they were all configured properly with recommended values. This deviated us from from LB and we were checking code , DB configuration  , integration code , basically we had all hands on deck and it started heating up inside the cleint’s office and outside client office as it was peak summer in Mumbai ( April month beginning ). Apart from load balancer we checked everything else. 
The loadbalancer/networking engineering  team was different and when we engaged them they flaged green for LB configuration and looks good . BTW the LB was NSX-T which is our product , so we could get quick assistance.
But I was insisting since requests/packets getting dropped according to tcpdump and it is a fact then it must be LB. So we made a list of timeout parameters instead changing one parameter and testing again . There it was one parameter with just “1” second as default timeout value and the parameter was “http keep-alive timeout” we increased the time to 30 seconds and Voila all the app launches were successful and working like charm.
But we were wondering as a real enduser (not as an engineer) is this for real 1 second is way too less to timeout by default in the  external internet world and is this for real. It should have been at least 10 seconds . 
The load-balancer(NSX-T) engineering  had their own reasons for keeping this value at 1 second default like to avoid request pileup etc and till date it is 1 second as can be seen in the  “http keep alive “ section of the link https://docs.vmware.com/en/VMware-Horizon-Cloud-Service/services/hzncloudmsazure.admin15/GUID-DEC9A12A-D3BA-497A-A930-E1174F9E26D2.html
Getting back to other tools is sometimes you would have a tool off the shelf to troubleshoot then that becomes an opportunity to create one.
There will be many such opportunities during your career , so be equipped with relevant skills and to learn something new on the feet and you get to be more creative.
There was an issue reported in our product that one of the feature change password is not working.
This was frequently seen in multiple customers , So as per official process tech support/filed goes on call with customer and do everything by book to troubleshoot from starting point of product installation checking other  configurational things not just related to change password , gather log bundles , screen shots , video recording etc .
So this was some sort of over kill and I thought of a tool which can only test the change password feature , which does only one thing provided username , oldpassword and new password and respective krb5.conf file it should change password or log appropriate error messages with this we can could easily narrow down / reduce everybody’s efforts.
Similarly there was another tool developed called java “LDAPSearch” with various options and custom debug messages. This was developed because we used to get frequent calls related to bind authentication / user pull/group pull is not working but the configuration is all done properly as per book or documentation . So we developed a small tool just containing these functionalities and customon log messages and with some customers we could even intall IDE like eclipse/intellij and and attach debugger and test it where in could identify point of failure .

The idea is that if there are no tools then you can treat this opportunity to unleash your creativity . the task would be generic and subset of product which solely help to address the issue . This will also help you to test your product in isolation and do not have to wait for entire setup.


# 9) Release vehicle:
You will be often to provide quick fixes / address immediate security issues remember recent log4j vulnerability ( 2021 ) / configuration issues / performance issues. Which cannot wait till official next release in case of on-prem , in case of cloud for next blue green switch.

You should have a proper channel to deliver it and maintain it, also to get information at which patch level customer is at. The source code need to be versioned /baselined accordingly ( GIT , Liquibase etc) 
In a continuous integration/coninous delivery (CI/CD)  system  employ these staging/pre-prod, blue-green deployments etc but this is usually take care by devops folks but sometimes you have to wear devops hat as well especially during fire-fighting. If it is on-prem then it becomes totally your job to create delivery mechanism and own it and maintain it. 
10)	Documentation / Credentials vault/Setup safeguarding :  whatever you do in this job document it in one for or other and do what is documented . this should be a repeating cycle and culture in the team which will be huge benefit over time.
Safegaurd your team/org’s setup information and have a credentials vault for test setup credentials and use it only withing your team . If you are loaning your setup for other team remember to provide different set of credentials.

Some highly sensitive customers like fedRamp / DOD ( Department of Defense )/banks would not like to share sensitive information in log bundles , they would either not share log bundle or they would sanitize log  files and share  . To make their sanitization job easier and deliver logs quickly , you can can have a custom log level which does not print sensitive data ( like hostname , ip address, URLS , client name , username , email etc ).

# 10)	Documentation / Credentials vault/Setup safeguarding :  
whatever you do in this job document it in one for or other and do what is documented . this should be a repeating cycle and culture in the team which will be huge benefit over time.
Safegaurd your team/org’s setup information and have a credentials vault for test setup credentials and use it only withing your team . If you are loaning your setup for other team remember to provide different set of credentials.

Some highly sensitive customers like fedRamp / DOD ( Department of Defense )/banks would not like to share sensitive information in log bundles , they would either not share log bundle or they would sanitize log  files and share  . To make their sanitization job easier and deliver logs quickly , you can can have a custom log level which does not print sensitive data ( like hostname , ip address, URLS , client name , username , email etc ).






 
   






 











