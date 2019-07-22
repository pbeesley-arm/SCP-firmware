.. role:: raw-latex(raw)
   :format: latex
..

Coding Style
============

To maintain consistency within the SCP/MCP software source code a series
of rules and guidelines have been created; these form the project's
coding style.

Encoding
--------

The source code must use the UTF-8 encoding. Comments, documentation and
strings may use non-ASCII characters when required (e.g. Greek letters
used for units).

Naming
------

Function, variable, file name and type names must: - Be written in
lower-case - Have compound words separated by underline characters -
Have descriptive names, avoiding contractions where possible
(e.g.cluster\_count instead of clus\_cnt)

Avoid using: - Camel case syntax (e.g. cssClusterCount) - Hungarian
notation, encoding types within names (e.g. int iSize)

Functions, macros, types and defines must have the "fwk\_" prefix (upper
case for macros and defines) to identify framework API.

It is fine and encouraged to use a variable named "i" (index) for loops.

Header Files
------------

The contents of a header file should be wrapped in an 'include guard' to
prevent accidental multiple inclusion of a single header. The definition
name should be the upper-case file name followed by "\_H". An example
for fwk\_mm.h follows:

:raw-latex:`\code
`#ifndef FWK\_MM\_H #define FWK\_MM\_H

(...)

endif /\* FWK\_MM\_H \*/ :raw-latex:`\endcode`
==============================================

The closing endif statement should be followed directly by a single-line
comment which replicates the full guard name. In long files this helps
to clarify what is being closed.

Space between definition inside the header file should be a single line
only.

If a unit (header or C file) requires a header, it must include that
header instead of relying on an indirect inclusion from one of the
headers it already includes.

Inclusions
----------

Header file inclusions should follow a consistent sequence, defined as:

-  Standard library (stdbool, stdint, etc)
-  Framework components (fwk\_.h)
-  Modules

For each group, order the individual headers alphabetically.

Indentation and Scope
---------------------

Indentation is made of spaces, 4 characters long with each line being at
most 80 characters long. Following K&R style, the open-brace goes on the
same line as the statement:

:raw-latex:`\code
`if (x == y) { (...) } :raw-latex:`\endcode`

The only exception is for functions, which push the opening brace to the
following line:

:raw-latex:`\code
`void function\_a(int x, int y) { (...) } :raw-latex:`\endcode`

Similarly, the case and default keywords should be aligned with the
switch statement:

:raw-latex:`\code
`switch (option) { case 1: (...) break; default: (...) break; }
:raw-latex:`\endcode`

Conditional statements with single line of code should not use braces,
preferring indentation only. A statement that spans multiple lines must
use braces to improve readability:

:raw-latex:`\code
`if (condition\_a == true) function\_call\_a();

if (condition\_b == true) { function\_call\_b(long\_variable\_name\_x \|
long\_variable\_name\_y); } :raw-latex:`\endcode`

In a chain of if-else statements involving multi-line and single-line
blocks, it is acceptable to mix statements with and without braces:

:raw-latex:`\code
`if (condition == [a]) { function\_call\_a(long\_variable\_name\_x \|
long\_variable\_name\_y); } else if (condition == [b])
function\_call\_b(); :raw-latex:`\endcode`

Empty loop statements should use "continue" instead of empty braces or
single semi-colon:

:raw-latex:`\code
`while (condition == false) continue; :raw-latex:`\endcode`

Multi-line statements should align on the openning delimiter:

:raw-latex:`\code
`long\_variable\_name = (long\_variable\_value << LONG\_CONSTANT\_POS) &
LONG\_CONSTANT\_MASK; :raw-latex:`\endcode`

In case the code extends beyond 80 columns, the first line can wrap
creating a new indented block: :raw-latex:`\code
` long\_variable\_name = (long\_variable\_value << LONG\_CONSTANT\_POS)
& LONG\_CONSTANT\_MASK; :raw-latex:`\endcode`

When a stacked multi-line statement aligns with the next code level,
leave a blank line to highlight the separation:

