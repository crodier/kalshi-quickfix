### QuickFixJ -  w/ Kashi specific Tags in the Dictionary
Pushing a branch of quickfix-j

### Maven Steps to Build

```shell
# Need Maven 3.9.9

# Build the "code generator" (one time step)
mvn clean install -pl quickfixj-codegenerator,quickfixj-dictgenerator,quickfixj-class-pruner-maven-plugin -am -DskipTests -DskipAT=true -Dmaven.javadoc.skip=true -Dmaven.site.skip=true -f pom.xml

# Build the 'orchestrate' plugin, (one time step)
mvn install -f pom.xml

# build the 'quickfixj-messages-all-SNAPSHOT-3.0.0.jar'
mvn package -DskipTests -DskipAT=true -Dmaven.javadoc.skip=true -Dmaven.site.skip=true -f pom.xml

# install as version 4.0.0 to avoid any ambiguity with default 3.0.0
mvn install -Dproject.version=4.0.0 -DskipTests -DskipAT=true -Dmaven.javadoc.skip=true -Dmaven.site.skip=true -f pom.xml
```

### Steps to build in IntelliJ

These runners kick off Maven from IntelliJ, and mimic the steps in shell above.

1. Run the "INSTALL CODE GENERATOR" intellJ runner
1. Run the "2nd Install Orchestration" intelilJ runner
2. Run the "LASTLY - quickfix..." intelliJ runner
3. Run the "Install to push to local Maven Repo, and can reference"

```xml
    <dependency>
        <groupId>org.quickfixj</groupId>
        <artifactId>quickfixj-messages-all</artifactId>
        <version>3.0.0-SNAPSHOT</version>
    </dependency>
```


### Code Changes (Notes)

1. Modifying **“FIX50SP2.modified.xml”** with Kalshi specific fields
2. Modifying **"FIX1.1T.xml"** with Kalshi header fields added to Logon
1. See the GIT history for which fields have been added
1. Adding a “runner” to IntelliJ; which shows up in the “runners” when the project is checked out.
1. This “runner” defines options to avoid java-docs, unit tests, which are slow to build.
   1. Building the project will produce a new
      1. “quickfixj-messages-all.jar”
      1. SNAPSHOT-3.0.0
      1. This can be “installed” to the local maven repo.
   1. Jenkins can make a numbered release for this; push to artifactory or whatever tooling is in use.

### Kalshi Question / Problems

#### Problem 1 - 21003 re-used

1. On one Drop Copy Tag (not urgent)
**Tag 21003** is duplicated Ctrl-F in PDF for this tag) for 
   1. **SkipPendingExecReports** and also
   1. **ResentEventCount.**
   
For now, I comment out the *ResetEventCount* as it is catastrophe drop copy related, not needed Day 1.

There may be a workaround; however, it is inconvenient to have this field number overloaded.

#### Problem 2 - MiscFees should be MiscFeesGrp

        <component name="MiscFeesGrp" required="N"/>

It seems you want the MiscFeesGrp on the MarketSettlementMessage; not one list of fees.

#### Problem 3 - Small Reason vs. Type typo in Spec

PDF Typo?

Note: There's a typo in the PDF where tag 532 is labeled as 
"MassCancelRequest**Type**" 

But the description says "Indicates why Order Mass Cancel Request was rejected"

Should this be MassCancelReject**Reason**?


