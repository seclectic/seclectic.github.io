---
layout: post
title:  "My OSCP Journey: Passing Without 100 Points Or Superhuman Speed"
date:   2024-12-28 12:26:01 -0500
categories: certifications
---

In this post, I’ll share my experience navigating the PWK learning path and OSCP certification process with full transparency—highlighting my struggles, failures, and how those challenges ultimately became the most rewarding aspects of the journey.

# Introduction

In the summer of 2020, I began my journey to study for and earn the Offensive Security Certified Professional (OSCP) certification from Offensive Security (OffSec). This entry-level certification in penetration testing covers topics like web application exploitation, privilege escalation, and buffer overflows. The official training course for this certification is PWK (Penetration Testing with Kali Linux), which is supposed to provide the foundational knowledge and hands-on practice needed to succeed.

# Why OSCP?

Many people are drawn to cybersecurity because they’re fascinated by the hacker persona: taking over remote systems and using techniques that seem almost magical. I was one of those kids. Back in the late 90s, a French hacker nicknamed "Rhylkim" had a website where he shared his passion for hacking, phreaking, carding, and more. That’s how I got hooked and earned my first stripes. In those early days, there were no virtual machines, I had to build a home lab to practice in my room using two old computers connected by a twisted Ethernet cable. I spent countless hours testing different operating systems and tweaking the few exploits I could find—whether from forums, peers on IRC, or packetstormsecurity.com. I was so immersed that an hour felt like ten minutes. I lost count of the all-nighters I pulled in that home lab, with Ideal J playing in the background (every good story needs a good soundtrack). 

