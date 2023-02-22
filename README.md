# Outlier Detector

|                |                                                                       |
| -------------- | --------------------------------------------------------------------- |
| Name           | Outlier Detector                                                      |
| Version        | v2.0.0                                                                |
| Dockerhub Link | [weevenetwork/cleaner](https://hub.docker.com/r/weevenetwork/cleaner) |
| authors        | Jakub Grzelak                                                         |

- [Outlier Detector](#outlier-detector)
  - [Description](#description)
  - [Environment Variables](#environment-variables)
    - [Module Specific](#module-specific)
    - [Set by the weeve Agent on the edge-node](#set-by-the-weeve-agent-on-the-edge-node)
  - [Module Testing](#module-testing)
  - [Dependencies](#dependencies)
  - [Input](#input)
  - [Output](#output)

## Description

Outlier Detector is a processing module responsible for data sanitization and anomaly detection of data passing through weeve data services.
Outlier Detector checks if received data are within constraints associated with some maximum and minimum acceptance value or change rate.
This module is containerized using Docker.

## Environment Variables

### Module Specific

The following module configurations can be provided in a data service designer section on weeve platform:

| Environment Variables          | type   | Description                                                                                                      |
| ------------------------------ | ------ | ---------------------------------------------------------------------------------------------------------------- |
| UPPER_THRESHOLD                | float  | Any data points exceeding this threshold should be considered outliers.                                          |
| LOWER_THRESHOLD                | float  | Any data points below this threshold should be considered outliers.                                              |
| RATE_OF_CHANGE_UPPER_THRESHOLD | float  | Any data point where the rate of change (per second) is greater than this value should be considered an outlier. |
| RATE_OF_CHANGE_LOWER_THRESHOLD | float  | Any data point where the rate of change (per second) is less than this value should be considered an outlier.    |
| OUTLIER_POLICY                 | string | What to do with outliers: keep, remove, smooth                                                                   |
| INPUT_LABEL                    | string | The input label on which to detect outliers.                                                                     |

### Set by the weeve Agent on the edge-node

Other features required for establishing the inter-container communication between modules in a data service are set by weeve agent.

| Environment Variables | type   | Description                                              |
| --------------------- | ------ | -------------------------------------------------------- |
| MODULE_NAME           | string | Name of the module                                       |
| MODULE_TYPE           | string | Type of the module (Input, Processing, Output)           |
| LOG_LEVEL             | string | Level of logging (DEBUG, INFO, WARNING, ERROR, CRITICAL) |
| EGRESS_URLS           | string | HTTP ReST endpoints for the next modules                 |
| INGRESS_HOST          | string | Host to which data will be received                      |
| INGRESS_PORT          | string | Port to which data will be received                      |

## Module Testing

To test module navigate to `test` directory. In `test/assets` edit both .json file to provide input for the module and expected output. During a test, data received from the listeners are compared against expected output data. You can run tests with `make run_test`.

## Dependencies

```txt
bottle
requests
```

## Input

Input to this module is:

* JSON body single object, example:

```json
{
    "temperature": 15,
}
```

* array of JSON body objects, example:

```json
[
    {
        "temperature": 15,
    },
    {
        "temperature": 17,
    }
]
```

## Output

Output of this module is as follows and depending on cleaning settings.

* JSON body single object, example:

```json
{
    "temperature": 15,
}
```

* array of JSON body objects, example:

```json
[
    {
        "temperature": 15,
    },
    {
        "temperature": 17,
    }
]
```
