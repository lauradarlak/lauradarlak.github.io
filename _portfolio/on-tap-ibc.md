---
title: "On Tap at Ithaca Beer Company"
excerpt: "RUBY // NOKOGIRI"
author_profile: false
header:
  teaser: /assets/images/projects/on-tap.png
sidebar:
  - title: "Technologies"
    text: "RUBY // NOKOGIRI"
  - title: "Code"
    text: "[Github](https://github.com/lauradarlak/on_tap_ibc/tree/master)"
---
***

{% include video id="/SlqnDn4psf4" provider="youtube" %}

***

The **On Tap IBC** gem provides the user with a list of the most current tap list available at Ithaca Beer Company. The list is sorted into two groups, standard beers and limited 5 Barrel Brews. The current tap list information is scraped from the Ithaca Beer Company online menu. The menu provides the name, a short description and ABV of each available beer.

Users are prompted to select the tap number for a beer that the user would like to learn more about. The additional beer information is scraped from one of two other pages on the Ithaca Beer site.

+ Parsed raw HTML from ithacabeer.com using Nokogiri to generate dynamic tap lists and beer specs
+ Built a CLI for interfacing with the application
+ Utilized Object Oriented Ruby

***

![On Tap CLI](/assets/images/projects/on-tap.png "On Tap CLI")
