# Working With Strings in Ruby
## Text Processing Introduction
**whois** - network diagnostic tool\
**Directory Harvest Attack** - A technique spammers use to discover email addresses by sending requests to email servers with many random email addresses in the hopes that some will be legitimate.
* Mitigate this type of attack by configuring your SMTP server to stop answering with invalid recipient responses when an e-mail fails to deliver.  This response is the clue that the attacker uses to determine if the address they're trying out is a real one or not.
## String Indexing
*String indexing* allows you to extract or even change certain portions of the text within a string.  
`str[start,length]`  
```ruby
str = "string beans are tasty!"
puts str[0,6] #Indexing
=> String 
str[0,6] = "Green" #Replace
puts str
=> Green beans are tasty! 
str[0,6] = "" #Delete
puts str
=> beans are tasty!
str[16,0] = " very" #Insert
puts str
=> beans are very tasty!
str = "String beans are tasty!"
puts str[-1] #Negative Number - Count from last character in string
=>!
```
**Range** - specify two indexes (beginning..end) - end is inclusive
```ruby
str = "String beans are tasty!"
puts str[7..10]
=> bean
puts str[-6..-1]
=> tasty!
```
## String Substitution
**Index** method
```ruby
str = "Supercalifragilisticexpialidocious even thought he sound of it is something quite atrocious"
puts str.index("even")
=> 35
puts str[35,4]
=> even
```
**Sub** method
```ruby
str = "Supercalifragilisticexpialidocious even thought he sound of it is something quite atrocious"
new_str = str.sub("atrocious", "delightful")
puts new_str
=> Supercalifragilisticexpialidocious even thought he sound of it is something quite delightful
```
Returns a *new* string  
`sub!` - In place substitution  
Only replaces the *first* instance of matching string  
String substitution on *all* of the substrings within a given piece of text, use the **gsub** method.  
```ruby
str = "It was the best of times, it was the worst of times."
new_str = str.gsub("times", "bananas")
puts new_str
=> It was the best of bananas, it was the worst of bananas.
```
**Include?** method
returns TRUE if the text contains the substring and FALSE if it does not  
```ruby
str = "Does this text contain a special string?"
puts str.include?("special")
=> true
puts str.include?("ordinary")
=> false

message = "[log message] ERROR: 418 - I'm a Teapot"
if message.include?("ERROR")
    raise "An unexpected error occurred: #{message}"
end
```
**Raise** method is one way a programmer can cause their programs to crash and exit intentionally.
## More String Methods
 Transformation examples
 * .capitalize
 * .downcase
 * .reverse  

 Other Methods
 * .length
 * .count
 * .chomp - removes specific characters at the end of a string (\n, \r, \r\n)  
 also accepts substrings `chomped_str = str.chom("world!)`
 * .strip - returns a copy of the string it's called on, stripped of whitespace, from its beginning and end.  
 Will also remove special characters like \r \n, horizontal and vertical tabs, and null characters.
 * .chop - removes last character in a string without exception
 * .dump

