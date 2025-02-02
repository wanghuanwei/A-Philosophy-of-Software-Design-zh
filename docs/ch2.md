# 第 2 章 复杂性的本质

> Chapter 2 The Nature of Complexity

This book is about how to design software systems to minimize their complexity. The first step is to understand the enemy. Exactly what is “complexity”? How can you tell if a system is unnecessarily complex? What causes systems to become complex? This chapter will address those questions at a high level; subsequent chapters will show you how to recognize complexity at a lower level, in terms of specific structural features.

> 这本书是关于如何设计软件系统以最小化其复杂性。第一步是了解敌人。究竟什么是“复杂性”？您如何判断系统是否过于复杂？是什么导致系统变得复杂？本章将在较高层次上解决这些问题。后续章节将向您展示如何从较低的层次上根据特定的结构特征来识别复杂性。

The ability to recognize complexity is a crucial design skill. It allows you to identify problems before you invest a lot of effort in them, and it allows you to make good choices among alternatives. It is easier to tell whether a design is simple than it is to create a simple design, but once you can recognize that a system is too complicated, you can use that ability to guide your design philosophy towards simplicity. If a design appears complicated, try a different approach and see if that is simpler. Over time, you will notice that certain techniques tend to result in simpler designs, while others correlate with complexity. This will allow you to produce simpler designs more quickly.

> 识别复杂性的能力是至关重要的设计技能。它使您可以先找出问题，然后再付出大量努力，并可以在其他选择中做出正确的选择。判断一个设计是否简单比创建一个简单的设计要容易得多，但是一旦您认识到一个系统过于复杂，就可以使用该功能指导您的设计哲学走向简单。如果设计看起来很复杂，请尝试其他方法，看看是否更简单。随着时间的流逝，您会注意到某些技术往往会导致设计更简单，而其他技术则与复杂性相关。这将使您更快地制作更简单的设计。

This chapter also lays out some basic assumptions that provide a foundation for the rest of the book. Later chapters take the material of this chapter as given and use it to justify a variety of refinements and conclusions.

> 本章还列出了一些基本假设，这些基本假设为本书的其余部分奠定了基础。后面的章节将采用本章的内容，并用其论证各种改进和结论。

## 2.1 Complexity defined 复杂性的定义

For the purposes of this book, I define “complexity” in a practical way. Complexity is anything related to the structure of a software system that makes it hard to understand and modify the system. Complexity can take many forms. For example, it might be hard to understand how a piece of code works; it might take a lot of effort to implement a small improvement, or it might not be clear which parts of the system must be modified to make the improvement; it might be difficult to fix one bug without introducing another. If a software system is hard to understand and modify, then it is complicated; if it is easy to understand and modify, then it is simple.

> 出于本书的目的，我以实用的方式定义“复杂性”。复杂性与软件系统的结构有关，这使它很难理解和修改系统(复杂性是指那些让系统难以理解或修改的与系统相关的任何事物)。复杂性可以采取多种形式。例如，可能很难理解一段代码是如何工作的。可能需要花费很多精力才能实现较小的改进，或者可能不清楚必须修改系统的哪些部分才能进行改进；如果不引入其他错误，可能很难修复（也可以是不引入额外问题的情况下，很难修复一个bug）。如果一个软件系统难以理解和修改，那就很复杂。如果很容易理解和修改，那就很简单。

You can also think of complexity in terms of cost and benefit. In a complex system, it takes a lot of work to implement even small improvements. In a simple system, larger improvements can be implemented with less effort.

> 您还可以考虑成本和收益方面的复杂性（你还可以根据成本和收益来评估复杂性）。在复杂的系统中，要实施甚至很小的改进都需要大量的工作。在一个简单的系统中，可以用更少的精力实现更大的改进。

