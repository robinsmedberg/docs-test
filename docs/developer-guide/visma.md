
# Visma

We use Visma as a service provider of BankId

## Visma-konfigurationer
Följande behöver konfigureras hos Visma. Detta går enbart att göra via mail. Agronods kontakt hos Visma är christian.nilsson@visma.com (sales manager erling.sjostrom@visma.com)

1. Metadata
Vår metadata finns på följande länk (det räcker med att maila över länken)
`https://YOUR_DOMAIN/samlp/metadata?connection=YOUR_CONNECTION_NAME`

2. Trusted sites
Visma behöver även en lista på trusted sites som får användas för callbacks. Ex:
`https://signup.agronod.com`

## Miljöer
Visma erhåller en test-miljö och en produktions-miljö. För SAML behöver följande matas in:

|Inställning|Test |Prod  |
|--|--|--|
|Sign-in URL  | https://idp-test.ciceron.cloud/authenticate.request?system=agronod  | https://idp.ciceron.cloud/authenticate.request?system=agronod |
|X509 Signing Certificate |Länk till certifikat*: https://idp-test.ciceron.cloud/metadata?system=agronod&type=IdP  |Länk till certifikat*: https://idp.ciceron.cloud/metadata?system=agronod&type=IdP  |
|Protocol Binding  |HTTP-POST  |HTTP-POST  |

\* Ett X509-certifikat behöver matas in (på länken ovan finns certifikatet som en base64-encoded string). För att göra om strängen till ett certifikat som går att ladda upp i Auth0:
- Ladda ner följande [exempel-textfil](/.attachments/Certificate-b4c4fffa-33d5-4513-abf5-2f552aec1f7a.txt).
- Klistra in den base64-encoded sträng som finns på ovanstående länk där det står <PASTE THE CERT HERE!>
- Spara filen som *.cer

## REST API
 
För att använda sig av Vismas REST API:er finns följande dokumentation tillgänglig:
[Visma TS-REST API.pdf](Visma%20TS-REST%20API.pdf)

## Test bankId för utveckling

1. Skaffa ett bankId för test: https://www.bankid.com/utvecklare/test/skaffa-testbankid/test-bankid-get

2. Logga in med personlig bankId produktion

3. Ladda ned bankId på fil och sedan installera

4. Konfiguera och följ anvisningar: https://www.bankid.com/utvecklare/test/skaffa-testbankid/testbankid-konfiguration. Skapa en ny fil i BankId/Config som heter CavaServerSelector.txt och innehåller texten "kundtest". Glöm inte att skapa en tom rad efter "kundtest"

5. Aktivera