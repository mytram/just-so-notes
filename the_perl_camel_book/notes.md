        Extracts From The Camel Book
... distilled books are like common distilled waters, flashy things.
                    Of Studies, Francis Bacon
Chapter 3: Unary and Binary Operators

Chapter 4: Statements and Declarations

_Simple Statements
* eval {}, do {}, and sub {} are simple, not compound, statements and they are just terms
* Any simple statement may optionally be followed by one of the following modifier, if, unless, while, until, or foreach (for)

_Compound Statements
* A sequence of statements within a scope is called a block. The scope can be an entire file, or a string being evaluated with eval, or a block surrounded by braces ({})
* Compound statements are built out of expressions and blocks surrounded by braces ({})

_if and unless Statements
* They are straightforward

_Loop Statements

__while and until Statements
* Nothing to say about it, refer to chaper 2 for implicit use of $_ in while loops
* while and until can have an optional continue {} block

__for Loops
* -t <FILEHANDLE>: FILEHANDLE is opened to a tty

__foreach Loops
* for and foreach can be used interchangeably
* foreach $key (sort keys %hash) { print "$key => $hash{$key}\n" }
* The foreach loop index variable is an implicit alias for each item in the list that you are looping over
* The loop index variable is implicitly lexical if the variable was previously declared with my: this means it is invisible to any function defined outside the lexical scope, even if called from within that loop
* If no lexical declaration is in scope the loop variable will be localised (dynamically scoped) global variable
* In either case, any previous value the localized had before the loop will be restored automatically upon loop exit
* The three points above may be better illustrated by the following example:

    sub find_some { print "not " unless defined $SOME;  print "found"; }

    $SOME = 10;
    foreach $SOME (9) { find_some; }    # found
    print $SOME, "\n";                          # 9

    undef $SOME;

    my $SOME = 10;
    foreach $SOME (9) { find_some; }    # not found
    print $SOME, "\n";                          # 9

__Loop Control - last, next, and redo
* A loop LABEL names the loop as a whole
* Loops are typically named for the item the loop is processing on each iteration
* The loop LABEL is optional for the loop control operators; if omitted, the operator refers to the innermost enclosing loop
* The LABEL can be anywhere in your dynamic loop!
* last exists the loop at once without executing the continue block, if any
* next skips the rest of the current iteration of the loop and starts the next; the continue block, if any, is executed before the condition is re-evaled
* redo starts the loop block without evaluating the conditional again; the continue block if any, is not executed
* An example of using redo:
     while (<>) {
         chomp;
         if (s/\\$//) {
             $_ .= <>;
             redo unless eof;
         }
         # process
     }

 _Bare Blocks

__Case Structures

__goto

_Global Declarations

* Declaration and definition

_Scoped Declarations
* The types of scope in Perl: block, file, and eval

__Scoped Variable Declarations
* my, our, local

__Lexically Scoped Variables: my
* scratchpad variables
* Lexcial variables are totally hidden from the world outside their immediately enclosing scope; reminder: there are three types of scope in Perl: block, file, and eval
* The newly declared variable does not show up until the statement after the statement containing the declaration
     my $x = $x; # mirrors the new x to the old x that will be hidden by the new one after this statement
* A global variable can only be accessed by using its qualified name if it's hidden by a lexical variable with the same name as its unqualified name

__Lexically Scoped Global Declarations: our
* lexically scoping a global varialbe is all we get by using our

__Dynamically Scoped Varaibles: local
* local on a global variable gives it a temporary value each time local is executed, but it does not affect that variable's global visibility. When the program reaches the end of that dynamic scope, this temporary value is discarded and the original value restored

_Pragmas

__Controlling warnings
* use warnings;

__Controlling the Use of Globals
* use strict;

Chapter 5:
Chapter 6: Subroutines

_Syntax
* Calling using & circumvents prototypes
* Functions called indirectly by Perl's runtime system are in all
  capitals
* Subroutines used for constant values are customarily named with all
  caps too
