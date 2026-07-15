# OS Storage Management in x86 Assembly

Two x86 AT&T assembly programs that model file allocation and defragmentation over fixed-size storage layouts.

- `Unidimensional.s` manages a linear array of 1,024 blocks.
- `Bidimensional.s` manages a 1,024 × 1,024 grid and can derive file entries from a real directory.

## Operations

Both implementations support:

1. **ADD** — allocate a contiguous range for one or more file descriptors;
2. **GET** — locate the range occupied by a descriptor;
3. **DELETE** — release a descriptor and print the remaining allocations;
4. **DEFRAGMENTATION** — compact allocations while preserving descriptor order.

The two-dimensional implementation also supports **CONCRETE**, which reads directory entries through Linux system calls, derives descriptors and block counts from file metadata, and attempts to place them in the simulated storage grid.

## Implementation notes

- Written in 32-bit x86 assembly using GNU AT&T syntax.
- Uses contiguous allocation with 8 KB logical blocks.
- Converts between flat indices and two-dimensional coordinates in the grid version.
- Implements compaction explicitly rather than delegating storage operations to a library.
- Uses C library I/O for the command interface and Linux system calls for directory traversal.

## Build

A 32-bit GCC toolchain and development libraries are required.

```bash
gcc -m32 -no-pie Unidimensional.s -o uni
gcc -m32 -no-pie Bidimensional.s -o bi
```

Run either executable and provide the operation sequence on standard input:

```bash
./uni < input.txt
./bi < input.txt
```

## Input format

The first integer is the number of operations. Each operation starts with its identifier:

| ID | Operation | Additional input |
| --- | --- | --- |
| `1` | ADD | number of files, followed by descriptor and size pairs |
| `2` | GET | descriptor |
| `3` | DELETE | descriptor |
| `4` | DEFRAGMENTATION | none |
| `5` | CONCRETE | directory path; two-dimensional version only |

File sizes are expressed in kilobytes and converted to 8 KB blocks internally.

## License

[MIT](LICENSE)
