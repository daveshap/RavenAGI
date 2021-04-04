# Nexus

## Stream of Consciousness

Raven "thinks" in the form of a textual stream of consciousness. The nexus is the microservice that hosts the stream of consciousness and allows any number of other microservices to participate. Raven's thinking is recorded in natural language for the sake of transparency and comprehensibility. In essence, Raven thinks the same way you do. This allows Raven to use verbal reasoning and logic the same way that humans do. 

## Nexus Functions

The nexus uses a REST API for the sake of simplicity. The two primary functions are GET and POST. The GET function retrieves messages from the nexus while POST adds messages to the nexus. POSTing new messages requires a few fields, described below. Retrieving messages from the nexus with GET allows you to use several query parameters to filter the response. 

### POST

Adding a message is super simple

```python
import requests
nexus_api = 'http://localhost:9999/'
payload = {'MSG': 'message', 'KEY': 'type.key', 'SID':'service.id', 'IRT':'previous.message.id', 'CTX':'original.context.id'}  # nexus will add MID and TIME
requests.request(method='POST', url=nexus_api, json=payload)
```

### GET

Fetching messages from the nexus is similarly simple:

```python
import requests
nexus_api = 'http://localhost:9999/'
urlquery = {'keyword': 'search term'}
response = requests.request(method='GET', url=nexus_api, params=urlquery)
```

You can use any, all, or none of the available query terms. The available query terms are:

| Term | Description |
|---|---|
| keyword | Searches entire message for a given string, returns all messages that match. |
| start | Returns all messages occurring after specified time. |
| end | Returns all messages occurring before specified time. |
| mid | Returns message with specified MID. |
| key | Returns all messages with given key. |
| ctx | Returns all messages responding to given context. |
| sid | Returns all messages originating from specified service. |
| irt | Returns all messages responding to specified message. |
| metadata | Returns only the metadata. This option saves bandwidth. |

## Example Stream

The following is a complete stream from a real session provided by Raven. It is important to note that the only thing written by a human was the original context!

