# Page Views by Nadir Sehnoun for Datadog

# Table of content
<a href="#features">Features</a>

<a href="#requirements">Requirements</a>

<a href="#getting-started">Getting started</a>

<a href="#Usage">Usage</a>

<a href="#navigation">Navigation</a>

<a href="#improval">Improval</a>

<a href="#the-algorithm">The Algorithm</a>

<a href="#blacklist-filter">Blacklist filter</a>

<a href="#data-structure">Data structure</a>

<a href="#socketio">Socket.io</a>

<a href="#date-range">Date Range</a>

<a href="#blacklist-filter">Blacklist filter</a>

<a href="#data-structure">Data structure</a>

# Features
Download and Ungzip of queried file through the web interface

Write of results in local files

Web Interface

Login / Logout

Profile Section with queried page views

# Requirements

Mongod server running

Redis server running

# Getting started
<h3> Step 1 : Dependencies </h3>
Clone the project locally

<pre> npm install </pre>

<h4>If you're on a unix based system (Linux MacOS)</h4>
<pre> npm install </pre>

Make sure to have a redis-server binary by typing redis-server -v.

If you already have a mongod server running, make sure to kill it before running the following script.

In any case, there will be a ./mongod.log file logging eventual problems.
<pre> npm run config-real-os</pre>
<pre> npm run fire</pre>
<h4>If you're on windows</h4>
Make sure that you have a MongoDb Database Server running

Install Redis from here => https://redis.io/download and launch it

<h3> Step 2 : Web </h3>

Once you're all set, navigate to http://localhost:3001/

# Usage

Navigate through the web site by creating an account.

The passwords are hashed once it arrives on the server side and stored in a local database.

You are free to use whatever password you want, there is no filter

Once you're logged in, you'll see several navigation button on the header.

# Navigation
<pre>'Product' is a quick description of how I worked and what I felt and also what need improval</pre> 

<pre>'Pageviews' is the interface where you input the parameters of the data you want to analyze.</pre> 

Set a beginning and ending date.

You cannot set an ending date that happens before the starting date. Same for the hours.

Since it is a WIP, I still need to figure out how to display the data in this page as it gets computed.

<pre>Profile is the user interface where you'll find the requested pageviews.</pre>

Clicking on them triggers an NgSelect module with all the sub-domains sorted.

The NgSelect module allows you to search and select any sub-domain contained in the pageview previously computed.

Select a sub-domain triggers a table component displaying major informations on the page view.

You can delete any selected sub-domain from the table.

<pre>During the computing, if a pageview failed to be computed or to be downloaded, you'll also find it in the profile section with a 
tooltip allowing you to re-compute it.</pre>

# Improval

A lot of things here need to be improved. Actually, everything.

As I have explained in the code comments, I had several choices to do based on the situation.

I really wanted to focus on the Fullstack aspect of the project rather than front or back end. More than anything, and even if it is a technical test,
I wanted to enjoy working on it and doing it my way.

Obviously the time constraints made me make quick and dirty choices which made the ensemble pretty fragile, but I am convinced that
with time, it shall become a very powerful tool.

# The Algorithm

As you'll notice quickly, the algorithm I implemented is pretty basic. It is more a brute force based algorithm than anything else.
I came up with the idea of splitting the original pageview file to be downloaded into several sub-domain files.

This method appeared to me with several advantages, one of the main ones being the blacklist filtering.

# Blacklist filter

Indeed, as the original file will be splitted in sub-domain files, so will the blacklist file. Therefore, each time I'll compute a sub-domain file to retrieve
the top 25 viewed pages, in order to filter the blacklisted pages out from it, I'll just have to read the same sub-domain blacklist file that corresponds.

# Data structure

I chose to make this project with Node.js and Angular.
I am pretty familiar with both and I love Typescript. It has revealed itself as being quite helpful in this project.

As you'll notice in the code, the main algorithmic part will be dealing with javascript object types. Not arrays, not strings but objects.

The advantage of this choice is the targetting.

Obviously, the most consuming task for a filtering project is browsing. Browsing through arrays, browsing through files, etc...

I was already doing enough browsing with all the file reading and file copying, so I thought about something easier.

