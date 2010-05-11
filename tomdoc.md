TomDoc for Ruby - Version 0.9.0
===============================

Purpose
-------

TomDoc is a code documentation specification that helps you write precise
documentation that is nice to read in plain text, yet structured enough to be
automatically extracted and processed by a machine.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in RFC 2119.


Class/Module Documentation
--------------------------

TomDoc for classes and modules consists of a block of single comment markers
(#) that appear directly above the class/module definition. Lines SHOULD be
wrapped at 80 characters. Lines that contain text MUST be separated from the
comment marker by a single space. Lines that do not contain text SHOULD
consist of just a comment marker (no trailing spaces).

Code examples SHOULD be indented two spaces (three spaces from the comment
marker).

    # Various methods useful for performing mathematical operations. All
    # methods are module methods and should be called on the Math module.
    # For example:
    #
    #   Math.square_root(9)
    #   # => 3
    #
    module Math
      ...
    end


Method Documentation
--------------------

A quick example will serve to best illustrate the TomDoc method documentation
format:

    # Duplicate some text an abitrary number of times.
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
that appears directly above the method. There MUST NOT be a blank line between
the comment block and the method definition. A TomDoc method block consists of
a description section (required), an arguments section (required if the method
takes any arguments), an examples section (optional), and a returns section
(required). Lines that contain text MUST be separated from the comment
marker by a single space. Lines that do not contain text SHOULD consist of
just a comment marker (no trailing spaces).

### The Description Section

The description section SHOULD be in plain sentences. Each sentence SHOULD end
with a period. Good descriptions explain what the code does at a high level.
Make sure to explain any unexpected behavior that the method may have, or any
pitfalls that the user may experience. Lines SHOULD be wrapped at 80
characters.

If a method's description begins with "Public:" then that method will be
considered part of the project's public API. For example:

    # Public: Initialize a new Widget.

This annotation is designed to let developers know which methods are
considered stable. You SHOULD use this to document the public API of your
project. This information can then be used along with [Semantic
Versioning](http://semver.org) to inform decisions on when major, minor, and
patch versions should be incremented.

If a method's description begins with "Deprecated:" then that method will be
considered as deprecated and users will know that it will be removed in a
future version.

### The Arguments Section

The arguments section consists of a list of arguments. Each list item MUST be
comprised of the name of the argument, a dash, and an explanation of the
argument in plain sentences. The expected type (or types) of each argument
SHOULD be clearly indicated in the explanation. When you specify a type, use
the proper classname of the type (for instance, use 'String' instead of
'string' to refer to a String type). The dashes following each argument name
should be lined up in a single column. Lines SHOULD be wrapped at 80 columns.
If an explanation is longer than that, additional lines MUST be indented at
least two spaces but SHOULD be indented to match the indentation of the
explanation. For example:

    # element - The Symbol representation of the element. The Symbol should
    #           contain only lowercase ASCII alpha characters.

All arguments are assumed to be required. If an argument is optional, you MUST
specify the default value:

    # host - The String hostname to bind (default: '0.0.0.0').

For hash arguments, you SHOULD enumerate each valid option in a way similar
to how normal arguments are defined:

    # options - The Hash options used to refine the selection (default: {}):
    #           :color  - The String color to restrict by (optional).
    #           :weight - The Float weight to restrict by. The weight should
    #                     be specified in grams (optional).

### The Examples Section

The examples section MUST start with the word "Examples" on a line by
itself. The next line SHOULD be blank. The following lines SHOULD be indented
by two spaces (three spaces from the initial comment marker) and contain code
that shows off how to call the method and (optional) examples of what it
returns. Everything under the "Examples" line should be considered code, so
make sure you comment out lines that show return values. Separate examples
should be separated by a blank line. For example:

    # Examples
    #
    #   multiplex('x', 4)
    #   # => 'xxxx'
    #
    #   multiplex('apple', 2)
    #   # => 'appleapple'

### The Returns Section

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

Lines SHOULD be wrapped at 80 columns. Wrapped lines MUST be indented under
the above line by at least two spaces. For example:

    # Returns the atomic mass of the element as a Float. The value is in
    #   unified atomic mass units.


Special Considerations
----------------------

### Attributes

Ruby's built in `attr_reader`, `attr_writer`, and `attr_accessor` require a
bit more consideration. With TomDoc you SHOULD NOT use `attr_access` since it
represents two methods with different signatures. Restricting yourself in this
way also makes you think more carefully about the read vs. write behavior and
whether each should be part of the Public API.

Here is an example TomDoc for `attr_reader`.

    # Public: Get the user's name.
    #
    # Returns the String name of the user.
    attr_reader :name

Here is an example TomDoc for `attr_writer`. The parameter name should be the
same as the attribute name.

    # Set the user's name.
    #
    # name - The String name of the user.
    #
    # Returns nothing.
    attr_writer :name

While this approach certainly takes up more space than listing dozens of
attributes on a single line, it allows for individual documentation of each
attribute. Attributes are an extremely important part of a class and should be
treated with the same care as any other methods.