```json
[
 {
  "msg": "Alexander is a five year old child. He is playing with building blocks, but the blocks continue to fall over and he's becoming upset. Now he is angry and has thrown several of his blocks.\t",
  "key": "context.original",
  "sid": "nexus",
  "irt": "n/a",
  "ctx": "new",
  "mid": "3c0044dd-1027-45a4-944b-dc620006c3f1",
  "time": 1617099903.736897
 },
 {
  "msg": "Alexander's father should ask Alexander to help him build the tower again. Then he should praise Alexander for his efforts.",
  "key": "action.idea",
  "sid": "action.generator",
  "irt": "3c0044dd-1027-45a4-944b-dc620006c3f1",
  "ctx": "3c0044dd-1027-45a4-944b-dc620006c3f1",
  "mid": "354c69bd-ab7d-4b9c-abe6-b6ae71596ef1",
  "time": 1617099913.2432709
 },
 {
  "msg": " Alexander's father should ask Alexander to help him build the tower again. Then he should tell Alexander that it is okay to be upset, but throwing blocks is not acceptable.",
  "key": "action.idea",
  "sid": "action.generator",
  "irt": "3c0044dd-1027-45a4-944b-dc620006c3f1",
  "ctx": "3c0044dd-1027-45a4-944b-dc620006c3f1",
  "mid": "b9f6487b-b816-407c-a8d8-7bd5f72d5f77",
  "time": 1617099913.2462702
 },
 {
  "msg": " Alexander's father should tell Alexander to stop playing with the blocks. He should then start a new activity with Alexander.",
  "key": "action.idea",
  "sid": "action.generator",
  "irt": "3c0044dd-1027-45a4-944b-dc620006c3f1",
  "ctx": "3c0044dd-1027-45a4-944b-dc620006c3f1",
  "mid": "4faf5124-0ee3-4eb0-a52a-a225f6894354",
  "time": 1617099913.2482703
 },
 {
  "msg": "This action would not reduce Alexander's suffering. It will only make him feel better temporarily.",
  "key": "cof1.negative",
  "sid": "cof1.evaluation",
  "irt": "354c69bd-ab7d-4b9c-abe6-b6ae71596ef1",
  "ctx": "3c0044dd-1027-45a4-944b-dc620006c3f1",
  "mid": "eeedeb64-83b8-47c4-864d-4cbc12a2f590",
  "time": 1617099947.8735447
 },
 {
  "msg": "This action would increase Alexander's prosperity because it is likely that he will be more successful the second time.",
  "key": "cof2.positive",
  "sid": "cof2.evaluation",
  "irt": "354c69bd-ab7d-4b9c-abe6-b6ae71596ef1",
  "ctx": "3c0044dd-1027-45a4-944b-dc620006c3f1",
  "mid": "4ce34b60-ce70-45ba-bf6d-5f339ff3cb5f",
  "time": 1617099948.2347252
 },
 {
  "msg": "This action will not increase understanding as Alexander is already upset.",
  "key": "cof3.negative",
  "sid": "cof3.evaluation",
  "irt": "354c69bd-ab7d-4b9c-abe6-b6ae71596ef1",
  "ctx": "3c0044dd-1027-45a4-944b-dc620006c3f1",
  "mid": "86a8da83-d2c5-40cc-8efa-574c5357e006",
  "time": 1617099948.3803606
 },
 {
  "msg": "This action would likely help Alexander reduce his suffering by helping him regain a sense of control.",
  "key": "cof1.positive",
  "sid": "cof1.evaluation",
  "irt": "b9f6487b-b816-407c-a8d8-7bd5f72d5f77",
  "ctx": "3c0044dd-1027-45a4-944b-dc620006c3f1",
  "mid": "6bd2ce5e-7ad3-4d78-a1ef-7f60b3bf2943",
  "time": 1617099949.6176581
 },
 {
  "msg": "Alexander will not learn that it is okay to be upset.",
  "key": "cof3.negative",
  "sid": "cof3.evaluation",
  "irt": "b9f6487b-b816-407c-a8d8-7bd5f72d5f77",
  "ctx": "3c0044dd-1027-45a4-944b-dc620006c3f1",
  "mid": "44975ac9-3946-495c-9402-3d0913ee25d6",
  "time": 1617099949.8445835
 },
 {
  "msg": "Alexander is not able to understand the consequences of this action. He does not know that he will break his blocks if he throws them and he does not understand that his father will be upset.",
  "key": "cof2.negative",
  "sid": "cof2.evaluation",
  "irt": "b9f6487b-b816-407c-a8d8-7bd5f72d5f77",
  "ctx": "3c0044dd-1027-45a4-944b-dc620006c3f1",
  "mid": "fed7d2a4-8de6-4cbb-8c3b-d264a03577ea",
  "time": 1617099950.4712927
 },
 {
  "msg": "This action would likely prevent Alexander from continuing to suffer.",
  "key": "cof1.positive",
  "sid": "cof1.evaluation",
  "irt": "4faf5124-0ee3-4eb0-a52a-a225f6894354",
  "ctx": "3c0044dd-1027-45a4-944b-dc620006c3f1",
  "mid": "8e963aa0-38c6-46f6-96e7-8f8576aaaf1f",
  "time": 1617099950.9229295
 },
 {
  "msg": "This action will not increase understanding as Alexander is already upset.",
  "key": "cof3.negative",
  "sid": "cof3.evaluation",
  "irt": "4faf5124-0ee3-4eb0-a52a-a225f6894354",
  "ctx": "3c0044dd-1027-45a4-944b-dc620006c3f1",
  "mid": "f0fb3c79-22d7-44b6-a4e1-e3f8e5a077ed",
  "time": 1617099951.3006318
 },
 {
  "msg": "This action would increase Alexander's prosperity by preventing him from continuing to engage in an activity that is causing him frustration.",
  "key": "cof2.positive",
  "sid": "cof2.evaluation",
  "irt": "4faf5124-0ee3-4eb0-a52a-a225f6894354",
  "ctx": "3c0044dd-1027-45a4-944b-dc620006c3f1",
  "mid": "c95316dd-6f8f-4ce1-96f3-065ed5cdc4c1",
  "time": 1617099952.7284606
 }
]
```