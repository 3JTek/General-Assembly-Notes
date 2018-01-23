# Software Development Process

Here are some different roles that you might find in a software development team:

- **Developers** - There are lots of different types of developer though.
  * **Front end** (html/css) - Develops what the user sees. User interface.
  * **Back end** (app & database) - Provides a base for the front end developer.
  * **Full Stack** - Able to do both front & back end

- **Designers** - Design the appearance of the product
  * **UX Designer** - Design the user experience
  * **UI Designer** - Design the user interface

- **Sysops (system operator)**:
  * Server management

- **Project Managers**
  * Manages team resources and time spend on the project to ensure that money is made by the company.

- **Product Managers**
  * Takes control of the project, decides on what features to build after discussions with the client.

- **Database Analyst**
  * Works with the database to ensure that it is efficient and backed up.

- **Development Manager**
  * Similar to a project manager, but works very closely with the development team. Often only found in large companies.

- **QA (Quality Assurance) Tester**
  * A QA tester will check code, often by using software to simulate lots of users. Will create bug reports if they find any and send them to the relevant development team.

- **Devops (development operations)**
  * After code has been signed off by a QA and is relatively bug-free, devops will deploy this code to production. Devops will also often be system administrators.

- **The Client**
  * Bad software development teams forget that the client is also a part of the team.

Other job titles might include:

- Solutions Architect
- Technical Architect
- Functional Analyst
- Lead Developer
- Junior Developer
- DBA
- Trainer
- Domain Experts
- SEO

Some teams have all of these whilst some might just have one or two.

Sometimes one person will cover lots of these roles (in a small team, they have to), other times, the roles can get very specific.

## Project Management Methodologies

### Waterfall development method

In the old days, software requirements were specified in a big book that took a while to create, and then these specs went to the developers. This is an inefficient way to develop software, because you cannot possibly know ahead of time everything that will be needed. This process is called [waterfall](http://en.wikipedia.org/wiki/Waterfall_model).

![Waterfall](https://lh6.googleusercontent.com/4vI2oaCcyGSiKOV5kAgQ7CLUoKTwZVtfgfnrBtsn08uazGgvo5DHy0vrTCwQ1_188i0w5gHfrFi6p_QoFo7jvOKSgAG6oTGWXdpPAlHg8FiLZkU4CSa8i4F1SA "Waterfall Diagram")

- **Requirements**:  Requirements for the project are set, these will outline the project but are not specific.
- **Functional Specification**: This is a more detailed version of the requirements which will go into depth.
- **Detailed design document**: Tells the developers what they are using and how to use it.
- **Coding**: Can eventually begin after everything else has been set in stone.
- **Testing**: Completed code is tested against the original requirements.

### Agile development method

The [agile development](http://en.wikipedia.org/wiki/Agile_software_development) process takes an **iterative approach**. A requirement is set, it is implemented, tested and refactored. This cycle is repeated until the product is complete. This is a much more flexible approach. Requirements are constantly being created and developers respond quickly to them.

![agile methodology](https://lh4.googleusercontent.com/n7nGFgC0WjFehArQ-YpF6amxpX7ssv3VI8G7Yd-zDlxs3yJK2hAy-fDiRZ-6JKqdNzidcbKJPEIsSzmWviVWI7WRJ9ZA6fvxjlCxgldYxm9CBxO9trT61QFlvA)

#### Agile Manifesto

> "We are uncovering better ways of developing software by doing it and helping others do it. Through this work we have come to value:

> - **Individuals and interactions** over **Processes and tools**
> - **Working software** over **Comprehensive documentation**
> - **Customer collaboration** over **Contract negotiation**
> - **Responding to change** over **Following a plan**

> That is, while there is value in the items on the right, we value the items on the left more."

## Scrum

Scrum is an agile software development model based on multiple small teams working in an intensive and interdependent manner. The term is named for the scrum (or scrummage) formation in rugby, which is used to restart the game after an event that causes play to stop,

The scrum concept was introduced by Hirotaka Takeuchi and Ikujiro Nonaka in a 1986 article in The Harvard Business Review, ["The New New Product Development Game"](https://hbr.org/1986/01/the-new-new-product-development-game) - the original context was manufacturing. Jeff Sutherland, John Scumniotales and Jeff McKenna are credited with adopting, implementing and documenting the model for software development at Easel Corporation in 1993.

#### Daily scrum

During the course, we will have a daily scrum aka ["stand-up meeting"](https://en.wikipedia.org/wiki/Stand-up_meeting) at 9am.

The daily scrum/stand-up will allow each member of the team to take 2 minutes to reflect on their progress, their blockers and where they intend to go next.

Scrum meetings usually involve the whole team:

![The Scrum Structure](https://lh5.googleusercontent.com/bun08_PrsvZaZosJRqKvZKgkhNDeYD3bVsnGHV2JzmGCF72S1Da3OwUQEno2bEQy89t-MRUpfIEyBXwTZ5oBOBXeHc_39h1hXWrpeArfJfgFCfHCEmUy4kWO-A "THE SCRUM")

![The Scrum Process](https://lh3.googleusercontent.com/XhlGKtWQC6kXZ84mN3aSzVqnZVPXSqJDJT0rAiWu3bxmajD9AaV-qfJeevYPiT9NLPs1DufWCUJ4ZuLUFJhZhghOazA_wIXXcWFBI-pe3dL6fIOmmj2Pzmmo_Q "THE SCRUM")

#### Scrum Rules

There are a few simple rules to a scrum meeting:

- Meet at same time and place each day
- The meeting starts/finishes on time (latecomers beware!)
- The meeting length is fixed to 15 minutes
- By standing up (no slouching against walls, or perching on desks) people don't get comfortable and settle into a long meeting
- Each team member answers three questions:
  1. What have you done since yesterday?
  2. What are you planning to do today?
  3. Do you have any "blockers" (i.e. stumbling blocks) stopping you achieving what you need to?
- Blockers are documented by the Scrum Master, and then resolved outside of the scrum meeting

So to get into the habit, we're going to practice Scrum every day at 9am.

## Testing

An important part of agile development is testing. During the course, you will hear us talk about:

- **TDD (Test Driven Development)**
- **BDD (Behaviour Driven Development)**

### TDD

TDD is a process by which a developer writes tests before they write code.

#### The process:

1. First the developer writes some tests.
2. The developer then runs those tests and (obviously) they fail because none of those features are actually implemented.
3. Next the developer actually implements those tests in code.
4. If the developer writes his code well, then the in next stage he will see his tests pass.

The developer can then refactor his code, add comments, clean it up, as he wishes because the developer knows that if the new code breaks something, then the tests will alert him by failing.

![tdd-flowchart](https://cloud.githubusercontent.com/assets/40461/8218941/4dcd6ba8-153e-11e5-8b8f-87285bfa0754.png)

### BDD

Some people will say BDD is similar to TDD, others will say that it is just TDD but with better guidelines, or even a totally different approach to developing.

The main difference is just the wording. BDD uses a more verbose style so that it can be read almost like a sentence.

For the duration of the course, we won't be following a TDD approach - because, since tests *are* code, until we're comfortable writing code, we won't comfortably write tests (though we will make steps toward it in the middle-end of the course, when we'll have done some test-driven exercises - we'll give you the tests to make pass - and look at some TDD technologies).

## Further reading

- [What is Agile?](http://www.agilenutshell.com/what_is_agile)
- [Agile vs Waterall: A Side-by-side Comparison](https://www.upwork.com/hiring/development/agile-vs-waterfall/)
