# ITI-43

The [ITI-43 (Retrieve Document Set)](https://profiles.ihe.net/ITI/TF/Volume2/ITI-43.html) transaction allows a [Document Consumer](https://profiles.ihe.net/ITI/TF/Volume2/ITI-43.html#3.43.2) to retrieve a set of documents. 

This transaction, implemented as per the specifications. All documents are retrievable with an ITI-43 transaction (with the proper access rights: only patients, representatives and policy administrators can retrieve [APPC (Advanced Patient Privacy Consent)](https://www.ihe.net/uploadedFiles/Documents/ITI/IHE_ITI_Suppl_APPC.pdf) documents).

See also [EPD by example's Retrieve Document Set](https://github.com/ehealthsuisse/EPD-by-example/blob/main/files/RetrieveDocumentSet.md) page.

## On-demand documents

Generation rules for CH EMED EPR [PML (Medication List)](https://fhir.ch/ig/ch-emed-epr/document_pml.html) and [PMLC (Medication Card)](https://fhir.ch/ig/ch-emed-epr/document_pmlc.html) documents are described in the [CH EMED EPR IG](https://fhir.ch/ig/ch-emed-epr/).

## Samples

### EMED

#### Request

`Header`
``` http
POST https://test.ahdis.ch/eprik-cara/camel/pmp-int/mag-pmp-int/pmp/services/xds/iti43 HTTP/1.1
host: test.ahdis.ch
x-request-id: 9e267acd6864a09d5b00e782e3b49423
x-real-ip: 188.154.194.120
x-forwarded-host: test.ahdis.ch
x-forwarded-port: 443
x-forwarded-proto: https
x-forwarded-scheme: https
x-scheme: https
content-length: 8920
content-type: multipart/related; type="application/xop+xml"; boundary="uuid:fe38a46e-a89a-4810-856a-28f5b866b9d8"; start="<root.message@cxf.apache.org>"; start-info="application/soap+xml"
accept: */*
user-agent: Apache-CXF/3.6.2
cache-control: no-cache
pragma: no-cache
```

`Body`
```xml
--uuid:fe38a46e-a89a-4810-856a-28f5b866b9d8
Content-Type: application/xop+xml; charset=UTF-8; type="application/soap+xml"
Content-Transfer-Encoding: binary
Content-ID: <root.message@cxf.apache.org>

<soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope">
  <soap:Header>
    <wsse:Security xmlns:wsse="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd">
      <saml2:Assertion xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion" xmlns:xsd="http://www.w3.org/2001/XMLSchema" ID="_ef4ef6e0-4d28-43cf-8b02-45855b6b5341" IssueInstant="2025-10-24T14:10:58.082024400Z" Version="2.0">
        <saml2:Issuer>http://ith-icoserve.com/eHealthSolutionsSTS</saml2:Issuer>
        <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
          <ds:SignedInfo>
            <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
            <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256"/>
            <ds:Reference URI="#_ef4ef6e0-4d28-43cf-8b02-45855b6b5341">
              <ds:Transforms>
                <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
                <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#">
                  <ec:InclusiveNamespaces xmlns:ec="http://www.w3.org/2001/10/xml-exc-c14n#" PrefixList="xsd"/>
                </ds:Transform>
              </ds:Transforms>
              <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256"/>
              <ds:DigestValue>U9hLZugp8YvFib3TUavtzZzYCRROaPgl5LkuYJ2ttJ8=</ds:DigestValue>
            </ds:Reference>
          </ds:SignedInfo>
          <ds:SignatureValue>[Signature removed for better readability]</ds:SignatureValue>
          <ds:KeyInfo>
            <ds:X509Data>
              <ds:X509Certificate>[Certificate removed for better readability]</ds:X509Certificate>
            </ds:X509Data>
          </ds:KeyInfo>
        </ds:Signature>
        <saml2:Subject>
          <saml2:NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent" NameQualifier="urn:e-health-suisse:2015:epr-spid">761337618116306629</saml2:NameID>
          <saml2:SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
            <saml2:SubjectConfirmationData InResponseTo="ID_fcbfa1e0-7d47-49a2-8d21-f5bd98cd88a7" NotOnOrAfter="2025-10-24T14:14:41.829Z" Recipient="https://ws-pmp-int.cara.ch/iua"/>
          </saml2:SubjectConfirmation>
        </saml2:Subject>
        <saml2:Conditions NotBefore="2025-10-24T14:09:40.829Z" NotOnOrAfter="2025-10-24T14:14:41.829Z">
          <saml2:AudienceRestriction>
            <saml2:Audience>urn:e-health-suisse:token-audience:all-communities</saml2:Audience>
          </saml2:AudienceRestriction>
        </saml2:Conditions>
        <saml2:AuthnStatement AuthnInstant="2025-10-24T14:09:41.829Z">
          <saml2:AuthnContext>
            <saml2:AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport</saml2:AuthnContextClassRef>
          </saml2:AuthnContext>
        </saml2:AuthnStatement>
        <saml2:AttributeStatement>
          <saml2:Attribute Name="urn:oasis:names:tc:xspa:1.0:subject:subject-id">
            <saml2:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="xsd:string">MARIJE GOTTSPONER</saml2:AttributeValue>
          </saml2:Attribute>
          <saml2:Attribute Name="urn:oasis:names:tc:xacml:2.0:subject:role">
            <saml2:AttributeValue>
              <Role xmlns="urn:hl7-org:v3" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" code="PAT" codeSystem="2.16.756.5.30.1.127.3.10.6" codeSystemName="eHealth Suisse EPR Actors" displayName="Patient" xsi:type="CE"/>
            </saml2:AttributeValue>
          </saml2:Attribute>
          <saml2:Attribute Name="urn:oasis:names:tc:xspa:1.0:subject:organization-id"/>
          <saml2:Attribute Name="urn:oasis:names:tc:xspa:1.0:subject:organization"/>
          <saml2:Attribute Name="urn:oasis:names:tc:xspa:1.0:subject:purposeofuse">
            <saml2:AttributeValue>
              <PurposeOfUse xmlns="urn:hl7-org:v3" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" code="NORM" codeSystem="2.16.756.5.30.1.127.3.10.5" codeSystemName="eHealth Suisse Verwendungszweck" displayName="Normal Access" xsi:type="CE"/>
            </saml2:AttributeValue>
          </saml2:Attribute>
          <saml2:Attribute Name="urn:oasis:names:tc:xacml:2.0:resource:resource-id">
            <saml2:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="xsd:string">761337618116306629^^^&amp;2.16.756.5.30.1.127.3.10.3&amp;ISO</saml2:AttributeValue>
          </saml2:Attribute>
          <saml2:Attribute Name="urn:ihe:iti:xca:2010:homeCommunityId" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:uri">
            <saml2:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="xsd:anyURI">urn:oid:2.16.756.5.30.1.191.1.0</saml2:AttributeValue>
          </saml2:Attribute>
        </saml2:AttributeStatement>
      </saml2:Assertion>
    </wsse:Security>
    <Action soap:mustUnderstand="true" xmlns="http://www.w3.org/2005/08/addressing">urn:ihe:iti:2007:RetrieveDocumentSet</Action>
    <MessageID xmlns="http://www.w3.org/2005/08/addressing">urn:uuid:361adfb8-e9fd-43a3-b295-6fbd4d96394a</MessageID>
    <To xmlns="http://www.w3.org/2005/08/addressing">https://test.ahdis.ch/eprik-cara/camel/pmp-int/mag-pmp-int/pmp/services/xds/iti43</To>
    <ReplyTo xmlns="http://www.w3.org/2005/08/addressing">
      <Address>http://www.w3.org/2005/08/addressing/anonymous</Address>
    </ReplyTo>
  </soap:Header>
  <soap:Body>
    <xds:RetrieveDocumentSetRequest xmlns:rmd="urn:ihe:iti:rmd:2017" xmlns:lcm="urn:oasis:names:tc:ebxml-regrep:xsd:lcm:3.0" xmlns:query="urn:oasis:names:tc:ebxml-regrep:xsd:query:3.0" xmlns="urn:oasis:names:tc:ebxml-regrep:xsd:rim:3.0" xmlns:xds="urn:ihe:iti:xds-b:2007" xmlns:rs="urn:oasis:names:tc:ebxml-regrep:xsd:rs:3.0">
      <xds:DocumentRequest>
        <xds:RepositoryUniqueId>2.16.756.5.30.1.1625.3.1.2.2</xds:RepositoryUniqueId>
        <xds:DocumentUniqueId>urn:uuid:14553069-399b-4845-ba35-30612e14e115</xds:DocumentUniqueId>
      </xds:DocumentRequest>
    </xds:RetrieveDocumentSetRequest>
  </soap:Body>
</soap:Envelope>

--uuid:fe38a46e-a89a-4810-856a-28f5b866b9d8--
```

#### Response

`Header`
``` http
200 
Connection: Keep-Alive
Content-Type: multipart/related; type="application/xop+xml"; boundary="uuid:257c3e80-0248-4d19-aeec-f81366d06e54"; start="<root.message@cxf.apache.org>"; start-info="application/soap+xml"
Date: Fri, 24 Oct 2025 14:11:00 GMT
Keep-Alive: timeout=5, max=99
Server: Apache
Transfer-Encoding: chunked
```

`Body`
```xml
--uuid:257c3e80-0248-4d19-aeec-f81366d06e54
Content-Type: application/xop+xml; charset=UTF-8; type="application/soap+xml"
Content-Transfer-Encoding: binary
Content-ID: <root.message@cxf.apache.org>

<soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope">
  <soap:Header>
    <Action xmlns="http://www.w3.org/2005/08/addressing">urn:ihe:iti:2007:RetrieveDocumentSetResponse</Action>
    <MessageID xmlns="http://www.w3.org/2005/08/addressing">urn:uuid:913a5539-f67b-4c94-8d9d-803e68714cb4</MessageID>
    <To xmlns="http://www.w3.org/2005/08/addressing">http://www.w3.org/2005/08/addressing/anonymous</To>
    <RelatesTo xmlns="http://www.w3.org/2005/08/addressing">urn:uuid:361adfb8-e9fd-43a3-b295-6fbd4d96394a</RelatesTo>
  </soap:Header>
  <soap:Body>
    <xds:RetrieveDocumentSetResponse xmlns:rmd="urn:ihe:iti:rmd:2017" xmlns:lcm="urn:oasis:names:tc:ebxml-regrep:xsd:lcm:3.0" xmlns:query="urn:oasis:names:tc:ebxml-regrep:xsd:query:3.0" xmlns="urn:oasis:names:tc:ebxml-regrep:xsd:rim:3.0" xmlns:xds="urn:ihe:iti:xds-b:2007" xmlns:rs="urn:oasis:names:tc:ebxml-regrep:xsd:rs:3.0">
      <rs:RegistryResponse status="urn:oasis:names:tc:ebxml-regrep:ResponseStatusType:Success"/>
      <xds:DocumentResponse>
        <xds:RepositoryUniqueId>2.16.756.5.30.1.1625.3.1.2.2</xds:RepositoryUniqueId>
        <xds:DocumentUniqueId>urn:uuid:14553069-399b-4845-ba35-30612e14e115</xds:DocumentUniqueId>
        <xds:mimeType>application/json+fhir</xds:mimeType>
        <xds:Document>
          <xop:Include xmlns:xop="http://www.w3.org/2004/08/xop/include" href="cid:a3062b13-58e7-4516-872e-fa67ec6a86ad-91@urn%3Aihe%3Aiti%3Axds-b%3A2007"/>
        </xds:Document>
      </xds:DocumentResponse>
    </xds:RetrieveDocumentSetResponse>
  </soap:Body>
</soap:Envelope>

--uuid:257c3e80-0248-4d19-aeec-f81366d06e54
Content-Type: application/json+fhir
Content-Transfer-Encoding: binary
Content-ID: <a3062b13-58e7-4516-872e-fa67ec6a86ad-91@urn:ihe:iti:xds-b:2007>

{
  "resourceType": "Bundle",
  "meta": {
    "profile": [
      "http://fhir.ch/ig/ch-emed-epr/StructureDefinition/ch-emed-epr-document-medicationcard"
    ]
  },
  "identifier": {
    "system": "urn:ietf:rfc:3986",
    "value": "urn:uuid:14553069-399b-4845-ba35-30612e14e115"
  },
  "type": "document",
  "timestamp": "2025-10-24T16:10:59.765+02:00",
  "entry": [Content removed for better readability]
}
--uuid:257c3e80-0248-4d19-aeec-f81366d06e54--
```

## Error codes

All of these can be errors or warnings.

| Error code | Description |
| ------ | ------ |
| XDSDocumentUniqueIdError | The document associated with the uniqueId is not available. This could be because the document is not available, the requestor is not authorized to access that document or the document is no longer available. |
| XDSMissingHomeCommunityId | A value for the homeCommunityId is required and has not been specified. |
| XDSRegistryBusy XDSRepositoryBusy | Too much activity. (Not used now) |
| XDSRegistryError XDSRepositoryError | Internal Error. The error codes XDSRegistryError or XDSRepositoryError shall be returned if and only if a more detailed code is not available. |
| XDSRegistryOutOfResources XDSRepositoryOutOfResources | Resources are low. (Not used now) |
| XDSResultNotSinglePatient | This error signals that a single request would have returned content for multiple Patient Ids. TODO: that would leak that the ID exists and belongs to another patient. For privacy reasons, would it be treated like a non-existing document? If yes, this error wouldn't be possible. |
| XDSUnavailableCommunity | A community that was queried was not available. |
| XDSUnknownCommunity | A value for the homeCommunityId is not recognized. |
| XDSUnknownRepositoryId | The repositoryUniqueId value could not be resolved to a valid document repository or the value does not match the repositoryUniqueId. |
