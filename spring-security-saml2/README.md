### Relevant Articles:
#Note: Security Assertion Markup Language (SAML) is for exchanging authentication and authorization data between parties.

- [SAML with Spring Boot and Spring Security](https://www.baeldung.com/spring-security-saml)


# Generate the local.key and local.crt files (use openssl as below - the local.crt will be requried to setup SAML2 in OKTA)
openssl req -newkey rsa:2048 -nodes -keyout local.key -x509 -days 365 -out local.crt


# On the Okta developer account after signup and login (required business account for free version).
#1). Click on Applications then Create App Integration
#2). Select SAML 2.0. We’ll click on ‘Next’ to start the ‘Create SAML Integration’ wizard. This is a 
three-step wizard. Let’s complete each step to finish our setup.

NOTE: he Audience URI in our example is http://localhost:8080/saml2/service-provider-metadata/okta 
while the single sign-on URL is http://localhost:8080/login/saml2/sso/okta

Additionally, let’s configure the ‘Single Logout URL’ as http://localhost:8080/logout/saml2/slo.



On the feedback step, let’s choose the option, “I’m an Okta customer adding an internal app”.


#3). SAML Setup (click on the side link) Single Sign On using SAML will not work until you configure the app to trust Okta as an IdP.


#4). Copy Provide the following IDP metadata to your SP provider (src/main/resource/metadata/metadata-idp-okta.xml)

<?xml version="1.0" encoding="UTF-8"?><md:EntityDescriptor entityID="http://www.okta.com/exkgx4kofxAsyVRtJ5d7" xmlns:md="urn:oasis:names:tc:SAML:2.0:metadata"><md:IDPSSODescriptor WantAuthnRequestsSigned="false" protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol"><md:KeyDescriptor use="signing"><ds:KeyInfo xmlns:ds="http://www.w3.org/2000/09/xmldsig#"><ds:X509Data><ds:X509Certificate>MIIDqDCCApCgAwIBAgIGAY9PdL1NMA0GCSqGSIb3DQEBCwUAMIGUMQswCQYDVQQGEwJVUzETMBEG
A1UECAwKQ2FsaWZvcm5pYTEWMBQGA1UEBwwNU2FuIEZyYW5jaXNjbzENMAsGA1UECgwET2t0YTEU
MBIGA1UECwwLU1NPUHJvdmlkZXIxFTATBgNVBAMMDGRldi05NDIyMjcwNzEcMBoGCSqGSIb3DQEJ
ARYNaW5mb0Bva3RhLmNvbTAeFw0yNDA1MDYxOTQ5MDFaFw0zNDA1MDYxOTUwMDFaMIGUMQswCQYD
VQQGEwJVUzETMBEGA1UECAwKQ2FsaWZvcm5pYTEWMBQGA1UEBwwNU2FuIEZyYW5jaXNjbzENMAsG
A1UECgwET2t0YTEUMBIGA1UECwwLU1NPUHJvdmlkZXIxFTATBgNVBAMMDGRldi05NDIyMjcwNzEc
MBoGCSqGSIb3DQEJARYNaW5mb0Bva3RhLmNvbTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoC
ggEBALYYX+z74AS06UrZUsNyKgUZCm5AcOocFmybMV/vA0Ehmg/NbYZ6C3PtBn/1aTewGxNJR02d
GiwtEjLJKaJ6jsJ9sxnD/fs2JhoSp9NGhuKFZAtpsXwm/nFES4cEzqfmb5G1tfaphd6KKeQJsJXG
AWIV+PgYGL//RWNhHtqD6RHOIbwjlkwUjTL+Iz9ftS3yZupyDr4TyyorKreVnoNftDCzkrmnACL7
r5d4h4oHPN9DE26tHSGQtiDWdRYtuQK8TWdhqfC0dX2GBfEAtvpuNGipx4owPPeuAiAb+cpfqUMx
1fKmaRV7u3KKp5BNlbZswRhfIXJvUrCYtEzJ9/VK9qsCAwEAATANBgkqhkiG9w0BAQsFAAOCAQEA
d+azUmQ3SfpwyH3Un0aWpnWS+j63B8OlD1y9Z7FFn0sz5tzmx45lf+mQai3F/o6fH1NK5T68cwdB
Z8eHgp6l/xLN01zqZEjNB3Kf+y7cf7HmkSnJwxcWmjUnzRPiR9UC7dcXBIWQal6gDy21R555LTsp
yuLPCb92HS0vz+7J44wD0Jt+pcYtNYHzCuK0NgbqGlz/ZTqTRbgxGuTv874K7DNPx6t5OtQuOXwx
qhMRoEG1J6cvbH0L7x+Gi8Vf6HFW6svt+pRxH8nwMMuyD3Aw2eqd6NwfsMJBUaY7axKeokJ+Rtc+
O1dx1CUMpnV0c04pNNBZ6Fc6JAlqeDCOTf+8XQ==</ds:X509Certificate></ds:X509Data></ds:KeyInfo></md:KeyDescriptor><md:SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location="https://dev-94222707.okta.com/app/dev-94222707_visioncomspringsecuritysaml2app_1/exkgx4kofxAsyVRtJ5d7/slo/saml"/><md:SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://dev-94222707.okta.com/app/dev-94222707_visioncomspringsecuritysaml2app_1/exkgx4kofxAsyVRtJ5d7/slo/saml"/><md:NameIDFormat>urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified</md:NameIDFormat><md:NameIDFormat>urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress</md:NameIDFormat><md:SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location="https://dev-94222707.okta.com/app/dev-94222707_visioncomspringsecuritysaml2app_1/exkgx4kofxAsyVRtJ5d7/sso/saml"/><md:SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://dev-94222707.okta.com/app/dev-94222707_visioncomspringsecuritysaml2app_1/exkgx4kofxAsyVRtJ5d7/sso/saml"/></md:IDPSSODescriptor></md:EntityDescriptor>



#5). Let’s sign in to the Okta developer account and navigate to the ‘People’ page under the ‘Directory’ section in the left sidebar. Here, we’ll fill out the ‘Add Person’ form to create a user. 
Sometimes, it might need a refresh of the ‘People’ page and add assigment.


#6). Now, we’re all set to test our app. Let’s launch our Spring Boot app and open the default endpoint for our app at http://localhost:8080