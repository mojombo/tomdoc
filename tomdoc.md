TomDoc for Ruby - Version 1.0.0-rc1
===================================

Purpose
-------

TomDoc is a code documentation specification that helps you write precise
documentation that is nice to read in plain text, yet structured enough to be
automatically extracted and processed by a machine.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in RFC 2119.

Table of Contents
-----------------

* [Method Documentation](#method)
  * [The Description Section](#description)
  * [The Arguments Section](#arguments)
  * [The Yields Section](#yields)
  * [The Examples Section](#examples)
  * [The Returns/Raises Section](#returns)
  * [The Signature Section](#signature)
* [Class/Module Documentation](#class)
* [Constants Documentation](#constants)
* [Special Considerations](#special)
  * [Constructor](#constructor)
  * [Attributes](#attributes)


<a name="method" />
Method Documentation
--------------------

A quick example will serve to best illustrate the TomDoc method documentation
format:

    # Public: Duplicate some text an arbitrary number of times.
    #
    # text  - The String to be duplicated.
    # count - The Integer number of times to duplicate the text.
    #
    # Examples
    #
    #   multiplex('Tom', 4)
    #   # => 'TomTomTomTom'
    #
    # Returns the duplicated String.
    def multiplex(text, count)
      text * count
    end

TomDoc for a specific method consists of a block of single comment markers (#)
that appears directly above the method. There SHOULD NOT be a blank line
between the comment block and the method definition. A TomDoc method block
consists of six optional sections: a description section, an arguments section,
a yields section, an examples section, a returns section, and a signature
section. Sections MUST appear in the order listed above. Lines that contain
text MUST be separated from the comment marker by a single space. Lines that do
not contain text SHOULD consist of just a comment marker (no trailing spaces).


<a name="description" />
### The Description Section

The description section SHOULD be in plain sentences. Each sentence SHOULD end
with a period. Good descriptions explain what the code does at a high level.
Make sure to explain any unexpected behavior that the method may have, or any
pitfalls that the user may experience. Paragraphs SHOULD be separated with
blank lines. Code within the description section should be indented three
spaces from the starting comment symbol. Lines SHOULD be wrapped at 80
characters.

To describe the status of a method, you SHOULD use one of several prefixes:

**Public:** Indicates that the method is part of the project's public API.
This annotation is designed to let developers know which methods are
considered stable. You SHOULD use this to document the public API of your
project. This information can then be used along with [Semantic
Versioning](http://semver.org) to inform decisions on when major, minor, and
patch versions should be incremented.

    # Public: Initialize a new Widget.

**Internal:** Indicates that the method is part of the project's
internal API. These are methods that are intended to be called from other
classes within the project but not intended for public consumption. For
example:

    # Internal: Normalize the filename.

**Deprecated:** Indicates that the method is deprecated and will be removed
in a future version. You SHOULD use this to document methods that were Public
but will be removed at the next major version.

    # Deprecated: Resize an object to the given dimensions.

An example description that includes all of these elements might look
something like the following.

    # Public: Format some data with the given format. Possible format
    # identifiers include:
    #
    # %i   - Output the Integer i.
    # %f.n - Output a Float f with n decimal places rounded.
    #
    # The format String may include any text. To escape a percent sign, prefix
    # it with a backslash:
    #
    #   "The sale price was %f.n\% off retail."


<a name="arguments" />
### The Arguments Section

The arguments section consists of a list of arguments. Each list item MUST be
comprised of the name of the argument, a dash, and an explanation of the
argument in plain sentences. The expected type (or types) of each argument
SHOULD be clearly indicated in the explanation. When you specify a type, use
the proper classname of the type (for instance, use 'String' instead of
'string' to refer to a String type). If the argument has other constraints
(e.g. duck-typed method requirements), simply state those requirements. The
dashes following each argument name SHOULD be lined up in a single column.
Lines SHOULD be wrapped at 80 columns.  Wrapped lines MUST be indented, as a
block, under the parent line by at least two spaces, but SHOULD be indented
to match the indentation of the explanation. For example:

    # element - The Symbol representation of the element. The Symbol should
    #           contain only lowercase ASCII alpha characters.

An argument that is String-like might look like this:

    # actor - An object that responds to to_s. Represents the actor that
    #         will be output in the log.

All arguments are assumed to be required. If an argument is optional, you MUST
specify the default value:

    # host - The String hostname to bind (default: '0.0.0.0').

For hash arguments, you SHOULD enumerate each valid option in a way similar
to how normal arguments are defined:

    # options - The Hash options used to refine the selection (default: {}):
    #           :color  - The String color to restrict by (optional).
    #           :weight - The Float weight to restrict by. The weight should
    #                     be specified in grams (optional).

Ruby allows for some interesting argument capabilities. In those cases, try
to explain what's going on as best as possible. Examples are a good way to
demonstrate how methods should be invoked. For example:

    # Print a log line to STDOUT. You can customize the output by specifying
    # a block.
    #
    # msgs  - Zero or more String messages that will be printed to the log
    #         separated by spaces.
    # block - An optional block that can be used to customize the date format.
    #         If it is present, it will be sent a Time object representing
    #         the current time. Your block should return a String version of
    #         the time, formatted however you please.
    #
    # Examples
    #
    #   log("An error occurred.")
    #
    #   log("No such file", "/var/log/server.log") do |time|
    #     time.strftime("%Y-%m-%d %H:%M:%S")
    #   end
    #
    # Returns nothing.
    def log(*msgs, &block)
      ...
    end


<a name="yields" />
### The Yields Section

The yields section is used to specify what is sent to the implicitly given
block. The section MUST start with the word "Yields" and SHOULD contain a
description and type of the yielded object. For example:

    # Yields the Integer index of the iteration.

Lines SHOULD be wrapped at 80 columns. Wrapped lines MUST be indented, as a
block, under the parent line by at least two spaces.

<a name="examples" />
### The Examples Section

The examples section MUST start with the word "Examples" on a line by
itself. The next line SHOULD be blank. The following lines SHOULD be indented
by two spaces (three spaces from the initial comment marker) and contain code
that shows how to call the method and OPTIONAL examples of what it
returns. Everything under the "Examples" line is considered code, so
you SHOULD comment out lines that show return values. For example:

    # Examples
    #
    #   multiplex('x', 4)
    #   # => 'xxxx'
    #
    #   multiplex('apple', 2)
    #   # => 'appleapple'


<a name="returns" />
### The Returns/Raises Section

The returns section should explain in plain sentences what is returned from
the method. The line MUST begin with "Returns". If only a single thing is
returned, state the nature and type of the value. For example:

    # Returns the duplicated String.

If several different types may be returned, list all of them. For example:

    # Returns the given element Symbol or nil if none was found.

If the return value of the method is not intended to be used, then you should
simply state:

    # Returns nothing.

If the method raises exceptions that the caller may be interested in, add
additional lines that explain each exception and under what conditions it may
be encountered. The lines MUST begin with "Raises". For example:

    # Returns nothing.
    # Raises Errno::ENOENT if the file cannot be found.
    # Raises Errno::EACCES if the file cannot be accessed.

Lines SHOULD be wrapped at 80 columns. Wrapped lines MUST be indented, as a
block, under the parent line by at least two spaces. For example:

    # Returns the atomic mass of the element as a Float. The value is in
    #   unified atomic mass units.


<a name="signature" />
### The Signature Section

The signature section allows you to specify the nature of methods that are
dynamically created at runtime.

The section MUST start with the word "Signature" on a line by itself. The next
line SHOULD be blank. The following lines SHOULD be indented by two spaces
(three spaces from the initial comment marker) and contain special code that
shows the method signature(s). For complex dynamic signatures, you SHOULD name
and demarcate signature variables with `<>` for required parts and `[]` for
optional parts. Use `...` for repeating elements. If there are dynamic
elements to the signature, document them in the same way as the Arguments
section, but leave out any type declarations. Documentation for metaprogrammed
methods may exist independently of any actual code, or may appear above the
code that creates the methods. Use your best judgment.

    # Signature
    #
    #   find_by_<field>[_and_<field>...](args)
    #
    # field - A field name.

Because metaprogrammed methods may be difficult to decipher, it's best to
include an examples section to demonstrate proper usage. For example:

    # Public: Find Records by a specific field name and value. This method
    # will be available for each field defined on the record.
    #
    # args - The value or Array of values of the field(s) to find by.
    #
    # Examples
    #
    #   find_by_name_and_email("Tom", "tom@mojombo.com")
    #
    # Returns an Array of matching Records.
    #
    # Signature
    #
    #   find_by_<field>[_and_<field>...](args)
    #
    # field - A field name.


<a name="class" />
Class/Module Documentation
--------------------------

TomDoc for classes and modules follows the same form as Method Documentation
but only contains the Description and Examples sections.

    # Public: Various methods useful for performing mathematical operations.
    # All methods are module methods and should be called on the Math module.
    #
    # Examples
    #
    #   Math.square_root(9)
    #   # => 3
    module Math
      ...
    end

Just like methods, classes may be marked as Public, Internal, or Deprecated
depending on their intended use.


<a name="constants" />
Constants Documentation
-----------------------

Constants should be documented with freeform comments. The type of the
constant and any important constraints should be stated.

    # Public: Integer number of seconds to wait before connection timeout.
    CONNECTION_TIMEOUT = 60

Just like methods, constants may be marked as Public, Internal, or Deprecated
depending on their intended use.


<a name="special" />
Special Considerations
----------------------

<a name="constructor" />
### Constructor

A Ruby class's `initialize` method does not have a significant return value.
You MAY exclude the returns section. A larger description of the purpose of
this class should be done at the Class level.

    # Public: Initialize a Widget.
    #
    # name - A String naming the widget.
    def initialize(name)
      ...
    end

<a name="attributes" />
### Attributes

Ruby's built in `attr_reader`, `attr_writer`, and `attr_accessor` require a
bit more consideration. With TomDoc you SHOULD document each of these method
generators separately. Because each part of a method documentation section is
optional, you can write concise yet unambiguous docs.

Here is an example TomDoc for `attr_reader`.

    # Public: Returns the String name of the user.
    attr_reader :name

Here is an example TomDoc for `attr_writer`.

    # Public: Sets the String name of the user.
    attr_writer :name

For `attr_accessor` you can use an overloaded shorthand that documents the
getter and setter simultaneously:

    # Public: Gets/Sets the String name of the user.
    attr_accessor :name
