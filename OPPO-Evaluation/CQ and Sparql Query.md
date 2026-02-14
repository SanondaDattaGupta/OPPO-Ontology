1. What types of personal data are being collected by Telegram, and how many of each type are there?

	```SELECT ?Type (COUNT(?Data) AS ?Count)
	WHERE {
	    ?Data rdf:type ?Type .
	    ?Type rdfs:subClassOf oppo:PersonalData .
	    ?Data oppo:isProvidedBy :TelegramUser .
	}
	GROUP BY ?Type```

2. What are the data that is being collected by Telegram?

	```SELECT DISTINCT ?Data 
	WHERE {
	    ?Data rdf:type ?Type .
	   ?Type rdfs:subClassOf oppo:PersonalData .
	    ?Data oppo:isProvidedBy :TelegramUser .
	}```

3. What information do I need to provide to Telegram and how long the information is being stored? 

	```select distinct ?Data ?StorageDuration where {
	    ?TelegramDataRecipientRole oppo:actsIn oppo:FirstPartyDataRecipientRole.
	    ?TelegramDataRecipientRole oppo:isRecipientOf ?Data.
	    ?Data oppo:isProvidedBy ?TelegramUsers.
	    ?Data oppo:isAbout ?TelegramUsers.
	    ?TelegramDataRecipientRole oppo:hasPolicy ?Policy.
	    ?Policy oppo:hasDataPractice ?Practice. 
	    ?Practice oppo:hasDurationDescription ?StorageDuration.
	    ?Practice oppo:actsOn ?Data.
	}```

4. What is the security mechanism the social media follows to keep my data secure?

	```select  distinct ?SecurityMechanism ?SecurityMechanismType where {
	    ?TelegramDataRecipientRole oppo:actsIn oppo:FirstPartyDataRecipientRole.
	    ?TelegramDataRecipientRole oppo:hasPolicy ?Policy.
	    ?Policy oppo:hasDataPractice ?Practice.
	    ?Practice oppo:hasSecurityMechanism ?SecurityMechanism.
	    ?SecurityMechanism rdf:type ?SecurityMechanismType.
	}```

5. Where does the social media store my video and photo?

	```select distinct ?Location where{
	    ?TelegramDataRecipientRole oppo:actsIn oppo:FirstPartyDataRecipientRole.
	    ?TelegramDataRecip oppo:hasPolicy ?Policy.
	    ?Policy oppo:hasDataPractice ?Practice. 
	    ?Practice oppo:hasStorageLocation ?Location.
	    ?Practice oppo:actsOn ?Data.
	    ?data oppo:isAbout ?TelegramUser filter (?Data in (oppo:Photo, oppo:Video)).
	}```

6. Where and how does Telegram store my information?

	```select ?location ?storagetype where{
	    ?TelegramDataRecipientRole oppo:actsIn oppo:FirstPartyDataRecipientRole.
	   ?TelegramDataRecipientRole oppo:hasPolicy ?Policy.
	    ?Policy oppo:hasDataPractice ?Practice. 
	   ?Practice oppo:hasStorageLocation ?location.
	   ?Practice oppo:hasStorageEntity ?storagetype.
	}```

7. How can I modify my personal information?

	```select ?ModificationRequest where {
	    ?TelegramDataRecipientRole oppo:actsIn oppo:FirstPartyDataRecipientRole.
	    ?TelegramDataRecipientRole oppo:hasPolicy ?Policy.
	    ?Policy oppo:hasDataPractice ?Practice.
	    ?Practice rdf:type oppo:DataStorageRectificationPractice.
	    ?Practice oppo:hasRequestType ?ModificationRequest.
	}```

8. What contents are stored for a maximum of 12 months?

	```select ?Data  where{
	    ?Organization oppo:actsIn oppo:FirstPartyDataRecipientRole.
	    ?Organization oppo:hasPolicy ?Policy.
	    ?policy oppo:hasDataPractice ?practice.
	    ?practice oppo:hasDurationDescription ?Duration.
	    ?Duration time:months ?Month filter (?Month <= (12)).
	    ?practice oppo:actsOn ?Data. 
	}```

