# cleos_key_fixer
Modified cleos tool that can fix simple private key typos. 

This is a modified version of cleos from the public master branch circa the v2.0.7 stable release of EOSIO.

# Compiling (tested on Ubuntu 18.04)

Clone the EOSIO repository

```git clone https://github.com/eosio/eos --recurse-submodules```

In the cloned repository, replace the file at ```eos/programs/cleos/main.cpp``` with the ```main.cpp``` file that is in this repository.

Compile EOSIO by running ```eos/scripts/eosio_build.sh``` (or whatever for your platform)

# Using 

The ```cleos wallet import``` command usually fails when you give it an invalid private key.

However, this version augments it a new option flag, ```--fix-key```, which attempts to fix the given private key if it is invalid.

The algorithm is a simple brute force search which tries a few things, like swapping two adjacent characters, filling in missing characters or deleting extra characters, and testing a given number of typos (i.e. tries to replace characters in certain positions with all other possibilities for that position).

The algorithm never stops searching, but if your private key has more than two or three mistakes in it, it will take too long to search for the solution.

# Improvements

There are several ways to speed up the search.

A basic improvement would be to make it multithreaded.

It could also be turned into a distributed search, which could pay off if someone botched a private key that has lots of money in it.

The search can also be made smarter. 

One possible heuristic is to consider how people can confuse lowercase and uppercase characters when writing private keys on paper.

Another thing that can be done is taking in consideration the usual private key prefixes, which seem to always start with "5K", "5J" or "5H" only.
