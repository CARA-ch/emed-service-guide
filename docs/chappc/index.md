# CH-APPC

Currently drafting the [swiss specification in ArtDecor](https://art-decor.org/art-decor/decor-project--ch-appc-).

Only a subset of the specification is supported in the eMedication service.

## Document boilerplate

```xml title="Document boilerplate"
<?xml version="1.0" encoding="UTF-8"?>
<PolicySet xmlns="urn:oasis:names:tc:xacml:2.0:policy:schema:os"
           xmlns:hl7="urn:hl7-org:v3"
           PolicySetId="urn:uuid:e3585197-9e3d-4ca3-9583-4540a3a5b64b"
           PolicyCombiningAlgId="urn:oasis:names:tc:xacml:1.0:policy-combining-algorithm:deny-overrides"> <!-- (1) -->
    <Description>Test policy set</Description> <!-- (2) -->
    <Target>
        <Resources>
            <Resource>
                <ResourceMatch MatchId="urn:hl7-org:v3:function:II-equal">
                    <AttributeValue DataType="urn:hl7-org:v3#II">
                        <hl7:InstanceIdentifier root="2.16.756.5.30.1.127.3.10.3" extension="761337611932009095"/> <!-- (3) -->
                    </AttributeValue>
                    <ResourceAttributeDesignator AttributeId="urn:e-health-suisse:2015:epr-spid" DataType="urn:hl7-org:v3#II"/>
                </ResourceMatch>
            </Resource>
        </Resources>
    </Target>
    <!-- The rules go here -->
</PolicySet>
```

1.  Since we use the DenyOverrides algorithm, the order of sub-PolicySets is not important; all will be evaluated and any `Deny` will override any `Allow`.
2.  A mandatory but not-quite-useful description.
3.  The patient EPR-SPID.

## Example

```xml title="Full document example"
<?xml version="1.0" encoding="UTF-8"?>
<PolicySet PolicySetId="urn:uuid:e3585197-9e3d-4ca3-9583-4540a3a5b64b" PolicyCombiningAlgId="urn:oasis:names:tc:xacml:1.0:policy-combining-algorithm:deny-overrides" xmlns="urn:oasis:names:tc:xacml:2.0:policy:schema:os" xmlns:hl7="urn:hl7-org:v3">
    <Description>Test policy set</Description><!-- (1) -->
    <Target>
        <Resources>
            <Resource>
                <ResourceMatch MatchId="urn:hl7-org:v3:function:II-equal">
                    <AttributeValue DataType="urn:hl7-org:v3#II">
                        <hl7:InstanceIdentifier root="2.16.756.5.30.1.127.3.10.3" extension="761337611932009095"/> <!-- (2) -->
                    </AttributeValue>
                    <ResourceAttributeDesignator AttributeId="urn:e-health-suisse:2015:epr-spid" DataType="urn:hl7-org:v3#II"/>
                </ResourceMatch>
            </Resource>
        </Resources>
    </Target>
    <PolicySet PolicySetId="urn:uuid:29e64cce-19f6-43c4-9cc9-0227cb361ba1" PolicyCombiningAlgId="urn:oasis:names:tc:xacml:1.0:policy-combining-algorithm:deny-overrides">
        <Description>description emergency</Description>
        <Target>
            <Subjects>
                <Subject>
                    <SubjectMatch MatchId="urn:hl7-org:v3:function:CV-equal">
                        <AttributeValue DataType="urn:hl7-org:v3#CV">
                            <hl7:CodedValue code="HCP" codeSystem="2.16.756.5.30.1.127.3.10.6" displayName="Healthcare professional"/>
                        </AttributeValue>
                        <SubjectAttributeDesignator AttributeId="urn:oasis:names:tc:xacml:2.0:subject:role" DataType="urn:hl7-org:v3#CV"/>
                    </SubjectMatch>
                    <SubjectMatch MatchId="urn:hl7-org:v3:function:CV-equal">
                        <AttributeValue DataType="urn:hl7-org:v3#CV">
                            <hl7:CodedValue code="EMER" codeSystem="2.16.756.5.30.1.127.3.10.5" displayName="Emergency Access"/>
                        </AttributeValue>
                        <SubjectAttributeDesignator AttributeId="urn:oasis:names:tc:xspa:1.0:subject:purposeofuse" DataType="urn:hl7-org:v3#CV"/>
                    </SubjectMatch>
                </Subject>
            </Subjects>
        </Target>
        <PolicySetIdReference>urn:e-health-suisse:2022:policies:pmp:emedication-access</PolicySetIdReference> <!-- (3) -->
    </PolicySet>
    <PolicySet PolicySetId="urn:uuid:46a1a4c0-ed1e-439b-8da8-a523b10ce2b5" PolicyCombiningAlgId="urn:oasis:names:tc:xacml:1.0:policy-combining-algorithm:deny-overrides">
        <Description>description hcp</Description>
        <Target>
            <Subjects>
                <Subject>
                    <SubjectMatch MatchId="urn:oasis:names:tc:xacml:1.0:function:string-equal">
                        <AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">7601002860123</AttributeValue>
                        <SubjectAttributeDesignator AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-id" DataType="http://www.w3.org/2001/XMLSchema#string"/>
                    </SubjectMatch>
                    <SubjectMatch MatchId="urn:oasis:names:tc:xacml:1.0:function:string-equal">
                        <AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">urn:gs1:gln</AttributeValue>
                        <SubjectAttributeDesignator AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-id-qualifier" DataType="http://www.w3.org/2001/XMLSchema#string"/>
                    </SubjectMatch>
                    <SubjectMatch MatchId="urn:hl7-org:v3:function:CV-equal">
                        <AttributeValue DataType="urn:hl7-org:v3#CV">
                            <hl7:CodedValue code="HCP" codeSystem="2.16.756.5.30.1.127.3.10.6" displayName="Healthcare professional"/>
                        </AttributeValue>
                        <SubjectAttributeDesignator AttributeId="urn:oasis:names:tc:xacml:2.0:subject:role" DataType="urn:hl7-org:v3#CV"/>
                    </SubjectMatch>
                    <SubjectMatch MatchId="urn:hl7-org:v3:function:CV-equal">
                        <AttributeValue DataType="urn:hl7-org:v3#CV">
                            <hl7:CodedValue code="NORM" codeSystem="2.16.756.5.30.1.127.3.10.5" displayName="Normal Access"/>
                        </AttributeValue>
                        <SubjectAttributeDesignator AttributeId="urn:oasis:names:tc:xspa:1.0:subject:purposeofuse" DataType="urn:hl7-org:v3#CV"/>
                    </SubjectMatch>
                </Subject>
            </Subjects>
            <Environments>
                <Environment>
                    <EnvironmentMatch MatchId="urn:oasis:names:tc:xacml:1.0:function:date-greater-than-or-equal">
                        <AttributeValue DataType="http://www.w3.org/2001/XMLSchema#date">2027-02-03</AttributeValue>
                        <EnvironmentAttributeDesignator AttributeId="urn:oasis:names:tc:xacml:1.0:environment:current-date" DataType="http://www.w3.org/2001/XMLSchema#date"/>
                    </EnvironmentMatch>
                </Environment>
            </Environments>
        </Target>
        <PolicySetIdReference>urn:e-health-suisse:2015:policies:exclusion-list</PolicySetIdReference><!-- (4) -->
    </PolicySet>
    <PolicySet PolicySetId="urn:uuid:23393af4-68aa-46d1-a807-767e80fbd112" PolicyCombiningAlgId="urn:oasis:names:tc:xacml:1.0:policy-combining-algorithm:deny-overrides">
        <Description>description group</Description>
        <Target>
            <Subjects>
                <Subject>
                    <SubjectMatch MatchId="urn:hl7-org:v3:function:CV-equal">
                        <AttributeValue DataType="urn:hl7-org:v3#CV">
                            <hl7:CodedValue code="HCP" codeSystem="2.16.756.5.30.1.127.3.10.6" displayName="Healthcare professional"/>
                        </AttributeValue>
                        <SubjectAttributeDesignator AttributeId="urn:oasis:names:tc:xacml:2.0:subject:role" DataType="urn:hl7-org:v3#CV"/>
                    </SubjectMatch>
                    <SubjectMatch MatchId="urn:hl7-org:v3:function:CV-equal">
                        <AttributeValue DataType="urn:hl7-org:v3#CV">
                            <hl7:CodedValue code="NORM" codeSystem="2.16.756.5.30.1.127.3.10.5" displayName="Normal Access"/>
                        </AttributeValue>
                        <SubjectAttributeDesignator AttributeId="urn:oasis:names:tc:xspa:1.0:subject:purposeofuse" DataType="urn:hl7-org:v3#CV"/>
                    </SubjectMatch>
                    <SubjectMatch MatchId="urn:oasis:names:tc:xacml:1.0:function:anyURI-equal">
                        <AttributeValue DataType="http://www.w3.org/2001/XMLSchema#anyURI">urn:oid:1.2.3</AttributeValue>
                        <SubjectAttributeDesignator AttributeId="urn:oasis:names:tc:xspa:1.0:subject:organization-id" DataType="http://www.w3.org/2001/XMLSchema#anyURI"/>
                    </SubjectMatch>
                </Subject>
            </Subjects>
            <Environments>
                <Environment>
                    <EnvironmentMatch MatchId="urn:oasis:names:tc:xacml:1.0:function:date-less-than-or-equal">
                        <AttributeValue DataType="http://www.w3.org/2001/XMLSchema#date">2032-01-01</AttributeValue>
                        <EnvironmentAttributeDesignator AttributeId="urn:oasis:names:tc:xacml:1.0:environment:current-date" DataType="http://www.w3.org/2001/XMLSchema#date"/>
                    </EnvironmentMatch>
                </Environment>
            </Environments>
        </Target>
        <PolicySetIdReference>urn:e-health-suisse:2022:policies:pmp:emedication-access</PolicySetIdReference><!-- (5) -->
    </PolicySet>
</PolicySet>

```

1.  A mandatory but not-quite-useful description.
2.  The EPR-SPID of the patient to whose record the rules will be applied.
3.  Autorizing emergency access of healthcare professionals to eMedication documents.
4.  Denying access to a single healthcare professional.
5.  Autorizing access to a group of healthcare professionals.
