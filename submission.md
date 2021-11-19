# Architecture Theory

---

### How would you calculate 98<sup>2</sup>  with just a pen and paper?

```
98^2 = (98 x 98)  
98 x 100 = 9800  
98 + 98 = 196  
9800 - 196 = 9604
```

This question is more about problem solving and demonstrating the thought process. In a real-world  situation, we would just use a calculator.

---

### What are the advantages and disadvantages of Dependency Injection?

Dependency Injection is a technique in software engineering where an object receives the other objects that it depends on. These are called 'dependencies' (not to be confused with 3rd-party libraries). In other words, the dependencies are injected, or passed into a dependent object.

The main intention and a key advantage of DI is to maintain a separation between the construction and usage of objects. By separating the implementation of dependencies, the code becomes more flexible in regards to reusability, unit testing, and maintenance. This also reduces the amount of redundant code since the dependency is handled by a single component. The object itself is ready to be used, it just has to be passed into a client so that it can be used.

DI is not without its disadvantages. The design pattern results in clients that demand these dependencies. The code itself may become more flexible, DI results in less flexibility as to how the code can actually be used. DI can also make code difficult to trace because it separates object behaviour and construction. Even though DI does have advantages, it takes a substantial amount of effort to implement it properly so that these advantages can be fully appreciated.

I'll admit that up until today, I have never actually heard of DI. It is just not something I have been exposed to on a theoretical level. That being said, I have had more practical exposure to it simply through reading the example code for different Node JS libraries and incorporating these libraries into my own projects. When this question was explained to me, there was a little confusion between 'dependencies' as-in objects vs. 3rd-party libraries when working in Node JS for example. Still, it is worth considering the latter meaning for the sake of argument. When you read through the example code of libraries, or working with libraries in general, you are exposed to a wide variety of programming styles. This comes back to the 'usage inflexibility' disadvantage because when you implement a 3rd-party library into your code, you have to use that library in a way that the developer deems appropriate, regardless of how it fits your own style.

To reiterate, while I may not know much about DI in theory, I have nonetheless been exposed to it in the form of working with 3rd-party Node JS libraries. In other words, there are times I could have incorporated DI practices into my code without even realizing it. I won't cite specific libraries but I found an [article](https://blog.risingstack.com/dependency-injection-in-node-js/) on RisingStack that discusses DI in a Node JS context. The article covers the implementation of a basic module both with and without DI. I have included the snippets below so that you can see the difference yourself.

**Without Dependency Injection**

```javascript
// team.js
var User = require('./user');

function getTeam(teamId) {
  return User.find({teamId: teamId});
}

module.exports.getTeam = getTeam;
```
\
**With Dependency Injection**

```javascript
// team.js
function Team(options) {
  this.options = options;
}

Team.prototype.getTeam = function(teamId) {
  return this.options.User.find({teamId: teamId})
}

function create(options) {
  return new Team(options);
}
```

---

### When would you use a Mediator Pattern? What are the advantages and disadvantages?

To start with a simple definition, the Mediator Pattern defines an object that encapsulates how a set of objects interact with one another. Rather than objects referring to each other explicitly, the mediator can be used to send and receive messages from other objects. This results in events that are fired throughout the program. Incorporating this practice is known as 'event-driven programming'.

Using a mediator allows the decoupling of component classes, having only the mediator as a dependency. The mediator organizes overall communication as a one-to-many structure rather than many-to-many. One mediator communicates across many objects, rather than many objects communicating amongst themselves. This pattern also has the effect of improving code reusability. Since the mediator has a clear interface, it is easy to create and use mediator objects as needed.

A disadvantage of the mediator pattern is that the mediator object has a very high level of privilege amongst the given objects. It is a dependency of all of them. The implementation of the mediator object itself can be quite heavy, making the code more complex and difficult to maintain.