_Semantics
* All function parameters are passed as one single, flat list of
  scalars, @_; Multiple return values are likewise returned to the
  called as one single, flat list of scalars
* @_ is a product of pass-by-reference semantics; @_ is a local array,
  but its values are aliases to the actual scalar parameters
* The value of the last expression evaluated is the return value of
  the sub if the explicit return statement is not present. The shadow of Lisp.
* The calling context, scalar or list, of the subroutine is the
  evaluation context of the final expression of the routine
* Calling context:
  sub foo { (10, 20, 30) }
  my @vals = foo;
  print "@{[@vals]}\n" # Print 10 20 30
  my $lastval = foo;
  print "$lastval\n"   # Print 30
__Tricks with Parameter Lists
* No great magic found here
__Error Indications
* wantarray returns true in list context, false in scalar context, and
  undef in void context
* `return;' returns false: undef in scalar context, and empty list in
  list context
__Scoping Issues
* my, our, and local
_Passing References
* Give the first reading to Chapter 8 now
* Take as quick a look at the section as possible
_Prototypes
* Prototype characters explained:
  * `require' means a compilation error will rise if not met
  * `force context' means the actual argument will be interpreted in the
    forced context
  * `;' <mandatory arguments>;<optional arguments>
  * `@' or `%' eats all the rest of the actual arguments and forces list
    context
  * `$' forces scalar context
  * `&' requires a reference to a named or anonymous subroutine
  * `*' allows the subroutine to accept anything in that slot that
    would be accepted by a built-in as a filehandle: a bare name, a
    constant, a scalar expression, a typeglob, or a reference to a
    typeglob; use qualify_to_ref to convert such arguments to a
    typeglob reference (use Symblol 'qualify_to_ref')
* A bare block is allowed in the slot prototyped by `&'; this feature
  can be used to expand the language
__Inlining Constant Functions
* In the example on page 229 GLARCH_VAL, N and NFACT are not inlined by my compiler
  (v5.8.8 ActivePerl) as redefining any of these does not give rise to
  the mandatory warning, which is used to indicate whether the
  compiler inlined a particular subroutine; meanwhile, redefining
  anyone of FLAG_FOO, FLAG_BAR, FLAG_MASK, OPT_GLARCH produces the
  warning saying that "Constant subroutine XYZ redefined at __FILE__
  line __LINE__."
* Now it's time to take a cursory look at use constant in Chapter 31
__Care with Prototypes
* Be careful of the prototype characters foring context
_Subroutine Attributes
__The locked and method Attributes
__The lvalue Attribute
Chapter 8 References
_What Is a Reference?
* Hard (or real) reference vs. symbolic reference
* The thing being referenced by a reference is called `referent' for
  lack of a more proper name
* A scalar variable can hold a reference pointing to a referent
* `Reference' vs. `dereference'
* Implicit referencing or dereferencing never occurs in Perl. Well,
  almost never. Well, it looks like another piece of seemingly useful
  information. Just forget about this remark though it pops up again later in
  the same chapter.

_Creating References
* On the section's title I argue whether `Creating' is the right word
  even taking into account autovivification
* `\' - this is the No 1 thing, but `\&' is for creating references referencing
  to subroutines

__Anonymous Data
* [ ], { }
* +{} is used to disambiguate braces
* No matter how many times you execute the line that creates a
  reference to a subroutine, it will still refer to the same
  subroutine; however, there may be several copies of lexcial variables

__Object Constructors
* Go read Chapter 12

__Hard References
* \*FILEHANDLE and *FILEHANDLE are interchangable when used to pass
  around filehandles
* Typeglobs can not be blessed, only references can
* Typeglobs references can't be passed back out of the scope of a
  localized typeglob; got to read chapter 4 to understand this and the example



Chapter 10: Packages

Chapter 11: Modules
* .pm files
* Go to http://www.cpan.org

_Using Modules
* use MODULE LIST; or use MODULE
* use does a preload of MODULE at compile time and then an import of the symbols you've requested so that they'll be available for the rest of the compilation
*
