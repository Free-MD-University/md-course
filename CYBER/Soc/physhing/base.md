the physhing allow to send mail to get info from a company or a personne. 

to understant physhing we need to have a good understanding of EMAIL PROTOCOL :


- POP3 

- IMAP 

- SMTP


the email format is international and is known as Internet Message Format (IMF).

each mail have headers : 
From - the sender's email address
Subject - the email's subject line
Date - the date when the email was sent
To - the recipient's email address
X-Originating-IP - The IP address of the email was sent from (this is known as an X-header)
Smtp.mailfrom/header.from - The domain the email was sent from (these headers are within Authentication-Results)
Reply-To (Return-Path) - This is the email address a reply email will be sent to instead of the From email address

the email have a body too : 

a body of an email can be a text or html. 

it can contains image (by passing an url) or a attachemnt . 


# Type 

there is multiple type of physhing than can be : 

spam : 
Phishing -  emails sent to a target(s) purporting to be from a trusted entity to lure individuals into providing sensitive information. 
Spear phishing - takes phishing a step further by targeting a specific individual(s) or organization seeking sensitive information.  
Whaling - is similar to spear phishing, but it's targeted specifically to C-Level high-position individuals (CEO, CFO, etc.), and the objective is the same. 
Smishing - takes phishing to mobile devices by targeting mobile users with specially crafted text messages. 
Vishing - is similar to smishing, but instead of using text messages for the social engineering attack, the attacks are based on voice calls. 

## Business Email Compromise

when you have access to en email emplyee and use this mail to send other mail to other employee it called Business Email Compromise. 

## TECHNIQUE :

URL shortening services : use a non real URL 

HTML to impersonate a legitimate brand: beatiful website 

Pixel tracking : track a mini pixel image to see if the sender read the email .

Link manipulation:

Credential harvesting: redirect to a fake website to get user connection data


# Analyzer

## HEADER ANALYSIS
https://toolbox.googleapps.com/apps/messageheader/analyzeheader

MXTOOLBOX (show spf and dmark header )

## SANDWARE BOX

https://app.any.run/
PhishTool