Complexity is what a developer experiences at a particular point in time when trying to achieve a particular goal. It doesn’t necessarily relate to the overall size or functionality of the system. People often use the word “complex” to describe large systems with sophisticated features, but if such a system is easy to work on, then, for the purposes of this book, it is not complex. Of course, almost all large and sophisticated software systems are in fact hard to work on, so they also meet my definition of complexity, but this need not necessarily be the case. It is also possible for a small and unsophisticated system to be quite complex.

> 复杂性是开发人员在尝试实现特定目标时在特定时间点所经历的。它不一定与系统的整体大小或功能有关。人们通常使用“复杂”一词来描述具有复杂功能的大型系统，但是如果这样的系统易于使用，那么就本书而言，它并不复杂。当然，实际上几乎所有大型复杂的软件系统都很难使用，因此它们也符合我对复杂性的定义，但这不一定是事实。小型而不复杂的系统也可能非常复杂。

Complexity is determined by the activities that are most common. If a system has a few parts that are very complicated, but those parts almost never need to be touched, then they don’t have much impact on the overall complexity of the system. To characterize this in a crude mathematical way:

> 复杂性取决于最常见的活动。如果系统中有一些非常复杂的部分，但是几乎不需要触摸这些部分，那么它们对系统的整体复杂性不会有太大影响。为了用粗略的数学方法来表征：

![](./figures/00009.gif)

The overall complexity of a system (C) is determined by the complexity of each part p (cp) weighted by the fraction of time developers spend working on that part (tp). Isolating complexity in a place where it will never be seen is almost as good as eliminating the complexity entirely.

> 系统的总体复杂度（C）由每个部分的复杂度（cp）乘以开发人员在该部分上花费的时间（tp）加权。在一个永远不会被看到的地方隔离复杂性几乎和完全消除复杂性一样好。

Complexity is more apparent to readers than writers. If you write a piece of code and it seems simple to you, but other people think it is complex, then it is complex. When you find yourself in situations like this, it’s worth probing the other developers to find out why the code seems complex to them; there are probably some interesting lessons to learn from the disconnect between your opinion and theirs. Your job as a developer is not just to create code that you can work with easily, but to create code that others can also work with easily.

> 读者比作家更容易理解复杂性。如果您编写了一段代码，对您来说似乎很简单，但是其他人则认为它很复杂，那么它就是复杂的。当您遇到这种情况时，有必要对其他开发人员进行调查，以找出为什么代码对他们而言似乎很复杂；从您的观点与观点之间的脱节中可能可以学习一些有趣的课程。作为开发人员，您的工作不仅是创建可以轻松使用的代码，而且还要创建其他人也可以轻松使用的代码。

## 2.2 Symptoms of complexity 复杂性的症状

Complexity manifests itself in three general ways, which are described in the paragraphs below. Each of these manifestations makes it harder to carry out development tasks.

> 复杂性通过以下三种段落中描述的三种一般方式体现出来。这些表现形式中的每一个都使执行开发任务变得更加困难。

Change amplification: The first symptom of complexity is that a seemingly simple change requires code modifications in many different places. For example, consider a Web site containing several pages, each of which displays a banner with a background color. In many early Web sites, the color was specified explicitly on each page, as shown in Figure 2.1(a). In order to change the background for such a Web site, a developer might have to modify every existing page by hand; this would be nearly impossible for a large site with thousands of pages. Fortunately, modern Web sites use an approach like that in Figure 2.1(b), where the banner color is specified once in a central place, and all of the individual pages reference that shared value. With this approach, the banner color of the entire Web site can be changed with a single modification. One of the goals of good design is to reduce the amount of code that is affected by each design decision, so design changes don’t require very many code modifications.

> 变更放大：复杂性的第一个征兆是，看似简单的变更需要在许多不同地方进行代码修改。例如，考虑一个包含几个页面的网站，每个页面显示带有背景色的横幅。在许多早期的网站中，颜色是在每个页面上明确指定的，如图 2.1（a）所示。为了更改此类网站的背景，开发人员可能必须手动修改每个现有页面；对于拥有数千个页面的大型网站而言，这几乎是不可能的。幸运的是，现代网站使用的方法类似于图 2.1（b），其中横幅颜色一次在中心位置指定，并且所有各个页面均引用该共享值。使用这种方法，可以通过一次修改来更改整个网站的标题颜色。

