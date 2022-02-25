---
layout: post
title: Lombok Annotations
categories: [Lombok]
---

Lombok Annotations:

| annotation                                                      | Description                                                                                                                                               |
|-----------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| @nonNull                                                        | How I learned to stop worrying and love the NullPointerException                                                                                          |
| @Cleanup                                                        | Automatic resource management: Call your close() methods safely with no hassle.                                                                           |
| @Getter/@Setter                                                 | Never write public int getFoo() {return foo;} again.                                                                                                      |
| @ToString                                                       | No need to start a debugger to see your fields: Just let lombok generate a toString for you!                                                              |
| @EqualsAndHashCode                                              | Equality made easy: Generates hashCode and equals implementations from the fields of your object..                                                        |
| @NoArgsConstructor @RequiredArgsConstructor @AllArgsConstructor |    Constructors made to order: Generates constructors that take no arguments, one argument per final / non-nullfield, or one argument for every field.    |
| @Data                                                           | All together now: A shortcut for @ToString, @EqualsAndHashCode, @Getter on all fields, and @Setter on all non-final fields, and @RequiredArgsConstructor! |
| @Value                                                          | Immutable classes made very easy.                                                                                                                         |
| @Builder                                                        | and Bob's your uncle: No-hassle fancy-pants APIs for object creation!                                                                                     |
| @SneakyThrows                                                   | To boldly throw checked exceptions where no one has thrown them before!                                                                                   |
| @Synchronized                                                   | synchronized done right: Don't expose your locks.                                                                                                         |
| @With                                                           | Immutable 'setters' - methods that create a clone but with one changed field.                                                                             |
| @Getter(lazy=true)                                              | Laziness is a virtue!                                                                                                                                     |
| @Log                                                            | Captain's Log, stardate 24435.7: "What was that line again?"                                                                                              |