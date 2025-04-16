---
# author: "Khalil Muhammad"
title: "Structural Subtyping in Python"
date: "2023-09-29"
description: "Python supports structural subtyping using typing.Protocols and @runtime_checkable."
tags: ["python", "typing"]
draft: true
ShowToc: false
ShowBreadCrumbs: false
ShowShareButtons: true
cover:
    image: "uml.jpeg"
    alt: "Structural Subtyping in Python"
    caption: "Structural Subtyping in Python"
    relative: false
    linkFullImage: true
# weight: 3
---

Structural subtyping in Python can be achieved using `typing.Protocol` and the `@runtime_checkable` decorator.

Rather than employing abc.ABC for nominal subclasses, a cleaner approach involves combining `@typing.runtime_checkable` with `typing.Protocol`. 

By decorating a protocol class with `@runtime_checkable`, it becomes a runtime protocol which is amenable to `isinstance()` and `issubclass()`. Without `@runtime_checkable`, attempting  `isinstance()` and `issubclass()` operations will result in a `TypeError`. Note that, for performance considerations, using hasattr() tends to be notably faster than `isinstance()` in these cases.

Structural subtyping is common to languages like Golang and C#, and it sidesteps issues associated with inheritance by favoring compositions.

For a comprehensive understanding and further details, refer to the Python Enhancement Proposal (PEP) 544 available at https://peps.python.org/pep-0544/.

This article is inspired by Hynek Schlawack's talk: [Subclassing, Composition, Python, and You](https://www.youtube.com/watch?v=2qpW1-7TnzA).

