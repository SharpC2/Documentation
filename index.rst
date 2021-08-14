Introduction
============

SharpC2 is a Command and Control (C2) framework written in C#, made of three main elements:

- An ASP.NET Core Team Server.
- A .NET Framework Implant.
- A .NET Client.

The focus of the framework is to provide a solid set of base primitives for the default implant, but more importantly, extensibility for the user.  This includes the ability to:

- Implement custom C2 handlers.
- Add new capabilities to the default implant and team server, even at runtime.
- Build 3rd party implants in other languages.

The SharpC2 source code is available on GitHub:  https://github.com/SharpC2/SharpC2

.. toctree::
   :maxdepth: 2
   
   building/index
   usage/index
   extending/index