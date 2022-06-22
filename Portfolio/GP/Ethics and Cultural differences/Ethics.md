# Ethics in software developer
Ethics are a very important but often overlooked detail of the software development branch nowadays. Even though you might not realise it, you run into a large amount of unethical software practices on the daily, some small and innocent, but also a lot of big malicious practices. A few examples of these practices given below, however there are many more unethical practices that can be used (and often are) for malicious intent:

- Click baiting in (mobile) games, websites and other digital environments;
![Clickbaiting](https://miro.medium.com/max/1400/1*xwfJkGuPQ9Q-cIck5c8thg.jpeg)

- (Questionably) selling user information for corporate income or malicious intent;
![Selling user data](https://scontent-ams4-1.xx.fbcdn.net/v/t1.6435-9/191053366_1410145139358602_711996923547134039_n.jpg?_nc_cat=111&ccb=1-7&_nc_sid=8bfeb9&_nc_ohc=nkAhrEnvvyoAX8vkDSX&_nc_ht=scontent-ams4-1.xx&oh=00_AT8kGx9PKD7QXe1K8GUBaubWb6qRJjpxTNcZc4xV4ytg0w&oe=62D785FA)

- Actively misleading users into taking unwanted actions;
![Misleading pricing](https://shopify.dev/assets/api/subscriptions/product-form-subcomponents-8c0326d4bf414da4d72e3456960648280bb4b5e21c00c407a5e5b11a5038568a.png)

<br><br>

## Software Engineering Code of Ethics
In the early 21st century an Ethics code guideline was set up by ACM (Association for Computing Machinery). This guideline is nowadays the world's leading guideline for educational and scientific societies around the world. The ethical principles set up by the ACM addresses the nowadays most common ethical malpractices in software engineering. The code consists of 8 broad principles, as taken from the official ACM publication:

<br>

1. Public
> Software engineers shall act consistently with the public interest.

2. Client and Employer
> Software engineers shall act in a manner that is in the best interests of their client and employer, consistent with the public interest.

3. Product
> Software engineers shall ensure that their products and related modifications meet the highest professional standards possible.

4. Judgement
> Software engineers shall maintain integrity and independence in their professional judgment.

5. Management
> Software engineering managers and leaders shall subscribe to and promote an ethical approach to the management of software development and maintenance.

6. Profession
> Software engineers shall advance the integrity and reputation of the profession consistent with the public interest.

7. Colleagues
> Software engineers shall be fair to and supportive of their colleagues.

8. Self
> Software engineers shall participate in lifelong learning regarding the practice of their profession and shall promote an ethical approach to the practice of the profession.

<br>

A more in depth description and information about each practice can be found [here](https://ethics.acm.org/code-of-ethics/software-engineering-code/) at the ACM's official code of ethics release. Furthermore, not following the commonly accpted ethical guidelines and for example intentionally creating a system with insecure user data or security holes that could compromise a systems security could expose the software engineer to legitimate courtial trials.

<br><br>

## Why is ethics in so important in software engineering?
As to why Ethics is such an important part in software engineering, one should take a look at the history of the profession. Since the birth of the internet, software has in many ways taken over the world. Because of the enormous group that interacts with software in many forms from websites to games, the product of software engineers can make huge impacts on people’s lives, privacy and sensitive data. Without a proper ethical compass malicious software could easily be written, and if spread through the internet, could affect billions of lives in unforeseeable ways. For this sole reason, upholding the highest standard of ethical engineering should be a practice everyone in the profession should be taught.

<br><br>

## How could ethics impact my project?
For my individual project I made a platform called Ordio, which is a platform that can be used by restaurant owners to upload their restaurant menus to a service, which can in turn be used by other developers in external applications to display or place order from these menus. This project has a few ethical hurdles to overcome.

<br>

The first major hurdle is the aspect of privacy: How can we as Ordio platform ensure that sensitive user data is secure and will not be leaked? The first step in the though process here could be to ensure user data, however another option would be to load this off to an external service that specialises in this. This second option is what I chose for in the Ordio platform, giving Auth0 authority over user authorization and user data. Auth0 is a widely trusted platform that upholds ethical practices and ensures its users a safe data storage solution.

<br>

A second hurdle the Ordio platform would have to overcome is regarding monetization: If the product were to be publicly released, how would one ethically monetize the platform? A few options come to mind here:
1. A (one time) subscription payment system for any companies that want to develop a frontend-application using the Ordio platform as menu-storage service;
2. A (one time) subscription payment system for restaurant owners that want to use the service to upload their menu's and use with any form of externally developed frontend-solution;
3. Monetization through advertising on any official Ordio webpages or applications;

<br>

Many other monetization solutions would not be apply able to the platform as for example selling the menu data to external companies so they could use the menus to advise competitors on what kind of items they should put on their menus to compete would not be ethically sound according to the ACM's 2nd principle, going directly against the client's interest.

<br><br>

## References
> Don Gotterbarn, Keith Miller, and Simon Rogerson (1997, November). *Software engineering code of ethics*. Retreived June 22, 2022, from ACM: https://ethics.acm.org/code-of-ethics/software-engineering-code

> Various contributors (2021, October 21). *Programming Ethics*. Retreived June 22, 2022, from: https://en.wikipedia.org/wiki/Programming_ethics