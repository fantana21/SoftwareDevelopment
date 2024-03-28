# CMake resources

CMake has been around for quite a while now. The downside of this is that you find a lot of outdated information about it on the web. Answers on Stack Overflow, e.g., are usually notoriously bad. Even the examples and tutorials from Kitware itself often use old and unidiomatic CMake.

This page tries to provide good resources for modern CMake because it is more powerful, elegant, cleaner and, most importantly, simpler to use. Depending on whom you ask, modern CMake is CMake 3.0+, 3.4+, 3.16+, 3.19+ or even 3.21+. Since new versions still introduce useful features, it is best to always keep CMake up to date.


### Contents

- [Minimum requirements](#minimum-requirements)
- [Further resources](#further-resources)


## Minimum requirements

The following list contains the (in my opinion) minimally required resources that you should read/watch/know about before you start using CMake.

- [C++ Weekly - Ep 78 - Intro to CMake](https://www.youtube.com/watch?v=HPMvU64RUTY) (~13 min): *very* basic intro to CMake, for those who have never done anything with it
- [Mathieu Ropert "Using Modern CMake Patterns to Enforce a Good Modular Design"](https://www.youtube.com/watch?v=eC9-iRN2b04) (~48 min + questions): great talk about modular design, a high level view on CMake as well as how and why modern CMake is better than the old style
- [Daniel Pfeifer "Effective CMake"](https://www.youtube.com/watch?v=bsXLMQ6WgIk) (~1 h 16 min + questions): this is *the* talk; must watch
- [Effective Modern CMake](https://gist.github.com/mbinna/c61dbb39bca0e4fb7d1f73b0d66a4fd1): great list of dos and don'ts as well as kind of a summary of the two talks above
- [cmake-init](): A CMake project initializer from friendlyanon themselves, one of the most competent and helpful CMake gurus over on the C++ slack. It follows all the best practices of modern CMake and comes with all the bells and whistles (probably overkill for small projects). There are also example projects generated from it which can be very helpful.
- [CMake Reference Documentation](https://cmake.org/cmake/help/latest/): Last but not least, the official documentation. Of course, you should not read it in its entirety, but it is good reference material (except for the examples, which are sometimes also very outdated).


## Further resources

For those of you who cannot get enough of CMake.

Videos/Talks:

- [Deniz Bahadir "More Modern CMake"](https://www.youtube.com/watch?v=y7ndUhdQuU8): nomen est omen
- [Deniz Bahadir "Oh No! More Modern CMake"](https://www.youtube.com/watch?v=y9kSr5enrSk): nomen est omen

Reading:

- [How to Use CMake Without the Agonizing Pain]: a series of blog posts by Alex Reinking, another one of the helpful CMake gurus on the C++ slack.
- [An Introduction to Modern CMake](https://cliutils.gitlab.io/modern-cmake/): some kind of online book. I only skimmed through it, but it seems useful, and it is free \o/.
- [Professional CMake: A Practical Guide](https://crascit.com/professional-cmake/): *The* CMake book (didn't get around to read it myself, though). Written by Craig Scott, one of the co-maintainers of CMake, it is not free but comprehensive and *very* good for beginners as well as advanced users.
