Introduction
============

SharpC2 is a Command and Control (C2) framework written in C#.  It's made of three main elements:

- An ASP.NET Core Team Server.
- A .NET Framework Implant.
- A .NET Client.

SharpC2's implant is called a **Drone**.

The focus of the framework is to provide a solid set of base primitives for the Drone, but more importantly, extensibility for the user.  Both the Drone and Team Server have a modular architecture, which allows the user to push new capabilities to each at runtime.

The SharpC2 source code is available on GitHub:  https://github.com/SharpC2/SharpC2.

Index
=====

.. toctree::
   :maxdepth: 2
   
   building/index
   usage/index
   peer-to-peer/index
   c2profile/index
   customising/index
   hosting-files/index
   command-ref/index
   detection/index
   credits/index
