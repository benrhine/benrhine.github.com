---
layout: post
title:  "Testing types and when to use them"
date:   2023-06-11
excerpt: "There are so many variations and naming conventions what testing type should you use?"
image: "https://imgs.xkcd.com/comics/good_code.png"
---

## Testing types and when to use them

As a developer you are always learning and by the time you are a seasoned developer you know more tools and process than
your average bear. So why is it that even after a number of years in the industry you still run across folks that have 
such a hard time understanding which test types are used for what part of the process? Or why you might want to include
a new testing type in your stack? I find it astounding the number of people I've run into who can not explain the when
and why you would use a certain test type. I also am in the job market and in the interview process have been 
commonly asked to explain various testing types and why you would use them (ie. you can use this post to brush up on your
interview skills). So this post is intended to be a summary of all the various testing types in a single post and why
you may or may not want to use them.

### Unit Testing

Let's start with the most common type of testing in development the one anyone in the field should be familiar with and 
that is Unit Testing. So first lets sum up unit testing in a single sentence ...
***Unit Testing is testing how YOU think it works.*** 
To expound on that statement a bit more and the reasoning behind it. The reason is, it is testing how you think it works,
because you are not actually testing any functionality. In a unit test the dependencies of the action are mocked (ie.
using fake resources), so what you are actually doing with the test is validating how you thought about the problem. It
is forcing you to check your own assumptions, your own logical process steps, attempting to bring to light flaws in
how you thought about the problem.

In general this is a great way to make you think about all the possible cases that a piece of code can run through and
helps you identify edge cases that may not have obvious on the task you started with.

#### Where is this applied?

You may see this type of testing applied at the following levels
- backend
- frontend

### Integration Testing (Live Testing)

The next most common type of testing and one I would still expect almost everyone to be familiar with is Integration Testing.
So same as above lets sum up integration testing in a single line ...
***Integration testing is testing if the code ACTUALLY works.*** 
This is the case as unlike unit testing, integration tests do not (or should not, as then they are not integration tests)
mock out resources. Stated a different way integration tests, test functionality like the piece of code was actually running
with all the resources from the entry point of the code on down. It behaves like a micro instantiation of your application.

*Note: Once in a while I have heard this referred to as "live" testing and this debate has caused a number of process issues
at various workplaces.*

#### Where is this applied?

You may see this type of testing applied at the following levels
- backend
- It may be possible to do this specifically on the frontend, but I have never personally seen or done it at that level

### Thintegration / WireMock Testing

This type of testing is not as common as the previous two types and depends a bit on your codes specific use case. It is
almost a hybridization of unit and integration testing. This is most commonly applicable when your application is calling
an outside resource API. It functions like an integration test in every way, meaning that it starts up and provides all 
the application resources just like an integration test, there are no mocks of internal resources BUT you do mock
the response from the external third party. So it is a live test of YOUR code but the response is fake.

So why would you want to do this? There are actually a couple of possibilities here ...

- Integration testing is significantly slower than unit testing and this can improve performance by not having to wait for the outside resource.
- This allows you to test scenarios of what happens when you get unexpected responses from the outside party
- You may have call limits on the outside resource
- You may only have access to one or two variations of the outside resource (dev/prod) and may not be able to test against it in all of your environments

#### Where is this applied?

You may see this type of testing applied at the following levels
- backend
- It may be possible to do this specifically on the frontend, but I have never personally seen or done it at that level

### API / Smoke / Load / Black / White Box / Functional Testing

Now this level of testing gets fascinating and has a lot of various names and some gray areas depending on what
exactly it is you are trying to achieve. So I will start with what I think is the most generalized variation here.

#### Functional Testing

Really any of the testing types listed under this main header can be considered "Functional" tests; as functional tests
are focused on the business requirements of the applications, ie for a certain input what do you get in output? None of
the tests above are concerned (or have the ability) to check the internal state, they only want to know they get valid
results for valid input.

#### API Testing

