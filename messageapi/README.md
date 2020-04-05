## sos-message-api [DRAFT]

A specification on a "common ground" for multiple pandemic-contact-tracking-systems.
Providing a "united" backend for leveraging on the larger user base. 

### introduction

After having some look into several code based systems I found that they are not far apart in the basic aspects.
Sometimes the different apps seemed to add a lot of additional features. 
However for them to be successful they would  require a full scale adoption, making a single solution unlikely. 

Only a global and cooperative effort is the way to ensure the best outcome. 

After looking into the very strange limitations of some advertisement methods it might need clarification  about what this spec is about, and what not.
  

#### goals

* define a simple api for cooperation 
* support a federated architecture
* distribute covid-status message to "acquaintances" without revealing your own identity (e.g. telling them directly )
* offer authorities the means of distributing authenticated status information, but otherwise authority agnostic    
* grant access to status-message only to a specific app/user (no public blackboard of infected users)  
* make illicit tracking of users difficult
* do not hinder efficient scaling
* stay extensible to additional features

#### no goals 

* define the only possible exchange format for BLE 
* define the only possible user id generation and dropping schemes
* define the only possible message notification and storing schemes
* fixate anything else except the cooperation of worldwide multiple services    

### concept

Making it possible for multiple systems to coexist in a federated approach utilizing a cascading message distribution. 
Authorities may choose to hold the privilege of generating tokens at their discretion. Making it improbable to disseminate "fake" status messages in the first place.    
In the end the Token-ID may be associated with official documentation on authorities level or have no association at all. 
By verification of certificates this could be changed with no or little changes in code.       

### interface specification

#### definitions

* tokens 
  * may be issued by an authority
  * is also used for idempotency 

* user-generated-address (UGA)
  * user generated ID, depending on the specifics of the app vendor
  * stored in the app

* encrypted-random-address (ERA) 
  * encrypted user-generated-address  
  * can be an arbitrary string  
  * different app vendors can up with different schemes to encrypt it 
  
* message 
  * a message to be disseminated
  * might be digitally signed    

#### required implementation 

The backend service MUST implement a rest interface accepting the seen ERAs.
It verifies that it is no resubmission, and the submitter is entitled to utilizing a token.  
It then tries to identify all ERAs it knows. The remainder is submitted to other services, implementing this scheme using the same tokens provided.      

The service MAY notify the user about the update  

#### basic mode of operation example

Devices generate new encrypted-random-address (ERA) of their IDs and advertise them in a particular way. App vendors/operators take care of the process of doing so.   
 
![advertise unique ids (you could renew that like always) ](docs/img/blehdah2.svg)

Other devices record these ERAs until the user needs to disseminate a status message 

![distribute status message](docs/img/poststatus2.svg)

Get your current status! 

![check status](docs/img/getstatus3.svg)

 
### ideas

* BLE ERAs (EID)
  * utilize existing shared key logic 
  * https://github.com/google/eddystone/blob/master/eddystone-eid/README.md

* BLE ERAs (type 16)
  * assuming 16 Bytes space available 
  * reducing UGA to 8 Byte width ->    