:raw-latex:`\code
`if (condition\_a \|\| condition\_b \|\| condition\_c) {

::

    do_something();

} :raw-latex:`\endcode`

Function definitions should follow the same approach: :raw-latex:`\code
`int foo(unsigned int param\_a, unsigned param\_b, unsigned param\_c) {
... } :raw-latex:`\endcode`

Preprocessor statements should be aligned with the code they are related
to:

:raw-latex:`\code
`#ifdef HAS\_FOO int foo(void) { #ifdef HAS\_BAR return bar();

::

    #else
    return -1;

    #endif

} #endif :raw-latex:`\endcode`

Where preprocessor statements are nested and they target the same code
stream, indentation is allowed but the hash symbol must be left aligned
with the code stream:

:raw-latex:`\code
`#ifdef HAS\_FOO int foo(void) { #ifdef HAS\_BAR return bar();

::

    #else
    #   ifdef DEFAULT_ERROR
    return -1;

    #   else
    return 0

    #   endif
    #endif

} #endif :raw-latex:`\endcode`

**Note** Such constructions like the example above should be avoided if
possible.

Types
-----

Import "stdint.h" (part of the C Standard Library) for exact-width
integer types (uint8\_t, uint16\_t, etc). These types can be used
wherever the width of an integer needs to be specified explicitly.

Import "stdbool.h" (also part of the C Standard Library) whenever a
"boolean" type is needed.

Avoid defining custom types with the "typedef" keyword where possible.
Structures (struct) and enumerators (enum) should be declared and used
with their respective keyword identifiers. If custom types are used then
they must have the suffix "\_t" appended to their type name where it is
defined. This makes it easier to recognize types that have been defined
using "typedef" when they appear in the code.

When using sizeof() pass the variable name as the parameter to be
evaluated, and not its type. This prevents issues arising if the type of
the variable changes but the sizeof() parameter is not updated.

:raw-latex:`\code
`size\_t size; unsigned int counter;

/\* Preferred over sizeof(int) \*/ size = sizeof(counter);
:raw-latex:`\endcode`

Operator Precedence
-------------------

Do not rely on the implicit precedence and associativity of C operators.
Use parenthesis to make precedence and associativity explicit:

:raw-latex:`\code
`if ((a == 'a') \|\| (x == 'x')) do\_something(); :raw-latex:`\endcode`

Parenthesis around a unary operator and its operand may be omitted:

:raw-latex:`\code
`if (!a \|\| &a) do\_something(); :raw-latex:`\endcode`

Comments
--------

To ensure a consistent look, the preferred style for single-line
comments is to use the C89 style of paired forward-slashes and
asterisks:

:raw-latex:`\code
`/\* A short, single-line comment. \*/ :raw-latex:`\endcode`

For multi-line comments the same applies, adding an asterisk on each new
line:

:raw-latex:`\code
`/* * This is a multi-line comment \* where each line starts with \* an
asterisk. \*/ :raw-latex:`\endcode`

#if 0 is preferred for commenting out blocks of code where it is
necessary to do so.

:raw-latex:`\code
`void function\_a(int x, int y) { (...) }

if 0
====

void function\_b(int x, int y) { (...) } #endif

:raw-latex:`\endcode`

Macros and Constants
--------------------

All names of macros and constants must be written in upper-case to
differentiate them from functions and variables.

Logical groupings of constants should be defined as enumerations, with a
common prefix, so that they can be used as parameter types. To find out
the number of items in an "enum", make the last entry to be
<prefix>\_COUNT.

:raw-latex:`\code
`enum command\_id { COMMAND\_ID\_VERSION, COMMAND\_ID\_PING,
COMMAND\_ID\_EXIT, /\* Do not add entries after this line \*/
COMMAND\_ID\_COUNT };

void process\_cmd(enum command\_id id) { (...) } :raw-latex:`\endcode`

Prefer inline functions instead of macros.

Initialization
--------------

