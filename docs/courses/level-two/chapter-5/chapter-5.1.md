---
contentType: tutorial
---

# Workflow 1: Merging data

The company's customer data is stored in Airtable. It contains information about the customers' ID, country, email, and join date, but lacks data about their respective region and subregion. You need to fill in these last two fields in order to create the reports for regional sales.

To accomplish this task, you first need to make a copy of this table in your Airtable account:

<iframe class="airtable-embed" src="https://airtable.com/embed/shrNX9tjPkVLABbNz?backgroundColor=orange&viewControls=on" frameborder="0" onmousewheel="" width="100%" height="533" style="background: transparent; border: 1px solid #ccc;"></iframe>

Next, you have to build a small workflow that merges data from Airtable and a REST API.

1. Use the [Airtable node](/integrations/builtin/app-nodes/n8n-nodes-base.airtable/){:target="_blank" .external} to list the data in the Airtable table named `customers`.
2. use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest/){:target="_blank" .external} to get data from the REST Countries API: `https://restcountries.com/v3.1/all`. This will return data about world countries. Note that the incoming data needs to be split into items.
3. Use the [Merge node](/integrations/builtin/core-nodes/n8n-nodes-base.merge/){:target="_blank" .external} to merge data from Airtable and the Countries API by country name (the common key), represented as `customerCountry` in Airtable and `name.common` in the Countries API, respectively.
4. Use the Airtable node to update the fields `region` and `subregion` in Airtable with the data from the Countries API.

The workflow should look like this:

<figure><img src="/_images/courses/level-two/chapter-five/workflow1.png" alt="" style="width:100%"><figcaption align = "center"><i>Workflow 1 for merging data from Airtable and the Countries API</i></figcaption></figure>


/// question | Quiz questions
* How many items does the HTTP Request node return?
* How many items does the Merge node return?
* How many unique regions are assigned in the customers table?
* What's the subregion assigned to the customerID 10?
///

