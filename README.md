# Chaos Manager

Chaos Manager is an automated fault injection module for OMNETPP. It was inspired by [chaos engineering principles](https://principlesofchaos.org/) used in distributed systems. 
Chaos Manager has been used in conjuction with the [CSMA-LEACH](https://github.com/Agr-IoT/LEACH) simulation model.

Chaos Manager is based on some functionality provided by [INET Scenario Manager](https://inet.omnetpp.org/docs/users-guide/ch-scenario-scripting.html). However it is standalone module that is not dependent on ScenarioManager. The current implementation of Chaos Manager is capable of the following:

- Random injection of hard faults into simulation (Shutdowns, Crashes)
- Intelligent detection of simulation parameters from .ini file (Number of nodes, simulation duration)
- Intelligent fault allocation avoiding duplicates (eg: a crash occuring to same node after shutdown).

Future features include:

- Intelligent restarting of nodes that have shutdown.

Limitations of Chaos Manager include:

- Faults are limited to hard faults (Shutdowns, Crashes)
- Faults are limited to only 2 hard fault types provided by LifeCycleManager
- Some parameters have been hardcoded into .cc files in current version.


Follow the below steps to run Chaos Manager module.

1. Prerequisites

2. Install and Build

3. Usage

## 1. Prerequisites

These models have been tested on the following versions of INET and OMNeT++.

- OMNeT++ version 5.6.2

- INET Framework version 4.2.5

## 2. Install and Build

Follow the following procedure to install and build the models.

1. Install and build [OMNeT++](https://omnetpp.org)

2. Create a new workspace in the OMNeT++ IDE

3. Install and build the [INET Framework](https://inet.omnetpp.org) in the created workspace

4. Locate the `common` directory in your INET installation.

5. Create subdirectory named `chaos` and clone or copy the contents of this repository in the newly created `chaos` subdirectory.

6. Rebuild INET.

## 3. Usage

1. Import the module into the `.NED` file of your network simulation

```
import inet.common.chaos.ChaosManager;
```

2. Add Chaos Manager as a submodule in your simulation

```
submodules:
    chaosManager: ChaosManager;
    ...
```

3. The current version of Chaos Manager exposes 2 parameters via the `.NED` file.

- `numNodes` - The number of nodes in a simulation
- `faultPerc` - The number of faulty nodes as a percentage

    `numNodes` can be accessed via the `.ini` file as follows:
    ```
    *.chaosManager.numNodes = 10
    ```
    One must make sure this number is exactly the same as the number of nodes deployed in the network.

    `numNodes` can also be assigned with a `.NED` parameter as follows:
    ```
    parameters:
        int numNodes;
        ...
    submodules:
		chaosManager: ChaosManager {
		    numNodes = numNodes;
		}
        ...
    ```

    `faultPerc` has a default value of 10, but can be overriden in the `.ini` file.

    ```
    *.chaosManager.faultPerc = 5
    ```

    A value that has not been exposed is the naming convention of nodes. This has been hardcoded into the `ChaosManager.cc` file.
    The relevant line has been commented to make identification easier. This will be amended in future as a NED parameter.

    For correct operation the module assumes that `sim-time-limit` parameter is filled in the respective simulation `.ini` file. Code changes are needed in the case that this is not present.

## License

This module is released under LGPL v3.0 license.

## Questions or Comments

If you have any questions, comments or suggestions, please include them as an issue in this repository.
Please be mindful to adhere to the issue template provided and add a reply when an issue is resolved.

Resolved issues will be closed at the maintainers discretion once identified as stale.

## Model Developers

This model was designed and  developed by,

  - [Nuwan Jayawardene](https://github.com/n-jay)