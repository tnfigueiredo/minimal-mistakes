---
title: "How living documentation and user stories acceptance tests can bring improvements?"
excerpt: >-
  This article brings the benefits of using a BDD approach through user story acceptance criteria 
  to have project improvements.
comments: true
lang: "en"
permalink: /en/2023-06-20-living-doc-bdd
toc: true
categories:
  - Software Engineering
  - BDD
  - Documentation
tags: 
  - software engineering
  - bdd
  - testing
  - tools
  - documentation
---

{% include figure image_path="assets/images/living-doc-bdd/mag-glass-ci-cd.jpg" alt="mag-glass-ci-cd.jpg" %}

In the software development community it is a known fact that keep documentation up to date according to the project requirements 
is something that needs team discipline, project deadlines, corporate policies and processes, and other issues. To keep documentation 
quality aligned with source code quality it is possible to take advantage of a TDD/BDD approach and artifacts generated through CI/CD 
pipelines. Depending on the project situation the teams sometimes choose to focus on delivering a running version of the project 
accepting the trade-off of giving not so much attention to code quality, test coverage, and related topics.

{% include figure image_path="assets/images/living-doc-bdd/why.jpg" alt="why.jpg" %}

Even being aware of the mentioned scenario it is undeniable that projects need documentation to register knowledge related to 
the business domain and also important aspects of the solution itself. When we come to the reality of agile projects it is 
important to take into consideration that documentation might be the right fit for project activities to not become something 
overwhelming and that will be the result of some effort that no one will read.

## BDD and effective software documentation

There are already some recommendations to write effective and useful software documentation in lean/agile approaches. The main 
purpose is to have documentation easy to maintain, composing a knowledge base that can help project members, and also to be a 
facilitator between the team project and stakeholders.

The BDD (Behavior-driven development) was an outspread of TDD (Test-driven development) which started in the early 2000s. The 
main objective was to approach testing and coding, and minimize misunderstandings. It has evolved into both analysis and automated 
testing at the acceptance level.Together with DDD (Domain-Driven Design), it was a useful tool to straighten the communication 
between the tech teams and stakeholders, once it uses natural language to describe acceptance criteria and software requirements. 
The goal is to shorten feedback cycles and eliminate misunderstood or vague requirements using real-world examples.

{% include figure image_path="assets/images/living-doc-bdd/automation-in-business.jpeg" alt="automation-in-business.jpeg" %}

Through the usage of Gherkin it is possible to write a human-readable specification, besides the fact to provide an input to test 
automation given its structure and meaning to executable specifications. In a summary, we can say that having a Gherkin written 
specification it is possible to link automated test steps to provide outputs for test results execution and also have software 
specification documentation based on the Gherkin input files.

Here we come to an important thing to mention: we are not talking about single test reports. The proposal here comes from the fact 
of having application living documentation (Something very different from test reports). While test reports are examples converted 
into executable specifications that developers must pass during their test-driven development cycle; living documentation is a 
human-readable record of each feature verified and kept current via an automated test suite as the final product of a process result.


## Getting living documentation in the software delivery process

All the topics covered by now had the purpose of bringing some background to justify having living documentation in the software 
7delivery process. And it fits perfectly with having an automated software delivery process through CI/CD approaches. Once a project 
has an orchestrated delivery process it is just to take advantage of its good usage to bring to the project automated documentation.

{% include figure image_path="assets/images/living-doc-bdd/CI-CD.png" alt="CI-CD.png" %}

One of the important aspects of CI/CD is bringing visibility to the executed process. Part of this process is addressed to the tech 
team itself, but part of the visibility in the process means making it clear to the stakeholders. From the stakeholder's perspective, 
the living documentation makes it possible to check the features validates and also the business rules covered in the application's 
automated test process.

### User Stories and event mapping