9. Which of my personal information will be removed after I request to delete my account?

	```select distinct ?Data where{
	    ?organization oppo:actsIn oppo:FirstPartyDataRecipientRole. 
	    ?organization oppo:hasPolicy ?policy.
	    ?policy oppo:hasDataPractice ?practice.
	    ?practice rdf:type oppo:DataStorageErasurePractice.
	    ?Practice oppo:actsOn ?Data.
	    ?organization oppo:isRecipientOf ?Data.
	    ?Data oppo:isAbout ?user.
	}```

10. How can I delete my account?

	```select distinct ?DeletionRequest where {
	    ?Organization oppo:actsIn oppo:FirstPartyDataRecipientRole.
	    ?Organization oppo:hasPolicy ?Policy.
	    ?Policy oppo:hasDataPractice ?Practice.
	    ?Practice rdf:type oppo:DataStorageErasurePractice.
	    ?Practice oppo:hasRequestType ?DeletionRequest.
	}```

11. Why does the OSN collect my cookie information?

	```select ?Purpose where {
	    ?organization oppo:actsIn oppo:FirstPartyDataRecipientRole.
	    ?organization oppo:hasPolicy ?policy.
	    ?policy oppo:hasDataPractice ?Practice. 
	    ?Practice oppo:hasPurpose ?Purpose.
	   ?Practice oppo:actsOn ?Data.
	    ?organization oppo:isRecipientOf ?Data.
	    ?Data oppo:isAbout ?TelegramUser filter (?Data in (oppo:CookieInformation)).
	}```

12. How does the OSN use the personal information provided by me?

	```select  distinct ?PurposeType  where {
	    ?Organization oppo:actsIn oppo:FirstPartyDataRecipientRole.
	    ?Organization oppo:hasPolicy ?Policy.
	    ?Policy oppo:hasDataPractice ?Practice.
	    ?Practice oppo:hasPurpose ?Purpose.
	    ?Purpose rdf:type ?PurposeType.
	    ?Practice oppo:actsOn ?Data.
	    ?Data oppo:isAbout ?Individual.
	    ?Individual rdf:type oppo:DataSubjectRole.     
	}```

13. What are the contents that have no definite storage duration specified?

	```select distinct ?Data where {
	    ?organization oppo:actsIn oppo:FirstPartyDataRecipientRole.
	    ?organization oppo:hasPolicy ?policy.
	    ?policy oppo:hasDataPractice ?Practice. 
	    ?Practice oppo:hasDurationDescription ?Duration.
	    ?Duration rdf:type oppo:IndefiniteDurationDescription.
	    ?Practice oppo:actsOn ?Data.
	    ?organization oppo:isRecipientOf ?Data.
	}```

14. How long does the OSN store my location information?


	```select ?Data ?Duration where {
	    ?organization oppo:actsIn oppo:FirstPartyDataRecipientRole.
	    ?organization oppo:hasPolicy ?policy.
	    ?policy oppo:hasDataPractice ?Practice. 
	    ?Practice oppo:hasDurationDescription ?Duration.
	    ?Practice oppo:actsOn ?Data.
	    ?Data oppo:isAbout ?User.
	    ?organization oppo:isRecipientOf ?Data filter (?Data in (oppo:Location)).
	}```

15. How long does it take the OSN to delete my account?

	```select ?type ?Delay where {
	    ?organization oppo:hasPolicy ?policy.
	    ?policy oppo:hasDataPractice ?Practice. 
	    ?Practice rdf:type oppo:DataStorageErasurePractice.
	    ?Practice oppo:hasResponseDelay ?DeletionDuration.
	    ?DeletionDuration ?type ?Delay.
	}```

