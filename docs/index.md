# Shor's Algorithm and Quantum Computing

The security of RSA encryption as we briefly descrive below, is based on the computational dificultty to factor large numbers. The best known algorithm, [General Number Field Sieve](https://en.wikipedia.org/wiki/General_number_field_sieve) perform this task in sub-exponential time $O\left(e^{1.9(\log N)^{1/3}(\log\log N)^{2/3}}\right)$, where $N$ is the number being factored, (then $\log N$ would be the order of the number of input bits)

In 1994, [Peter Shor](http://www-math.mit.edu/~shor/) came up with a Las Vegas algorithm for prime factorization and discrete logarithms, that take advantage of quantum computation to achieve a polynomial complexity in the number of bits, which is $O\left((\log N)^2(\log\log N)(\log \log\log N)\right)$.

## RSA Preliminaries

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.
