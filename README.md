# Scid File Format Specification Version 4

Scid is a popular free chess database program. It stores games in it's own binary format. Whereas Scid is open source, there is no single clear file format specification, and one has to resort to deep study of it's C++ source code to understand the format. This document attempts to provide a technical specification of the format.

A Scid database consists of three files:

- An Index File (.si4) containing meta-information for each game
- A file containing the actual game data (.sg4)
- A file containing player names, tournament names and other textual information (.sn4)

