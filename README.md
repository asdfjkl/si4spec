# Scid File Format Specification Version 4

Scid is a popular free chess database program. It stores games in it's
own binary format. Whereas Scid is open source, there is no single clear
file format specification, and one has to resort to deep study of it's
C++ source code to understand the format. This document attempts to
provide a technical specification of the format in order to facilitate
the implementation of the format by programs other than direct forks of
the SCID source base.

A Scid database consists of three files:

  - An Index File (.si4) containing meta-information for each game
  - A file containing the actual game data (.sg4)
  - A file containing player names, tournament names and other textual
    information (.sn4)

# The Index File

The Index file starts with a header of 182 byte. After that index
records follow. There is one index record for each stored game. Each
index record has a length of 47 byte.

<table>
<tbody>
<tr class="odd">
<td>Data</td>
<td>Length (Byte)</td>
<td>Description</td>
</tr>
<tr class="even">
<td>Magic Header</td>
<td>8</td>
<td><p>53 63 69 64 2E 73 69 00 in Hex</p>
<p>i.e. Scid.si\NUL in Ascci</p></td>
</tr>
<tr class="odd">
<td>Version Number</td>
<td>2</td>
<td>01 90 (i.e. 400 in decimal). All existing SCID forks support (and write) according to this standard</td>
</tr>
<tr class="even">
<td>Type of Database</td>
<td>4</td>
<td><p>00 00 00 00 </p>
<p>A byte sequence that describes the type of the database, e.g. tournament, theory, etc. All existing SCID forks seem to set this to a sequence of four zero bytes. In KingbaseLite this is set to 00 00 00 05</p></td>
</tr>
<tr class="odd">
<td>Number of Stored Games</td>
<td>3</td>
<td><p>Interpreted as an unsigned int. This theoretically limits the number of stored games in a database to 16777215.</p>
<p>Example 11 8F 32 denotes that there are 1150770 games in the database.</p></td>
</tr>
<tr class="even">
<td>Auto Load Game</td>
<td>3</td>
<td><p>This entry denotes which game to automatically load on opening the database. The encoding is as follows:</p>
<p>00 00 00: The first game should be loaded</p>
<p>00 00 01: It is not defined which game to load</p>
<p>If none of the above, the encoding is: Let the three bytes denote an integer N. Load the game N – 1.</p>
<p>E.g. 00 00 02: load the first game (this is redundant but due to backward compatibility to .si3).</p>
<p>00 00 03: load the second game.</p>
<p>Etc.</p></td>
</tr>
<tr class="odd">
<td>Database Info</td>
<td>108</td>
<td><p>This contains a general description of the database and it‘s contents. The last byte MUST be set to 00.</p>
<p>With the usual SCID forks, this is just a sequence of 128 zero-bytes.</p></td>
</tr>
<tr class="even">
<td>Custom Flags</td>
<td>54</td>
<td>There are six custom flag entries. Each entry is nine byte long and consists of a zero-terminated ASCII string (last charachter is NUL, i.e. a zero byte). Within each entry, any byte following a zero byte must be ignored.</td>
</tr>
<tr class="odd">
<td>Overall Length</td>
<td>182</td>
<td></td>
</tr>
</tbody>
</table>

After the fixed length header, index entries follows. Each index entry
has a length of 47 byte, and describes general information about a game,
as well as offsets into other files (e.g. for the actual encoded
sequence of moves, or for the player names).

|                |               |             |
| -------------- | ------------- | ----------- |
| Data           | Length (Byte) | Description |
|                |               |             |
|                |               |             |
|                |               |             |
|                |               |             |
|                |               |             |
|                |               |             |
|                |               |             |
| Overall Length | 47            |             |
