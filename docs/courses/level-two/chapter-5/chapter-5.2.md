---
contentType: tutorial
---

# Workflow 2: Generating reports

In this workflow, you will merge data from different sources, transform binary data, generate files, and send notifications about them. The final workflow should look like this:

<figure><img src="/_images/courses/level-two/chapter-five/workflow2.png" alt="" style="width:100%"><figcaption align = "center"><i>Workflow 2 for aggregating data and generating files</i></figcaption></figure>

To make things easier, let's split the workflow into three parts.

## Part 1: Getting data from different sources

The first part of the workflow consists of five nodes:

<figure><img src="/_images/courses/level-two/chapter-five/workflow2_1.png" alt="" style="width:100%"><figcaption align = "center"><i>Workflow 1 – Getting data from different sources</i></figcaption></figure>

1. Use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest/){:target="_blank" .external} to get data from the API endpoint that stores company data. Configure the following node parameters:

      * **Authentication**: Header Auth
      * **Credentials for Header Auth**: The Header Auth name and Header Auth value you received in the email when you signed up for this course.
      * **URL**: The Dataset URL you received in the email when you signed up for this course.
      * **Options > Add Option > Split Into Items**: toggle to true.
      * **Headers > Add Header**:
          * **Name**: `unique_id`
          * **Value**: The unique ID you received in the email when you signed up for this course.

2. Use the [Airtable node](/integrations/builtin/app-nodes/n8n-nodes-base.airtable/){:target="_blank" .external} to list data from the `customers` table (where you updated the fields `region` and `subregion`).
3. Use the [Merge node](/integrations/builtin/core-nodes/n8n-nodes-base.merge/){:target="_blank" .external} to merge data from the Airtable and HTTP Request node, based on the common key `customer ID`.
4. Use the [Sort](/integrations/builtin/core-nodes/n8n-nodes-base.sort/) node to sort data by `orderPrice` in descending order.