Cognitive load: The second symptom of complexity is cognitive load, which refers to how much a developer needs to know in order to complete a task. A higher cognitive load means that developers have to spend more time learning the required information, and there is a greater risk of bugs because they have missed something important. For example, suppose a function in C allocates memory, returns a pointer to that memory, and assumes that the caller will free the memory. This adds to the cognitive load of developers using the function; if a developer fails to free the memory, there will be a memory leak. If the system can be restructured so that the caller doesn’t need to worry about freeing the memory (the same module that allocates the memory also takes responsibility for freeing it), it will reduce the cognitive load. Cognitive load arises in many ways, such as APIs with many methods, global variables, inconsistencies, and dependencies between modules.

> 认知负荷：复杂性的第二个症状是认知负荷，这是指开发人员需要多少知识才能完成一项任务。较高的认知负担意味着开发人员必须花更多的时间来学习所需的信息，并且由于错过了重要的东西而导致错误的风险也更大。例如，假设 C 中的一个函数分配了内存，返回了指向该内存的指针，并假定调用者将释放该内存。这增加了使用该功能的开发人员的认知负担。如果开发人员无法释放内存，则会发生内存泄漏。如果可以对系统进行重组，以使调用者不必担心释放内存（分配内存的同一模块也负责释放内存），它将减少认知负担。（认知负荷出现在很多方面，例如很多方法的API，全局变量，不一致和模块间依赖）

System designers sometimes assume that complexity can be measured by lines of code. They assume that if one implementation is shorter than another, then it must be simpler; if it only takes a few lines of code to make a change, then the change must be easy. However, this view ignores the costs associated with cognitive load. I have seen frameworks that allowed applications to be written with only a few lines of code, but it was extremely difficult to figure out what those lines were. Sometimes an approach that requires more lines of code is actually simpler, because it reduces cognitive load.

> 系统设计人员有时会假设可以通过代码行来衡量复杂性。他们认为，如果一个实现比另一个实现短，那么它必须更简单；如果只需要几行代码就可以进行更改，那么更改必须很容易。但是，这种观点忽略了与认知负荷相关的成本。我已经看到了仅允许使用几行代码编写应用程序的框架，但是要弄清楚这些行是什么极其困难。有时，需要更多代码行的方法实际上更简单，因为它减少了认知负担。

![](./figures/00010.jpeg)

Figure 2.1: Each page in a Web site displays a colored banner. In (a) the background color for the banner is specified explicitly in each page. In (b) a shared variable holds the background color and each page references that variable. In (c) some pages display an additional color for emphasis, which is a darker shade of the banner background color; if the background color changes, the emphasis color must also change.

> 图 2.1：网站中的每个页面都显示一个彩色横幅。在（a）中，横幅的背景色在每页中都明确指定。在（b）中，共享变量保留背景色，并且每个页面都引用该变量。在（c）中，某些页面会显示其他用于强调的颜色，即横幅背景颜色的暗色；如果背景颜色改变，则强调颜色也必须改变。

Unknown unknowns: The third symptom of complexity is that it is not obvious which pieces of code must be modified to complete a task, or what information a developer must have to carry out the task successfully. Figure 2.1(c) illustrates this problem. The Web site uses a central variable to determine the banner background color, so it appears to be easy to change. However, a few Web pages use a darker shade of the background color for emphasis, and that darker color is specified explicitly in the individual pages. If the background color changes, then the the emphasis color must change to match. Unfortunately, developers are unlikely to realize this, so they may change the central bannerBg variable without updating the emphasis color. Even if a developer is aware of the problem, it won’t be obvious which pages use the emphasis color, so the developer may have to search every page in the Web site.

