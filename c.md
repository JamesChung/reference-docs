# C Reference

## Portability Issues

Five kinds of portability issues are enumerated in Annex J of the C Standard documents:

- Implementation-defined behavior
- Unspecified behavior
- Undefined behavior
- Locale-specific behavior
- Common extensions

### Implementation-Defined Behavior

Implementation-defined behavior is program behavior that’s not specified by the C Standard and that may offer different results among implementations, but has consistent, documented behavior within an implementation. An example of implementation-defined behavior is the number of bits in a byte.

Implementation-defined behaviors are mostly harmless but can cause defects when porting to different implementations. Whenever possible, avoid writing code that depends on implementation-defined behaviors that vary among the C implementations you might use to compile your code. A complete list of implementation-defined behaviors is enumerated in Annex J.3 of the C Standard. You can document your dependencies on these implementation-defined behaviors by using a static_assert declaration, as discussed in Chapter 11. Throughout this book, I note when code has implementation-defined behavior.


### Unspecified Behavior

Unspecified behavior is program behavior for which the standard provides two or more options. The standard imposes no requirements on which option is chosen in any instance. Each execution of a given expression may have different results or produce a different value than a previous execution of the same expression. An example of unspecified behavior is function parameter storage layout, which can vary among function invocations within the same program. Avoid writing code that depends on the unspecified behaviors enumerated in Annex J.1 of the C Standard.

### Undefined Behavior

Undefined behavior is behavior that isn’t defined by the C Standard, or less circularly, “behavior, upon use of a nonportable or erroneous program construct or of erroneous data, for which the standard imposes no requirements.” Examples of undefined behavior include signed integer overflow and dereferencing an invalid pointer value. Code that has undefined behavior is often erroneous, but is more nuanced than that. Undefined behaviors are identified in the standard as follows:

- When a “shall” or “shall not” requirement is violated, and that requirement appears outside a constraint, the behavior is undefined
- When behavior is explicitly specified by the words “undefined behavior”
- By the omission of any explicit definition of behavior

The first two kinds of undefined behavior are frequently referred to as explicit undefined behaviors, while the third kind is referred to as _implicit undefined behavior_. There is no difference in emphasis among these three; they all describe behavior that is undefined. The C Standard Annex J.2, “Undefined behavior,” contains a list of explicit undefined behaviors in C.

Developers frequently misunderstand undefined behaviors to be errors or omissions in the C Standard, but the decision to classify a behavior as undefined is intentional and considered. Behaviors are classified as undefined by the C Standards committee to do the following:

- Give the implementer license not to catch program errors that are difficult to diagnose
- Avoid defining obscure corner cases that would favor one implementation strategy over another
- Identify areas of possible conforming language extension in which the implementer may augment the language by providing a definition of the officially undefined behavior

Compilers (implementations) have the latitude to do the following:

- Ignore undefined behavior completely, giving unpredictable results
- Behave in a documented manner characteristic of the environment (with or without issuing a diagnostic)
- Terminate a translation or execution (with issuing a diagnostic)

None of these options are great (particularly the first), so it’s best to avoid undefined behaviors except when the implementation specifies these behaviors are defined to allow you to invoke a language augmentation.

### Locale-Specific Behavior and Common Extensions

_Locale-specific behavior_ depends on local conventions of nationality, culture, and language that each implementation documents. Common extensions are widely used in many systems but are not portable to all implementations.
