---
date: "2021-05-09"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "ClassEvents"
linktitle: "ClassEvents"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2021-05-09"
weight: "1"
---

This example demonstrates how class events are ported to C++. Public, protected and private class events preserve their accessibility level. Internal class events become protected. Events are ported to fields of type System::Event<T> which is a part of asposecpplib.

Additional command-line options passed to CsToCppPorter: none.

## Source C# Code ##

{{< highlight cs >}}
namespace MembersPorting
{
    public delegate void SomeDelegate();

    public class ClassEvents
    {
        public event SomeDelegate PublicEvent;

        internal event SomeDelegate InternalEvent;

        protected event SomeDelegate ProtectedEvent;

        private event SomeDelegate PrivateEvent;
    }
}
{{< /highlight >}}

## Ported Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/object.h>
#include <system/multicast_delegate.h>
#include <system/event.h>

namespace MembersPorting {

using SomeDelegate = System::MulticastDelegate<void()>;

class ClassEvents : public System::Object
{
    typedef ClassEvents ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    System::Event<void()> PublicEvent;
    
protected:

    System::Event<void()> InternalEvent;
    System::Event<void()> ProtectedEvent;
    
private:

    System::Event<void()> PrivateEvent;
    
};

} // namespace MembersPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "ClassEvents.h"

namespace MembersPorting {

RTTI_INFO_IMPL_HASH(65540873u, ::MembersPorting::ClassEvents, ThisTypeBaseTypesInfo);

} // namespace MembersPorting

{{< /highlight >}}