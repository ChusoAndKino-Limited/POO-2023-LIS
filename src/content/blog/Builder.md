---
title: "Builder: .Net"
description: "The Builder design pattern is a creational pattern used to build a complex object step by step. This pattern separates the construction of an object from its representation, allowing the creation of different representations of the same object."
pubDate: "Dec 12 2023"
heroImage: "/blog-placeholder-4.jpg"
---

## Builder: .NET

Builder is a creational design pattern that lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code.

#### **Key components of the Builder pattern:**

**Director (Director):**

Coordinates the construction process through a constructor object. The director defines an abstract interface for construction and uses a concrete constructor to perform construction.

**Builder:**

An interface that defines the steps to build the object and an abstract class that implements this interface. Each construction step is usually represented by a method in the abstract constructor.

**Concrete Builder:**

Implements the builder interface and provides the specific implementation for constructing and assembling the parts of the object. Each concrete constructor can produce a different object.

**Product:**

The complex object that is built. The final representation of the object may vary depending on the constructor used.

#### **Builder pattern workflow:**

- A director object is created and associated with a concrete constructor.
- The director initiates the construction of the object by calling the constructor methods in a specific order.
- The concrete constructor implements the methods to construct and assemble the parts of the object.
- The final result is the complex object created according to the specifications of the director and the concrete constructor.

#### **Advantages of the Builder pattern:**

- Allows step-by-step construction of complex objects.
- It facilitates variability in the creation of objects by allowing different representations.
- It separates construction from representation, making the construction process more flexible.

#### **Typical usage of the Builder pattern:**

- When an object can have several representations.
- To build complex objects that require specific configurations.
- To improve the readability and maintainability of the code by separating the construction of the object from its internal structure.

## Real application example

---

