Comp-Nix-Check-CPU-Memory

> Collect the CPU and memory stats of a Linux server

## Versions

| Version | Author | Comment |
| :----: | :----- | :----- |
| `1.0.0` | Umar Butt | Create automation. |

## Requirements

The following variables need to be set when calling this component as a linked automation:

| Variable    | IN/OUT   | Required | Comment                                   |
| ----------- | -------- | -------- | ----------------------------------------- |
| `MSG_STD`   | IN & OUT | Yes      | Standard output message JSON array.       |
| `MSG_ERR`   | IN & OUT | Yes      | Error output message JSON array.          |
| `EXIT_CODE` | OUT      | Yes      | Standard automation return code variable. |


## Execution

This component automation should be called from a SOP automation.

## Actions

The script/markup files in source control.

| Name                                   | Description |
| -------------------------------------- | ----------- |
| [Construct_Json.js](./Construct_Json.js) | Creates a JSON object containing details of CPU and memory stats. |
| [Create_Report.js](./Create_Report.js) | Generate a report of all services to be added to `MSG_STD` object (for ticket update). |

<div class="page-break"><!-- used for page break in PDF document --></div>

## Details

The automation will run a set of commands to return processes that are the most CPU intensive, the most memory intensive, and a measure of the memory use overall. An active `host_connection` connection should also be passed in (created by `Hand-Gen-Device-Connection`) along with all associated connection variables.

The automation will extract outputs of the commands and combine these into a JSON object as variable `cpu_mem_json`, a sample of which is shown below:

```json
[
  {
    "mem_top_procs": [
      {
        "s": "S",
        "shr": "1952",
        "mem": "0.1",
        "res": "4268",
        "pid": "6939",
        "cpu": "3.9",
        "user": "nobody",
        "virt": "197m",
        "command": "garbd",
        "time": "2919:44",
        "ni": "0",
        "pr": "20"
      },
    ],
    "mem_utilisation": {
      "memory": {
        "free": "3300",
        "shared": "0",
        "total": "7232",
        "used": "3931",
        "cached": "2253",
        "buffers": "128"
      },
      "swap": {
        "free": "1934",
        "total": "2047",
        "used": "113"
      }
    },
    "cpu_top_procs": [
      {
        "s": "R",
        "shr": "1084",
        "mem": "0.0",
        "res": "1504",
        "pid": "4876",
        "cpu": "8.5",
        "user": "ipautoma",
        "virt": "30220",
        "command": "top",
        "time": "0:00.09",
        "ni": "0",
        "pr": "20"
      },
    ]
  }
]
```

<div class="page-break"><!-- used for page break in PDF document --></div>

The following properties are included in the object:

| Name | Comment |
| :----- | :---- |
| `mem_top_procs` | Top 15 memory consuming processes. |
| `mem_utilisation` | Memory utilisation statistics (broken down further into general `memory` and `swap` usage). |
| `cpu_top_procs` | Top 15 CPU consuming processes. |

If the `MSG_STD` variable is passed in and out of the component, then a ticket summary will be added in the format below:

![ticket_update_1.png](media/ticket_update_1.png)
