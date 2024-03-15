..
   Copyright (c) 2011, 2013, 2018, 2020-2023, Arm Limited and its affiliates.  All rights reserved.
   CC-BY-SA-4.0 AND Apache-Patent-License
   See LICENSE file for details

.. |release| replace:: 2023Q3
.. |date-of-issue| replace:: 6\ :sup:`th` October 2023
.. |copyright-date| replace:: 2011, 2013, 2018, 2020-2023
.. |footer| replace:: Copyright © |copyright-date|, Arm Limited and its
                      affiliates. All rights reserved.

.. _AAPCS64: https://github.com/ARM-software/abi-aa/releases
.. _AAELF64: https://github.com/ARM-software/abi-aa/releases
.. _CPPABI64: https://github.com/ARM-software/abi-aa/releases

Procedure Call Standard for the Arm® 64-bit Architecture (AArch64)
******************************************************************

.. class:: version

|release|

.. class:: issued

Date of Issue: |date-of-issue|

.. class:: logo

.. image:: Arm_logo_blue_RGB.svg
   :scale: 30%

.. section-numbering::

.. raw:: pdf

   PageBreak oneColumn

Preamble
========

Abstract
--------

This document describes the Procedure Call Standard used by the Application Binary Interface (ABI) for the Arm 64-bit architecture.

Keywords
--------

Procedure call, function call, calling conventions, data layout

Latest release and defects report
---------------------------------

Please check `Application Binary Interface for the Arm® Architecture
<https://github.com/ARM-software/abi-aa>`_ for the latest
release of this document.

Please report defects in this specification to the `issue tracker page
on GitHub
<https://github.com/ARM-software/abi-aa/issues>`_.

.. raw:: pdf

   PageBreak

Licence
-------

This work is licensed under the Creative Commons
Attribution-ShareAlike 4.0 International License. To view a copy of
this license, visit http://creativecommons.org/licenses/by-sa/4.0/ or
send a letter to Creative Commons, PO Box 1866, Mountain View, CA
94042, USA.

Grant of Patent License. Subject to the terms and conditions of this
license (both the Public License and this Patent License), each
Licensor hereby grants to You a perpetual, worldwide, non-exclusive,
no-charge, royalty-free, irrevocable (except as stated in this
section) patent license to make, have made, use, offer to sell, sell,
import, and otherwise transfer the Licensed Material, where such
license applies only to those patent claims licensable by such
Licensor that are necessarily infringed by their contribution(s) alone
or by combination of their contribution(s) with the Licensed Material
to which such contribution(s) was submitted. If You institute patent
litigation against any entity (including a cross-claim or counterclaim
in a lawsuit) alleging that the Licensed Material or a contribution
incorporated within the Licensed Material constitutes direct or
contributory patent infringement, then any licenses granted to You
under this license for that Licensed Material shall terminate as of
the date such litigation is filed.

About the license
-----------------