I stored the listed pages inside an object, a container, and indexed it with the name of the page rather than a number.

This provided me with one major advantage for the blacklist and most viewed pages filter :

Instead of browsing through a whole array of pages, I'd just get a page with it's name, this is a space complexity of O(1) instead of O(n).

Obviously, in order to build that kind of object, I had to pass through a O(n) complexity. But this is already making me win time and processing power.

If I had more time, I'd think about a way to read the data incoming by chunks, instead of whole files. Then, I'd sort and merge each chunks as they come.

Obviously, Node.js has its strengths and asynchronicity greatly helped through this exercise, but I feel like there would have been far better technologies for this
kind of task.

# Socket.io

I chose to implement socket.io in order to communicate with the client each time a sub-domain is computed.

This has the advantage of keeping the user updated with the process rather than making him wait until the whole computing is done.


# Date Range

For now, it just loops between the start and end date and computes each file within the range.

It was pretty hard to implement that feature along with HTTP requests since the headers are sent back to the client after the first date processed.

I had to use socket.io once again to update the client with the dates remaining.

# Questions : 

<h3>What additional things would you want to operate this application in a production setting?</h3>
In a production setting, I'll first improve the algorithm, like I said earlier, I'd compute by chunks received.
Then, I'd certainly think about a way to efficiently provide the data to the user.

My take on that is if the algorithm was better, the socket.io module would have been a lot easier to integrate and therefore,
it'd have correctly restituted the data without overloading issues.

I think that about the file download and ungzipping.I did pretty good.

For the ungzipping, since a node library would consumed a lot of memory during the treatment, nothing beats a bash level
command.

To be honest, I think that Node isn't that bad compared to Python or C for this task. The only 'problem' was the 
filtering algorithm.

<h3>What might change about your solution if this application needed to run automatically for each hour of the day?</h3>

I didn't search a lot about scheduling tasks for Node.js during this exercise, but I've been working at my current job 
on automating a user data scrapping feature with Java.

One of the first thing I'd implement for an automated task are logs.
Since it is an automated task, we constantly need to have monitoring data as it is running hour after hours.

I'd also add notifications. One needs to know exactly when an error occurred or when the task worked correctly.

Obviously, since it needs to be run every hour, the algorithm should change. That task needs to be fast and work smoothly.

<h3>How would you test this application?</h3>
One of the good practice is to implement unit tests as the developments are ongoing.

I didn't do that for this exercise, and I'm sorry to not being able to present it to you.

One of the reasons on why I didn't do it is because I focused a lot on the main features and as time advanced,
I found myself with higher priorities than unit tests.

If I had implemented them, I'd proceed this way :

Since it is a RESTful application, I'd first test endpoints and apis in order to be certain that whatever the user
input is, it would be correctly treated and error cases managed.

Then the download and ungzip of the pageview file.  What if the file doesn't exists ? Or what if the data inside is
corrupted ? Can we work with the kind of data retrieved ?
All of this needs to be tested before providing the algorithm with the file data.

After that, the algorithm should be tested. Would it work in all edge cases ?
What if, for example, instead of putting the number of views at the third position of the line, it is put on the second
position ? Would the algorithm still work ?
This would be a strong part of the testing, since the algorithm should be solid and work as expected.

Once we're done with the algorithm, I'd test how the data is sent.
Since I send to the user each sub-domain treated once it ready, I'll need to be sure that it is the correct output sent.
I'll also test the correct reception of EventEmitter signals, since it is a main feature of my app.

<h3>How you’d improve on this application design?</h3>
Since I wanted to focus on the Fullstack aspect of the project, the front part also took time to build.
The user and file management features, the profile section where the user would find and recompute Pageviews that failed,
the way the sub-domains are received, all of that took a lot of time.

But as you can see, it is still basic.
I'd certainly improve the architecture of the front in order to make the website chubby, energized and not appear empty or 
responseless from time to time.

I'd work on a digital identity for the app that matches its core features.
I'd improve the way the app hosts several users.

Obviously, two weeks for this task is too few and I know that the goal of your test wasn't necessarily to have a top
10 application but rather to test how I fare with the treatment of huge loads of data.

I can do a lot better with more time, and I really loved working on this exercise.

I hope you'll like it.

    


