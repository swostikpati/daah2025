---
title: "Post: Assignment2"
date: 2025-04-15
categories:
  - Blog
tags:
  - Spatial Data Analysis
  - Kepler
  - Overpass Turbo
---

# Tracking the HHS Cupid: A Spatial History Project

When we began this assignment, we were drawn to the idea of tracking the movements of a single ship—_HHS Cupid_—across the waters surrounding Zanzibar between **April 1919** and **January 1920**. In the end, the project ended up being about more than just plotting ports on a map; it became a deep dive into the messy realities of historical data, the limits of technology, and the ways spatial thinking can help us make sense of the past. In line with the provocations posed by Klein et al., this reminded us that while models or maps can generate outputs, it’s human interpretation that assigns meaning, especially when working with historical and cultural data. Their argument—that tools like generative models or mapping tools lack context unless informed by humanistic inquiry—matched our experience of turning old records into a story told through space.

## Why We Chose the HHS Cupid

We focused on the _HHS Cupid_, tracking its movements from **April 30, 1919** to **January 26, 1920**, because its journey was rich in spatial detail. The _Zanzibar Gazette_ frequently listed the ship’s arrivals and departures from different ports like **Zanzibar**, **Tanga**, **Weti & Chake-Chake**, **Mombasa**, and **Dar-es-Salaam**.

These movements were inherently _spatial_: each location could be _geocoded_ and tied to _coordinates on a map_. So from the very beginning, we knew this dataset had potential not just as historical text, but as something we could see—a sort of story unfolding across geography, not just time.

That’s what made this project feel **alive**. It wasn’t just a list of names and dates; it was a **trail across the Indian Ocean**, revealing patterns in trade, colonial logistics, and even bureaucratic evolution over time. These port calls had meaning—political, economic, and human—and placing them on a map would help surface those deeper stories.

From the beginning, _geocoding_ was at the heart of how we approached this work. We wanted the final output to be a visual representation of _HHS Cupid_’s journey across the Indian Ocean region, and for that, we needed each place name to link to a **latitude and longitude**.

The idea was to eventually feed our structured data into **kepler.gl**, an _open-source mapping tool_ that would let us see the ship's routes dynamically. What started as lines in a ledger would then transform into lines on a map. That transformation, however, took a lot more effort than we expected.

<!-- checkpoint -->

[PDF with screenshotted data from the Zanzibar Gazette](/daah2025/assets/files/pdf1.pdf)

## From Chaos to Structure

One of the first things we noticed when going through the _Gazette_ issues month by month was how **inconsistent** the formatting was. Early entries would list a ship arriving somewhere, but then never mention _when (or if)_ it left. Some ports would be referenced by **multiple names**—_Dar-es-Salaam_, _D’Salaam_, or _Mombasa_ vs. _M’basa_—even within the same page. It was clear that **no standardization** existed for _place names_, _punctuation_, or even the _structure of a single entry_.

For example, on **page 3** of the PDF referenced earlier, there’s an entry stating that on **Saturday, June 7**, _HHS Cupid_ left _Tanga_ and arrived at _Weti & Chake-Chake_. Then, on **Sunday, June 8**, it arrived at _Zanzibar_. But there is **no mention** of it ever leaving _Weti & Chake-Chake_. We knew it had to have departed, obviously, but the record simply skips that part. These kinds of **gaps** were frustrating but also **revealing**. They reminded us that historical data is not always tidy—and that our job was to **make sense of that ambiguity** without flattening it.

As we moved forward month by month, though, we began to notice a **gradual shift**: the records became more structured. By _late 1919_ and _early 1920_, departure and arrival data were more reliably paired, and the formatting settled into something closer to a regular schedule. This evolving structure tells a story in itself—maybe of administrative refinement, maybe of a better understanding of how shipping data should be communicated. Either way, it impacted how much we could **trust the data** and what kinds of **gaps we had to fill manually**.

## Data Extraction (Using LLMs) + Enriching

To manage the vast amount of _unstructured data_, we turned to **Large Language Models (LLMs)**—like _ChatGPT 4o_, _Claude_, _Gemini_, and _Perplexity_—to try and automate the extraction process. We gave them sections of the _Gazette_ and asked them to pull out **ship names**, **dates**, **locations**, and **directions** (_arrival_ or _departure_). _In theory_, this should have saved us hours of work. _In practice_, it turned out to be **much more complicated**.

The same port was often written in **multiple different ways**. For example, sometimes the ship was said to have gone to _“Weti & Chake-Chake,”_ while other times it was _“Chake-Chake & Mkoani.”_ On other pages, it might just say _“Weti”_ or _“Chake-Chake”_ on their own. When we looked all these up, we realized they were actually all **ports located on the same island—Pemba**. And yet, in some entries, the _Gazette_ simply said _“Pemba,”_ without naming any specific port. This kind of **variation** made it really difficult to **organize the data clearly**, especially when trying to **show it on a map**.