Since talking about living documentation is related to keeping software requirements and software implementation integrated and 
aligned, it is necessary to link how it can be related to the source of an application's requirements. Agile projects will mainly 
register requirements through [User Stories](https://www.atlassian.com/agile/project-management/user-stories). If we think of 
handling it through a Domain Driven Design (DDD) and event driven approach, they are mainly related 
to [Domain Events](https://learn.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/domain-events-design-implementation). 
If there is the purpose of matching project requirements, software deliverables, and living documentation; it is important to 
match requirements artifacts with project coding.

Thinking of a User Story as a description of a business need through a user perspective, the acceptance criteria written in BDD 
fits as a matcher to validate if the those business requirements are met.

{% include figure image_path="assets/images/living-doc-bdd/user-story-acceptance-criteria.png" alt="user-story-acceptance-criteria.png" %}

When we think of Domain Events, the process is not so direct. So it is necessary to translate the dynamic vision of the Domain Events 
resulting from [Event Storming sessions](https://creately.com/blog/meeting-visual-collaboration/event-storming/) to business 
requirements possible to be [validated as an acceptance criteria](https://cucumber.io/blog/bdd/bdd-with-event-mapping/).

{% include figure image_path="assets/images/living-doc-bdd/event-mapping-bdd.jpg" alt="event-mapping-bdd.jpg" %}

In both cases, the main purpose is to create a relationship between the requirements artifacts and the software deliverables 
validation. In the practical example it will become clearer how this relationship can be created.

### Continuous testing to achieve living documentation

Once the team figure it out how to match the business requirements into the software acceptance criteria, the challenge remains 
on how to use tools to make all those things to work properly. In the context of an application life-cycle it means to match 
test frameworks, software specification, software implementation, build resulting artifacts, and CI/CD automated pipelines.

{% include figure image_path="assets/images/living-doc-bdd/ci-ci-pipelines.jpeg" alt="ci-ci-pipelines.jpeg" %}

If it is taken into consideration that agile projects work in CI/CD approach, it is possible to say that they already have a 
structure to introduce steps to produce the living documentation. Automated test process used into application building process 
can be taken as a starting point to move forward through this direction. This article presents an example that uses as main 
technologies:

- Gherkin: used to write the example specification;
- Cucumber: used as test framework for acceptance tests;
- Serenity: to tight it all together to bring BDD and living documentation;
- GitHub actions: an example of its usage in a CI/CD approach.

For a release delivery flow, it is possible to say that a set of user stories brings the discovery journey for a feature to 
the tech team. This will transform a business requirement into a feature that will be handled through an automated process which 
has as a final result a working software.

{% include figure image_path="assets/images/living-doc-bdd/bdd-practices-diagram.png" alt="bdd-practices-diagram.png" %}

### Practical example

To bring an example it was created a [GitHub Project](https://github.com/tnfigueiredo/living-doc-kotlin-spring-sample) with a sample using 
Kotlin, Cucumber, Serenity and Gradle to show some features for a BDD approach to generate project business documentation into the SDLC 
(software development life cycle). it was created a fictitious business scenario for a course center to illustrate an example of this 
approach. The sample in the GitHub repo explore a few more things, but here it will be presented mainly a general scenario for creating 
the software specification based on user stories and how it is related to software documentation in an SDLC.

According to the mentioned idea, this fictitious scenario is to help us to describe the living documentation approach. To make it closer 
to an ongoing project following an Agile SDLC, we are going to have features done from a previous delivery, and pending features to be implemented in future sprints.

Taking this previous description in consideration, here follows a class diagram to bring an idea of the domain model for the project. In this domain 
it is possible to consider as main business concepts: departments, courses, and students. Those domain concepts will be mapped as the epics for this project, 
and they will be the criteria for the user stories classification.

![cached image](http://www.plantuml.com/plantuml/proxy?src=https://raw.githubusercontent.com/tnfigueiredo/living-doc-kotlin-spring-sample/master/docs/myfictitious-course-center-domain.puml)

Considering a proposal for developing the features in a possible BDD approach, it is possible to take in consideration a set of features done, 
a set of features as pending for the next software delivery cycles. The repository has more details about it, but here it will be mentioned 
only the initial state of this possible scenario. For this domain it is possible to consider the following set of use cases:

#### Departments use cases
![cached image](http://www.plantuml.com/plantuml/proxy?src=https://raw.githubusercontent.com/tnfigueiredo/living-doc-kotlin-spring-sample/master/docs/departments-use-cases.puml)

#### Courses use cases
![cached image](http://www.plantuml.com/plantuml/proxy?src=https://raw.githubusercontent.com/tnfigueiredo/living-doc-kotlin-spring-sample/master/docs/courses-use-cases.puml)

#### Students use cases
![cached image](http://www.plantuml.com/plantuml/proxy?src=https://raw.githubusercontent.com/tnfigueiredo/living-doc-kotlin-spring-sample/master/docs/students-use-cases.puml)

#### Expected result over the documentation

The expected results is to have a separation of topics according to the project epics (Departments, Students, Courses). The project epics
will be the starting point to crate the user stories relates to those mapped use cases presented previously. Each use case will be related to a set
of user stories that will describe the application features. And for each user story there will be available some acceptance
criteria that will validate the business rule to be followed. It is possible to consider it will be something like this:

{% include figure image_path="assets/images/living-doc-bdd/spec-struct.jpeg" alt="spec-struct.jpeg" %}

This structure will be the concept to create the files in the documentation for the BDD test cases and specification. Since it is common to
group the user stories in epics, the layer described as use cases will not be present in the documentation. The files structure will
be explained better when presenting it in the source code project structure.

#### Project structure

Considering this initial project configuration there is the test source code structure and the business specification and BDD files
structure. For the test source code there are 3 main components:

- AcceptanceTestRunner: This class has the configuration for the Test Suit runner and the Cucumber configurations. It was used a default
  configuration that can read the whole Spring context to the dependencies injection for the test context and the path for the
  feature files.
- CucumberSpringConfiguration: An empty stub just to enable the Cucumber context for a Spring Test class.
- StepsDefinition classes: Those classes have the implementation for the Given/When/Them BDD validations related to the description
  written in the feature files. For this sample, they were grouped according to the Use Case definitions.

{% include figure image_path="assets/images/living-doc-bdd/test-source-code-structure.png" alt="test-source-code-structure.png" %}

The sample of this project follows the recommended file structure mentioned on the [Serenity official
documentation](https://serenity-bdd.github.io/docs/reporting/living_documentation#the-requirements-hierarchy). This file structure
was organized into a hierarchy for Epics and features, being the feature files used to describe a set of User Stories. The Epics are
described through "readme.md" files, which bring the Epic business description. The feature files represent the Use Cases description,
which will group the User Stories related to that:

{% include figure image_path="assets/images/living-doc-bdd/spec-files-structure.png" alt="spec-files-structure.png" %}

Once the files structure is described it is easier to understand how the Given/When/Then BDD definitions written in the Gherkin files
are related to the steps definition implementation. In this sample, the Use Case feature file is related to the StepsDefinition BDD test
implementation, except for the Students Epic which has the implementation in a single class:


| Feature File     | Koltin Implementation |
|------------------|-----------------------|
| manage_courses.feature    | CoursesManagementStepsDefinition.kt |
| report_students_by_course.feature | CoursesReportStepsDefinition.kt |
| manage_departments.feature | DepartmentsManagementStepsDefinition.kt |
| report_courses_by_department.feature | DepartmentsReportStepsDefinition.kt |
| course_enrollment.feature and course_participation_cancellation.feature | StudentsManagementStepsDefinition.kt |

Here follows an example for the mentioned files:

- Feature specification:

```gherkin
   Feature: Manage Courses
   
     A use case description...
   
     Rule: A business rule...
   
       Scenario: Create a course successfully
         Given A department administrator needs to manage course information
         And there is an existing course: "MyCourse1", "MyC1", "Programming", "2023-02-18", "2023-06-16"
         When it is informed the course information: "MyCourse2", "MyC2", "Programming", "2023-07-18", "2023-12-16"
         Then a course is created successfully
```

- Steps definition:

```kotlin
package com.tnfigueiredo.docsample.bdd.courses

import...

class CoursesManagementStepsDefinition {
    
    private lateinit var creator: GeneralUser

    @Given("A department administrator needs to manage course information")
    fun givenDepartmentAdministratorNeedsManageCourseInformation(){
        creator = GeneralUser(1, "dptoAdmin", UserProfile.DEPARTMENT_ADMINISTRATOR)
    }

    @And("there is an existing course: {string}, {string}, {string}, {string}, {string}")
    fun andExistingCourseAvailable(courseName: String, courseCode: String, area: String, startDate: String, endDate: String){
        //TODO Implement
    }

    @When("it is informed the course information: {string}, {string}, {string}, {string}, {string}")
    fun whenCourseInformationIsInformed(courseName: String, courseCode: String, area: String, startDate: String, endDate: String){
        //TODO Implement
    }

    @Then("a course is created successfully")
    fun thenCourseCreatedSuccessfully(){
        //TODO Implement
    }
}
```

Those tests will have common implementation already used to run validations like the integration and acceptance tests. The steps are called 
according to the description in the Gherkin files and the description in the annotations need to match the text described in the feature files. 

#### Project build and report results

For this sample it was not created any special configuration for the report output folders. Considering so, the project source code
remains being generated on the project build folder, while the results for the Serenity reports are generated in the target folder.
The resulting report is generated in an HTML files format, into the folder structure ./target/site/serenity:

{% include figure image_path="assets/images/living-doc-bdd/build-reports-folder.png" alt="build-reports-folder.png" %}

It is possible to open the resulting report in a browser through the index.html file generated in the Serenity reports folder.
The first view this index.html file presents in the report is an overview about the project test cases grouped by a set os possible
status. This overview also presents tests statistics and classification. In the case of this sample by Epic and Features:

{% include figure image_path="assets/images/living-doc-bdd/serenity-reports-overview.png" alt="serenity-reports-overview.png" caption="Report Summary Overview"%}

The Requirements tab present and overview about the epics and the features classification, together with the test coverage scenario:

{% include figure image_path="assets/images/living-doc-bdd/requirements-overview.png" alt="requirements-overview.png" caption="Requirements Overview"%}

The Capabilities tab presents the Epics summary test cases:

{% include figure image_path="assets/images/living-doc-bdd/epics-summary.png" alt="epics-summary.png" caption="Epics Summary Overview"%}

It is possible to check the "readme.md" epic file description when you click in a Capability:

{% include figure image_path="assets/images/living-doc-bdd/epic-description.png" alt="epic-description.png" caption="Epic Description"%}

The Features tab presents the Features summary test cases:

{% include figure image_path="assets/images/living-doc-bdd/features-summary.png" alt="features-summary.png" caption="Features Summary Overview"%}

When accessing a specific feature it is possible to check the feature description and the summary of its test cases:

{% include figure image_path="assets/images/living-doc-bdd/features-description.png" alt="features-description.png" caption="Features Description"%}

And if it is accessed a specific test case it is possible to see its execution and description:

{% include figure image_path="assets/images/living-doc-bdd/test-case-example.png" alt="test-case-example.png" caption="Test Case Example"%}

## Final Considerations

If it is taken in consideration the tests pyramid it is possible to perceive that the BDD tests will be mostly in the range of validations 
done also by acceptance tests and E2E (ent-to-end) tests. The focus is on business scenarios that will validate application behaviours 
collected together with the stakeholders and other team members. That is also a reason why it improves the communication with stakeholders. 
Since the validations are done in a business language, if the stakeholders are involved in the software process they will be able to understand 
what was validated in the software releases.

{% include figure image_path="assets/images/living-doc-bdd/tests-pyramid.jpeg" alt="tests-pyramid.jpeg" %}

If it is taken in consideration the tech team perspective it is possible to say that the BDD tests also works like kind a regression test tool, 
since it helps to make sure that the business requirements remain behaving as expected through the delivery of the releases.

{% include figure image_path="assets/images/living-doc-bdd/test-quadrant.jpg" alt="test-quadrant.jpg" %}

There are some project aspects that are important to be well planned due to impacts on the project and tests maintenance, but even with some extra effort
to have this layer of tests, it is possible to clearly achieve value to the project deliverables.

## References

##### The Holy Grail of “Always-Up-to-Date” Documentation
* [Svetlana Novikova: The Holy Grail of “Always-Up-to-Date” Documentation](https://blog.jetbrains.com/writerside/2022/01/the-holy-grail-of-always-up-to-date-documentation/)

##### Living Documentation
* [Serenity official site](https://serenity-bdd.github.io/docs/reporting/living_documentation)

##### Core Practices for Agile/Lean Documentation
* [Scott Ambler: Core Practices for Agile/Lean Documentation](http://agilemodeling.com/essays/agileDocumentationBestPractices.htm)

##### History of BDD
* [Cucumber official site - History of BDD](https://cucumber.io/docs/bdd/history/)

##### Gherkin Reference
* [Cucumber official site - Gherkin Reference](https://cucumber.io/docs/gherkin/reference/)

##### Living Documentation vs. Test Reports: What’s the Difference?
* [Aurlane Pascal](https://cucumber.io/blog/test-automation/living-documentation-vs-test-reports-whats-the-dif/)

##### What is BDD?
* [Cucumber official site - What is BDD?](https://cucumber.io/docs/bdd/)

##### CI\CD and Automation Testing: What’s the Connection Between These Concepts
* [Mykhailo Poliarush](https://testomat.io/blog/ci-cd-and-automation-testing-whats-the-connection%E2%9D%93between-these-concepts/)

##### Behaviour-Driven Development: Value through Collaboration
* [Jan Stenberg](https://www.infoq.com/news/2013/12/bdd-value-collaboration/)

##### Visibility into the CI/CD Pipeline
* [Liquibase official site - Visibility into the CI/CD Pipeline](https://www.liquibase.com/blog/visibility-ci-cd-pipeline)

##### Writing BDD Test Cases in agile software development
* [Mykhailo Poliarush](https://testomat.io/blog/writing-bdd-test-cases-in-agile-software-development-examples-best-practices-test-case-templates/)