16. Who has access to my credit card information ?

	```select ?Data ?DataReciever where {
	    ?organization oppo:actsIn oppo:FirstPartyDataRecipientRole.
	    ?organization oppo:hasPolicy ?policy.
	    ?policy oppo:hasDataPractice ?Practice. 
	    ?Practice rdf:type oppo:DataSecurityAccessPractice.
	    ?Practice oppo:actsOn ?Data.
	    ?Data oppo:isAbout ?User.
	    ?DataReciever oppo:isRecipientOf ?Data filter (?Data in (oppo:CreditCardNumber)).
	}```

17. How long does the OSN store my photos?

	```select ?Data ?Duration where {
	    ?organization oppo:actsIn oppo:FirstPartyDataRecipientRole.
	    ?organization oppo:hasPolicy ?policy.
	    ?policy oppo:hasDataPractice ?Practice. 
	    ?Practice oppo:hasDurationDescription ?Duration.
	    ?Practice oppo:actsOn ?Data.
	    ?Data oppo:isAbout ?User.
	    ?organization oppo:isRecipientOf ?Data filter (?Data in (oppo:Photo)).
	}```

18. Where long does the social media typically store my public chat and private chat?

	```SELECT ?ChatType ?DurationDescription 
	WHERE {
	{   
	       ?Practice oppo:actsOn oppo:PublicChat .
	       ?Practice oppo:hasDurationDescription ?DurationDescription .	       
	       BIND("Public Chat" AS ?ChatType)
	    }
	    UNION
	    {
	        ?Practice oppo:actsOn oppo:PrivateChat .
	       ?Practice oppo:hasDurationDescription ?DurationDescription .
	        BIND("Private Chat" AS ?ChatType)
	    }
	}```

19. what sensitive data (According to GDPR) is collected by telegram? 

	```SELECT distinct ?Data  ?Type ?User
	WHERE {
	   	?Data oppo:isProvidedBy ?User .
	    ?User rdf:type oppo:DataProviderRole .
	    ?Data rdf:type ?Type.
	    ?Type rdfs:subClassOf gdpr:SensitiveData. 
	}```

20. For what purpose do you use my data?

	 ```select ?Purpose
		WHERE {
	    ?Data rdf:type oppo:PersonalData .
	    ?Data oppo:isProvidedBy ?user .
	    ?Practice oppo:actsOn ?Data .
	    ?Practice oppo:hasPurpose ?Purpose .
	}```

21. What data practices are included in the Telegram policy?

	```SELECT DISTINCT ?Practice
		WHERE {
		    ?SocialMedia oppo:hasPolicy ?Policy .
		    ?Policy rdf:type oppo:PrivacyPolicy .
		    ?Policy oppo:hasDataPractice ?DataPractice .
		    ?DataPractice rdf:type ?Practice .
		    ?Practice rdfs:subClassOf oppo:DataPractice .		    
		}
		```

22. Which data practices act on public chats?
	
	```SELECT ?Practice
	WHERE {
	    ?SocialMedia oppo:hasPolicy ?Policy .
	    ?Policy rdf:type oppo:PrivacyPolicy .
	    ?Policy oppo:hasDataPractice ?DataPractice .
	    ?DataPractice oppo:actsOn oppo:PublicChat .
	  	?DataPractice rdf:type ?Practice .
	    ?Practice rdfs:subClassOf oppo:DataPractice .
	}```

23. Which data practices act on Location?

	```SELECT ?Practice
	WHERE {
	    ?SocialMedia oppo:hasPolicy ?Policy .
	    ?Policy rdf:type oppo:PrivacyPolicy .
	    ?Policy oppo:hasDataPractice ?DataPractice .
	    ?DataPractice oppo:actsOn oppo:Location .
	  	?DataPractice rdf:type ?Practice .
	    ?Practice rdfs:subClassOf oppo:DataPractice .
	}```

