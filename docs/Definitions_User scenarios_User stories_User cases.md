User scenarios, user stories, user cases - what's the difference?

User stories, scenarios, and use cases - all are different concepts that help communicate what a solution that is being created needed to do. The differences are in structure and perspective.  
### User stories
**User stories** are from the perspective of the end user, and very simple in structure:
As a (user type) I want to (action/feature) so that (reason)
As a (user type) I want to (action/feature) because (reason)

They also contain details that indicate what should happen, driven by context and other situations:
If (context) and (additional context) when (event) then (outcome)

User stories are used in the agile development process to scope features.  
They allow developers to focus on what the user has to do.  
The best user stories are grounded by user research and understanding - they aren't "made up" and actually reflect what users have to do.

### User scenarios
The next level of detail, the next "format" these stories can take in a design/development project are **user scenarios**.  
These are much more detailed and include details such as user's motivation and environment.  
As opposed to the simple narrative of above, these are often presented in storyboards and as "blueprints" with a lot of details and texture that may often be superfluous. 

User scenarios paint a picture that is more complete, but they are often viewed as superfluous on projects that have an aggressive schedule.  
When to use scenarios depends on the project and the needs of the project.  

### Use cases  
Finally, **use cases** are structured documents that contain requirements and details of what functionality should exist.  
Use cases are (usually) extremely entailed and detail both user behavior and system response.  
They are less about user needs and mental models and **more prescriptive direction as to what needs to be developed**.
As these documents usually take some time to create, they have fallen out of favor with many companies who have adopted agile development processes.



#### User Scenarios
Think of user scenarios as a real-world narrative of the process to accomplish a user's goal.  
A user scenario will articulate the goal, the process, and the outcome that is contextually relevant to the user.  
In doing so, a lot can be discovered or validated about the assumptions regarding the user and their situation/problem. 

(a) **User scenarios** are thus quite useful in requirements discovery: trying to understand how the user is going to go about solving their problem will point to various kinds of flows and objects that are needed to accomplish a task.  
So, the exploratory/discovery type of scenario usage helps one plan a user interface and system interaction for supporting the user's needs.  
You will have have a clearer definition of the path the user will go through, and the system objects (tools) they will need to reach the end of that path.  
Note that there must be no reference to a user interface or user interface elements. Keep it high level. 

(b) A second type of **user scenario** that I've identified is the type to validate a design and design assumptions. You've created a user interface design and employing a real-world narrative helps you see whether the path that the user takes is realistic or not. Does it seem reasonable that they do X when presented with Y in trying to accomplish Z? We can tell a lot about the sensibility of your designs as we match up the design to a real scenario. Here it's okay to talk about the user interface. Note that this type bears a lot of similarities to the usability testing scenario or cognitive walkthrough technique.

#### User Story
A **user story** is most often used for the purposes of AGILE development.  
A properly-formed story has the following structure:  
A [type of user] wants to [do something] so that [outcome happens].  
Very often, the user story is on the level of a specific piece of functionality that it is trying to justify and/or articulate.  
There are of course minor user stories and also epic user stories.  
Epic user stories are macroscopic in scope and contain many supporting user stories. 

#### Use Case
A **use case** is also used in a similar way to a user story.  
Generally a use case is highly context-specific.  
It speaks to a particular point of usage for a given situation so that the point of entry and exits are well-defined.  
We would have a better understanding of a specific piece of functionality by mapping it to an articulated use case.  
It's likely to be most effective when used as a reality-check on a given requirement.  
The use case might thus be confined to a single or simple task flow.  
An understanding of the user's goal here is built into the use case.  
One differentiator for the use case is that there can be more than one for a feature.  
It's useful to check several use cases for the same feature to understand how that feature might fail to satisfy all the possible user needs.

Still confused? Maybe a more tangible real-life example will help. . .

**Scenario:**

Josh is a 30 something mid-level manager for an ad agency, metro-sexual and beer aficionado. He likes to try new and exotic beers in trendy locations. He also enjoys using a variety of social apps on his smart phone. He reads a review on Yelp of a new burger & beer joint downtown with over 100 beers on tap, and decides to go walk over after work and check it out.

**User Story:**

A user must want to find a bar and drink a beer.

**Use Case:**

Customer walks to the restaurant

Customer enters the restaurant
Customer finds a seat at the bar

Customer scans the menu
Customer selects a beer
Customer orders selected beer
Bartender takes order
Bartender pours beer
Bartender delivers beer
User drinks beer
User pays for beer