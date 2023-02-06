+++
showdate = true
title = "The CIA Triad ♺"
date = 2023-01-10
url = "/blog/cia_triad"
+++

NOPE it's not the [CIA](https://www.cia.gov/) who watches us all, the open, friendly yet closed, confidential, epitome of ahem ahem ahem :P. The CIA triad is a set of three basic principles of information security: confidentiality, integrity, and availability. These three principles are the foundation of information security and are the basis for all security policies, standards, procedures, and guidelines. 
The CIA triad is also known as the "Holy Trinity" of information security.

## Confidentiality
Confidentiality refers to keeping data private. Any information system capable of storing, accessing and modifying data should provide the consumer a state of confidentiality. Every action performed on the data should be authorized, and no unauthorized access should be allowed. 

*Implementations of confidentiality in information systems include Access Level Restrictions (ACLs), Usage of Cryptographic algorithms while storing and transferring information etc.*

## Integrity
Integrity means that the system and the data in it have not been improperly altered or changed without authorization. Any unauthorized modification, either during storage or during the time of transmission should not be possible.

*Usage of anomaly checks and hashing on both storage applications and processing applications are few integrity implementations.*

## Availability
Availability means being able to use the system as anticipated. Here again, it’s not merely the system going down that makes availability a security concern; software errors and “blue screens of death” happen to our computers all the time. It becomes a security issue when and if someone tries to exploit the lack of availability in some way. An attacker could do this either by depriving users of a system that they depend on (such as how the loss of GPS would hamper military units in a conflict) or by merely threatening the loss of a system, known as a “ransomware” attack.

*Availability can be implemented by using redundancy in the system, using load balancers, using caching mechanisms etc.*

---
**While the above three are the most basic principles of information security, there are many other principles that are also important. These include:**

## Accountability
It is the authority of information systems to successfully scrutinize the actions of an entity and hold them accountable for the aforementioned actions.

While confidentiality makes sure that that data in question isn’t tampered with whereas Accountability comes into place once an authorized change is done to the information. No entity (either an authorized actor or the owner itself) should be able to deny the changes made.

*Accountability can be implemented by using audit logs, using access control lists, using digital signatures etc.*

## Privacy
The feature that gives the ability to refrain from sharing who/what owns an asset and the associated attributes. The asset owner depending upon the implementation of the information system should be able to decide the norms on which access and work on that information is performed.

*It also involves complying to various legislation and political problems surrounding them like GDPR, Privacy Framework by NIST etc.*

## Authenticity
The feature that gives the ability to verify the identity of an entity. It is the ability to verify the authenticity of the information and the entity that is associated with it. Any system in the information system tree should be able to successfully establish trust and authenticity before acting on the mutual motive of the interaction i.e. before the key purpose to be achieved, the two entities engaging in the communication should be able to trust and authenticate each other. Once this is achieved, the actual transaction can go ahead.

## Non-Repudiation
Any subject that has acted on a particular information object should be able to confirm the action performed and must not get away by denying the change otherwise proven. A system’s failure of not being able to provide its users with non-repudiation might lead to serious impersonation attacks.

## Auditability
Every change or action performed on an informational asset should be in a position to be audited. This is achieved by recording and validating every change ever made to an asset and storing them for a future audit. 

These changes should include ones done by both humans and information processing systems. Both external, internal audits should be possible and must lead towards compliance. It should be also applicable for a both centralized and decentralized information system tree.

The extent of auditability, storage and applicable laws may vary depending upon the industry utilizing the informational systems. For example: a payment company may have to follow a stricter regulatory compliance guidelines when compared to an organization than a flight or bus ticketing organization.