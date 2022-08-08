# marsara Monitor Application Runtimes, Stop Arbitrary Repartitioning Attacks.
This is a prototype for Linux implemented by the authors of the paper, Validating the Integrity of Audit Logs Against Execution Repartitioning Attacks.

This document work is done by Yan Tingzhen, who is not one of the authors of MARSARA.

The original project can be found at https://github.com/nouredd2/marsara or https://github.com/carter-yagemann/MARSARA.

This project is made up with 7 directories: dotgraph, event, parsers, provgraph, record, tracker and utils, whose functions will be introdeced below(accordting to my understanding).

dotgraph: DotGraph and its data structures, for painting the provenance graph.
event: Data structures of some kinds of events, mainly those for PT events.
parsers: Readers, parsers, validators and partitioners. The main functions of validation and partition are achieved here. Besides some important files, there are three subdirectories.
  jgraph: The data structure of omegalog graph and a GraphParser to read graph from a file in json format.
  jparser: JValidator(important) and so on.
  partitioner: JPartitioner(important) and so on.
provgraph: Data structures of provenance graph.
record: Event records in execution units. UnitManager
tracker: Entrance of MARSARA. Agamotto
utils: utils.

To start with, go to the main function in tracker/Agamotto.java. Jump with the functions called in it. In the construction function, a PTAnalyzer is initialized with arguments input. In tracker/PTAnalyzer.java, we can see that during the initialization of PTAnalyzer, pt event sequence ptSeq, omegalog graph omGraph and audit logs are parsed from each one's corresponding files. As for where do these files come from, I have no idea from this open source code.
Next, in generateGraphs in tracker/Agamotto.java, almost all the work of validation and partition is done in ptAnalyzer.analyzeTrace(). Let's go back into PTAnalyzer.java to see what analyzeTrace looks like.
Note that in audit logs, events from different processes or threads are intertwined. So this part first clears events out from different processes into their own process by buildPerPIDLists to form a map, pidEvents.
Then the following part can be found in my ppt in the format of a flow chart.
The validation and checking whether to partition part is in consumeEvent function in JValidator.java, while the start new execution part is in unitManager. All these logics can be found in my ppt for corresponding flow charts.
After the partition of execution unit, agamotto draws each path in each execution unit independently.
