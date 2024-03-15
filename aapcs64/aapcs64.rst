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