??? note "Show me the solution"

    To check the configuration of the nodes, you can copy-paste the JSON code of the workflow:

    ```json
    {
    "meta": {
        "templateCredsSetupCompleted": true,
        "instanceId": "cb484ba7b742928a2048bf8829668bed5b5ad9787579adea888f05980292a4a7"
    },
    "nodes": [
        {
        "parameters": {},
        "id": "16de0270-c143-4bb7-a30b-ad9454d1ffbc",
        "name": "When clicking \"Test workflow\"",
        "type": "n8n-nodes-base.manualTrigger",
        "typeVersion": 1,
        "position": [
            680,
            540
        ]
        },
        {
        "parameters": {
            "operation": "search",
            "base": {
            "__rl": true,
            "value": "apprtKkVasbQDbFa1",
            "mode": "list",
            "cachedResultName": "All your base",
            "cachedResultUrl": "https://airtable.com/apprtKkVasbQDbFa1"
            },
            "table": {
            "__rl": true,
            "value": "tblInZ7jeNdlUOvxZ",
            "mode": "list",
            "cachedResultName": "Course L2, Workflow 1",
            "cachedResultUrl": "https://airtable.com/apprtKkVasbQDbFa1/tblInZ7jeNdlUOvxZ"
            },
            "options": {}
        },
        "id": "91645956-61c7-474b-a902-3f5973a4a89a",
        "name": "Airtable",
        "type": "n8n-nodes-base.airtable",
        "typeVersion": 2,
        "position": [
            920,
            440
        ],
        "credentials": {
            "airtableTokenApi": {
            "id": "MIplo6lY3AEsdf7L",
            "name": "Airtable Personal Access Token account 4"
            }
        }
        },
        {
        "parameters": {
            "url": "https://restcountries.com/v3.1/all",
            "options": {}
        },
        "id": "e10ec8b0-48b4-407a-8e7e-a89207f70a1a",
        "name": "HTTP Request",
        "type": "n8n-nodes-base.httpRequest",
        "typeVersion": 4.1,
        "position": [
            920,
            660
        ]
        },
        {
        "parameters": {
            "mode": "combine",
            "mergeByFields": {
            "values": [
                {
                "field1": "customerCountry",
                "field2": "name.common"
                }
            ]
            },
            "options": {}
        },
        "id": "2709c9bd-41e3-41b7-b5db-920a9b458a02",
        "name": "Merge",
        "type": "n8n-nodes-base.merge",
        "typeVersion": 2.1,
        "position": [
            1140,
            540
        ]
        },
        {
        "parameters": {
            "operation": "update",
            "base": {
            "__rl": true,
            "value": "apprtKkVasbQDbFa1",
            "mode": "list",
            "cachedResultName": "All your base",
            "cachedResultUrl": "https://airtable.com/apprtKkVasbQDbFa1"
            },
            "table": {
            "__rl": true,
            "value": "tblInZ7jeNdlUOvxZ",
            "mode": "list",
            "cachedResultName": "Course L2, Workflow 1",
            "cachedResultUrl": "https://airtable.com/apprtKkVasbQDbFa1/tblInZ7jeNdlUOvxZ"
            },
            "columns": {
            "mappingMode": "defineBelow",
            "value": {
                "id": "={{ $json.id }}",
                "region": "={{ $json.region }}",
                "subregion": "={{ $json.subregion }}"
            },
            "matchingColumns": [
                "id"
            ],
            "schema": [
                {
                "id": "id",
                "displayName": "id",
                "required": false,
                "defaultMatch": true,
                "display": true,
                "type": "string",
                "readOnly": true,
                "removed": false
                },
                {
                "id": "customerID",
                "displayName": "customerID",
                "required": false,
                "defaultMatch": false,
                "canBeUsedToMatch": true,
                "display": true,
                "type": "number",
                "readOnly": false,
                "removed": false
                },
                {
                "id": "customerCountry",
                "displayName": "customerCountry",
                "required": false,
                "defaultMatch": false,
                "canBeUsedToMatch": true,
                "display": true,
                "type": "string",
                "readOnly": false,
                "removed": false
                },
                {
                "id": "customerEmail",
                "displayName": "customerEmail",
                "required": false,
                "defaultMatch": false,
                "canBeUsedToMatch": true,
                "display": true,
                "type": "string",
                "readOnly": false,
                "removed": false
                },
                {
                "id": "customerSince",
                "displayName": "customerSince",
                "required": false,
                "defaultMatch": false,
                "canBeUsedToMatch": true,
                "display": true,
                "type": "string",
                "readOnly": false,
                "removed": false
                },
                {
                "id": "region",
                "displayName": "region",
                "required": false,
                "defaultMatch": false,
                "canBeUsedToMatch": true,
                "display": true,
                "type": "string",
                "readOnly": false,
                "removed": false
                },
                {
                "id": "subregion",
                "displayName": "subregion",
                "required": false,
                "defaultMatch": false,
                "canBeUsedToMatch": true,
                "display": true,
                "type": "string",
                "readOnly": false,
                "removed": false
                }
            ]
            },
            "options": {}
        },
        "id": "856bd214-7893-4dcc-b347-238efa7834c0",
        "name": "Airtable1",
        "type": "n8n-nodes-base.airtable",
        "typeVersion": 2,
        "position": [
            1360,
            540
        ],
        "credentials": {
            "airtableTokenApi": {
            "id": "MIplo6lY3AEsdf7L",
            "name": "Airtable Personal Access Token account 4"
            }
        }
        }
    ],
    "connections": {
        "When clicking \"Test workflow\"": {
        "main": [
            [
            {
                "node": "Airtable",
                "type": "main",
                "index": 0
            },
            {
                "node": "HTTP Request",
                "type": "main",
                "index": 0
            }
            ]
        ]
        },
        "Airtable": {
        "main": [
            [
            {
                "node": "Merge",
                "type": "main",
                "index": 0
            }
            ]
        ]
        },
        "HTTP Request": {
        "main": [
            [
            {
                "node": "Merge",
                "type": "main",
                "index": 1
            }
            ]
        ]
        },
        "Merge": {
        "main": [
            [
            {
                "node": "Airtable1",
                "type": "main",
                "index": 0
            }
            ]
        ]
        }
    },
    "pinData": {}
    }
    ```