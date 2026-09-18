#What types of personal data are being collected by the OSN, and how many of each type are there?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?OSN ?Policy ?DataType
       (COUNT(DISTINCT ?Data) AS ?Count)
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .
    ?Practice oppo:actsOn ?Data .

    ?Data rdf:type ?DataType .
    ?DataType rdfs:subClassOf* oppo:PersonalData .
}
GROUP BY ?OSN ?Policy ?DataType
ORDER BY ?OSN ?DataType
```

#What are the data that is being collected by the OSN?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>

SELECT DISTINCT ?OSN ?Policy ?Data
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .
    ?Practice oppo:actsOn ?Data .
}
ORDER BY ?OSN ?Data
```

#What information do I need to provide to the OSN and how long the information is being stored?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>
PREFIX time: <http://www.w3.org/2006/time#>

SELECT DISTINCT ?OSN ?Policy ?Data ?Duration ?Months
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Data oppo:isProvidedBy ?User .

    OPTIONAL {
        ?Policy oppo:hasDataPractice ?Practice .
        ?Practice oppo:actsOn ?Data ;
                  oppo:hasDurationDescription ?Duration .

        OPTIONAL { ?Duration time:months ?Months . }
    }
}
ORDER BY ?OSN ?Data
```

#What is the security mechanism the social media follows to keep my data secure?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?OSN ?Policy ?Mechanism ?MechanismType
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .
    ?Practice oppo:hasSecurityMechanism ?Mechanism .

    ?Mechanism rdf:type ?MechanismType .
    ?MechanismType rdfs:subClassOf* oppo:SecurityMechanism .
}
ORDER BY ?OSN ?MechanismType
```

#Where does the social media store my video and photo?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>

SELECT DISTINCT ?OSN ?Policy ?Data ?StorageEntity ?StorageLocation
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .

    ?Practice oppo:actsOn ?Data .

    VALUES ?Data {
        oppo:Video
        oppo:Photo
    }

    OPTIONAL {
        ?Practice oppo:hasStorageEntity ?StorageEntity .
    }

    OPTIONAL {
        ?Practice oppo:hasStorageLocation ?StorageLocation .
    }
}
ORDER BY ?OSN ?Data
```

#Where and how does the OSN store my information?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>

SELECT DISTINCT ?OSN ?Policy ?Data ?StorageEntity ?StorageLocation
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .
    ?Practice oppo:actsOn ?Data .

    OPTIONAL {
        ?Practice oppo:hasStorageEntity ?StorageEntity .
    }

    OPTIONAL {
        ?Practice oppo:hasStorageLocation ?StorageLocation .
    }

    FILTER(BOUND(?StorageEntity) || BOUND(?StorageLocation))
}
ORDER BY ?OSN ?Data
```


#How can I modify my personal information?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>

SELECT DISTINCT ?OSN ?Policy ?Data ?RequestType
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .

    ?Practice a oppo:DataStorageRectificationPractice ;
              oppo:actsOn ?Data ;
              oppo:hasRequestType ?RequestType .
}
ORDER BY ?OSN ?Data
```

#What contents are stored for a maximum of 12 months?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>
PREFIX time: <http://www.w3.org/2006/time#>

SELECT ?Organization ?Policy
       (IF(STRLEN(GROUP_CONCAT(DISTINCT STR(?Data); separator=", ")) = 0,
           "No result",
           GROUP_CONCAT(DISTINCT STR(?Data); separator=", "))
        AS ?StoredData)
WHERE {
    ?Organization oppo:actsIn oppo:FirstPartyDataRecipientRole .
    ?Organization oppo:hasPolicy ?Policy .

    OPTIONAL {
        ?Policy oppo:hasDataPractice ?Practice .
        ?Practice oppo:hasDurationDescription ?Duration .
        ?Duration time:months ?Month .
        ?Practice oppo:actsOn ?Data .

        FILTER (?Month <= 12)
    }
}
GROUP BY ?Organization ?Policy
ORDER BY ?Organization
```

