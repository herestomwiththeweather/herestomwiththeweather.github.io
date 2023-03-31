---
title: "Translating Books and Papers"
layout: post
date: 2023-03-31 16:38:20
---
I have a side project called [BookLoggr](https://github.com/herestomwiththeweather/bookloggr) which was created for the purpose of saving notes to books as I have done for decades but on the empty back pages of books.  A year ago I added a feature for uploading pdfs and found myself often just cutting and pasting interesting sentences.  I soon started saving academic papers as well as books.

Only recently have I begun to appreciate reading beginner books in french.  On each page, I'll find many new words that I need to look up and annotate with the english word inside the book.  It is very time consuming and awkward to put the book down, pull up [deepl.com](https://deepl.com/translate) and type the word into the translation form.  It is much less disruptive to be able to click a button to translate the page and I can optionally write the english word inside the physical copy of the book.

So, I decided to repurpose my existing interface for adding a note (which was already associated with a page number) to make the note the entire contents of a page.  Now, before I start reading the french book, I add each page of the book as a note.  Last night, I wrote [the code](https://github.com/herestomwiththeweather/bookloggr/commit/cb728b0f0552a84303e70d107f62eb95ecb226b0) to allow clicking a button to translate a page of french text using DeepL.

![DeepL](https://coffeebucks.s3.amazonaws.com/bookloggr_deepl.png)

I added the pages last night and will try it out and see how it goes...
