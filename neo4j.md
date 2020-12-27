---
Created: 'Dec 23, 2020 5:58 PM'
author: Kailash Vetal
slug: 56258131-6646-4e38-85b1-00b34e532b6e
WebClipIndex: 25
URL: 'http://blog.bruggen.com/2014/07/using-loadcsv-to-import-data-from.html'
secret: true
ArticleId: 56258131-6646-4e38-85b1-00b34e532b6e
title: Bruggen Blog- Using LoadCSV to Import data from Google Spreadsheet
Updated: 'Dec 23, 2020 7:32 PM'
ArticleIndex: 25
---

# Neo4j

My colleague [Rickard](https://twitter.com/rickardoberg) recently did a great [graphgist](http://gist.neo4j.org/?db50901283291f8ea22c) on [Elite:Dangerous](https://www.kickstarter.com/projects/1461411552/elite-dangerous) trading. When I read his post, the first thing that struck me was that he had found a great clever way to import data into Neo4j. And since I have been into data imports for a while, I decided to take it for a spin, and write it up for you.

The mechanism uses two fundamental capabilities:

* Google Spreadsheets have a great capability to export the data into a CSV format. That export capability generates a unique URI that you can download the CSV of a specific sheet in the spreadsheet from.
* [Neo4j's](http://www.neo4j.com/) [Load CSV capability](http://docs.neo4j.org/chunked/milestone/cypherdoc-importing-csv-files-with-cypher.html?_ga=1.218299163.349775399.1339575792) can leverage data that is located at **any** URI. The data does not have to be local - it can be anywhere on the network.

So let's give this a try.

## Preparing the Google Spreadsheet

What we want to do is to put some sample data into a spreadsheet first.

You can find that actual sheet [over here](https://docs.google.com/spreadsheets/d/1RSzuulrnDkg49q05DKQVFLGkCu_UH5vmQVkA5PHdPZA/edit?usp=sharing). Clearly it's not a very big sheet - we are just trying to do a little test.

Now, in order to export this file to CSV, and to make that export accessible over the internet, we need two things:

1. the spreadsheet needs to be publicly accessible over the internet. Otherwise Google will ask you to authenticate first, and the Neo4j Load CSV process will not know how to do that.
2. you need to generate the download URI of the CSV export. That is very simple too. First you do the export: And then you take a look at your browser download history to figure out what the URI of the download was. Easy: You can copy that URI from there \(in this case it is [this ugly thing](https://docs.google.com/a/neotechnology.com/spreadsheets/d/1RSzuulrnDkg49q05DKQVFLGkCu_UH5vmQVkA5PHdPZA/export?format=csv&id=1RSzuulrnDkg49q05DKQVFLGkCu_UH5vmQVkA5PHdPZA&gid=0)!\)- and then we move on to the next stage: importing into Neo4j.

## Importing the data into Neo4j

That Import process is very simple now, with Load CSV. Here's the query that I wrote, which uses the URI of the CSV version of the spreadsheet \(in green\):

It's very simple. First I add the colours using a MERGE, then I connect the persons to their respective colours using a create. Running that import gives me results in a matter of milliseconds, and with no intermediate steps:

And although this graph is very haphazard and not very interesting, I can query it straight away.

That's all there is to it really. Importing data was already way easier with LoadCSV, but thanks to Rickard, we can now do it straight from a Google Spreadsheet. Thanks Rickard!