> 未知的未知:复杂性的第三个症状是，必须修改哪些代码才能完成任务，或者开发人员必须获得哪些信息才能成功地执行任务，这些都是不明显的。图 2.1(c)说明了这个问题。网站使用一个中心变量来确定横幅的背景颜色，所以它看起来很容易改变。但是，一些 Web 页面使用较暗的背景色来强调，并且在各个页面中明确指定了较暗的颜色。如果背景颜色改变，那么强调的颜色必须改变以匹配。不幸的是，开发人员不太可能意识到这一点，所以他们可能会更改中央 bannerBg 变量而不更新强调颜色。即使开发人员意识到这个问题，也不清楚哪些页面使用了强调色，因此开发人员可能必须搜索 Web 站点中的每个页面。

Of the three manifestations of complexity, unknown unknowns are the worst. An unknown unknown means that there is something you need to know, but there is no way for you to find out what it is, or even whether there is an issue. You won’t find out about it until bugs appear after you make a change. Change amplification is annoying, but as long as it is clear which code needs to be modified, the system will work once the change has been completed. Similarly, a high cognitive load will increase the cost of a change, but if it is clear which information to read, the change is still likely to be correct. With unknown unknowns, it is unclear what to do or whether a proposed solution will even work. The only way to be certain is to read every line of code in the system, which is impossible for systems of any size. Even this may not be sufficient, because a change may depend on a subtle design decision that was never documented.

> 在复杂性的三种表现形式中，未知的未知是最糟糕的。一个未知的未知意味着你需要知道一些事情，但是你没有办法找到它是什么，甚至是否有一个问题。你不会发现它，直到错误出现后，你做了一个改变。更改放大是令人恼火的，但是只要清楚哪些代码需要修改，一旦更改完成，系统就会工作。同样，高的认知负荷会增加改变的成本，但如果明确要阅读哪些信息，改变仍然可能是正确的。对于未知的未知，不清楚该做什么，或者提出的解决方案是否有效。唯一确定的方法是读取系统中的每一行代码，这对于任何大小的系统都是不可能的。甚至这可能还不够，因为更改可能依赖于一个从未记录的细微设计决策。

One of the most important goals of good design is for a system to be obvious. This is the opposite of high cognitive load and unknown unknowns. In an obvious system, a developer can quickly understand how the existing code works and what is required to make a change. An obvious system is one where a developer can make a quick guess about what to do, without thinking very hard, and yet be confident that the guess is correct. Chapter 18 discusses techniques for making code more obvious.

> 良好设计的最重要目标之一就是使系统显而易见。这与高认知负荷和未知未知数相反。在一个显而易见的系统中，开发人员可以快速了解现有代码的工作方式以及进行更改所需的内容。一个显而易见的系统是，开发人员可以在不费力地思考的情况下快速猜测要做什么，同时又可以确信该猜测是正确的。第 18 章讨论使代码更明显的技术。

## 2.3 Causes of complexity 复杂性的原因

Now that you know the high-level symptoms of complexity and why complexity makes software development difficult, the next step is to understand what causes complexity, so that we can design systems to avoid the problems. Complexity is caused by two things: dependencies and obscurity. This section discusses these factors at a high level; subsequent chapters will discuss how they relate to lower-level design decisions.

> 既然您已经了解了复杂性的高级症状以及为什么复杂性会使软件开发变得困难，那么下一步就是了解导致复杂性的原因，以便我们设计系统来避免这些问题。复杂性是由两件事引起的：依赖性和模糊性。本节从高层次讨论这些因素。随后的章节将讨论它们与低级设计决策之间的关系。

For the purposes of this book, a dependency exists when a given piece of code cannot be understood and modified in isolation; the code relates in some way to other code, and the other code must be considered and/or modified if the given code is changed. In the Web site example of Figure 2.1(a), the background color creates dependencies between all of the pages. All of the pages need to have the same background, so if the background is changed for one page, then it must be changed for all of them. Another example of dependencies occurs in network protocols. Typically there is separate code for the sender and receiver for the protocol, but they must each conform to the protocol; changing the code for the sender almost always requires corresponding changes at the receiver, and vice versa. The signature of a method creates a dependency between the implementation of that method and the code that invokes it: if a new parameter is added to a method, all of the invocations of that method must be modified to specify that parameter.

