title = "ROXML Factory from Text Node Value"
date = "2011-08-29T12:15:00Z"
template = "main"
tags = []
---
# ROXML Basics  {#basics}

The [ROXML](http://github.com/Empact/roxml) library provides ruby objects with an XML binding that is easy to specify and round-trip. (Those familiar with ROXML may wish to skip to the [punch line](#interesting).)

    class Library
      include ROXML
      xml_accessor :name
    end

    lib = Library.from_xml( xml ) # pass an IO or String containing the XML
    lib.to_xml.to_s # render XML as a String

Easy.

Handle mixedCase element names such as _phoneNumber_ by specifying a convention.

    class Library

      include ROXML
   
      xml_convention do |s|
        c = s.camelcase
        c[0..0].to_s.downcase + c[1..-1].to_s
      end

      xml_accessor :phone_number
    end

Changing the element name for an object is easy too.

     class Library
       include ROXML
       xml_name 'mediaCenter'  # yuck
     end

We can nest objects:

     class Book
       include ROXML
       xml_accessor :title
       xml_accessor :author
     end

     class Library
       include ROXML
       xml_accessor :name
       xml_accessor :books, :in   => :books, 
                       :as   => [Book]
     end

    lib = Library.new
    lib.name = 'Fullerton Public'
    lib.books = []

    a_book = Book.new
    a_book.title  = 'The Story About Ping'
    a_book.author = 'Marjorie Flack'

    lib.books 
      <name>Fullerton Public</name>
      <books>
         <book>
            <title>The Story About Ping</title>
            <author>Marjorie Flack</author>
            ....

The reverse works too.

    Library.from_xml(xml).books.first.title # => The Story About Ping

ROXML also supports much more complex mappings of not just elements but attributes and not only to Arrays but also Hashes. The yardoc for [Declarations](http://rubydoc.info/gems/roxml/3.1.6/ROXML/ClassMethods/Declarations#xml_attr-instance_method) is a helpful start.

# An interesting problem {#interesting}

Now what if our system must also cope with special rules for journals, newspapers, eight-track tapes, laser discs, and the like? Since our XML is coming from a system designed by Acme Card Catalogs and Kitchen Appliances INC., we need to handle the following:

    <library>
       <media>
         <item>
            <medium>cassette-tape</medium>
            <title>The Story About Ping</title> 
            ....

So we need to look in the _media_ element to know which class to load--and we want ROXML to handle it all for us.

# Tricky Bits {#tricky}

In the following solution a base class, _Medium_, provides generic mappings for all media and a registration system for subclasses which define additional xml mappings. 

    class Medium

      include ROXML

      class  'item',
                   :in   => 'media',
                   :as   => [Medium]
    end
    
    CassetteTape.register
    Book.register

With this in place one may now read and write xml in the form:

    <library>
       <media>
         <item>
            <medium>cassette</medium>
            <title>The Story About Ping</title> 
            <readby>A. Reader</readby>
         </item>
         <item>
            <medium>book</medium>
            <title>The Story About Ping</title> 
            <binding>hardcover</binding>
         </item>
       </media>
    </library>

We now have objects with generic methods for all Media as well as data and behaviors specific to a medium.

     lib = Library.from_xml(xml)
     lib.media.each do |item|
       puts "#{item.title} (#{item.class})"
    end

The advantage of using this simple registration system is that we can easily substitute mocks and stubs for testing without a fancy dependency injection framework which is typically overkill in Ruby given the language's power to reopen classes and the availability terrific mocking/stubbing libraries. For example, to test generic functionality without dependencies on specific media:

    class FakeMedium </item></media></library></book></books>