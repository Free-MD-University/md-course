# explain theoritical priciple

## On Premise

what is on premise ?
who use it ?
why ?

## Serverless :

what is serverless arch ?
who use it ?
why ?

## cloud arch :

what is cloud arch ?
who use it ?
why ?

## hybride (serverless + cloud) arch :

what is cloud arch ?
who use it ?
why ?
explain what is a VPN

# Security

## IAM :

create a new instance.
give to this instance an alias
create a secure IAM user
connect to this IAM user using:

-   the instance account ID
-   the instance alias

## MFA:

put in place MFA auth. what is it?
what is virtual MFA / U2F MFa ?

# General AWS Service

## regions:

check what is the region you are .
define how a region is named (three part )

-   Region
-   Availabilty Zone : subnet in the same AZ will be connected together with fast BandWidth
-   subnet
    define a type of service with a region
    define a type of service without a region (global service)
    find a service available in a region and not in another
    which region recevie first new service : US-EAST
    how much Availabilty zone there is per region : 3

## Type Of Cloud computing

there is three type of computer calcul :
CPU > given by EC2
GPU > AWS Inf1
Quantic Computing > AWS BRACKET

## Benifit of cloud computing

Agility :
Pay As You Go :
Economy Scale:
Security:
Global access:
Availability:
Scability:
Elastibility :

## Global key-term

### Innovation Wave:

what is kondratiev tech ? what the actual wave ?
Expansion > Boom > Reccesion > Depression

### Burning Platform :

what is a burning platform : when a company shift tech to survive (on-premise) > (cloud computing)

### Fault domain

limiting in case of failure the faillire to the only device or system that fail . avoid cascade failure.
we can have fault domain in fault domain for exemple a server, a room of server a building ...
the CSP define the level of FAULT DOMAIN that is created
by default a region is a Fault Domain, inside AZ is a fault domain .

### AWS Global Network

the network that connect each data center. called "BackBone of AWS"
we can securize the transfert of data by not letting the data going to internet but by using the AGN .
we can create multiple CDN to be near of each end user .

### POP(point of presence):

there are intermediate location of the end user and the data. there are DC or Specific Hardware own by amazon
and partner to deliver in the fastest way file from internet
divised by two categories :

-   Edge Location : Hold Cache Copy of files to reduce the distance between End User and the Server
-   Region Edge Location: same of a Edge Location but between Region and are more larger

# hight Scability:

allow to define there is no point of failure and promise a certain amount of performance.

## Load Balancer :

define lot of server with same code the Load balancer is responsible to route data/request only to
working server .

## Scaling Up :

upgrade for more core and hardware in the same computer

## Scaling horizontal:

increase the number of machine with the same size.

you can use Auto Scaling Group to remove or add server according to rule that you defined .
this is a automatic Scaling Horizontal

## BCP :

define how to recover/continue from a disater .

### RPO : Recovery Point Objective

the maximum amount of data allowed to loss

### RTO: Recovery Time Objective

the maximum amount of time allowed to be down

## DR : Disaster recovery:

define what we have if occur a disaster . how much time we need to recover full backup ?

# billing

be careful with default options (exemple with redis )
with default options of redis say the price by hour do you really need all the ad ons ?
say the price for a month
check the doreferance of pricing beetween region

## Budget

GO TO BILLING DASHBOARD
how much cost to make a budget
create a 10 dollars budget monthly
explain difference between monthly and daily budget
how much it cost for a dayli budget
create an alert for when you reach 80 and 100 % by email

## Consilidated Billing

AWS Org feature to merge all billing in a big one

##

### Free Tiers:

explain what is free tiers, define alert on free tier usage limit reached (in preference)

### Cloud Watch

create a cloud watch alert by mail when the total cost exceed 1 dollars

# Services :

## AWS Direct CONNECT:

create a direct private/deicated connection from your work/DATACENTER to your AWS data
2 connection options:

-   lower
-   hight

### Direct Connect Locaiton:

trusted partner datacenter create direct connection from your on premise to your AWS

### AWS CHINA:

AWS CHINA is isolated from other region . need to have chinese license, not all service is available .

### MAP :

-   Local Zone : need aws ticket to access to it, close to population area, only few service are available
-   wavelength zone : zone created for 5g network, more used for mobile phone user .

# AWS ARN:

amazon ressource name identofy each AWS ressource by unique ID there is serveral variation .