> 就本书而言，当无法孤立地理解和修改给定的一段代码时，便存在依赖关系。该代码以某种方式与其他代码相关，如果更改了给定代码，则必须考虑和/或修改其他代码。在图 2.1（a）的网站示例中，背景色在所有页面之间创建了依赖关系。所有页面都必须具有相同的背景，因此，如果更改一页的背景，则必须更改所有背景。依赖关系的另一个示例发生在网络协议中。通常，协议的发送方和接收方有单独的代码，但是它们必须分别符合协议。更改发送方的代码几乎总是需要在接收方进行相应的更改，反之亦然。

Dependencies are a fundamental part of software and can’t be completely eliminated. In fact, we intentionally introduce dependencies as part of the software design process. Every time you write a new class you create dependencies around the API for that class. However, one of the goals of software design is to reduce the number of dependencies and to make the dependencies that remain as simple and obvious as possible.

> 依赖关系是软件的基本组成部分，不能完全消除。实际上，我们在软件设计过程中有意引入了依赖性。每次编写新类时，都会围绕该类的 API 创建依赖关系。但是，软件设计的目标之一是减少依赖关系的数量，并使依赖关系保持尽可能简单和明显。

Consider the Web site example. In the old Web site with the background specified separately on each page, all of the Web pages were dependent on each other. The new Web site fixed this problem by specifying the background color in a central place and providing an API that individual pages use to retrieve that color when they are rendered. The new Web site eliminated the dependency between the pages, but it created a new dependency around the API for retrieving the background color. Fortunately, the new dependency is more obvious: it is clear that each individual Web page depends on the bannerBg color, and a developer can easily find all the places where the variable is used by searching for its name. Furthermore, compilers help to manage API dependencies: if the name of the shared variable changes, compilation errors will occur in any code that still uses the old name. The new Web site replaced a nonobvious and difficult-to-manage dependency with a simpler and more obvious one.

> 考虑网站示例。在每个页面分别指定背景的旧网站中，所有网页都是相互依赖的。新的网站通过在中心位置指定背景色并提供一个 API，供各个页面在呈现它们时检索该颜色，从而解决了该问题。新的网站消除了页面之间的依赖关系，但是它围绕 API 创建了一个新的依赖关系以检索背景色。幸运的是，新的依赖性更加明显：很明显，每个单独的网页都取决于 bannerBg 颜色，并且开发人员可以通过搜索其名称轻松找到使用该变量的所有位置。此外，编译器还有助于管理 API 依赖性：如果共享变量的名称发生变化，任何仍使用旧名称的代码都将发生编译错误。新的网站用一种更简单，更明显的方式代替了一种不明显且难以管理的依赖性。

The second cause of complexity is obscurity. Obscurity occurs when important information is not obvious. A simple example is a variable name that is so generic that it doesn’t carry much useful information (e.g., time). Or, the documentation for a variable might not specify its units, so the only way to find out is to scan code for places where the variable is used. Obscurity is often associated with dependencies, where it is not obvious that a dependency exists. For example, if a new error status is added to a system, it may be necessary to add an entry to a table holding string messages for each status, but the existence of the message table might not be obvious to a programmer looking at the status declaration. Inconsistency is also a major contributor to obscurity: if the same variable name is used for two different purposes, it won’t be obvious to developer which of these purposes a particular variable serves.

> 复杂性的第二个原因是晦涩。当重要的信息不明显时，就会发生模糊。一个简单的例子是一个变量名，它是如此的通用，以至于它没有携带太多有用的信息(例如，时间)。或者，一个变量的文档可能没有指定它的单位，所以找到它的惟一方法是扫描代码，查找使用该变量的位置。晦涩常常与依赖项相关联，在这种情况下，依赖项的存在并不明显。例如，如果向系统添加了一个新的错误状态，可能需要向一个包含每个状态的字符串消息的表添加一个条目，但是对于查看状态声明的程序员来说，消息表的存在可能并不明显。不一致性也是造成不透明性的一个主要原因:如果同一个变量名用于两个不同的目的，那么开发人员就无法清楚地知道某个特定变量的目的是什么。

