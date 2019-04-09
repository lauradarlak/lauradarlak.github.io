---
title: "On Tap at Ithaca Beer Co"
categories: [portfolio projects]
classes: wide
---

TL;DR
When scraping a website, don’t make assumptions about another person's code.

The completion of my first Ruby CLI Gem is the result of dedication and perseverance.  I anticipated that the project would provoke a range of feelings, including obsession, frustration, and joy; moreover,  I knew that my Gem would need to relate to a topic that held my interest.

The On Tap IBC gem provides the user with a list of the most current tap list available at Ithaca Beer Company. The list is sorted into two groups, standard beers and limited 5 Barrel Brews. The current tap list information is scraped from the Ithaca Beer Company online menu. The menu provides the name, a short description and ABV of each available beer.

Users are prompted to select the tap number for a beer that the user would like to learn more about. The additional beer information is scraped from one of two other pages on the Ithaca Beer site.
Prior to starting any code for this project, I spent time thinking through the architecture and how I would obtain the information that I needed.

All of the beers with additional information appear on an intermediary page on ithacabeer.com. My plan was to scrape this page for the anchor tags and match each link to the beer objects that were instantiate when scraping the menu. The links that led to the appropriate beer description pages followed this pattenr: `/pageurl#beername`​.  Since multiple beers were listed on the two beer profile pages, the ​`#beername`​ anchor is the element that leads a user to the desired beer. I assumed that `​#beername`​ was used as an CSS id for the individual container for each beer.

I spent two days figuring out how to assign each scraped anchor tag to the appropriate beer. I rejoiced when the feature worked! I was proud. The rest of the program would be a cakewalk. In the gem, once the user selected a beer to learn more about, the unique url for the beer would be supplied to a scraper method that would then create attributes for the selected beer based on the information provided in the profile. Attributes would only be assigned as needed—limiting the number of page requests made.

At this point all of my Classes and methods were communicating to each other. Unfortunately, regardless of the beer selected from the tap list, the detailed profile information supplied was for tap #1 Flower Power. I took a closer look at the HTML for the beer profile pages. That is when I realized that the unique url that I worked so hard to find and assign to the appropriate beer was irrelevant. The `id ` I was targeting for each beer led to a deeply nested h4 in a container that was used to provide padding to each beer when a user was sent to the page after selecting the beer of choice. I would need to iterate over every beer profile container for information and store those details then assign to the proper beer.

I was momentarily devastated to say the least. I knew I would miss a deadline that I had made for completing the project and that more issues would arise as I reworked my program. I kept working, and here I am. I am proud and excited to move forward!
