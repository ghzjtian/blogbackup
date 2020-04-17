---
title: Protocol Buffer 的学习过程
tags: [data]
categories: [data]
date: 2019-12-23 17:46:19
---

> 记录 protocol 的用法.

<!-- more -->

## [1.参考](#references)
## [2.protocol buffer 定义及数据的保存形式](#data_type)
## [3.C#例子](#c_sharp_demo)

***
***
***

## 1.参考<a name="references"/>
* [0.Protocol buffers are a language-neutral, platform-neutral extensible mechanism for serializing structured data.](https://developers.google.com/protocol-buffers)
* [1.Mac下ProtocolBuffer的安装和编译](https://www.jianshu.com/p/93aeae0d78e5)
* [2.Protocol Buffer Basics: C#](https://developers.google.com/protocol-buffers/docs/csharptutorial)
* [3.Protocol Buffer Basics: Java](https://developers.google.com/protocol-buffers/docs/javatutorial)
* [4.Android Protobuf Tutorial with example app](https://medium.com/@elye.project/simple-android-protobuf-tutorial-with-actual-code-bfb581299f47?)

## 2.protocol buffer 定义及数据的保存形式<a name="data_type"/>
* 1.proto 文件的定义

```
syntax = "proto3";
package tutorial;

message Person {
    string name = 1;
    int32 id = 2;
    string email = 3;
    string phone = 4;
}

```

* 2.数据的显示

```
 Person.Builder person = Person.newBuilder();
        person.setEmail("wyzjtian@163.com");
        person.setId(11);
        person.setName("Tian");
        person.setPhone("12345678");

        // 真实的数据为:  10,4,84,105,97,110,16,11,26,16,119,121,122,106,116,105,97,110,64,49,54,51,46,99,111,109,34,8,49,50,51,52,53,54,55,56,
        byte[] data = person.build().toByteArray();
```

## 3.C#例子<a name="c_sharp_demo"/>

* 1.新建一个 proto 文件, 并且写入所需要的协议.

```
// See README.txt for information and build instructions.
//
// Note: START and END tags are used in comments to define sections used in
// tutorials.  They are not part of the syntax for Protocol Buffers.
//
// To get an in-depth walkthrough of this file and the related examples, see:
// https://developers.google.com/protocol-buffers/docs/tutorials

// [START declaration]
syntax = "proto3";
package tutorial;

//import "google/protobuf/timestamp.proto";
// [END declaration]

// [START java_declaration]
option java_package = "com.example.tutorial";
option java_outer_classname = "AddressBookProtos";
// [END java_declaration]

// [START csharp_declaration]
option csharp_namespace = "Google.Protobuf.Examples.AddressBook";
// [END csharp_declaration]

// [START messages]
message Person {
  string name = 1;
  int32 id = 2;  // Unique ID number for this person.
  string email = 3;

  enum PhoneType {
    MOBILE = 0;
    HOME = 1;
    WORK = 2;
  }

  message PhoneNumber {
    string number = 1;
    PhoneType type = 2;
  }

  repeated PhoneNumber phones = 4;

  //google.protobuf.Timestamp last_updated = 5;
}

// Our address book file is just one of these.
message AddressBook {
  repeated Person people = 1;
}
// [END messages]

```

* 2.编译 proto 文件，得出一个 cs 文件(需要先安装一个 protoc 编译器).

```
protoc --csharp_out=. ./addressbook.proto
```

* 3.在项目中导入 `Google.Protobuf` nuget 包, 这个包是对 proto 生成文件的解析.

* 4.在项目中应用

```
using System;
using System.IO;
using Google.Protobuf;
using Google.Protobuf.Examples.AddressBook;

namespace ProtocolBufferTest
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Start!");

            AddressBook addressBook;

            string addressBookFile = "addressbook.data";
            // Get the Address book
            if (File.Exists(addressBookFile))
            {
                using (Stream file = File.OpenRead(addressBookFile))
                {
                    addressBook = AddressBook.Parser.ParseFrom(file);
                }
            }
            else
            {
                Console.WriteLine("{0}: File not found. Creating a new file.", addressBookFile);
                addressBook = new AddressBook();
            }


            Person person = new Person();
            person.Name = "Tian";
            person.Id = 1;
            person.Email = "Abc@123.com";

            // Add an address
            addressBook.People.Add(person);

            // Write the new address book back to disk.
            using (Stream output = File.OpenWrite(addressBookFile))
            {
                addressBook.WriteTo(output);
            }

            // Read the existing address book.
            using (Stream stream = File.OpenRead(addressBookFile))
            {
                AddressBook addressBookRead = AddressBook.Parser.ParseFrom(stream);
                Print(addressBookRead);
            }

        }

        private static void Print(AddressBook addressBook) {
            foreach (Person person in addressBook.People) {
                Console.WriteLine(person.Id);
                Console.WriteLine(person.Name);
                Console.WriteLine(person.Email);
            }
            
        }

    }
}

```