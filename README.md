[![Build Status](https://travis-ci.org/typelevel/scala.svg?branch=typelevel-readme)](https://travis-ci.org/typelevel/scala) [![Join the chat at https://gitter.im/typelevel/scala](https://badges.gitter.im/typelevel/scala.svg)](https://gitter.im/typelevel/scala?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

# Typelevel Scala

## What is this repository?

This [repository][tls] contains the [Typelevel][typelevel] [fork][fork] of the Scala compiler.

Typelevel Scala is a conservative, collaborative and binary compatible fork of [Lightbend Scala][lbs]. The intention
is for it to provide early access to bug fixes and enhancements of the Scala toolchain which are of particular
interest to [projects][projects] which use the "Typelevel style" &mdash; extensive use of type classes, generic
programming and exploitation of the distinctive features of Scala's type system such as higher-kinded, dependent and
singleton types.

## Relationship with Lightbend Scala

Typelevel Scala releases will be made in lockstep with Lightbend Scala and will by default generate code which is
binary compatible with code generated by Lightbend Scala. Code generated with Typelevel Scala can be freely linked
against binary dependencies generated with Lightbend Scala and vice versa. This allows Typelevel Scala features and
fixes to be used with minimal risk.

The policy for inclusion of a fix or a feature in a Typelevel Scala release is as follows,

+ It must be first submitted as a pull request to the corresponding version of Lightbend Scala.
+ It must have a reasonable likelihood of being merged in a future Lightbend Scala release.
+ It must offer a significant fix or feature for which there is no reasonable alternative or workaround.
+ It must maintain binary compatibility with Lightbend Scala unless a binary change is the primary motivation for the
  addition.

The aim of this policy is to keep Typelevel Scala as close as possible to Lightbend Scala whilst still providing
significant benefits to people who are affected by the issues it addresses. The requirement that all changes exist as
a pull request against Lightbend Scala _first_ and have a reasonable chance of being merged in a future Lightbend
Scala release is intended to encourage collaboration and convergence between Typelevel and Lightbend Scala
contributors.

Apart from the first criterion there is room for interpretation &mdash; after all, what counts as "reasonable"? Where
the letter of the policy is unclear we appeal to its spirit &mdash; we want access to these features and fixes in our
projects _now_ and for that to be feasible it is essential that we maintain the greatest possible degree of binary
interoperability with the rest of the Scala ecosystem.

## Typelevel Scala releases

Typelevel Scala releases are distinguished from the corresponding Lightbend Scala releases by a version number suffix
which indicates the Typelevel feature level beyond the baseline compiler. We are attempting to maintain parity of
Typelevel features across the Scala compiler versions we support.

The current Typelevel feature level is 4 and it is avaliable as a drop in replacement for Lightbend Scala 2.11.11 and
2.12.2. Full release notes are available,

+ Typelevel Scala 4 [2.12.2/2.11.11](https://github.com/typelevel/scala/blob/typelevel-readme/notes/typelevel-4.md).

Support for Scala 2.13.0-M1 will be added in due course. Support for Scala 2.10.6 will be considered if sponsors step
forward to support the necessary work.

## Should I use Typelevel Scala? In production?

The baseline for Typelevel Scala is the corresponding version of Lightbend Scala. Every bugfix or feature addition to
Typelevel Scala exists as a pull request against that Lightbend Scala version, so will have been passed by the full
Scala toolchain test suite and, in most cases, will have been reviewed by the Lightbend Scala compiler team.

If you are affected by one of the bugs which Typelevel Scala addresses then you will have to weigh the risks and costs
of using a compiler with the bug (perhaps with workarounds using unnecessarily complex encodings, macros or compiler
plugins), against the risks and costs of using an alternative Scala distribution which fixes that bug. Only you can
make that call. The same considerations apply to the additional features that Typelevel Scala supports.

More generally there are many reasons why you might want to use and contribute to Typelevel Scala,

+ You want to evaluate or contribute to a feature or fix.
+ You want to explore how new language features interact with existing libraries.
+ You want to explore new library designs enabled by new language features.
+ You want to propose new language features motiviated by your experiences with library design.
+ You want to contribute a feature or fix to Lightbend/Typelevel Scala and want to evaluate its interactions with
  other pending features and fixes.

Within the Typelevel family of projects we are particularly excited by the prospect of being able to coevolve
libraries along with the language and believe that this is one of the best ways to keep the language fresh and
relevant to practitioners.

## Try Typelevel Scala with an Ammonite instant REPL

The quickest way to get to a Typelevel Scala 2.12.2 REPL path is to run the provided
["try Typelevel Scala"][try-tls] script, which has no dependencies other than an installed JDK. This script
downloads and installs [coursier][coursier] and uses it to fetch the [Ammonite][ammonite] REPL and Typelevel Scala
2.12.2. It then drops you immediately into a REPL session,

```text
% curl -s https://raw.githubusercontent.com/typelevel/scala/typelevel-readme/try-typelevel-scala.sh | bash
Loading...
Compiling replBridge.sc
Compiling interpBridge.sc
Compiling HardcodedPredef.sc
Compiling ArgsPredef.sc
Compiling predef.sc
Compiling SharedPredef.sc
Compiling LoadedPredef.sc
Welcome to the Ammonite Repl 0.8.4
(Scala 2.12.2-bin-typelevel-4 Java 1.8.0_131)
If you like Ammonite, please support our development at www.patreon.com/lihaoyi
@
@ repl.compiler.settings.YliteralTypes.value = true

@ trait Cond[T] { type V ; val value: V }
defined trait Cond
@
@ implicit val condTrue = new Cond[true] { type V = String ; val value = "foo" }
condTrue: AnyRef with Cond[true]{type V = String} = $sess.cmd2$$anon$1@4fef2259
@ implicit val condFalse = new Cond[false] { type V = Int ; val value = 23 }
condFalse: AnyRef with Cond[false]{type V = Int} = $sess.cmd3$$anon$1@463887f6
@
@ def cond[T](implicit cond: Cond[T]): cond.V = cond.value
defined function cond
@
@ cond[true] : String
res5: String = "foo"
@ cond[false] : Int
res6: Int = 23
@ Bye!
%
```

[try-tls]: https://github.com/typelevel/scala/blob/typelevel-readme/try-typelevel-scala.sh
[coursier]: https://github.com/coursier/coursier
[ammonite]: https://github.com/lihaoyi/Ammonite

## How to use Typelevel Scala 4 with SBT

Requirements for using Typelevel Scala in your existing projects,

+ You must be using Lightbend Scala 2.12.2 or 2.11.11.
+ You must be using SBT 0.13.13 or later.
+ Your build should use `scalaOrganization.value` and `CrossVersion.patch` appropriately.

  You can find many examples linked to from [this issue][build-tweaks-1].
+ (Optional) Your build should scope `scalaVersion` to `ThisBuild`.

  You can find an example in [the shapeless build][build-tweaks-2],

  ```
  inThisBuild(Seq(
    organization := "com.chuusai",
    scalaVersion := "2.12.2",
    crossScalaVersions := Seq("2.10.6", "2.11.8", "2.12.2", "2.13.0-M1")
  ))
  ```

You can now temporarily build with Typelevel Scala by entering,

```
> ; set every scalaOrganization := "org.typelevel" ; ++2.12.2-bin-typelevel-4
```

on the SBT REPL.

To switch your project permanently to Typelevel Scala 4 update your `build.sbt` as follows,

```
inThisBuild(Seq(
  scalaOrganization := "org.typelevel"
  scalaVersion := "2.12.2-bin-typelevel-4"
))
```

Alternatively, if you want to try Typelevel Scala without modifying your `build.sbt` and you have scoped
`scalaVersion` as shown earlier you can instead create a file `local.sbt` at the root of your project with the
following content,

```
inThisBuild(Seq(
  scalaOrganization := "org.typelevel",
  scalaVersion      := "2.11.11-bin-typelevel-4"
))
```

These settings will override the `ThisBuild` scoped settings in your `build.sbt`.

Whichever method you choose, your build should now function as before but using the Typelevel Scala toolchain instead
of the Lightbend one. You can verify that the settings have been updated correctly from the SBT prompt using `show`,

```
> show scalaOrganization
[info] org.typelevel
> show scalaVersion
[info] 2.12.2-bin-typelevel-4
```

[build-tweaks-1]: https://github.com/typelevel/scala/issues/135
[build-tweaks-2]: https://github.com/milessabin/shapeless/blob/master/build.sbt#L13-L17

## How to use Typelevel Scala 4 with Maven

If you are using maven with the `scala-maven-plugin`, set the `<scalaOrganization>` to `org.typelevel`,

```
<plugin>
  <groupId>net.alchim31.maven</groupId>
  <artifactId>scala-maven-plugin</artifactId>
  <version>3.2.1</version>
  <configuration>
    <scalaOrganization>org.typelevel</scalaOrganization>
    <scalaVersion>2.12.2-bin-typelevel-4</scalaOrganization>
  </configuration>
</plugin>
```

## Roadmap

The following are high priority issues for Typelevel projects on which progress is likely to be made in 2017,

+ Partial type application
+ Multiple implicit parameter blocks
+ ~~Improved compile times for inductive implicits~~ &mdash; **Done**
+ Improved control over implicit prioritization
+ ~~Improved error reporting for implicit resolution failures~~ &mdash; **Done**
+ GADT and singleton type bugfixes.
+ Literal syntax for Byte and Short types.
+ ~~Support for alternative `scala.Predef`~~ &mdash; **Done**

In accordance with the policy for inclusion contributions on these issues will be made as pull requests against
Lightbend Scala in the first instance.

## How to contribute to Typelevel Scala

Because of our policy of "pull request against Lightbend Scala first" the primary route to contributing to Typelevel
Scala is by contributing to Lightbend Scala. The Lightbend Scala team have made huge advances in lowering the barrier
to participation in compiler development recently, in large part due to a new SBT-based build, and [helpful
documentation][lbs-readme] &mdash; the Scala distribution is now an SBT project which you can expect to work on in
much the same way as any other github hosted FLOSS Scala project. [Miles Sabin][milessabin] has provided a write up
(now slightly outdated) of the process [here][scalac-post].

Because a pull request against [scala/scala][lbs] is the first step, to contribute you will need to sign the [Scala
CLA][scala-cla] and be willing to make your contributions under the [Scala License][scala-license].  Typelevel is
strongly in favour of the currently Scala license/CLA (which requires contributors to grant a patent license but does
not grant one in return) being replaced by the Apache 2.0 [license][apache-2.0-license] and [ICLA][apache-2.0-icla]
(which both requires and grants a patent license) as soon as is feasible. If you would like to contribute but are
unable to sign the CLA please raise this issue on the [Lightbend][lbs-gitter] and [Typelevel][tls-gitter] gitter
channels.

## Community

Topics specific to Typelevel Scala are discussed on its [gitter channel][tls-gitter]. Scala compiler development is
discussed on the [scala/contributors gitter channel][lbs-gitter].

People are expected to follow the [Typelevel Code of Conduct][coc] when discussing Typelevel Scala on the Github page,
Gitter channel, or other venues.

We hope that our community will be respectful, helpful, and kind. If you find yourself embroiled in a situation that
becomes heated, or that fails to live up to our expectations, you should disengage and contact one of the [project
maintainers](#maintainers) in private. We hope to avoid letting minor aggressions and misunderstandings escalate into
larger problems.

If you are being harassed, please contact one of [us](#maintainers) immediately so that we can support you.

## Maintainers

The current maintainers (people who can merge pull requests) are:

 + Erik Osheim ([@non](https://github.com/non))
 + George Leontiev ([@folone](https://github.com/folone))
 + Jon Pretty ([@propensive](https://github.com/propensive))
 + Lars Hupel ([@larsrh](https://github.com/larsrh))
 + Mike O'Connor ([@stew](https://github.com/stew))
 + Miles Sabin ([@milessabin](https://github.com/milessabin))
 + Tom Switzer ([@tixxit](https://github.com/tixxit))

[tls]: https://github.com/typelevel/scala
[fork]: http://typelevel.org/blog/2014/09/02/typelevel-scala.html
[lbs]: https://github.com/scala/scala
[projects]: http://typelevel.org/projects/
[tls-gitter]: https://gitter.im/typelevel/scala
[lbs-gitter]: https://gitter.im/scala/contributors
[typelevel]: http://typelevel.org/
[coc]: http://typelevel.org/conduct.html
[lbs-readme]: https://github.com/scala/scala/blob/2.12.x/README.md
[scalac-post]: http://milessabin.com/blog/2016/05/13/scalac-hacking/
[milessabin]: https://github.com/milessabin
[scala-cla]: http://www.lightbend.com/contribute/cla/scala
[scala-license]: https://github.com/scala/scala/blob/2.12.x/doc/LICENSE.md
[apache-2.0-license]: http://www.apache.org/licenses/LICENSE-2.0
[apache-2.0-icla]: https://www.apache.org/licenses/icla.txt