#Which of my personal information will be removed after I request to delete my account?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?OSN ?Policy ?Data ?DataType
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .

    ?Practice rdf:type oppo:DataStorageErasurePractice ;
              oppo:actsOn ?Data .

    ?Data rdf:type ?DataType .
    ?DataType rdfs:subClassOf* oppo:PersonalData .
}
ORDER BY ?OSN ?DataType ?Data
```


#How can I delete my account?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>

SELECT DISTINCT ?OSN ?Policy ?DeletionRequest
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .

    ?Practice rdf:type oppo:DataStorageErasurePractice ;
              oppo:hasRequestType ?DeletionRequest .
}
ORDER BY ?OSN
```

#How does the OSN use the personal information provided by me?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>

SELECT DISTINCT ?OSN ?Policy ?Data ?Purpose
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .

    ?Practice oppo:actsOn ?Data ;
              oppo:hasPurpose ?Purpose .

    ?Data oppo:isProvidedBy ?User .
}
ORDER BY ?OSN ?Data ?Purpose
```

#Why does the OSN collect my cookie information?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>

SELECT DISTINCT ?OSN ?Policy ?Purpose
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .

    ?Practice oppo:actsOn oppo:CookieInformation ;
              oppo:hasPurpose ?Purpose .
}
ORDER BY ?OSN ?Purpose
```

#What are the contents that have no definite storage duration specified?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>

SELECT DISTINCT ?OSN ?Policy ?Data ?Duration
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .

    ?Practice oppo:actsOn ?Data ;
              oppo:hasDurationDescription ?Duration .
              ```

#How long does the OSN store my location information?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>

SELECT DISTINCT ?OSN ?Policy ?Duration
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .

    ?Practice oppo:actsOn oppo:Location ;
              oppo:hasDurationDescription ?Duration .
}
ORDER BY ?OSN ?Duration
```

#How long does it take the OSN to delete my account?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>
PREFIX time: <http://www.w3.org/2006/time#>

SELECT DISTINCT ?OSN ?Policy ?ResponseDelay ?Hours
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .

    ?Practice a oppo:DataStorageErasurePractice ;
              oppo:hasResponseDelay ?ResponseDelay .

    OPTIONAL {
        ?ResponseDelay time:hour ?Hours .
    }
}
ORDER BY ?OSN
```

#Who has access to my credit card information ?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>

SELECT DISTINCT ?Recipient
WHERE {
    ?Recipient oppo:isRecipientOf oppo:CreditCardNumber .
}
```

#How long does the OSN store my photos?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>

SELECT DISTINCT ?OSN ?Policy ?Data ?Duration
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .

    VALUES ?Data {
        oppo:Photo
        oppo:Image
    }

    ?Practice oppo:actsOn ?Data ;
              oppo:hasDurationDescription ?Duration .
}
ORDER BY ?OSN ?Data
```

#How long does the social media typically store my public chat and private chat?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>

SELECT DISTINCT ?OSN ?Policy ?Duration
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .

    ?Practice oppo:actsOn oppo:PrivateChat ;
              oppo:hasDurationDescription ?Duration .
}
ORDER BY ?OSN ?Duration
```
#For what purpose do you use my data?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>

SELECT DISTINCT ?OSN ?Policy ?Data ?Purpose
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .

    ?Practice oppo:actsOn ?Data ;
              oppo:hasPurpose ?Purpose .
}
ORDER BY ?OSN ?Data ?Purpose
```

#What data practices are included in the the OSN policy?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?OSN ?Policy ?Practice ?PracticeType
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .

    ?Practice rdf:type ?PracticeType .
    ?PracticeType rdfs:subClassOf* oppo:DataPractice .
}
ORDER BY ?OSN ?PracticeType ?Practice
```

#Which data practices act on public chats?

```
PREFIX oppo: <http://umaine.edu/oppo/core/v1#>
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?OSN ?Policy ?Practice ?PracticeType
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .

    ?Practice oppo:actsOn oppo:PublicChat ;
              rdf:type ?PracticeType .

    ?PracticeType rdfs:subClassOf* oppo:DataPractice .
}
ORDER BY ?OSN ?PracticeType ?Practice
```

#Which data practices act on Location?

