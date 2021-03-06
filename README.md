PolicyBench: A SDN Data Plane Policy Generator
===============================================
- [Introduction](#introduction)
- [Installation](#installation)
- [User Guide](#user-guide)
- [Commands](#commands)
- [References](#references)


## Introduction

PolicyBench simulates the data plane policy semantic of SDN with different network management applications. It can be used for following purposes:

1. benchmarking SDN flow table management schemes,

2. generating policy workloads for research on SDN policy enforcements,

3. analyzing the semantic characteristic of SDN data plane policies.

As a start, PolicyBench generates rules for 3 kinds of SDN Applications: Reachability, Measurement and Security applications. It does that with the help of 2 correlated tools: PolicyGenerator and RuleGenerator. 

PolicyGenerator: From the given characteristics of enterprise network topology, PolicyGenerator generates reachability, measurement and security policies. It then feds this policies to RuleGenerator.

RuleGenerator: RuleGenerator uses [Pyretic run-time engine](http://frenetic-lang.org/pyretic/) to compile these policies and composes OpenFlow rules on the Switch. By utilizing pyretic's parallel and sequential compolsition, RuleGenerator can generate wildcard rules, add complex matching fields (e.g., MPLS label), rules with action chaining and can install them on OpenFlow switches. 

Our project is motivated by ClassBench - a packet classification benchmark. Conventional ClassBench is being used to reflect the policies of Enterprise Networks for simulations in most research work. However, Conventional ClassBench rules are exact rules and they are based on the 5 tuples matching fields[2]. The rules also can not support chaining in actions. In the modern network infrastructure, we analyze the use of wildcard rules, additional matching fields other than the standard 5-tuples (e.g., MPLS label, VLAN tags) and action chaining. Therefore, rules generated by Conventional ClassBench may not reflect the Current Policies of Modern Enterprise Networks & Data center Networks. This **bottlenecks** are a motivation to develop a new tool: PolicyBench.

##Installation

To install and run the tool, you need to have linux machine. Next, Fork and clone the repository in your machine through:

    wget https://github.com/vishleshpatel/PolicyBench
    
    
##User-guide

To generate rules, first you need to use policy generator to create policies and then use this output as input for the rule generator application.

To generate overlapped reachability and measurement policies, run overlapping policy generator tool like this:

    username@ubuntu$ python ./PolicyGenerator/generateOverlappedPolicies.py 
    
This command will write both reachablity policies and measurement policies into the overlappedReachabilityPolicies.txt and overlappedMeasurementPolicies.txt file in the current directory.

Once, these 2 policy files generated, it will be used to generate rules. Rules can be generated using OverlappingPoliciesToRules.py. Command for this is as follows:

    username@ubuntu$ ./pyretic.py -m p0 pyretic.modules.OverlappingPoliciesToRules

the command assumes your current directory is /home/username/PolicyBench/pyretic. 

Note: Policy generator scripts imports another module's class method to perform several actions. So, you need to modify the system path append line (**sys.path.append**) in generateOverlappedPolicies.py as per the path of SDN_RuleSetGenerator folder in your machine.

you may also need to set path variables first before using rule generator pyretic application like this:

    export PATH=$PATH:/home/yourusername/pox
    export PYTHONPATH=/home/yourusername/pox:/home/yourusername/PolicyBench/pyretic

##Commands




##References
1. Theophilus Benson, Aditya Akella and David A. Maltz - "Mining Policies From Enterprise Network Configuration"
2. David E. Taylor, Jonathan S. Turner - "ClassBench: A Packet Classification Benchmark"
3. Christopher Monsanto, Joshua Reich, Nate Foster, Jennifer Rexford, David Walker - "Composing Software-Defined Networks"
4. http://frenetic-lang.org/pyretic/