When local variables require being initialized to 0, please use their
respective type related initializer value: - 0 (zero) for integers - 0.0
for float/double - :raw-latex:`\0` for chars - NULL for pointers - false
for booleans (stdbool.h)

Array and structure initialization should use designated initializers.
These allow elements to be initialized using array indexes or structure
field names and without a fixed ordering.

Array initialization example: :raw-latex:`\code
`uint32\_t clock\_frequencies[3] = { [2] = 800, [0] = 675 };
:raw-latex:`\endcode`

Structure initialization example: :raw-latex:`\code
`struct clock clock\_cpu = { .name = "CPU", .frequency = 800, };
:raw-latex:`\endcode`

Device Register Definitions
---------------------------

The format for structures representing memory-mapped device registers is
standardized.

-  The file containing the device structure must include stdint.h to
   gain access to the uintxx\_t and UINTxx\_C definitions.
-  The file containing the device structure must include fwk\_macros.h
   to gain access to the FWK\_R, FWK\_W and FWK\_RW macros.
-  All non-reserved structure fields must be prefixed with one of the
   above macros, defining the read/write access level.
-  Bit definitions should be declared via UINTxx\_C macros.
-  Bit definitions must be prefixed by the register name it relates to.
   If bit definitions apply to multiple registers, then the name must be
   as common as possible and a comment must explicit show which
   registers it applies to.
-  The structure name for the programmer's view must follow the pattern
   "struct \_reg { ...registers... };"

:raw-latex:`\code
`#include <stdint.h> #include <fwk\_macros.h>

struct devicename\_reg { /\* Readable and writable register \*/ FWK\_RW
uint32\_t CONFIG; uint32\_t RESERVED1;

::

    /* Write-only register */
    FWK_W  uint32_t IRQ_CLEAR;

    /* Read-only register */
    FWK_R  uint32_t IRQ_STATUS;
           uint32_t RESERVED2[0x40];

};

/\* Register bit definitions \*/ #define CONFIG\_ENABLE
UINT32\_C(0x00000001) #define CONFIG\_SLEEP UINT32\_C(0x00000002)

define IRQ\_STATUS\_ALERT UINT32\_C(0x00000001)
===============================================

:raw-latex:`\endcode`

**Note:** A template file can be found in doc/template/device.h

Doxygen Comments
----------------

The project APIs are documented using Doxygen comments.

It is mandatory to document every API exposed to other elements of the
project. By default, the provided Doxygen configuration omits
undocumented elements from the compiled documentation.

At a minimum: - All functions and structures must have at least a
":raw-latex:`\brief`" tag. - All functions must document their
parameters (if any) with the ":raw-latex:`\param`" tag. - All functions
should use the ":raw-latex:`\return`" or ":raw-latex:`\retval`" tags to
document their return value. When the return is void, simply give "None"
as the return value.

Alignment and indentation: - Documentation must also obey the 80 columns
limit. - Multiple lines of documentation on an entry (e.g. details) must
be indented using the equivalent of two 4-space based tabs (see example
below).

Function documentation example: :raw-latex:`\code
`/*! * :raw-latex:`\brief `Enable the watchdog. * *
:raw-latex:`\details `This function enables the watchdog. If
m\_wdog\_set\_interval() has \* not been called beforehand then the
watchdog defaults to a 500 \* millisecond timeout period. * *
:raw-latex:`\return `None. \*/ void m\_wdog\_enable(void);
:raw-latex:`\endcode`

Structure documentation example: :raw-latex:`\code
`/*! * :raw-latex:`\brief `Queue item \*/ typedef struct
\_fwk\_dlinks\_t { /*! Pointer to the next item */ struct
\_fwk\_dlinks\_t \*next;

::

    /*! Pointer to the previous item */
    struct _fwk_dlinks_t *prev;

} fwk\_dlinks\_t; :raw-latex:`\endcode`

Python based tools
------------------

Python based tools must follow the
`PEP8 <https://www.python.org/dev/peps/pep-0008/>`__ specification.
