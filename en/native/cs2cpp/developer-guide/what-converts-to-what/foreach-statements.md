---
date: "2021-07-09"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "ForeachStatements"
linktitle: "ForeachStatements"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2021-07-09"
weight: "1"
---

This example demonstrates how foreach statement is ported to C++. It is translated to C++ for statement.

Additional command-line options passed to CsToCppPorter: -o foreach_as_range_based_for_loop=true.

## Source C# Code ##

{{< highlight cs >}}
using System;
using System.Collections;
using System.Collections.Generic;
using CsToCppPorter;

namespace StatementsPorting
{
    public class ForeachStatements : IEnumerable<string>
    {
        [CppGenerateBeginEndMethods]
        private List<string> m_collection;

        public IEnumerator<string> GetEnumerator()
        {
            return m_collection.GetEnumerator();
        }

        [CppSkipEntity]
        IEnumerator IEnumerable.GetEnumerator()
        {
            return null;
        }

        public void Foreach(string[] values)
        {
            foreach (string value in values)
            {
                Console.WriteLine(value);
            }
        }

        public void EnclosedForeach(string[][] values)
        {
            foreach (string[] row in values)
                foreach (string value in row)
                {
                    Console.WriteLine(value);
                }
        }

        public void ForeachOverList()
        {
            var list = new List<string>(new string[] { "1", "2", "3" });
            foreach (string i in list)
            {
                Console.WriteLine(i);
            }
        }

        public void ForeachOverIList()
        {
            IList<string> list = new List<string>(new string[] { "1", "2", "3" });
            foreach (string i in list)
            {
                Console.WriteLine(i);
            }
        }

        public void ForeachOverThis()
        {
            m_collection = new List<string>(new string[] { "1", "2", "3" });
            foreach (string i in this)
            {
                Console.WriteLine(i);
            }
        }
    }
    
    public class Record
    {
        public int Value1 { get; set; }
        public int Value2 { get; set; }
    }

    public class Map : Dictionary<uint, Dictionary<uint, Record>>
    {
        public void Add(uint key, Record record)
        {
        }

        public void Add(Map map)
        {
            foreach (var pair in map)
            {
                foreach (var fir in pair.Value)
                    Add(pair.Key, fir.Value);
            }
        }
    }
    
}

{{< /highlight >}}

## Ported Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/details/pointer_collection_helpers.h>
#include <system/collections/list.h>
#include <system/collections/ienumerable.h>
#include <system/collections/dictionary.h>
#include <system/array.h>
#include <cstdint>

namespace System { namespace Collections { namespace Generic { template <typename> class IEnumerator; } } }

namespace StatementsPorting {

class ForeachStatements : public System::Collections::Generic::IEnumerable<System::String>
{
    typedef ForeachStatements ThisType;
    typedef System::Collections::Generic::IEnumerable<System::String> BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:
    /// A collection type whose iterator types is used as iterator types in the current collection.
    using iterator_holder_type = System::Collections::Generic::List<System::String>;
    /// Iterator type.
    using iterator = typename iterator_holder_type::iterator;
    /// Const iterator type.
    using const_iterator = typename iterator_holder_type::const_iterator;
    
public:

    System::SharedPtr<System::Collections::Generic::IEnumerator<System::String>> GetEnumerator() override;
    void Foreach(System::ArrayPtr<System::String> values);
    void EnclosedForeach(System::ArrayPtr<System::ArrayPtr<System::String>> values);
    void ForeachOverList();
    void ForeachOverIList();
    void ForeachOverThis();
    /// Gets iterator pointing to the first element (if any) of the collection.
    /// @return An iterator pointing to the first element (if any) of the collection
    iterator begin() noexcept;
    /// Gets iterator pointing right after the last element (if any) of the collection.
    /// @return An iterator pointing right after the last element (if any) of the collection
    iterator end() noexcept;
    /// Gets iterator pointing to the first element (if any) of the const-qualified instance of the collection.
    /// @return An iterator pointing to the first element (if any) of the const-qualified instance of the collection
    const_iterator begin() const noexcept;
    /// Gets iterator pointing right after the last element (if any) of the const-qualified instance of the collection.
    /// @return An iterator pointing right after the last element (if any) of the const-qualified instance of the collection
    const_iterator end() const noexcept;
    /// Gets iterator pointing to the first const-qualified element (if any) of the collection.
    /// @return An iterator pointing to the first const-qualified element (if any) of the collection
    const_iterator cbegin() const noexcept;
    /// Gets iterator pointing right after the last const-qualified element (if any) of the collection.
    /// @return An iterator pointing right after the last const-qualified element (if any) of the collection
    const_iterator cend() const noexcept;
    
protected:

    virtual ~ForeachStatements();
    
    #ifdef ASPOSE_GET_SHARED_MEMBERS
    System::Object::shared_members_type GetSharedMembers() override;
    #endif
    
    
private:

    System::SharedPtr<System::Collections::Generic::List<System::String>> m_collection;
    
};

class Record : public System::Object
{
    typedef Record ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    int32_t get_Value1();
    void set_Value1(int32_t value);
    int32_t get_Value2();
    void set_Value2(int32_t value);
    