To give an example use case, a chatroom server could use the mediator pattern. This is a system where many clients each receive a message whenever one of the other clients performs an action, such as sending a message. Indeed, any scenario that requires bidirectional, low-latency, and real-time communication can benefit from using the mediator pattern. As far as Node JS is concerned, the [Express](https://expressjs.com/) and [Socket.IO](https://socket.io/) libraries pair well together in these scenarios. Socket.IO even has an official [tutorial](https://socket.io/get-started/chat) for implementing a chatroom using the two libraries. I had a go at it myself when I learned Node JS for the first time.

This is not the only way that Node JS enables event-driven programming. Using a [callback](https://nodejs.org/en/knowledge/getting-started/control-flow/what-are-callbacks/) structure is an event in itself. I won't go into too much detail here but when calling an asynchronous function, a second function is passed as an argument, which is called when the async function is finished. Hence, this is an event, allowing the code to do other things while waiting for the event to fire. I use callbacks a lot when writing Node JS so those sorts of events are something I am very familiar with. I have decided to include a code snippet from the [Node JS documentation](https://nodejs.org/api/fs.html#fsreadfilepath-options-callback) to show you what a callback event looks like.

```javascript
import { readFile } from 'fs';

readFile('/etc/passwd', (err, data) => {
  if (err) throw err;
  console.log(data);
});
```

---

### When would you use a Relational database as opposed to a Document-Oriented database?

The difference between 'relational' and 'document-oriented' database types is the structure used, or lack thereof. Under a relational model, data is categorized into a number of predefined types which are manifested as tables. Each row in a table refers to an individual object of that type, as well as its property values, represented by columns. This means that while every object will have different values, they all share the same basic structure.

A document-oriented database on the other hand does not strictly require an exact structure. You may choose to incorporate a structure if you want to but the relational principles are not an intrinsic part of document databases. As the name would suggest, it is better at dealing with documents rather than strictly defined object types. That way, the schema is dynamic and allows you to work with unstructured data.

In a relational database, you need to figure out the schema well in advance before adding any data. You can update the schema afterwards but as you can imagine, it can be quite painful to modify existing data in order to comply with a new schema. When using a document-oriented database, you are free to figure things out as you go, as the data is much easier to update as requirements change.

As for which one you should use for what scenario, I think of these sorts of questions as subjective. There is no real right or wrong answer, but there are guidelines you may want to take into consideration. First, consider what kind of data you're working with and how you plan on using it. If the data is organized and has a fixed structure, relational would be the better choice. On the other hand, if there is lots of data that is unorganized or constantly evolving, you may want to consider a document database.

While I have heard of document-oriented databases, I was not entirely aware of what the term meant. I am more familiar with the 'NoSQL' terminology. As the name would suggest, there is no SQL. While a document-oriented database could refer to literal documents such as from Microsoft Office, the 'documents' in question really refer more to notations such as JSON and XML. For example, you could store a collection of JSON objects in an array.

To give you a real-world example, the IT system at Response Services Incorporated, where I worked previously, was largely made up of Google Drive documents. We did not have any fancy DBMS software or server infrastructure. We just had Google Drive and that was it. I cannot go into any organization-specific details but each client that we worked with had their own folder on our Drive. That folder housed all of the information that we had on that particular client. What really made things interesting was how we used scripts and macros in order to create something at least vaguely resembling a relational structure. For example, when registering a new client, we would run a script to create the basic folder structure as well as their initial files. Of course, no client folder had exactly the same structure but through the use of automation, we were able to make things consistent enough so that we could work with our data in a relational manner when necessary. I remember sitting down to write query scripts in order to retrieve client data. It would iterate through all of the client folders, scan the files for the target information, and compile the results. In hindsight, while the RSI database was document-oriented in principle, it still incorporated some of the relational principles by maintaining a reasonably consistent structure between entries of a would-be object type. I was hired long after the system was put in place but I can almost imagine my former boss designing at least *some* kind of relational database on paper, and then implementing it in the form of a Google Drive, using documents instead of relations.

When doing a practical group project for an internship back in March 2021, I was given basic exposure to MongoDB, which is a type of document-oriented / NoSQL database. The project was to implement a Node JS API server and Mongo was one of the technical requirements. My personal verdict is that it is basically working with JSON collections within a DBMS-style environment, similar to SQL Server for the relational side of things. Unfortunately, I did not own the group's code repo and I did not get a chance to make a backup before the rightful owner deleted it. Therefore, I cannot show you anything I did with Mongo on that project. I still have the database design but I think it would be pointless to bring it up here without the Mongo implementation to go with it.

---

## References

**Question 2**
* [Wikipedia - Dependency Injection](https://en.wikipedia.org/wiki/Dependency_injection)

* [Dependency Injection in Node.js](https://blog.risingstack.com/dependency-injection-in-node-js/)  
\-RisingStack  
Last updated: 17 September 2021

\
**Question 3**
* [Wikipedia - Mediator Pattern](https://en.wikipedia.org/wiki/Mediator_pattern)

* [Mediator Design Pattern](https://javadevcentral.com/mediator-design-pattern)  
\-Java Developer Central  
23 March 2020

* [Mediator Design Pattern](https://www.geeksforgeeks.org/mediator-design-pattern-2/)  
\-[@DesignPattern](https://auth.geeksforgeeks.org/user/DesignPattern/articles) et al. (GeeksForGeeks)  
Last updated: 17 June 2021

* [Express.js](https://expressjs.com/)

* [Socket.IO](https://socket.io/)

* [Chatroom Tutorial](https://socket.io/get-started/chat)  
\-Socket.IO documentation

* [What Are Callbacks?](https://nodejs.org/en/knowledge/getting-started/control-flow/what-are-callbacks/)  
\-Node JS documentation  
26 August 2011

* [Callback Example](https://nodejs.org/api/fs.html#fsreadfilepath-options-callback)  
\-Node JS documentation  

\
**Question 4**
* [Wikipedia - Document Oriented Database](https://en.wikipedia.org/wiki/Document-oriented_database)

* [Techopedia - Document Oriented Database](https://www.techopedia.com/definition/30329/document-oriented-database)

* [Document vs. Relational Databases](https://dev.to/tehbakey/document-vs-relational-databases-4km7)  
\-[@tehbakey](https://dev.to/tehbakey) (DEV Community)  
Posted on 10 February 2020  
Updated on 16 March 2020

* [NoSQL vs. Relational Databases](https://www.mongodb.com/scale/nosql-vs-relational-databases)  
\-MongoDB documentation

---
