# ASCII Art Steganography & Key Generation Engine

A sophisticated systems-level tool written in C that transforms digital files (such as PDFs, raw binaries, or text documents) into a visually structured 32x32 cryptographic matrix. 

By taking full advantage of powerful low-level C features like **Bit-fields**, **Unions**, and **Bitwise XOR operations**, the engine deconstructs raw binary byte streams directly in memory. It maps them dynamically into an ANSI-colored ASCII art framework rendered straight onto the terminal screen, generating a unique, visual cryptographic signature (Visual Cryptographic Signature) for any file processed.

The application executes natively in two strict operational modes:
1. **Generation Mode (-g):** Processes a target file, mutates the predefined cipher matrix, prints the visual grid, and exports the payload as a standalone key file.
2. **Comparison Mode (-c):** Dynamically regenerates the visual structure of a file and matches it byte-for-byte against a previously saved key file to verify content integrity.

---

## Technical Features

* **Memory-Optimized Bit-Field Mapping:** Safely slices a single raw byte (8 bits) into precise graphical attributes instantly within the hardware register space, avoiding memory overhead:
  * `visibility` (1 bit) -> Decides if a character is drawn or skipped as a structural blank space.
  * `color` (3 bits) -> Generates 8 configurations mapping directly to terminal ANSI foreground paint codes.
  * `background` (1 bit) -> Flags a specialized background display style modifier.
  * `symbol` (3 bits) -> Indexes a static look-up array pointing to 8 distinct structural ASCII chars: `.`, `|`, `/`, `#`, `!`, `,`, `<`, or `>`.

* **In-Place XOR Cipher Integration:** Blends your predefined high-entropy random matrix with the incoming document bytes bitwise, creating an entirely customized layout for every distinct file state.

* **Native CLI Input Processing:** Features strict parsing of standard terminal arguments (`argc` / `argv`) to accept variable path entries cleanly at runtime without requiring hardcoded static strings.

---

## Architecture & Data Flow

[Raw File Stream Entry] ---> [Byte-by-Byte Sequential Read]
|
v
[Bitwise XOR Fusion with Cipher Matrix]
|
v
[Deconstruction via Custom Union Definition]
+------------+------------+---------------+----------+
| Symbol (3b)|   BG (1b)  |   Color (3b)  | Vis (1b) |
+------------+------------+---------------+----------+
|
v
[Render 32x32 Colored Art Frame to Terminal]


---

## Compilation & System Requirements

Because the runtime code heavily relies on native bash escape sequences to render colorized terminal boards, it is fully optimized to execute out-of-the-box in any standard Linux terminal emulator (e.g., Ubuntu).

Compile the source tree clean using `gcc`:

```bash
gcc 03.c -o key_engine

Usage Guide

The compiled binary evaluates specific command-line arguments flags passed into the interpreter vector to route program execution:
1. Key Generation and Export Mode (-g)

Scans the user-supplied document, processes the matrix values in place, displays the corresponding block-graphics layout, and pushes the binary footprint to an external .key asset.

    Syntax Pattern:

Bash

./key_engine -g <target_file_path>

    Execution Example:

Bash

./key_engine -g sample.pdf

Renders the file's visual signature to the active shell window and generates an authentication token named sample.pdf.key.
2. Matching and Verification Mode (-c)

Pulls a target document together with its historical key record. It rebuilds the active evaluation matrix on-the-fly and tests it against the reference key data, reporting whether the file layout matches perfectly or has been altered.

    Syntax Pattern:

Bash

./key_engine -c <target_file_path> <reference_key_file>

    Execution Example:

Bash

./key_engine -c sample.pdf sample.pdf.key

Implemented Stability Upgrades

To secure deterministic memory lookups and actively isolate the executable from kernel-level crash signals, the following architecture refinements were introduced:

    State Initialization Safety (cmp_keys): The isolated evaluation counters i and j in the key matching validation routine have been properly zero-initialized (unsigned char i = 0, j = 0;). This prevents unallocated garbage memory stack positions from feeding unmanaged offset indices into the loop, completely eliminating unmapped pointer exceptions (Segmentation Faults).

    Interpreter Vector Protections (main Boundary Checking): Structured explicit defensive guard conditions verifying argc capacity before jumping execution into the -c logical module. This safeguards operations so that if a shell operator skips passing the mandatory key descriptor (argv[3]), the application throws an elegant exit code warning instead of forcing the CPU to read a NULL memory pointer.