**\r** - indicates a carriage return, which means that the cursor should be moved to the beginning of the line.  
**\n** - A newline separator indicating that the cursor should be moved to a new line.
## Summary
[String Documentation](https://ruby-doc.org/core-2.4.1/String.html)  
# Regular Expressions
## What are regular expressions?
**Regular expressions** allow you to answer questions like, "What are all of the four-letter words in a file?" or "Give me all of the email addresses in a request log."
## Why use regular expressions?
```ruby
str = "July 31 07:51:48 mycomputer bad_process[12345]: ERROR Performing package upgrade"
regex = /\[(\d+)\]/
results = regex.match(str)  
=> #<MatchData "[12345]" 1:"12345">
puts results.captures
=> 12345
``` 
## Basic Syntax
`/Ruby/` Slash symbols are delineators (will match "Ruby")  
`puts /Ruby/ =~ "The word Ruby is contained in this text."`  
**=~** - Find out if regex pattern matches a string. If there is a match, the starting position of the substring match will be returned. If there is no match, nil will be returned.  
```ruby
pattern = /t?igers/
puts pattern =~ ("tigers")
=> 0
puts pattern =~ ("ligers")
=> 1
puts patter =~ ("bears")
=> nil
```
**^** - should match beginning  
**$** - should match ending
```ruby
puts /^x/ =~ "xylem"
=> 0
puts /^x/ =~ "foxes"
=> nil
puts /z$/ =~ "quiz"
=> 3
puts /z$/ =~ "zanzibar"
=> nil
```
The **^** and **$** specifically match the start and end of a line, not a string.  If you have a string that spans multiple lines, you can use the **\A** and **\Z** anchors to match the start and end of the whole string, not just the line.   
  
Ruby regex can accept modifiers after the final /.  
```ruby
puts /yelling/i =~ "I'm not YELLING!"
=> 8
puts /yelling/ =~ "I'm not YELLING!"
=> nil
```
**i** character ignores *case* when performing a match  
**.** symbol is a wildcard
```ruby
/.ig/ =~ "wig"
=> 0
/.ig/ =~ "pig"
=> 0
```
```ruby
/.ickle$/i =~ "triCKle"
=> 1
/.ickle$/i =~ "tickle"
=> 0
/.ickle$/i =~ "TRICKLE"
=> 1
/.ickle$/i =~ "tricycle"
=> nil
/.ickle$/i =~ "trickles"
=> nil
```
Repetition symbols include both the **+** character, which means "match one or more occurrences of the preceding character," and the __*__ symbol, which means "match zero or more occurrences of the preceding character."   
```ruby
/p*ickle/ =~ "pickle"
=> 0
/p*ickle/ =~ "pppppickle"
=> 0
/p*ickle/ =~ "ickle"
=> 0

/p+ickle/ =~ "pickle"
=> 0
/p+ickle/ =~ "pppppickle"
=> 0
/p+ickle/ =~ "ickle"
=> nil
```
```ruby
/^A.*a$/ =~ "Ablania"
=> 0
/^A.*a$/ =~ "Argentina"
=> 0
/^A.*a$/ =~ "Afghanistan"
=> nil
```
**\\** *escape* character to match symbols used by regex
```ruby
/$/ =~ "$10.00"
=> 6
/\$/ =~ "$10.00"
=> 0
```
If you start regex with **%r** you can use any deliminator you want instead of forward slashes.  
```ruby
%r{/} =~ "forward/slash"  
=> 7
```
[Special characters, anchors and modifiers](https://ruby-doc.org/core-2.4.1/Regexp.html)  
[Anchors](https://ruby-doc.org/core-2.4.1/Regexp.html#class-Regexp-label-Anchors)  
[Metacharacters](https://ruby-doc.org/core-2.4.1/Regexp.html#class-Regexp-label-Metacharacters+and+Escapes)
## Advanced Matching
**Match** method  
Requires a string to match the pattern against, and optional positional argument to specify where in the string to start the search.  
`/R.*y/.match("Ruby", 1)`  
```ruby
m = /Ruby/.match("Scripting with Ruby!")
puts m.string
=> Scripting with Ruby!
m.regexp
=> /Ruby/
puts m.to_s
=> Ruby
puts m.pre_match
=> Scripting with 
puts m.post_match
=> !
```
Returns either nil or **MatchData** Object  
print original string you are trying to match with **.string** method.  
print pattern you are looking for with **.regexp** method.  
To see the string that matched your pattern, use the **.to_s** method.
Use **.pre_match** and **post_match** to see data before and after match.  
  
You can also use Blocks.
```ruby
/Ruby/.match("Scripting with Ruby!") do |m|
    puts m.to_s
end
=> Ruby

/Emerald/.match("Scripting with Ruby!") do |m|
    puts m.to_s
end
=> nil
```
MatchData Objects separate any MatchData captured in parenthetical groups.  
Regular expression patterns can be grouped using parenthesis, usually called *capture groups*.
```ruby
m = /(abc).*(123).*/i.match("abcdefg1234567")
=> #<MatchData "abcdefg1234567" 1:"abc" 2:"123">
puts m.captures.inspect
=> ["abc", "123"]
puts m[0]
=> abcdefg1234567
puts m[1]
=> abc
puts m[2]
=> 123
```
**Captures** method  
**Inspect** method - gives you a representation of the object it's called on  
In example above, m.capture returns an array.  
The first element contains the text matched by the entire regular expression  
Each successive element contains data that was matched by every subsequent match group.
## Character Classes
Allow you to match against the set of characters contained within them.  
Delineated by square brackets. **[]**
```ruby
regex = /[Rr]uby/ #will match both below:
m1 = regex.match("Ruby")
puts m1.to_s
=> Ruby

m2 = regex.match("ruby")
pust m2.to_s
=> ruby
```
A **-** specifies a range of characters.
```ruby
m = /[0-9]/.match("2")
puts m.to_s
=> 2

puts /[a-z0-9]/.match("a").to_s
=> a
puts /[a-z0-9].match("5").to_s
=> 5
```
Ruby also offers some specialized metacharacters that behave like character classes. These can be used as shortcuts to match specific types of text.   
```ruby
puts /\d/.match("5").to_s #\d metacharacter to match numeric digit
=> 5
puts /\d/.match("a").to_s
=> nil

puts /\d{3}/.match("123").to_s #match any 3 numbers
=> 123
puts /\d{3}/.match("123456").to_s
=> 123
```
```ruby
str = "July 31 07:51:48 mycomputer bad_process[12345]: ERROR Performing package upgrade"
regex = /\[(\d+)\]/
results = regex.match(str)
puts results.captures
=>12345
```
## Regular Expressions and String Methods
```ruby
s = "You say tomato, I say tomahto"
puts s.sub(/tomah?to/, "banana")
=> You say banana, I say tomahto
```
You can also iterate through a string using regular expressions and the scan method, which accepts a regexp as an argument.  
```ruby
"Ruby".scan(/./) do |letter|
    puts letter
end
=> R
=> u
=> b
=> y
```
You can use **split** to chop up a string into an array, using either a character or regular expression to tell Ruby where to do the splitting.  
```ruby
octets = "192.168.1.1".split(/\./)
octets.each { |octet| puts octet }
=> 192
=> 168
=> 1
=> 1
```
## Summary
# Log File Processing
## Processing Log Files
`File.open()` Open *method*, from the File *Class*
```ruby
File.open("/var/log/syslog").each do |line|
    puts line
end
```
**Cron job** - Used to schedule scripts on Unix-based operating systems
```ruby
File.open("/var/log/syslog").each do |line|
    if line.include?("CRON")
        #process the file
    end
end
```
* Escape characters
* Capture groups
* End-of-string anchor

`/\((.+))$/`  **$** anchors to the end of the log line, username is wrapped in parentheses so escape them **\\** , then use a capture group **( )** to obtain only the contents between the parentheses.
```ruby
str = "July 6 14:03:01 computer.name CRON[29440]: USER (naughty_user)"
```
Remember that *hashes* are data structures used to store information in **key=>value** format.  Each entry has a key with an associated value.  
```ruby
h = Hash.new(0)
usernames = {"admin" => 0}
usernames["admin"] += 1
puts usernames
=> {"admin"=>1}
```
```ruby
usernames = Hash.new(0)

File.open("/var/log/syslog").each do |line|
    if line.include?("CRON")
        m = /\((.+)\)$/.match(line)
        username = m[1]
        usernames[username] +=1
    end
end

puts usernames
```
## Command Line Text Processing
Using **grep** with cron log example
```bash
grep naughty_user /var/log/systemlog | wc -l

grep CRON /var/log/systemlog | grep -oE '\((.+)\)$'
```
Along with saving scripts to a file, Ruby also offers system administrators the ability to **process text from the command line** itself, without having to go through the trouble of writing and saving a new script file.  
**-e** - This flag lets us execute code we pass directly into Ruby on the command line, without the need tos ave it to disk first.  
`ruby -e 'puts "This is pretty cool!"'`  
**-n** - This flag tells Ruby that we want to apply our code to each line of STDIN until there's no more.  
```ruby
while gets
    # Process the line here with the code given by the -e flag
end
```
Ruby stores each line of the output in a special variable called **$_**  
`cat /var/log/systemlog | ruby -ne 'if $_.include?("CRON") then m = /\((.+)\)$/.match($_); puts m.captures end'`
# Other Data Formats
## Introduction
* Structure
* Store
* Transport

**HTML** - A markup format which defines the content of a webpage  
**JSON** - A data-interchange format commonly used to pass data between computers on networks (especially the internet)  
Ruby provides many libraries for using these formats. e.g.:  
* CSV
* JSON

Gems can extend formats for Ruby  
* Google's Protobuf format
* Nokogiri gem

## Reading CSV
CSV stands for "Comma-Separated Values."  
CSV files are stored in plain text, and each line in a csv file generally represents a single data record.  
`require "csv"`
```ruby
require "csv"

CSV.foreach("csv_file.txt") do |line|
    puts line.inspect
end
```
The return value of the CSV read method is an "array of arrays", which means that several inner arrays are nested inside an outer one.
```ruby
require "csv"
people = CSV.read("csv_file.txt")

puts people[0].inspect

puts people [0][0]
```
## Searching CSV files
**find** method  
**find_all** method  
These methods are provided by the *Array* Class.  
```ruby
perl = people.find { |person| person[0] =~ /^Perl/ }
puts perl.inspect

ruby = people.find_all { |person| person[0] =~ /^Ruby/ }
puts ruby.inspect
```
## Modifying CSV files
**add_row** method  
```ruby
require "csv"

CSV.open("csv_file.txt", "a") do |people|
    new_person = ["Rusy Haskell", "698-369-1247", "SRE"]
    people.add_row(new_person)
end
```
The important difference between File.open and CSV.open is that with File.open, you append strings to a file.  With CSV.open, you append rows, which are represented as arrays.
## CSV Summary
It can be helpful to think of a CSV file in terms of a spreadsheet, where each line corresponds to a rowe, and each comma-separated field corresponds to a column.  
Another CSV gem - **fastercsv**  
[fastercsv library](https://rubygems.org/gems/fastercsv)
## HTML processing
HTML - HyperText Markup Language  
HTML data format - Describes and defines the content of a webpage  
**Scrape** - to download and process  
`curl www.google.com | grep google`  
## HTML Parsing Libraries
**Nokogiri** Gem  
```ruby
require "nokogiri"
html_page = Nokogiri::HTML("<html><title>A miniscule HTML page</title></html>")
```
More often than not, you'll want to access HTML-formatted data from an **external source** rather than a string.  
```ruby
require "nokogiri"

html_page = File.open("webpage.html") { |file| Nokogiri::HTML(file) }
puts html_page.title
```
**OpenURI** Class  
```ruby
require 'nokogiri'
require 'open-uri'

html_page = Nokogiri::HTML(open("http://www.google.com/"))
puts html_page.title
```
**CSS** nokogiri method  
```ruby
html_page = File.open("webpage.html") { |file| Nokogiri::HTML(file) }
puts html_page.search("p")
```
[OpenURI class](https://ruby-doc.org/stdlib-2.4.1/libdoc/open-uri/rdoc/OpenURI.html)  
[nokogiri](http://www.nokogiri.org/)
## Summary




