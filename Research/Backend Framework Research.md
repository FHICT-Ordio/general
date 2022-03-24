# Backend platform research

Currently theres two viable options for creating the backend API part of the Ordio Project: JavaScript using Node.js and C# using ASP.NET.

Each language and platform has its own advantages and drawbacks. These pros and cons will be discussed in this chapter, after which a conclusion will be drawn on what platform I will be using for the backend for the API of Ordio.

<br />
<br />

## Node.js vs ASP.NET
---

| Aspect | Node.js | ASP.NET | Winner |
| ------ | ------- | ------- | :----: |
| Language | JavaScripit | C# | - |
| Speed | Asynchronous, Extremely fast | Synchronous, efficient but slightly less fast | Node.js |
| Support | Little StackOverflow support | Verry much StackOverflow support | ASP.NET |
| Library-support | Very large amount of independent smaller libraries | Large amount of scalable built-in libraries | Tied |
| External Tools[*](#external-tools) | Little developer tools available | Large amount of developer tools available | ASP.NET |
| Performance[*](#performance) | Better overall performance | Slightly worse overall performance | Node.js |
| Reliablity[*](#reliability) | Weak reliability and overall more difficult to bugfix | Very strong reliability and error handling | ASP.NET |
| Experience[*](#experience) | Little experience, beginner level | Much experience, advanced level | Node.js |
| Scalibility[*](#scalability) | Very little scalability, no multithreading support | Very scalable with integrated multithreading features | ASP.NET |
| Maintainability[*](#maintainability) | Limited maintainability | Very high maintainability | Tied |
| Testability[*](#testability) | Automated testing possible, not natively supported | Exelent native automated and manual testing | ASP.NET |

<br />

### External Tools
External tools are an important aspect of development and ease of use of a programming language. Both platforms offer a variety of external tools. These include IDEs for making editing of code easier, but also other tools like refactoring tools. The most vital ones of these tools for the two platforms are the following:

Node.js
| Tool | Usecase |
| ---- | ------- |
| WebStorm | JavaScript-specific IDE used for making code. Includes code autocompleter and other built in features |


<br />

ASP.NET

| Tool | Usecase |
| ---- | ------- |
| Visual Studio | C# and ASP.NET IDE used for writing C# code. Includes code autocompleter and other built in features |
| Intellisense | Visual Studio's built in coding tool. Predicts code behaviour and can write code for you, adaptive on the context of said code |
| ReSharper <br /> SonarLint | Static code analysis tools. These are used to give live feedback on code and refactoring suggestions |
| Swagger-integration | Automated API documenting tool. Generates API documentation and test-environment based on the live API code |

> [ASP.NET] From gathered sources it seems like ASP.NET offers a lot more external tools to make development easier and more efficient than Node.js. The biggest advantages and gamechangers ASP.NET has here are, in my opinion, the availability of Intellisense and automated Swagger integration, which are not inherently available for Node.js. In this category ASP.NET is the definite winner in this category.

<br/><br/>

### Performance
Another important aspect is that the framework needs to have is good performance. A good server-side performance means two things: being able to process requests fast, and being able to take a big load of request (Speed and Load). 

I've looked into benchmarks and analysis of the both these factors of both platforms. The results of these are as following: 

![Benchmark run by github-user Ilyaigpetrov](https://camo.githubusercontent.com/322e8c46b60197a695f6b0ed4cd6563bcdd8957c4f4c856c26fa14d52db361ab/68747470733a2f2f692e696d6775722e636f6d2f4e356b3058695a2e706e67 "Benchmark by Ilyaigpetrov")<br />
[Benchmark](https://gist.github.com/ilyaigpetrov/f6df3e6f825ae1b5c7e2) run by github-user Ilyaigpetrov

<br />

The second benchmark is from an academic research paper by Oliver Karlsson from Jönöping University released in July 2021 on this exact subject. The paper in question can be found [here](https://www.diva-portal.org/smash/get/diva2:1586295/FULLTEXT01.pdf).

<br />

Both these benchmark show the same result: JavaScript is generally in both tests slightly faster, leading by an average of 8.2% in terms of speed and 9.0% in terms of load in Ilyaigpetrov's benchmark, and (taking my general usecase of big load with little data transfer) a 9.8% lead in terms of speed in Karlsson's benchmark

> [Node.js] From gathered sources and benchmarks it seems Node.js leads in terms of performance by an estimated margin of roughly 9% in both speed and load, making Node.js the winner in this category. 


<br />

### Reliability
Another important aspect of a solid development platform is its reliability. Reliability means how easily server-sided crashes are avoidable and the extend to which theyre self-processable by the application itself. More avoidability means a general lower amount of downtime. 

In this catergory theres two mayor differences between ASP.NET and Note.js: Error handling and strong and weak typing. Node.js is a platform that doesnt support strong typing, meaning theres no checks in place what type a veriable should be or is. ASP.NET does rely on strong typing, making errors due to this way easier to handle and much more avoidable. Furthermore, ASP.NET generally offers way more support for error handling in comparison to Node.js.

> [ASP.NET] In this category ASP.NET is the definite winner. The main advantages ASP.NET has over Node.js here are its strong typing vs Node.js's weak typing, and the huge leap in support for, partially automated, server-sided error handling.

<br />

### Experience
A smaller but also definitely important aspect in the choice of what platform I'll be using is experience. In both my acedemic journy personal expercience, I've worked on some projects with both platforms. Though that being said, I have a lot more experience with C# and ASP.NET compared to Node.js, meaning I know a lot more about the inner workings and ins-and-outs of ASP.NET than its counterpart.

In terms of skill level, I would place myself at an advanced level op C# and ASP.NET, while my JavaScript and Node.js skills are at a beginner / intermediate level. However, this is definitely NOT a gamebreaker for using JavaScript and Note.js for me, in fact quite the oposite: Whats the point of a study if youre not learning new skills. However, I also recognise that just the fact that I want to learn a new platform should not overrule the rest of the reasearch I'm doing.

> [Node.js] In term of experience, I have more confidence in working with C# and ASP.NET compared to JavaScript and Node.js. However, I would very much like to learn Node.js and this project would be the perfect way to do so. Becouse of this, Node.js might not be he winner in this aspect, but will be the preffered platform in acedemic value.

<br />

### Scalability
Another important aspect that I value in a development platform is scalability. Scalability is defined by how easily an application can be upscaled without adding a lot of intricate details or dependency on a lot of other parts of the application. 

ASP.NET offers a solution this with SOLID. SOLID is a set of principles that addresses common issues, and following these principles makes scaling up or down an application way easier then usual. A major aspect in this is the user of interfaces, a feature wich are not supported in JavaScript and Node.js, meaning that in Node.js individual pieces of code, even between different modules, are heavily dependent on one another, which is not the case in ASP.NET

> [ASP.NET] In terms of scalability ASP.NET has a major advantage over Node.js. The use of SOLID principles and Interfaces allows for a way more robust application which can be up- or downscaled much easier than possible in Node.js. I value this aspect very much in software development.

<br />

### Maintainability
Maintainability defines how easily the inner workings of an application can be maintained once its up and running. This mainly refers to how easily bugs and error withing an application can be tracked down and fixed in a live envornment. 

ASP.NET and Node.js both have advantages and disadvantages in this aspect Though ASP.NET makes it a lot easier to track down bugs due to its availability of external tools and testing posibilities, taking down an ASP.NET server, fixing it, and bringing it back live again is way more time consuming and troublesome than doing so with a Node.js service.

The main advantage Node.js has here is, as mentioned above, the speed and ease with which a live service can be taken dow, recompiled and put back live. 

> [ASP.NET] In terms of maintainability, both ASP.NET and Node.js have their advantages and disadvantages. In this category, both platforms tie inn my opinion.

<br />

### Testability
The final aspect is testibilty. For me, this is a very important measure as a well testibile application makes tracking bugs and errors a lot easier, making the development and maintaining process a lot smoother.

Both ASP.NET and Node.js have frameworks for automated testing of applications. The most widely known one for ASP.NET is MSTest, a built in microsoft unit testing platform. Node.js has a lot of individual third party frameworks for automated testing, but these are not by nature native to the platform, but do the work none the less.

Another important measure of testibilty besides the automated testing is the ease of manual testing. API's are in this case a bit of a special case, since they dont offer much visual interface for developers to work with. Howerver, ASP.NET offers a solution for this in the form of Swagger intergration, which Node.js does not natively offer, making integration and testing using Swagger or similar tools much more difficult. 

> [ASP.NET] In terms of testibilty ASP.NET is the definte winner. This conclusion is based on the native integration of both automated and manual testing which Node.js does not offer. 

<br />

---
## Conclusion
After carefully considering all above stated aspect of both ASP.NET and Node.js, I have come to a descision on what platform I'll be using for the Ordio backend API services. 

<br />

First of all, one important part that needs to be stated is that since ill be using API's the implementation of the backend of these services should not impact the working of the overall application. This means that im not restricted to using ONE platform for all the services.

The Ordio backend will consist of at least two API services: One responsible for loading and processing menu items for individual restaurants, and one responsible for handling round-orders for every restaurant. These two services value different aspects: The former service doesnt need to work very fast as menu items dont need to regularly be fetched or updated, but they need to transmit a large amount of data. The latter service is the exact oposite, it doesnt need to handle a large amount of data but the amount of requests will be much higher.

<br />

Becouse of this and the fact that using different platforms will improve academic value, I have decided not to stick to one specific framework as talked about before, but to use both frameworks where their strong suits are needed:
* The menu service prioritises efficiency, C# with ASP.NET will be used for this service
* The order service prioritises speed, JavaScript with Node.js will be used for this service

Possible future services which are still anaccounted for will be made in a platform that suits their needs. The strong and weak sides this document talks about will be used in making that decision

<br />

## Sources
https://www.esparkinfo.com/blog/node-js-vs-asp-net.html

https://www.diva-portal.org/smash/get/diva2:1586295/FULLTEXT01.pdf

https://www.techmagic.co/blog/node-js-vs-net-what-to-choose/

https://www.geeksforgeeks.org/difference-between-node-js-and-asp-net/

https://www.jetbrains.com/webstorm/

https://docs.microsoft.com/en-us/visualstudio/ide/visual-csharp-intellisense?view=vs-2022

https://stackoverflow.com/questions/860690/what-refactoring-tools-do-you-use-for-net

https://gist.github.com/ilyaigpetrov/f6df3e6f825ae1b5c7e2

https://en.wikipedia.org/wiki/Strong_and_weak_typing

https://www.testim.io/blog/node-js-unit-testing-get-started-quickly-with-examples/

https://docs.microsoft.com/en-us/visualstudio/test/getting-started-with-unit-testing?view=vs-2022&tabs=dotnet%2Cmstest