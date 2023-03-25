+++
title = "Creativity. Reflection 8 - CSV SQLite Converter"
date = "2023-03-21T07:11:18+01:00"
author = "Mateusz Konat"
authorTwitter = "" #do not include @
cover = "images/cover.png"
tags = ["creativity", "coding"]
keywords = ["creativity", "coding"]
description = "People usually associate programming with Big Tech and even bigger money. They don't realize how much great code is open to public to use and contribute to."
showFullContent = false
readingTime = false
hideComments = false
color = "" #color from the theme settings
draft = "true"
+++

# Open source culture
Here is a magnificent video that explains the philosophy of open source culture:

***
{{< youtube id="gobBQwtFeyk" >}}
***

It highlights two most important aspects of open source code:
1. It is available for everyone
2. Together, we can make it better

This idea of creating useful code appeals many programmers and push them toward open-sourcing their own code to be improved with the help of others.

In this blog post, I show you my adventure with open source.

# Necessity is the mother of invention
Hoping to land my first job in the oncoming summer, I have started modifying my [GitHub portfolio](https://github.com/undeMalum) to make it look more professional and appealing for possible employers. Apart from subtle changes such as referencing socials, I have extended my portfolio with [new projects](https://github.com/undeMalum/Taxonomy-game) and so-called [README files](https://github.com/undeMalum/Taxonomy-game/blob/main/README.md). (_On the whole, README file is a brief description of a project with installation and usage details_). However, while uploading [Taxonomy game](https://github.com/undeMalum/Taxonomy-game), I have encountered an annoying issue - [GitHub file size limit](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-large-files-on-github#file-size-limits).

This is because the size of game's database is three times above the threshold of GitHub file size rejection. I needed to figure out a way to overcome this problem since the game is useless without its database. Fortunately, I could take advantage of [CSV files](https://en.wikipedia.org/wiki/Comma-separated_values).

# Technical overview
**NOTE:** although the section below is not very detailed documentation, it does contain a fair degree of geeky knowledge. If you want to avoid uncharted waters (and be a chicken), please proceed to [CSV-SQLite Converter](#csv-sqlite-converter) section. I have warned you - your move chief.

## CSV files
CSV (**comma-separated values**) files allow store data in a structured manner. The first raw indicates columns' names. The rest is actual data. It looks more or less like this:

         #employees.csv
    1    ID, name, surname, email
    2    123, John, Smith, john.smith@gmail.com
    3    111, Samanta, Black, samanta.black@gmail.com
    4    222, Jerry, Smith, jerry.smith@gmail.com

As you can see from the imaginary `employees.csv` file, the first raw stores the names of columns (in this case: ID, name, surname, and email). Remaining raws (2-4) contain data about each employee. For instance, raw 3 holds data for an employee named Samanta Black whose ID is 111 and email is samanta.black@gmail.com.

## Advantage of CSV files
You may wonder why would I choose CSV format over a database? The answer is simple: CSV files are smaller.

Without going into much detail, databases ([SQLite](https://en.wikipedia.org/wiki/SQLite) in our case) are very efficient in storing, updating and manipulating large amounts od data. Therefore, they are a great choice for applications that require manipulating lots of data - for example our Taxonomy game. Sadly, because SQL-like databases are [binary files](https://en.wikipedia.org/wiki/Binary_file), they occupy much disk space and are hard to compress - nitty-gritty of this blog post. 

On the other hand, CSV files do not scale for larger datasets where frequent manipulation of data is required due to the slow retrival. However, at the cost of bad performance comes small file size and a variety of compression opportunities.

Relative compactness of CSV format makes it ideal for our purpose.

## CSV and other formats
The last "hi-tech" thing I'd like to discuss is why I chose CSV over other format such as [JSON](https://en.wikipedia.org/wiki/JSON). 

Seemingly, both CSV and JSON have their own python libraries ([`csv`](https://docs.python.org/3/library/csv.html) and [`json`](https://docs.python.org/3/library/json.html) respectively) within [Python Standard Library](https://docs.python.org/3/library/) and share small size advantage (see [Advantage of CSV files](#advantage-of-csv-files)). So what differs them?

First of all, CSV is arguably more human readable than JSON. I mean, take a look at the data example from [CSV files](#csv-files) rewritten in JSON:

    # employees.json
    {
        "emplyees":
        [
            {
                "ID": 123,
                "name": "John",
                "surname": "Smith",
                "email": "john.smith@gmail.com"
            },
            {
                "ID": 111,
                "name": "Samanta",
                "surname": "Black",
                "email": "samanta.black@gmail.com"
            },
            {
                "ID": 222,
                "name": "Jerry",
                "surname": "Smith",
                "email": "jerry.smith@gmail.com"
            },
        ]
    }

I just pray to the Lord I didn't make any mistakes while typing. Admittedly, for a skilled programmer, JSON may be actually more readable and easier to operate on. However, I believe that an average human being would rather go for CSV in terms of readability. Moreover, in JSON, there's a lot of repetition as each field needs to be written as many times as we have employees.

Besides, the structure of a CSV file resembles the structure of SQLite database's table (single unit of the database). Take a look at the comparison of SQLite table schema with a single CSV files below:

Aforementioned employees.csv:

    1    ID, name, surname, email
    2    123, John, Smith, john.smith@gmail.com
    3    111, Samanta, Black, samanta.black@gmail.com
    4    222, Jerry, Smith, jerry.smith@gmail.com

SQLite table schema:

| ID | name | surname | email |
| --- | --- | --- | --- |
| 123 | John | Smith | john.smith@gmail.com |
| 111 | Samanta | Black | samanta.black@gmail.com |
| 222 | Jerry | Smith | jerry.smith@gmail.com |

Very similar, isn't it?

Last, but not least, CSV is prevalent in Data Science world. I don't know the specific reason, but most datasets (whatever it is - either sentences, protein sequences etc.) are available as CSV files. Since I've been tinkering with [language processing models](https://en.wikipedia.org/wiki/Natural_language_processing) recently, I find it natural to use CSV.

# What is a package?
I hope you didn't get bored and stayed afloat. We're getting to the point, but I need to introduce one more thing - packages.

I know you may be confused by how on the Earth I switched from talking about CSV to being a postman. Let me explain.

CSV files seem to be a great choice for _storing_ data. However, this data needs to be somehow _transferred_ from the SQLite database to CSV files. There _has to_ be an additional step, a script, that would make this transfer possible. This is where the concept of package comes into play.

A package looks like a normal script and the only difference is its intendent purpose. Usually, an app is meant to be run and "do something". For example, the Taxonomy game allows students to learn biological classification through the means of gameplay and Word makes editing text files easier thanks to its useful Graphical User Interface (shortly, GUI). A package serves a bit different purpose. 

It also "does something", but is not an application in the same sense as the Taxonomy game and Word are. A package provides a set of tools that simplifies the process of developing software. To give an illustration, [`PyQt5`](https://pypi.org/project/PyQt5/) comes with a whole bunch of GUI elements such as buttons, entries, display lists etc. Let's say, I'd like to create an accounting system with GUI. With `PyQt5`, I can assemble some GUI elements to make my app more user-friendly and I don't need to worry of _how this elements are actually implemented_. All I need to know is _how to use them_ by learning `PyQt4`'s API.

(_As a sidenote, API stands for Application Software Interface. Generally, API defines the communication mechanism between two software components. In the example above, between the accounting system and PyQt5._)

This is exactly what happens with the CSV-SQLite Converter - it is not an application in common sense (although it can be used as such), but a very handy tool kit for programmers who don't have to think about the inner workings of the converter - they just need to know its API.

# CSV-SQLite Converter
Finally, after more than 1000 words and 7000 characters, I can jump straight to the software of interest - [CSV-SQLite Converter](https://github.com/undeMalum/csv-sqlite-converter).

## DRY
Following the [DRY principle](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) (Don't Repeat Yourself), I'm not going to dive deep into how the converter works. I have already done it once. Instead, I'd like to direct you to my GitHub to study the converter's [README file](https://github.com/undeMalum/csv-sqlite-converter/blob/main/README.md). This should provide you with good understanding of my package.

Having said that, let's move on to discussing the [Current state](#current-state) of the project and [Future developments](#future-developments).

## Current state
As of today, I have not uploaded the CSV-SQLite converter to [PyPi](https://pypi.org) yet. (_"The Python Package Index (PyPI) is a repository of software for the Python programming language."_) What follows is that the converter cannot be installed with `pip` (package installer for Python.) I have published my package to [TestPyPi](https://test.pypi.org), which is a separate service from PyPi and, as the name suggests, is for testing - if the package uploads correctly, if installation goes as expected and so on.

Fortunately, the CSV-SQLite converter has passed all tests successfully - both [internal](https://github.com/undeMalum/csv-sqlite-converter/tree/main/tests) and external (TestPyPi) ones. Therefore, I am almost ready to publish my first package to PyPi. What has left are minor improvements to the documentation and meaningful versioning of the project.

Apart from that, the converter can be shared with the world! Furthermore, thanks to the converter, the Taxonomy game makes sense at last...

## Future developments
Inasmuch as I like DRY principle, I'm not a huge fan of "_if ain't broken, don't fix it_" rule. Of course, if you have a deadline, it's meaningless to make your work better and better all the time because there will always be something to work on. At some point you need to decide: "okay, this is a final version". This holds true for IBDP with its Internal Assessment and Extended Essay. However, such an attitude would be considered lazy with regard to open source.

In open source, the only final date is the day of a new version's release. You can add new features, fix bugs or whatever as long as you wish on condition that you ship those changes before the release. If you didn't manage to made all changes you desired on time, you're lucky as there'll be new releases in the future which can include your precious contributions.

I have embraced this approach in the context of the CSV-SQLite converter. I've made it work and cleaned up the code. It looks nice, but I'm aware there's still an awful lot of work to be done. I know that my current design choices are not the best in general. They are the best to my knowledge though. Thus, what I can do is to expand this knowledge.

I have a broad picture as to where I can look for improvements. I'm talking about two extremely popular python packages: [pandas](https://pypi.org/project/pandas/) and [polars](https://pypi.org/project/polars/). They were both designed for efficient data processing, which may not sound promising as far as the converter is concerned. After all, it just transfers data from one format to another - it does NOT process this data. However, these packages provide tools for the shift of format.

Maybe I could take advantage of that? I don't know, but I'll find out A.S.A.P.

# Final remark
It was a long read and tough journey thorugh the cybernetic jungle. At the end, I just want to go over what we've covered in this article and possibly share my thoughts on the process.

I started by talking about open source culture and its main characteristics. Then, I justified the need for the converter and introduced CSV and other formats. After that, I familiarized you with the notion of package. Lastly, I talked about the CSV-SQLite converter itself as well as everything that is connected with it.

I'm happy that I've started this project, I really am. Not only was it an intermediate step for the Taxonomy game, but also a great relief from coding my Computer Science Internal Assessment. I was a bit overwhelmed and needed a break. Being intelectually well-rested rest, I look forward to return to my IA and finish it. Moreover, I also learned a lot about storing data in file format, [virtual environments](https://docs.python.org/3/library/venv.html), Python packaging system and many, many others thanks to the converter. After years of learning programming I finally begin to see the meaning in what I'm creating. I've been waiting for so long to feel like that.

Thanks for your attention, it's been lovely talking to you!

# Learning outcomes:
- LO 1 Strength and Growth
- LO 2 Challenge and Skills
- LO 3 Initiative and Planning
- LO 4 Commitment and Perseverance
- LO 5 Collaborative Skills
- LO 6 Global Engagement
- LO 7 Ethics of Choices and Actions
