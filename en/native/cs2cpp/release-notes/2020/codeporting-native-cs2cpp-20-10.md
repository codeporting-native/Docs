---
date: "2020-08-16"
author:
  display_name: "Muhammad Rizwan"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 20.10"
linktitle: "CodePorting.Native Cs2Cpp 20.10"
menu:
  docs:
    parent: "2020"
    weight: "3"
lastmod: "2020-10-12"
weight: "3"
---

## Major Features
1. Porter is now capable of overloading methods that accept System.IO.Stream with auto-generated ones which accept STL stream instead. CppIOStreamWrapper attribute and force\_wrap\_iostream option are introduced to enable this.
2. use\_stream\_based\_io option was supported to translate System.Console invocations into std::cout operations.
3. DNS support was added into System::Net namespace.

## Minor fixes
1. False positive both CppConstMethod and CppConstWrapper attributes apply to same method warnings were fixed. The closest attribute is now tracked down properly.
2. Incorrect CsToCppPorter::Details::MemoryManagement::BindLifetime() overload selection was fixed. Comments were added to CsToCppPorter::Details::MemoryManagement methods for better understanding of how they work and which methods should be used.
3. Incorrect documentation for STDIOStreamWrapperBase and PropertyInfo classes was fixed.
4. Zero length graphics path elements are no longer kept, same as in .Net.
5. Queue(int capacity) constructor was fixed to create a zero-length queue.
6. ICollection <T>::Contains() method no longer throws NotImplementedException.
7. Some internal state checks were optimized in System::IO classes.
8. USING\_STATEMENT\_BEHAVIOR\_START and USING\_STATEMENT\_BEHAVIOR\_END macros were added to make it easier invoking the code related to using statement translation from C++ code.
9. Performance of is operator translation for final classes was improved.
10. Custom System::Details::fast\_equals() function was supported.
11. Stack trace of SmartPtr destructor was reduced.
12. SmartPtr::dynamic\_pointer\_cast of null-pointers was fixed.
13. LinearGradientBrush transformation methods were implemented.
14. Missing SetTemplateWeakPtr() methods generated by porter were fixed.
15. Matching filename masks was fixed for some cases.
16. HEAD request type was supported by HttpClientHandler class.
17. Custom XmlTextReader::ReadSubtreeInnerXml() method was supported.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release

|   Key |   Summary |   Category |
| --- | --- | --- |
| CSPORTCPP-3557 | Issues with &#39;override&#39; subsystem | Bug |
| CSPORTCPP-3579 | Make porter generate wrappers for methods which have Stream arguments | New feature |
| CSPORTCPP-3678 | Fix doxygen warnings | Bug |
| SLIDESCPP-2538 | Fix RegressionTests\_v19\_10.SLIDESNET\_33555 test (Linux) | Bug |
| SLIDESCPP-2581 | Fix a queue constructor which receive capacity | Bug |
| SLIDESCPP-2563 | Fix &quot;System::NotImplementedException&quot; when invoking ICollection<T>::Contains() method | Bug
| PDFCPP-1411 | System::IO optimization | Enhancement |
| CSPORTCPP-1833 | Add macros to use using wrapper in C++ code. | Enhancement |
| SLIDESCPP-2536 | Investigate solutions to improve performance of &quot;is&quot; operator | Enhancement |
| PDFCPP-1421 | Optimize ReverseSearch | Enhancement |
| WORDSCPP-1009 | Port Console.Write() and Console.WriteLine() calls using standart C++ stream-based I/O | Enhancement |
| CSPORTCPP-3692 | Optimize shared pointers | Enhancement |
| SLIDESCPP-2593 | Fix &quot;System::NotImplementedException&quot; when invoking LinearGradientBrush::TranslateTransform() method | Task |
| WORDSCPP-1015 | Fix method System::IO::Directory::GetFiles() | Bug |
| WORDSCPP-1018 | TestUtil::VerifyWebResponseStatusCode throws C++ exception with description &quot;partial message&quot; | Bug |
| WORDSCPP-1006 | Manually implement OdtParagraphReader.ParagraphProcessor | New feature |
| EMAILCPP-247 | Porting DNS support, implement DNS feature in Asposecpplib | New feature |

## Public API and Backward Incompatible Changes
1. Namespace for version information-related symbols was actualized.
2. Some redundant static and inline qualifiers were removed from public headers.
