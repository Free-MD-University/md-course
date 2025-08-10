# SSDLC

during the devops life cycle dev the security was the last process . 
analyze a bug at the end is 15x costly that find him at the begining .

# DevSecOps Process :


before to test the devsecops of a solution . we need to undesrtand what we have doing a gap analisys, les process et le training des engenieurs.
- Risk Assetment : define secuirty objective
    - factor that motivate the threat factor 
    - rosk evaluation 
    - how much user will be affected 

- Threat Modeling : define what are the threat
    - STRIDE stands for Spoofing, Tampering, Repudiation, Information Disclosure, Denial Of Service, and Elevation/Escalation of Privilege
    - DREAD : Damage Potential, Reproducibility, Exploitability, Affected Users, and Discoverability
    - PASTA stands for Process for Attack Simulation and Threat Analysis
- Code Scanning: scan the code to search exploit 
    - white scan testing:
        - SAST:
            Static Application Security Testing, a white box testing method that directly analyses the source code.
        - SCA:
            Software Composition Analysis (SCA). SCA is used to scan dependencies for security vulnerabilities.
        - DAST:
            Dynamic Application Security Testing, a black-box testing method that finds vulnerabilities at runtime
- Security assetment :operations & maintenance 


# SAST 

## Semantic analysis : check for dangerous function

## Dataflow Analysis : interact with the all dataflow input from the user to the end 

## Control flow analysis : search for race condition 

## Structural analysis : specific code language analysis like type .. 

## Configuration Analysis: check the config


# DAST 

dast is a process to automate test of vulnerability .

there is two possiblity to launch a DSAT : 
## Manual DAST : 
an engineer launch manually the DAST . this allow to no disturb the dev process and launch at end time only 

## Auto DAST : 
runned in a CI CD Pipenline .

### ZAP 
zap is good tols for DAST. DAST allow us to scan file :

#### Mode 
in the top right corner there is the mode. is the mode is safe we can use the attacke tools

#### Spider 
in the tools section we can use spider that will crawl the entire site to create a map of all request / URL .
if the url need JS to be loaded like react or AJAX we can use AJAX spider (in red ).

#### Active Scan

in the tools > Active scan we can create a scan for vulnerability .
##### Scan Policy 
 Analyse -> Scan Policy Manager. allow us to define what to check like sql injection or mmand injection or other .

 #### Auth Context 

 we can allow zap to do a login to access to specific page of the app . 
 this is called a auth contect .
 1) create a zest script (sign of a record in the bar one before the end)
 2) the zest script is created > go in the firefox icon to create the script 

 #### ZAP API 

 we can import the zap API with open api doc.
 in the import OPenApi def . 