I think this is the most generalized form of all the above titles and what it is should be pretty clear from the name.
You are hitting an API in your code externally just like a real user would to see what responses you get. To keep with the
above and to try to sum it into a single statement
***API testing is testing like a real user***
In my opinion this is one of, if not the most important testing layer. My reasoning for this is if you are hitting your code like a 
real user then you are testing a real user experience and will notice things that may not be obvious at the lower levels
of testing. Sure, you may have already proved your code works but does it work in a way that is consistent and makes sense
to your users? This should reveal those details to you. Additionally, I believe this layer is super important to troubleshooting
and debugging after a product is in production. My reasoning for this is if you have tested how you think it works (unit)
and verified it actually works (integration), and verified it works as expected from a user perspective (API), chances
that you missed an edge case are minimal which should help narrow down how an issue occurred. If you had an issue in production,
and you have these 3 layers of testing in place that should (could) indicate that the bug is in the frontend code (caller code),
is a data issue, or possibly an infrastructure issue. In short, it helps narrow your bug search, which should further be
minimized by reviewing if there are any test cases not in place and good logging.

Finally, I also 110% believe this level of testing should still be on the
development team to provide and NOT qa. I don't mean to hate on qa too much but only twice in my 16 year
career can I truthfully say that I believe qa has had the technical ability to provide true and honest feedback on the product.
Most times in my personal career experience I have found that qa is just someone with good business knowledge who was 
pushed into the position and while they can look at a product with a business view they lack the technical chops to sufficiently
test or give good feedback to the developers. Also, this style of testing is easily achievable using the exact same testing
framework that the dev has already used to write both the unit and integration testing (from my perspective JUnit).
There is no reason for a business to invest in additional tooling to this point as this can all be achieved with what
your devs are already working with. For these reasons I believe that attempting to do better automated product 
validation remains with the developer and should be a tier 1 consideration on any new product and should be budgeted for.

*Note: API tests execute on deployed artifact. Ie. these tests run after your application is deployed.*

##### Where is this applied?

You may see this type of testing applied at the following levels
- backend

#### Smoke Testing

***Smoke testing is the process of testing a deployed artifact externally to validate that the responses received are what you expected.***
Smoke testing gets a little weird and can be applied in several ways. This could be applied as a statement to the whole 
product or could be a subset and applied only to the API. It is completely reasonable to say you want to smoke test your
API. If you are talking just about smoke testing your API and you run your API tests ONCE after you have deployed then 
your API tests can also be considered smoke tests. Or you could be talking about smoke testing the entire product end to
end using a tool such as Selenium or Cucumber which we will touch on later.

*Note: Depending on setup or use case this may include e2e testing"*

##### Where is this applied?

You may see this type of testing applied at the following levels
- backend
- frontend
- combined / End to End (e2e)

#### Load (Stress) Testing

***Load testing is the process of seeing how your code responds under expected and unexpected load*** 
Same as smoke testing this could be viewed several ways, it could be load testing of an entire application or just part
of the application like the API on down. Load testing is also a bit unique as there are a number of tools that allow you
to run unique tests such as various numbers of users on multiple nodes and to play with responses times and such. 
If you have written your API/Smoke tests using JUnit it is completely possible to re-leverage those same tests to get 
basic load metrics from your application. They may not be as thorough as some other frameworks, but it is completely
possible without adding any additional tools or costs to a project.

##### Where is this applied?

You may see this type of testing applied at the following levels
- backend
- Can you load test a frontend? Some frontend specific people let me know.
- combined / End to End (e2e)

#### Black / White (Clear/Open/Transparent/Glass) Box Testing (who)

Pardon the pun but how you define black / white box is a bit of a gray area ... (hahahaha). Again, speaking from the API
on down and to create a simple summation ...
***If the dev wrote the above API/Smoke tests it is technically a white box test as the dev is privy to the inner workings
of the code, but if someone external wrote the API/Smoke test (ie QA) then it would be considered a black box test since
they may not be able to see the inner workings.***
In my opinion this break down is mostly about the "who" is doing the test. The advantage of having your devs write tests
at this level is they can attempt to write solid, automated tests for all inbound data and response types, so you have great
coverage (white box). But, a disadvantage to this is they can miss cases because they are too close to it and may be blind to edge
cases because if they haven't thought of it through 3 test levels they probably won't. Which can be a reason to have QA
write these sorts of tests (black box) to prevent that sort of blindness. In short, this testing designation is way more
about the **who** and not so much about the **how**.

##### Where is this applied?

You may see this type of testing applied at the following levels - again this is about **who** not **how**.
- backend
- frontend
- combined / End to End (e2e)

