SDN_RuleSetGenerator: Generating OpenFlow rules for Modern Enterprise Networks
===============================================
- [Objective](#Objective)
- [Introduction](#introduction)
- [Installation](#installation)
- [User Guide](#user-guide)
- [Commands](#commands)
- [References](#references)


## Objective
Generate set of OpenFlow rules that reflects current policies of Modern enterprise networks and data center networks.

## Motivation

Enterprise networks are governed by network policies[1]. Network operators compose these policies through rules. Modern enterprise networks and data center networks are evolving and they impose a variety of constraints on point-to-point network communication[1]. In addition, due to increasing use of Network functions e.g., Distributed firewalls, Load balancer, Intrusion prevention system (IPS) in modern network, these polcies are being enormously complex. They may need rules with chaining in actions (e.g. load balance -> firewall -> IPS), wildcard rules, additional matching fields (e.g., MPLS labels).

For the simulations purpose in most  research works, Conventional ClassBench is being used to reflect the policies of Enterprise Networks. However, Conventional ClassBench rules are exact rules and they are based on the 5 tuples matching fields[2]. Moreover, the rules can not support chaining in actions. Therefore, rules generated by Conventional ClassBench may not reflect the Current Policies of Modern Enterprise Networks & Data center Networks. This **bottleneck** becomes a motivation behind the SDN RuleSetGenerator.


##Introduction

SDN RuleSetGenerator tool is divided into 2 separate tools: PolicyGenerator and RuleGenerator. From the characteristics of modern enterprise networks, PolicyGenerator generates variety of complex policies such as reachability, measurement and security policies. RuleGenerator uses [Pyretic run-time engine](http://frenetic-lang.org/pyretic/) to compile these policies and composes OpenFlow rules on the Switch. 
By utilizing pyretic's parallel and sequential compolsition, RuleGenerator can generate wildcard rules, add additional matching fields (e.g., MPLS label), rules with action chaining and install them on OpenFlow switches.  

**PolicyGenerator** module has three python scripts that can be executed on command line to generate policies: 
- generateReachabilityPolicies - It generates reachability policies for each host in the network. Each host belongs to 1 reachability policy unit in the network. 
- generateMeasurementPolicies - By executing this script, one can generate measurement policies.
- generateOverlappedPolicies - It generates overlapped reachibility and measurement policies. 

**RuleGenerator** tool resides in pyretic subfolder, **3 pyretic applications** (OverlappingPoliciesToRules, reachabilityPoliciesToRules and measurementPoliciesToRules) composes SDN policies created by policy generator on OpenvSwitch in mininet via pyretic run-time engine.

##Installation

To install and run the tool, you need to have linux machine. Next, Fork and clone the repository in your machine through:

    wget https://github.com/vishleshpatel/SDN_RuleSetGenerator
    
    
##User-guide

To generate rules, first you need to use policy generator to create policies and then you this output as input for the rule generator pyretic application.

To generate overlapped reachability and measurement policies, run overlapping policy generator tool like this:

    username@ubuntu:~/SDN_RuleSetGenerator$ python3 ./SDN_PolicyGenerator/generateOverlappedPolicies.py 
    
the command assumes your current directiry is SDN_RuleSetGenerator. This command will will write both reachablity policies and measurement policies into the overlappedReachabilityPolicies.txt and overlappedMeasurementPolicies.txt file in the current directory.

Once, these 2 .txt files generated, it will used to generate rules. Rules can be generated using OverlappingPoliciesToRules.py. Command for this is as follows:

    username@ubuntu:~/SDN_RuleSetGenerator/pyretic$ ./pyretic.py -m p0 pyretic.modules.OverlappingPoliciesToRules

the command assumes your current directory is /home/username/SDN_RuleSetGenerator/pyretic. 

Note: Policy generator scripts imports another module's class method to perform several actions. So, you need to modify the system path append line (**sys.path.append**) in generateOverlappedPolicies.py as per the path of SDN_RuleSetGenerator folder in your machine.

you may also need to set path variables first before using rule generator pyretic application like this:

    export PATH=$PATH:/home/yourusername/pox
    export PYTHONPATH=/home/yourusername/pox:/home/yourusername/SDN_RuleSetGenerator/pyretic

##Commands




##References
1. Theophilus Benson, Aditya Akella and David A. Maltz - "Mining Policies From Enterprise Network Configuration"
2. David E. Taylor, Jonathan S. Turner - "ClassBench: A Packet Classification Benchmark"
3. Christopher Monsanto, Joshua Reich, Nate Foster, Jennifer Rexford, David Walker - "Composing Software-Defined Networks"
4. http://frenetic-lang.org/pyretic/