```
PREFIX oppo: <http://umaine.edu/oppo/core/v1#>
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?OSN ?Policy ?Practice ?PracticeType
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .

    ?Practice oppo:actsOn oppo:Location ;
              rdf:type ?PracticeType .

    ?PracticeType rdfs:subClassOf* oppo:DataPractice .
}
ORDER BY ?OSN ?PracticeType ?Practice
```

#What security mechanisms are applied to my personal data collected by the OSN?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?OSN ?Policy ?Data ?Mechanism ?MechanismType
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .

    ?Practice oppo:hasSecurityMechanism ?Mechanism .

    ?Mechanism oppo:appliesTo ?Data ;
               rdf:type ?MechanismType .

    ?MechanismType rdfs:subClassOf+ oppo:SecurityMechanism .
}
ORDER BY ?OSN ?MechanismType ?Data
```

#Where does the OSN's data servers located?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>

SELECT DISTINCT ?OSN ?Policy ?StorageEntity ?StorageLocation
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .

    ?Practice oppo:hasStorageEntity ?StorageEntity ;
              oppo:hasStorageLocation ?StorageLocation .
}
ORDER BY ?OSN ?StorageEntity ?StorageLocation
```

#What methods are used by the OSN to encrypt which types of data?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?OSN ?Policy ?Data ?Mechanism ?EncryptionType
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .
    ?Practice oppo:hasSecurityMechanism ?Mechanism .

    ?Mechanism oppo:appliesTo ?Data ;
               rdf:type ?EncryptionType .

    ?EncryptionType rdfs:subClassOf* oppo:EncryptionMechanism .
}
ORDER BY ?OSN ?EncryptionType ?Data
```

#What authentication methods are supported by the OSN ?

```
PREFIX oppo: <http://umaine.edu/oppo/core/v1#>
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?OSN ?Policy ?Mechanism ?AuthenticationType
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .
    ?Practice oppo:hasSecurityMechanism ?Mechanism .

    ?Mechanism rdf:type ?AuthenticationType .

    ?AuthenticationType
        rdfs:subClassOf* oppo:AuthenticationMechanism .
}
ORDER BY ?OSN ?AuthenticationType
```

#What is the procedure for requesting the erasure of personal data on the OSN and how long does it take to process the request?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>
PREFIX time: <http://www.w3.org/2006/time#>

SELECT DISTINCT ?OSN ?Policy ?RequestType ?ResponseDelay ?Hours
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .

    ?Practice a oppo:DataStorageErasurePractice .

    OPTIONAL {
        ?Practice oppo:hasRequestType ?RequestType .
    }

    OPTIONAL {
        ?Practice oppo:hasResponseDelay ?ResponseDelay .
        OPTIONAL {
            ?ResponseDelay time:hour ?Hours .
        }
    }
}
ORDER BY ?OSN ?Practice
```

#Which types of personal data will be received by which third party?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?ThirdParty ?Data ?DataType
WHERE {
    ?ThirdParty oppo:actsIn oppo:ThirdPartyDataRecipientRole ;
                oppo:isRecipientOf ?Data .

    ?Data rdf:type ?DataType .
    ?DataType rdfs:subClassOf* oppo:PersonalData .
}
ORDER BY ?ThirdParty ?DataType ?Data
```

#Which of my Identity data has been collected by the OSN and for which purpose?

```PREFIX oppo: <http://umaine.edu/oppo/core/v1#>
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?OSN ?Policy ?Data ?DataType ?Purpose
WHERE {
    ?OSN oppo:actsIn oppo:FirstPartyDataRecipientRole ;
         oppo:hasPolicy ?Policy .

    ?Policy oppo:hasDataPractice ?Practice .
    ?Practice oppo:actsOn ?Data .

    ?Data rdf:type ?DataType .
    ?DataType rdfs:subClassOf* oppo:IdentityPersonalData .

    OPTIONAL {
        ?Policy oppo:hasDataPractice ?PurposePractice .
        ?PurposePractice oppo:actsOn ?Data ;
                         oppo:hasPurpose ?Purpose .
    }
}
ORDER BY ?OSN ?Data ?Purpose
```








