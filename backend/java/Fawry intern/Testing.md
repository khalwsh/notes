application forms of modules each module is a group of function
![[Pasted image 20250624100029.png]]

steps to build a software
![[Pasted image 20250624100355.png]]

Issues fixing cost
![[Pasted image 20250624102744.png]]

what is Maven?
it is a uniform build system 
- simple project setup
- Dependency management
- Extensible (plugins)
- easy to work with multiple projects/modules
- start with ready made templates (archtypes)
- maintain project quality (run tests)
- support multiple profiles

Maven project structure
![[Pasted image 20250624104309.png]]

pom file is an xml file that provide meta data for maven , maven get the dependences from maven central repository and save it in local repo , in-order not to install every time
![[Pasted image 20250624104405.png]]

Maven life cycle
![[Pasted image 20250624104912.png]]

when Mvn apply unit test it search for class that start with Test then any name and run it until the test fails or the test pass
![[Pasted image 20250624105239.png]]

Unit test
test a small piece of code (usually a function) independently from other parts
![[Pasted image 20250624105807.png]]

Unit - testing principles 
tests should be repeatable , should not depend on any data in the environment/instance and has deterministic results , Each test should setup or arrange it's own data

Principles (F.I.R.S.T)
- fast
- isolated / independent
- repeatable
- self-validating
- Thorough

Test coverage
![[Pasted image 20250624131146.png]]