In many cases, obscurity comes about because of inadequate documentation; Chapter 13 deals with this topic. However, obscurity is also a design issue. If a system has a clean and obvious design, then it will need less documentation. The need for extensive documentation is often a red flag that the design isn’t quite right. The best way to reduce obscurity is by simplifying the system design.

> 在许多情况下，由于文档不足而导致模糊不清。第 13 章讨论了这个主题。但是，模糊性也是设计问题。如果系统设计简洁明了，则所需的文档将更少。对大量文档的需求通常是一个警告，即设计不正确。减少模糊性的最佳方法是简化系统设计。

Together, dependencies and obscurity account for the three manifestations of complexity described in Section 2.2. Dependencies lead to change amplification and a high cognitive load. Obscurity creates unknown unknowns, and also contributes to cognitive load. If we can find design techniques that minimize dependencies and obscurity, then we can reduce the complexity of software.

> 依赖性和模糊性共同构成了第 2.2 节中描述的三种复杂性表现。依赖性导致变化放大和高认知负荷。晦涩会产生未知的未知数，还会增加认知负担。如果我们找到最小化依赖关系和模糊性的设计技术，那么我们就可以降低软件的复杂性。

## 2.4 Complexity is incremental 复杂度是递增的

Complexity isn’t caused by a single catastrophic error; it accumulates in lots of small chunks. A single dependency or obscurity, by itself, is unlikely to affect significantly the maintainability of a software system. Complexity comes about because hundreds or thousands of small dependencies and obscurities build up over time. Eventually, there are so many of these small issues that every possible change to the system is affected by several of them.

> 复杂性不是由单个灾难性错误引起的；它堆积成许多小块。单个依赖项或模糊性本身不太可能显着影响软件系统的可维护性。之所以会出现复杂性，是因为随着时间的流逝，成千上万的小依赖性和模糊性逐渐形成。最终，这些小问题太多了，以至于对系统的每次可能更改都会受到其中几个问题的影响。

The incremental nature of complexity makes it hard to control. It’s easy to convince yourself that a little bit of complexity introduced by your current change is no big deal. However, if every developer takes this approach for every change, complexity accumulates rapidly. Once complexity has accumulated, it is hard to eliminate, since fixing a single dependency or obscurity will not, by itself, make a big difference. In order to slow the growth of complexity, you must adopt a “zero tolerance” philosophy, as discussed in Chapter 3.

> 复杂性的增量性质使其难以控制。可以很容易地说服自己，当前更改所带来的一点点复杂性没什么大不了的。但是，如果每个开发人员对每种更改都采用这种方法，那么复杂性就会迅速累积。一旦积累了复杂性，就很难消除它，因为修复单个依赖项或模糊性本身不会产生很大的变化。为了减缓复杂性的增长，您必须采用第 3 章中讨论的“零容忍”理念。

## 2.5 Conclusion 结论

Complexity comes from an accumulation of dependencies and obscurities. As complexity increases, it leads to change amplification, a high cognitive load, and unknown unknowns. As a result, it takes more code modifications to implement each new feature. In addition, developers spend more time acquiring enough information to make the change safely and, in the worst case, they can’t even find all the information they need. The bottom line is that complexity makes it difficult and risky to modify an existing code base.

> 复杂性来自于依赖性和模糊性的积累。随着复杂性的增加，它会导致变化放大，高认知负荷和未知的未知数。结果，需要更多的代码修改才能实现每个新功能。此外，开发人员花费更多时间获取足够的信息以安全地进行更改，在最坏的情况下，他们甚至找不到所需的所有信息。最重要的是，复杂性使得修改现有代码库变得困难且冒险。