As identified more fully in the Licence_ section, this project
is licensed under CC-BY-SA-4.0 along with an additional patent
license.  The language in the additional patent license is largely
identical to that in Apache-2.0 (specifically, Section 3 of Apache-2.0
as reflected at https://www.apache.org/licenses/LICENSE-2.0) with two
exceptions.

First, several changes were made related to the defined terms so as to
reflect the fact that such defined terms need to align with the
terminology in CC-BY-SA-4.0 rather than Apache-2.0 (e.g., changing
“Work” to “Licensed Material”).

Second, the defensive termination clause was changed such that the
scope of defensive termination applies to “any licenses granted to
You” (rather than “any patent licenses granted to You”).  This change
is intended to help maintain a healthy ecosystem by providing
additional protection to the community against patent litigation
claims.

Contributions
-------------

Contributions to this project are licensed under an inbound=outbound
model such that any such contributions are licensed by the contributor
under the same terms as those in the `Licence`_ section.

Trademark notice
----------------

The text of and illustrations in this document are licensed by Arm
under a Creative Commons Attribution–Share Alike 4.0 International
license ("CC-BY-SA-4.0”), with an additional clause on patents.
The Arm trademarks featured here are registered trademarks or
trademarks of Arm Limited (or its subsidiaries) in the US and/or
elsewhere. All rights reserved. Please visit
https://www.arm.com/company/policies/trademarks for more information
about Arm’s trademarks.

Copyright
---------

Copyright (c) |copyright-date|, Arm Limited and its affiliates.  All rights
reserved.

.. raw:: pdf

   PageBreak

.. contents::
   :depth: 3

.. raw:: pdf

   PageBreak

About this document
===================

Change control
--------------

Current status and anticipated changes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following support level definitions are used by the Arm ABI specifications:

**Release**
   Arm considers this specification to have enough implementations, which have
   received sufficient testing, to verify that it is correct. The details of these
   criteria are dependent on the scale and complexity of the change over previous
   versions: small, simple changes might only require one implementation, but more
   complex changes require multiple independent implementations, which have been
   rigorously tested for cross-compatibility. Arm anticipates that future changes
   to this specification will be limited to typographical corrections,
   clarifications and compatible extensions.

**Beta**
   Arm considers this specification to be complete, but existing
   implementations do not meet the requirements for confidence in its release
   quality. Arm may need to make incompatible changes if issues emerge from its
   implementation.

**Alpha**
   The content of this specification is a draft, and Arm considers the
   likelihood of future incompatible changes to be significant.

Parts related to SME are at **Beta** release quality.

The ILP32 variant is at **Beta** release quality.

All other content in this document is at the **Release** quality level.

Change history
^^^^^^^^^^^^^^

If there is no entry in the change history table for a release, there are no
changes to the content of the document for that release.

.. class:: aapcs64-change-history

+------------+--------------------+------------------------------------------------------------------+
| Issue      | Date               | Change                                                           |
+============+====================+==================================================================+
| 00Bet3     | 25th November 2011 | Beta release                                                     |
+------------+--------------------+------------------------------------------------------------------+
| 1.0        | 22nd May 2013      | First public release                                             |
+------------+--------------------+------------------------------------------------------------------+
| 1.1-beta   | 6th November 2013  | ILP32 Beta                                                       |
+------------+--------------------+------------------------------------------------------------------+
| 2018Q4     | 31st December 2018 | Added rules for over-aligned types                               |
+------------+--------------------+------------------------------------------------------------------+
| 2019Q4     | 30th January 2020  | Github release with an open source license.                      |
|            |                    |                                                                  |
|            |                    | Major changes:                                                   |
|            |                    |                                                                  |
|            |                    | 1. New Licence_, with relative explanation in                    |
|            |                    |    `About the license`_.                                         |
|            |                    |                                                                  |
|            |                    | 2. New sections on Contributions_, `Trademark notice`_, and      |
|            |                    |    Copyright_.                                                   |
|            |                    |                                                                  |
|            |                    | 3. Specify that the frame chain should use the signed return     |
|            |                    |    address (`The Frame Pointer`_).                               |
|            |                    |                                                                  |
|            |                    | 4. Add description of half-precision Brain floating-point format |
|            |                    |    (`Half-precision Floating Point`_, `Half-precision format     |
|            |                    |    compatibility`_, `Arithmetic types`_, `Types varying by data  |
|            |                    |    model`_, `APPENDIX Support for Advanced SIMD Extensions`_).   |
|            |                    |                                                                  |
|            |                    | 5. Update C++ mangling to reflect existing practice              |
|            |                    |    (`APPENDIX C++ mangling`_).                                   |
|            |                    |                                                                  |
|            |                    | Minor changes:                                                   |
|            |                    |                                                                  |
|            |                    | 1. The section `Bit-fields subdivision`_ has been renamed to make|
|            |                    |    the associated implicit link target unique and avoid clashing |
|            |                    |    with the one of `Bit-fields`_.                                |
|            |                    |                                                                  |
|            |                    | 2. Several formatting changes have been applied to the sources to|
|            |                    |    fix the rendered page produced by github.                     |
+------------+--------------------+------------------------------------------------------------------+
| 2020Q2     | 1st July 2020      | Add requirements for stack space with MTE tags.                  |
|            |                    | Extend the AAPCS64 to support SVE types and registers.           |
|            |                    | Conform aapcs64 volatile bit-fields rules to C/C++.              |
+------------+--------------------+------------------------------------------------------------------+
| 2020Q3     | 1st October 2020   | Specify ABI handling for 8.7-A's new FPCR bits.                  |
+------------+--------------------+------------------------------------------------------------------+
| 2021Q1     | 12\ :sup:`th` April| - Clarify rule C.4 of the `Parameter passing rules`_ when there  |
|            | 2021               |   is an overaligned HFA.                                         |
|            |                    | - Minor formatting changes.                                      |
+------------+--------------------+------------------------------------------------------------------+
| 2021Q3     | 1\ :sup:`st`       | - Add support for Decimal-floating-point formats                 |
|            | November 2021      |                                                                  |
+------------+--------------------+------------------------------------------------------------------+
| 2022Q3     | 20\ :sup:`th`      | - Add alpha-level support for SME.                               |
|            | October 2022       | - Across the document, use “thread” rather than “process”.       |
+------------+--------------------+------------------------------------------------------------------+
| 2023Q3     | 6\ :sup:`th`       | In `Data Types`_  include _BitInt(N) in language mapping.        |
|            | October 2023       |                                                                  |
+------------+--------------------+------------------------------------------------------------------+
|            |                    | - Change the status of the SME support from Alpha to Beta.       |
|            |                    | - Add soft-float PCS variant.                                    |
+------------+--------------------+------------------------------------------------------------------+

References
^^^^^^^^^^

This document refers to, or is referred to by, the following documents:

.. class:: refs

+-------------------------------------------------------------------------+----------------------------------------------------+----------------------------------------------------------+
| Ref                                                                     | URL or other reference                             | Title                                                    |
+=========================================================================+====================================================+==========================================================+
| AAPCS64_                                                                | Source for this document                           | Procedure Call Standard for the Arm 64-bit Architecture  |
+-------------------------------------------------------------------------+----------------------------------------------------+----------------------------------------------------------+
| CPPABI64_                                                               | IHI 0059                                           | C++ ABI for the Arm 64-bit Architecture                  |
+-------------------------------------------------------------------------+----------------------------------------------------+----------------------------------------------------------+
| GC++ABI                                                                 | https://itanium-cxx-abi.github.io/cxx-abi/abi.html | Generic C++ ABI                                          |
+-------------------------------------------------------------------------+----------------------------------------------------+----------------------------------------------------------+
| C99                                                                     | https://www.iso.org/standard/29237.html            | C Programming Language ISO/IEC 9899:1999                 |
+-------------------------------------------------------------------------+----------------------------------------------------+----------------------------------------------------------+
| C2x                                                                     | http://www.open-std.org/jtc1/sc22/wg14/            | Draft C Programming Language (expected circa 2023)       |
+-------------------------------------------------------------------------+----------------------------------------------------+----------------------------------------------------------+


Terms and abbreviations
-----------------------

This document uses the following abbreviations:

A32
   The instruction set named Arm in the Armv7 architecture; A32 uses 32-bit
   fixed-length instructions.

A64
   The instruction set available when in AArch64 state.

AAPCS64
   Procedure Call Standard for the Arm 64-bit Architecture (AArch64).

AArch32
   The 32-bit general-purpose register width state of the Armv8 architecture,
   broadly compatible with the Armv7-A architecture.

AArch64
   The 64-bit general-purpose register width state of the Armv8 architecture.

ABI
   Application Binary Interface:

   1. The specifications to which an executable must conform in order to
      execute in a specific execution environment. For example, the
      *Linux ABI for the Arm Architecture*.

   2. A particular aspect of the specifications to which independently produced
      relocatable files must conform in order to be statically linkable and
      executable.  For example, the CPPABI64_, AAELF64_, ...

Arm-based
   ... based on the Arm architecture ...

Floating point
   Depending on context floating point means or qualifies: (a) floating-point
   arithmetic conforming to IEEE 754 2008; (b) the Armv8 floating point
   instruction set; (c) the register set shared by (b) and the Armv8 SIMD
   instruction set.

Q-o-I
   Quality of Implementation – a quality, behavior, functionality, or
   mechanism not required by this standard, but which might be provided
   by systems conforming to it.  Q-o-I is often used to describe the
   toolchain-specific means by which a standard requirement is met.

MTE
   The Arm architecture's Memory Tagging Extension.

SIMD
   Single Instruction Multiple Data – A term denoting or qualifying:
   (a) processing several data items in parallel under the control of one
   instruction; (b) the Armv8 SIMD instruction set: (c) the register set
   shared by (b) and the Armv8 floating point instruction set.

SIMD and floating point
   The Arm architecture’s SIMD and Floating Point architecture comprising
   the floating point instruction set, the SIMD instruction set and the
   register set shared by them.

_`SME`
   The Arm architecture's Scalable Matrix Extension.

SVE
   The Arm architecture's Scalable Vector Extension.

_`SVL`
   Streaming Vector Length; that is, the number of bits in a `Scalable Vector`_
   when the processor is in streaming mode.

_`SVL.B`
   As for `SVL`_, but measured in bytes rather than bits.

T32
   The instruction set named Thumb in the Armv7 architecture; T32 uses
   16-bit and 32-bit instructions.

VG
   The number of 64-bit “vector granules” in an SVE vector; in other words,
   the number of bits in an SVE vector register divided by 64.

ILP32
   SysV-like data model where int, long int and pointer are 32-bit.

LP64
   SysV-like data model where int is 32-bit, but long int and pointer are 64-bit.

LLP64
   Windows-like data model where int and long int are 32-bit, but long long int and pointer are 64-bit.

This document uses the following terms:

Routine, subroutine
   A fragment of program to which control can be transferred that, on completing its task, returns control to its caller at an instruction following the call. Routine is used for clarity where there are nested calls: a routine is the caller and a subroutine is the callee.

Procedure
   A routine that returns no result value.

Function
   A routine that returns a result value.

Activation stack, call-frame stack
   The stack of routine activation records (call frames).

Activation record, call frame
   The memory used by a routine for saving registers and holding local variables (usually allocated on a stack, once per activation of the routine).

PIC, PID
   Position-independent code, position-independent data.

Argument, parameter
   The terms argument and parameter are used interchangeably. They may denote a formal parameter of a routine given the value of the actual parameter when the routine is called, or an actual parameter, according to context.

Externally visible [interface]
   [An interface] between separately compiled or separately assembled routines.

Variadic routine
   A routine is variadic if the number of arguments it takes, and their type, is determined by the caller instead of the callee.

Global register
   A register whose value is neither saved nor destroyed by a subroutine. The value may be updated, but only in a manner defined by the execution environment.

Program state
   The state of the program’s memory, including values in machine registers.

Scratch register, temporary register, caller-saved register
   A register used to hold an intermediate value during a calculation (usually, such values are not named in the program source and have a limited lifetime). If a function needs to preserve the value held in such a register over a call to another function, then the calling function must save and restore the value.

Callee-saved register
   A register whose value must be preserved over a function call. If the function being called (the callee) needs to use the register, then it is responsible for saving and restoring the old value.

SysV
   Unix System V. A variant of the Unix Operating System. Although this specification refers to SysV, many other operating systems, such as Linux or BSD use similar conventions.

Platform
   A program execution environment such as that defined by an operating system or run-time environment. A platform defines the specific variant of the ABI and may impose additional constraints. Linux is a platform in this sense.

More specific terminology is defined when it is first used.

.. raw:: pdf

   PageBreak

Scope
=====

The AAPCS64 defines how subroutines can be separately written, separately compiled, and separately assembled to work together. It describes a contract between a calling routine and a called routine, or between a routine and its execution environment, that defines:

- Obligations on the caller to create a program state in which the called routine may start to execute.

- Obligations on the called routine to preserve the program state of the caller across the call.

- The rights of the called routine to alter the program state of its caller.

- Obligations on all routines to preserve certain global invariants.

This standard specifies the base for a family of *Procedure Call Standard* (PCS) variants generated by choices that reflect arbitrary, but historically important, choice among:

- Byte order.

- Size and format of data types: pointer, long int and wchar\_t and the format of half-precision floating-point values. Here we define three data models (see `The standard variants`_ and `Arm C and C++ language mappings`_ for details):

    - ILP32: **(Beta)** SysV-like variant where int, long int and pointer are 32-bit.

    - LP64: SysV-like variant where int is 32-bit, but long int and pointer are 64-bit.

    - LLP64: Windows-like variant where int and long int are 32-bit, but long long int and pointer are 64-bit.

- Whether floating-point operations use floating-point hardware resources or are implemented by calls to integer-only  routines [#aapcs64-f1]_.

This standard is presented in four sections that, after an introduction, specify:

- The layout of data.

- Layout of the stack and calling between functions with public interfaces.

- Variations available for processor extensions, or when the execution environment restricts the addressing model.

- The C and C++ language bindings for plain data types.

This specification does not standardize the representation of publicly visible C++-language entities that are not also C language entities (these are described in `CPPABI64`_) and it places no requirements on the representation of language entities that are not visible across public interfaces.

.. raw:: pdf

   PageBreak

Introduction
============

The AAPCS64 is the first revision of Procedure Call standard for the Arm 64-bit Architecture. It forms part of the complete ABI specification for the Arm 64-bit Architecture.


Design goals
------------

The goals of the AAPCS64 are to:

- Support efficient execution on high-performance implementations of the Arm 64-bit Architecture.

- Clearly distinguish between mandatory requirements and implementation discretion.


Conformance
-----------

The AAPCS64 defines how separately compiled and separately assembled routines can work together. There is an externally visible interface between such routines. It is common that not all the externally visible interfaces to software are intended to be publicly visible or open to arbitrary use. In effect, there is a mismatch between the machine-level concept of external visibility—defined rigorously by an object code format—and a higher level, application-oriented concept of external visibility—which is system specific or application specific.

Conformance to the AAPCS64 requires that [#aapcs64-f2]_:

- At all times, stack limits and basic stack alignment are observed (`Universal stack constraints`_).

- At each call where the control transfer instruction is subject to a BL-type relocation at static link time, rules on the use of IP0 and IP1 are observed (`Use of IP0 and IP1 by the Linker`_).

- The routines of each publicly visible interface conform to the relevant procedure call standard variant.

- The data elements [#aapcs64-f3]_ of each publicly visible interface conform to the data layout rules.

.. raw:: pdf

   PageBreak

Data types and alignment
========================

Fundamental Data Types
----------------------

`Table 1`_, shows the fundamental data types (Machine Types) of the machine.

.. _Table 1:

.. table:: Table 1, Byte size and byte alignment of fundamental data types

  +------------------------+---------------------------------------+------------+---------------------------+-----------------------------------------------+
  | Type class             | Machine type                          | Byte size  | Natural Alignment (bytes) | Note                                          |
  +========================+=======================================+============+===========================+===============================================+
  | Integral               | Unsigned byte                         | 1          | 1                         | Character                                     |
  |                        +---------------------------------------+------------+---------------------------+                                               |
  |                        | Signed byte                           | 1          | 1                         |                                               |
  |                        +---------------------------------------+------------+---------------------------+-----------------------------------------------+
  |                        | Unsigned half-word                    | 2          | 2                         |                                               |
  |                        +---------------------------------------+------------+---------------------------+                                               |
  |                        | Signed half-word                      | 2          | 2                         |                                               |
  |                        +---------------------------------------+------------+---------------------------+-----------------------------------------------+
  |                        | Unsigned word                         | 4          | 4                         |                                               |
  |                        +---------------------------------------+------------+---------------------------+                                               |
  |                        | Signed word                           | 4          | 4                         |                                               |
  |                        +---------------------------------------+------------+---------------------------+-----------------------------------------------+
  |                        | Unsigned double-word                  | 8          | 8                         |                                               |
  |                        +---------------------------------------+------------+---------------------------+                                               |
  |                        | Signed double-word                    | 8          | 8                         |                                               |
  |                        +---------------------------------------+------------+---------------------------+-----------------------------------------------+
  |                        | Unsigned quad-word                    | 16         | 16                        |                                               |
  |                        +---------------------------------------+------------+---------------------------+                                               |
  |                        | Signed quad-word                      | 16         | 16                        |                                               |
  +------------------------+---------------------------------------+------------+---------------------------+-----------------------------------------------+
  | Floating Point         | Half precision                        | 2          | 2                         | See `Half-precision Floating Point`_          |
  |                        +---------------------------------------+------------+---------------------------+-----------------------------------------------+
  |                        | Single precision                      | 4          | 4                         | IEEE 754-2008                                 |
  |                        +---------------------------------------+------------+---------------------------+                                               |
  |                        | Double precision                      | 8          | 8                         |                                               |
  |                        +---------------------------------------+------------+---------------------------+                                               |
  |                        | Quad precision                        | 16         | 16                        |                                               |
  |                        +---------------------------------------+------------+---------------------------+-----------------------------------------------+
  |                        | 32-bit decimal fp                     | 4          | 4                         | IEEE 754-2008 using BID encoding              |
  |                        +---------------------------------------+------------+---------------------------+                                               |
  |                        | 64-bit decimal fp                     | 8          | 8                         |                                               |
  |                        +---------------------------------------+------------+---------------------------+                                               |
  |                        | 128-bit decimal fp                    | 16         | 16                        |                                               |
  +------------------------+---------------------------------------+------------+---------------------------+-----------------------------------------------+
  | Short vector           | 64-bit vector                         | 8          | 8                         | See `Short Vectors`_                          |
  |                        +---------------------------------------+------------+---------------------------+                                               |
  |                        | 128-bit vector                        | 16         | 16                        |                                               |
  +------------------------+---------------------------------------+------------+---------------------------+-----------------------------------------------+
  | Scalable Vector        | VG×64-bit vector of 8-bit elements    | VG×8       | 16                        | See `Scalable Vectors`_                       |
  |                        +---------------------------------------+            |                           |                                               |
  |                        | VG×64-bit vector of 16-bit elements   |            |                           |                                               |
  |                        +---------------------------------------+            |                           |                                               |
  |                        | VG×64-bit vector of 32-bit elements   |            |                           |                                               |
  |                        +---------------------------------------+            |                           |                                               |
  |                        | VG×64-bit vector of 64-bit elements   |            |                           |                                               |
  +------------------------+---------------------------------------+------------+---------------------------+-----------------------------------------------+
  | Scalable Predicate     | VG×8-bit predicate                    | VG         | 2                         | See `Scalable Predicates`_                    |
  +------------------------+---------------------------------------+------------+---------------------------+-----------------------------------------------+
  | Pointer                | 32-bit data pointer **(Beta)**        | 4          | 4                         | See `Pointers`_                               |
  |                        +---------------------------------------+------------+---------------------------+                                               |
  |                        | 32-bit code pointer **(Beta)**        | 4          | 4                         |                                               |
  |                        +---------------------------------------+------------+---------------------------+                                               |
  |                        | 64-bit data pointer                   | 8          | 8                         |                                               |
  |                        +---------------------------------------+------------+---------------------------+                                               |
  |                        | 64-bit code pointer                   | 8          | 8                         |                                               |
  +------------------------+---------------------------------------+------------+---------------------------+-----------------------------------------------+


Half-precision Floating Point
-----------------------------

The architecture provides hardware support for half-precision values. Three formats are currently supported:

1. half-precision format specified in IEEE 754-2008

2. Arm Alternative format, which provides additional range but has no NaNs or Infinities.

3. Brain floating-point format, which provides a dynamic range similar to the 32-bit floating-point format, but with less precision.

The first two formats are mutually exclusive. The base standard of the AAPCS specifies use of the IEEE 754-2008 variant, and a procedure call variant that uses the Arm Alternative format is permitted.

Decimal Floating Point
----------------------

The AAPCS permits use of Decimal Floating Point numbers encoded using
the BID format as specified in IEEE 754-2008.  Unless explicitly noted
elsewhere, Decimal floating-point objects should be treated in exactly
the same way as (binary) Floating Point objects for the purposes of
structure layout, parameter passing, and result return.

.. note:: There is no support in the AArch64 ISA for Decimal Floating
	  Point, so all operations must be emulated in software.

Short Vectors
-------------

A short vector is a machine type that is composed of repeated instances of one fundamental integral or floating-point type. It may be 8 or 16 bytes in total size. A short vector has a base type that is the fundamental integral or floating-point type from which it is composed, but its alignment is always the same as its total size. The number of elements in the short vector is always such that the type is fully packed. For example, an 8-byte short vector may contain 8 unsigned byte elements, 4 unsigned half-word elements, 2 single-precision floating-point elements, or any other combination where the product of the number of elements and the size of an individual element is equal to 8. Similarly, for 16-byte short vectors the product of the number of elements and the size of the individual elements must be 16.

Elements in a short vector are numbered such that the lowest numbered element (element 0) occupies the lowest numbered bit (bit zero) in the vector and successive elements take on progressively increasing bit positions in the vector. When a short vector transferred between registers and memory it is treated as an opaque object. That is a short vector is stored in memory as if it were stored with a single STR of the entire register; a short vector is loaded from memory using the corresponding LDR instruction. On a little-endian system this means that element 0         will always contain the lowest addressed element of a short vector; on a big-endian system element 0 will contain the highest-addressed element of a short vector.

A language binding may define extended types that map directly onto short vectors. Short vectors are not otherwise created spontaneously (for example because a user has declared an aggregate consisting of eight consecutive byte-sized objects).

Scalable Vectors
----------------

.. _`Scalable Vector`:

Like a short vector (see `Short Vectors`_), a scalable vector is a
machine type that is composed of repeated instances of one fundamental
integral or floating-point type. The number of bytes in the vector is
always VG×8, where VG is a runtime value determined by the execution
environment. VG is an even integer greater than or equal to 2; the ABI
does not define an upper bound. VG is the same for all scalable vector
types and scalable predicate types.

Each element of a scalable vector has a zero-based index. When stored
in memory, the elements are placed in index order, so that element *N*
comes before element *N*\ +1. The layout of each individual element
is the same as if it were scalar. When stored in a scalable vector
register, the least significant bit of element 0 occupies bit 0
of the corresponding short vector register. Note that the layout of the
vector in a scalable vector register does not depend on whether the
system is big- or little-endian.

Scalable Predicates
-------------------

A scalable predicate is a machine type that is composed of individual bits.
The number of bits in the predicate is always VG×8, where VG is the same
value as for scalable vector types (see `Scalable Vectors`_). The number
of bits in a scalable predicate is therefore equal to the number of bytes
in a scalable vector.

Each bit of a scalable predicate has a zero-based index. When stored in
memory, index 0 is placed in the least significant bit of the first byte,
index 1 is stored in the next significant bit, and so on.

Pointers
--------

Code and data pointers are either 64-bit or 32-bit unsigned types [#aapcs64-f4]_. A NULL pointer is always represented by all-bits-zero.

All 64 bits in a 64-bit pointer are always significant. When tagged addressing is enabled, a tag is part of a pointer’s value for the purposes of pointer arithmetic. The result of subtracting or comparing two pointers with different tags is unspecified. See also `Memory addresses`_, below. A 32-bit pointer does not support tagged addressing.

.. note::

    **(Beta)**

    The A64 load and store instructions always use the full 64-bit base register and perform a 64-bit address calculation. Care must be taken within ILP32 to ensure that the upper 32 bits of a base register are zero and 32-bit register offsets are sign-extended to 64 bits (immediate offsets are implicitly extended).


Byte order ("Endianness")
-------------------------

From a software perspective, memory is an array of bytes, each of which is addressable. This ABI supports two views of memory implemented by the underlying hardware.

- In a little-endian view of memory the least significant byte of a data object is at the lowest byte address the data object occupies in memory.

- In a big-endian view of memory the least significant byte of a data object is at the highest byte address the data object occupies in memory.

The least significant bit in an object is always designated as bit 0.

The mapping of a word-sized data object to memory is shown in the following figures. All objects are pure-endian, so the mappings may be scaled accordingly for larger or smaller objects [#aapcs64-f5]_.

.. figure:: aapcs64-bigendian.svg
    :scale: 50%

    Memory layout of big-endian data object

.. figure:: aapcs64-littleendian.svg
    :scale: 50%

    Memory layout of little-endian data object


Composite Types
---------------

A Composite Type is a collection of one or more Fundamental Data Types that are handled as a single entity at the procedure call level. A Composite Type can be any of:

- An aggregate, where the members are laid out sequentially in memory (possibly with inter-member padding).

- A union, where each of the members has the same address.

- An array, which is a repeated sequence of some other type (its base type).

The definitions are recursive; that is, each of the types may contain a Composite Type as a member.

*  The *member alignment* of an element of a composite type is the
   alignment of that member after the application of any language alignment
   modifiers to that member

*  The *natural alignment* of a composite type is the maximum of
   each of the member alignments of the 'top-level' members of the composite
   type i.e. before any alignment adjustment of the entire composite is
   applied

.. _`aggregate`:

Aggregates
^^^^^^^^^^

- The alignment of an aggregate shall be the alignment of its most-aligned member.

- The size of an aggregate shall be the smallest multiple of its alignment that is sufficient to hold all of its members.

Unions
^^^^^^

- The alignment of a union shall be the alignment of its most-aligned member.

- The size of a union shall be the smallest multiple of its alignment that is sufficient to hold its largest member.

Arrays
^^^^^^

- The alignment of an array shall be the alignment of its base type.

- The size of an array shall be the size of the base type multiplied by the number of elements in the array.

Bit-fields subdivision
^^^^^^^^^^^^^^^^^^^^^^

A member of an aggregate that is a Fundamental Data Type may be subdivided into bit-fields; if there are unused portions of such a member that are sufficient to start the following member at its Natural Alignment then the following member may use the unallocated portion. For the purposes of calculating the alignment of the aggregate the type of the member shall be the Fundamental Data Type upon which the bit-field is based [#aapcs64-f6]_. The layout of bit-fields within an aggregate is defined by the appropriate language binding (see `Arm C and C++ Language Mappings`_).

Homogeneous Aggregates
^^^^^^^^^^^^^^^^^^^^^^

A Homogeneous Aggregate is a composite type where all of the Fundamental Data Types of the members that compose the type are the same. The test for homogeneity is applied after data layout is completed and without regard to access control or other source language restrictions. Note that for short-vector types the fundamental types are 64-bit vector and 128-bit vector; the type of the elements in the short vector does not form part of the test for homogeneity.

A Homogeneous Aggregate has a Base Type, which is the Fundamental Data Type of each Member. The overall size is the size of the Base Type multiplied by the number uniquely addressable Members; its alignment will be the alignment of the Base Type.

Homogeneous Floating-point Aggregates (HFA)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A Homogeneous Floating-point Aggregate (HFA) is a Homogeneous Aggregate with a Fundamental Data Type that is a Floating-Point type and at most four uniquely addressable members.

Homogeneous Short-Vector Aggregates (HVA)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A Homogeneous Short-Vector Aggregate (HVA) is a Homogeneous Aggregate with a Fundamental Data Type that is a Short-Vector type and at most four uniquely addressable members.

Pure Scalable Types (PSTs)
--------------------------

A type is a Pure Scalable Type if (recursively) it is:

* a Scalable Vector Type;

* a Scalable Predicate Type;

* an array that contains a constant (nonzero) number of elements and whose
  Base Type is a Pure Scalable Type; or

* an aggregate in which every member is a Pure Scalable Type.

As with Homogeneous Aggregates, these rules apply after data layout is
completed and without regard to access control or other source language
restrictions. However, there are several notable differences from
Homogeneous Aggregates:

* A Pure Scalable Type may contain a mixture of different Fundamental
  Data Types. For example, an aggregate that contains a scalable vector
  of 8-bit elements, a scalable predicate, and a scalable vector of
  16-bit elements is a Pure Scalable Type.

* Alignment and padding do not play a role when determining whether
  something is a Pure Scalable Type. (In fact, a Pure Scalable Type
  that contains both predicate types and vector types will often contain
  padding.)

* Pure Scalable Types are never unions and never contain unions.

.. note:: Composite Types have at least one member and the type of each
          member is either a Fundamental Data Type or another Composite Type.
          Since all Fundamental Data Types have nonzero size, it follows
          that all members of a Composite Type have nonzero size.

          Any language-level members that have zero size must therefore
          disappear in the language-to-ABI mapping and do not affect
          whether the containing type is a Pure Scalable Type.

.. raw:: pdf

   PageBreak

The Base Procedure Call Standard
================================

The base standard defines a machine-level calling standard for the A64 instruction set. It assumes the availability of the vector registers for passing floating-point and SIMD arguments. Application code is expected to conform to one of three data models defined in this standard; ILP32, LP64 or LLP64.

Machine Registers
-----------------

The Arm 64-bit architecture defines two mandatory register banks: a general-purpose register bank which can be used for scalar integer processing and pointer arithmetic; and a SIMD and Floating-Point register bank. In addition, the architecture defines an optional set of scalable vector registers that overlap the SIMD and Floating-Point register bank, accompanied by a set of scalable predicate registers.

General-purpose Registers
^^^^^^^^^^^^^^^^^^^^^^^^^

There are thirty-one, 64-bit, general-purpose (integer) registers visible to the A64 instruction set; these are labeled r0-r30. In a 64-bit context these registers are normally referred to using the names x0-x30; in a 32-bit context the registers are specified by using w0-w30. Additionally, a stack-pointer register, SP, can be used with a restricted number of instructions. Register names may appear in assembly language in either upper case or lower case. In this specification upper case is used when the register has a fixed role in this procedure call standard. `Table 2`_, General-purpose registers and AAPCS64 usage summarizes the uses of the general-purpose registers in this standard. In addition to the general-purpose registers there is one status register (NZCV) that may be set and  read by conforming code.

.. _Table 2:

.. class:: aapcs64-table-2

.. table:: Table 2, General-purpose registers and AAPCS64 usage

   +-----------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | Register  | Special  | Role in the procedure call standard                                                                                                                 |
   +===========+==========+=====================================================================================================================================================+
   | SP        |          | The Stack Pointer.                                                                                                                                  |
   +-----------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | r30       | LR       | The Link Register.                                                                                                                                  |
   +-----------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | r29       | FP       | The Frame Pointer                                                                                                                                   |
   +-----------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | r19…r28   |          | Callee-saved registers                                                                                                                              |
   +-----------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | r18       |          | The Platform Register, if needed; otherwise a temporary register. See notes.                                                                        |
   +-----------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | r17       | IP1      | The second intra-procedure-call temporary register (can be used by call veneers and PLT code); at other times may be used as a temporary register.  |
   +-----------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | r16       | IP0      | The first intra-procedure-call scratch register (can be used by call veneers and PLT code); at other times may be used as a temporary register.     |
   +-----------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | r9…r15    |          | Temporary registers                                                                                                                                 |
   +-----------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | r8        |          | Indirect result location register                                                                                                                   |
   +-----------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | r0…r7     |          | Parameter/result registers                                                                                                                          |
   +-----------+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------+


The first eight registers, r0-r7, are used to pass argument values into a subroutine and to return result values from a function. They may also be used to hold intermediate values within a routine (but, in general, only between subroutine calls).

Registers r16 (IP0) and r17 (IP1) may be used by a linker as a scratch register between a routine and any subroutine it calls (for details, see `Use of IP0 and IP1 by the linker`_). They can also be used within a routine to hold intermediate values between subroutine calls.

The role of register r18 is platform specific. If a platform ABI has need of a dedicated general-purpose register to carry inter-procedural state (for example, the thread context) then it should use this register for that purpose. If the platform ABI has no such requirements, then it should use r18 as an additional temporary register. The platform ABI specification must document the usage for this register.

.. note::

    Software developers creating platform-independent code are advised to avoid using r18 if at all possible. Most compilers provide a mechanism to prevent specific registers from being used for general allocation; portable hand-coded assembler should avoid it entirely. It should not be assumed that treating the register as callee-saved will be sufficient to satisfy the requirements of the platform. Virtualization code must, of course, treat the register as they would any other resource provided to the virtual machine.

A subroutine invocation must preserve the contents of the registers r19-r29 and SP. All 64 bits of each value stored in r19-r29 must be preserved, even when using the ILP32 data model **(Beta)**.

In all variants of the procedure call standard, registers r16, r17, r29 and r30 have special roles. In these roles they are labeled IP0, IP1, FP and LR when being used for holding addresses (that is, the special name implies accessing the register as a 64-bit entity).

.. note::

    The special register names (IP0, IP1, FP and LR) should be used only in the context in which they are special. It is recommended that disassemblers always use the architectural names for the registers.

The NZCV register is a global condition flag register with the following properties:

- The N, Z, C and V flags are undefined on entry to and return from a public interface.

SIMD and Floating-Point registers
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The Arm 64-bit architecture also has a further thirty-two registers, v0-v31, which can be used by SIMD and Floating-Point operations. The precise name of the register will change indicating the size of the access.

.. note::

    Unlike in AArch32, in AArch64 the 128-bit and 64-bit views of a SIMD and Floating-Point register do not overlap multiple registers in a narrower view, so q1, d1 and s1 all refer to the same entry in the register bank.

The first eight registers, v0-v7, are used to pass argument values into a subroutine and to return result values from a function. They may also be used to hold intermediate values within a routine (but, in general, only between subroutine calls).

Registers v8-v15 must be preserved by a callee across subroutine calls; the remaining registers (v0-v7, v16-v31) do not need to be preserved (or should be preserved by the caller). Additionally, only the bottom 64 bits of each value stored in v8-v15 need to be preserved [#aapcs64-f7]_; it is the responsibility of the caller to preserve larger values.

The FPSR is a status register that holds the cumulative exception bits of the floating-point unit. It contains the fields IDC, IXC, UFC, OFC, DZC, IOC and QC. These fields are not preserved across a public interface and may have any value on entry to a subroutine.

The FPCR is used to control the behavior of the floating-point unit. It is a global register with the following properties.

- The exception-control bits (8-12), rounding mode bits (22-23), flush-to-zero bits (24), and the AH and FIZ bits (0-1) may be modified by calls to specific support functions that affect the global state of the application.

- The NEP bit (bit 2) must be zero on entry to and return from a public interface.

- All other bits are reserved and must not be modified. It is not defined whether the bits read as zero or one, or whether they are preserved across a public interface.

Decimal Floating-Point emulation code requires additional control bits
which cannot be stored in the FPCR.  Since the information must be
held for each thread of execution, the state must be held in
thread-local storage on platforms where multi-threaded code is
supported.  The exact location of such information is platform
specific.

Scalable vector registers
^^^^^^^^^^^^^^^^^^^^^^^^^

The Arm 64-bit architecture also defines an optional set of thirty-two
scalable vector registers, z0-z31. Each register extends the
corresponding SIMD and Floating-Point register so that it can hold the
contents of a single Scalable Vector Type (see `Scalable vectors`_).
That is, scalable vector register z0 is an extension of SIMD and
Floating-Point register v0.

z0-z7 are used to pass scalable vector arguments to a subroutine, and to
return scalable vector results from a function. If a subroutine takes
at least one argument in scalable vector registers or scalable predicate
registers, or if it is a function that returns results in such registers,
it must ensure that the entire contents of z8-z23 are preserved across
the call. In other cases it need only preserve the low 64 bits of z8-z15,
as described in `SIMD and Floating-Point registers`_.

Scalable Predicate Registers
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The Arm 64-bit architecture defines an optional set of sixteen scalable
predicate registers p0-p15. These registers are available if and only if
the scalable vector registers are available (see `Scalable vector registers`_).
Each register can store the contents of a Scalable Predicate Type
(see `Scalable Predicates`_).

p0-p3 are used to pass scalable predicate arguments to a subroutine and
to return scalable predicate results from a function. If a subroutine takes
at least one argument in scalable vector registers or scalable predicate
registers, or if it is a function that returns results in such registers,
it must ensure that p4-p15 are preserved across the call. In other cases
it need not preserve any scalable predicate register contents.

SME state
---------

**(Beta)**

`SME`_ defines the following pieces of processor state:

ZA storage
   a storage array of size `SVL.B`_ × `SVL.B`_ bytes, hereafter referred to
   simply as “ZA”

_`PSTATE.SM`
   indicates whether the processor is in “streaming mode” (PSTATE.SM==1) or
   “non-streaming mode” (PSTATE.SM==0)

_`PSTATE.ZA`
   indicates whether ZA might have useful contents (PSTATE.ZA==1) or
   whether it definitely does not (PSTATE.ZA==0)

TPIDR2_EL0
   a system register that software can use to manage thread-local state

   See `TPIDR2_EL0`_ for a description of how the AAPCS64 uses this register.

Threads and processes
---------------------

.. _`threads`:

The AAPCS64 applies to a single _`thread` of execution.  Each thread is in
turn part of a _`process`.  A process might contain one thread or several
threads.

The exact definitions of the terms “thread” and “process” depend on the
platform. For example, if the platform is a traditional multi-threaded
operating system, the terms generally have their usual meaning for
that operating system. If the platform supports multiple processes
but has no separate concept of threads, each process will have a single
thread of execution. If a platform has no concurrency or preemption
then there will be a single thread and process that executes all
instructions.

Each thread has its own register state, defined by the contents of the
underlying machine registers. A process has a program state defined by
its threads' register states and by the contents of the memory that the
process can access. The memory that a process can access, without causing
a run-time fault, may vary during the execution of its threads.

Memory and the Stack
--------------------

Memory addresses
^^^^^^^^^^^^^^^^

The address space consists of one or more disjoint regions. Regions
must not span address zero (although one region may start at zero).

The use of tagged addressing is platform specific and does not apply to
32-bit pointers. When tagged addressing is disabled, all 64 bits of an
address are passed to the translation system. When tagged addressing is
enabled, the top eight bits of an address are ignored for the purposes
of address translation. See also `Pointers`_, above.

Properties of a thread
^^^^^^^^^^^^^^^^^^^^^^

**(Beta)**

The AAPCS64 classifies `threads`_ as follows, with the classification being
invariant for the lifetime of a given thread:


Footnotes
=========

.. [#aapcs64-f1]
   This base standard requires that AArch64 floating-point resources be used by floating-point operations and floating-point parameter passing. However, it is acknowledged that operating system code often prefers not to perturb the floating-point state of the machine and to implement its own limited use of floating-point in integer-only code: such code is permitted, but not conforming.

.. [#aapcs64-f2]
   This definition of conformance gives maximum freedom to implementers. For example, if it is known that both sides of an externally visible interface will be compiled by the same compiler, and that the interface will not be publicly visible, the AAPCS64 permits the use of private arrangements across the interface such as using additional argument registers or passing data in non-standard formats. Stack invariants must, nevertheless, be preserved because an AAPCS64-conforming routine elsewhere in the call chain might otherwise fail. Rules for use of IP0 and IP1 must be obeyed or a static linker might generate a non-functioning executable program.

   Conformance at a publicly visible interface does not depend on what happens behind that interface. Thus, for example, a tree of non-public, non-conforming calls can conform because the root of the tree offers a publicly visible, conforming interface and the other constraints are satisfied.

.. [#aapcs64-f3]
   Data elements include: parameters to routines named in the interface, static data named in the interface, and all data addressed by pointers passed across the interface.

.. [#aapcs64-f4]
   The distinction between code and data pointers is carried forward from the AArch32 PCS where bit[0] of a code pointer determines the target instruction set state, A32 or T32. The presence of an ISA selection bit within a code pointer can require distinct handling within a toolchain, compared to data pointer.

   ISA selection does not exist within AArch64 state, where bits[1:0] of a code pointer must be zero.

.. [#aapcs64-f5]
   The underlying hardware may not directly support a pure-endian view of data objects that are not naturally aligned.

.. [#aapcs64-f6]
   The intent is to permit the C construct ``struct {int a:8; char b[7];}`` to have size 8 and alignment 4.

.. [#aapcs64-f7]
   This includes double-precision or smaller floating-point values and 64-bit short vector values.

.. [#aapcs64-f8]
   The Advanced SIMD Extension does not provide any vector operations for Decimal Floating-point types, so short vector types are not defined for these.