To handle this, we decided to **standardize the way we recorded locations**. We created a **set list of main places**—like _Zanzibar_, _Pemba_, _Mafia_, _Mombasa_, _Tanga_, and _DSalaam_ (_short for Dar-es-Salaam_)—and made sure every row in our spreadsheet referred only to one of these **standardized place names**. If there were additional ports mentioned, we listed them in a separate column for reference. This way, the main location data stayed clean and consistent for geocoding, while the details still remained available.

<!-- checkpoint -->

[Spreadsheet of extracted data](https://docs.google.com/spreadsheets/d/1_KpHkwCcPSc5CbmQ6yhHRqYUtsHw5nFvHLpgYzPD9vI/edit?usp=sharing)

We compiled all these standard place names into a list in **Sheet 2 of our Google Sheet** (linked above), where we kept track of every **city or town** that appeared and all of the smaller ports were grouped under the main places they belonged to. This was really important, especially when dealing with names that sounded almost the same. For instance, _Kilindini_ (a port in _Mombasa_) and _Kilindoni_ (a port in _Mafia_) are just one letter apart, but they refer to _totally different places_. In such situations, it is easy for even a _human_ to get confused, let alone an _AI tool_. This kind of **inconsistency** made it extremely difficult for any **LLM** to understand what was going on without **very specific training**.

To help the **LLMs**, we first designed a **schema**—a structure that showed what kind of information we wanted. This included things like **date**, **place**, **arrival time**, and **departure time**. We then gave the entire **PDF of the ship logs** to different **AI tools** to see how they would handle it. Unfortunately, most of them couldn’t deal with the PDF very well. Some of the tools couldn’t even perform **OCR (optical character recognition)**, possibly because the file was made from screenshots turned into a PDF. This layering sometimes confuses AI tools, making the text unreadable or hard to extract. Some tools tried to help by giving us code to run locally on our own computers, instead of doing the processing themselves. That’s helpful to an extent, but it kind of defeated the purpose of trying to automate the process through the tool directly. Among all the tools, **Perplexity** performed slightly better than the others—it was able to extract some information—but the way it structured the output was inconsistent and messy. There was no clear way to track the ship’s journey across time and space based on what it gave back. For the most part, we had to do a lot of manual cleanup and cross-checking ourselves.

From our experience using **large language models (LLMs)**—and based on discussions in class—we found that the most effective way to work with these tools was by breaking the task into _small batches_ and having a back-and-forth “conversation” with the model. Instead of pasting all the instructions at once and expecting a perfect result, we made better progress by guiding the **LLM** step by step. This approach worked especially well when we kept explaining the **context** and refining the instructions gradually. That said, there were still a few pages that the **LLMs** just couldn’t handle, so we had to manage those manually.

We started by manually entering data from the first two pages ourselves. This helped us understand the **structure** and figure out what kind of problems we’d face. Initially, we created columns like **date**, **day**, **action**, **location**, **time**, and **additional info**. **Latitude and longitude columns** were added later for **mapping**. Once we had a small sample ready, we showed it to **ChatGPT** along with the original **screenshot** and our formatted table and explained, _“This is the input I had, and this is the kind of result I want.”_ That gave the model a clear sense of what we were trying to achieve, and the output it returned was surprisingly good—very close to what we expected, with only minor issues. For example, on page 2 there’s a record that says something like “Zanzibar arrives _p.m._”—which means the ship arrived in the evening, but no exact time is mentioned. **ChatGPT** would sometimes include _“p.m.”_ in the time column, but that would cause problems when analyzing the data, especially if it were to be analyzed using code (since _“p.m.”_ isn’t a valid time value). So we instructed it to only include rows where an actual numeric time was given, and to always use **24-hour format**.

We also emphasized a key rule: every **arrival must have a matching departure**. If a ship arrives somewhere, it must have left another place beforehand. **ChatGPT** gradually began to understand that logic, and we kept reinforcing it with more examples. We even re-explained confusing entries like _“Weti & Chake-Chake”_ or other variations we had already standardized. This process of slow, iterative prompting really helped improve the quality of its responses. Sometimes it would still go completely off track, but overall, it kept learning and getting better.

There were definitely a few issues here and there, but after a while it became more about reviewing the results the **LLM** gave us—and that part usually went pretty smoothly. Occasionally, we’d ask it clarifying questions like, _“Are all of these ports in the same place?”_ and it would respond accurately, but for the most part, we just kept progressing through the data.

On a side note, **Page 12** was extremely difficult to understand. Our best guess was that the entries from _8th to 15th September_ were all repeating the events of _8th September_ each day. So we asked the **LLM** to give us the records for _8th September_, then we manually copied and pasted them, changing only the **date** each time until we reached _15th September_. It was very confusing for both the model and us to interpret that section otherwise.

When we got to _October and November 1919_, the format changed drastically, and we were unsure if the **LLM** would still be able to handle the task. Surprisingly, it did a great job—**page 13** especially came out very clean. By this point, the model had learned quite a bit from all the feedback, so it was producing better and more consistent results. There were a few small things—like if something was written as “Weti _4pm_,” it might put the whole thing in the _“Additional Info”_ column instead of splitting “Weti” and “4pm” into the right columns—but these were easy to fix.

Eventually, we compiled a **dataset** with over 400 rows. Since every **arrival** and **departure** record was split into separate rows, the actual number of events is closer to _200 entries_. There were a few cases of missing data as well—these are marked as **greyed-out** cells in the spreadsheet. For example, in **row 249**, the data jumps from _30th November_ to _13th October_ without any explanation of what happened in between.

So that wrapped up the **data extraction** and **enrichment** phase. A lot of these steps happened hand-in-hand as we worked through the dataset. One of the main **enrichment tasks** was adding **latitude** and **longitude** coordinates for each location. To do this, we created a **reference table** starting from column M in the spreadsheet linked previously. This table listed each key location (like Zanzibar, Tanga, etc.) along with their corresponding latitude and longitude. Then, using **Excel formulas**, we automatically matched the coordinates from the **reference table** to the locations listed in the dataset. For example, every time “Zanzibar” appeared in the location column, the correct **latitude** and **longitude** would be pulled in from column M.

## Geocoding

We used **kepler.gl** for the mapping phase of our project, since we were working entirely with **geographical data**.

We began by creating a **heatmap** to visualize how frequently different locations were visited. For this, we used the "**count**" column from our spreadsheet. Technically, each visit is recorded twice—once for **arrival** and once for **departure**—so the count is effectively doubled. However, since we were primarily interested in a **relative comparison** between locations (rather than exact totals), this scaling was acceptable. We exported the spreadsheet as a **CSV** file, uploaded it to **kepler.gl**, and it automatically generated a map showing both the point markers and the **heatmap** based on visit frequency. We had already added the **latitude** and **longitude** data for each place by looking it up online and incorporating it into the spreadsheet.

<!-- checkpoint -->

![Kepler image without routes](/daah2025/assets/images/kepler_heatmap.jpeg)
_Figure 1: Initial Kepler map with heatmaps that indicate how often each location was visited_

The second goal was to map out the **routes** that the ship followed between locations. While working on the spreadsheet, we noticed something interesting: there were only a few **unique routes**, and these were repeated multiple times throughout the logs. We documented these routes in a separate "**routes**" spreadsheet (<!-- checkpoint -->[Routes Spreadsheet](https://docs.google.com/spreadsheets/d/1esyZCmksQYqd28VjMpknDYfM54VCbPgON8gKADXUJ2E/edit?usp=sharing)), and then asked **ChatGPT** to help us generate a **GeoJSON** file representing all of these paths. We provided it with both the **route data** and the corresponding **latitude/longitude** information, and it generated a **Python script** that could produce the **GeoJSON**. Once we had the **GeoJSON** file (<!-- checkpoint -->[Routes GeoJSON file](daah2025/assets/files/HHS_Cupid_kepler.json)), we layered it on top of our existing **Kepler.gl** map. The result was a network of hexagon-like connections showing all the circuits the ship regularly traveled. It worked perfectly, giving us a clear visual of the ship's **movement patterns**. We also used Overpass Turbo to add a layer of some historical monuments in the Zanzibar region to the Kepler map, giving our project additional historical context and helping connect the ship’s movements to landmarks that still exist today.

<!-- checkpoint -->

![Final Kepler image](/daah2025/assets/images/kepler_all2.jpeg)

<!-- checkpoint -->

![Final Kepler image](/daah2025/assets/images/kepler_all.jpeg)
_Figures 2 and 3: Final Kepler maps (from different angles) showing ship routes and visit frequency heat maps_

<!-- checkpoint -->

Use this file to view the map directly on Kepler: [Kepler file](/daah2025/assets/files/HHS_Cupid_kepler.json)

## Conclusion

In the end, this project helped us see how history can come to life when you work with data in a **hands-on** way. What started as a list of **ship movements** became a much deeper exploration of **place**, **time**, and **meaning**. By organizing, cleaning, and mapping the data, we were able to tell a **visual story** of HHS Cupid’s journey and understand the patterns behind it. We faced challenges, made small discoveries, and learned how tools like **ChatGPT** and **kepler.gl** can support historical research. Most importantly, we saw how working with **space**—not just time—can help us connect with the past in a more engaging and meaningful way.

## References:

- _Zanzibar Gazette_. (1919–1920). Government Printer, Zanzibar.

- Klein, L., Martin, M., Brock, A., Antoniak, M., Walsh, M., Johnson, J. M., Tilton, L., & Mimno, D. (2024). _Provocations from the Humanities for Generative AI Research_. Retrieved from [arXiv](https://arxiv.org/abs/2502.19190).
