---
layout: post
title: My experience with Streamlit
subtitle: Summary of my experiences using the Streamlit dashboarding framework.
tags: [python, streamlit, plotly, dashboarding]
---


## My experience with Streamlit so far

### My history with visualization tools
I believe a visualization tool is one of the most important arrows to have in a data analyst's quiver. It allows you to summarize black box models & thousands of line of data in a single image that can be understood by any kind of an audience. In my 8 years of analytics journey so far, I have come across multiple dashboarding tools used within my companies - VBA, Qlikview, Tableau, Power BI etc. to name a few. I have personally used Tableau quite extensively and found it to be quite a powerful tool. Even though these kind of **drag and drop** tools are quite great, they don't give you a lot of flexibility in terms of handling data or how different components on a dashboard interact with each other. Tableau in particular I found to be a tool where its really easy to build simple applications, but it gets exponentially more difficult as the scope of your dashboard gets bigger and more complicated. All these tools expect the final data to be fully available and then all they do is allow you to build pretty visualizations on top and slice and dice the data in multiple ways. 

When I started learning data science more extensively last year, I found that Tableau and similar dashboarding tools to be pretty unsuitable for data science tasks where you had to deploy a model in production. These tasks require a Python/R script to be run every time a user makes a bunch of selection, therefore you would be generating fresh data for visualization with every single run. This isn't something that is possible in Tableau in a straightforward way. 

### My experience with Streamlit so far 
This prompted me to learn dashboarding applications built on top of programming languages - R, Python etc. which allowed me to deploy a model in production. I first tried out Python Dash but found it very difficult to navigate through it initially, the learning curve was a bit steep for new users and it took a week for me to able to deploy my application. 

I then came across a few tweets on Streamlit and one common thing I observed among all the mentions was the ease of use and how it was perfectly suited for data science applications. So far i've tried it out on my [Fantasy League & IPL Prediction projects](https://github.com/arpitsolanki?tab=repositories) and really enjoyed using it. 

Here are some of the things I loved about Streamlit - 
1. **Easy to use** - It is very straightforward to get an app up and running on Streamlit. For you to start working on streamlit, all you need to do is `pip install streamlit` and you can start coding right away. I was expecting it would take me days to actually learn a new web application framework, but I was able to deploy a basic version of Streamlit app within a day. Streamlit doesn't expect me to have an advanced level of CSS or JS proficiency, basic python knowledge is more than enough, which was a real blessing. I was completely focused on learning DS algorithms & math at the time and didn't want to divert my attention towards anything else that was new, therefore Streamlit was perfect for me. 
2. **Compatibility** - It is compatibe with popular Python viz libraries like Altair, Plotly, Matplotlib etc. So you don't need to learn anything new in order to create visualizations on Streamlit dashboards.
3. **Excellent Documentation** - Being an average coder, usually I struggle a lot reading through the documentations for packages and end up going to Stack overflow to get my queries answered, but Streamlit documentation is as good as I've ever seen. It is very well organized with excellent code snippets to get you started. For most of my needs so far, I've never had to look beyond their user forum and documentation sections. 
4. **Widgets** - It is extremely easy to add in new widgets to your applications. All it needs is a single line of code. You have lots of options available  - dropdowns, radio buttons, sliders, textboxes etc. As I mentioned above, the quality documentation makes it really easy for you to get started, also the dashboard gallery has a good variety of applications that can quickly familiarize you with all the widgets that are available. 
5. **Design & Layout** - This is one area where I've struggled a lot with tools like Plotly Dash due to my limited knowledge of CSS & Javascript. I've always struggled to arrange components and charts in a nice manner with dashboaring tools. Streamlit provides something called as **Beta Columns, Containers & Expanders**, which make it very easy to arrange basic components of your dashboard and make them visually appealing. 

Even though its a great tool overall, I felt it does have a few shortcomings as well - 
1. **Busy roadmap** - This is something I've noticed is quite common across popular visualization tools. As user base expands, they keep requesting for new features to support their use cases and before you know it, development team has hundreds of new feature requests. Since the development team can only do so much at a given point of time, sometimes it can take months for requests to be picked up. So be prepared for running into a wall or trying some workaround for advanced use cases.
2. **Lack of Stateless Apps** - Every time a user changes filters on the dashboard, Streamlit runs the entire python script for the application, it doesn't remember a user's past history or the current state of application. This however is something I believe will be made available pretty soon. 
3. **Lack of Security Layer** - Although Streamlit is great for building public applications where you don't want a login page or a security layer, this maybe a challenge when it comes to enterprise use cases where you want to control access. Though I believe this is a part of natural evolution of the product and its more a question of if, not when this will be made available. 
4. **Layout compatibility for Mobile Devices ** - This is something that didn't quite work out for me, though it is possible that others may have figured this out. When I opened my apps on my tablet or mobile phone, the layout was all over the place and everything looked messy. 

I personally felt that all of the challenges above were not significant enough to dissuade me from using Streamlit. 

With Streamlit you are working at a very high level of abstraction and as an end user you can be focused on your data science task at hand and not have to worry about lots of back end stuff like HTML, CSS, Javascript and all the issues that come up with dashboarding tools. I personally believe this is what makes Streamlit such an awesome tool.  Its finally great to have a visualization tool that is built with data scientists in mind. I think its safe to say that Streamlit will be my first choice for all my dashboarding needs for all my learning projects. 

*Disclaimer - As you might know, dashboarding frameworks are dynamic in nature, and it is possible that some of the things i've mentioned here may not be applicable anymore. Please treat this as a summary of end user experience and for all info on latest features on Streamlit, do visit their [website](https://streamlit.io/)* .




