# tools_data_modificaiton

# OracleTracker

This project place probes after the instantiation of exceptions in source code and test code.

To generate the jar file, run

```
mvn clean compile assembly:single test-compile
```

change the probe info in visitMethodInsn method in Oraclevisitor to distinguish probes for source code from probes for test code.

To instrument the project source code's bytecode(for maven projects)

```
java -jar "XXXX.jar" target/classes
```

# Modified PIT

For PIT: We choose to report the exception type, lineNumber, FileName, and MethodName on the scene.
We also chose to make some modification in org/pitest/testapi/execute/Pitest.java where we know the test start before it really starts in execution.


Running mutation analysis with "FullMutationMatrix" and "Verbose" mode. The detailed test exceuction inforamtion can be directed to standard error.


# Data
Data1-5 and 6-10 have 10 csv files which contain test run information for all covered mutants labeled as "KILLED" or "SURVIVED" by PIT.

Each line has such fields:
[’mutation_id','mutated_file','mutated_line_number',"mutator","mutation_state",’test_id',"test_state","exception","cause","loc"]

1. “mutation_id” is a unique mutation id for each project, they are given orderly and meaningless  
2. “mutated_file”, ‘mutated_line_number”,”mutator”: describes the mutation  
3. “mutation_state” can be: [“killed”, “survive”, “killed(noResource)”,”extra”]      
 "extra means the mutations that our probes introduce to the project for mutants that are killed due to occupying too much resources, no tests information is available
4. “test_id”: uniquely describes a test in a project “test_state”: [“pass”, “fail”]

5. if a test pass, the last three fields are None, otherwise:
	“exception”: the exception type 
	“cause”: [“exogenous_crash”, “source_oracle”, “test_oracle”]
	“loc”: the description of the immediate stack information

Note: some Exceptions have empty stack trace, so the info in incomplete in “loc”, such as some java.lang.OutofMemoryError
