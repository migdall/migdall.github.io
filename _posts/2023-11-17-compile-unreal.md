---
layout: post
author: Jes&uacute;s Noland
title: "My Journey Compiling Unreal Engine 5: Overcoming Challenges and Bugs"
date: 2023-11-17
categories: [Programming]
---

# My Journey Compiling Unreal Engine 5: Overcoming Challenges and Bugs

Hello fellow game developers! I recently embarked on an exciting and challenging journey: compiling Unreal Engine 5 for the first time. This task was not just a personal goal but a challenge posed to me by my game developer mentor. It was a journey filled with learning, overcoming technical hurdles, and ultimately, a testament to the power of perseverance in the face of complex software challenges. In this post, I'll share my experience, the tools I used, and a particularly tricky bug I encountered and resolved.

My adventure began with the decision to compile Unreal Engine 5 using the documentation provided on Unreal's official website. But I was first turned to this documentation from the Pro Unreal Engine Game Coding Udemy course. As a developer, I knew this would be a significant step in understanding the engine's internals and capabilities.

To start, I used Visual Studio 2022 as my primary IDE, relying on its robust features and seamless integration with C++ projects. Additionally, GitHub Desktop was my choice for managing the source code, providing a user-friendly interface for the complex Git operations.

As I dived deeper into the compilation process, I encountered an unexpected hurdle. It turned out that Unreal Engine needed to be placed in a Windows path without any spaces. This bug was not immediately apparent and took some time to figure out. The solution was to clone the repository into a path without spaces, such as C:\UnrealEngine5.

After resolving the bug, the compilation process was smooth but lengthy. It took approximately three and a half hours to compile Unreal Engine 5 on my machine. I am running an AMD Ryzen 9 5900X with 16GB of DDR4, so I was hoping the compile time would be faster. This duration gave me a deeper appreciation of the complexity and power of this game engine.

Compiling Unreal Engine 5 was a rewarding experience that deepened my understanding of game development and software compilation. It was a blend of following detailed documentation, utilizing modern tools, and troubleshooting unexpected challenges. To anyone looking to undertake a similar journey, I say go for it! The learning and satisfaction you gain are well worth the effort.
