# PackageCompiler.jl and Gaston.jl
@def date = Date(2020, 6, 16)

[PackageCompiler.jl](https://github.com/JuliaLang/PackageCompiler.jl) (by [Kristoffer Carlsson](https://github.com/KristofferC) et al.) allows you to compile a Julia project into a relocatable “app” that can be sent and run on other machines that don’t have Julia installed. The main downsides of these “apps” are:

* The size of the bundle is usually hundreds of megabytes even for simple programs with few dependencies. This is because it includes a Julia system image and compiled libraries (see [this topic](https://discourse.julialang.org/t/packagecompiler-size/23487)).
* The popular package [Plots.jl](https://github.com/JuliaPlots/Plots.jl) is not [relocatable](https://julialang.github.io/PackageCompiler.jl/dev/apps/#Relocatability-1) (see [this issue](https://github.com/JuliaLang/PackageCompiler.jl/issues/357)).

I created a topic in the Julia Discourse forum asking for [alternatives to Plots.jl that are relocatable](https://discourse.julialang.org/t/is-there-any-plot-package-that-is-relocatable/38699). A user suggested [Gadfly.jl](https://github.com/GiovineItalia/Gadfly.jl). I tried to compile a simple program that solves the Lorenz system and plot the 2D time evolution of the attractor. I was able to send the app to a friend and he could run it and see the plot. My friend told me that the program showed several warnings about absolute paths referencing files in my computer. Also, the bundle was ~500 MB (~80 MB compressed in 7-zip format).

Recently, I found a plotting package called [Gaston.jl](https://github.com/mbaz/Gaston.jl). It is a Julia front-end for gnuplot. It has few dependencies and access the gnuplot installation in your system. After reporting a [weird issue](https://github.com/mbaz/Gaston.jl/issues/136), my friend [Gustavo](https://github.com/gaaraujo) and I managed to add plotting capabilities to [GMDApp](https://github.com/gaaraujo/GMDApp) with Gaston.jl. Then, I was able to use PackageCompiler.jl to create [GMDApp bundles for Windows and Linux](https://github.com/gaaraujo/GMDApp/releases/tag/v0.1.0).

GMDApp bundles with Gaston.jl have a moderate size (~300 MB, ~50 compressed), and they proved to be successfully relocatable. For now, If I have to send compiled Julia plotting programs to my colleagues, I'll stick with Gaston.jl.

The issue I reported was about Gaston.jl showing weird warnings. According to Gaston.jl author ([Miguel Bazdresch](https://github.com/mbaz)), the problem is caused by a [bad interaction of gnuplot with Windows streams](https://sourceforge.net/p/gnuplot/bugs/2279/). Gustavo and I found out that Gaston.jl was able to show the first plot without showing warnings. That was ok for our use case because our program only needs to show one plot.