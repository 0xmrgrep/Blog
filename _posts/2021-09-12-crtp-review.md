---
title: My CRTP Review
author: Abhisar Pandey[MrGrep]
date: 2021-09-12 11:33:00 +0800
categories: [Exam Review, CRTP]
tags: [crtp ,crtp review, active directory, certified red team professional, AD]
math: true
mermaid: true
image:
  path: /assets/img/crtp.jpg
  width: 850
  height: 585
---

In this post  I have discussed about [**CRTP Course**](https://www.pentesteracademy.com/activedirectorylab) offered by the [Pentester Academy](https://www.pentesteracademy.com/).
<br>

## Introduction
Hello reader!! I am Abhisar Pandey aka [MrGrep](https://twitter.com/imabhisarpandey). I am currently B.Tech. final year computer science and engineering student. Before CRTP I have done CEH(Practical) Certification back in 2020. I am CTF Player and CTF Developer. I use to hunt bugs sporadically.<br>
 Now let's talk about CRTP Course :D
<br>
Before starting the CRTP course, I started solving tryhackme rooms which boosted my confidence to go ahead.
After solving tryhackme rooms, watching youtube tutorials and reading some blogs related to active directory and its attack, I decided to go ahead and purchase Certified Red Team Professional course from pentester academy.<br>

## The Course
The **Course Instructor** of CRTP Certification is [**Nikhil Mittal**](https://twitter.com/nikhil_mitt?s=20) who is creator of [Nishang](https://github.com/samratashok/Nishang), [Kautilya](https://github.com/samratashok/Kautilya) and many other tools. Once I purchased the course, I got my account dashboard login instructions within 24 hours of purchase. I got my mail from Pentester academy CRTP Lab Team and thereafter I logged in the dashboard and finally got my course materials consisting of PDF Slides, videos and tools. In the meantime I sent an email to the Lab Team informing that I would start my lab after making my notes for the course. On 16 August, 2021 I sent an email and requested for activating my lab. The Lab Team, on my request, activated my lab access on my account dashboard.
I got 2 ways to access the lab:
- Either Web Browser based(Chrome Browser Preferred)
- Via RDP(after connecting vpn)
<br>

## About the Exam
If I refer to the OSCP as a point of reference for CRTP, just like the OSCP, the CRTP exam is also a 24 hours long exam but here one gets 48 hours for writing the report after finishing the exam whereas OSCP exam provides 24 hours of time to submit the report. The CRTP exam lab consists of 5 machines with a user machine. The main aim of solving a machine is to achieving code execution on the available machines, although not necessarily as an administrator.<br>
For Report writing there's no specific template provided by Pentester Academy but one can follow [Offensive Security Report](https://www.offensive-security.com/reports/penetration-testing-sample-report-2013.pdf) template or any similar template of your choice.<br>
Below I am mentioning some points that you should keep in mind before purchasing the CRTP course.
## Prerequisites
Before purchasing the course I would highly suggest you to go with some blogs and tryhackme rooms listed as:<br>
- **Tryhackme Rooms**
  - [Basic AD(Paid Room)](https://tryhackme.com/room/activedirectorybasics)
  - [AttackiveDirectory(Free Room)](https://tryhackme.com/room/attacktivedirectory)
  - [Post Exploitation Basics](https://tryhackme.com/room/postexploit)
  - [Powershell for pentesters](https://tryhackme.com/room/powershellforpentesters)
  - [Attacking Kerberos](https://tryhackme.com/room/attackingkerberos)
- **Blogs To read**
  - [https://activedirectorypro.com/glossary/](https://activedirectorypro.com/glossary/)
  - [https://www.windows-active-directory.com/active-directory-fundamentals](https://www.windows-active-directory.com/active-directory-fundamentals)
  - [https://zer1t0.gitlab.io/posts/attacking_ad/](https://zer1t0.gitlab.io/posts/attacking_ad/)
<br>

## Purchase options
There are three purchase options available at Pentester Academy. They are as under:<br>

| Lab Duration  | Cost    |
|:--------------|--------:|
| 1 Month       | $249    |
| 2 Month       | $379    |
| 3 Month       | $499    |

So far as my journey is concerned, I proceeded with 1 month lab but if one can afford, I would suggest to go with 2 months of lab because there's a great learning experience one will get in solving the labs :D<br>

> Before purchasing the course, I would like to suggest to brush up your basic AD knowledge.


> Once you prepare your basics, you will be good to purchase the course. To purchase the course, please visit <https://www.pentesteracademy.com/activedirectorylab> and click on purchase options and fill the form available for it.<br>
> Please note: The lab will be deployed for the email used while purchasing the courrse. So, it is recommended to use official email id.<br>

Within 24 hours of payment, you'll get your email for the account activation and information.
You'll also find videos,slides and tools on account dashboard after login. So I recommend you to first watch those videos and make proper notes thereon and then make a request for your lab activation. Once you get your lab activated, you're good to start with lab practices.

## Preparation Tips:

- Make proper notes of commands and their uses.
- Complete all lab objective provided to you.
- If you encounter any doubt you can directly mail it to Lab Team and they'll respond you within 4 hours(during business hour) usually.
- If you think you're not good at report making, try making report for the lab objectives(for practice purpose)
- Make a checklist if possible for the attacks and tools shown in the course.

## Before Exam day

- [x] Good Internet Connection.
- [x] Review all notes once.
- [x] Download provided tools locally so that you can export in exam lab.
- [x] Download openvpn software in your machine if not have.


## During Exam

- Have patience.
- Restart machines if things not working.
- Stay hydrated.
- Eat light food and avoid unhealthy food.
- Take proper rest in between if required don't stress yourself too much.
- Take screenshots time to time for every command and its output.
- Try to write raw report while solving the exam, this way things will not be messed while report making.
- After solving all machines don't forget to press `End Exam` button from exam dashboard.
<br>

## After Exam
- You will get 48 hours of time for report making after the exam ends. So, don't rush for report making right after exam as taking some rest is necessary for making an effective report.

## For those who do not able solve all 5 machines

- If one is not able to comppromise all the 5 machines, one need not panic but make a good report with explaining all the steps for the machines compromised. Also include references along with attacks and mitigations involved.
- Do not assume yourself that you would fail if you do not able to solve more than 3 or 4 machines. Let the Lab Team decide after reviewing your report.

# Final Thoughts
My experience was really good as I learnt a lot about Active Directory enumerations, exploitation possibilities, tools and techniques. I would like to give special shoutout to the Lab Team because they are very responsive for technical support and doubts.
So, 4 day after submiting my report I got an email from Lab Team that I have successfully cleared my CRTP exam and now I am **Certified Red Team Profesional**.

<img src="/assets/img/crtpss.png" alt="CRTP" width="500" height="333">
<br>
If you have reached till here, I would like to thank you for reading my blog.
I oftenly tweet stuffs and resources related to cybersecurity topics for which you can check my [Twitter](https://twitter.com/imabhisarpandey)
<br>
If you really liked my blog and if you could afford you can consider buying me a coffee :D
<br>
<center><a href="https://www.buymeacoffee.com/0xMrGrep"><img src="https://img.buymeacoffee.com/button-api/?text=Buy me a coffee&emoji=&slug=0xMrGrep&button_colour=ff0000&font_colour=ffffff&font_family=Lato&outline_colour=ffffff&coffee_colour=FFDD00"></a></center>