24. What security mechanisms are applied to my personal data collected by Telegram?


	```SELECT distinct ?Data ?Security
	WHERE {
	    ?SocialMedia oppo:hasPolicy ?Policy .
	    ?Policy rdf:type oppo:PrivacyPolicy .
	    ?Policy oppo:hasDataPractice ?DataPractice .
	    ?DataPractice oppo:hasSecurityMechanism ?SecurityMechanisms .
	    ?SecurityMechanisms rdf:type ?Security .
	    ?Security rdfs:subClassOf oppo:SecurityMechanism .
	    ?SecurityMechanisms oppo:appliesTo ?Data .
	    ?Data rdf:type oppo:PersonalData .
	}```


25. Where does Telegram's data servers located?

	```SELECT  ?StorageLocation 
	WHERE {
	    ?SocialMedia oppo:hasPolicy ?Policy .
	    ?Policy rdf:type oppo:PrivacyPolicy .
	    ?Policy oppo:hasDataPractice ?DataPractice .
	    ?DataPractice oppo:hasStorageEntity ?Entity.
	    ?DataPractice oppo:hasStorageLocation ?StorageLocation .
	}
	```

26. What methods are used by Telegram to encrypt which types of data?

	```Select ?Method ?Data
	{
	    ?SocialMedia oppo:hasPolicy ?Policy .
	    ?Policy rdf:type oppo:PrivacyPolicy .
	    ?Policy oppo:hasDataPractice ?DataPractice .
	    ?DataPractice oppo:hasSecurityMechanism ?SecurityMechanism .
	    ?SecurityMechanism rdf:type ?Method .
	    ?Method rdfs:subClassOf oppo:EncryptionMechanism .
	    ?SecurityMechanism oppo:appliesTo ?Data .
	     FILTER(?Method != oppo:EncryptionMechanism)
	}```	

27.  What authentication methods are supported by Telegram ?

	```Select ?Method {
		    ?SocialMedia oppo:hasPolicy ?Policy .
		    ?Policy rdf:type oppo:PrivacyPolicy .
		    ?Policy oppo:hasDataPractice ?DataPractice .
		    ?DataPractice oppo:hasSecurityMechanism ?SecurityMechanism .
		    ?SecurityMechanism rdf:type ?Method .
		    ?Method rdfs:subClassOf oppo:AuthenticationMechanism .
		}
	```
28. What is the procedure for requesting the erasure of personal data on Telegram and how long does it take to process the request?

	```select distinct ?DeletionRequest ?DeletionDurationDelay where {
		    ?Organization oppo:hasPolicy ?Policy.
		    ?Policy oppo:hasDataPractice ?Practice.
		    ?Practice rdf:type oppo:DataStorageErasurePractice.
		    ?Practice oppo:hasRequestType ?DeletionRequest.
	    	?Practice oppo:hasResponseDelay ?DeletionDuration .
	    	?DeletionDuration ?type ?DeletionDurationDelay.
		}
		```
29. Which types of personal data will be received by which third party?

	```select distinct ?Data ?Type ?ThirdParty where {
	    	?Data oppo:isAbout ?User.
	    	?Data oppo:isProvidedBy ?User.
	    	?ThirdParty oppo:isRecipientOf ?Data.
	    	?ThirdParty oppo:actsIn oppo:ThirdPartyDataRecipientRole.
		    ?Organization oppo:hasPolicy ?Policy.
	     	?Data rdf:type ?Type .
	    	?Type rdfs:subClassOf oppo:PersonalData .
			FILTER(?Type != oppo:PersonalData)
		}
		```

30. Which of my Identity data has been collected by Telegram and for which purpose?

	```SELECT ?Data ?Purpose 
	WHERE {
	    ?SocialMedia oppo:hasPolicy ?Policy .
	    ?Policy rdf:type oppo:PrivacyPolicy .
	    ?Policy oppo:hasDataPractice ?DataPractice .
	    ?DataPractice oppo:actsOn ?Data .
	    ?Data rdf:type oppo:IdentityPersonalData.
	    ?DataPractice oppo:hasPurpose ?Purpose .
	}```

	

	





	