    Record();
    
private:

    int32_t pr_Value1;
    int32_t pr_Value2;
    
};

class Map : public System::Collections::Generic::Dictionary<uint32_t, System::SharedPtr<System::Collections::Generic::Dictionary<uint32_t, System::SharedPtr<StatementsPorting::Record>>>>
{
    typedef Map ThisType;
    typedef System::Collections::Generic::Dictionary<uint32_t, System::SharedPtr<System::Collections::Generic::Dictionary<uint32_t, System::SharedPtr<StatementsPorting::Record>>>> BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    void Add(uint32_t const &key, System::SharedPtr<Record> const &record);
    void Add(System::SharedPtr<Map> const &map);
    void SetTemplateWeakPtr(uint32_t argument) override;
    
protected:

    virtual ~Map();
    
};

} // namespace StatementsPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "ForeachStatements.h"

#include <system/enumerator_adapter.h>
#include <system/console.h>
#include <system/collections/keyvalue_pair.h>
#include <system/collections/ilist.h>
#include <system/collections/ienumerator.h>

namespace StatementsPorting {

RTTI_INFO_IMPL_HASH(2238368721u, ::StatementsPorting::ForeachStatements, ThisTypeBaseTypesInfo);

System::SharedPtr<System::Collections::Generic::IEnumerator<System::String>> ForeachStatements::GetEnumerator()
{
    return m_collection->GetEnumerator();
}

void ForeachStatements::Foreach(System::ArrayPtr<System::String> values)
{
    for (System::String value : values)
    {
        System::Console::WriteLine(value);
    }
    
}

void ForeachStatements::EnclosedForeach(System::ArrayPtr<System::ArrayPtr<System::String>> values)
{
    for (System::ArrayPtr<System::String> row : values)
    {
        for (System::String value : row)
        {
            System::Console::WriteLine(value);
        }
        
    }
    
}

void ForeachStatements::ForeachOverList()
{
    auto list = System::MakeObject<System::Collections::Generic::List<System::String>>(System::MakeArray<System::String>({u"1", u"2", u"3"}));
    for (const auto& i : list)
    {
        System::Console::WriteLine(i);
    }
}

void ForeachStatements::ForeachOverIList()
{
    System::SharedPtr<System::Collections::Generic::IList<System::String>> list = System::MakeObject<System::Collections::Generic::List<System::String>>(System::MakeArray<System::String>({u"1", u"2", u"3"}));
    for (const auto& i : System::IterateOver(list))
    {
        System::Console::WriteLine(i);
    }
}

void ForeachStatements::ForeachOverThis()
{
    m_collection = System::MakeObject<System::Collections::Generic::List<System::String>>(System::MakeArray<System::String>({u"1", u"2", u"3"}));
    for (const auto& i : *this)
    {
        System::Console::WriteLine(i);
    }
}

ForeachStatements::~ForeachStatements()
{
}

ForeachStatements::iterator ForeachStatements::begin() noexcept
{
    return m_collection->begin();
}

ForeachStatements::iterator ForeachStatements::end() noexcept
{
    return m_collection->end();
}

ForeachStatements::const_iterator ForeachStatements::begin() const noexcept
{
    return m_collection->cbegin();
}

ForeachStatements::const_iterator ForeachStatements::end() const noexcept
{
    return m_collection->cend();
}

ForeachStatements::const_iterator ForeachStatements::cbegin() const noexcept
{
    return m_collection->cbegin();
}

ForeachStatements::const_iterator ForeachStatements::cend() const noexcept
{
    return m_collection->cend();
}

#ifdef ASPOSE_GET_SHARED_MEMBERS
System::Object::shared_members_type StatementsPorting::ForeachStatements::GetSharedMembers()
{
    auto result = System::Object::GetSharedMembers();
    
    result.Add("StatementsPorting::ForeachStatements::m_collection", this->m_collection);
    
    return result;
}
#endif

RTTI_INFO_IMPL_HASH(3718855054u, ::StatementsPorting::Record, ThisTypeBaseTypesInfo);

int32_t Record::get_Value1()
{
    return pr_Value1;
}

void Record::set_Value1(int32_t value)
{
    pr_Value1 = value;
}

int32_t Record::get_Value2()
{
    return pr_Value2;
}

void Record::set_Value2(int32_t value)
{
    pr_Value2 = value;
}

Record::Record() : pr_Value1(0), pr_Value2(0)
{
}

RTTI_INFO_IMPL_HASH(1762413471u, ::StatementsPorting::Map, ThisTypeBaseTypesInfo);

void Map::Add(uint32_t const &key, System::SharedPtr<Record> const &record)
{
}

void Map::Add(System::SharedPtr<Map> const &map)
{
    for (auto pair : map)
    {
        for (auto fir : pair.get_Value())
        {
            Add(pair.get_Key(), fir.get_Value());
        }
    }
}

Map::~Map()
{
}

void ::StatementsPorting::Map::SetTemplateWeakPtr(uint32_t argument)
{
}

} // namespace StatementsPorting

{{< /highlight >}}
