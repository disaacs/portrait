# Coding Challenge

## Dave Isaacs, March 2019

For the coding challenge I decided to tackle the Customer Model enhancement. I liked this challenge because it covered a lot of different areas
* New model associations and controllers
* Session managements
* Password security—those plaintext passwords really bothered me :)

The guts of my implementation can be found in [pull request #1: Adding Customer model and session support](https://github.com/disaacs/portrait/pull/1). There are a lot of details in the PR, and I have not squashed any of my commits. I will not repeat those details here.

I also kept an [online scratch document](https://docs.google.com/document/d/1vwKcfG2NzHhSqQ-_9rvDl_5SOd1bQMZG0x5_W8q3jNk/edit?usp=sharing), which I used to capture some of my thoughts and progress.

I started by getting to know the application. I reviewed the source and drew a model diagram, just to get myself oriented. I then created a Docker container to work in, mounting the forked repo on my host, so I could run in an isolated Linux environment and still use my favourite editor (VS Code).

I am not very experienced with NPM, and even less so with Puppeteer, so I spent a little time researching the topics and reviewing the JS code in the repo more closely. There was a bit of a struggle getting headless Chrome to launch: there were a lot of missing dependencies. But I got it all running eventually.

The first programming task I assigned myself was getting the new Customer model integrated into the application. I estimated 4 hours, and it took my way *way* **WAY** longer. I have to be honest and say that the real roadblock was that my Rails is rusty. The last time I did any serious Rails development was in 2017. It took me a while to get back into the "Rails way" of doing this. The last Rails project I worked on was on version 4.2, so some of the features of Rails 5.2 were new to me.

The next task was setting up some simple session management. This was required since users now required 3 pieces of information to login: Customer name, User name, and password. HTTP Authentication only handles username/password. Since I was now more comfortable with my Rails knowledge, this went smoother. Again, I estimated 4 hours. It did take me longer than that, but I did spend a lot of time fiddling around with the UI aspects, which had to be done simultaneously while re-arranging the authentication flow in the ApplicationController.

The final task was setting up secure passwords. Those plain text passwords were really bothering me. Rails has built-in support for handling secure passwords, which made this pretty straightforward. I estimated a couple of hours and, other than a deep rabbit-hole with getting bcrypt to work properly, that is about how long it took. There were a lot of tests (all of them I think) broken by this change, since the whole rspec authentication model has to be reworked. I thought I'd be able to set the session directly, but that no longer works in Rails 5. I had to simulate a post to the new session controller method to log in for each test. One major shortcut I took was NOT designing a migration that ported existing user password to the new model. I just did the migration and then blew away the database and recreated it from scratch (i.e., the ol' nuke and pave using db:reset).

I do have a list of "known issues" concerning the work I have done:

* Fix bug: non-admin users cannot edit themselves (including changing password)
* Admin users should see all Sites from all users of the same Customer
* Enhance the session management to use a randomized token instead of the user_id (steps up the security a bit).
* More elegant way to pass admin_required parameter to user_required hook.
* Edit customer page
* Allow customers to cancel accounts


I had a good time working on this challenge. I had forgotten how much fun Rails can be to work with.

Thanks, and cheers!

Dave Isaacs