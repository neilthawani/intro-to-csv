---
title: 'Parsing CSV in Ruby'
length: 60-90 mins
tags: ruby, csv
---

### Goals

By the end of this lesson, you will:

* be able to open and read a CSV (comma-separated values) file
* learn different data parsing methods and procedures
* learn about options involved in parsing data

### Introduction

* Why is this lesson important? How do we make this interesting?

Please see [this video](https://www.youtube.com/watch?v=Ma7MT2rGb_o).

### Opening

* What is a CSV? Why do we care?
* We will learn how to open, read, parse, manipulate and write CSV data.

The purpose of learning to parse CSV files is for data consolidation, manipulation, and display in an easy-to-view and easy-to-parse format.

A CSV, or a comma-separated value file, allows data to be saved in a simple, structured format. CSVs can be opened in word processors, IDEs, or spreadsheet programs like Microsoft Excel.

By the end of this lesson, you will be able to read from and write to CSV files and brush upon data encoding and data type conversion.

### Introduction to New Material (I do)

* In the Real World, you will have to use strong GoogleFu to find answers to questions.
* Let's do just that.
* I'll demonstrate my thought process for finding out how to solve this problem.

Please see [this video](https://www.youtube.com/watch?v=nOC5oVfU3hY).

### Guided Practice (We do)

* Here, I'll go over the Ruby CSV library and a basic implementation of its read/write operations.
* Let's open the pry REPL in Terminal and parse ```guided.csv```, located in this repo.

Please see [the accompanying video](https://www.youtube.com/watch?v=qb8WBNI7Bp0).

```
guided.csv:
Name,Designation,DOB,Last Active
Jean-Luc Picard,Captain,7/13/2305,2379
William Riker,First Officer,8/19/2335,2379
Deanna Troi,Counselor,3/29/2336,2379
Wesley Crusher,Lieutenant Junior Grade,7/29/2349,2379
Beverly Crusher,Doctor,10/13/2324,2379
Worf,Ambassador,12/9/2340,2379
Geordi La Forge,Chief Engineer,2/16/2335,2379
Data,Lieutenant Commander,2/2/2338,2379
```

**Q:** Take a look at the data. What data types do we have?

**A:** Strings, dates, and numerics.

#### Reading from a File

```ruby
require 'csv'
data = CSV.read('guided.csv')
=> [["Name", "Designation", "DOB", "Last Active"],
 ["Jean-Luc Picard", "Captain", "7/13/2305", "2379"],
 ["William Riker", "First Officer", "8/19/2335", "2379"],
 ["Deanna Troi", "Counselor", "3/29/2336", "2379"],
 ["Wesley Crusher", "Lieutenant Junior Grade", "7/29/2349", "2379"],
 ["Beverly Crusher", "Doctor", "10/13/2324", "2379"],
 ["Worf", "Ambassador", "12/9/2340", "2379"],
 ["Geordi La Forge", "Chief Engineer", "2/16/2335", "2379"],
 ["Data", "Lieutenant Commander", "2/2/2338", "2379"]]
```

We observe the following:

* this returns an array of arrays (which is good and makes sense!)
* the first element of the 2D array is an array of the file's headers
* each value is read in as a string, despite the fact that some values are date types and some are numerics

##### What is a header?

From [Wikipedia](https://en.wikipedia.org/wiki/Comma-separated_values#Standardization):
> Some implementations allow or require single or double quotation marks around some or all fields; and some reserve the very first record as a header containing a list of field names.

```ruby
data = CSV.read('guided.csv', headers: true).each { |row| puts row }
Jean-Luc Picard,Captain,7/13/2305,2379
William Riker,First Officer,8/19/2335,2379
Deanna Troi,Counselor,3/29/2336,2379
Wesley Crusher,Lieutenant Junior Grade,7/29/2349,2379
Beverly Crusher,Doctor,10/13/2324,2379
Worf,Ambassador,12/9/2340,2379
Geordi La Forge,Chief Engineer,2/16/2335,2379
Data,Lieutenant Commander,2/2/2338,2379
=> #<CSV::Table mode:col_or_row row_count:9>
```

We see that the data is stored as a ```CSV::Table``` object. We can pass in another parameter, then, called "header_converters," and set the types of the headers to :symbol.

```ruby
data = CSV.read('guided.csv', headers: true, header_converters: :symbol)
=> #<CSV::Table mode:col_or_row row_count:9>
```

#### Writing to a File

Please see [the accompanying video](https://www.youtube.com/watch?v=Ifr5Z5fl5kU).

Let's just output the CSV we read in before to another file called guided2.csv.

```ruby
require 'csv'
data = CSV.read('guided.csv', headers: true, header_converters: :symbol)
data = data.to_a
CSV.open('guided2.csv', 'wb') do |csv|
    data.each do |row|
        csv << row
    end
end
```

Reading this using `cat guided2.csv` in the Terminal yields what we needed:
```
name,designation,dob,last_active
Jean-Luc Picard,Captain,7/13/2305,2379
William Riker,First Officer,8/19/2335,2379
Deanna Troi,Counselor,3/29/2336,2379
Wesley Crusher,Lieutenant Junior Grade,7/29/2349,2379
Beverly Crusher,Doctor,10/13/2324,2379
Worf,Ambassador,12/9/2340,2379
Geordi La Forge,Chief Engineer,2/16/2335,2379
Data,Lieutenant Commander,2/2/2338,2379
```

### Independent/Pair Practice (You do)

* Parse `pair.csv` (located in this repository) line-by-line using the following options:
    - Ensure that headers are recognized by the parser.
    - Store headers as symbols instead of strings.
    - Use 'ISO-8859-1' encoding.
* Store the data as an array of arrays, making the following changes:
    - Convert all numeric strings to numeric-type variables.
    - Convert negative percentage values to negative decimal values.
* Write the new data to an output file called `pair_parsed.csv`.

#### Instructor's Proposed Solution:
```ruby
data = CSV.foreach('pair.csv', headers: true, header_converters: :symbol, encoding: 'ISO-8859-1', converters: :all).to_a
new_data = Array.new
data.each do |row|
    row[:change] = (row[:change].sub("%", "").to_f / 100).round(4)
    new_data << row
end

CSV.open('pair_parsed.csv', 'wb') do |csv|
    new_data.each do |row|
        csv << row
    end
end
```

### The Closing

Please see the preliminary [accompanying video](https://www.youtube.com/watch?v=icBPUJO7vlA).

* Did you finish the individual/pair programming exercise? Any questions?
* What unexpected challenges did you encounter?
* Today we learned how to read, parse, manipulate, and write CSV file data while debugging issues that may come up in the process.

For a walkthrough of the code, please [view this video](https://www.youtube.com/watch?v=A8YUCJr8UdE).

### Possible questions and/or misunderstandings

Please see [this video](https://www.youtube.com/watch?v=D0nmXlTqM6E).

* What if we don't like the Ruby docs?
    - There are other resources via Google and Stackoverflow. Appended to the end of this lesson are tutorials and some resources.

* [Why do we convert strings to symbols for our CSV headers?](http://stackoverflow.com/questions/8189416/why-use-symbols-as-hash-keys-in-ruby)

* [Why are we unable to read the pair.csv file initially](http://stackoverflow.com/questions/5053216/when-we-import-csv-data-how-eliminate-invalid-byte-sequence-in-utf-8)

* [Why do we use `encoding: 'ISO-8859-1'` when parsing our file?](https://en.wikipedia.org/wiki/ISO/IEC_8859-1)

* [Why do floats in Ruby produce numbers appended to the quotient?](http://www.rails-troubles.com/2011/12/ruby-float-quirks.html)

An alternative solution would have been:
```ruby
BigDecimal.new(data[0][:change].sub("%", "").to_f / 100, 4).to_f
```

### Outside Resources / Further Reading

* [CSV parsing tutorial with custom variable type converters](http://technicalpickles.com/posts/parsing-csv-with-ruby/)
* [CSV parsing tutorial - extremely detailed](https://www.sitepoint.com/guide-ruby-csv-library-part/)
* [File I/O modes in Ruby](http://stackoverflow.com/questions/3682359/what-are-the-ruby-file-open-modes-and-options)
* [Source of paired exercise data](https://en.wikipedia.org/wiki/List_of_Metropolitan_Statistical_Areas)