# AWS SDK

allow to manage AWS ressource from a lot of computer language .

# AWS CLI :

CLI shell to interact with aws
there is all lot of parameter for each services .

## AWS CLI Credential :

all credential of AWS is stored in ~/.aws/credential

# AWS API:

aws use HTTP Api to make everything we make from the web app

# AWS Management console

unified ui to manage AWS services (not a CMD console

## Service Console:

Specific Ui for a service .

# AWS CloudShell:

web based web shell. defined by region .

# AWS Cloudformation

allow to create a IAC Stack. a config file to create ressource in a automatic way.

## CDK

allow to create IAC in a code language that you want

# AWS Tools for Powershell

AWS Powershell CMDLET let us manipulate AWS from posershell

# AWS OutPost :

create a phisical server in your data center

### data residency

the geographic location of where the data is

-   Compliance Boundaries : legal requirement that describe where data and cloud resources are allowed to be
-   Data Soverainty : juridical control where data is

# AWS Config :

let configure policy and data configuration/alert to define data location or some particular specification that you have

# AWS Ground Station:

let you control satellite communication, process data and scale operation . for exemple can be used to speak with a satellite.

# AWS BRACKET

allow to create computer auqntic task
the creation of a quantic simulation is included in the free tiers but not access to the hardware

# RDS Multi-ZA:

allow to run multiple database zone that is synchronized the second server is used when the primary DB Failed

# AWS Cloud Formation :

allow to create a an IAC . can used describe language (JSON / YAML ) or scripted language (JS, Python, Ruby)

# AWS EC2 :

computing service that create a VM. each VM is an instance . it's hightly configurable (CPU, RAM, OS) some AMI (AMAZON MACHINE IMAGE ) are VM preconfigured by default.
is the Backcone of AWS because other Service use EC2 to create the machine (RDS, Redis ...).

to create the machine you need to :

-   define the OS
-   the instance type (ram, cpu ...)
-   storage (EBS, EFS)

## Pricing Model:

-   on-demand:
-   Spot: 90% less expansive but can have interruption . non-critical BG task
-   Reserved: 75% less expansive but need to create a contract between 1 / 3 year long term pricing model
-   dedicated: most expensive but have your own hardware .

### Saving plan:

a saving plan is like a reserved pricing model but in more easy we will have (1 / 3 year contract), pay upfront / partial upfront
or hourly like the reserved pricing model.

there is 3 saving plan in AWS allow you to make reduction cost :

-   compute : apply AWS lambda + EC2 and Fargate allow to save 66% no defined by AZ/region or family
-   EC2 : reduce the cost of the instance familly no defined by AZ/region give possiblity to change between familly ...
-   SageMaker : AI service to reduce usage

## instance Template :

allow to create a template of a machine, each machine will launch the same template (same app or server / file).

## auto scaling:

auto scaling group allow to scaling my number of machine of a particular template according to some variable / conditions.
you can put a load balancer directly to a scaling group.

## ALB:

create a load balancer between machine .
there is 4 type fo load balancer :

-   Classic:
-   Gateway:
-   Network:
-   Application: allow to define a _target group_ as a target of the load balancer

## EC2 Host Type

### dedicated Host

allow you to have your own computer in AWS. like a VPS, you need to handle everything . you have access on machine carectistique .

### dedicated Instance:

your server is always on the same machine.

### default:

your instance is regenerated at each reboot, the place where live the machine can change after reboot.

# Instance :

## Instance Family (not specific to AWS but I put it HERE )

define the ressource you need for your computerized process. you have multiple category :

-   general Instance : A*, T*, M\*
-   Compute Optimized : C\*
-   Memory Optimized : R*, X*, z\*
-   Accelerated Optimized : P*, G* ...
-   Storage Optimized : I*, D*

## Instance Type:

define the size and install familly of a service .

# AWS ECS :

allow to create a container config (Docker). you have to run only the docker config and define ressource usage AWS handle the rest (OS, Image ...)

# AWS Kuberbetes

allow to create a Kubernetes config (Docker). you have to run only the K8s config and define ressource usage AWS handle the rest (OS, Image ...)

# Storage Type:

there is 3 type of storage type :

## EBS :

data in block, have lock on write by volume , like a Hard Drive for the OS.

the section is down from EC2

## AWS EFS :

file system in network wors as a NAS multiple read write lock by file.

## AWS S3:

S3 storage no lock, no limit.
the storage is unlimited each bject (file ) have :

-   key : name
-   value: the data in bytes
-   Version Id: the version of the object (need to enble versioning)
-   Metadata : the metadata of the objhect

### S3 Bucket :

a bucket is like a HD to store object. bucet can have folder that have object too .
file an go from 0byte to 5TB

### Storage Class:

there is different storage class that change the durability, accessible time and accessibility of an object :

-   Default : Fast, replicated at least in 3 AZ.
-   Inteligent tiering : use ML to determine the staorage class to use
-   IA : infrequent Acess : cheper but can have fee if you see the file too often
-   One-Zone-Ai : only one AZ have the data. retrieval fee is applied
-   Glacier : very cheap long term storage can take hour to get the data
-   deep glacier : can take 12 hour to retrieve the data very very cheap

### S3 lifecycle

in bucket you can declare lifecycle, this is action to do in object (like delete or move in deep storage) when not used or according to some condition

## Snow familly :

phisical device to move data in and out of the cloud

# DB :

a DB is data grouped in column/row (relational) or in document (non-relational)

## Data Warehouse

relational datastore designed for analytic. column oriented DB . make aggregate more fast get data from other DB on regular basis
the AWS DW is REDSHIFT

## Document Store:

a no-sql database document are stored in XML or JSON-like format

### Dynamo DB

the NoSQL DB of AWS (no needs to manage shards)

### RDS:

relational Database, easy to use AWS support : Mysql, MariaDB, PQL, AURORA, MSQL and Oracle.

# Network Service:

## Cloud Service

AWS Account > Region > VPC

to connect a service to internet you need a IGW (Internet Gateway Service) this IGW need to be connected to a router (with route table).
the route table create a nat with the VPC to give access to the services . Betwwen the router and the VPC there is ACL (Firewall)

## Hybride Service

to connect a private cloud to AWS (Hybride) we can use :

### VPN : Virtual Private Network

connect a on-premise network to AWS with a secure direct line

### DirectConnect :

dedicated GIGABIT connection from your server to AWS

## VPC :

a virtaul private cloud with you're range of IP you want you can choose the CIDR Range you want to use.
you can create multiple private / public subnet. public subnet have access to internet .

## NACL :

is firewall rule of a subnet. each rule can be an allow or deny rule.

## Security Group :

is firewall of an instance (EC2, eks ...). by default every route is denied and you can create allow rules (not deny so you can't deny one IP address) .

## EIP:

elastic IP, each IP default of a EC2 or other instance are no static the ip changed everytime the instance reboot .
EIP allow us to define a static IP that will never change (for a domain for exemple).

# CloudFront.

allow cache data you need to define the origin (what to cache, from where you want to cache data ),
the method to cache the data,
invalidation rules (not to cache)..

# Zero Trust Model:

don't trust anyone. create your own trust model .

-   based your AIM on IP, MFA, TIME,

## CloudTrail:

allow to check each API called

## GuardDuty:

allow to detect maliscious based on log / cloud trail

## Detective :

analyse security issue

# Direcotry servive :

map network ressource to network address, is shared information structure of the company allowing to manage, admin and locating ressources .
we can use Azure Directory as a directory service .

# IAM :

an IAM allow to handle user permission, user group and the ressource permission .

## Identities:

there is 3 type of identity :

### User :

define a user

### Group:

define a group of user .

### Roles:

define specific permission to an API action .

## IAM Policy:

JSON doc that define permission for a group or a user .

{
"Version": "2012-10-17",
"Statement": [ // STATEMENT ARE DEFINITION OF A RULE
{
"Sid": "DenyAllOutsideRequestedRegions", // LABEL NOT MANDATORY
"Effect": "Deny", // THE POLICY
"Principal": "", ?? THE AWS ACCOUNT / AIM / ROLE THAT IS DEFINED TO THIS POLICY NOT MANDATORY _ PER DEFAULT  
 "NotAction": [ //ACTION OR NOTACTION WHAT ARE INCLUDED OR EXCLUDED OF THIS POLICY
"cloudfront:_",
"iam:_",
"organizations:_",
"route53:_",
"support:_"
],
"Resource": "\*", // WHAT PARICULAR RESSOURCE IS INCLUDE IN THIS POLICY
"Condition": {
"StringNotEquals": {
"aws:RequestedRegion": [
"eu-central-1",
"eu-west-1",
"eu-west-2",
"eu-west-3"
]
}
}
}
]
}

## POLP : Principe of least privilege

let the user a minimal amount of time / permission to do what he wants :

JIT : Just In Time: allow to let him only amount of time to do it
JEA: Just Enought Acces : just enought access

## IAM Permission :

API Action hant can or cannot be performed

## Root account.

the main account of AWS he have some permission that no user can have (delete AWS account, restore AIM permission ...)

## AWS SSO

allow to control access from your AWS org . allow multiple type of sign in service like OAuth2, JWEB ...

# Queue (PUB/SUB)

## SQS :

simple queue service, decuple and scale microservice

## Amazon MQ / Amazon Kafka Service (AKS):

managed broker service from MQ or Kafka

## Streaming :

queue consumer that can react to queue. like AMAZON KINESIS .

## Simple Notification Service :

allow to sub to consumer, handle/sort & filtering data to launch it to consumer

## State machine :

allow to create a workl=flow of machine according to some condition. these conditions are defined by **AWS Step Function**

## Event Bus / EventBridge:

receiver of event that send it to a consumer according to rules .
EventBridge is serverless not to define the machine the ressource are defined according to the uze .

# Amazon AppSync :

graphQL service .

# AWS organizations:

allow to create multiple new AWS Account to manage billing, access, services and users.
AWS account can be divised in OU (organizations units) that are group of aws accounts. OU can be inside other OU.
to manage permiussions, we are using Service Control Policy that are permission for all account .

# AWS Control Tower:

is the service allowing organizations to create AWS multi-account.
provide base for create a multi aws account with best pratice :

## Landing zone:

handle that your service use best pratice .

## Account Factory :

automate the creation of new account in the org, define default permission and service to use.

## guardtrail :

pre-package security rules .

# AWS Config

AWS Config is a cac (Compliance as Code ) that allow to automatate the monitoring enforcing and remediate to change .
AWS is region basis

# AWS QuickStart :

allow to use template in cloudformation for specific need .

# Tagging:

a tag is a key/value pair that we can assign to a ressource .

## Ressource Group :

allow to manage ressource with tags

# Buisness Service:

## Amazon connect :

call service

## Workspace :

create windows / linux desktop in minute

## WorkDocs :

like google sheet or microsoft 365

## chime :

video presentation service like zoom

## WorkMail:

like gmail

## PinPoint :

marketing service. allow send campaign by SMS/Email ...

## QuickSigth :

like powerBi

# Elastic Benstalk :

is the heroku equivalent of AWS allowing to deploy webapp in second .
can deploy loadbalancer, code, S3 DB in seconds .

# AWS License Manager:

with AWS you can BYOL (Bring your own license) like a windows license for Windows EC2 Machine .

# AI & ML & DL:

## Amazon SageMaker :

create, train and deploy AI machine .
amazon sagemaker Ground Thruth is data labeling service to train the AI.
amazon sagemaker can use Amazon Augmented AI that if the AI don't sure so the question is queued for human verification.

## Amazon CodeGuru:

ML code review analyzer .

## Amazon Lex :

conversion interface, voice and chat textbot.

## Amazon Personalize:

real time recomendation

## Amazon rekognition:

image & video recognisation

## Amazon Transcribe:

spech to text service

## Amazon Polly:

text to speech service

## Amazon Texttrack :

from paper to digital data .

## Amazon Translate:

Translate service .

## Amazon Comprehended:

prediction according to data.

## Docker & AMI Image.

Amazon have some preinstalled image with ML frameworks for EC2 or docker .

# Big Data :

Amazon have a lot of service for big data .

# AWS Well-Architected frameworks:

is a whitepaper that divided best pratice of AWS in 5 pillar.
each pillar have his own whitepaper .
a pillar is composed from :

-   Design Principe :
-   Definition : categories of best pratices
-   Best pratice : best pratice to use
-   Ressources : Additional content

## Operational Exelent Pillar :

Run & Monitor System

## Security Pillar:

Protect Data And System

## Reliatbility Pillar :

Migrate and recover from disaster

## performance Pillar :

performance pillar

## Cost Optimization Pillar :

cost optimization for the company

# Cost Organization

# AWS trusted Advisor :

create some advisor according to your org some advise are free some not