So two decades later, when I decided to switch to a career in cybersecurity, my first goal was to pick up where I left off—and there was now a name for it: penetration testing. I was one year into a Governance, Risk, and Compliance (GRC) job when I decided to enroll in the OSCP program. It was a way of breaking the monotony of GRC work with something more appealing to me. Plus at the time, the OSCP certification had good market value (I think it's still the case today) and was widely regarded as an excellent way to secure interviews for entry-level or junior pentester roles. I saw it as a potential pathway to transition into a more technical, hands-on role later in my career if I chose to.

# My Learning Plan

When I decided to pursue the OSCP certification, my initial plan was to exclusively rely on the materials provided by OffSec, practice diligently in their labs, and attempt the exam. I wanted to avoid third-party platforms or additional resources, aiming to keep things cheap, straightforward, focused, and manageable. However, this approach didn’t work out well for me due to the PWK pricing model at the time and a limited budget.

## RTFM

The official course material was particularly helpful in clarifying various attack techniques. For example, as a Linux enthusiast, I had always found privilege escalation on Windows systems more challenging, the PWK book provided valuable insights that helped me improve my skills in that area. I especially enjoyed the sections on stack overflows and data exfiltration.

## Watch the Videos

I committed to watching the accompanying videos, expecting they might offer extra insights beyond what was covered in the PDFs. They didn’t. For the most part, the videos repeated the written material. However, I chose to treat them as a second lecture—a chance to reinforce what I’d already learned.

I’m not sure I’d take the same approach again if faced with a similar situation. In hindsight, I’d probably dedicate that time to more hands-on exercises. Practical application often yields greater value, especially when preparing for something as technical as the OSCP.

## Hands-On Training

The hands-on lab experience was by far the most enjoyable part of the training. However, due to budget constraints, my only option for the PWK training was to get the 90-day lab bundle (approx. $1000 in 2020, $1,649 in 2024), which turned out to be way too short for me. I couldn’t even complete half of the challenges before my lab time expired. Perhaps 90 days would be sufficient if you’re able to train full-time or if you have a strong pentesting background and only need to focus on a few topics in the curriculum.

The PWK lab extension pricing model ($359 for an additional 30 days) didn’t work well for me—it was too expensive and too short—so I began exploring other options.

N.B: Since then, OffSec has updated their pricing model and now offers a package ([Learn One](https://www.offsec.com/products/learn-one/)) with 1 year of lab access and 2 exam attempts for $2,079. I think this pricing model is much more adapted to the PWK/OSCP training.

While browsing Reddit, I discovered the NetSecFocus PWK resources, which included a [comprehensive list of CTF-like websites with labs mapped to OSCP/PWK skills](https://docs.google.com/spreadsheets/u/1/d/1dwSMIAPIam0PuRBkCiDI88pU3yzrqqHkDtBngUHNCw8/htmlview). One of the recommended platforms was Hack The Box (HTB)—a platform where I already had an account.

I decided to upgrade to a HTB VIP membership for $20/month (no commitment & month to month) and delve in. While the affordable pricing was appealing, the greater value came from [IppSec's library of HTB walkthroughs](https://www.youtube.com/@ippsec). His content is an incredible resource for aspiring pentesters and the curious alike.

## Exam Details

The OSCP exam is infamous for its length. Candidates have 24 hours (actually 23 hours and 45 minutes) to break into and possibly take full control of 5 servers (the new version of the exam now includes an AD lab), followed by an additional 24 hours to draft a penetration test report. This level of intensity was new to me. I had pulled many all-nighters (aka "hack nights") doing untimed CTFs in my life, but nothing compared to this. Additionally, I had only attempted AWS and CompTIA certifications up to that point, with exams that typically last no longer than 2–3 hours and consist of multiple-choice questions.

### Scoring and Point Distribution

You need to score 70/100 to pass. The server points are distributed as follows:

- **Two 25-point machines**: one 25-point machine is dedicated to the buffer overflow (BoF) challenge
- **Two 20-point machines**
- **One 10-point machine**

The more points a server is worth, the more complex the challenge.

## Exam Strategy

I would begin with the BoF machine, as it’s the most predictable challenge. This will give me time to perform scans and enumerate the other machines in the background. To streamline the process, I created a very basic and dirty bash script to automate scans. Here is a breakdown of the tools it ran:

- **Nmap TCP SynScan** on all ports  
- **Nmap UDP** on all ports  
- **Nmap Banner Analysis** and default scripts on open ports  
- **Gobuster** on any services responding to HTTP requests  

### Using Metasploit

Note that you can also use Metasploit during the exam; however, it can only be used once. This could come in handy if you're struggling to find an exploit on the internet.

# 1st Attempt - 03/2021 - Enough Points to Pass but ...

By the time I reached the 8-month mark in my preparation, I had worked through all the official resources and about half of the VMs listed in the NetSecFocus spreadsheet. I felt like I was ready to take a shot at the OSCP exam. I scheduled my exam, which I decided to start it in the morning, at 7:00 AM.

On my first attempt, I managed to score the required 70 points within the 24-hour exam period. While I theoretically had enough points to pass, my note-taking methodology wasn’t great, and I was probably missing a few screenshots. Convinced I could achieve a perfect 100—and having enjoyed the exam experience—I chose not to write or submit the report, forfeiting this attempt. You might think I’m a bit of a masochist, but for me, the dopamine rush from the exam more than justified the $249 expense of retaking it. 

At the time, I was confident I’d only need to do it one more time.

## Fixing My Note-Taking Methodology

My first takeaway from this initial attempt was that I needed to improve and refine my note-taking technique. I fully embraced the practice of "Document-As-You-Go." This approach is essential for succeeding with the OSCP certification (and I’d recommend it for work as well). Every step of the process is documented on the fly. In such an intense situation as a 24-hour hands-on exam, failing to proceed this way could lead to overlooking important details.

I opted for **Obsidian** (Nb: I am using Apple Notes for work), a note-taking tool I discovered through IppSec videos. I appreciated its Markdown support and the ability to paste screenshots directly into the notes. I prepared a note template, I'd use it for each machine. It included the following sections:

1. Executive Summary  
2. Enumeration  
3. Exploitation  
4. Post-Exploitation Enumeration  
5. Privilege Escalation  
6. Loot  

## Cooling-Off Period

After failing the exam for the first time, OffSec enforces a cooling-off period of 4 weeks before candidates are allowed to retake it. I was convinced that improving my note-taking methodology and leveraging my experience from the first attempt would give me an edge, so I didn’t study more than usual between the two attempts.

# 2nd Attempt - 04/2021 - The Reality Check  

I decided to start in the morning again, this time at 8:00 AM.

On my second attempt, I stumbled on two boxes, encountering vulnerabilities I couldn’t fully grasp and wasn’t comfortable with. It threw me off. 

In a real pentest scenario, these are the kinds of vulnerabilities you’d step away from and revisit with a fresh perspective the next day. But that approach doesn’t work in a time-constrained exam, where the stress compounds and makes it much harder to think clearly.

OffSec’s slogan is "try harder," as if the struggle and failure are designed to be part of the process—I should have seen it coming. I fought until the very last minute but couldn’t score more than 50 points. It hurt.

The cooling-off period after the second failure was eight weeks. I would use that time wisely.

# Reflections On My First Two Attempts

This second failure humbled me. Some might say failing twice was just bad luck—that I happened to stumble upon a harder version of the exam, or one that was more challenging for me personally. Maybe that’s true, but it didn’t matter to me. Let’s call it what it is, a fail is a fail.

After spending more years in the field and having had the chance to lead and conduct multiple pentests across different roles, I now recognize this moment as similar to when a pentester hasn’t found any vulnerabilities yet or failed to weaponize an exploit, and doubt reaches its peak. A pentester or student often wrestles with painful but honest questions:

- Am I cut out for this?  
- Am I missing key skills?  
- Is my methodology flawed?  
- Am I putting enough effort into it?  
- Why am I doing this? Is it for the knowledge and skills, or to enhance my resume or LinkedIn profile?  
- Am I trying to prove something—to myself, or to others?

Reflecting on these questions helped me clarify my objectives and identify what I needed to fix in my approach.

## Motivation

One thing I knew was that motivation did not need a fix. I’ve been a martial artist for most of my life, competing in numerous tournaments throughout my youth. Those experiences instilled in me a strong sense of determination. I believe determination is essential for any human being, but it’s even more critical for security professionals—and absolutely vital for pentesters.

My drive for strong determination comes from pride. Pride does not mean arrogance. A security professional can leverage both humility (accepting the fall) and pride (always getting back up) to drive progress.

## Re-Prioritizing My Objectives

I decided to make acquiring knowledge my top priority. Everything else, including earning the certification, became secondary. Nobody knew I was studying for the OSCP—not my employer, my friends, or anyone else. I had no external pressure. I would focus on one concept at a time, allowing it to fully sink in before moving on. 

A helpful and simple metric I adopted was asking myself:

- Can I explain this concept clearly to someone else?
- Do I know how to detect such vulnerability?
- Do I know how to exploit it?

If the answer was no, I wasn’t ready to move forward. I'd seize every opportunity at work and in my personal life to challenge myself and test me against that metric.

## Fixing My Learning Style & My Pace

Many blogs and articles describe incredible levels of engagement, meticulous planning, and seemingly flawless execution. I respect those who can pull that off, but it’s important not to confuse intensity with unrealistic timeframes. The trend of “get rich fast” or “become a cybersecurity engineer or a full-stack developer in six months” often prioritizes risk, speed, and intensity over meaningful learning.

Certification and training companies don’t make it any easier—they take advantage of this by incentivizing short-term training programs, betting on your failure and the likelihood of you paying for costly extensions and additional attempts.

As time goes on, we tend to accumulate more responsibilities while still being constrained by the same 24-hour bandwidth. These phenomena conflict and present us with difficult dilemmas.

My attempt to aim for a "no compromise" approach by blending hardcore training with an already demanding life only made the experience more painful and resulted in lower knowledge retention. It’s also not healthy—at a certain point in life, all-nighters become much harder to recover from.

For anyone considering a similar path, I encourage you to reflect on your own constraints, goals, and priorities. You don’t need to follow high-pressure routines just because they’re popular or other people do it. Instead, choose an approach that fits your life—one that is sustainable, rewarding, and aligned with your pace. After all, **this journey is meant to enhance your life, not take it over.**

I decided to switch strategies and embrace a **"paced and consistent learning"** approach. I went back to my books, cleared the remaining NetSecFocus HTB boxes, watched more IppSec videos. I did all of this at my pace, and to me, a schedule of 60 minutes a day every other day proved far more effective—and less exhausting—than cramming for six hours every weekend and missing family/friends quality time. 

If I could give my younger self one piece of advice, it would be to prioritize steady progress over cramming sessions. No matter your age, I believe **consistency beats intensity every time**. That said, if you’re in a stage of life where you have plenty of energy, time, and a supportive environment, you can combine consistency with bursts of intensity to reach your peak. Take advantage of that balance as long as it’s sustainable, and you’ll be unstoppable.

The final clue that proved to me this shift was the right move is that it re-introduced the fun. Fun is also a great metric—it’s commonly agreed that [one learns better when they play](https://ssec.si.edu/stemvisions-blog/5-benefits-gamification).

# 3rd Attempt - 11/2021

7 months after my second attempt, I was finally ready for the last dance. I implemented one last small change: I decided to start at 12:00 PM. This would give me the chance to sleep at night and approach the next morning with a fresh mind to tackle any challenges I couldn’t crack right away.

About 20 hours in, I had enough points to pass (75/100). I kept pushing for the full 100 points, but that didn’t work out. After being humbled by my earlier attempts, this still felt like a great victory.

I rewarded myself with some well-deserved sleep before tackling the final step: the report.

**Note:** I did not use Metasploit. 

# Writing the Report

When I woke up, I dove straight into grinding on the report. Back then, there was no generalized, publicly available GenAI, so everything had to be written and reviewed manually. I stuck to the template provided by OffSec. This time, my notes were clean and well-organized, thanks to the Document-As-You-Go methodology, so all I had to do was copy, paste, and polish the format of the document.

# The Verdict

A few days later, I received the email informing me that I had passed.

# Lessons Learned  

My journey to earning the OSCP taught me technical skills—I don’t want to downplay that aspect—and that could easily be the subject of another post. However, the biggest takeaways for me were found elsewhere:

1. **Be Accountable**: Remember the [Hacker's Manifesto](https://phrack.org/issues/7/3)—computers don’t make mistakes; the human in front of them does. Acknowledging your failures and shortcomings, and learning from them, is essential for growth, especially during training. I am not ashamed or afraid to admit that I’ve failed or don’t know something. We don’t know everything; we teach, we learn, and sometimes we fail because we have the merit of trying.

2. **Resilience and Adaptability**: Failure can be humbling and expensive, but it builds character. Each attempt forced me to reassess my strategies, identify weaknesses, and adapt. This [resilience](https://product-images.tcgplayer.com/4146.jpg) is crucial not only for exams but also in professional and personal life. Don't know about you, but when failures cost money, the lessons learned retention lasts longer. The OSCP journey wasn’t just a test of technical expertise; it was a test of character. Their "Try Harder" slogan is not usurped at all.

3. **The Value of Methodology**: Clear, repeatable methods are invaluable for both exams and real-world activities. Automating enumeration and embracing Document-As-You-Go were game changers.

4. **Focus on Quality, Don’t Be Afraid to Study Over a Year**: Balancing work, family, and study time meant it took me longer to prepare. That’s fair—don’t sacrifice the quality of those hours for the sake of speed in earning a certification. Consistency is key. I personally focus on steady, sustainable progress, which I believe is key to building a healthy and long-lasting career.

5. **The Journey is the Reward**: Certifications validate your skills, get you a seat at the recruiter's table, but the real value lies in the growth, knowledge, and confidence you gain along the way.

# Conclusion

This story of bouncing back stronger from failure often comes in handy when I need to encourage a teammate who has failed a certification exam. The continuous learning required in the information security industry demands both experience with learning methodologies and strong determination (and money). I’m grateful that I experienced failure early in my life, not just in my career, as it allowed me to reform myself and make self-reflection and improvement an ongoing process. Since this journey, I’ve applied these lessons to study for and successfully pass multiple other certifications, including the CISSP and CIPP/E.

I am not a pentester and no longer aspire to become one, having since transitioned to Security Engineering. However, the skills I gained through PWK/OSCP have proven invaluable for internal pentesting and vulnerability management, as well as when collaborating with third-party pentest providers. These skills enable more effective scoping and ensure findings are rapidly triaged, well-documented, and easily understood by engineers for swift remediation. They have also been instrumental in supporting purple team exercises by bridging the gap between offensive and defensive strategies.

# Resources

- [OSCP - OffSec](https://www.offsec.com/courses/pen-200/)
- [OSCP Exam Guide](https://help.offsec.com/hc/en-us/articles/360040165632-OSCP-Exam-Guide-Newly-Updated)
- [Hack the Box](https://app.hackthebox.com)
- [IppSec YouTube Channel](https://www.youtube.com/@ippsec)
- [NetSecFocus](https://docs.google.com/spreadsheets/u/1/d/1dwSMIAPIam0PuRBkCiDI88pU3yzrqqHkDtBngUHNCw8/htmlview)
- [OffSec report templates](https://help.offsec.com/hc/en-us/articles/360046787731-PEN-200-Reporting-Requirements#pwk-report-templates)