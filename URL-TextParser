import re
import urllib.request

type = input("enter 'text' or 'url':   ")
texturl = input("enter:   ")

#positive word samples
words = { "great":"positive","good" :"positive", "awesome": "positive", "great" : "positive" , "amazing" : "positive", "cool": "positive", "wow": "positive", "lame" : "negative", "bad":"negative", "shit":"negative", "suck":"negative", "sucks":"negative", "awesome":"positive"}

def parse_text(text):
    text = text.lower()
    text_split = text.split()
    negative = 0
    positive = 0
    for word in range(0,len(text_split)):
        for entry in range(0,len(words)):
            if str(word) == str(entry):
                if words[str(entry)] == "positive":
                    positive = positive + 1
                else:
                    negative = negative + 1
    return positive, negative
    
#uses positive and negative word values to find the overall tone
                    
#return overall tone of text
def tone(positive, negative):
    result = ""
    if positive + negative == 0:
        result = "unknown"
        return result
    sum = positive + negative
    negative_percent = (negative/sum)*100
    positive_percent = (positive/sum)*100
    if positive_percent > negative_percent:
        result= "overall positive"
        return result
    if negative_percent > positive_percent:
        result = "overall positive"
        return result

#calls tone()
def find_tone(text): 
    final = tone(parse_text(text))
    return final

#return next link in html
def get_next_target(page):
    start_link = page.find('<a href=')
    if start_link == -1: 
        return None, 0
    start_quote = page.find('"', start_link)
    end_quote = page.find('"', start_quote + 1)
    url = page[start_quote + 1:end_quote]
    return url, end_quote

#return next html <img> content/link
def get_next_target_picture(page):
    start_link = page.find('<img src=')
    if start_link == -1: 
        return None, 0
    start_quote = page.find('"', start_link)
    end_quote = page.find('"', start_quote + 1)
    url = page[start_quote + 1:end_quote]
    return url, end_quote

def get_urls(url):
    urls = re.findall('http[s]?://(?:[a-zA-Z]|[0-9]|[$-_@.&+]|[!*\(\),]|(?:%[0-9a-fA-F][0-9a-fA-F]))+', url)
    return urls

#returns union of two lists
def union(p,q):
    if p == [] and q != []:
        return q
    elif p != [] and q == []:
        return p
    else:
        for e in q:
            if e not in p:
                p.append(e)
                return p

#get all links in page
def get_all_links(page):
    links = []
    while True:
        url,endpos = get_next_target(page)
        if url:
            links.append(url)
            page = page[endpos:]
        else:
            break
    return links
    
#returns next picture in html
def get_next_picture(page):
    start_link = page.find('<img src=')
    if start_link == -1: 
        return None, 0
    start_quote = page.find('"', start_link)
    end_quote = page.find('"', start_quote + 1)
    picture = page[start_quote + 1:end_quote]
    return picture, end_quote

#returns list of all pictures in html
def get_all_pictures(page):
    pictures = []
    while True:
        picture,endpos = get_next_target_picture(page)
        if picture:
            pictures.append(picture)
            page = page[endpos:]
        else:
            break
    return pictures
    
#parse numbers from html
def number_parse(text):
    numbers = []
    string = text.split(" ")
    for word in string:
        if word.isalpha():
            continue
        else:
            if "1234567890" not in word:
                continue
            else:
                numbers.append(int(word))
    return numbers

# returns word count of html
def word_count(text):
    number = 0
    text = text.split(" ")
    for word in text:
        number = number + 1
    return number

#returns amount of uppercase and lowercase letter in html
def word_info(text):
    caps = 0
    lowercase = 0
    info = text.split(" ")
    for word in info:
        if word[0].isupper():
            caps = caps + 1
        else:
            lowercase = lowercase + 1
    return caps, lowercase

#remove given value from given text
def removern(text, value):
    text.split(" ")
    return [word for word in text if word != value]

#remove html elements from text
def remove_html(text):
    ntext = str(text)
    remove = re.compile('<.*?>')
    newtext = re.sub(remove," ",ntext)
    new = removern(newtext, "\r\n")
    new2 = "".join(newtext)
    return new2

#returns true if input is in text, count of occurences, otherwise return false
def word_search(input, text):
    count = 0
    words = text.split(" ")
    for word in words:
        if input == word:
            count = count + 1
    if count > 0:
        return true, count
    else:
        return false, count
        
#parse html
def text_parse_url(url):
    link = ""
    if url.find("http") != -1:
        link = "" + str(url)
        url_text = urllib.request.urlopen(link)
    else:
        link = "http://"+str(url)
        url_text = urllib.request.urlopen(link)
    text = url_text.read()
    links = get_all_links(str(text))
    pictures = get_all_pictures(str(text))
    text = remove_html(text)
    print("\nLinks: " + str(links) + "\n\n\n\n")
    print("Pictures " + str(pictures) + "\n\n\n\n")
    print(text)

#parse text
def text_parse(text):
    links = get_all_links(text)
    pictures = get_all_pictures(text)
    numbers = number_parse(text)
    wordcount = word_count(text)
    caps, lowercase = word_info(text)
    print("\nLinks: " + str(links))
    print("\nPictures " + str(pictures))
    print("\nNumbers: " + str(numbers))
    print("This text has " + str(wordcount) + " words, " + str(caps) + " capital letters, and " + str(lowercase) + " lowercase letters")

#parse text or html
def parse(text):
    if type == 'url':
        text_parse_url(text)
    elif type == 'text':
        text_parse(text)
    else:
        print("You did not enter 'text' or 'url'")

parse(texturl)