/// question | Quiz questions
* What's the name of the employee assigned to customer 1?
* What's the order status of customer 6?
* What's the highest order price?
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
        "id": "e5ae1927-b531-401c-9cb2-ecf1f2836ba6",
        "name": "Airtable",
        "type": "n8n-nodes-base.airtable",
        "typeVersion": 2,
        "position": [
            1040,
            740
        ],
        "credentials": {
            "airtableTokenApi": {
            "id": "MIplo6lY3AEsdf7L",
            "name": "Airtable Personal Access Token account 4"
            }
        }
        },
        {
        "parameters": {},
        "id": "c0236456-40be-4f8f-a730-e56cb62b7b5c",
        "name": "When clicking \"Test workflow\"",
        "type": "n8n-nodes-base.manualTrigger",
        "typeVersion": 1,
        "position": [
            780,
            600
        ]
        },
        {
        "parameters": {
            "url": "https://internal.users.n8n.cloud/webhook/level2-erp",
            "authentication": "genericCredentialType",
            "genericAuthType": "httpHeaderAuth",
            "sendHeaders": true,
            "headerParameters": {
            "parameters": [
                {
                "name": "unique_id",
                "value": "recFIcD6UlSyxaVMQ"
                }
            ]
            },
            "options": {}
        },
        "id": "cc106fa0-6630-4c84-aea4-a4c7a3c149e9",
        "name": "HTTP Request",
        "type": "n8n-nodes-base.httpRequest",
        "typeVersion": 4.1,
        "position": [
            1040,
            480
        ],
        "credentials": {
            "httpHeaderAuth": {
            "id": "qeHdJdqqqaTC69cm",
            "name": "Course L2 Credentials"
            }
        }
        },
        {
        "parameters": {
            "mode": "combine",
            "mergeByFields": {
            "values": [
                {
                "field1": "customerID",
                "field2": "customerID"
                }
            ]
            },
            "options": {}
        },
        "id": "1cddc984-7fca-45e0-83b8-0c502cb4c78c",
        "name": "Merge",
        "type": "n8n-nodes-base.merge",
        "typeVersion": 2.1,
        "position": [
            1280,
            600
        ]
        },
        {
        "parameters": {
            "sortFieldsUi": {
            "sortField": [
                {
                "fieldName": "orderPrice",
                "order": "descending"
                }
            ]
            },
            "options": {}
        },
        "id": "2f55af2e-f69b-4f61-a9e5-c7eefaad93ba",
        "name": "Sort",
        "type": "n8n-nodes-base.sort",
        "typeVersion": 1,
        "position": [
            1500,
            600
        ]
        }
    ],
    "connections": {
        "Airtable": {
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
        "When clicking \"Test workflow\"": {
        "main": [
            [
            {
                "node": "HTTP Request",
                "type": "main",
                "index": 0
            },
            {
                "node": "Airtable",
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
                "index": 0
            }
            ]
        ]
        },
        "Merge": {
        "main": [
            [
            {
                "node": "Sort",
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

## Part 2: Generating file for regional sales

The second part of the workflow consists of five nodes:

<figure><img src="/_images/courses/level-two/chapter-five/workflow2_2.png" alt="" style="width:100%"><figcaption align = "center"><i>Workflow 2 – Generating file for regional sales</i></figcaption></figure>

1. Use the [IF node](/integrations/builtin/core-nodes/n8n-nodes-base.if/){:target="_blank" .external} to filter order from the region Americas.
2. Use the [Convert to File node](/integrations/builtin/core-nodes/n8n-nodes-base.converttofile/){:target="_blank" .external} to transform the incoming data from JSON to binary format. Select the 'Each Item to Separate File' option.
3. Use the [Read/Write Files from Disk node](/integrations/builtin/core-nodes/n8n-nodes-base.filesreadwrite/){:target="_blank" .external} to create and store files with the orders information. In the File Name field, use an expression to include the order id in the file name, like this: `report_orderID{order_id}.json` (you need to replace the `{order id}` with the reference the Move Binary Data node).
4. Use the [Gmail node](/integrations/builtin/app-nodes/n8n-nodes-base.gmail/){:target="_blank" .external} (or another email node) to send the files using email to an address you have access to. Note that you need to add an attachment with the data property.
5. Use the [Discord node](/integrations/builtin/app-nodes/n8n-nodes-base.discord/){:target="_blank" .external} to send a message in the n8n Discord channel `#course-level-two`. In the node, configure the following parameters:
    * Webhook URL: The webhook URL you received in the email when you signed up for this course.
    * Text: "I sent the file using email with the label ID `{label ID}` and wrote the binary file `{file name}`. My ID: " followed by your ID. <br/> Note that you need to replace the text in curly braces `{}` with expressions that reference the data from the nodes.

/// note | Read/Write Files from Disk
This node is only available on self-hosted instances, and not on n8n Cloud.
///

/// question | Quiz questions
* How many orders are assigned to the region Americas?
* What's the total price of the orders in the region Americas?
* How many items are returned by the *Write Binary File node*?
///

## Part 3: Generating files for total sales

The third part of the workflow consists of seven nodes:

<figure><img src="/_images/courses/level-two/chapter-five/workflow2_3.png" alt="" style="width:100%"><figcaption align = "center"><i>Workflow 3 – Generating files for total sales</i></figcaption></figure>

1. Use the [Loop Over Items node](/integrations/builtin/core-nodes/n8n-nodes-base.splitinbatches/){:target="_blank" .external} to split data from the Item Lists node into batches of 5.
2. Use the [Set node](/integrations/builtin/core-nodes/n8n-nodes-base.set/){:target="_blank" .external} to set four values, referenced with expressions from the previous node: `customerEmail`, `customerRegion`, `customerSince`, and `orderPrice`.
3. Use the [Date & Time node](/integrations/builtin/core-nodes/n8n-nodes-base.datetime/){:target="_blank" .external} to change the date format of the field `customerSince` to the format MM/DD/YYYY.
4. Use the [Spreadsheet File node](/integrations/builtin/core-nodes/n8n-nodes-base.spreadsheetfile/){:target="_blank" .external} to create a CSV spreadsheet with the file name set as the expression: `{{$runIndex > 0 ? 'file_low_orders':'file_high_orders'}}`.
5. Use the [Discord node](/integrations/builtin/app-nodes/n8n-nodes-base.discord/){:target="_blank" .external} to send a message in the n8n Discord channel `#course-level-two`. In the node, configure the following parameters:
    * Webhook URL: The webhook URL you received in the email when you signed up for this course.
    * Text: "I created the spreadsheet `{file name}`. My ID:" followed by your ID. <br/> The `{file name}` should be an expression that references data from the Spreadsheet File node.<br/>

/// question | Quiz questions
* What's the lowest order price in the first batch of items?
* What's the formatted date of customer 7?
* How many items are returned by the *Spreadsheet File node*?
///
