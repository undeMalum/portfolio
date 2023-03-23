+++
title = "Creativity. Reflection 8 - CSV SQLite Converter"
date = "2023-03-21T07:11:18+01:00"
author = "Mateusz Konat"
authorTwitter = "" #do not include @
cover = ""
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

It highlites two most important aspects of open source code:
1. It is available for everyone
2. Together, we can make it better

This idea of creating useful code appeals many programmers and push them toward open-sourcing their own code to be improved with the help of others.

In this blog post, I show you my adventure with open source.

# Necessity is the mother of invention
Hoping to land my first job in the oncoming summer, I have started modificating my [GitHub portfolio](https://github.com/undeMalum) to make it look more professional and appealing for possible employers. Apart from subtle changes such as referencing socials, I have extended my portfolio with [new projects](https://github.com/undeMalum/Taxonomy-game) and so-called [README files](https://github.com/undeMalum/Taxonomy-game/blob/main/README.md). (_On the whole, README file is a brief description of a project with installation and usage details_). However, while uploading [Taxonomy game](https://github.com/undeMalum/Taxonomy-game), I have encountered an iritating issue - [GitHub file size limit](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-large-files-on-github#file-size-limits).

This is because the size of game's database is three times above the threshold of GitHub file size rejection. I needed to figure out a way to overcome this problem since the game is useless without its database. Fortunately, I could take advantage of [CSV files](https://en.wikipedia.org/wiki/Comma-separated_values).

# Technical overview
**NOTE:** although the section below is not very detailed documentation, it does contain a fair degree of geeky knowledge. If you want to avoid uncharted waters (and be a chicken), please proceed you [The Package](#the-package) section. I have warned you - your move chief.

## CSV files
CSV (**comma-separated values**) files allow store data in a structured manner. The first raw indicates columns' names. The rest is actual data. It looks more or less like this:

         #employees.csv
    1    ID, name, surname, email
    2    123, John, Smith, john.smith@gmail.com
    3    111, Samanta, Black, samanta.black@gmail.com
    4    222, Jerry, Smith, jerry.smith@gmail.com

As you can see from the imaginary `employees.csv` file, the first raw stores the names of columns (in this case: ID, name, surname, and email). The remaining raws (2-4) contain data about each employee. For instance, raw 3 holds data for an employee named Samanta Black whose ID is 111 and email is samanta.black@gmail.com.

## Advantage of CSV files
You may wonder why would I choose CSV format over a database? The answer is simple: CSV files are smaller.

Without going into much detail, databases ([SQLite](https://en.wikipedia.org/wiki/SQLite) in our case) are very efficient in storing, updataing and manipulating large amounts od data. Therefore, they are a great choice for applications that require manipulating lots of data - for example our Taxonomy game. Sadly, because SQL-like databases are [binary files](https://en.wikipedia.org/wiki/Binary_file), they occupy much disk space and are hard to compress - nitty-gritty of this blog post. 

On the other hand, CSV files do not scale for larger datasets where frequent manipulation of data is required due to the slow retireval. However, at the cost of bad performance comes small file size and a variety of compression opportunitiesm.

Relative compactness of CSV format makes it ideal for our purpose.

## CSV and other formats
The last "hi-tech" thing I'd like to discuss is why I chose CSV over other format such as [JSON](https://en.wikipedia.org/wiki/JSON). 

Seemingly, both CSV and JSON have their desiganted python libraries ([csv](https://docs.python.org/3/library/csv.html) and [json](https://docs.python.org/3/library/json.html) respectively) within [Python Standard Library](https://docs.python.org/3/library/) and share small size advantage (see [Advantage of CSV files](#advantage-of-csv-files)). So what differes them?

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

I just pray to the Lord I didn't make any mistakes while typing. Admittedly, for a skilled programmer, JSON may be actually more readable and easier to operate on. However, I believe that an average human being would rather go for CSV in terms of readibility. Moreover, in JSON, there's a lot of repetition as each field needs to be written as many times as we have employees.

Besides, the structure of a CSV file resembles the structure of SQLite database's table (single unit of the database). Take a look at the comparison of SQLite table shema with a single CSV files below:

Aforementiond employees.csv:

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

Last, but not least, CSV is prevelant in Data Science world. I don't know the specific reason, but most datasets (whatever it is - either sentences, protein sequences etc.) are available as CSV files. Since I've been tinkering with [language processing models](https://en.wikipedia.org/wiki/Natural_language_processing) recently, I find it natural to use CSV.

# The package
I hope you didn't get bored and stayed afloat. We're about to dive deep into the [CSV-SQLite Converter](https://github.com/undeMalum/csv-sqlite-converter) package.

## Why package?
You may be confused what a package is and, more importantly, how on the Earth I switched from talking about CSV to being a postman. Let me explain.

CSV files seem to be a great choice for _storing_ data. However, this data needs to be somehow _transferred_ from the SQLite database to CSV files. There _has to_ be an additional step, a script, that would make this transfer possible. 