[![net.png](https://i.postimg.cc/bJ5chPLq/net.png)](https://postimg.cc/R6wb7jjY)

.NET is a software development framework developed by Microsoft. It provides a unified platform for building a variety of applications, from desktop applications to web applications and web services. .NET is used to develop and run software on Windows operating systems, as well as in cross-platform environments through .NET Core and .NET 5 (and later).

There are some key components of the .NET platform:

**Common Language Runtime (CLR):** It is the .NET virtual machine that manages the execution of programs written in .NET compatible languages, such as C#, F#, and Visual Basic. The CLR provides services such as garbage collection, memory management, and type safety.

**.NET Class Library:** It is a set of reusable classes, interfaces, and types that developers use to build .NET applications. It provides a wide variety of functionality, from file manipulation to network operations.

**.NET Programming Languages:** .NET supports several programming languages, including C#, F#, Visual Basic, and others. Each language has its own set of features, but they all share the same CLR foundation, allowing interoperability between them.

**ASP.NET:** It is a framework designed specifically for building web applications and web services. Provides tools and libraries to develop high-performance, scalable web applications.

**.NET Core and .NET 5 (and later):** .NET Core is a modular, cross-platform, open source version of the .NET platform. Starting with .NET 5, Microsoft unified the .NET Framework and .NET Core into a single platform called .NET, eliminating the distinction between the two.

**Visual Studio:** It is the main integrated development environment (IDE) for working with .NET. Provides powerful tools for writing, debugging, and deploying .NET applications.

In short, .NET is a versatile and robust platform that offers developers the tools and libraries necessary to build a wide variety of applications, from small desktop applications to large enterprise systems and modern web applications.

> #### Code in C#

```csharp

// ==++==
//
//   Copyright (c) Microsoft Corporation.  All rights reserved.
//
// ==--==

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq.Expressions;
using System.Runtime.CompilerServices;

namespace Microsoft.CSharp.RuntimeBinder
{
    /// <summary>
    /// Contains factory methods to create dynamic call site binders for CSharp.
    /// </summary>
    [EditorBrowsable(EditorBrowsableState.Never)]
    public static class Binder
    {
        //////////////////////////////////////////////////////////////////////

        /// <summary>
        /// Initializes a new CSharp binary operation binder.
        /// </summary>
        /// <param name="flags">The flags with which to initialize the binder.</param>
        /// <param name="operation">The binary operation kind.</param>
        /// <param name="context">The <see cref="System.Type"/> that indicates where this operation is used.</param>
        /// <param name="argumentInfo">The sequence of <see cref="CSharpArgumentInfo"/> instances for the arguments to this operation.</param>
        /// <returns>Returns a new CSharp binary operation binder.</returns>
        public static CallSiteBinder BinaryOperation(
            CSharpBinderFlags flags,
            ExpressionType operation,
            Type context,
            IEnumerable<CSharpArgumentInfo> argumentInfo)
        {
            bool isChecked = (flags & CSharpBinderFlags.CheckedContext) != 0;
            bool isLogical = (flags & CSharpBinderFlags.BinaryOperationLogical) != 0;

            CSharpBinaryOperationFlags binaryOperationFlags = 0;
            if (isLogical)
            {
              binaryOperationFlags |= CSharpBinaryOperationFlags.LogicalOperation;
            }

            return new CSharpBinaryOperationBinder(operation, isChecked, binaryOperationFlags, context, argumentInfo);
        }


        //////////////////////////////////////////////////////////////////////

        /// <summary>
        /// Initializes a new CSharp convert binder.
        /// </summary>
        /// <param name="flags">The flags with which to initialize the binder.</param>
        /// <param name="type">The type to convert to.</param>
        /// <param name="context">The <see cref="System.Type"/> that indicates where this operation is used.</param>
        /// <returns>Returns a new CSharp convert binder.</returns>
        public static CallSiteBinder Convert(
            CSharpBinderFlags flags,
            Type type,
            Type context)
        {
            CSharpConversionKind conversionKind =
                ((flags & CSharpBinderFlags.ConvertExplicit) != 0) ?
                    CSharpConversionKind.ExplicitConversion :
                    ((flags & CSharpBinderFlags.ConvertArrayIndex) != 0) ?
                        CSharpConversionKind.ArrayCreationConversion :
                        CSharpConversionKind.ImplicitConversion;
            bool isChecked = (flags & CSharpBinderFlags.CheckedContext) != 0;

            return new CSharpConvertBinder(type, conversionKind, isChecked, context);
        }


        //////////////////////////////////////////////////////////////////////

        /// <summary>
        /// Initializes a new CSharp get index binder.
        /// </summary>
        /// <param name="flags">The flags with which to initialize the binder.</param>
        /// <param name="context">The <see cref="System.Type"/> that indicates where this operation is used.</param>
        /// <param name="argumentInfo">The sequence of <see cref="CSharpArgumentInfo"/> instances for the arguments to this operation.</param>
        /// <returns>Returns a new CSharp get index binder.</returns>
        public static CallSiteBinder GetIndex(
            CSharpBinderFlags flags,
            Type context,
            IEnumerable<CSharpArgumentInfo> argumentInfo)
        {
            return new CSharpGetIndexBinder(context, argumentInfo);
        }

        //////////////////////////////////////////////////////////////////////

        /// <summary>
        /// Initializes a new CSharp get member binder.
        /// </summary>
        /// <param name="flags">The flags with which to initialize the binder.</param>
        /// <param name="name">The name of the member to get.</param>
        /// <param name="context">The <see cref="System.Type"/> that indicates where this operation is used.</param>
        /// <param name="argumentInfo">The sequence of <see cref="CSharpArgumentInfo"/> instances for the arguments to this operation.</param>
        /// <returns>Returns a new CSharp get member binder.</returns>
        public static CallSiteBinder GetMember(
            CSharpBinderFlags flags,
            string name,
            Type context,
            IEnumerable<CSharpArgumentInfo> argumentInfo)
        {
            bool allowCallables = (flags & CSharpBinderFlags.ResultIndexed) != 0;
            return new CSharpGetMemberBinder(name, allowCallables, context, argumentInfo);
        }

        //////////////////////////////////////////////////////////////////////

        /// <summary>
        /// Initializes a new CSharp invoke binder.
        /// </summary>
        /// <param name="flags">The flags with which to initialize the binder.</param>
        /// <param name="context">The <see cref="System.Type"/> that indicates where this operation is used.</param>
        /// <param name="argumentInfo">The sequence of <see cref="CSharpArgumentInfo"/> instances for the arguments to this operation.</param>
        /// <returns>Returns a new CSharp invoke binder.</returns>
        public static CallSiteBinder Invoke(
            CSharpBinderFlags flags,
            Type context,
            IEnumerable<CSharpArgumentInfo> argumentInfo)
        {
            bool resultDiscarded = (flags & CSharpBinderFlags.ResultDiscarded) != 0;

            CSharpCallFlags callFlags = 0;
            if (resultDiscarded)
            {
              callFlags |= CSharpCallFlags.ResultDiscarded;
            }

            return new CSharpInvokeBinder(callFlags, context, argumentInfo);
        }

        //////////////////////////////////////////////////////////////////////

        /// <summary>
        /// Initializes a new CSharp invoke member binder.
        /// </summary>
        /// <param name="flags">The flags with which to initialize the binder.</param>
        /// <param name="name">The name of the member to invoke.</param>
        /// <param name="typeArguments">The list of type arguments specified for this invoke.</param>
        /// <param name="context">The <see cref="System.Type"/> that indicates where this operation is used.</param>
        /// <param name="argumentInfo">The sequence of <see cref="CSharpArgumentInfo"/> instances for the arguments to this operation.</param>
        /// <returns>Returns a new CSharp invoke member binder.</returns>
        public static CallSiteBinder InvokeMember(
            CSharpBinderFlags flags,
            string name,
            IEnumerable<Type> typeArguments,
            Type context,
            IEnumerable<CSharpArgumentInfo> argumentInfo)
        {
            bool invokeSimpleName = (flags & CSharpBinderFlags.InvokeSimpleName) != 0;
            bool invokeSpecialName = (flags & CSharpBinderFlags.InvokeSpecialName) != 0;
            bool resultDiscarded = (flags & CSharpBinderFlags.ResultDiscarded) != 0;

            CSharpCallFlags callFlags = 0;
            if (invokeSimpleName)
            {
              callFlags |= CSharpCallFlags.SimpleNameCall;
            }
            if (invokeSpecialName)
            {
              callFlags |= CSharpCallFlags.EventHookup;
            }
            if (resultDiscarded)
            {
              callFlags |= CSharpCallFlags.ResultDiscarded;
            }

            return new CSharpInvokeMemberBinder(callFlags, name, context, typeArguments, argumentInfo);
        }

        //////////////////////////////////////////////////////////////////////

        /// <summary>
        /// Initializes a new CSharp invoke constructor binder.
        /// </summary>
        /// <param name="flags">The flags with which to initialize the binder.</param>
        /// <param name="context">The <see cref="System.Type"/> that indicates where this operation is used.</param>
        /// <param name="argumentInfo">The sequence of <see cref="CSharpArgumentInfo"/> instances for the arguments to this operation.</param>
        /// <returns>Returns a new CSharp invoke constructor binder.</returns>
        public static CallSiteBinder InvokeConstructor(
            CSharpBinderFlags flags,
            Type context,
            IEnumerable<CSharpArgumentInfo> argumentInfo)
        {
            return new CSharpInvokeConstructorBinder(CSharpCallFlags.None, context, argumentInfo);
        }

        //////////////////////////////////////////////////////////////////////

        /// <summary>
        /// Initializes a new CSharp is event binder.
        /// </summary>
        /// <param name="flags">The flags with which to initialize the binder.</param>
        /// <param name="name">The name of the event to look for.</param>
        /// <param name="context">The <see cref="System.Type"/> that indicates where this operation is used.</param>
        /// <returns>Returns a new CSharp is event binder.</returns>
        public static CallSiteBinder IsEvent(
            CSharpBinderFlags flags,
            string name,
            Type context)
        {
            return new CSharpIsEventBinder(name, context);
        }

        //////////////////////////////////////////////////////////////////////

        /// <summary>
        /// Initializes a new CSharp set index binder.
        /// </summary>
        /// <param name="flags">The flags with which to initialize the binder.</param>
        /// <param name="context">The <see cref="System.Type"/> that indicates where this operation is used.</param>
        /// <param name="argumentInfo">The sequence of <see cref="CSharpArgumentInfo"/> instances for the arguments to this operation.</param>
        /// <returns>Returns a new CSharp set index binder.</returns>
        public static CallSiteBinder SetIndex(
            CSharpBinderFlags flags,
            Type context,
            IEnumerable<CSharpArgumentInfo> argumentInfo)
        {
            bool isCompoundAssignment = (flags & CSharpBinderFlags.ValueFromCompoundAssignment) != 0;
            bool isChecked = (flags & CSharpBinderFlags.CheckedContext) != 0;
            return new CSharpSetIndexBinder(isCompoundAssignment, isChecked, context, argumentInfo);
        }

        //////////////////////////////////////////////////////////////////////

        /// <summary>
        /// Initializes a new CSharp set member binder.
        /// </summary>
        /// <param name="flags">The flags with which to initialize the binder.</param>
        /// <param name="name">The name of the member to set.</param>
        /// <param name="context">The <see cref="System.Type"/> that indicates where this operation is used.</param>
        /// <param name="argumentInfo">The sequence of <see cref="CSharpArgumentInfo"/> instances for the arguments to this operation.</param>
        /// <returns>Returns a new CSharp set member binder.</returns>
        public static CallSiteBinder SetMember(
            CSharpBinderFlags flags,
            string name,
            Type context,
            IEnumerable<CSharpArgumentInfo> argumentInfo)
        {
            bool isCompoundAssignment = (flags & CSharpBinderFlags.ValueFromCompoundAssignment) != 0;
            bool isChecked = (flags & CSharpBinderFlags.CheckedContext) != 0;
            return new CSharpSetMemberBinder(name, isCompoundAssignment, isChecked, context, argumentInfo);
        }

        //////////////////////////////////////////////////////////////////////

        /// <summary>
        /// Initializes a new CSharp unary operation binder.
        /// </summary>
        /// <param name="flags">The flags with which to initialize the binder.</param>
        /// <param name="operation">The unary operation kind.</param>
        /// <param name="context">The <see cref="System.Type"/> that indicates where this operation is used.</param>
        /// <param name="argumentInfo">The sequence of <see cref="CSharpArgumentInfo"/> instances for the arguments to this operation.</param>
        /// <returns>Returns a new CSharp unary operation binder.</returns>
        public static CallSiteBinder UnaryOperation(
            CSharpBinderFlags flags,
            ExpressionType operation,
            Type context,
            IEnumerable<CSharpArgumentInfo> argumentInfo)
        {
            bool isChecked = (flags & CSharpBinderFlags.CheckedContext) != 0;
            return new CSharpUnaryOperationBinder(operation, isChecked, context, argumentInfo);
        }
    }
}
```

## Relationship

---

1. #### Director (Binder):

   - The Binder class acts as the "director" in the Builder pattern. Provides static methods that act as constructors for different types of CallSiteBinder objects.

2. #### Builder (CallSiteBinder):

   - CallSiteBinder objects are the "products" that are constructed using the static methods of the Binder class. Each static method in Binder is responsible for constructing a specific type of CallSiteBinder.

3. #### ConcreteBuilder Methods (BinaryOperation, Convert, etc.):

   - Each static method in Binder serves as a "concrete constructor" for a specific type of CallSiteBinder. For example, the BinaryOperation method constructs a CSharpBinaryOperationBinder, and the Convert method constructs a CSharpConvertBinder.

4. #### Parameter Settings (CSharpBinderFlags, ExpressionType, Type, etc.):

   - The methods accept parameters that represent the configuration necessary to construct the CallSiteBinder object. These parameters include enumerations (CSharpBinderFlags, CSharpBinaryOperationFlags, etc.) and specific types (ExpressionType, Type, etc.).

5. #### Complex Product (CallSiteBinder):

   - CallSiteBinder is the complex "product" that is built through the methods of the Binder class. Each type of CallSiteBinder has its own specific construction and configuration logic.

### Detailed Example (BinaryOperation):

The BinaryOperation method acts as a representative example. Let's discuss how the concepts of the Builder pattern are applied in this method:

```csharp

public static CallSiteBinder BinaryOperation(
    CSharpBinderFlags flags,
    ExpressionType operation,
    Type context,
    IEnumerable<CSharpArgumentInfo> argumentInfo)
{
    // Configuración específica para CSharpBinaryOperationBinder
    bool isChecked = (flags & CSharpBinderFlags.CheckedContext) != 0;
    bool isLogical = (flags & CSharpBinderFlags.BinaryOperationLogical) != 0;

    CSharpBinaryOperationFlags binaryOperationFlags = 0;
    if (isLogical)
    {
        binaryOperationFlags |= CSharpBinaryOperationFlags.LogicalOperation;
    }

    // Construcción del producto (CSharpBinaryOperationBinder)
    return new CSharpBinaryOperationBinder(operation, isChecked, binaryOperationFlags, context, argumentInfo);
}

```

**Builder (Binder):** The Binder class provides methods such as BinaryOperation to construct different types of CallSiteBinder.

**Builder Interface (CallSiteBinder):** CallSiteBinder is the interface that defines the structure of complex objects built by Binder.

**ConcreteBuilder Method (BinaryOperation):** This method builds a CSharpBinaryOperationBinder.

**Parameter Settings (flags, operation, context, argumentInfo):** The provided parameters customize the construction of the CSharpBinaryOperationBinder.

**Complex Product (CSharpBinaryOperationBinder):** It is the complex object built with the specified configuration.

### Benefits of the Builder Pattern in this Context:

**Decoupling:** The client (Binder) does not need to know the construction details of each type of CallSiteBinder. You just need to invoke the Binder methods with the appropriate parameters.

**Flexible Configuration:** The pattern allows you to configure and build different variants of CallSiteBinder with ease.

**Readability:** Binder-specific methods provide a clear interface for object construction, improving code readability.

This design makes it easier to extend, modify, and maintain the code in the future, since the construction details are encapsulated in Binder methods.