### Frontend Testing

I can't speak as thoroughly at this level as I have always been more backend leaning and the last time I was on a project
that was heavy frontend there wasn't much in the way you could do for programmatic testing of frontend code. As we've already covered in
several previous sections it is possible, and you absolutely should also write unit tests for your frontend code. As with
the API tests investing this extra time will significantly reduce your debug time when an issue occurs.

#### Where is this applied?

You may see this type of testing applied at the following levels - kinda implied in the name
- frontend

### Regression Testing (when)

Similar to the designation of Black / White box testing regression testing is more about **when** than **who** or **how**.
Regression testing normally ocurrs shortly before a new release should go out and is intended to make sure no issues
(or no new issues) will be released with the software. Sometimes a company may have a whole deployment environment dedicated
to regression that is only updated once a release cycle, that way they always have a separate environment to compare any
issues that may happen in prod to. Many times this testing is manually performed by QA but if you have a really solid
API/Smoke test suite it is entirely possible to use a successful run of that test layer to be considered a successful 
regression run. Also be

#### Where is this applied?

You may see this type of testing applied at the following levels - again this is about **when**.
- backend
- frontend
- combined / End to End (e2e)

### Selenium / Cucumber Testing / End-to-End / System Testing

*Note: All tests at this level can be considered a form of **Black Box** testing* 

This is kind of the crème de la crème of testing if you ever reach this point. This is programming tests to be able to 
sign in to your app, input data, and click buttons just like a real user. If you want to absolutely make sure everything 
is working, and you don't want to do any manual testing this is where you need to get to. One thing about this level of
testing is that it can be very temperamental as it relies on checking html id's and nested class declarations in order to
function; items that frequently change, meaning that tests at this level generally end up needing to be updated all the 
time. While they are super impressive to watch run and I think a good thing to strive for ... most of the time companies
/ projects never make it to this level as the overhead becomes too much. I've even seen 2 companies over my career abandon
efforts to write tests at this level in favor of more rigorous API testing due to how hard it was to maintain these tests.

Testing at this level is a major effort, and you really need to have larger buy in from the company before you start attempting
to write these. Before even considering testing at this level I would say you should have better than 85% coverage in your
unit, integration, API, and Smoke test layers. Even going a bit further I would say you would be better off striving for
85% coverage on your frontend tests before trying to write tests at this level. Reasoning behind this is if your coverage
is solid everywhere else you can use these sorts of tests to check very specific things and not have to try to use them
for coverage.

#### Where is this applied?

You may see this type of testing applied at the following levels
- combined / End to End (e2e)

### Performance Testing

Performance testing is similar to load testing but rather than seeing how the application performs with many users, this 
is geared toward raw performance in an optimal setting. So this is more like seeing how high you can set the baseline 
before you see how well it holds up when running load tests.

#### Where is this applied?

You may see this type of testing applied at the following levels
- API
- combined / End to End (e2e)

### Acceptance Testing

These tests are most commonly provided by the business and are the minimum criteria that a developer needs to meet in order
for new code to be added into the system. Normally these would be listed as part of the ticket for inbound work and 
numerous types of tests mentioned above may be written to demonstrate that these criteria have been met.

#### Alpha Testing

This is a type of acceptance testing that is generally performed by QA to validate that the developer hit all the required
development points in their ticket.

#### Beta Testing

This is a type of pre-release testing where if the developer has completed all the testing types above and a product
passed acceptance/alpha testing with QA and internal stakeholders the code may be released to a beta environment with that
would allow a select set of customers try out the new functionality.

### Other types of testing

You may on occasion run across other types of non-functional testing such as the following
- Security Testing
  - Really this should be pretty well covered if you did all the above
- Compatibility Testing
  - Checks how things work in a browser or that things were not broken between version changes
- Usability Testing
  - Is it easy for a user to operate

### Conclusion

I hope this post helps clarify some of the testing variations that exist. I may even update this post or create a part 2
later as there are still other testing variations out there and or other names I have seen that may correlate with the
above and I want to try to make this as comprehensive reference as possible to lower any confusion when conversations
are had around testing. 

### Reference
- https://www.javatpoint.com/system-testing
- https://www.guru99.com/white-box-testing.html
- https://www.softwaretestinghelp.com/types-of-software-testing/

























