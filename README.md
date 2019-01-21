Note: This is forked from https://github.com/membrane/soa-model and published to Maven central in fix this problem https://github.com/membrane/soa-model/issues/238 (via https://github.com/membrane/soa-model/commit/0705e1c693cd79450fd6b6a31d4e97f6e350aeb2):

SBT:

    libraryDependencies += "com.eluvio" % "soa-model-core" % "1.6.1"

Maven:

    <dependency>
      <groupId>com.eluvio</groupId>
      <artifactId>soa-model-core</artifactId>
      <version>1.6.1</version>
    </dependency>


Membrane SOA Model 
==================
Check the [Repository at GitHub](https://github.com/membrane/soa-model) for the latest source code.
What is SOA Model?
-
Membrane SOA Model is an open-source toolkit and Java API for WSDL and XML Schema, licensed under ASF 2.0. that can:

- Parse, create or modify a WSDL or XML Schema Document from Java
- Compare two WSDL or XML Schema Documents
- Create a SOAP Request or Template
- Analyze a WSDL or Schema document and generate an HMTL report

Command Line Tools
------------------
Membrane SOA Model provides command line tools beside the java api for

- Analyzing a WSDL document and generating a report
- Creating a SOAP request or template out of WSDL
- Comparing two WSDL or Schema documents

All you need is to download and run the batch with commands.

Code Samples
-----------
Here are some examples of the tasks you can achive with SOA Model.
For more details see the [documentation](http://membrane-soa.org/soa-model-doc/).

Parsing a wsdl:

    WSDLParser parser = new WSDLParser();
    Definitions defs = parser.parse("http://ws.xwebservices.com/XWebBlog/V2/XWebBlog.wsdl");

Creating a new wsdl:

    Definitions wsdl = new Definitions("http://predic8.com/wsdl/AddService/1/", "AddService");
    wsdl.add(schema);
    PortType pt = wsdl.newPortType("AddPortType");
    Operation op = pt.newOperation("add");
    op.newInput("add").newMessage("add").newPart("parameters", "tns:add");
    op.newOutput("addResponse").newMessage("addResponse").newPart("parameters", "tns:addResponse");
    return wsdl;
Comparing two wsdls:

    Definitions wsdl1 = parser.parse("resources/diff/1/article.wsdl");
    Definitions wsdl2 = parser.parse("resources/diff/2/article.wsdl");
    WsdlDiffGenerator diffGen = new WsdlDiffGenerator(wsdl1, wsdl2);
    List<Difference> lst = diffGen.compare();
Comparing two schemas:

    Schema schema1 = parser.parse("resources/diff/1/common.xsd"); 
    Schema schema2 = parser.parse("resources/diff/2/common.xsd");
    SchemaDiffGenerator diffGen = new SchemaDiffGenerator(schema1, schema2);
    List<Difference> lst = diffGen.compare();
Creating a soap request:

    Definitions wsdl = parser.parse("resources/article/article.wsdl");
    StringWriter writer = new StringWriter();
    SOARequestCreator creator = new SOARequestCreator(wsdl, new RequestTemplateCreator(), new MarkupBuilder(writer));
    //creator.createRequest(PortType name, Operation name, Binding name);
    creator.createRequest("ArticleServicePT", "create", "ArticleServicePTBinding");
    System.out.println(writer);

Integration Testing
-------------------

To run "mvn integration-test" in an isolated environment, you may run

    docker build .

if you have a Docker Engine available.



# Eluvio Maven Notes

We usually spend are time using SBT so we need a cheatsheet for maven commands.

Sonatype notes for Maven are here: https://central.sonatype.org/pages/apache-maven.html

## To Compile

    JAVA_HOME=/opt/java8 mvn compile
    
## To Test

    JAVA_HOME=/opt/java8 mvn test
    
## To publish a -SNAPSHOT on Sonatype

A `-SNAPSHOT` version will automatically publish to the Snapshots repository

### Stage to Sonatype

    JAVA_HOME=/opt/java8 GPG_TTY=$(tty) mvn clean deploy -P ossrh

## To publish a Release version Sonatype

### Prepare Release

    JAVA_HOME=/opt/java8 GPG_TTY=$(tty) mvn release:clean release:prepare -P ossrh

### Perform Release

    JAVA_HOME=/opt/java8 mvn release:perform -P ossrh
